# Topic 03: Ports, Volumes, and Configuration

A container's network is isolated by default — `-p hostPort:containerPort` punches a hole so your host can reach it. Container filesystem changes vanish when the container is removed, unless you use a **volume** (Docker-managed storage) or a **bind mount** (a specific host folder). Environment variables let the same image behave differently per environment without rebuilding it.

**How this topic is organized:** Local exercises: port mapping, bind mounts vs. named volumes, environment variables. AWS exercise: take the same containerized app and run it as a Lambda function using environment variables for configuration — no networking/volume setup needed, since Lambda handles that differently, which is itself an important lesson in this topic. Work through the local exercises fully before touching AWS -
they're free, fast to repeat, and build the exact muscle memory the AWS exercise assumes you already have.
