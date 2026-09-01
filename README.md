# flask-microservices

A minimal Flask microservice application designed for microservices testing in lab environments and GitOps deployment. This project demonstrates a containerized Flask application with Kubernetes Helm deployment configurations.

## Project Structure

```
flask-microservices/
├── app/                              # Flask application code
│   ├── app.py                        # Main Flask application
│   └── requirements.txt               # Python dependencies
├── charts/                           # Helm charts for Kubernetes deployment
│   └── flask-microservice/           # Helm chart for the Flask microservice
│       ├── Chart.yaml                # Helm chart metadata
│       ├── values.yaml               # Default configuration values
│       └── templates/                # Kubernetes resource templates
│           ├── deployment.yaml       # Kubernetes Deployment
│           ├── service.yaml          # Kubernetes Service
│           ├── hpa.yaml              # Horizontal Pod Autoscaler
│           └── _helpers.tpl          # Helm template helpers
├── Dockerfile                        # Docker image definition
└── README.md                         # This file
```

## Features

### Flask Application
- **Index Endpoint** (`GET /`) - Returns JSON response with service status
- **Health Check Endpoint** (`GET /healthz`) - Returns health status for load balancers and orchestration platforms
- **Production Ready** - Uses Gunicorn as WSGI server for production deployments

### Docker Support
- Python 3.12-slim base image for minimal footprint
- Gunicorn server bound to `0.0.0.0:5000`
- Optimized requirements installation with `--no-cache-dir`

### Kubernetes Deployment
- Helm chart for easy Kubernetes deployment
- Configurable replica count and resource limits
- Horizontal Pod Autoscaler support (disabled by default)
- ClusterIP service on port 80 (maps to container port 5000)
- Resource requests and limits configured
- Container port exposure (5000)

## Quick Start

### Local Development

1. **Install dependencies:**
   ```bash
   cd app
   pip install -r requirements.txt
   ```

2. **Run the application:**
   ```bash
   python app.py
   ```

   The application will start on `http://localhost:5000`

3. **Test the endpoints:**
   ```bash
   # Health check
   curl http://localhost:5000/healthz
   
   # Main endpoint
   curl http://localhost:5000/
   ```

### Docker

1. **Build the image:**
   ```bash
   docker build -t flask-microservice:latest .
   ```

2. **Run the container:**
   ```bash
   docker run -p 5000:5000 flask-microservice:latest
   ```

### Kubernetes with Helm

1. **Install the Helm chart:**
   ```bash
   helm install flask-release charts/flask-microservice/
   ```

2. **Upgrade an existing release:**
   ```bash
   helm upgrade flask-release charts/flask-microservice/
   ```

3. **Customize deployment with values:**
   ```bash
   helm install flask-release charts/flask-microservice/ \
     --set replicaCount=3 \
     --set image.tag=v1.0.0
   ```

## Dependencies

### Python
- **flask==3.0.3** - Web framework
- **gunicorn==22.0.0** - WSGI HTTP server

### Kubernetes
- Kubernetes cluster (1.20+)
- Helm 3.x

## Configuration

### Helm Chart Values
The default values can be customized in `charts/flask-microservice/values.yaml`:

- **replicaCount** - Number of pod replicas (default: 1)
- **image.repository** - Docker image repository (default: docker.io/charungooa/flask-microservice)
- **image.tag** - Image tag (default: latest)
- **image.pullPolicy** - Image pull policy (default: IfNotPresent)
- **service.type** - Kubernetes service type (default: ClusterIP)
- **service.port** - Service external port (default: 80)
- **service.targetPort** - Container target port (default: 5000)
- **resources.requests** - CPU/memory requests (50m CPU, 64Mi memory)
- **resources.limits** - CPU/memory limits (250m CPU, 128Mi memory)
- **autoscaling.enabled** - Enable Horizontal Pod Autoscaler (default: false)

## Endpoints

| Method | Endpoint | Description | Response |
|--------|----------|-------------|----------|
| GET | `/` | Main service endpoint | `{"message": "hello from flask-microservice", "status": "ok"}` |
| GET | `/healthz` | Health check endpoint | `{"status": "healthy"}` (HTTP 200) |

## Use Cases

- **Microservices Testing** - Use this as a base microservice for testing microservices patterns and communication
- **GitOps Deployment** - Deploy using GitOps tools (Flux, ArgoCD) with the included Helm charts
- **Kubernetes Learning** - Learn Kubernetes deployment patterns with a simple, real-world example
- **Lab Environment** - Quick deployment for testing and lab scenarios

## License

This project is open source and available under the terms of your choice.

## Author

Charungooa - [GitHub Profile](https://github.com/Charungooa)
