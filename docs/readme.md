# GitHub Workflows

## Backend CI

- Trigger: Push to main
- Build Maven project
- Build Docker image
- Push image to Docker Hub

---

## Frontend CI

- Trigger: Push to main
- Build React application
- Build Docker image
- Push image to Docker Hub

---

## Deploy

- Configure AWS credentials
- Update kubeconfig
- Apply Kubernetes manifests
- Wait for rollout

---

## Rollback

- Roll back to the previous Deployment revision
- Verify rollout status