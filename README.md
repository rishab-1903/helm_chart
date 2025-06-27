# 🛠️ Helm Deployment for 3-Tier Web App on GKE with static frontend & flask app backend code.

This repository contains a Helm chart to deploy a simple **3-tier application** on **Google Kubernetes Engine (GKE)**. It includes:

* 📦 **Flask Backend API** (`/health`, `/api/products`)
* 🌐 **Static HTML Frontend** using Bootstrap
* ☁️ **Cloud SQL (PostgreSQL)** – manually created with public IP
* 🌍 **Ingress** for unified access - in /templates/frontend-ingress.yaml

---

## 📁 Folder Structure

'''
helm\_chart/
├── backend/
│   ├── .gitignore
│   ├── Dockerfile
│   ├── app.py
│   └── requirements.txt
├── frontend/
│   ├── Dockerfile
│   └── index.html
├── templates/
│   ├── backend-config.yaml
│   ├── backend-deployment.yaml
│   ├── backend-service.yaml
│   ├── frontend-deployment.yaml
│   ├── frontend-ingress.yaml
│   └── frontend-service.yaml
├── Chart.yaml
├── values.yaml
└── README.md
'''

---

## ⚙️ Prerequisites

* Google Cloud SDK (`gcloud`)
* Kubernetes CLI (`kubectl`)
* Helm
* A Google Cloud project with:

  * GKE enabled
  * A **Cloud SQL PostgreSQL instance** created manually
  * Docker images for frontend and backend pushed to **GCR**

---

## 🚀 Create and Configure GKE Cluster

'''
gcloud auth login
gcloud config set project YOUR\_PROJECT\_ID

# Create cluster

gcloud container clusters create my-cluster&#x20;
\--zone us-central1-a&#x20;
\--num-nodes 2

# Get credentials for kubectl

gcloud container clusters get-credentials my-cluster --zone us-central1-a
'''

---

## 🧾 Configure `values.yaml`

Set environment variables and image tags:

'''
backend:
image: gcr.io/YOUR\_PROJECT\_ID/backend-image\:latest
env:
DB\_HOST: "CLOUD\_SQL\_PUBLIC\_IP"
DB\_NAME: "your\_database"
DB\_USER: "your\_user"
DB\_PASSWORD: "your\_password"

frontend:
image: gcr.io/YOUR\_PROJECT\_ID/frontend-image\:latest

ingress:
enabled: true
hostname: ""  # Leave blank to use external IP
'''

---

## 📦 Deploy with Helm

'''

# Go to the chart directory

cd helm\_chart/

# Install the Helm chart

helm upgrade --install my-app . --namespace default
'''

Check resource status:

'''
kubectl get pods
kubectl get svc
kubectl get ingress
'''

---

## 🌐 Access the App

Once the Ingress is ready:

'''
kubectl get ingress
'''

Use the **EXTERNAL-IP** to access:

* Frontend: `http://<INGRESS_IP>`
* Backend Health Check: `http://<INGRESS_IP>/health`
* Product API: `http://<INGRESS_IP>/api/products`

---

## 🧪 Example API Calls

'''
curl http\://\<INGRESS\_IP>/health

curl http\://\<INGRESS\_IP>/api/products
'''

---

## 🧹 Clean Up

'''
helm uninstall my-app

gcloud container clusters delete my-cluster --zone us-central1-a
'''

---

## 📌 Notes

* This is a basic public-IP setup for Cloud SQL. **Use private IP + Cloud SQL Proxy for production.**
* You can extend the `frontend-ingress.yaml` with host-based rules or TLS configuration.

---

## 🙋‍♂️ Maintainer

Created by [@rishab-1903](https://github.com/rishab-1903) – feel free to raise issues or contribute.
'''

Let me know if you'd like:

* Diagram/architecture illustration
* GitHub Action for image builds
* TLS support via HTTPS
* Sample Docker build & push commands

All are quick additions!
