# Topic 08: Deployments and Services

A Deployment manages a ReplicaSet, which manages Pods - giving you self-healing, scaling, and rolling updates that bare Pods don't have. A Service gives your Deployment's Pods a single, STABLE address, since individual Pod IPs change every time a Pod is replaced. Labels and selectors are the literal mechanism connecting Services to the right Pods.

**How this topic is organized:** Local exercises: Deployments, self-healing, ClusterIP Services, labels/selectors, namespaces - all on your free kind cluster. AWS exercise: the exact same YAML, applied inside your own namespace on the shared EKS cluster. Work through the local exercises fully before touching AWS -
they're free, fast to repeat, and build the exact muscle memory the AWS exercise assumes you already have.
