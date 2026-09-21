# Topic 3 / AWS: Run the Same App as a Lambda Function

The same exercise as the local version, now done for real on AWS using your assigned IAM user credentials -
about 15 minutes. Estimated cost: ~$0.00 (Lambda free tier covers this easily), and only accurate if you complete the cleanup step below
right after finishing - don't leave this running "to come back to later." Prerequisites: Topic 3 Local A-C complete, Topic 2's ECR push complete, or willing to zip a plain handler instead.

Deploy the same message-printing app as an AWS Lambda function, configured via environment variables just like Local C — but notice Lambda has no port mapping or volumes at all, which is an important architectural difference to understand. This is the same concept you just practiced locally - the goal here is seeing how it plays out
under real AWS constraints (IAM permissions, tagging requirements, shared-account limits), not relearning the
concept itself.

## Step 1: Fetch your shared Lambda execution role

```
ROLE_ARN=$(aws iam get-role --role-name course-lambda-basic-execution --query 'Role.Arn' --output text)
```


## Step 2: Write a Lambda-compatible handler (different entry point than a web server)

```
mkdir -p ~/course/lambda-demo && cd ~/course/lambda-demo
cat > index.js <<'EOF'
exports.handler = async () => {
  const message = process.env.MESSAGE || 'default message';
  return { statusCode: 200, body: message };
};
EOF
zip fn.zip index.js
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `index.js`, paste this, and save (`Ctrl+S`):

```
exports.handler = async () => {
  const message = process.env.MESSAGE || 'default message';
  return { statusCode: 200, body: message };
};
```


## Step 3: Create the function with your env var set

```
aws lambda create-function --function-name msg-$PARTICIPANT --runtime nodejs20.x --handler index.handler --role $ROLE_ARN --zip-file fileb://fn.zip --environment 'Variables={MESSAGE=hello from Lambda env var}' --tags Owner=$PARTICIPANT,Course=cloudnative-course-2026 --region us-east-1
```


## Step 4: Invoke it

```
aws lambda invoke --function-name msg-$PARTICIPANT --region us-east-1 out.json
cat out.json
```


## Sanity checks

- out.json contains your custom message in the body field.

Common places beginners get stuck: Expecting to use -p port mapping with Lambda (Lambda doesn't work that way at all — there's no persistent container listening on a port; AWS invokes your handler function per-request. This is the key conceptual difference this exercise is meant to surface.) Trying to pass your own IAM role instead of the shared one (You don't have `iam:CreateRole` — you must use `course-lambda-basic-execution`, fetched by name as shown above.)

## Cleanup (do this now, not later)

```
aws lambda delete-function --function-name msg-$PARTICIPANT --region us-east-1
```

Then complete the mandatory checklist, `06-cleanup-checklist.md`, before moving to the next topic.
