```markdown
# Voting App on Kubernetes with Prometheus & Grafana Monitoring

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-v1.XX-blue.svg)](https://kubernetes.io/)
[![Prometheus](https://img.shields.io/badge/Prometheus-Enabled-orange.svg)](https://prometheus.io/)
[![Grafana](https://img.shields.io/badge/Grafana-Enabled-FF4088.svg)](https://grafana.com/)

A complete **three-tier voting application** deployed on a local **KinD (Kubernetes in Docker)** cluster, fully instrumented with **Prometheus** for metrics collection and **Grafana** for observability and visualization.

---

## 🚀 Overview

This project demonstrates a production-grade setup of a microservices-based voting application running on Kubernetes. It showcases:

- Containerized microservices (Python, .NET, Node.js)
- Persistent storage with PostgreSQL
- Message queuing with Redis
- Local Kubernetes cluster management with KinD
- Comprehensive monitoring stack using Prometheus + Grafana via Helm
- Service exposure and inter-service communication
- Resource monitoring dashboards (CPU, Memory, Bandwidth, etc.)

**Perfect for learning Kubernetes, DevOps practices, observability, and monitoring in a hands-on way.**

---

## 🏗️ Architecture

### Application Components

- **Vote (Frontend)**: Python/Flask web app for casting votes (Cats vs Dogs)
- **Redis**: In-memory data store for queuing votes
- **Worker**: .NET background worker that processes votes from Redis
- **PostgreSQL (DB)**: Persistent database for storing vote results
- **Result**: Node.js web app for real-time visualization of voting results

### Infrastructure

- **KinD Cluster**: Multi-node local Kubernetes cluster
- **Prometheus**: Scrapes metrics from cluster and applications
- **Grafana**: Rich dashboards for visualization and alerting
- **Kubernetes Services & Deployments**: Defined in `/k8s` directory

![Architecture Diagram](architecture.png) <!-- Replace with actual image if available -->

---

## 📁 Project Structure

```bash
Voting-App-using-K8s-Prometheus-Grafana/
├── k8s/                    # Kubernetes manifests (Deployments, Services)
│   ├── vote-deployment.yaml
│   ├── vote-service.yaml
│   ├── worker-deployment.yaml
│   ├── db-deployment.yaml
│   ├── redis-deployment.yaml
│   └── result-deployment.yaml
├── kind-cluster/           # KinD configuration
│   ├── config.yml
│   └── dashboard-adminuser.yml
├── vote/                   # Python Flask frontend
├── worker/                 # .NET worker
├── result/                 # Node.js results app
├── get_helm.sh             # Helm installer script
├── CPU usage.png
├── Bandwidth.png
└── README.md
```

---

## ✨ Features

- **Full-stack Microservices** on Kubernetes
- **Real-time Voting** with live results
- **Persistent Storage** using PVCs
- **Monitoring & Observability**
  - Prometheus metrics scraping
  - Pre-configured Grafana dashboards
  - Resource utilization tracking (CPU, Memory, Network)
- **Easy Local Setup** with KinD
- **Helm-based Monitoring Stack**
- **Scalable Deployments**

---

## 🛠️ Prerequisites

- Docker (latest version recommended)
- kubectl
- Helm (v3+)
- Git
- Minimum 4GB RAM (8GB+ recommended)

---

## 🚀 Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/Roay-Abdullah/Voting-App-using-K8s-Prometheus-Grafana.git
cd Voting-App-using-K8s-Prometheus-Grafana
```

### 2. Create KinD Cluster

```bash
# Create cluster with multi-node config
kind create cluster --config kind-cluster/config.yml --name voting-app

# Verify cluster
kubectl get nodes
```

### 3. Install Helm (if not installed)

```bash
chmod +x get_helm.sh
./get_helm.sh
```

### 4. Deploy Monitoring Stack (Prometheus + Grafana)

```bash
# Add Prometheus Helm repo
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

# Create monitoring namespace
kubectl create namespace monitoring

# Install kube-prometheus-stack
helm install prometheus prometheus-community/kube-prometheus-stack \
  --namespace monitoring \
  --set grafana.enabled=true \
  --set prometheus.prometheusSpec.serviceMonitorSelectorNilUsesHelmValues=false
```

### 5. Deploy the Voting Application

```bash
kubectl apply -f k8s/
```

### 6. Access the Applications

```bash
# Port-forward services
kubectl port-forward svc/vote 5000:5000
kubectl port-forward svc/result 5001:5001
kubectl port-forward svc/prometheus-operated -n monitoring 9090:9090
kubectl port-forward svc/prometheus-grafana -n monitoring 3000:80
```

- **Vote App**: http://localhost:5000
- **Results**: http://localhost:5001
- **Grafana**: http://localhost:3000 (default credentials: admin / prom-operator)
- **Prometheus**: http://localhost:9090

---

## 📊 Observability

### Grafana Dashboards

- Cluster resource usage
- Pod metrics
- Application-specific metrics
- Custom voting app dashboards

**Example Screenshots:**

![CPU Usage](CPU%20usage.png)
![Bandwidth](Bandwidth.png)

---

## 🔧 Customization

### Scaling

```bash
kubectl scale deployment/vote --replicas=3
```

### Adding Custom Metrics

1. Instrument your applications with Prometheus client libraries
2. Create ServiceMonitor CRDs
3. Import dashboards into Grafana

### Persistent Volumes

PostgreSQL uses a hostPath volume for local development. For production, use cloud storage (EBS, GCE PD, etc.).

---

## 🧪 Testing

- Cast votes via the frontend
- Watch real-time updates on the results page
- Monitor metrics in Prometheus/Grafana
- Check logs: `kubectl logs -f deployment/worker`

---

## 🛡️ Cleanup

```bash
# Delete application
kubectl delete -f k8s/

# Uninstall monitoring
helm uninstall prometheus -n monitoring

# Delete cluster
kind delete cluster --name voting-app
```

---

## 🏆 Technologies Used

- **Kubernetes** (KinD)
- **Docker**
- **Prometheus**
- **Grafana**
- **Helm**
- **Python/Flask**
- **.NET**
- **Node.js**
- **Redis**
- **PostgreSQL**

---

## 📝 Resume / Portfolio Description

**Project Title:**  
Voting Application with Kubernetes, Prometheus & Grafana Observability

**Description:**  
Designed and implemented a complete three-tier voting application deployed on a local Kubernetes (KinD) cluster. Integrated Prometheus for metrics collection and Grafana for visualization, demonstrating end-to-end observability, microservices architecture, and modern DevOps practices. Automated deployment using Kubernetes manifests and Helm charts.

**Key Achievements:**
- Successfully orchestrated multi-language microservices
- Implemented comprehensive monitoring and alerting
- Configured local multi-node Kubernetes environment
- Demonstrated real-time data processing and visualization

**Key Technologies:** Kubernetes, KinD, Prometheus, Grafana, Helm, Docker, Redis, PostgreSQL

---

## 🤝 Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the project
2. Create your feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

---

## 🙏 Acknowledgments

- Original voting app example from Docker
- Prometheus Community Helm Charts
- KinD project for local Kubernetes

---
