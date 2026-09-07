# Screenshot Evidence Index

This index maps the captured evidence in `Screen_Shorts.docx` to the project requirements.

| Evidence | What it demonstrates |
|---|---|
| Screenshot 1 | GitHub repository forked from `UnpredictablePrashant/StreamingApp` |
| Screenshot 2 | Personal fork cloned and Git remote points to the student's repository |
| Screenshot 3 | Project structure and Dockerfiles for backend services/frontend |
| Screenshot 4 | Docker environment/setup evidence |
| Screenshot 5 | Docker Compose services running |
| Screenshot 6 | Local frontend returns HTML successfully |
| Screenshot 7 | Five application Docker images built |
| Screenshot 8 | AWS CLI authenticated and region configured |
| Screenshot 9 | Amazon ECR repositories created |
| Screenshot 10 | ECR image/tag push evidence |
| Screenshot 11 | Jenkins pipeline successful |
| Screenshot 12 | New ECR tags created by Jenkins (GitHub -> Jenkins -> ECR) |
| Screenshot 13 | GitHub webhook / test commit / automatic Jenkins trigger |
| Screenshot 14 | EKS nodes in Ready state |
| Screenshot 15 | Helm chart/template structure |
| Screenshot 16 | `helm lint` successful with 0 failed charts |
| Screenshot 17 | Kubernetes resources created; application pods running; Mongo PVC pending |
| Screenshot 18 | Frontend scaled from 2 replicas to 4 replicas |
| Screenshot 19 | Frontend rollout and rollout history evidence |
| Self-healing evidence | Deleted frontend pod automatically replaced by Kubernetes |
| Screenshot 20 | Final Helm chart file layout |

## Important Limitations Visible in Evidence

- MongoDB PVC is shown as `Pending` in the Kubernetes resource screenshot because of Auth,& streaming had issues, since EKS/ECR charges are high I deleted all resources immediately



