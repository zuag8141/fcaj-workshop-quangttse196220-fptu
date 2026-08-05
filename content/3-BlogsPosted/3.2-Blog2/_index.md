---
title: "Blog 2"
date: "2026-07-14"
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# How Scale to Win strengthened DDoS protection with AWS WAF — summary

**Source:** Adapted from an AWS Architecture article about Scale to Win

Scale to Win experienced high‑volume DDoS traffic during a major campaign. Their approach focused on rejecting malicious requests at the edge and preventing attackers from bypassing the CDN.

Core techniques:
- Place Amazon CloudFront in front of the Application Load Balancer so the edge absorbs bulk traffic.
- Use AWS WAF with rate‑based rules, CAPTCHA/challenge flows and bot controls at the CloudFront layer.
- Prevent direct origin bypass by restricting the ALB security group to CloudFront IP ranges and requiring a private header from CloudFront.
- Combine heuristic detection (request patterns, headers, TLS fingerprints) with segmented rate limits so legitimate shared‑IP clients are not accidentally blocked.

Outcome: a layered, practical defense that reduces regional load and preserves legitimate traffic while improving incident response.