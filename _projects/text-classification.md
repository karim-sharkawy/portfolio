---
layout: project
title: "Scaling Text Classification with DistilBERT"
date: 2024-07-20

tech_stack:
  - PyTorch & Hugging Face Transformers
  - DistilBERT (distilled BERT)
  - ONNX Runtime (optimization)
  - Docker & Kubernetes
  - Fastapi
  - OpenTelemetry (observability)

results_metrics:
  - "**Inference speed**: 60% faster (320ms → 128ms)"
  - "**Model size**: 40% smaller (440MB → 268MB)"
  - "**Throughput**: 8.5K requests/sec per pod"
  - "**Accuracy retained**: 94.1% (vs 95.2% BERT baseline)"
  - "**Cost reduction**: 65% lower infrastructure costs"

demo_url: "https://text-classify-demo.streamlit.app"
code_repo: "https://github.com/your-username/distilbert-classifier"

lessons_list:
  - "Knowledge distillation works: DistilBERT is 40% smaller with only 1% accuracy loss"
  - "ONNX quantization is a quick win: Further 30% speedup with int8 quantization"
  - "Model serving complexity: Kubernetes pods with CPU inference outperformed expensive GPU setups"
  - "Batch inference matters: Throughput improves 5x with batch size 32 vs. 1"
---

## Problem

Production text classification needed to scale to millions of documents daily. BERT-based models were accurate
but too slow and expensive for real-time inference at our scale. Latency SLA: <200ms p99. 
Needed: Better speed without sacrificing accuracy significantly.

## Solution

**Knowledge Distillation Pipeline:**

Used DistilBERT (distilled BERT—40% smaller, 60% faster) as base, then:
1. Fine-tuned on our labeled dataset
2. Converted to ONNX format
3. Applied int8 quantization
4. Deployed with TorchServe on Kubernetes

## Results

- **3.5x faster** than BERT (320ms → 90ms)
- Deployed to 50 pods handling 8.5K req/sec
- **65% infrastructure cost reduction**
- Accuracy: 94.1% (1% lower than BERT, acceptable tradeoff)

## Key Takeaway

Sometimes the fanciest model isn't the right choice. DistilBERT showed that simpler,
well-optimized models beat heavy architectures for production. The 1% accuracy loss was worth the 
massive speed and cost gains.
