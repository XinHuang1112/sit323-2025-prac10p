# 10.1P Monitoring & Visibility Report

## 1. Environment Setup
1.1 **Authenticate & select project**  
   ```bash
   gcloud auth login
   gcloud config set project sit323-2025-prac10
   ```
1.2**Enable required APIs**
  ```bash
  gcloud services enable \
  container.googleapis.com \
  artifactregistry.googleapis.com \
  monitoring.googleapis.com \
  logging.googleapis.com
  ```
1.3**Configure Docker authentication**
  ```bash
  gcloud auth configure-docker asia-southeast1-docker.pkg.dev
  ```
## 2. Build & Push Container Image
2.1**Build locally**
  ```bash
  docker build -t my-cloud-app:latest .
  ```
2.2**Tag & push to Artifact Registry**
  ```bash
  docker tag my-cloud-app:latest \
  asia-southeast1-docker.pkg.dev/sit323-2025-prac10/sit323-repo/my-cloud-app:latest

  docker push asia-southeast1-docker.pkg.dev/sit323-2025-prac10/sit323-repo/my-cloud-app:latest
  ```
## 3.Create GKE Cluster (with Logging & Monitoring)**
  ```bash
  gcloud container clusters create cloud-app-cluster \
  --project=sit323-2025-prac10 \
  --zone=asia-southeast1-a \
  --num-nodes=2 \
  --logging=SYSTEM \
  --monitoring=SYSTEM \
  --disk-size=50

  gcloud container clusters get-credentials cloud-app-cluster \
  --project=sit323-2025-prac10 \
  --zone=asia-southeast1-a
  ```
## 4.Deploy the Application
  ```bash
  kubectl apply -f k8s/app/app-deployment.yaml
  kubectl apply -f k8s/app/app-service.yaml

  kubectl get pods -l app=myapp
  kubectl get svc app-service
  ```
## 5.Create Monitoring Dashboard
5.1**Save the `monitoring-dashboard.json` file in the project root:**

5.2.**Create the dashboard via CLI:**
 ```bash
    gcloud monitoring dashboards create \
  --project=sit323-2025-prac10 \
  --config-from-file=monitoring-dashboard.json
 ```
## 6.View Application Logs
6.1**In the Cloud Console, go to Logging → Logs Explorer.**
6.2**Select Resource: GKE Container, then add a filter:**
 `cluster_name="cloud-app-cluster"`
6.3**Run the query to see your `myapp` container logs.**

## 7.Challenges & Solutions
- **Dashboard JSON import issues**: had issues to import json file so I switched to CLI gcloud monitoring dashboards create.

## 8. Conclusion
**Building on the 9.1P database integration, we have:**

- Containerized and pushed our app to Artifact Registry

- Deployed to a GKE cluster with built-in Cloud Monitoring & Logging

- Created a custom dashboard via JSON & CLI showing CPU and memory metrics

- Verified live logs in Logs Explorer












  
