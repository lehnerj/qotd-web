# 📦 qotd-web

A lightweight **Quote of the Day (QOTD)** web application packaged and deployed using **Helm** for Kubernetes.

This repository contains both:
- A simple web service that returns a quote
- A Helm chart for deploying it into a Kubernetes cluster

---


## 🚀 What This Repo Contains

This is a **minimal, demo-friendly microservice** used for:

- Kubernetes platform validation
- Helm chart testing
- Demo environments and workshops
- CI/CD pipeline validation

The application itself is intentionally simple — the focus is on **deployment patterns**, not business logic.

---

## 🏗️ Repository Structure

```
qotd-web/
├── chart/                 # Helm chart
│   ├── templates/         # Kubernetes manifests
│   ├── values.yaml        # Default configuration
│   └── Chart.yaml         # Chart metadata
├── Dockerfile             # Container image definition
├── src/                   # Application source code
└── README.md
```

### Key Observations

- The app is **stateless**
- No database or persistence layer
- Helm chart drives all runtime configuration
- Designed for **simple HTTP response serving**

---

## ⚙️ Application Behavior

The application:

- Runs a small HTTP server
- Returns a quote (static or simple rotation)
- Exposes a single endpoint (typically `/`)

This makes it ideal for:

- Smoke testing clusters
- Validating ingress/controllers
- Demonstrating scaling behaviour

---

## ☸️ Helm Chart Overview

The Helm chart follows standard Helm structure:

- `templates/` → Kubernetes manifests
- `values.yaml` → configurable runtime values
- `Chart.yaml` → chart metadata

---

## 📦 Deployment

### Clone the repository

```bash
git clone https://github.com/IBM/qotd-web.git
cd qotd-web
```

### Install with Helm

```bash
helm install qotd-web ./chart
```

This will:

- Create a Deployment
- Expose a Service
- Start the qotd-web pod

### Verify deployment

```bash
kubectl get pods
kubectl get svc
```

---

## 🌍 Accessing the Application

### Option 1: Port Forward

```bash
kubectl port-forward svc/qotd-web 8080:80
```

Then open:

```
http://localhost:8080
```

### Option 2: Ingress (if enabled)

If your chart supports ingress:

```yaml
ingress:
  enabled: true
```

Then configure DNS or `/etc/hosts`.

---

## ⚙️ Configuration (values.yaml)

Typical configurable options:

```yaml
replicaCount: 1

image:
  repository: qotd-web
  tag: latest
  pullPolicy: IfNotPresent

service:
  type: ClusterIP
  port: 80
```

### What These Control

- `replicaCount` → scaling
- `image.*` → container source
- `service.*` → networking exposure

---

## 🔄 Scaling

Because the app is stateless, scaling is trivial:

```bash
kubectl scale deployment qotd-web --replicas=3
```

---

## 🧪 Health & Debugging

### Logs

```bash
kubectl logs deploy/qotd-web
```

### Describe resources

```bash
kubectl describe pod <pod-name>
```

---

## 🐳 Building the Image

If you modify the app:

```bash
docker build -t qotd-web:latest .
```

Push to your registry and update:

```yaml
image:
  repository: <your-repo>/qotd-web
```

---

## 🎯 Design Principles

This project intentionally follows:

- Minimal dependencies
- Stateless architecture
- Kubernetes-first design
- Helm-driven configuration

---

## 📊 Use Cases

- Demo app for Kubernetes clusters
- Testing ingress / service mesh
- CI/CD pipeline validation
- Helm chart development practice
- SRE onboarding labs

---

## ⚠️ Limitations

- No persistence
- No authentication
- No API backend
- Not production-hardened

---

## 🔮 Possible Enhancements

- Add `/health` endpoint
- Add Prometheus metrics
- External quote API integration
- Configurable quotes via ConfigMap
- OpenTelemetry tracing

---

## 🤝 Contributing

1. Fork the repo  
2. Create a branch (`feature/my-feature`)  
3. Commit changes  
4. Open a Pull Request  

---

## 📜 License

See repository license.

---

## 🧠 Maintainers Notes (Useful for SREs)

- Good candidate for canary deployments
- Useful for testing autoscaling (HPA)
- Can simulate traffic easily
- Ideal for validating:
  - Ingress
  - DNS
  - TLS
  - Service mesh routing
