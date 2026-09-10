Issue Resolution Log
Problems encountered during the project and how they were resolved

1. Git Push Conflict
--------------------

Problem: Remote branch had new commits and the local branch was behind.
Fix: Fetched remote history, rebased locally, resolved conflicts, then pushed the updated state.
Lesson: Maintain a clean GitOps repository and manage branch history carefully.

2. Helm Not Installed
---------------------

Problem: Helm command was missing on the bootstrap VM.
Fix: Installed Helm and used it to manage chart deployments.
Lesson: Tools installed on the admin node are not the same as runtime tools inside the cluster.

3. Grafana Not Publicly Accessible
----------------------------------

Problem: The service remained pending or inaccessible from the internet.
Fix: Validated LoadBalancer and security group configuration, then confirmed public access.
Lesson: AWS LoadBalancer networking must be reviewed independently from EC2 security groups.

4. SonarQube Memory Issue
-------------------------

Problem: Jenkins agent disconnected during SonarQube analysis due to insufficient memory.
Fix: Added swap space and ensured sufficient memory headroom for the agent.
Lesson: CI stability depends on adequate resource headroom.

5. HTTP 500 from Application
----------------------------

Problem: Application failed to load admin users and returned HTTP 500.
Fix: Inspected logs, verified PostgreSQL driver loading, corrected database secret values, and restarted the pods.
Lesson: External application errors often originate in configuration or dependency loading, not in the networking layer.

6. ImagePullBackOff due to Incorrect Image Tag
----------------------------------------------

Problem: Argo CD reported ImagePullBackOff and marked the application as Degraded. New pods failed to start.
Fix: Corrected the image tag written by Jenkins into the GitOps values.yaml and updated the repository.
Lesson: Always verify that Jenkins writes the correct image tag — a wrong or unchanged tag commonly causes ImagePullBackOff.

7. Old CD Pipeline Confusion
----------------------------

Problem: Historical pipeline artifacts and old repository structure did not match the final GitOps model.
Fix: Removed outdated deployment artifacts and focused documentation on the current architecture only.
Lesson: Present only the current, accurate design in the final project documentation.
