# 05 Issue Resolution Log

## Purpose

This document lists concrete project issues, their fixes, and the validation that confirmed each fix.

## Git Push Conflict

**Problem:** The remote branch contained commits that were missing locally.

**Resolution:** Fetched the remote history, rebased the local work, resolved conflicts, and pushed the aligned branch.

**Validation:** The local branch and remote branch reported the same commit.

## Grafana LoadBalancer Access

**Problem:** Grafana was pending or unreachable from outside the cluster.

**Resolution:** Checked the Service type, AWS load-balancer security group, routes, and exposed port.

**Validation:** The Service received an external endpoint and the dashboard became reachable.

## Jenkins Agent Memory Pressure

**Problem:** SonarQube analysis caused the Jenkins agent to disconnect during scanning.

**Resolution:** Added swap and increased available memory headroom on the build agent.

**Validation:** The analysis completed without the agent being killed.

## Application HTTP 500

**Problem:** The admin view returned HTTP 500 while loading database records.

**Resolution:** Verified PostgreSQL driver loading, corrected the Kubernetes database Secret values, and restarted the application pods.

**Validation:** The admin request returned successfully and displayed registered data.

## ImagePullBackOff

**Problem:** New pods could not start because the GitOps image tag did not match the pushed image.

**Resolution:** Corrected the tag written to Helm `values.yaml` and committed the updated desired state.

**Validation:** Argo CD reconciled the new revision and Kubernetes pulled the image successfully.
