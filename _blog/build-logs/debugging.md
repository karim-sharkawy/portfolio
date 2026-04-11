---
layout: post
title: "Debugging a Production Model Failure"
date: 2024-04-05
category: "Build Logs"
---

At 3 AM, I got the alert: "Fraud detection model accuracy dropped from 92% to 71%."

This is the story of the next 6 hours, what went wrong, and what we learned.

## Timeline

**3:05 AM**: Alert triggered. Accuracy nosedive is real.

**3:10 AM**: First hypothesis—data pipeline issue.
```python
# Check recent predictions
query = """
  SELECT COUNT(*) from predictions
  WHERE created_at > NOW() - INTERVAL '1 hour'
"""
# Result: 0. Pipeline broken.
```

**3:15 AM**: Not broken. The queue processor was running, but predictions weren't being saved.
Found error in logs:
```
KeyError: 'transaction_id' in line 512 of feature_builder.py
```

**3:20 AM**: The upstream data source changed format. `transaction_id` renamed to `txn_id`.
But our code expected the old schema.

**3:25 AM**: Temporary fix: Added data schemavalidation to catch this.
```python
required_fields = ['txn_id', 'amount', 'user_id', 'timestamp']
missing = [f for f in required_fields if f not in data]
if missing:
    log_alert(f"Missing fields: {missing}")
    return fallback_prediction()
```

## But That's Not the Real Problem

Fast-forward to 5 AM: Schema fixed, predictions running again.

But accuracy is *still* 71%.

Root cause analysis:

1. During the 2-hour downtime, fraud patterns changed
2. High-risk transactions accumulated (data point: spike in suspicious activity)
3. Model was seeing completely different data distribution
4. The model trained on historical data couldn't adapt

**The real issue**: No retraining for 6 months.

## What Happened Under the Hood

```
Training data (6 months ago):
├─ Normal transactions: 95%
└─ Fraud: 5%

Current data (during outage):
├─ Normal transactions: 88%
├─ Fraud: 12%
└─ Plus new fraud patterns we've never seen
```

The model was biased toward "normal = no fraud" based on historical data.
When fraud rate jumped, every prediction was catastrophically wrong.

## Fixes We Implemented

### 1. **Automated Data Validation**

```python
def validate_data_distribution():
    historical = get_historical_distribution()
    recent = get_recent_distribution()
    
    # Check for drift
    ks_stat = scipy.stats.ks_2samp(historical, recent).statistic
    
    if ks_stat > 0.05:  # Threshold
        log_alert("Data drift detected")
        trigger_retraining()
```

### 2. **Continuous Retraining**

Before:
- Model: 6 months old
- Retrained: Never

After:
- Model: Retrained weekly
- But we check for drift first (not every day)

```python
if drift_detected():
    retrain_model(data_from_last_week)
    if new_model_better_than_current:
        deploy_new_model()
```

### 3. **Fallback System**

If model fails:
1. Log error
2. Serve last-known-good model
3. Alert team
4. Don't silently fail

```python
try:
    prediction = model.predict(features)
except Exception:
    prediction = fallback_model.predict(features)
    log_alert("Using fallback model")
```

### 4. **Alerting on Metrics, Not Just Errors**

Old alerts:
- Pipeline error? Alert
- No predictions? Alert

New alerts:
- Accuracy dropped > 5%? Alert
- Prediction distribution changed? Alert
- Confidence scores all low? Alert

```python
def monitor_predictions():
    predictions = get_recent_predictions()
    
    accuracy = get_validation_accuracy(predictions)
    if accuracy < 0.87:  # Our SLA
        alert("Accuracy below SLA")
    
    avg_confidence = np.mean(predictions.confidence)
    if avg_confidence < 0.5:
        alert("Model confidence too low")
```

### 5. **Root Cause: Missing Explicit Feedback Loop**

The biggest lesson: **We weren't verifying predictions.**

We had:
- Model predictions
- Actual labels (from ground truth 1 week later)

But they were **never compared**.

```python
# Added: Weekly accuracy check
def weekly_accuracy_report():
    predictions_from_7_days_ago = get_predictions(days_back=7)
    actual_outcomes = get_actual_outcomes(days_back=7)
    
    accuracy = (predictions_from_7_days_ago == actual_outcomes).mean()
    
    log_metric("model_accuracy_weekly", accuracy)
    
    # This would have shown the problem immediately
    return accuracy
```

## Lasting Changes

1. **Weekly retraining** on recent data
2. **Drift detection** for when to retrain
3. **Fallback predictions** when model fails
4. **Weekly accuracy reports** connected to Slack
5. **Feature monitoring**: Track distribution of input features
6. **A/B testing** framework before deploying new models

## What We Learned

1. **Production isn't a static optimization problem.** Data changes. You must adapt.

2. **Monitoring accuracy in production is hard** because labels arrive late. But drift detection (input distribution) is easy and useful.

3. **Fallback systems save lives** (figuratively). Always have a way to serve reasonable predictions if your fancy model fails.

4. **Logs aren't enough.** Errors are rare. Silently degraded performance is the real killer. Track metrics, not just exceptions.

5. **The simplest failure** locked our monitoring: a schema change. Defensive programming matters.

---

## Postscript

The 6-month stale model wasn't malice or oversight. It was:
- Model worked well initially ✓
- No obvious way to break it ✓
- No automation to retrain ✓
- No monitoring to catch drift ✓

Classic production story: Systems slowly degrade invisibly until they catastrophically fail.

**Prevention > Cure.** Better monitoring and automatic retraining would have caught this in day 1, not day 180.
