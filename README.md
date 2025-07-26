# 🌸 Iris Classifier API - CI/CD with Docker & Kubernetes

This project demonstrates a complete CI/CD pipeline for deploying a machine learning API using FastAPI, Docker, and Kubernetes with Google Cloud Platform.

## 🏗️ Architecture

- **API**: FastAPI with Iris flower classification model
- **Containerization**: Docker
- **Orchestration**: Google Kubernetes Engine (GKE)
- **CI/CD**: GitHub Actions with CML integration
- **Container Registry**: Google Artifact Registry

## 📋 Prerequisites

1. Google Cloud Project with the following APIs enabled:
   - Container Registry API
   - Kubernetes Engine API
   - Artifact Registry API

2. GitHub repository secrets configured:
   - `GCP_PROJECT_ID`: Your Google Cloud Project ID
   - `GCP_SA_KEY`: Service Account JSON key with necessary permissions
   - `GAR_LOCATION`: Artifact Registry location (e.g., us-central1)
   - `GAR_REPOSITORY`: Artifact Registry repository name
   - `GCP_REGION`: GCP region for deployment
   - `GKE_CLUSTER_NAME`: Kubernetes cluster name
   - `GKE_CLUSTER_ZONE`: Kubernetes cluster zone

## 🚀 Deployment Process

### Automated CI/CD Pipeline

The pipeline automatically triggers on:
- Pull requests to `main` branch (testing only)
- Pushes to `main` branch (testing + deployment)
- Pushes to `dev` branch (testing only)

### Pipeline Stages

1. **Code Quality Checks**
   - Black code formatting
   - Flake8 linting
   - API endpoint testing

2. **Docker Build & Push**
   - Build Docker image
   - Push to Google Artifact Registry
   - Tag with commit SHA and latest

3. **Kubernetes Deployment**
   - Deploy to GKE cluster
   - Create LoadBalancer service
   - Health checks and rollout verification

4. **CML Reporting**
   - Automated comments on PRs/commits
   - Deployment status reports
   - Performance metrics

## 🛠️ Manual Setup (if needed)

### 1. Build and Push Docker Image

```bash
# Build the image
docker build -t iris-image:v1 .

# Tag for Artifact Registry
docker tag iris-image:v1 LOCATION-docker.pkg.dev/PROJECT_ID/REPOSITORY/iris-image:v1

# Push to registry
docker push LOCATION-docker.pkg.dev/PROJECT_ID/REPOSITORY/iris-image:v1
```

### 2. Deploy to Kubernetes

```bash
# Get cluster credentials
gcloud container clusters get-credentials CLUSTER_NAME --zone=ZONE --project=PROJECT_ID

# Apply deployment
kubectl apply -f k8s-deployment.yaml

# Check deployment status
kubectl get pods
kubectl get services
```

## 📝 API Endpoints

- `GET /`: Health check endpoint
- `POST /predict/`: Iris flower classification

### Example Usage

```bash
# Health check
curl http://YOUR_EXTERNAL_IP/

# Make prediction
curl -X POST "http://YOUR_EXTERNAL_IP/predict/" \
  -H "Content-Type: application/json" \
  -d '{
    "sepal_length": 5.1,
    "sepal_width": 3.5,
    "petal_length": 1.4,
    "petal_width": 0.2
  }'
```

## 📊 Monitoring

The CI/CD pipeline includes:
- Automated testing reports via CML
- Deployment status tracking
- Resource utilization monitoring
- Service health checks

## 🔧 Local Development

```bash
# Install dependencies
pip install -r requirements.txt

# Run locally
uvicorn iris_fastapi:app --reload --port 8000

# Test endpoints
curl http://localhost:8000/
```

## 📦 Project Structure

```
.
├── .github/workflows/
│   └── iris_cicd_pipeline.yml    # CI/CD pipeline
├── iris_fastapi.py               # FastAPI application
├── model.joblib                  # Trained ML model
├── requirements.txt              # Python dependencies
├── Dockerfile                    # Container definition
├── k8s-deployment.yaml          # Kubernetes manifests
└── README.md                    # This file
```

## 🎯 Assignment Completion

This project fulfills the assignment requirements:
- ✅ FastAPI application for Iris classification
- ✅ Docker containerization
- ✅ Google Artifact Registry integration
- ✅ Google Kubernetes Engine deployment
- ✅ Continuous Deployment with GitHub Actions
- ✅ CML integration for automated reporting
