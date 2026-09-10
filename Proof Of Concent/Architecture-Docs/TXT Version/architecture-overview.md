Fashion Signup App
==================

AWS EKS Docker Jenkins Argo CD Helm SonarQube Grafana RDS S3

1. Architecture Overview
------------------------

This application follows a practical GitOps and Kubernetes deployment model. Code is
developed in GitHub, validated by Jenkins, scanned for quality and vulnerabilities,
pushed to Docker Hub, and then deployed with Helm through Argo CD into Amazon EKS.

2. Execution Flow
-----------------

- Developer commits application code to the app repository.

- Jenkins fetches the source and runs Maven build and unit tests.

- SonarQube performs code quality analysis and exposes the result.

- Docker builds the Java application image and Trivy scans it for vulnerabilities.

- Docker Hub stores the versioned application image.

- Jenkins updates the Helm values file in the GitOps repository with the new image
tag.

- Argo CD detects the commit and syncs the Kubernetes deployment.

- Kubernetes deploys the app pod and exposes it via a LoadBalancer service.

- The app connects to PostgreSQL RDS and uses cloud assets from S3 where needed.

- Prometheus collects cluster and workload metrics; Grafana visualizes them.

3. Core Components
------------------

Source and Version Control

- GitHub app repository

- GitHub GitOps repository

- Helm chart with values and templates

CI and Quality

- Jenkins CI pipeline

- Maven build

- SonarQube analysis

- Trivy security scan

Deployment

- Docker image build

- Docker Hub registry

- Helm chart deployment

- Argo CD GitOps sync

Runtime and Data

- Amazon EKS cluster

- LoadBalancer Service

- PostgreSQL RDS

- Prometheus + Grafana

4. Security and Config Considerations
-------------------------------------

Area Configuration

Networking Security groups were reviewed to allow only necessary ports for EC2, EKS, and
LoadBalancer services.

Identity IAM roles and access policies were configured for cluster and cloud access.

Application Configuration Database credentials and runtime env values were passed via
Kubernetes Secrets.

Monitoring Prometheus and Grafana were deployed in the monitoring namespace and exposed
publicly through LoadBalancer services.

5. Architecture Summary
-----------------------

The architecture emphasizes clarity, observability, and automation. Jenkins provides
continuous validation, GitOps ensures predictable deployment behavior, Argo CD
manages the desired cluster state, and Kubernetes orchestrates the application
runtime. Prometheus and Grafana add operational visibility, while PostgreSQL RDS
provides a durable and scalable database backend.
