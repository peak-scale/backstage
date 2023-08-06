# GKE Deployment

Steps to deploy Backstage to a Kubernetes cluster on GKE.

Inspired by <https://github.com/guymenahem/how-to-devops-tools/blob/main/backstage/README.md>

## Prerequisites

- GKE cluster exists: `ps-gke-ap-01`
- Artifact Registry exists: `ps-ar-docker-01`

## Build container image

```bash
yarn install --frozen-lockfile
yarn tsc
yarn build:backend
docker image build . -f packages/backend/Dockerfile --tag europe-west6-docker.pkg.dev/ps-backstage/ps-ar-docker-01/backstage:1.0.0 --platform linux/amd64
docker push europe-west6-docker.pkg.dev/ps-backstage/ps-ar-docker-01/backstage:1.0.0
```

## Deploy Kubernetes resources

```bash
kubectl create ns backstage
```

### PostgreSQL

```bash
kubectl apply -f deploy/gke/postgres -n backstage
```

### Backstage

Prepare the GitHub token, see `deploy/gke/backstage/template/bs-secret.yaml`.

```bash
kubectl apply -f deploy/gke/backstage -n backstage
```
