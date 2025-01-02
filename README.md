# Jenkins CI CD Pipeline

This repository contains a Jenkins Pipeline that builds, scans, tests, and deploys a react web app application to a Kubernetes cluster.

### Using
- [Docker](https://hub.docker.com/)
- [Kubernetes](https://kubernetes.io/)
- [Jenkins](https://www.jenkins.io/)
- [Sonarkube](https://www.sonarsource.com/)
- [React](https://reactjs.org/)
- [Git](https://git-scm.com/)

### Pipeline
The pipeline consists of the following stages:
1. Checkout: Checkout the code from the Git repository.
2. Build: Build the react web app using the `npm run build` command.
3. Scan: Scan the built react web app using sonarkube server on port 9000.
4. Deploy: Deploy the react web app to the Kubernetes cluster using the `kubectl apply` command.

### Kubernetes Deployment
The Kubernetes deployment is defined in the `Kubernetes/deployment.yaml` file. It specifies the container image to use, the number of replicas, and the port to expose.

### Architecture 
![Description](architecture.svg)

