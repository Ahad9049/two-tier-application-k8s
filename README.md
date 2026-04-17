# 🐳 Two-Tier Flask App with MySQL on Kubernetes

![Python](https://img.shields.io/badge/Python-3.11-blue?logo=python&logoColor=white)
![Flask](https://img.shields.io/badge/Flask-Web%20App-black?logo=flask)
![MySQL](https://img.shields.io/badge/MySQL-Database-orange?logo=mysql&logoColor=white)
![Kubernetes](https://img.shields.io/badge/Kubernetes-KIND-326CE5?logo=kubernetes&logoColor=white)
![Docker](https://img.shields.io/badge/Docker-Containerized-2496ED?logo=docker&logoColor=white)
![AWS](https://img.shields.io/badge/AWS-EC2-FF9900?logo=amazon-aws&logoColor=white)

A production-ready **two-tier architecture** deploying a Flask web application with a MySQL database on **Kubernetes (KIND)** running inside **AWS EC2**. Credentials are managed securely via **Kubernetes Secrets** — never hardcoded.

---

## 📐 Architecture Overview

```
                        ┌─────────────────────────────────────┐
                        │           AWS EC2 Instance           │
                        │                                      │
                        │   ┌──────────────────────────────┐   │
                        │   │      KIND Kubernetes Cluster  │   │
                        │   │                              │   │
                        │   │  ┌──────────┐  ┌──────────┐ │   │
                        │   │  │  Flask   │  │  MySQL   │ │   │
                        │   │  │  Pod     │──│  Pod     │ │   │
                        │   │  │(frontend │  │(database)│ │   │
                        │   │  │  + API)  │  │          │ │   │
                        │   │  └──────────┘  └──────────┘ │   │
                        │   │        │             │        │   │
                        │   │  ┌─────┴─────────────┴────┐  │   │
                        │   │  │    Kubernetes Secrets   │  │   │
                        │   │  │  (DB credentials, keys) │  │   │
                        │   │  └─────────────────────────┘  │   │
                        │   └──────────────────────────────┘   │
                        └─────────────────────────────────────┘
```

**Tier 1 — Application:** Flask web app serving the frontend UI and REST API  
**Tier 2 — Data:** MySQL database storing user and transaction data

---

## 🚀 Features

- **Kubernetes on KIND** — Full K8s orchestration on a single EC2 instance (no EKS cost)
- **Kubernetes Secrets** — DB credentials managed securely, never in plain text
- **Docker multi-stage build** — Optimized, lightweight container image
- **CI/CD ready** — GitHub integrated for rapid deployment
- **Cost-effective PoC** — Proves scalability and high availability without cloud overhead

---

## 📁 Project Structure

```
two-tier-application-k8s/
├── k8s-manifests/          # Kubernetes manifests organized by component
│   ├── flask-app/          # Flask application resources
│   │   ├── deployment.yaml
│   │   └── service.yaml
│   ├── mysql-db/           # MySQL database resources
│   │   ├── deployment.yaml
│   │   └── service.yaml
│   └── namespaces/         # Namespace definitions
│       ├── flask-app.yaml
│       └── mysql-db.yaml
├── templates/              # Flask HTML templates (Jinja2)
├── twotier-env/            # Environment configuration files
├── .env                    # Local environment variables (NOT committed)
├── Dockerfile              # Docker build instructions for Flask app
├── Makefile                # Convenience commands for build & deploy
├── app.py                  # Flask application entry point
├── kind-config.yaml        # KIND cluster configuration
├── message.sql             # MySQL schema and seed data
├── requirements.txt        # Python dependencies
└── README.md
```

---

## 🛠️ Tech Stack

| Layer | Technology |
|---|---|
| Web Framework | Flask (Python) |
| Database | MySQL |
| Containerization | Docker |
| Orchestration | Kubernetes (KIND) |
| Cloud | AWS EC2 |
| Secret Management | Kubernetes Secrets |

---

## ⚙️ Prerequisites

- AWS EC2 instance (Ubuntu 22.04 recommended)
- Docker installed
- kubectl installed
- KIND installed
- Python 3.11+

---

## 🏃 Quick Start

### 1. Clone the repository

```bash
git clone https://github.com/Ahad9049/two-tier-application-k8s.git
cd two-tier-application-k8s
```

### 2. Create the KIND cluster

```bash
kind create cluster --config kind-config.yaml --name two-tier
```

### 3. Build and load the Docker image

```bash
docker build -t flask-app:latest .
kind load docker-image flask-app:latest --name two-tier
```

### 4. Create Kubernetes Secrets

```bash
kubectl create secret generic mysql-secret \
  --from-literal=MYSQL_ROOT_PASSWORD=<your-password> \
  --from-literal=MYSQL_DATABASE=<your-db-name> \
  --from-literal=MYSQL_USER=<your-user> \
  --from-literal=MYSQL_PASSWORD=<your-password>
```

### 5. Apply Kubernetes manifests

```bash
kubectl apply -f k8s-manifests/namespaces/
kubectl apply -f k8s-manifests/mysql-db/
kubectl apply -f k8s-manifests/flask-app/
```

### 6. Verify pods are running

```bash
kubectl get pods
kubectl get services
```

### 7. Access the application

```bash
# Port-forward to access locally
kubectl port-forward svc/flask-service 5000:5000
```

Then open: `http://localhost:5000`

---

## 🔐 Security Design

Credentials are **never hardcoded** in any file. All sensitive values are injected at runtime via Kubernetes Secrets:

```yaml
env:
  - name: MYSQL_PASSWORD
    valueFrom:
      secretKeyRef:
        name: mysql-secret
        key: MYSQL_PASSWORD
```

> **Rule:** If it's a secret, it lives in a Kubernetes Secret — not in your repo.

---

## 🗄️ Database Setup

The `message.sql` file contains the schema. It is automatically applied when the MySQL pod initializes. To apply manually:

```bash
kubectl exec -it <mysql-pod-name> -- mysql -u root -p < message.sql
```

---

## 🧹 Cleanup

```bash
# Delete all resources
kubectl delete -f k8s-manifests/flask-app/
kubectl delete -f k8s-manifests/mysql-db/
kubectl delete -f k8s-manifests/namespaces/

# Delete the KIND cluster
kind delete cluster --name two-tier
```

---

## 📌 Industry Use Case

> A fintech startup building a **personal expense tracking platform** needs a PoC on AWS that proves scalability, high availability, and security — while staying cost-effective. This project delivers exactly that.

---

## 👨‍💻 Author

**Abdul Ahad**  
Junior DevOps Engineer | Docker · Kubernetes · AWS · CI/CD  
GitHub: [@Ahad9049](https://github.com/Ahad9049)

---

## 📄 License

This project is open source and available under the [MIT License](LICENSE).
