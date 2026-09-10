# 1. Project Overview

This project implements a complete end-to-end DevOps workflow for a Java-based web
application deployed to Amazon Elastic Kubernetes Service (EKS). The solution
combines source control, continuous integration, static code quality, container
security, Docker image management, GitOps deployment orchestration, and monitoring
in one cohesive pipeline. The objective is to show how an application can be built,
validated, scanned, packaged, and deployed in a production-style cloud environment
using modern DevOps tools and practices.

Architecture Overview

2. What the Project Delivers
----------------------------

Automated Java application build and test pipeline using Maven.

Static code quality analysis with SonarQube.

Containerization using Docker and vulnerability scanning with Trivy.

Image versioning and registry publishing to Docker Hub.

GitOps-driven deployment updates through a Helm values repository.

Cluster deployment to Amazon EKS using Argo CD.

Database connectivity to PostgreSQL RDS using Kubernetes Secrets.

Operational visibility using Prometheus and Grafana.

3. Architecture and Execution Flow
----------------------------------

The application architecture follows a clear GitOps and Kubernetes deployment pattern.
The developer pushes code to the GitHub application repository. Jenkins detects the
change and runs the build stage, then executes unit tests and SonarQube analysis.
Once the code passes the CI checks, Docker builds the application image and Trivy
scans it for high and critical vulnerabilities. After a successful scan, Jenkins
publishes the image to Docker Hub and updates the Helm values file in the GitOps
repository. Argo CD detects the repository change and syncs the Kubernetes
deployment in Amazon EKS. The Java application runs in pods behind a Kubernetes
LoadBalancer service and connects to PostgreSQL RDS using Secret-based environment
values. The cluster contains three worker nodes and is monitored using Prometheus
and Grafana.

EKS Cluster

4. Core Technology Stack
------------------------

GitHub: source control and GitOps repository management

Jenkins: CI/CD orchestration and pipeline execution

Java + Maven: application build and test lifecycle

SonarQube: code quality and static analysis

Docker: application containerization

Trivy: vulnerability scanning of the container image

Docker Hub: container image registry

Helm: Kubernetes deployment templating

Argo CD: GitOps reconciliation with the cluster

Amazon EKS: container orchestration platform

AWS Load Balancer: service exposure for the application

PostgreSQL RDS: persistent application database

Prometheus + Grafana: observability and dashboards

5. CI/CD and GitOps Implementation
----------------------------------

The Jenkins pipeline was structured to automate the complete software delivery process.
It starts with a clean workspace, checks out the application code, executes Maven
build and tests, and then runs SonarQube analysis for code quality. After this, the
pipeline builds the container image, performs a Trivy vulnerability scan, and pushes
the final image to Docker Hub. In the next stage, the pipeline updates the image tag
in the GitOps repository values file. Argo CD watches this repository, detects the
Docker version change, and synchronizes the Kubernetes Deployment state in the
cluster. This design ensures a repeatable, version-controlled delivery path from
source code to running application.

Jenkins CI Pipeline

6. Infrastructure and Cloud Components
--------------------------------------

The project is hosted on AWS and demonstrates a real cloud-native setup. The application
infrastructure includes networking, EC2 hosts, EKS worker nodes, RDS PostgreSQL, IAM
access controls, S3 for storage-related resources, and monitoring services. The
Kubernetes cluster is configured to run the application in a scalable manner, while
the database layer remains externalized for maintainability and separation of
concerns. The application uses Kubernetes Secrets to store database credentials
rather than hardcoding them into the application configuration.

EC2 hosts served as infrastructure nodes for Jenkins, SonarQube, and bootstrap/admin
access.

EKS cluster runs the application and manages pod orchestration.

Three worker nodes are used for the application runtime workload.

RDS PostgreSQL stores application data and registration records.

S3 is used for supporting storage and cloud assets related to the project.

IAM roles and access policies govern secure resource access.

7. Monitoring, Observability, and Operational Visibility
--------------------------------------------------------

A key strength of the project is its operational visibility. Prometheus collects cluster
and workload health metrics, while Grafana visualizes them through dashboards. This
gives the deployment an operational layer that helps demonstrate the practical
realities of managing a live Kubernetes application in AWS. Monitoring is critical
for understanding pod health, resource usage, infrastructure status, and
application-level behavior after deployment.

Grafana Monitoring

8. GitHub and Repository Structure
----------------------------------

The project is organized into separate repositories for the application code and the
GitOps deployment configuration. The application repository contains the Java source
code, Maven build files, Dockerfile, Jenkins pipeline, database schema, and web
application assets. The GitOps repository contains the Helm chart, values file, and
Kubernetes manifests that define the desired state for deployment. This separation
makes the solution more maintainable, scalable, and aligned with modern cloud-native
operational practices.

GitHub Repository

9. Business Value and Outcome
-----------------------------

This project demonstrates how a Java application can be transformed from a local
codebase into a deployed, monitored, and automated cloud-native service using AWS
and Kubernetes. It highlights the value of infrastructure automation, security
checks, central versioning, GitOps deployment patterns, and observability in a
modern DevOps environment. The result is a deployment model that is more consistent,
repeatable, and easier to manage than traditional manual deployment methods.

10. Conclusion
--------------

The project successfully demonstrates a real-world DevOps lifecycle for a Java web
application running on Amazon EKS. From GitHub to Jenkins to Docker to Kubernetes
and monitoring, the architecture reflects a modern automated deployment pipeline.
The project proves the ability to build, secure, package, deploy, and observe a
cloud-native application in an AWS ecosystem using a practical and industry-aligned
toolchain.
