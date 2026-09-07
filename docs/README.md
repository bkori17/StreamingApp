# StreamingApp – Container Orchestration and Scaling

## Project Overview

This project demonstrates containerization, CI automation, container image publishing, Kubernetes orchestration, Helm-based deployment, ingress exposure, scaling, rolling updates, and self-healing for a multi-service StreamingApp.

The application consists of:

- `authService` – authentication and JWT handling
- `streamingService` – streaming/catalogue APIs
- `adminService` – administration and media-management APIs
- `chatService` – chat/WebSocket APIs
- `frontend` – React frontend served through Nginx
- `MongoDB` – shared persistence layer

## High-Level Architecture

```text
Developer
   |
   | git push
   v
GitHub Fork (bkori17/StreamingApp)
   |
   v
Hero Vired Jenkins
   |
   | Build / Tag / Push
   v
Amazon ECR
   |
   v
Amazon EKS
   |
   +--> Ingress
   |      |
   |      +--> frontend-svc
   |      +--> auth-svc
   |      +--> streaming-svc
   |      +--> admin-svc
   |      +--> chat-svc
   |
   +--> MongoDB StatefulSet + PVC
```

## Tools and Services Used

- Git and GitHub
- Docker and Docker Compose
- Amazon ECR
- Hero Vired shared Jenkins server
- AWS CLI
- Amazon EKS
- `kubectl`
- Helm 3
- NGINX Ingress Controller
- MongoDB StatefulSet / PersistentVolumeClaim

## Repository Workflow

The original StreamingApp repository was forked into the student's GitHub account and cloned to an Ubuntu EC2 development machine.

```bash
git clone https://github.com/bkori17/StreamingApp.git
cd StreamingApp
git remote -v
```

## Local Container Validation

The application was built and started using Docker Compose.

```bash
docker compose up --build
docker compose ps
```

The frontend returned HTML successfully during local validation.

Five application images were built:

```text
streaming-auth
streaming-stream
streaming-admin
streaming-chat
streaming-frontend
```

## Amazon ECR

Dedicated ECR repositories were created for the application components and images were pushed to ECR. Jenkins also created new image tags during CI execution.

Typical repository names used:

```text
streaming-auth
streaming-stream
streaming-admin
streaming-chat
streaming-frontend
```

## Jenkins CI Pipeline

The Hero Vired shared Jenkins server was used for CI.

The pipeline performs:

```text
GitHub checkout
    -> ECR login
    -> Build five Docker images
    -> Tag images
    -> Push images to Amazon ECR
```

A successful Jenkins pipeline execution was captured. A GitHub webhook was also configured and a test commit was used to demonstrate automatic pipeline triggering.

> Note: Jenkins is used for CI through ECR. EKS/Helm deployment is performed from the development/admin machine rather than automatically from Jenkins.

## Amazon EKS

An EKS cluster was created and verified with:

```bash
kubectl get nodes
```

Worker nodes reached `Ready` state.

## Helm Chart

The Helm chart is stored under:

```text
helm/streamingapp/
```

Expected structure:

```text
streamingapp/
├── Chart.yaml
├── values.yaml
└── templates/
    ├── auth-deployment.yaml
    ├── auth-service.yaml
    ├── streaming-deployment.yaml
    ├── streaming-service.yaml
    ├── admin-deployment.yaml
    ├── admin-service.yaml
    ├── chat-deployment.yaml
    ├── chat-service.yaml
    ├── frontend-deployment.yaml
    ├── frontend-service.yaml
    ├── mongo-statefulset.yaml
    ├── mongo-service.yaml
    ├── configmap.yaml
    ├── secret.yaml
    └── ingress.yaml
```

The chart was validated with:

```bash
helm lint .
```

and the lint check completed with no failed charts.

## Kubernetes Resources

The deployment creates:

- 5 Deployments
- 5 ClusterIP Services
- ConfigMap
- Secret
- MongoDB StatefulSet
- PersistentVolumeClaim
- Ingress

Validation commands:

```bash
kubectl get pods
kubectl get svc
kubectl get statefulset
kubectl get pvc
kubectl get ingress
kubectl get pods,svc,statefulset,pvc,ingress
```

At the time of evidence capture, the application service pods were running. MongoDB storage was not fully healthy because the MongoDB PVC remained in `Pending` state. This limitation is documented rather than hidden.

## Ingress

Ingress was created using the host:

```text
streamingapp.local
```

The Ingress received an AWS ELB address and routes traffic to the frontend and backend services.

Expected routes:

```text
/               -> frontend-svc:80
/api/auth       -> auth-svc:3001
/api/streaming  -> streaming-svc:3002
/api/admin      -> admin-svc:3003
/api/chat       -> chat-svc:3004
```

## Scaling Demonstration

The frontend deployment was scaled from 2 replicas to 4 replicas.

Example command:

```bash
kubectl scale deployment frontend --replicas=4
kubectl get deployment frontend
kubectl get pods -l app=frontend
```

## Rolling Update Demonstration

A frontend rolling update was demonstrated using a newer image tag, and rollout status completed successfully.

```bash
kubectl rollout status deployment/frontend
kubectl rollout history deployment/frontend
```

## Self-Healing Demonstration

A frontend pod was manually deleted and Kubernetes automatically created a replacement pod to maintain the desired replica count.

```bash
kubectl delete pod <frontend-pod-name>
kubectl get pods -l app=frontend
```

## Validation Status

### Successfully demonstrated

- GitHub fork and clone
- Project/Dockerfile structure
- Docker Compose execution
- Local frontend response
- Five Docker images built
- AWS CLI connectivity
- ECR repositories and pushed images
- Jenkins CI success
- GitHub webhook / automatic Jenkins trigger
- EKS worker nodes in Ready state
- Helm chart creation
- `helm lint` success
- Kubernetes application resources created
- Ingress resource created
- Scaling from 2 to 4 replicas
- Rolling update
- Pod self-healing


## Production Improvements

For a production deployment, I would separate workloads into namespaces, use HTTPS/TLS, store credentials in AWS Secrets Manager or another secret-management system, add Horizontal Pod Autoscaling, define CPU/memory requests and limits, add network policies, run the application across multiple availability zones, use a production-grade managed database, and configure complete centralized monitoring, logging, alerting, backup, and disaster-recovery policies.

## Evidence

See:

```text
docs/Screenshot-Evidence.md
```

and the submitted `Screen_Shorts.docx` evidence document.

## ECR image URI, not as a public web link.
## Amazon ECR Images

The Docker images for all application components were pushed to a private Amazon ECR registry.

- Auth Service  
  `378494867940.dkr.ecr.ap-south-1.amazonaws.com/streaming-auth:1.0.0`

- Streaming Service  
  `378494867940.dkr.ecr.ap-south-1.amazonaws.com/streaming-stream:1.0.0`

- Admin Service  
  `378494867940.dkr.ecr.ap-south-1.amazonaws.com/streaming-admin:1.0.0`

- Chat Service  
  `378494867940.dkr.ecr.ap-south-1.amazonaws.com/streaming-chat:1.0.0`

- Frontend  
  `378494867940.dkr.ecr.ap-south-1.amazonaws.com/streaming-frontend:1.0.0`

> Note: These images are stored in a private Amazon ECR registry and require AWS authentication and appropriate permissions for pull access.

## Cleanup

After grading, remove temporary AWS resources to avoid charges, especially the EKS cluster, EC2 worker nodes/load balancers, and development EC2 instance if no longer needed.
