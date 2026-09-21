# Topic 11: Rolling Updates and Scaling

A rolling update replaces old Pods with new ones gradually, keeping the app available throughout - the declarative model from Topic 6 in action for updates specifically. Scaling up or down is just changing the desired replica count and letting Kubernetes reconcile toward it.

**How this topic is organized:** Local exercises: rolling update, rollback, manual scaling, another self-healing demo. AWS exercise: the same operations on your EKS namespace, this time bounded by your namespace's real ResourceQuota (set by the instructor) - a constraint your local kind cluster doesn't have. Work through the local exercises fully before touching AWS -
they're free, fast to repeat, and build the exact muscle memory the AWS exercise assumes you already have.
