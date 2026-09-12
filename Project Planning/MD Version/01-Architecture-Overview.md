# 01 Architecture Overview

Java Application on AWS EKS

Accurate Project Title

Jenkins CI and Argo CD GitOps Deployment of a Java Application on Amazon EKS

Purpose

This project demonstrates how a Java web application can be built, tested, scanned,
containerized, and deployed to Kubernetes on AWS. It is a hands-on DevOps project,
not a production claim.

Current Architecture

flowchart LR

Developer[Developer] --> AppGit[GitHub App Repository]

AppGit --> Jenkins[Jenkins CI]

Jenkins --> Maven[Maven Build and Tests]

Maven --> Sonar[SonarQube Analysis]

Sonar --> Docker[Docker Image Build]

Docker --> Trivy[Trivy Vulnerability Scan]

Trivy --> Registry[Docker Hub]

Jenkins -->|Update image tag| GitOps[GitHub GitOps Repository]

GitOps --> Argo[Argo CD]

Argo --> EKS[Amazon EKS]

EKS --> App[Java App Pods]

App --> Service[Kubernetes LoadBalancer Service]

Service --> User[Browser User]

App --> RDS[PostgreSQL RDS]

EKS --> Prom[Prometheus]

Prom --> Grafana[Grafana Dashboard]

Actual Flow

1. Developer pushes application code to the `register-app` GitHub repository.
-----------------------------------------------------------------------------

2. GitHub triggers the Jenkins CI pipeline.
-------------------------------------------

3. Jenkins checks out the application and runs Maven build/tests.
-----------------------------------------------------------------

4. Jenkins runs SonarQube analysis.
-----------------------------------

5. Jenkins builds a Docker image tagged with the release and build number.
--------------------------------------------------------------------------

6. Trivy scans the image for high and critical vulnerabilities.
---------------------------------------------------------------

7. Jenkins pushes the image to Docker Hub.
------------------------------------------

8. Jenkins checks out the `GitOps-Fashion-Signup-App` repository.
-----------------------------------------------------------

9. Jenkins updates the image tag in Helm `values.yaml` and pushes that Git change.
----------------------------------------------------------------------------------

10. Argo CD detects the GitOps commit and reconciles the Helm chart.
--------------------------------------------------------------------

11. Kubernetes performs a Deployment rollout and starts new application pods.
-----------------------------------------------------------------------------

12. Prometheus collects cluster metrics and Grafana displays dashboards.
------------------------------------------------------------------------

13. The application connects to PostgreSQL RDS using Kubernetes Secret values.
------------------------------------------------------------------------------

Repository Responsibilities

Application repository

Location: `Git_Repos/Fashion-Register-App`

Contains:
- Java web application source

Maven project files

Dockerfile

CI Jenkinsfile

application README

database schema files

GitOps repository

Location: `Git_Repos/GitOps-Fashion-Signup-App`

Contains:
- `Chart.yaml`: chart identity and version

`values.yaml`: deployment values, including image tag

`templates/deployment.yaml`: Kubernetes Deployment template

`templates/service.yaml`: Kubernetes Service template

The GitOps repository no longer contains the old CD Jenkinsfile. Argo CD is the
deployment controller.

AWS Components Used

EC2: Jenkins, SonarQube, and EKS bootstrap/admin access

EKS: Kubernetes control plane and worker nodes

VPC and subnets: network boundary for AWS resources

Security groups: network access control

RDS PostgreSQL: application database

Load Balancers: public access to the application and Grafana

S3: application image/static assets where applicable

DevOps Ownership Model

Jenkins owns build, test, analysis, image creation, and GitOps commit.

GitHub stores the desired application version.

Argo CD owns Kubernetes reconciliation.

Kubernetes owns scheduling and rollout of pods.

Prometheus owns metrics collection.

Grafana owns metrics visualization.

RDS owns persistent relational data.
