# Topic 16 / AWS: Run a Real Build with AWS CodeBuild

The same exercise as the local version, now done for real on AWS using your assigned IAM user credentials -
about 15 minutes. Estimated cost: ~$0.01-0.02 (CodeBuild bills per build-minute; a small build is fractions of a cent), and only accurate if you complete the cleanup step below
right after finishing - don't leave this running "to come back to later." Prerequisites: Topic 16 Local A complete.

Run the BUILD stage from Local A for real, using AWS CodeBuild and your shared execution role. This is the same concept you just practiced locally - the goal here is seeing how it plays out
under real AWS constraints (IAM permissions, tagging requirements, shared-account limits), not relearning the
concept itself.

## Step 1: Fetch your shared CodeBuild role

```
ROLE_ARN=$(aws iam get-role --role-name course-codebuild-execution --query 'Role.Arn' --output text)
```


## Step 2: Create a minimal CodeBuild project (no source repo needed for this exercise)

```
aws codebuild create-project --name build-$PARTICIPANT \
  --source type=NO_SOURCE,buildspec='version: 0.2\nphases:\n  build:\n    commands:\n      - echo Build stage running on CodeBuild' \
  --artifacts type=NO_ARTIFACTS \
  --environment type=LINUX_CONTAINER,image=aws/codebuild/standard:7.0,computeType=BUILD_GENERAL1_SMALL \
  --service-role $ROLE_ARN \
  --tags key=Owner,value=$PARTICIPANT key=Course,value=cloudnative-course-2026 \
  --region us-east-1
```


## Step 3: Start a build

```
BUILD_ID=$(aws codebuild start-build --project-name build-$PARTICIPANT --region us-east-1 --query 'build.id' --output text)
```


## Step 4: Watch it complete

```
aws codebuild batch-get-builds --ids $BUILD_ID --region us-east-1 --query 'builds[0].buildStatus'
```

Re-run this until it shows SUCCEEDED (usually under a minute).


## Sanity checks

- buildStatus shows SUCCEEDED.

Common places beginners get stuck: Trying to pass your own IAM role instead of the shared one (You don't have `iam:CreateRole` - always fetch `course-codebuild-execution` by name, exactly as shown.) Leaving the CodeBuild project around (It doesn't bill when idle, but delete it anyway to keep the account tidy for the next topic/cohort.)

## Cleanup (do this now, not later)

```
aws codebuild delete-project --name build-$PARTICIPANT --region us-east-1
```

Then complete the mandatory checklist, `03-cleanup-checklist.md`, before moving to the next topic.
