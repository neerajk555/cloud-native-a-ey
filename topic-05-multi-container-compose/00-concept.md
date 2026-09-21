# Topic 05: Multi-Container Apps with Docker Compose

Real apps are rarely one container — a web server, an API, and a database each need their own networking, startup order, and configuration. Docker Compose lets you describe an entire multi-container app in one YAML file and bring it all up (or down) with a single command.

**How this topic is organized:** This topic is local-only. Compose's direct cloud equivalent is ECS task definitions, which this course's AWS account isn't set up to use — instead, the SAME multi-container mental model (services, networking-by-name, shared configuration) carries directly into Kubernetes starting in Topic 6, which you WILL deploy to real AWS infrastructure (the shared EKS cluster). Work through the local exercises fully before touching AWS -
they're free, fast to repeat, and build the exact muscle memory the AWS exercise assumes you already have.
