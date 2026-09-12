# Current Architecture

Mermaid architecture source converted to Word.

This diagram describes the end-to-end DevOps flow for the Java application on Amazon
EKS.

flowchart LR Dev[Developer] --> AppRepo[GitHub App Repo\nregister-app] AppRepo -->
Jenkins[Jenkins CI] Jenkins --> Maven[Maven build and tests] Maven -->
Sonar[SonarQube] Sonar --> Docker[Docker build] Docker --> Trivy[Trivy scan] Trivy
--> Hub[Docker Hub] Jenkins --> Values[Update Helm values.yaml] Values -->
GitOps[GitHub GitOps Repo\nGitOps-Fashion-Signup-App] GitOps --> Argo[Argo CD] Argo -->
EKS[Amazon EKS] EKS --> Deploy[Kubernetes Deployment] Deploy --> Pods[Java
application pods] Pods --> Service[LoadBalancer Service] Service --> User[Browser]
Pods --> RDS[PostgreSQL RDS] EKS --> Prom[Prometheus] Prom --> Grafana[Grafana]
