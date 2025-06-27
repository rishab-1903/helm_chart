# Product App Deployment with Helm on GKE

This project deploys a simple product listing application with:

- A static HTML/JS frontend
- A Flask backend (with PostgreSQL)
- Kubernetes-native services (`ClusterIP`)
- Routing via Google Cloud Ingress

---

## Folder Structure


rishab1903@rishab1903-NUC11PAHi5:~/helm_chart$ tree
.
├── Chart.yaml
├── templates
│   ├── backend-config.yaml
│   ├── backend-deployment.yaml
│   ├── backend-service.yaml
│   ├── frontend-deployment.yaml
│   ├── frontend-ingress.yaml
│   └── frontend-service.yaml
└── values.yaml


---

## Prerequisites

- A GKE cluster with Ingress enabled
- Docker images for both frontend and backend pushed to a container registry (e.g., GCR)
- Helm installed and configured

---

## Sample values.yaml

'''
frontend:
  image:
    repository: gcr.io/YOUR_PROJECT/frontend
    tag: latest

  service:
    type: ClusterIP
    port: 80
    targetPort: 80

  ingress:
    enabled: true

backend:
  image:
    repository: gcr.io/YOUR_PROJECT/backend
    tag: latest

  service:
    type: ClusterIP
    port: 5000
    targetPort: 5000
'''

---

## Deployment

1. Install the Helm chart:

'''
helm upgrade --install product-app ./product-frontend
'''

2. Check the Ingress IP:

'''
kubectl get ingress
'''

3. Access the app in your browser using the Ingress IP.

---

## Ingress Routing

The Ingress is configured to route:

- `/` to the frontend service
- `/api` to the backend service
- `/health` to the backend service

Ingress uses `spec.ingressClassName: gce`.

---

## Notes

- Both services use `ClusterIP` and are only exposed through Ingress.
- The frontend should use relative paths like `/api/products` for API access.
- The backend should have a working `/health` route on port 5000.

---
