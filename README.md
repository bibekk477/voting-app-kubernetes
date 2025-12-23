# Project Overview

This repository showcases a complete voting application deployed on Kubernetes, highlighting fundamental and intermediate Kubernetes patterns. It's an excellent learning resource for understanding how to structure Kubernetes manifests and manage multi-component applications in containerized environments.

# Project Structure

voting-app-kubernetes/
├── deployments/ # Deployment manifests for scaling and management
├── pods/ # Pod definitions and configurations
├── services/ # Service definitions for networking
└── README.md

# Architecture Overview

The voting application follows a typical multi-tier architecture deployed on Kubernetes:

- Pods Directory - Contains individual pod specifications demonstrating:
  Pod creation and configuration
  Container specifications
  Environment variables and configuration

- Services Directory - Defines networking and service discovery:

  Cluster IP services for internal communication
  NodePort services for external access

- Deployments Directory - Production-ready deployment configurations:

  ReplicaSets for pod scaling
  Rolling updates and rollbacks
  Self-healing capabilities

# Prerequisites

Before deploying this application, ensure you have:

- A running Kubernetes cluster (Minikube) - minikube start
- kubectl command-line tool installed and configured
- Basic understanding of Kubernetes concepts:

  Pods
  Services (ClusterIP, NodePort)
  Deployments
  ReplicaSets

# Tech Stack

Container Orchestration: Kubernetes
Configuration Format: YAML
Architecture: Microservices

# Deployment Steps (Recommended Order)

Deploy the application components in sequence to ensure dependencies are ready.

1. Deploy Redis
   kubectl apply -f deployments/redis-deployment.yaml
   kubectl apply -f services/redis-service.yaml

   Verify:

   kubectl get pods
   kubectl get svc

2. Deploy PostgreSQL
   kubectl apply -f deployments/postgres-deployment.yaml
   kubectl apply -f services/postgres-service.yaml

   Verify:

   kubectl get pods
   kubectl get svc

3. Deploy Voting App
   kubectl apply -f deployments/voting-app-deployment.yaml
   kubectl apply -f services/voting-app-service.yaml

4. Deploy Worker App (Deployment Only)

   Worker runs in the background and does not expose a service.

   kubectl apply -f deployments/worker-app-deployment.yaml

   Verify:

   kubectl get pods

5. Deploy Result App
   kubectl apply -f deployments/result-app-deployment.yaml
   kubectl apply -f services/result-app-service.yaml

6. Verify All Resources
   kubectl get deployments
   kubectl get pods
   kubectl get svc

All pods should be in the Running state.

6. Access Applications Using Minikube

   Expose the frontend services:

   Start Voting App
   minikube service voting-app-service

   Start Result App
   minikube service result-app-service

   Minikube will open each service in your browser and display the service URL.

# Common Operations

```bash
## Scaling Replicas

kubectl scale deployment <deployment-name> --replicas=3

## Follow logs in real-time

kubectl logs -f <pod-name>

## View logs from all pods in a deployment

kubectl logs -l app=<app-label> -f

## Update Deployment

kubectl apply -f deployments/<deployment-name>.yaml

## Check rollout status

kubectl rollout status deployment/<deployment-name>

## Rollback Deployment

kubectl rollout history deployment/<deployment-name>

## Rollback to previous version

kubectl rollout undo deployment/<deployment-name>

## Rollback to specific revision

kubectl rollout undo deployment/<deployment-name> --to-revision=2

## Execute Commands in Pod

kubectl exec -it <pod-name> -- /bin/sh

## Port Forward

hkubectl port-forward svc/<service-name> <local-port>:<service-port>

## Debugging

bashkubectl describe pod <pod-name>

## Check Service Connectivity


kubectl describe svc <service-name>
kubectl get endpoints <service-name>

```
