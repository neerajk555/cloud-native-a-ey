# Topic 17: Observability

You can't fix what you can't see. `docker logs`/`kubectl logs` are your first line of visibility; `docker stats`/`kubectl top` show resource usage. On AWS, container logs flow into CloudWatch Logs automatically, and CloudWatch Alarms can notify you when something crosses a threshold - the cloud-scale version of watching a terminal yourself.

**How this topic is organized:** Local exercise: a reference sweep of the logging/metrics commands you already have from earlier topics. AWS exercise: view the same app's logs in CloudWatch Logs, and set one basic alarm. Work through the local exercises fully before touching AWS -
they're free, fast to repeat, and build the exact muscle memory the AWS exercise assumes you already have.
