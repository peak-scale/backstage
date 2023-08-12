# GKE Deployment

Steps to deploy Backstage to a Kubernetes cluster on GKE.

Inspired by <https://github.com/guymenahem/how-to-devops-tools/blob/main/backstage/README.md>

## Prerequisites

- GKE cluster exists: `ps-gke-ap-01`
- Public IP exists: `ps-backstage-ip` (<https://cloud.google.com/kubernetes-engine/docs/how-to/managed-certs>)
- Artifact Registry exists: `ps-ar-docker-01`

## Build container image

```sh
yarn install --frozen-lockfile
yarn tsc
yarn build:backend
docker image build . -f packages/backend/Dockerfile --tag europe-west6-docker.pkg.dev/ps-backstage/ps-ar-docker-01/backstage:1.0.0 --platform linux/amd64
docker push europe-west6-docker.pkg.dev/ps-backstage/ps-ar-docker-01/backstage:1.0.0
```

## Deploy Kubernetes resources

```sh
kubectl create ns backstage
```

### PostgreSQL

```sh
kubectl apply -f deploy/gke/postgres -n backstage
```

### Backstage

Prepare the GitHub token, see `deploy/gke/backstage/template/bs-secret.yaml`.

```sh
kubectl apply -f deploy/gke/backstage -n backstage
```

### Example app

In order to test various features (TechDocs, Kubernetes plugin etc.), an example app is deployed:

```sh
kubectl apply -f examples/example-app/k8s
```

The Backstage component can then be imported into the catalog: <https://github.com/peak-scale/backstage/blob/main/examples/example-app/example-app-backstage.yaml>

### Shutdown

In order to save costs, the workloads can be scaled to zero:

```sh
kubectl scale deployment users-api-nginx --replicas 0 -n default
kubectl scale deployment backstage --replicas 0 -n backstage
kubectl scale deployment postgres --replicas 0 -n backstage
```

Scale the deployments back up:

```sh
kubectl scale deployment postgres --replicas 1 -n backstage
kubectl scale deployment backstage --replicas 1 -n backstage
kubectl scale deployment users-api-nginx --replicas 1 -n default
```
