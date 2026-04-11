---
layout: post
title: "Building a Model Registry: Lessons from Production"
date: 2024-07-15
category: "Build Logs"
---

We built a model registry to manage model versions, metadata, and lineage across our ML platform.
This post covers the architecture, failures, and what we learned.

## The Problem

After 6 months, we had chaos:

- Models stored in random S3 buckets with unclear versions
- No tracking of which model was in production where
- Data scientists retrained the same model twice without knowing
- Rollback meant manually finding old code and data
- No visibility into model drift or performance decay

We needed centralized model management.

## What We Built

A simple registry with:
- **Model versioning**: Track all trained versions
- **Metadata**: Training config, performance metrics, data used
- **Lineage**: Which data, code, and hyperparameters created each version
- **Governance**: Approval workflow before promotion to production
- **Monitoring**: Performance tracking and drift detection

## Architecture

```
┌─────────────────────────────────────┐
│     Data Scientists' Workspace      │
│  (Notebooks, Training Scripts)      │
└────────────┬────────────────────────┘
             │ register_model()
             ▼
┌─────────────────────────────────────┐
│      Model Registry Service         │
│    (FastAPI + PostgreSQL)           │
├─────────────────────────────────────┤
│ - Version control                   │
│ - Metadata storage                  │
│ - Artifact management               │
│ - Access control                    │
└────────────┬────────────────────────┘
             │
    ┌────────┴────────┬──────────┐
    ▼                 ▼          ▼
┌────────────┐  ┌──────────┐  ┌──────────┐
│ S3 Bucket  │  │PostgreSQL│  │ Dashboard│
│(Models)    │  │(Metadata)│  │(UI)      │
└────────────┘  └──────────┘  └──────────┘
```

## Lessons Learned (Hard Way)

### 1. **Model Artifacts Need Versioning Too**

**Mistake**: Stored model weights with git, tried to version via commit hash.

**Reality**: Weights are binary—git doesn't handle them well. Huge repo bloat.

**Fix**: Separate code versioning (git) from artifact versioning (S3 + manifest).

```python
# Registry entry includes:
{
    "model_id": "demand-v1.2",
    "code_version": "a3f2d8c",  # Git commit
    "artifact_path": "s3://models/demand-v1.2/model.pt",
    "artifact_hash": "sha256:4x8f2...",  # Verify integrity
    "training_data": "2024-01-15 to 2024-08-10",
    "metrics": {"mape": 0.114, "rmse": 3900}
}
```

### 2. **Metadata Completeness >>> Pretty UI**

**Mistake**: Spent 2 weeks building a fancy dashboard.

**Reality**: Nobody used it. They needed the registry API to integrate with their pipelines.

**Fix**: Built CLI + Python SDK first, dashboard second.

```python
# What we actually needed
model, version = registry.load_model("demand-latest")
registry.promote("demand-v1.2", "staging")
registry.set_production("demand-v1.2", region="us-west")

# Integrated into Airflow DAGs seamlessly
```

### 3. **Access Control is Hard**

**Mistake**: Allowed anyone to promote models to production immediately.

**Reality**: A bad model got deployed without review. Customers saw degraded performance.

**Fix**: Added approval workflow.

```
Data Scientist                Registry                          SRE
    │                           │                              │
    ├─ train_model() ────────────>                            
    │                           │
    ├─ register_model() ────────>  (REGISTERED state)         
    │                           │
    └─ request_production() ───>  (PENDING_APPROVAL)          
                                │
                                └──── Notify SRE ──────────────>
                                                    │
                                        <─ review + approve ─────┤
                                │                              │
                        (PRODUCTION)                           │
                                │<──── deploy ────────────────┤
```

### 4. **Fallback Strategy Prevents Cascades**

**Mistake**: When registry was down, entire training pipeline failed.

**Reality**: Service unavailability = no new models = stale predictions in production.

**Fix**: Graceful degradation + local caching.

```python
# Fallback logic
try:
    model = registry.load_model("demand-latest")
except RegistryUnavailable:
    # Use last 24hr cached model
    model = load_from_local_cache()
    log_alert("Registry unavailable, using cached model")
```

### 5. **Lineage Prevents Accidental Retraining**

**Mistake**: Two teams trained the same model independently on the same data the same week.

**Reality**: Wasted compute + confusion about which version is "right".

**Fix**: Full lineage tracking.

```python
# Registry stores:
{
    "training_script": "github.com/org/ml-pipelines/blob/a3f2d8c/train_demand.py",
    "training_data_version": "demand-v2.3",
    "hyperparameters": {"lr": 0.001, "batch_size": 32},
    "random_seed": 42,
    "trained_by": "sarah@company.com",
    "trained_at": "2024-08-10T14:32:00Z"
}

# Query before training:
registry.find_similar_models(
    script_version="a3f2d8c",
    data_version="demand-v2.3"
)
# Returns: "demand-v1.2 trained 3 days ago with same config"
```

### 6. **Performance Monitoring is Essential**

**Mistake**: Promoted a "better" model to production. It was better on validation set but worse in production.

**Reality**: Distribution shift. Production data was different from training.

**Fix**: A/B testing + continuous monitoring.

```python
# A/B test endpoint
def get_prediction(user_id, use_new_model=False):
    if use_new_model:
        model = registry.load_model("demand-v1.3-candidate")
    else:
        model = registry.load_model("demand-production")
    
    pred = model.predict(...)
    
    # Log for monitoring
    log_prediction_event({
        "model": "demand-v1.3-candidate" if use_new_model else "demand-production",
        "prediction": pred,
        "actual": actual_value,  # Logged later
        "timestamp": now()
    })
    return pred
```

## What We Got Right

1. **Started simple**: CRUD API + S3 storage. No fancy ML ops platform.
2. **Integrated early**: Connected to Airflow, monitoring, deployment pipelines immediately.
3. **Made governance lightweight**: Approval process was one click, not bureaucratic.
4. **Built in observability**: Every action logged and traceable.

## Current Status

The registry is now the backbone of our ML platform:
- 200+ registered models
- 60+ in production (across 15 teams)
- Average deployment time: 30 minutes (vs. hours before)
- Zero accidental overwrites in 18 months
- Easy rollbacks (< 5 min)

## What's Next

- **Automated retraining**: Trigger retraining on data drift
- **Cost tracking**: Show per-model inference costs
- **Performance baselines**: Auto-flag models underperforming baseline
- **Model cards**: Standardized documentation for each model

---

**Lesson**: Build ML infrastructure for your team, not for some imaginary future scale.
Fix it as it breaks. Governance grows with team size.
