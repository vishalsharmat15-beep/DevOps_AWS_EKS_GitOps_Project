# 03 Issues Encountered and Resolved

<!-- markdownlint-disable MD024 -->

Problems Encountered and Resolutions

This is the actual troubleshooting history for the project.

1. Git push rejected as non-fast-forward
----------------------------------------

Symptom

GitHub rejected a push because the remote `main` branch had commits that were not in the
local branch.

Resolution

Fetched the remote branch, rebased the local commit, resolved the old-manifest deletion
conflict by keeping the Helm-only structure, and pushed normally.

Lesson

Always inspect `git status`, fetch, and integrate remote history. Do not force-push
shared branches.

2. Helm was not installed on the bootstrap server
-------------------------------------------------

Symptom

`helm: command not found`.

Resolution

Installed Helm on the Ubuntu EKS bootstrap EC2 instance. The bootstrap server is an
administration node; Prometheus and Grafana run inside EKS.

Lesson

Tools such as `aws`, `kubectl`, and `helm` run from the management host, while
Kubernetes workloads run in the cluster.

3. Grafana LoadBalancer showed pending
--------------------------------------

Symptom

The Grafana Service initially showed `EXTERNAL-IP: <pending>`.

Resolution

Waited for AWS to provision the internet-facing Classic Load Balancer.

Lesson

Kubernetes LoadBalancer creation is asynchronous.

4. Grafana browser timeout
--------------------------

Symptom

The public Grafana hostname timed out in the browser.

Investigation

The service, endpoints, pods, and target instances were checked. The LoadBalancer
security group was identified separately from the bootstrap EC2 security group.

Resolution

Verified the correct LoadBalancer security group and public HTTP rule. Testing with
`curl.exe` and launching the URL with PowerShell confirmed public access.

Lesson

A security group on the bootstrap EC2 instance is not automatically the security group
attached to an EKS-created LoadBalancer.

5. Jenkins agent disconnected during SonarQube
----------------------------------------------

Symptom

The Sonar scanner stopped during JavaScript analysis and Jenkins reported the agent
going offline.

Evidence

The Jenkins agent had approximately 908 MiB RAM and no swap. Kernel logs showed Java
being killed by the Linux OOM killer.

Resolution

Created a 2 GiB swap file and added it to `/etc/fstab`. The better long-term option is
an agent with at least 2 GiB RAM.

Lesson

Always inspect agent memory, swap, disk, and connection health when CI tools fail
unexpectedly.

6. Sonar Quality Gate failed
----------------------------

Symptom

The Sonar background task succeeded, but the Quality Gate returned `ERROR`.

Evidence

The gate showed 8 new issues and 0% new-code coverage against an 80% requirement.

Resolution for this demo

Set `abortPipeline: false` so the pipeline can continue while the Quality Gate result
remains visible. This is a temporary demonstration choice, not a claim that the code
passes the gate.

Lesson

A successful Sonar analysis and a passing Quality Gate are different results.

7. Admin page returned HTTP 500
-------------------------------

Symptom

The admin endpoint returned `Users could not be loaded`.

Investigation

Kubernetes networking was healthy. Tomcat logs showed `No suitable driver found`.

Resolution

Added the PostgreSQL dependency/driver loading path and deployed a new image. The WAR
was verified to contain `postgresql-42.7.12.jar`.

The next error showed the database password was empty. The Kubernetes Secret was
corrected and the pods were restarted.

Final result

The admin page loaded registered users successfully.

Lesson

Trace errors from the outside inward: LoadBalancer, Service, endpoints, pod, application
log, driver, Secret, and database.

8. Admin results were not consistently ordered
----------------------------------------------

Symptom

Rows with equal timestamps appeared in an unexpected order.

Resolution

Changed the SQL ordering to:
- ORDER BY created_at DESC, id DESC

Lesson

A database query needs a deterministic tie-breaker when the primary sort value is not
unique.

9. Old CD pipeline was removed
------------------------------

Previous design

Jenkins CI triggered a separate GitOps CD Jenkins job.

Current design

Jenkins CI pushes the updated Helm image tag to the GitOps repository. Argo CD detects
the Git change and deploys it.

Lesson

Argo CD should own Kubernetes deployment reconciliation in a pure GitOps model. The old
CD screenshots are historical and must not be presented as the current architecture.
