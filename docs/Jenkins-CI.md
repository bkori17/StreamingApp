# Jenkins CI

## Jenkins Environment

The shared Hero Vired Jenkins instance supplied for the assignment was used instead of installing a separate Jenkins server.

## Pipeline Purpose

```text
GitHub
  -> Jenkins checkout
  -> Amazon ECR login
  -> Build 5 images
  -> Tag images
  -> Push images to ECR
```

## Evidence Captured

- Successful Jenkins pipeline execution
- Successful stages for checkout, ECR login, image build, tag/push and post actions
- New ECR tags created by Jenkins
- GitHub webhook configured
- Test commit pushed to GitHub
- Pipeline triggered automatically

## Security

AWS access keys, GitHub tokens, and passwords must not be stored directly in the Jenkinsfile or committed to GitHub. Jenkins credentials should be used instead.

## Deployment Boundary

The assignment implementation keeps Jenkins focused on CI through Amazon ECR. Helm and `kubectl` operations are executed manually from the administration/development machine for EKS deployment and validation.
