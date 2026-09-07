# Kubernetes and Helm

## Helm Chart Layout

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

## Kubernetes Objects

- One Deployment + ClusterIP Service per application component
- ConfigMap for non-secret configuration
- Secret for sensitive configuration placeholders/runtime values
- MongoDB StatefulSet and PVC
- Ingress for external routing

## Validation

```bash
helm lint .
helm template streamingapp .
kubectl get pods,svc,statefulset,pvc,ingress
```

## Ingress Routing

```text
/               -> frontend-svc:80
/api/auth       -> auth-svc:3001
/api/streaming  -> streaming-svc:3002
/api/admin      -> admin-svc:3003
/api/chat       -> chat-svc:3004
```

The Ingress used host `streamingapp.local` and received an external AWS ELB address.

## MongoDB Status

The MongoDB StatefulSet and service were created, but the MongoDB PVC was still `Pending` at the time of screenshot capture. This is recorded as a known limitation rather than represented as a successful persistence deployment.
