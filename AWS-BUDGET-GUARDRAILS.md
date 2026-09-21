# AWS Budget Guardrails — Read Before Topic 2

This is a **shared AWS training account**. Every participant's actions affect the same account-wide budget.
The rules below are not suggestions.

## The core rule: cleanup is mandatory, immediately, every time

Every topic with an AWS exercise ends with a **cleanup checklist file**. You are not done with that topic until
every box on that checklist is checked. Do this **immediately after finishing the exercise**, not "later" or
"at the end of the day." Resources left running are the #1 cause of training-account cost overruns — not the
exercises themselves.

## What you're allowed to do (and why it's restricted this way)

Your IAM user has:
- Full access to specific services (EC2, RDS, S3, Lambda, EKS, ALB/NLB, DynamoDB, Bedrock, API Gateway,
  CloudWatch/Logs), all restricted to **us-east-1 only**
- EC2 instances capped at `t2.micro`/`t3.micro`; RDS at `db.t3.micro`/`db.t4g.micro`, single-AZ only
- Bedrock limited to 3 small, low-cost models
- **No** `iam:CreateRole`, `iam:CreateUser`, or `iam:CreatePolicy` — you cannot create your own IAM roles; every
  exercise that needs one uses a pre-created **shared execution role**, fetched by name
- **No** ability to create an EKS cluster yourself — you connect to the one shared cluster your instructor
  provisions for the cohort (see Topic 6 and 7)
- `Owner` and `Course` tags **required** on EC2, RDS, EKS, and load-balancer creation — the AWS API will reject
  the request outright if these are missing

None of this is arbitrary — it's what keeps 20+ people sharing one account from colliding with each other or
running up unpredictable cost.

## The one topic where "nothing running" doesn't mean "nothing billing"

**Topic 12 (Storage)** provisions a real AWS EBS volume. Unlike everything else in this course, an EBS volume
keeps billing every month **even with zero Pods using it**, until the volume itself is explicitly deleted.
Deleting the Pod, or even the PVC object, is not always enough to guarantee the underlying volume is gone —
Topic 12's cleanup checklist has you explicitly verify this with `aws ec2 describe-volumes`. Do not skip that
verification step.

**Topic 9 (Ingress/ALB)** creates a real Application Load Balancer, which bills **hourly** regardless of
traffic. It's the single most expensive per-minute resource in this course. Delete it the moment you're done
testing it — don't leave it up "just in case you need it again later."

## If you're not sure something is cleaned up

Run this at any time, using your own `$PARTICIPANT` value, to see everything currently tagged to you across
the account's granted services:

```bash
aws resourcegroupstaggingapi get-resources --tag-filters Key=Owner,Values=$PARTICIPANT --region us-east-1
```

An empty result is what you want between topics. If it shows something you don't recognize or can't explain,
ask your instructor before assuming it's safe to ignore.

## What happens if cleanup is skipped

Cost accumulates against the shared account budget, other participants' quota headroom shrinks (EKS namespace
resource limits are finite and shared), and — depending on your instructor's policy — repeated skipped cleanup
may result in your IAM access being paused until it's addressed. This isn't punitive; it's the same discipline
real teams need when sharing cloud infrastructure.
