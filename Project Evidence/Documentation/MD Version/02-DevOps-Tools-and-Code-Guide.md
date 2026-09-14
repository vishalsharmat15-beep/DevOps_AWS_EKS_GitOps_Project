# DevOps Tools and Code Guide

## Purpose

This document explains what each tool contributes and where its configuration lives. It is a component guide, not a second architecture narrative.

## Source and Build

| Tool | Use in this project | Main location |
| --- | --- | --- |
| GitHub | Stores application and GitOps repositories | Repository remotes and `.gitmodules` |
| Java | Compiles and runs the application | Maven modules in `Fashion-Register-App` |
| Maven | Builds the server and web modules and runs tests | `pom.xml` |
| Jenkins | Executes the CI pipeline | `Jenkinsfile` |

The pipeline runs `mvn clean package` followed by `mvn test`. The Maven project contains `server` and `webapp` modules and produces the application artifacts.

## Quality and Security

### SonarQube

The Jenkinsfile runs the SonarQube Maven scanner and waits for the configured quality-gate result. The analysis task result and quality-gate result are separate values and must be reported separately.

### Trivy

Trivy scans the built image for `HIGH` and `CRITICAL` vulnerabilities before the image is pushed to Docker Hub.

## Container Delivery

The Dockerfile packages the web application for Tomcat. Jenkins builds an image using the release and Jenkins build number, then pushes both the immutable build tag and `latest` to Docker Hub. Local image copies are removed from the build agent after the push.

## Helm and Kubernetes

The GitOps chart uses four main inputs:

- `image`: repository, tag, and pull policy
- `replicaCount`: desired pod count
- `resources`: CPU and memory limits
- `databaseSecret`: Secret name and key mappings

`templates/deployment.yaml` creates the pods, injects database values, and exposes container port `8080`. `templates/service.yaml` creates a stable Service and requests an AWS LoadBalancer endpoint.

## Argo CD

Argo CD watches the GitOps repository. A new image tag changes the desired Deployment state; Argo CD detects the commit and reconciles the cluster. Kubernetes then performs the rollout.

## Monitoring

Prometheus collects cluster and workload metrics. Grafana queries Prometheus and presents dashboards. Monitoring resources are separate from the application Deployment and use their own namespace and Service configuration.

## Infrastructure Bootstrap

The EC2 user-data scripts install the supporting tools:

- EKS bootstrap host: AWS CLI, `kubectl`, and `eksctl`
- Jenkins master: Java and Jenkins service
- Jenkins agent: Java, Git, Docker, SSH, and build dependencies
- SonarQube host: PostgreSQL, Java 17, SonarQube, and its systemd service
