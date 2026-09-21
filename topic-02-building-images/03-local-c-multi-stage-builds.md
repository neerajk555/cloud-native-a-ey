# Topic 2 / AWS: Push Your Image to Amazon ECR

The same exercise as the local version, now done for real on AWS using your assigned IAM user credentials -
about 15 minutes. Estimated cost: ~$0.00-0.01 (a few MB of ECR storage for a few minutes), and only accurate if you complete the cleanup step below
right after finishing - don't leave this running "to come back to later." Prerequisites: Topic 2 Local A-C complete, AWS CLI configured with your issued credentials, VS Code open on ~/course/myapp.

Push a real image to a private ECR repository, using your IAM user's ECR access. Everything that gets pushed lives in ONE file - the Dockerfile - so this exercise starts in VS Code confirming that file is exactly right, then builds fresh from it and pushes the result. This is the same concept you just practiced locally - the goal here is seeing how it plays out
under real AWS constraints (IAM permissions, tagging requirements, shared-account limits), not relearning the
concept itself.

## Step 1: Open the one file that defines what you're pushing


Local C's own cleanup already removed the `myapp:multi` image (`docker rmi myapp:single myapp:multi`) - that's expected, not a problem to work around. The Dockerfile itself is still there, and that's all this exercise actually needs: everything you're about to push to AWS is fully defined by this one file.

In VS Code's Explorer panel, open the `~/course/myapp` folder (**File > Open Folder**, if it isn't open already), then click `Dockerfile`. Confirm it reads exactly like this (the multi-stage version from Local C - if yours looks different, replace the contents now and save with `Ctrl+S`):

```dockerfile
FROM node:20 AS build
WORKDIR /app
COPY index.js .

FROM node:20-slim
WORKDIR /app
COPY --from=build /app/index.js .
CMD ["node", "index.js"]
```

That's it - one file, fully self-contained. Every step from here just rebuilds this exact file and pushes the result; nothing depends on an image still existing from an earlier exercise.


## Step 2: Create your own ECR repo (name includes your username to avoid collisions)

```
aws ecr create-repository --repository-name myapp-$PARTICIPANT --region us-east-1 --tags Key=Owner,Value=$PARTICIPANT Key=Course,Value=cloudnative-course-2026
```


## Step 3: Authenticate Docker to ECR

```
ACCOUNT_ID=$(aws sts get-caller-identity --query Account --output text)
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin $ACCOUNT_ID.dkr.ecr.us-east-1.amazonaws.com
```

Run this from VS Code's integrated terminal (`` Ctrl+` ``) so you stay in the same window as the Dockerfile you just confirmed.


## Step 4: Rebuild the Dockerfile from Step 1 and push it

```
cd ~/course/myapp
docker build -t myapp:multi .
docker tag myapp:multi $ACCOUNT_ID.dkr.ecr.us-east-1.amazonaws.com/myapp-$PARTICIPANT:v1
docker push $ACCOUNT_ID.dkr.ecr.us-east-1.amazonaws.com/myapp-$PARTICIPANT:v1
```

`docker build .` reads the Dockerfile in the current directory - the exact file you just opened and confirmed in VS Code, rebuilt fresh since Local C's cleanup removed the old image.


## Step 5: Confirm it's there

```
aws ecr describe-images --repository-name myapp-$PARTICIPANT --region us-east-1
```


## Sanity checks

- describe-images shows one image with tag v1.

Common places beginners get stuck: Forgetting the region flag (ECR is region-scoped — always confirm you're pushing to us-east-1, matching your account's permission boundary.) Skipping the Owner/Course tags on the repo (The permission boundary requires these on several resource types — get in the habit now.) Assuming you need to rebuild the local image before this exercise 'because it's missing' (That's the expected state after Local C's cleanup, not a problem - Step 3 rebuilds it from the Dockerfile every time, on purpose.) Editing the Dockerfile after building (If you change the Dockerfile in VS Code after `docker build`, the image you push is still the OLD version until you rebuild - always rebuild after any edit, right before you push.)

## Cleanup (do this now, not later)

```
aws ecr delete-repository --repository-name myapp-$PARTICIPANT --region us-east-1 --force
```

Then complete the mandatory checklist, `05-cleanup-checklist.md`, before moving to the next topic.
