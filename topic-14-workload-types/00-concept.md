# Topic 14: Workload Types

A Deployment expects its Pods to run FOREVER. A Job expects its Pod to run ONCE and finish. A CronJob creates Jobs on a schedule. A StatefulSet gives Pods stable, ordered identity for cases like databases where that matters. Choosing the right one is mostly a matter of asking the right questions, not memorizing syntax.

**How this topic is organized:** Local exercises: Jobs, CronJobs, StatefulSet vs. Deployment, and a decision-tree exercise across 8 scenarios. AWS exercise: apply the exact same Job/CronJob YAML to your EKS namespace - this reuses permissions you already have and completes in seconds, so it's a free, low-friction way to confirm the concept transfers directly. Work through the local exercises fully before touching AWS -
they're free, fast to repeat, and build the exact muscle memory the AWS exercise assumes you already have.
