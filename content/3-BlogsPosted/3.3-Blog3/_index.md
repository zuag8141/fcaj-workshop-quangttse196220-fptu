---
title: "Blog 3"
date: 2026-07-17
weight: 1
chapter: false
pre: " <b> 3.3. </b> "
---
# Architecting for Agentic AI Development on AWS

> **Original article:** [Architecting for agentic AI development on AWS](https://aws.amazon.com/blogs/architecture/architecting-for-agentic-ai-development-on-aws/)

**Author:** Alan Oberto Jimenez  
**Published:** March 26, 2026  
**Categories:** Architecture, Intermediate (200), Kiro

## Introduction

Cloud architectures designed for conventional development workflows often create unnecessary friction when software is built with AI agents.

Many development teams are experimenting with AI coding assistants and autonomous agents. However, they frequently discover a gap between the capabilities promised by these tools and the limitations imposed by their existing systems.

An AI agent may generate or modify code in seconds, but validating that change can still take several minutes or even hours. Long deployment cycles, tightly coupled services, shared development environments, and codebases with unclear structures make every iteration slower and more difficult.

When feedback takes too long, AI agents cannot operate effectively on their own. Developers must repeatedly step back into the process to deploy changes, inspect errors, and validate results manually.

Agentic development represents a broader model than traditional code assistance. Instead of only recommending code snippets, an AI agent can participate in a complete development cycle:

- Interpret requirements
- Design a solution
- Write code
- Execute tests
- Deploy resources
- Observe application behavior
- Identify failures
- Refine the implementation

To support this workflow, both the system architecture and the codebase must be designed around three principles:

1. Fast validation
2. Safe and isolated iteration
3. Explicit architectural intent

This article presents architectural patterns for building AWS environments that support agentic development. It first explains why traditional architectures limit AI agents, then introduces system-level patterns for rapid feedback and codebase-level patterns that help agents understand, modify, and validate applications reliably.

## Why Traditional Architectures Hinder Agentic AI

Most cloud architectures were originally designed for development processes controlled primarily by humans.

They often assume:

- Long-lived development and testing environments
- Manual validation
- Infrequent deployments
- Centralized integration environments
- Human understanding of undocumented conventions
- Direct dependencies between application code and cloud services

These assumptions do not work well in an agentic workflow.

AI agents need to validate changes continuously. If every test requires provisioning AWS resources, waiting for a deployment pipeline, or debugging a problem that appears only after deployment, the feedback loop becomes too slow.

Tightly coupling business logic to managed services also makes local testing difficult. For example, if application rules directly call Amazon DynamoDB, Amazon SNS, or another cloud service, an agent cannot easily validate that logic without access to a deployed environment.

Inconsistent repository structures create additional uncertainty. An agent may struggle to determine:

- Where a new feature should be implemented
- Which modules are safe to modify
- Which architectural constraints must be followed
- Which services may be affected
- Which tests are relevant
- How the application should be deployed

Without architectural support, autonomous development can introduce more risk than value.

The solution is not simply to create better prompts. The system must be designed so that rapid feedback, predictable structure, and clear boundaries are treated as primary architectural concerns.

## System Architecture for Fast Agentic Feedback Loops

The effectiveness of agentic development depends heavily on how quickly an agent can observe the results of its work.

A fast feedback loop allows an agent to:

1. Make a change.
2. Execute the relevant validation.
3. Inspect the result.
4. Correct the implementation.
5. Repeat the process.

The shorter this cycle becomes, the more effectively the agent can refine its output.

A complete agentic development architecture can combine local testing, short-lived cloud environments, automated delivery pipelines, and production feedback.

![High-level architecture enabling agentic development on AWS](/aws_internship/images/blog3/agentic-ai-development-architecture.png)

*Figure 1: A high-level architecture for agentic development, including local test loops, an ephemeral test stack, and an AI-triggered CI/CD pipeline.*

## Local Emulation as the Default Feedback Path

Whenever possible, AI-generated changes should be tested locally before the agent provisions or modifies cloud resources.

Local emulation provides several advantages:

- Faster execution
- Lower cost
- Reduced risk
- Easier debugging
- More frequent testing
- Less dependence on shared environments

### Serverless applications

Applications built with [AWS Lambda](https://aws.amazon.com/lambda/) and [Amazon API Gateway](https://aws.amazon.com/api-gateway/) can be tested locally with the [AWS Serverless Application Model](https://aws.amazon.com/serverless/sam/).

For example, the following command starts a locally emulated API Gateway endpoint:

```bash
sam local start-api
```

An AI agent can then perform the following cycle:

```text
Modify a Lambda function
        |
        v
Start the local API
        |
        v
Send a test request
        |
        v
Inspect the response and logs
        |
        v
Update the implementation
```

Because the application does not need to be deployed after every change, the agent can validate its work in seconds instead of waiting for a complete cloud deployment.

### Container-based applications

Applications designed to run on [Amazon Elastic Container Service](https://aws.amazon.com/ecs/) or [AWS Fargate](https://aws.amazon.com/fargate/) can use the same container images during local development.

The agent can build and run a container locally to validate:

- Application startup
- API behavior
- Environment configuration
- Dependency installation
- Error handling
- Container health checks
- Runtime compatibility

Testing the same image locally reduces the difference between development and production behavior.

### Local data persistence

Applications that depend on [Amazon DynamoDB](https://aws.amazon.com/dynamodb/) can use DynamoDB Local.

DynamoDB Local implements the DynamoDB API in a local environment, allowing the agent to test:

- Create operations
- Read operations
- Update operations
- Delete operations
- Query behavior
- Data validation

The agent can therefore verify data-access logic without connecting to a shared AWS table.

> **Key point:** Local emulation allows AI-generated code to be validated in seconds, potentially reducing both the cost and risk of experimentation.

## Offline Development for Data and Analytics Workloads

Not every application follows a simple request-response model.

Data-processing systems often involve:

- Large datasets
- Distributed execution
- Batch processing
- Transformation pipelines
- Data-quality rules
- Managed analytics services

Even for these workloads, development should begin with the smallest practical feedback loop.

[AWS Glue](https://aws.amazon.com/glue/) provides Docker images that include AWS Glue ETL libraries. These images make it possible to execute AWS Glue jobs in a local container.

An AI agent can use this environment to:

1. Load a representative sample dataset.
2. Run transformation logic locally.
3. Inspect intermediate data.
4. Detect schema and quality problems.
5. Modify the transformation.
6. Validate the updated result.
7. Move the job to AWS only when scale testing is necessary.

A general workflow for data and machine learning workloads is:

```text
Separate the core processing logic
        |
        v
Use a reduced local dataset
        |
        v
Validate transformations and outputs
        |
        v
Correct data or schema issues
        |
        v
Deploy the validated workload to AWS
        |
        v
Test at production scale
```

This approach prevents the agent from repeatedly starting expensive cloud jobs during early experimentation.

> **Key point:** Offline development shortens feedback loops for data workloads and reduces unnecessary cloud executions during the initial development stages.

## Hybrid Testing with Lightweight Cloud Resources

Some AWS services cannot be reproduced completely in a local environment.

For these services, the goal should not be to eliminate cloud testing. Instead, the architecture should make cloud validation small, isolated, automated, and predictable.

For event-driven systems using [Amazon Simple Notification Service](https://aws.amazon.com/sns/) or [Amazon Simple Queue Service](https://aws.amazon.com/sqs/), teams can define minimal testing stacks through infrastructure as code.

Suitable tools include:

- [AWS CloudFormation](https://aws.amazon.com/cloudformation/)
- [AWS Cloud Development Kit](https://aws.amazon.com/cdk/)

An AI agent can deploy only the resources required for one test.

For example:

```text
AI agent
   |
   v
Deploy a temporary SQS queue
   |
   v
Deploy or invoke the component being tested
   |
   v
Send a test message
   |
   v
Inspect the result
   |
   v
Delete the temporary resources
```

The agent can use the AWS SDK to interact with the real service and confirm that the application behaves correctly.

Temporary test stacks should include:

- Predictable naming conventions
- Minimal resource scope
- Automated provisioning
- Automated cleanup
- Restricted IAM permissions
- Defined lifetime limits
- Cost controls

This model treats the cloud as a controlled test dependency rather than requiring a complete environment for every validation cycle.

> **Key point:** Hybrid testing confirms real AWS service behavior early while keeping cloud usage focused and controlled.

## Preview Environments and Contract-First Design

Local testing and lightweight cloud resources are valuable, but end-to-end validation is still necessary when multiple services interact.

A preview environment is a short-lived application stack created for a specific feature, branch, or pull request.

Because the environment is defined through infrastructure as code, an AI agent can create and remove it consistently.

A preview environment allows the agent to:

- Deploy the full application
- Configure dependent services
- Validate service-to-service communication
- Run integration tests
- Run smoke tests
- Inspect logs
- Collect test results
- Destroy the environment after validation

A typical workflow is:

```text
AI-generated change
        |
        v
Create a feature branch
        |
        v
Deploy a preview environment
        |
        v
Run contract, integration, and smoke tests
        |
        +-- Tests pass --> Continue to review or merge
        |
        +-- Tests fail --> Return results to the agent
```

Preview environments work particularly well with contract-first development.

In contract-first design, service interfaces are defined before implementation. For APIs, an OpenAPI specification can describe:

- Endpoints
- HTTP methods
- Request parameters
- Request schemas
- Response schemas
- Authentication requirements
- Error responses

An AI agent can use the contract to create or validate clients and services before every component is fully implemented.

> **Key point:** Preview environments reduce integration risk and provide a safe place to validate AI-generated changes before they reach production.

## Codebase Architecture for AI-Friendly Development

System architecture determines how quickly an agent can test a change. Codebase architecture determines whether the agent understands what it is changing.

An AI-friendly repository should provide:

- Predictable organization
- Explicit architectural boundaries
- Clear dependency rules
- Fast and relevant tests
- Machine-readable documentation
- Consistent development commands

## Domain-Driven Structure with Explicit Boundaries

Agentic development is more effective when the repository clearly communicates architectural intent.

A structure inspired by Domain-Driven Design can separate core business logic from application orchestration and infrastructure integrations.

A project might use the following structure:

```text
src/
├── domain/
│   ├── entities/
│   ├── value-objects/
│   ├── services/
│   └── rules/
├── application/
│   ├── commands/
│   ├── queries/
│   ├── use-cases/
│   └── interfaces/
├── infrastructure/
│   ├── repositories/
│   ├── persistence/
│   ├── messaging/
│   └── aws/
└── interfaces/
    ├── api/
    ├── events/
    └── cli/
```

The `/domain` layer contains business rules and should not directly depend on AWS services.

The `/application` layer coordinates use cases and defines the interfaces needed by the domain.

The `/infrastructure` layer provides integrations with systems such as:

- Amazon DynamoDB
- Amazon SNS
- Amazon SQS
- AWS Lambda
- External APIs

This separation allows an AI agent to modify and test business logic without provisioning cloud infrastructure.

[Hexagonal architecture](https://docs.aws.amazon.com/prescriptive-guidance/latest/hexagonal-architectures/welcome.html) reinforces the same concept by representing databases, queues, APIs, and cloud services as external adapters.

For example, application logic may depend on a repository interface:

```typescript
export interface CustomerRepository {
  findById(customerId: string): Promise<Customer | null>;
  save(customer: Customer): Promise<void>;
}
```

The infrastructure layer can then provide an Amazon DynamoDB implementation:

```typescript
export class DynamoDbCustomerRepository implements CustomerRepository {
  async findById(customerId: string): Promise<Customer | null> {
    // DynamoDB implementation
    return null;
  }

  async save(customer: Customer): Promise<void> {
    // DynamoDB implementation
  }
}
```

For unit testing, the agent can replace this adapter with an in-memory implementation.

> **Key point:** Clear boundaries reduce unintended side effects and make AI-generated changes easier to understand and test.

## Encoding Architectural Intent with Project Rules

Repository structure alone may not communicate every architectural decision.

A project may include rules such as:

- Domain code must not import AWS SDK packages.
- Database access must go through repository classes.
- Lambda handlers must remain thin.
- Public APIs must validate input.
- Infrastructure must be defined using AWS CDK.
- New features must include automated tests.
- IAM policies must follow least-privilege principles.

These rules should be written in a format that an AI agent can automatically access.

[Kiro](https://kiro.dev/) supports [steering files](https://kiro.dev/docs/cli/steering/), which are Markdown documents stored under:

```text
.kiro/steering/
```

A project can organize steering files as follows:

```text
.kiro/
└── steering/
    ├── architecture.md
    ├── coding-standards.md
    ├── testing.md
    ├── security.md
    └── deployment.md
```

For example:

```markdown
# Data Access Rules

- Domain and application code must not call DynamoDB directly.
- All database operations must use repository interfaces.
- Repository implementations must be stored in `src/infrastructure/repositories/`.
- Unit tests must use in-memory repository implementations.
```

The agent can consult these rules while performing a task.

This approach reduces the need to repeat the same constraints in every prompt and helps prevent generated code from drifting away from the intended architecture.

> **Key point:** Project rules reduce architectural drift and help maintain consistency as AI agents operate more autonomously.

## Tests as Executable Specifications

In agentic development, tests do more than identify regressions. They describe the behavior the system is expected to provide.

A layered testing strategy is particularly effective.

### Unit tests

Unit tests validate domain and application logic in isolation.

They should:

- Run quickly
- Avoid network access
- Avoid real AWS resources
- Produce deterministic results
- Clearly identify failed behavior

Fast unit tests provide the primary feedback loop for frequent AI-generated changes.

### Contract tests

Contract tests verify that services follow agreed interfaces.

They can validate:

- API request formats
- API response formats
- Event schemas
- Required fields
- Error codes
- Compatibility between services

Contract tests detect breaking changes before multiple services are deployed together.

### Smoke tests

Smoke tests run against a deployed environment and confirm that critical functionality works.

They may verify that the application can:

- Start successfully
- Access required resources
- Handle a basic request
- Publish an event
- Consume an event
- Read data
- Write data

Smoke tests can reveal problems that appear only at runtime, including missing [AWS Identity and Access Management](https://aws.amazon.com/iam/) permissions.

A complete validation flow might be:

```text
Code change
    |
    v
Unit tests
    |
    v
Contract tests
    |
    v
Deploy preview environment
    |
    v
Smoke and integration tests
    |
    v
Production approval
```

Tests also act as machine-readable documentation.

When a test fails, the agent can inspect:

- The test description
- Expected output
- Actual output
- Error messages
- Stack traces

The agent can use that information to refine the implementation.

> **Key point:** Tests provide fast and objective validation for AI-generated code while reducing the risk of subtle integration failures.

## Monorepositories and Machine-Readable Documentation

AI agents perform better when they can access broad and consistent context.

A monorepository places multiple services, shared libraries, contracts, and infrastructure definitions in one repository.

For example:

```text
repository/
├── services/
│   ├── customer-service/
│   ├── order-service/
│   └── notification-service/
├── packages/
│   ├── shared-domain/
│   ├── api-contracts/
│   └── testing-tools/
├── infrastructure/
├── docs/
└── .kiro/
```

This structure allows the agent to:

- Navigate between related services
- Discover shared patterns
- Update common interfaces
- Identify cross-service dependencies
- Evaluate system-wide effects
- Run tests for all affected components

Documentation should be concise, structured, and easy for both humans and agents to interpret.

Useful files may include:

```text
AGENT.md
ARCHITECTURE.md
RUNBOOK.md
CONTRIBUTING.md
SECURITY.md
```

`AGENT.md` can describe:

- System architecture
- Important boundaries
- Common commands
- Prohibited modifications
- Testing requirements
- Deployment expectations

`RUNBOOK.md` can document:

- Common operational failures
- Log locations
- Recovery steps
- Rollback procedures
- Escalation processes

`CONTRIBUTING.md` can define:

- Branch conventions
- Pull request requirements
- Code quality standards
- Required tests
- Review procedures

Machine-readable formats are usually easier for agents to process than long and unstructured prose.

Useful formats include:

- YAML
- JSON
- TOML
- OpenAPI
- JSON Schema
- Structured Markdown

Kiro can also use foundational steering documents that summarize the project’s structure, technology choices, and product requirements.

> **Key point:** Shared and structured context improves the quality of AI-generated changes and reduces the need for manual correction.

## Integrating Agents Safely into Delivery Pipelines

As AI agents gain more capabilities, governance remains essential.

AI-generated changes should pass through CI/CD controls before they reach production.

Recommended safeguards include:

- Required unit tests
- Contract tests
- Static analysis
- Dependency scanning
- Infrastructure validation
- Automated reviews
- Branch protection
- Required approvals
- Deployment policies
- Rollback mechanisms

A controlled delivery workflow can follow this model:

```text
AI agent creates a change
        |
        v
Create a feature branch
        |
        v
Run local validation
        |
        v
Open a pull request
        |
        v
Execute automated CI checks
        |
        v
Deploy a preview environment
        |
        v
Run smoke and integration tests
        |
        v
Obtain human approval for high-impact changes
        |
        v
Deploy to production
```

Agent autonomy can increase gradually.

Initially, an agent may only be allowed to:

- Recommend changes
- Modify a feature branch
- Run local tests
- Open pull requests

After the organization gains confidence, the agent may also be allowed to:

- Deploy preview environments
- Correct failed tests
- Update low-risk dependencies
- Merge low-risk changes after validation
- Initiate controlled deployments

Humans should remain involved in high-impact decisions, particularly changes involving:

- IAM permissions
- Security policies
- Production data
- Network configuration
- Compliance controls
- Destructive infrastructure operations

This balance allows AI agents to accelerate routine development without unnecessarily increasing operational risk.

## Conclusion

Successful agentic AI development requires more than adding an AI coding assistant to an existing software process.

The architecture must prioritize:

- Fast feedback
- Clear boundaries
- Explicit project rules
- Repeatable environments
- Automated validation
- Safe deployment controls

Local emulation gives AI agents the fastest possible feedback for application logic.

Lightweight cloud testing allows agents to validate the real behavior of AWS services without repeatedly deploying complete environments.

Preview environments provide isolated end-to-end validation before changes reach production.

Within the codebase, domain-driven organization, hexagonal architecture, layered tests, monorepositories, and machine-readable documentation give agents the context needed to make reliable changes.

Tools such as Kiro help connect human architectural decisions with autonomous execution by making project rules and constraints directly available to the agent.

When system architecture and codebase architecture are aligned with agentic workflows, AI agents can become genuine force multipliers. They can handle rapid implementation and refinement cycles while development teams focus on architecture, product strategy, security, and innovation.

To learn more about implementing agentic solutions on AWS, visit [AWS Agentic AI](https://aws.amazon.com/ai/agentic-ai/).

## About the Author

### Alan Oberto Jimenez

Alan Oberto Jimenez is an Application Architect focused on designing modern cloud-native applications and AI-assisted development systems.

His work covers scalable cloud architecture, autonomous development workflows, agentic technologies, and tools that use AI to simplify and accelerate the software development lifecycle.