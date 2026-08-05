---
title: "Blog 1"
date: "2026-07-17"
weight: 1
chapter: false
pre: " <b> 3.1. </b> "
---
# How Generali Malaysia optimized operations with Amazon EKS — summary

**Source:** Adapted from an AWS Architecture case study (Generali Malaysia)

Generali Malaysia modernized several core applications by moving to Amazon EKS. The migration emphasized containerizing workloads, adopting Kubernetes best practices, and automating infrastructure management so that the operations team could focus on higher-value tasks.

Key takeaways:
- Adopt Well‑Architected principles (operational excellence, security, reliability, performance efficiency, cost optimization, sustainability).
- Use managed features (for example, EKS Auto Mode) to reduce day‑to‑day node maintenance, OS patching, and upgrades.
- Implement disruption controls (Pod Disruption Budgets, maintenance windows, multiple replicas) to avoid downtime during infrastructure updates.
- Combine security services (GuardDuty, Inspector, Network Firewall, Secrets Manager) for layered protection of cluster workloads.
- Use tagging and cost-allocation practices to attribute Kubernetes costs to teams or projects.

Benefits observed:
- Reduced operational overhead for Kubernetes management.
- Improved security posture and vulnerability prioritization.
- Better resource utilization and clearer cost visibility.

Further reading: original case study linked in the source materials.