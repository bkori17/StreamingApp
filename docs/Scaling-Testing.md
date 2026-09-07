# Scaling and Testing

## Scaling

The frontend deployment was scaled from 2 replicas to 4 replicas.

```bash
kubectl get deployment frontend
kubectl scale deployment frontend --replicas=4
kubectl get deployment frontend
kubectl get pods -l app=frontend
```

## Rolling Update

A frontend image update was rolled out successfully. Evidence shows an update from the earlier image tag to a later version and a successful rollout status.

```bash
kubectl rollout status deployment/frontend
kubectl rollout history deployment/frontend
```

## Self-Healing

A frontend pod was deleted manually. Kubernetes recreated a replacement pod automatically to maintain the requested replica count.

```bash
kubectl delete pod <frontend-pod-name>
kubectl get pods -l app=frontend
```

## Functional Testing Status

Local frontend response was validated successfully before EKS deployment.

On EKS, infrastructure-level validation was completed for pods, services, ingress, scaling, rollout, and self-healing. Because MongoDB persistent storage was still pending, this submission does not claim complete login/upload/playback/chat validation with persistent data.
