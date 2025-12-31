# Kubernetes Demo

This repository contains Kubernetes configuration files for a containerized web application with MongoDB backend.

## Project Overview

This is a demonstration project that shows how to deploy a multi-tier application on Kubernetes. It includes:
- A MongoDB database
- A web application that connects to MongoDB
- Kubernetes deployment and service configurations

## Architecture

The project consists of the following components:

### MongoDB Database
- **Deployment**: `mongo.yaml` - Deploys MongoDB container (v6.0)
- **Service**: Internal service for database connectivity
- **Configuration**: Uses ConfigMap and Secret for credentials

### Web Application
- **Deployment**: `webapp.yaml` - Deploys the web app (nanajanashia/k8s-demo-app:v1.0)
- **Service**: NodePort service exposing the app on port 30100
- **Environment**: Configured via secrets and config maps

## Files

- **mongo.yaml** - MongoDB Deployment and Service configuration
- **webapp.yaml** - Web application Deployment and NodePort Service configuration
- **mongo-secret.yaml** - Kubernetes Secret containing MongoDB credentials
- **mongo-config.yaml** - Kubernetes ConfigMap containing MongoDB URL

## Credentials

The secret credentials are base64 encoded:
- **Username**: mongousér
- **Password**: mongopassword

> ⚠️ Note: For production use, use a proper secret management solution instead of base64 encoding.

## Accessing the Application

Once deployed to Kubernetes:
- **Web Application URL**: `http://<NodeIP>:30100`
- **MongoDB Service**: `mongo-service:27017` (internal cluster access)

## Prerequisites

- Kubernetes cluster (v1.19+)
- kubectl configured to access your cluster

## Deployment

Apply the configurations to your Kubernetes cluster:

```bash
# Apply secret and config
kubectl apply -f mongo-secret.yaml
kubectl apply -f mongo-config.yaml

# Apply database
kubectl apply -f mongo.yaml

# Apply web application
kubectl apply -f webapp.yaml
```

To verify deployment:

```bash
# Check pods
kubectl get pods

# Check services
kubectl get svc

# Check logs
kubectl logs -f deployment/mongo-deployment
kubectl logs -f deployment/webapp-deployment
```

## Cleanup

To remove all resources:

```bash
kubectl delete -f mongo-secret.yaml
kubectl delete -f mongo-config.yaml
kubectl delete -f mongo.yaml
kubectl delete -f webapp.yaml
```

Or delete everything at once:

```bash
kubectl delete -f .
```

## Notes

- MongoDB is set to 1 replica for demo purposes
- The web application runs on port 3000 internally, exposed via NodePort on port 30100
- Both services are configured to communicate within the cluster using service DNS names
