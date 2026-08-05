---
title: "Blog 2"
date: 2026-07-17
weight: 1
chapter: false
pre: " <b> 3.2. </b> "
---

# How Scale to Win Strengthened DDoS Protection with AWS WAF

> **Original source:** [AWS Architecture Blog — How Scale to Win uses AWS WAF to block DDoS events](https://aws.amazon.com/blogs/architecture/how-scale-to-win-uses-aws-waf-to-block-ddos-events/)

**Authors:** Ben “Fuzzy” Shonaldmann, Fenil Patel, and Patrick Duffy  
**Published:** July 14, 2025  
**Category:** AWS Architecture, AWS WAF, Customer Solutions

## Background

Scale to Win is a campaign technology company established by organizers with experience working on major political campaigns in the United States. Its platform originally focused on peer-to-peer texting and later expanded to support calling, fundraising, and broader campaign communication activities.

During the 2024 United States presidential election cycle, the company became the target of distributed denial-of-service attacks. At the highest point of the attacks, its systems received more than two million requests per second from almost ten thousand different IP addresses.

To improve the security and availability of its services, Scale to Win worked with [Amazon Web Services](https://aws.amazon.com/) and implemented a layered protection architecture using:

- [AWS WAF](https://aws.amazon.com/waf/)
- [AWS Shield Advanced](https://aws.amazon.com/shield/)
- [Amazon CloudFront](https://aws.amazon.com/cloudfront/)

A major part of the solution involved using [AWS WAF CAPTCHA and Challenge](https://docs.aws.amazon.com/waf/latest/developerguide/waf-captcha-and-challenge.html).

Suspicious clients were required to complete a CAPTCHA after exceeding a lower request-rate threshold. A separate and higher rate threshold was also configured to block extremely large amounts of traffic from a single shared IP address.

The company also needed to address a more advanced attack technique. An attacker could solve one CAPTCHA, copy the resulting token, and distribute that token to multiple machines. The final architecture therefore combined edge protection, traffic segmentation, rate-based rules, CAPTCHA challenges, and bot-management controls.

## 1. Moving Protection to the AWS Edge

### Original Architecture

In the original architecture, public DNS records pointed directly to an Application Load Balancer.

The load balancer acted as the public entry point and forwarded incoming requests to an Auto Scaling group of [Amazon EC2](https://aws.amazon.com/ec2/) instances.

![Scale to Win's initial architecture with an Application Load Balancer connecting directly to Amazon EC2 instances in an Auto Scaling group](/aws_internship/images/blog2/original-architecture.png)

Although this architecture supported normal application traffic, the Application Load Balancer remained directly exposed to large volumes of malicious requests.

### Updated Architecture

The redesigned architecture placed Amazon CloudFront and AWS WAF in front of the load balancer.

![The updated architecture with Amazon CloudFront and AWS WAF placed in front of the Application Load Balancer](/aws_internship/images/blog2/cloudfront-waf-architecture.png)

This architecture provided several important benefits.

First, CloudFront could absorb and process very large volumes of traffic at the AWS edge before the requests reached regional infrastructure.

Second, AWS WAF deployed with CloudFront could scale more effectively than a regional-only web ACL attached directly to the Application Load Balancer.

Third, CloudFront could reject traffic from specific geographic locations through [geographic restrictions](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/georestrictions.html).

A significant percentage of the malicious traffic received by Scale to Win originated from regions where the company did not need to provide access. Blocking those locations at the CloudFront layer reduced both the amount of traffic reaching AWS WAF and the associated request-processing costs.

## 2. Preventing Direct Access to the Load Balancer

Placing CloudFront in front of an application does not fully protect the origin if attackers can still connect directly to the Application Load Balancer.

Scale to Win added two additional controls to prevent users from bypassing CloudFront.

### Allowing Only CloudFront Traffic

The security group associated with the Application Load Balancer was configured to accept connections only from CloudFront edge locations.

This configuration used the AWS-managed CloudFront prefix list.

More information is available in [CloudFront edge locations and IP address ranges](https://docs.aws.amazon.com/AmazonCloudFront/latest/DeveloperGuide/LocationsOfEdgeServers.html).

With this rule in place, traffic coming directly from the public internet could not establish a connection with the load balancer unless it originated from a CloudFront address.

### Adding a Shared Secret Header

CloudFront was also configured to attach a private custom header to every request sent to the origin.

Example:

```text
x-stw-example-secret: <private-value>
```
![Configure a custom header in Amazon CloudFront origin settings](/aws_internship/images/blog2/cloudfront-custom-header.png)

The Application Load Balancer listener inspected this header before forwarding the request to the application.

```text
IF x-stw-example-secret contains the correct value
    Forward the request to the application target group
ELSE
    Return HTTP 403
```
![The Application Load Balancer listener rule verifies the shared secret header before forwarding the request](/aws_internship/images/blog2/alb-listener-secret-rule.png)

This control prevented attackers from creating their own CloudFront distribution and configuring it to use the same Application Load Balancer as an origin.

Even if the traffic came from an authorized CloudFront IP address, the request would still be rejected unless it included the correct private header value.

The secret value could also be rotated automatically by using:

* [AWS Secrets Manager](https://aws.amazon.com/secrets-manager/)
* [AWS Lambda](https://aws.amazon.com/lambda/)
* [AWS CloudTrail](https://aws.amazon.com/cloudtrail/)

CloudTrail could record configuration changes, while Secrets Manager and Lambda could generate a new secret and update both the CloudFront origin configuration and the Application Load Balancer listener rule.

## 3. Combining Heuristic Rules with Hard Rate Limits

Scale to Win used two complementary methods to detect and stop malicious traffic:

1. Heuristic detection based on suspicious request characteristics.
2. Hard rate limits based on properties that were more difficult for attackers to manipulate.

### Heuristic Detection

The team reviewed:

* [AWS WAF sampled web requests](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl-testing-view-sample.html)
* [AWS WAF traffic logs](https://docs.aws.amazon.com/waf/latest/developerguide/logging.html)

The goal was to identify attributes that appeared frequently in attack traffic but rarely in legitimate requests.

Possible indicators included:

* Unusual HTTP headers
* Suspicious query-string values
* Unexpected request paths
* Repeated request-body patterns
* Abnormal combinations of request properties

A practical process for testing heuristic rules was:

1. Create an [AWS WAF rule](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rules.html) that matches the suspicious pattern.
2. Initially configure the rule with the `Count` action instead of blocking traffic.
3. Compare rule matches with high-volume source IP addresses.
4. Use [Amazon Athena](https://aws.amazon.com/athena/) to [query AWS WAF logs](https://docs.aws.amazon.com/athena/latest/ug/waf-logs.html).
5. Review false-positive and false-negative matches.
6. Change the rule action to `Block` after confirming that the rule is sufficiently accurate.

Starting with the `Count` action allowed the team to test the rule against production traffic without interrupting legitimate users.

However, heuristic detection has an important limitation. Attackers can change request characteristics after discovering which values are being blocked.

For example, attackers may:

* Randomize HTTP headers
* Modify request paths
* Replay realistic session data
* Change query parameters
* Reproduce normal browser behavior

As a result, heuristic rules need continuous observation and adjustment.

### JA4 and JA3 Fingerprints

TLS fingerprints such as JA4 and JA3 can help group clients according to the characteristics of their TLS handshakes.

A large number of automated clients using the same software may produce the same or similar fingerprint.

However, fingerprint-based blocking creates two risks.

First, a malicious client may share a fingerprint with a legitimate browser, runtime, or API library. Blocking the fingerprint could therefore affect real users.

Second, advanced attackers may deliberately change parts of the TLS handshake to generate multiple fingerprints.

For these reasons, TLS fingerprints are useful security signals, but they should not be used as the only mechanism for blocking traffic.

### Hard Rate Limits

The second method relied on request properties that were more difficult to falsify, particularly the source IP address.

Although network packets can contain spoofed addresses, a spoofed client usually cannot complete the TCP and TLS handshakes required to send a valid HTTP request and receive a response.

AWS WAF supports per-source request limits through [rate-based rule statements](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-rate-based.html).

However, Scale to Win could not use one universal request limit for all traffic.

Different parts of the platform had very different traffic patterns.

For example:

* Some APIs received large volumes of machine-generated traffic.
* Providers such as Twilio could send thousands of callbacks from a small number of IP addresses.
* Campaign offices could place hundreds of legitimate users behind one public IP address.
* Universities or phone banks could also generate large volumes of valid traffic from shared networks.

A rate limit that was low enough to stop an attacker could also block an entire campaign office.

The company therefore separated traffic into different categories before applying rate limits.

## 4. Separating Machine Traffic from Browser Traffic

AWS WAF rules can inspect and match request URI paths.

Scale to Win used URI patterns to distinguish machine-to-machine endpoints from routes accessed by human users through web browsers.

```text
Known webhook and API paths -> Machine traffic
All other application paths -> Browser traffic
```

Each traffic type was then evaluated using a different protection strategy.

### Machine-to-Machine Traffic

CAPTCHA is not suitable for API clients, webhook services, or other automated systems because these clients cannot interact with a visual challenge.

For API clients, Scale to Win configured expected per-IP limits.

When a client exceeded its permitted rate, the application or protection layer returned:

```text
HTTP 429 Too Many Requests
```

The API clients were designed to recognize temporary failures and retry the request later.

For webhook providers, the team created an [AWS WAF IP set](https://docs.aws.amazon.com/waf/latest/developerguide/waf-ip-set-managing.html) containing trusted provider IP addresses.

A request to a webhook route was rejected when its source IP was not included in the approved IP set.

![AWS WAF rule that allows machine traffic when the URI path and source IP address match the configured conditions](/aws_internship/images/blog2/waf-machine-traffic-rule-1.png)

![AWS WAF rule that allows machine traffic when the URI path and source IP address match the configured conditions](/aws_internship/images/blog2/waf-machine-traffic-rule-2.png)

Where possible, machine-to-machine traffic should also use stronger identity controls such as:

* API keys
* Cryptographically signed requests
* Certificate-based authentication
* Stable source IP addresses
* Provider-specific webhook signatures
* Request timestamps and replay protection

Expected machine traffic was allowed before browser-focused CAPTCHA and blocking rules were evaluated.

### Browser Traffic

Browser traffic was controlled using a two-level rate-limit model.

#### Lower Rate Threshold

The lower threshold represented the maximum request volume expected from a small group of users sharing the same IP address.

When an IP address exceeded this threshold, AWS WAF returned a CAPTCHA challenge.

The CAPTCHA helped distinguish a real user from a basic automated client.

#### Upper Rate Threshold

The upper threshold represented the maximum legitimate volume expected from a large office, university, phone bank, or other shared network.

When an IP address exceeded the upper threshold, AWS WAF blocked the traffic completely.

The upper blocking rule needed to be evaluated before the lower CAPTCHA rule.

This order was important because solving a CAPTCHA should not allow a client to send an unlimited number of requests.

The expected behavior was:

```text
Normal shared IP
    -> No interruption

Busy but legitimate shared IP
    -> CAPTCHA challenge

Extremely high-volume IP
    -> Request blocked
```

![Rule evaluation order within the AWS WAF Web ACL](/aws_internship/images/blog2/waf-rule-priority.png)

This approach allowed Scale to Win to protect the application without unnecessarily blocking legitimate users working from shared networks.

## 5. Integrating CAPTCHA into the Frontend

For traditional server-rendered websites, AWS WAF can manage most of the CAPTCHA workflow automatically.

The process works as follows:

1. A request reaches a CAPTCHA-protected rule without a valid token.
2. AWS WAF displays a managed CAPTCHA page.
3. The user completes the CAPTCHA.
4. AWS WAF stores a token in a browser cookie.
5. Future requests are allowed during the configured immunity period.

Single-page applications require additional frontend logic because much of their traffic is sent through background API requests rather than full page loads.

A React application can use the following flow:

```text
Send API request using AwsWafIntegration.fetch()
    |
    +-- HTTP 200
    |      -> Parse the response
    |      -> Display the requested content
    |
    +-- HTTP 405
           -> Render the AWS WAF CAPTCHA
                  |
                  +-- CAPTCHA successful
                  |      -> Retry the original API request
                  |
                  +-- CAPTCHA failed
                         -> Display an application error
```

The frontend application uses the AWS WAF JavaScript integration and CAPTCHA APIs to detect when a challenge is required.

After the user successfully completes the challenge, the original request is sent again with a valid AWS WAF token.

The complete React implementation is available in the [original AWS Architecture Blog article](https://aws.amazon.com/blogs/architecture/how-scale-to-win-uses-aws-waf-to-block-ddos-events/).

## 6. Preventing CAPTCHA Token Reuse

A valid CAPTCHA token may still become a security risk if an attacker solves one challenge and distributes the resulting token to many machines.

This allows multiple automated clients to appear as if they successfully completed a CAPTCHA.

[AWS WAF Bot Control](https://aws.amazon.com/waf/features/bot-control/) can detect suspicious token reuse patterns across:

* Different IP addresses
* Different autonomous system numbers
* Different countries
* Different network locations

Scale to Win used the following implementation approach:

1. Add the [AWSManagedRulesBotControlRuleSet](https://docs.aws.amazon.com/waf/latest/developerguide/aws-managed-rule-groups-bot.html) after the custom rate-limiting rules.
2. Use the targeted inspection and protection level.
3. Override individual managed rules with `Count` or `Allow` when they should not block a specific category of legitimate traffic.
4. Monitor production requests.
5. Adjust rule actions based on real traffic patterns.

Placing Bot Control after the custom rate-based rules allowed the platform to apply its own traffic policy first.

The managed bot rules could then detect CAPTCHA token sharing, automated browsers, and other suspicious behavior that was not captured by the custom limits.

## Final Architecture

The resulting protection flow can be summarized as follows:

```text
Internet users
      |
      v
Amazon CloudFront
      |
      +-- Geographic restrictions
      |
      v
AWS WAF
      |
      +-- Heuristic rules
      +-- Trusted machine traffic rules
      +-- API and webhook rules
      +-- Upper blocking rate limit
      +-- Lower CAPTCHA rate limit
      +-- AWS WAF Bot Control
      |
      v
Application Load Balancer
      |
      +-- CloudFront-only security group
      +-- Shared-secret header validation
      |
      v
Application servers on Amazon EC2
```

## Key Takeaways

Scale to Win’s DDoS protection strategy did not depend on one AWS service or one security rule.

Instead, it used multiple defensive layers.

* Amazon CloudFront absorbed and filtered traffic at the AWS edge.
* AWS WAF inspected requests and applied custom security rules.
* AWS Shield Advanced provided additional DDoS protection.
* Geographic restrictions blocked traffic from unnecessary regions.
* The Application Load Balancer accepted only traffic from CloudFront.
* A private custom header prevented unauthorized CloudFront distributions from reaching the origin.
* Machine traffic and browser traffic followed different validation paths.
* Trusted webhook providers were verified using IP sets.
* API clients were controlled using appropriate rate limits.
* A lower request threshold triggered CAPTCHA challenges.
* A higher threshold blocked extreme traffic volumes.
* AWS WAF Bot Control detected CAPTCHA token reuse and other automated behavior.

This layered approach allowed Scale to Win to protect its application from large distributed HTTP attacks while maintaining access for legitimate users, including users working from offices and other shared networks.

## Further Reading

* [AWS WAF](https://aws.amazon.com/waf/)
* [AWS Shield](https://aws.amazon.com/shield/)
* [Amazon CloudFront](https://aws.amazon.com/cloudfront/)
* [AWS WAF CAPTCHA and Challenge](https://docs.aws.amazon.com/waf/latest/developerguide/waf-captcha-and-challenge.html)
* [AWS WAF rate-based rules](https://docs.aws.amazon.com/waf/latest/developerguide/waf-rule-statement-type-rate-based.html)
* [AWS WAF Bot Control](https://aws.amazon.com/waf/features/bot-control/)
* [AWS WAF sampled web requests](https://docs.aws.amazon.com/waf/latest/developerguide/web-acl-testing-view-sample.html)
* [AWS WAF logging](https://docs.aws.amazon.com/waf/latest/developerguide/logging.html)
* [Querying AWS WAF logs with Amazon Athena](https://docs.aws.amazon.com/athena/latest/ug/waf-logs.html)
* [AWS WAF IP sets](https://docs.aws.amazon.com/waf/latest/developerguide/waf-ip-set-managing.html)
* [Fine-tune and optimize AWS WAF Bot Control mitigation capability](https://aws.amazon.com/blogs/security/fine-tune-and-optimize-aws-waf-bot-control-mitigation-capability/)

## About the Authors

### Ben “Fuzzy” Shonaldmann

Ben is a software engineer with experience building, scaling, and securing technology products. His professional background also includes political campaigns, voter-engagement organizations, and political technology companies.

### Fenil Patel

Fenil is an AWS Solutions Architect based in New Jersey. His work focuses on helping customers improve and secure content delivery through AWS edge services.

### Patrick Duffy

Patrick is an AWS Solutions Architect working with commercial customers. He focuses on improving cloud security awareness and helping organizations strengthen the security of their AWS workloads.

