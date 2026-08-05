---
title: "Proposal"
date: 2026-08-05
weight: 2
chapter: false
pre: " <b> 2. </b> "
---
{{% notice warning %}}
⚠️ **Note:** Below is an adapted proposal based on workshop references; do not copy verbatim into official reports.
{{% /notice %}}

# Splitly — Group Expense Sharing (adapted proposal)

## Executive summary
Splitly is a lightweight platform to help small groups track shared expenses, compute balances, and manage settlements. The implementation targets a student-level deployment on AWS using familiar components: a React/Vite frontend, a Node.js backend, MongoDB Atlas for data, and Amazon S3 for storing receipts. The architecture favors simple deployment and operational visibility while keeping costs low.

## Problem and goal
Many groups still manage shared expenses using spreadsheets or chat messages. That approach becomes error-prone as the number of members and transactions grows. Splitly aims to centralize records, automate balance calculations, keep receipts searchable, and make settlement transparent.

## Proposed solution
- Frontend: React + Vite served by Nginx (initially co‑hosted with backend; later moved to S3 + CloudFront for better performance).
- Backend: Node.js / Express providing REST APIs, process management via PM2.
- Data: MongoDB Atlas for core business data; S3 for receipt objects (store metadata in MongoDB).
- Observability: Amazon CloudWatch for logs/metrics; Amazon SNS for alerts.
- Security: EC2 instance uses an IAM role for least-privilege access to S3 and CloudWatch; secrets are supplied via environment variables.

## Architecture notes
- Short-term: run frontend and backend on a single EC2 instance behind Nginx. Static files served by Nginx, API proxied to Node on an internal port.
- Mid-term: split frontend to S3 + CloudFront, add Route 53 and ACM for HTTPS, and harden with AWS WAF as needed.

## Implementation milestones (high level)
- Stage 1 — Design & cost estimate (Weeks 1–2): finalize data model, choose instance type, estimate S3 needs.
- Stage 2 — Core features (Weeks 3–4): authentication, group management, expense creation, receipt upload.
- Stage 3 — Deploy to AWS (Weeks 5–6): EC2 setup, S3 bucket, IAM role, CloudWatch.
- Stage 4 — Test & finalize (Weeks 7–8): functional testing, monitoring, cost tuning, documentation.

## Budget (ballpark)
Initial student-oriented deployment can run on Free Tier resources for development. Minimal estimated monthly cost for a low-traffic demo is a few USD (EC2 + small S3 usage + CloudWatch). Revisit estimates with the AWS Pricing Calculator for production traffic.

## Risks and mitigations
- Risk: secret leakage — Mitigate by using IAM roles and environment variables, avoid embedding keys.
- Risk: receipt upload failures — Mitigate by validation and retry logic, and by storing metadata separately in MongoDB.
- Risk: cost overrun — Mitigate by setting AWS Budgets and CloudWatch alarms.

## Outcome
Deliver a simple, maintainable demo that implements the core flows: record expense, compute shares, attach receipts, and mark settlements. The design is intended to be extensible (separate frontend and backend, add CDN, use managed services) as usage grows.