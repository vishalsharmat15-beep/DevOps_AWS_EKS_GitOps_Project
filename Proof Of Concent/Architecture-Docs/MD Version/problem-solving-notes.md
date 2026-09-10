# Problem Solving Notes

What was encountered, what was fixed, and why the system ended up in this final
architecture.

1. Git Conflicts and Repository Cleanup
---------------------------------------

GitHub rejected pushes because the remote branch had newer history than the local
branch. The fix was to fetch, rebase, resolve conflicts, and keep the Helm-only
GitOps structure.

This was important because the project had stale files from an old CD pipeline. The
final structure was simplified so Argo CD became the single deployment mechanism.

2. Jenkins Agent Memory and SonarQube Instability
-------------------------------------------------

The Jenkins agent ran out of memory during SonarQube scanning. Java was being killed
by the kernel, which caused pipeline instability.

The solution was to add swap and increase memory headroom so the analysis process
could complete. The pipeline was also configured to continue when the quality gate
did not fully pass, while still exposing the result transparently.

3. Java and JDK Version Mismatch Across Jenkins
-----------------------------------------------

The Jenkins controller, Jenkins agent, and application build were using different
Java and JDK versions. This caused repeated pipeline failures because Jenkins
requires a compatible Java runtime, while Maven compilation and the application
runtime must also use compatible JDK and bytecode versions.

The issue was resolved by checking the Java version on the controller and agent,
aligning the Jenkins tool configuration with the application JDK, and confirming the
Maven compiler target. The important lesson is to validate both the Jenkins runtime
Java and the application build/runtime Java. They do not have to be identical, but
they must be deliberately configured and compatible.

4. Database Driver and Password Issues
--------------------------------------

The application initially returned an HTTP 500 error because the PostgreSQL JDBC
driver was not being loaded correctly and the database password value was empty.

The application was updated to explicitly load the PostgreSQL driver, and the
Kubernetes Secret value was corrected and restarted. The admin page then connected
successfully and displayed registered users.

5. Admin Ordering Issue
-----------------------

Rows were not appearing in a clean, deterministic order when timestamps were the
same.

The SQL query was updated to sort by created_at descending and id descending. This
ensured a stable and readable list of the newest records first.

6. Grafana Public Access and LoadBalancer Configuration
-------------------------------------------------------

Grafana was deployed in the monitoring namespace and exposed using a Service of
type LoadBalancer. Public access was validated after confirming the correct security
group and route.

Important lesson: the EC2 bootstrap security group and the AWS-managed LoadBalancer
security group are not the same object.

7. Argo CD and GitOps Ownership
-------------------------------

In the final design, Jenkins is responsible for build validation, Docker image
creation, and updating the GitOps chart values. Argo CD is responsible for
reconciling the cluster with the desired state declared in Git. This is the cleanest
and most professional GitOps pattern.

8. Summary of Lessons Learned
-----------------------------

- Always follow the logs from the app container, the pod, the Service, and the
cluster instead of assuming infrastructure is healthy.

- Do not confuse bootstrap EC2 configuration with Kubernetes Service or LoadBalancer
networking rules.

- Use secrets for database credentials and environment configuration instead of
hardcoding them.

- Keep the GitOps repository focused on the application deployment manifest and
chart values only.

- Monitoring should be separate from the application runtime and must be checked
from the cluster namespace itself.
