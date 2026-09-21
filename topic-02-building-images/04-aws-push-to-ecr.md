# Topic 2 / AWS: Push Your Image to Amazon ECR

The same exercise as the local version, now done for real on AWS using your assigned IAM user credentials -
about 15 minutes. Estimated cost: ~$0.00-0.01 (a few MB of ECR storage for a few minutes), and only accurate if you complete the cleanup step below
right after finishing - don't leave this running "to come back to later." Prerequisites: Topic 2 Local A-C complete, AWS CLI configured with your issued credentials.

Push the exact image you built locally in Local C to a private ECR repository, using your IAM user's ECR access. This is the same concept you just practiced locally - the goal here is seeing how it plays out
under real AWS constraints (IAM permissions, tagging requirements, shared-account limits), not relearning the
concept itself.

## Step 1: Create your own ECR repo (name includes your username to avoid collisions)

```
aws ecr create-repository --repository-name myapp-$PARTICIPANT --region us-east-1 --tags Key=Owner,Value=$PARTICIPANT Key=Course,Value=cloudnative-course-2026
```


## Step 2: Authenticate Docker to ECR

```
ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin $ACCOUNT_ID.dkr.ecr.us-east-1.amazonaws.com
```


## Step 3: Tag and push your multi-stage image from Local C

```
cd ~/course/myapp
docker build -t myapp:multi .
docker tag myapp:multi $ACCOUNT_ID.dkr.ecr.us-east-1.amazonaws.com/myapp-$PARTICIPANT:v1
docker push $ACCOUNT_ID.dkr.ecr.us-east-1.amazonaws.com/myapp-$PARTICIPANT:v1
```


## Step 4: Confirm it's there

```
aws ecr describe-images --repository-name myapp-$PARTICIPANT --region us-east-1
```


## Sanity checks

- describe-images shows one image with tag v1.

Common places beginners get stuck: Forgetting the region flag (ECR is region-scoped — always confirm you're pushing to us-east-1, matching your account's permission boundary.) Skipping the Owner/Course tags on the repo (The permission boundary requires these on several resource types — get in the habit now.)

## Cleanup (do this now, not later)

```
aws ecr delete-repository --repository-name myapp-$PARTICIPANT --region us-east-1 --force
```

Then complete the mandatory checklist, `05-cleanup-checklist.md`, before moving to the next topic.
