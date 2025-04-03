
# **Overview**

| **Layer**        | **Tool**                              |
| ---------------- | ------------------------------------- |
| Model Training   | PyTorch, LightFM                      |
| Optimization     | TorchScript, ONNX                     |
| Serving          | FastAPI, Triton Server                |
| Containerization | Docker                                |
| Orchestration    | Kubernetes (EKS)                      |
| Monitoring       | Prometheus, Grafana                   |
| CI/CD            | GitHub Actions                        |
| Cloud            | AWS: EKS, ECR, S3, IAM                |
| A/B Testing      | NGINX, PostgreSQL, Prometheus Metrics |

This project delivers an **end-to-end real-time music recommendation pipeline**.

It covers everything from training implicit-feedback models, optimizing them for fast inference, deploying them to a scalable API infrastructure using **FastAPI**, and serving them efficiently with **NVIDIA Triton** on **AWS EKS**, monitored by **Prometheus/Grafana**, and maintained through **CI/CD with GitHub Actions**, and a built-in **A/B testing infrastructure** to compare multiple recommendation strategies in live environments.

---

## **Features**

- 🧠 **PyTorch Model**: BPR / MF with implicit feedback
    
- ⚡ **Model Optimization**: TorchScript & ONNX
    
- 🔌 **Serving API**: FastAPI + async inference
    
- 🎯 **Scalable Serving**: NVIDIA Triton + Kubernetes
    
- 📈 **Monitoring**: Prometheus + Grafana + Custom Metrics
    
- 🔄 **Feedback Logging**: For later model refinement
    
- ☁️ **AWS-Ready**: ECR, EKS, S3, IAM
    
- 🛠️ **CI/CD**: Automated build, test, deploy via GitHub Actions
	
- 🧪 **A/B Testing**: Live model experimentation with traffic splitting 

---

# **Algorithms**

| **Model**          | **Description**                                                 |
| ------------------ | --------------------------------------------------------------- |
| BPR                | Bayesian Personalized Ranking – optimized for implicit feedback |
| ALS                | Alternating Least Squares – scalable for matrix factorization   |
| GRU4Rec (optional) | Sequential recommendation with RNNs for dynamic user behavior   |

---

# **Project Structure**

```
music-recommender/
├── model/                # Training & export
├── api/                  # FastAPI recommender server
├── triton/               # Triton model repo & config.pbtxt
├── nginx/                # A/B routing reverse proxy
├── abtest/               # Feedback logging API, PostgreSQL logger
├── k8s/                  # K8s manifests for all services
├── monitor/              # Prometheus + Grafana dashboards
├── .github/workflows/    # GitHub Actions CI/CD
├── docker/               # Dockerfiles for all services
├── scripts/              # AWS deployment scripts
└── README.md             # This file
```

---

# **Sample API Request**

- **POST** /recommend

```json
{
  "user_id": 42,
  "top_k": 10
}
```


- **Response:**

```json
{
  "recommended_items": [123, 87, 456, 19, 654]
}
```


---

# **AWS Setup Guide**

### **1. Create ECR Repositories**


### **2. Upload Optimized Model to S3**


### **3. Deploy EKS Cluster**


### **4. Apply Kubernetes Manifests**


---

# **CI/CD: GitHub Actions**


```yaml
name: CI/CD for Music Recommender

on:
  push:
    branches: [main]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@v3

    - name: Login to ECR
      uses: aws-actions/amazon-ecr-login@v1

    - name: Build & Push
      run: |
        docker build -t ${{ secrets.ECR_REPO }}/music-api ./api
        docker push ${{ secrets.ECR_REPO }}/music-api

    - name: Deploy to EKS
      run: |
        aws eks update-kubeconfig --name music-recsys --region us-west-2
        kubectl apply -f k8s/
```


Secrets required:

- AWS_ACCESS_KEY_ID
    
- AWS_SECRET_ACCESS_KEY
    
- ECR_REPO
    
- AWS_REGION


---
# Monitoring (Grafana)

| **Metric**           | **Description**               |
| -------------------- | ----------------------------- |
| inference_latency_ms | Time to respond per request   |
| click_through_rate   | User engagement by group      |
| server_error_rate    | Failures in FastAPI or Triton |
| cpu_usage, mem_usage | Resource consumption per pod  |


---

# **A/B Testing Architecture**

- **User → /recommend endpoint**
    
- Split traffic via **NGINX reverse proxy**
    
- Group A → Model A (TorchScript via FastAPI)
    
- Group B → Model B (ONNX via Triton Server)
    
- Results (clicks, skips, latency) logged to PostgreSQL
    
- Metrics visualized in **Grafana dashboards**


### Feedback Logging Example

```json
{
  "user_id": 42,
  "group": "B",
  "clicked_item": 87,
  "skipped_items": [456, 654],
  "inference_time_ms": 103
}
```

-> Stored in PostgreSQL or DynamoDB for later evaluation.