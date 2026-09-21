# Topic 13: Probes and Troubleshooting

A liveness probe answers 'is this container still alive, or should it be restarted?' A readiness probe answers 'is this container ready to receive traffic right now?' A failing readiness probe removes a Pod from Service traffic WITHOUT restarting it; a failing liveness probe triggers a restart. These are Kubernetes' more precise version of Topic 4's Docker health checks.

**How this topic is organized:** Local exercises: configure both probe types, then a guided debugging exercise with 3 intentionally broken Pods. AWS exercise: the same probes on your EKS namespace, since the concept and YAML are identical - the value here is just confirming it behaves the same way on real infrastructure. Work through the local exercises fully before touching AWS -
they're free, fast to repeat, and build the exact muscle memory the AWS exercise assumes you already have.
