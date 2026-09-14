# Final Project Synopsis

## Project Summary

The Fashion Signup App is a Java web application delivered through a CI/CD and GitOps workflow on AWS. The implementation separates application source from Kubernetes deployment configuration and uses Git as the source of deployment intent.

## Technology Stack

- GitHub for source and GitOps repositories
- Jenkins for CI automation
- Maven and Java for build and tests
- SonarQube for static analysis
- Docker and Docker Hub for image delivery
- Trivy for container vulnerability scanning
- Helm for Kubernetes packaging
- Argo CD for GitOps reconciliation
- Amazon EKS for application orchestration
- PostgreSQL RDS for persistent data
- Prometheus and Grafana for monitoring

## Delivery Sequence

1. A change is pushed to `Fashion-Register-App`.
2. Jenkins checks out the `main` branch and runs Maven build and tests.
3. SonarQube analyzes the source and returns the configured quality result.
4. Jenkins builds and scans a Docker image.
5. The image is pushed to Docker Hub with a build-specific tag.
6. Jenkins updates the image tag in `GitOps-Fashion-Signup-App`.
7. Argo CD detects the GitOps commit and synchronizes EKS.
8. Kubernetes rolls out the new pods behind a LoadBalancer Service.

## Application Runtime

The web application runs in Tomcat-based containers. Kubernetes maintains the requested replica count and routes traffic through a stable Service. Database connection values are injected from a Kubernetes Secret and point to PostgreSQL RDS.

## Resulting Operating Model

The pipeline provides a repeatable path from source change to running workload. Jenkins performs validation and delivery preparation; GitOps records the desired image version; Argo CD applies that version; Kubernetes manages runtime placement and rollout; monitoring provides operational visibility.
