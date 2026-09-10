Configuration and Operations Checklist
Everything that was configured and validated for the Fashion Signup App
CI / CD

1. Jenkins Configuration
------------------------

✓ GitHub repository access configured
- ✓ Docker Hub credentials configured
- ✓ SonarQube credentials configured
- ✓ Maven toolchain available
- ✓ Docker installed and usable in the pipeline
- ✓ Trivy installed for security scanning
- ✓ Email notification configuration prepared
- ✓ GitOps repository credentials configured to push image tag updates
Container

2. Docker and Image Flow
------------------------

✓ Application built using Maven
- ✓ Dockerfile creates the runtime image
- ✓ Image tagged with version / build number
- ✓ Trivy scans the image for vulnerabilities
- ✓ Approved image pushed to Docker Hub
GitOps

3. Helm and GitOps Configuration
--------------------------------

✓ Helm chart present in the GitOps repository
- ✓ Chart.yaml contains chart identity
- ✓ values.yaml manages image tag and deployment settings
- ✓ deployment.yaml defines the Kubernetes workload
- ✓ service.yaml exposes the app via LoadBalancer
- ✓ Argo CD configured to watch the repository and sync state
Runtime

4. Kubernetes Runtime Notes
---------------------------

✓ Application runs in a Pod managed by a Deployment
- ✓ Public access handled by a Service of type LoadBalancer
- ✓ Database credentials injected via Kubernetes Secret
- ✓ Monitoring delivered from a separate monitoring namespace
Security

5. Security Group Notes
-----------------------

✓ EC2 bootstrap security group kept separate from EKS LoadBalancer SG
- ✓ HTTP/80 and required ports opened
- ✓ Public access restricted to necessary traffic only
- ✓ Database access limited to the application network path
Observability

6. Monitoring Notes
-------------------

✓ Prometheus deployed inside the cluster
- ✓ Grafana exposed publicly via LoadBalancer
- ✓ Cluster resources visible through pre-built dashboards
- ✓ Application health observable via metrics and pod status
Data

7. Database Notes
-----------------

✓ PostgreSQL hosted on Amazon RDS
- ✓ Connection details stored as Kubernetes Secrets
- ✓ Application explicitly loads the PostgreSQL JDBC driver
- ✓ Query results returned in deterministic order
Validation

8. Final Validation Checklist
-----------------------------

✓ Jenkins pipeline runs successfully
- ✓ SonarQube analysis is visible
- ✓ Docker image is built and pushed
- ✓ GitOps repository updated with the new image tag
- ✓ Argo CD sync is healthy
- ✓ Application pod is running and accessible
- ✓ Database connection is working
- ✓ Grafana dashboard is reachable
- ✓ Security groups allow the required traffic flows
