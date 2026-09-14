# 04 Problem-Solving Notes

## Purpose

This document records the engineering decisions that shaped the final system. It focuses on design reasoning and lessons, not a chronological incident list.

## Repository and GitOps Design

The project uses separate application and GitOps repositories. Jenkins validates the application, builds the image, and updates the Helm image tag. Argo CD owns reconciliation from the GitOps repository to Kubernetes. This removes competing deployment mechanisms and keeps the desired state in Git.

## Resource and Runtime Decisions

- The Jenkins agent requires enough memory and swap for Maven and SonarQube analysis.
- Jenkins tools and the application compiler target must be configured deliberately; controller, agent, and build JDK versions must be compatible.
- PostgreSQL connection settings belong in Kubernetes Secrets rather than deployment templates or source code.
- Monitoring is deployed separately from the application workload so application and observability concerns remain isolated.

## Networking Decisions

The application and Grafana use Kubernetes `LoadBalancer` Services for external access. The AWS-managed load-balancer security group is distinct from the EC2 bootstrap security group, so both layers must be checked independently.

## Data and Query Decisions

The admin user list uses a deterministic ordering: newest `created_at` first, followed by descending `id` when timestamps match. This makes the result stable and predictable.

## Operational Lessons

- Follow the path from container logs to pods, Services, and cluster networking when diagnosing failures.
- Treat image tags as deployment inputs and verify the tag written to Helm values.
- Keep the GitOps repository focused on deployment configuration.
- Separate application runtime, monitoring, and persistent database responsibilities.
