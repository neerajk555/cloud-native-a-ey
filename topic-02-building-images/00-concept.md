# Topic 02: Building Images

A Dockerfile is a recipe for building an image. Each instruction adds a layer, and Docker caches layers — if a layer's inputs haven't changed, it reuses the cached result instead of rebuilding it, which is why instruction ORDER in a Dockerfile matters for build speed.

A **registry** stores images so they can be pulled elsewhere. Docker Hub is the public default; **Amazon ECR** is AWS's private registry — same underlying concept, different address and authentication.

**How this topic is organized:** Local exercises: write a Dockerfile, understand caching, build a multi-stage image. AWS exercise: push that exact same image to a private ECR repository, using your IAM user's scoped ECR access. Work through the local exercises fully before touching AWS -
they're free, fast to repeat, and build the exact muscle memory the AWS exercise assumes you already have.
