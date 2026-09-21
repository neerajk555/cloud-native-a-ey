# Start Here

This course teaches Docker and Kubernetes by having you learn every concept **locally first, in depth, for
free**, then immediately transfer that exact skill to **real AWS infrastructure** using your assigned IAM user.
Nothing here assumes prior Docker or Kubernetes experience.

## How the course is organized

```
cloud-native-course/
├── 00-START-HERE.md                    This file
├── SETUP-UBUNTU-VSCODE.md              One-time machine setup - do this first
├── AWS-BUDGET-GUARDRAILS.md            Mandatory reading before Topic 2 (first AWS exercise)
├── INSTRUCTOR-CLUSTER-LIFECYCLE.md     Instructor-only - not needed if you're a participant
├── topic-01-containers-101/
│   ├── 00-concept.md                   Read this first - no commands
│   ├── 01-local-a-...md                Local exercises, in order
│   ├── 02-local-b-...md
│   ├── 03-local-c-...md
│   ├── 04-aws-...md                    The AWS transfer exercise (most topics have one)
│   └── 05-cleanup-checklist.md         Mandatory if there was an AWS exercise
├── topic-02-building-images/
│   └── ... same shape
...
└── topic-18-capstone/
    └── ... same shape
```

Every topic follows this exact shape. Once you've done Topic 1, you know exactly what Topic 2 will feel like.

## The rule that matters most: local before AWS, always

Do **every local exercise in a topic**, in order, before touching that topic's AWS exercise. The AWS exercise
is deliberately written assuming you already understand the concept — it focuses on what's DIFFERENT about
real AWS infrastructure (IAM permissions, tagging requirements, shared-account limits, cost), not on re-teaching
the concept itself.

## The rule that matters second most: cleanup is mandatory, not optional

Every topic with an AWS exercise ends with a **cleanup checklist** — a literal list of checkboxes, not a
suggestion. Do not start the next topic's AWS exercise until the current one's checklist is fully checked. This
is how a shared training account stays on budget. See `AWS-BUDGET-GUARDRAILS.md` for the full policy and what
happens if cleanup is skipped.

## Time budget

- Each local exercise: ~15 minutes (a few run longer where marked, like the capstone)
- Each AWS exercise: ~15 minutes, plus cleanup
- A topic with 3 local exercises + 1 AWS exercise: roughly 60-75 minutes total, doable in one sitting
- Full course: 18 topics — expect several weeks at a sustainable pace, not a weekend sprint

## What you need before starting

- An Ubuntu VM (yours or provided by the course)
- VS Code (recommended, not required) — see `SETUP-UBUNTU-VSCODE.md`
- Your AWS IAM user credentials, issued by your instructor, before Topic 2
- Nothing else — Topic 1 has zero prerequisites beyond a working Docker install

Start with `SETUP-UBUNTU-VSCODE.md`, then `AWS-BUDGET-GUARDRAILS.md`, then `topic-01-containers-101/00-concept.md`.
