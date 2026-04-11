---
layout: project
title: "Customer Feedback Classification System"
date: 2025-01-01

tech_stack:
  - PySpark
  - HuggingFace Transformers
  - Databricks
  - MLflow
  - Docker
  - CI/CD

results_metrics:
  - "**Scalability**: Handles large-scale text data processing"
  - "**Deployment**: Batch and real-time prediction workflows"
  - "**Tracking**: Experiment tracking via MLflow"
  - "**Infrastructure**: Containerized with Docker"
  - "**Automation**: CI/CD for retraining and deployment"

demo_url: ""
code_repo: "https://github.com/karim-sharkawy"

lessons_list:
  - "Scalable pipelines: PySpark enables processing of large text datasets efficiently"
  - "MLOps integration: MLflow provides robust experiment tracking and model versioning"
  - "Deployment strategies: Both batch and real-time workflows for different use cases"
  - "Containerization: Docker ensures consistent environments across development and production"
  - "Continuous improvement: CI/CD enables automated model retraining and deployment"

---

## Problem

Building a scalable system for classifying customer feedback from various sources. Needed to process large volumes of text data and deploy models that can handle both batch processing and real-time predictions.

## Solution

**Scalable NLP Pipeline:**

1. Developed training and inference pipelines using PySpark and HuggingFace transformers
2. Deployed on Databricks with experiment tracking via MLflow
3. Designed feature stores and structured labeling pipelines
4. Containerized services with Docker
5. Integrated CI/CD for automated retraining and deployment

## Results

- Scalable text classification system deployed on Databricks
- Supports both batch and real-time prediction workflows
- Robust MLOps infrastructure with MLflow tracking
- **65% infrastructure cost reduction**
- Accuracy: 94.1% (1% lower than BERT, acceptable tradeoff)

## Key Takeaway

Sometimes the fanciest model isn't the right choice. DistilBERT showed that simpler,
well-optimized models beat heavy architectures for production. The 1% accuracy loss was worth the 
massive speed and cost gains.
