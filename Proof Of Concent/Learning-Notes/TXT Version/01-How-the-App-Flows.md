# Fashion Signup App – End-to-End Execution Notes

Complete walkthrough of architecture, configuration, request flow, and operational design

1. High-Level Architecture
--------------------------

The project is a Java web application running inside a container on Amazon EKS . The runtime is exposed through a Kubernetes Service with a public LoadBalancer, while the database runs in Amazon RDS PostgreSQL .
Basic Flow
Browser → AWS LoadBalancer → Kubernetes Service → Java App Pod → PostgreSQL RDS │ └── Prometheus + Grafana (monitoring)

2. What I Configured from Scratch
---------------------------------

2.1 AWS and IAM
---------------

- Created and managed EC2 instances for Jenkins, SonarQube, and cluster bootstrap access.
- Configured IAM roles and user policies for EKS access and cloud management.
- Verified that bootstrap EC2 configuration and EKS cluster networking are separate concerns.

2.2 EKS and Kubernetes
----------------------

- Installed Kubernetes CLI and Helm on the admin machine.
- Created the EKS cluster and node setup for running workloads.
- Deployed the application using Helm through Argo CD GitOps.
- Exposed the application using a Kubernetes LoadBalancer Service.

2.3 Database Connectivity
-------------------------

- Configured PostgreSQL RDS as the application database.
- Passed credentials using Kubernetes Secrets.
- Ensured environment variables matched the actual DB URL, username, and password.
- Diagnosed failing admin page requests by checking pod logs and application stack traces.

2.4 Monitoring and Observability
--------------------------------

- Installed Prometheus and Grafana in the monitoring namespace.
- Exposed Grafana through a LoadBalancer for public dashboard access.
- Verified dashboards for cluster and pod health.

2.5 CI/CD Pipeline
------------------

- Configured Jenkins with GitHub and Docker Hub credentials.
- Added SonarQube integration for quality checks.
- Added Docker build and Trivy scan steps.
- Updated the GitOps Helm values so Argo CD could deploy the new image.
- Enabled email alerts after pipeline runs.

3. How the Application Works
----------------------------

The application is a Java web app packaged as a WAR and running on Tomcat inside the container. It receives browser requests, executes servlet logic, and reads/writes user data from PostgreSQL.
Request Flow
- Client visits the application URL.
- The LoadBalancer routes traffic to the Kubernetes Service.
- The Kubernetes Service routes to the application pod.
- The Java app reads or writes user registration data.
- The app connects to PostgreSQL RDS using environment values from the Secret.
- The response is returned to the user.

4. LoadBalancer and Security Group Logic
----------------------------------------

Why the LoadBalancer matters
Without a Kubernetes Service of type LoadBalancer, the application cannot be reached from the internet. The LoadBalancer provides the public endpoint.
Key lesson: The EC2 bootstrap security group and the EKS LoadBalancer security group are not the same object. They must be reviewed separately.

5. Database and S3 Notes
------------------------

PostgreSQL RDS
Connection details ( DB_URL , DB_USER , DB_PASSWORD ) are injected via Kubernetes Secret and read by the Java application at runtime.
S3
Used for static assets or supporting content. Application runtime and storage concerns are kept independent.

6. Argo CD and Deployment Model
-------------------------------

Argo CD is the GitOps controller. It watches the GitOps repository and keeps the cluster in sync with the desired state.
Jenkins updates Helm values → Git push → Argo CD sync → Kubernetes Deployment → App rollout
The Git repository is the single source of truth. Kubernetes is continuously reconciled to match it.

7. Grafana and Monitoring
-------------------------

Application + Cluster → Prometheus → Grafana Dashboard
Grafana was deployed in the monitoring namespace and exposed via LoadBalancer, allowing public dashboard access without exposing cluster nodes directly.

8. Jenkins Pipeline Sequence
----------------------------

Checkout source code from GitHub
- Maven build and unit tests
- SonarQube code quality analysis
- Docker image build
- Trivy vulnerability scan
- Push image to Docker Hub
- Update Helm values in GitOps repository
- Git push to GitOps repo
- Argo CD deploys the new version to EKS
- Email notification on pipeline result

9. Application Runtime Model
----------------------------

Java source → Maven build → WAR artifact → Tomcat container → Docker image → Kubernetes Pod

10. Troubleshooting Approach
----------------------------

Issues were diagnosed layer by layer:
- Browser error / HTTP status
- Public endpoint and LoadBalancer
- Kubernetes Service
- Pod status and events
- Container and application logs
- Database connectivity and secrets
- Cluster networking and security groups
This systematic approach is what ultimately made the system stable.

11. Final Summary
-----------------

This project demonstrates a complete modern DevOps workflow covering:
- Source control and CI validation
- Code quality and security scanning
- Containerization and image management
- Helm-based packaging
- GitOps reconciliation with Argo CD
- Kubernetes runtime management
- Monitoring with Prometheus and Grafana
- Database and cloud connectivity
It is not just code deployment — it is a full operational pipeline from development to production-style monitoring and support.
