# Topic 07: Your First Cluster

`kind` (Kubernetes IN Docker) runs a real Kubernetes cluster using Docker containers as its 'nodes' - perfect for free, disposable local learning. On AWS, you will NOT create a cluster yourself - your instructor already created one shared EKS cluster for the whole cohort (Topic 6's exploration exercise showed you this). Your AWS exercise here is connecting to it and confirming your own private workspace (namespace) inside it.

**How this topic is organized:** Local exercises: install kind, create a cluster, write your first raw Pod, learn core kubectl commands. AWS exercise: connect to the shared cluster and verify your own namespace - this is NOT 'create a cluster', since you don't have that permission, and for good reason (cost and quota control across 25+ people). Work through the local exercises fully before touching AWS -
they're free, fast to repeat, and build the exact muscle memory the AWS exercise assumes you already have.
