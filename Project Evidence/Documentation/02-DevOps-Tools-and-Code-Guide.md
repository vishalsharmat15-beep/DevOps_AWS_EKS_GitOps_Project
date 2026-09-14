# 02 DevOps Tools and Code Guide

Tool Guide and Code Reading Notes

This document explains the project from a DevOps engineer's point of view.

GitHub

GitHub stores two separate concerns:
- `register-app`: source code and CI pipeline

`GitOps-Fashion-Signup-App`: desired Kubernetes deployment state

A code change starts the CI flow. An image-tag change in the GitOps repo starts the Argo
CD deployment flow.

Jenkinsfile

File: `Git_Repos/Fashion-Register-App/Jenkinsfile`

The pipeline stages are:
- 1. **Clean Workspace**: removes files from the previous build.

2. **Checkout SCM**: checks out the application `main` branch.

3. **Build Application**: runs `mvn clean package`.

4. **Test Application**: runs `mvn test`.

5. **SonarQube Analysis**: sends source analysis to SonarQube using the explicit scanner

version.

6. **SonarQube Quality Check**: waits for the result. `abortPipeline: false` allows this

demo to continue while the gate reports `ERROR`.

7. **Build Docker Image**: builds `repository:release-build-number`.

8. **Trivy Scan**: scans the image for HIGH and CRITICAL vulnerabilities.

9. **Push Docker Image**: pushes the build tag and `latest` to Docker Hub.

10. **Cleanup Docker Images**: removes local image copies from the agent.

11. **Update GitOps Helm Values**: checks out the GitOps repo, changes `values.yaml`,

commits, and pushes it. Argo CD reacts to that commit.

12. **post / emailext**: sends the final build result to Jenkins default recipients

after SMTP is configured.

Credentials are referenced by Jenkins ID, never embedded as values in the pipeline.

Helm Chart

Chart.yaml

Identifies the Helm chart:
- chart name: `fashion-signup-app`

chart type: application

chart version: chart package version

app version: application release version

values.yaml

Contains environment-specific values without changing templates:

`replicaCount`: desired number of app pods

`image.repository`: Docker image repository

`image.tag`: immutable build tag deployed by Jenkins

`image.pullPolicy`: image-pull behavior

`service`: Kubernetes Service type and ports

`resources`: CPU and memory limits

`databaseSecret`: existing Secret name and key names

`app`: stable application, Deployment, and Service names

templates/deployment.yaml

Creates the application `Deployment`.

Important behavior:
- uses `replicaCount` to run multiple pods

selects pods using the `app` label

builds the image from repository plus tag

injects `DB_URL`, `DB_USER`, and `DB_PASSWORD` from a Kubernetes Secret

exposes container port 8080

applies resource limits

When the image tag changes, the pod template changes. Kubernetes then performs a rolling
update.

templates/service.yaml

Creates the Kubernetes `Service`.

Important behavior:
- selects pods using the same `app` label

exposes port 8080

forwards traffic to container port 8080

`LoadBalancer` asks AWS to create an external load balancer

The Service is stable even when individual pod IPs change.

Kubernetes Concepts

**Pod**: smallest running unit; contains the app container.

**Deployment**: maintains the requested replica count and performs rollouts.

**Service**: stable network endpoint for matching pods.

**Secret**: stores database connection values and injects them as environment variables.

**Namespace**: separates application resources, such as `register-app`, `monitoring`,
and `argocd`.

Argo CD

Argo CD continuously compares the GitOps repository with the live EKS cluster.

`Synced`: live resources match Git.

`OutOfSync`: live resources differ from Git.

`Healthy`: resources are running correctly.

a new Helm image tag creates a new desired state and a Kubernetes rollout.

The old Jenkins CD job is not part of the current design.

Prometheus and Grafana

Prometheus runs in the `monitoring` namespace and collects metrics.

Grafana queries Prometheus and displays dashboards.

`kube-prometheus-stack` installs the standard cluster monitoring components.

Grafana is exposed through an AWS LoadBalancer for persistent browser access.

SonarQube and Trivy

SonarQube performs static code analysis and reports issues, coverage, and Quality Gate
status.

Trivy scans the container image for vulnerabilities before push.

A passing scanner task does not automatically mean the Quality Gate passes.

Application Overview

The application is a Java web application packaged as a WAR and served by Tomcat. It
provides a fashion storefront/registration page and an admin customer listing. It
uses PostgreSQL RDS for registration data and receives database settings from
Kubernetes Secrets.
