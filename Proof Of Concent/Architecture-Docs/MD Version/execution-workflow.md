# Execution Workflow

How the Fashion Signup App was built, deployed, monitored, and verified end to end.

1. Code and Build Pipeline
--------------------------

1

Application Repo

The Java web application and project build files were stored in the app repository.

2

Jenkins Trigger

Jenkins detected code changes and started the CI pipeline.

3

Maven Build

Maven compiled the source and executed the application tests.

4

SonarQube

Static analysis identified bugs, code smells, and quality issues.

2. Packaging and Security
-------------------------

- Docker built the app image from the Java project and runtime files.

- Trivy scanned the image for high and critical vulnerabilities.

- Docker Hub stored the final image for deployment reuse.

- Jenkins updated the GitOps Helm values with the new image tag.

3. GitOps Deployment
--------------------

- The GitOps repository contained the Helm chart and environment values.

- The Jenkins pipeline committed the updated image tag into the chart values file.

- Argo CD detected the Git repository change and started reconciliation.

- Kubernetes rolled out the new Deployment and updated application pods.

4. Runtime Architecture
-----------------------

Layer Role

Browser Client makes requests to the public app URL.

LoadBalancer Service Exposes app traffic through AWS Load Balancer technology.

EKS Pod Runs the Java web application and handles registration/admin requests.

PostgreSQL RDS Stores application data and registration records.

Prometheus / Grafana Collects metrics and shows cluster workload status.

5. What I Configured from Scratch
---------------------------------

- Installed and configured Jenkins on the EC2 bootstrap VM.

- Configured GitHub credentials, Docker Hub credentials, and SonarQube
credentials.

- Set up Java and Maven as required by the app build.

- Configured Argo CD to track the GitOps Helm repository.

- Deployed Prometheus and Grafana inside the EKS cluster.

- Exposed Grafana publicly using an AWS LoadBalancer service.

- Connected the application to PostgreSQL RDS with secret-based env injection.

- Reviewed and corrected security group exposure and port permissions.
