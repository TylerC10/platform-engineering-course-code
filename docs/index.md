## Python App Documentation

### Overview
This is a simple Flask application that exposes two HTTP endpoints for basic service introspection and health checking. It is designed to run in Kubernetes and is suitable as a starter service for CI/CD demonstrations and platform engineering exercises.

### Base URL
- Example ingress URL: `https://python-app.test.com`

### Endpoints
1) `GET /api/v1/info`
   - Returns current server time, the pod hostname, a friendly message, and deployment environment.
   - Example response:
     ```json
     {
       "time": "01:23:45PM  on September 16, 2025",
       "hostname": "python-app-6f7c9b7cdb-abc12",
       "message": "You are doing great, little human!! <3",
       "deployed_on": "kubernetes"
     }
     ```

2) `GET /api/v1/healthz`
   - Liveness/readiness style endpoint. Returns 200 OK when the app is up.
   - Example response:
     ```json
     { "status": "up" }
     ```

### Quick Start (Local)
Requirements:
- Python 3.10+
- `pip`

Steps:
```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
python src/app.py
```

Test locally:
```bash
curl -s http://127.0.0.1:5000/api/v1/healthz | jq .
curl -s http://127.0.0.1:5000/api/v1/info | jq .
```

### Access in Kubernetes
- Health check: `https://python-app.test.com/api/v1/healthz`
- Info: `https://python-app.test.com/api/v1/info`

If using the provided Helm chart, make sure your DNS (or `/etc/hosts`) points `python-app.test.com` to your ingress controller.

### Deployment Notes
- Container image is built via CI and deployed to Kubernetes using Helm (see `.github/workflows/cicd.yaml`).
- Helm chart location: `charts/python-app`
- Kubernetes manifests (raw): `k8s/`

Common commands:
```bash
# Install/upgrade via Helm
helm upgrade --install python-app charts/python-app -n default

# Check resources
kubectl get deploy,svc,ingress
```

### Troubleshooting
- If `/api/v1/healthz` fails:
  - Verify pod is running: `kubectl get pods`
  - Check logs: `kubectl logs deploy/python-app`
  - Confirm ingress resolves to your cluster IP and TLS/hosts are correct.

### License
This repository is for learning purposes. Use as-is or adapt for your demos.