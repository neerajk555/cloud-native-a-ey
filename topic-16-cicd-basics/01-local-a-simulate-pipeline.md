# Topic 16 / Local A: Simulating a Build-Test-Deploy Pipeline

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Topic 2 complete.

Write a simple script that mimics the stages a real CI/CD pipeline runs, so you understand the pattern independent of any specific tool.

## Step 1: Write a pipeline script

```
mkdir -p ~/course/pipeline-demo && cd ~/course/pipeline-demo
cat > pipeline.sh <<'EOF'
#!/bin/bash
set -e
echo "[SOURCE] Checking out code..."
sleep 1
echo "[BUILD] Building Docker image..."
docker build -t pipeline-demo -f - . <<'DOCKERFILE'
FROM alpine
CMD ["echo", "built successfully"]
DOCKERFILE
echo "[TEST] Running tests..."
docker run --rm pipeline-demo
echo "[DEPLOY] Would deploy here in a real pipeline"
echo "Pipeline complete."
EOF
chmod +x pipeline.sh
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `pipeline.sh`, paste this, and save (`Ctrl+S`):

```
#!/bin/bash
set -e
echo "[SOURCE] Checking out code..."
sleep 1
echo "[BUILD] Building Docker image..."
docker build -t pipeline-demo -f - . <<'DOCKERFILE'
FROM alpine
CMD ["echo", "built successfully"]
DOCKERFILE
echo "[TEST] Running tests..."
docker run --rm pipeline-demo
echo "[DEPLOY] Would deploy here in a real pipeline"
echo "Pipeline complete."
```


## Step 2: Run it

```
./pipeline.sh
```


## Step 3: Make it fail on purpose and see the pipeline stop

```
sed -i 's/CMD \["echo", "built successfully"\]/CMD \["false"\]/' pipeline.sh
./pipeline.sh || echo "Pipeline correctly stopped after TEST failed"
```


## Sanity checks

- The second run stops after the TEST stage fails, and DEPLOY never prints - proving each stage gates the next.

Common places beginners get stuck: Not using `set -e` in the script (Without it, bash keeps running subsequent commands even after one fails, which is the opposite of what a real pipeline should do.)

## Cleanup

```
docker rmi pipeline-demo 2>/dev/null || true
cd .. && rm -rf pipeline-demo
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.
