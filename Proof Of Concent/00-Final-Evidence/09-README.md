# Fashion Signup App

## Project Title
Fashion Signup App on AWS EKS with Docker, Jenkins, Argo CD, Helm, SonarQube, Grafana, RDS, and S3

## Purpose
This project demonstrates a practical DevOps implementation for a Java web application built, scanned, containerized, and deployed on Amazon EKS using Jenkins CI and Argo CD GitOps. It covers CI/CD, Kubernetes deployment, monitoring, security, and cloud services integration.

## Deliverables
- Architecture overview
- Execution workflow
- Problem-solving notes
- Screenshot evidence pack
- Learning notes and configuration checklist

## Files in this folder
- [architecture-overview.html](../Architecture-Docs/architecture-overview.html)
- [execution-workflow.html](../Architecture-Docs/execution-workflow.html)
- [problem-solving-notes.html](../Architecture-Docs/problem-solving-notes.html)
- [screenshot-evidence.html](../Architecture-Docs/screenshot-evidence.html)
- [diagrams/02-complete-architecture.png](../Architecture-Docs/diagrams/02-complete-architecture.png)
- [diagrams/01-cicd-pipeline-flow.png](../Architecture-Docs/diagrams/01-cicd-pipeline-flow.png)
- [diagrams/04-kubernetes-traffic-flow.jpg](../Architecture-Docs/diagrams/04-kubernetes-traffic-flow.jpg)

## Project flow
```text
Developer -> GitHub app repo -> Jenkins CI -> SonarQube -> Docker -> Trivy -> Docker Hub
                                     \
                                      -> GitOps repo values update
                                                   -> Argo CD -> Amazon EKS
App pods -> LoadBalancer -> Browser
App pods -> PostgreSQL RDS
EKS cluster -> Prometheus -> Grafana
```

## Notes
- GitHub hosts the application code and the Helm chart values repository.
- Jenkins handles CI and image promotion into the GitOps repository.
- Argo CD is the GitOps deployment controller.
- PostgreSQL is hosted in AWS RDS.
- Prometheus and Grafana provide monitoring and dashboards.
- S3 and cloud storage were used for assets and supporting resources.

## PDF-ready export
This workspace does not include a PDF exporter, so the files are prepared as polished HTML and Markdown documents. These can be printed to PDF directly from a browser or opened in Word for export.



i dont know whats below this point please check if its imp if not remove

# Full Screenshot Archive

This folder contains the complete raw screenshot archive for the project.

## Purpose
- preserve every screenshot taken during the build, troubleshooting, and validation process
- keep a complete record for later review
- allow manual selection of the final curated proof set

## Categories
- 01-Jenkins
- 02-SonarQube
- 03-ArgoCD
- 04-AWS
- 05-Grafana
- 07-Historical

The curated application screenshots are stored in `08-Application` directly under the parent `00-Final-Evidence` directory. Empty archive categories were removed after cleanup.

## Important note
The final portfolio and client-facing evidence are intentionally curated from this archive. Not every screenshot in this folder is required in the final report. This archive exists so you can revisit any earlier state, compare versions, and choose the best proof set later.
