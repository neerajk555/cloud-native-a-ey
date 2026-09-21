# Topic 04: Container Reliability

Docker can periodically check if your app is actually WORKING (a health check), not just running, and can automatically restart a container that crashes (a restart policy). Resource limits stop one container from starving everything else on the same machine.

**How this topic is organized:** This topic is local-only. Its direct cloud equivalent (ECS task health checks) isn't part of this course's AWS service list — but everything here still applies conceptually once you reach Kubernetes probes in Topic 13, which you'll practice on both kind and the shared EKS cluster. Work through the local exercises fully before touching AWS -
they're free, fast to repeat, and build the exact muscle memory the AWS exercise assumes you already have.
