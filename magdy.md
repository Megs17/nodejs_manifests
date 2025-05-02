# 🚀 Node.js App Deployment on Kubernetes with HPA, STATEFULSET DATABASE, TLS, NGINX INGRESS, EXTERNAL SECRETES BY AWS SECRET MANAGER

## 📋 Overview
This project demonstrates how to deploy a containerized Node.js application to a Kubernetes cluster using:
- Helm templating
- Nginx Ingress with HTTPS (Let's Encrypt)
- Horizontal Pod Autoscaler (HPA)
- Secure environment variables via Kubernetes Secrets and AWS Secret Manager
- Deploment for app and StatefulSet for Database

## 🛠️ Technologies Used
- Kubernetes
- Helm
- NGINX Ingress Controller
- Cert-Manager
- Let's Encrypt
- Horizontal Pod Autoscaler (HPA)
- No-IP Dynamic DNS
- External-Secrets
- AWS SECRET MANAGER
## 📦 Chart Structure

```
.
├── app-deployment.yaml       # Deployment manifest (Helm-templated)
├── app-service.yaml          # Service manifest
├── cluster-issuer.yaml       # Cert-Manager ClusterIssuer (Let's Encrypt)
├── hpa.yaml                  # Horizontal Pod Autoscaler
├── ingress.yaml              # HTTPS Ingress rule for custom domain
├── mysql-service.yaml        # Headless sql service
├── mysql-stateful.yaml       # Mysql statefulset yaml
├── redis-deployment.yaml     # redis-deployment
├── redis-service.yaml        # redis cluster ip service
├── externalsecret.yaml       # Secrets referenced from aws and passed to app 
└── secret-store.yaml         # Get Authentication to aws secrets
```

## . 🌐 DNS Setup
```
1. Visit [No-IP](https://www.noip.com/login?ref_url=console)
2. Create a free DNS record like `itiproject.zapto.org`
3. Point it to the public IP from:

```


## 🚀 Setup Instructions


### 1. 🌐 Prerequisites
```
- Kubernetes cluster (EKS)
- Helm installed
- kubectl configured
- DNS (e.g. [No-IP](https://www.noip.com)) configured to point to your cluster’s external IP (External Loadbalancer)
- AWS Secret defined
```
### 2. 📥 Install NGINX Ingress Controller

```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

helm install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx \
  --create-namespace \
  --set controller.service.type=LoadBalancer
```

### 3. 🔐 Enable HTTPS using Cert-Manager

```
# Add the Jetstack Helm repository
helm repo add jetstack https://charts.jetstack.io
helm repo update

# Install cert-manager with CRDs
helm install cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --create-namespace \
  --set installCRDs=true
```
### 4. 🔐 Install External-secrets
```
helm repo update

helm install external-secrets external-secrets/external-secrets
```