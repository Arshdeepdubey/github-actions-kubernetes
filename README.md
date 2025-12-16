# GitHub Actions CI/CD to Kubernetes

This repo holds the application code and CI/CD workflow for building a Node.js image and deploying it to Kubernetes. It works together with a separate configuration repo (`node-js-config/`) that stores the Kubernetes manifests.

## How the pieces fit together
- `github-actions-kubernetes-1/` (this repo): contains the sample Node.js app, Dockerfile, and GitHub Actions workflow that builds and pushes the image to Docker Hub. During the workflow it also checks out the config repo.
- `node-js-config/`: contains `deployment.yaml` and `service.yaml` used by the pipeline. The workflow in this repo applies those manifests to the cluster after the image push.
- Result: application image is built and published from this repo, then the manifests from the config repo are applied to the target cluster, keeping code and cluster configuration decoupled.

## Repository layout
- `app.js`: Express “Hello, Kubernetes” HTTP server on port 3000.
- `Dockerfile`: Builds the Node 14-based container image.
- `.github/workflows/ci-cd.yaml`: GitHub Actions pipeline that builds, pushes, then deploys using manifests from the config repo.
- `.github/trigger.txt`: empty file to help trigger workflows if needed.

## CI/CD pipeline walkthrough
Triggered on: pushes to `master`.

1) Checkout this repo’s code.  
2) Build and push image with Podman to Docker Hub: `docker.io/${DOCKERHUB_USERNAME}/demo-app:latest`.  
3) Checkout the Kubernetes config repo (`Arshdeepdubey/nodejs-k8s-config`, checked out to `nodejs-k8s-config/`).  
4) Run `kubectl apply -f` on `deployment.yaml` and `service.yaml` from the config repo against the runner’s kubeconfig (self-hosted runner expected to have cluster access).  

### Required secrets
- `DOCKERHUB_USERNAME`, `DOCKERHUB_TOKEN`: credentials to push the image.
- `K8S_CONFIG_TOKEN`: token with read access to the config repo.

### Runner prerequisites
- Self-hosted runner on macOS with Podman, Docker Hub auth allowed, and `kubectl` configured to point to the target cluster (workflow also prints `kubectl version` and `kubectl cluster-info` for diagnostics).

## Local development
```bash
npm install
node app.js
# App listens on http://localhost:3000
```

To build and run locally:
```bash
podman build -t demo-app:dev .
podman run -p 3000:3000 demo-app:dev
```

## Updating image version
- Change code as needed, then let the workflow push `demo-app:latest` to Docker Hub.  
- Ensure the config repo’s `deployment.yaml` uses the same image reference; update it when changing the tag or registry.

## New joiner checklist
- Ensure access to Docker Hub credentials and the Kubernetes cluster kubeconfig on the runner.  
- Verify `K8S_CONFIG_TOKEN` grants access to the config repo.  
- Confirm the image name/tag used in `node-js-config/deployment.yaml` matches what the workflow builds (`docker.io/<user>/demo-app:latest`).  
- Test locally with `node app.js`, then push to `master` to exercise the full pipeline.

## References
- GitHub Actions: https://docs.github.com/actions
- Podman: https://docs.podman.io
- Kubernetes Deployments: https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
- Kubernetes Services: https://kubernetes.io/docs/concepts/services-networking/service/