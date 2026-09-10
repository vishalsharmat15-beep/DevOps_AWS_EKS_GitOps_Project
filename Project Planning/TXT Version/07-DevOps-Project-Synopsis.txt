07 DevOps Project Synopsis
--------------------------

==========================

Project Synopsis

Jenkins CI and Argo CD GitOps Deployment of a Java Application on Amazon EKS

Application CI and GitOps delivery

Objective

Build a practical CI/CD and GitOps workflow for a Java web application running on Amazon
EKS, with image scanning and Kubernetes monitoring.

Current Architecture

Developer

-> GitHub application repository

-> Jenkins CI

-> Maven build and tests

-> SonarQube analysis

-> Docker image build

-> Trivy scan

-> Docker Hub

-> Jenkins updates Helm values.yaml

-> GitHub GitOps repository

-> Argo CD

-> Amazon EKS

-> Kubernetes Deployment and LoadBalancer Service

-> Browser

EKS workloads -> Prometheus -> Grafana

Application pods -> PostgreSQL RDS

Diagram source: `08-Current-Architecture.docx`

What I Implemented

Created and maintained a Java WAR application delivered through Tomcat.

Created a Jenkins CI pipeline for Maven build, tests, SonarQube, Docker, Trivy, and
Docker Hub.

Created a Helm chart containing the Deployment and Service templates.

Used a separate GitOps repository for the image tag and deployment values.

Configured Argo CD to reconcile the GitOps repository into EKS.

Installed Prometheus and Grafana in the EKS monitoring namespace.

Exposed Grafana through an AWS LoadBalancer and verified the Kubernetes dashboard.

Connected the application to PostgreSQL RDS through Kubernetes Secret environment
variables.

Current Repositories

Application: `register-app`

Contains source code, Maven files, Dockerfile, database schema, and the CI Jenkinsfile.

GitOps: `gitops-register-app`

Contains `Chart.yaml`, `values.yaml`, `templates/deployment.yaml`, and
`templates/service.yaml`. It does not contain the old CD Jenkinsfile.

Lessons From Troubleshooting

A successful Sonar analysis task can still have a failed Quality Gate.

Low-memory Jenkins agents can lose Java during Sonar analysis; the agent required swap.

Kubernetes Service and endpoint checks separate routing problems from application
problems.

The PostgreSQL JDBC driver must be available in the WAR runtime classpath.

Kubernetes Secret values must be valid and loaded by restarting pods after changes.

A LoadBalancer security group is different from the bootstrap EC2 security group.

Argo CD deploys when the desired GitOps state changes; a successful Jenkins CD job alone
does not guarantee a rollout.

Honest Limitations

The Sonar Quality Gate recorded 0% new-code coverage and new issues; the demo pipeline
continues with `abortPipeline: false` while the result remains visible.

Email notification depends on Jenkins SMTP configuration and was not treated as a core
deployment dependency.

Old CD pipeline screenshots are historical and are excluded from current architecture
evidence.

Evidence

Use `04-Final-Evidence-Screenshot-Plan.docx` for the exact screenshot list and naming
convention.

Supporting Documents

`01-Architecture-Overview.docx`: architecture and ownership model

`02-DevOps-Tools-and-Code-Guide.docx`: tools, Jenkinsfile, Helm, and Kubernetes concepts

`03-Issues-Encountered-and-Resolved.docx`: troubleshooting history

`04-Final-Evidence-Screenshot-Plan.docx`: evidence pack

`05-AWS-Resource-Cleanup-Runbook.docx`: safe teardown after evidence capture

`06-Operations-and-Interview-Guide.docx`: explanation and interview preparation

`07-Final-Project-Synopsis.docx`: concise synopsis source for Word/PDF export

`08-Current-Architecture.docx`: diagram source
