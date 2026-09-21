# Topic 16: CI/CD Basics

Continuous Integration/Continuous Deployment automates the build-test-deploy cycle so it happens the same way every time, without manual steps. Before using a cloud CI/CD service, it helps to understand the PATTERN itself: source -> build -> test -> deploy, each stage gated on the previous one succeeding.

**How this topic is organized:** Local exercise: simulate the same 4-stage pipeline with a plain bash script, so the PATTERN is clear before any cloud service is involved. AWS exercise: run an equivalent build stage for real using AWS CodeBuild and your shared execution role. Work through the local exercises fully before touching AWS -
they're free, fast to repeat, and build the exact muscle memory the AWS exercise assumes you already have.
