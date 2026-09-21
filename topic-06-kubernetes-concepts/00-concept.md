# Topic 06: Kubernetes Concepts

Compose runs everything on ONE machine. Kubernetes manages a CLUSTER of machines (nodes) as one logical pool, so you can survive a machine dying, scale beyond one machine's capacity, and update with zero downtime. A **Pod** is the smallest deployable unit (usually one container). The **control plane** is the cluster's brain — it decides which node each Pod runs on and continuously works to keep reality matching what you declared (the 'declarative model').

**Declarative vs. imperative:** `docker run` is imperative — 'do this now.' Kubernetes YAML is declarative — 'this is what should always exist.' If a Pod dies, Kubernetes notices and fixes it, without you doing anything.

**How this topic is organized:** Local exercises: pure concept, no CLI (this genuinely doesn't need commands to teach). AWS exercise: a free, read-only look at your shared EKS cluster's control plane and nodes, so you can compare it side-by-side with what you'll build locally with kind in Topic 7. Work through the local exercises fully before touching AWS -
they're free, fast to repeat, and build the exact muscle memory the AWS exercise assumes you already have.
