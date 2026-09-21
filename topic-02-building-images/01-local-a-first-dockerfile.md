# Topic 2 / Local A: Writing Your First Dockerfile

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Topic 1 complete.

Write, build, and run a Dockerfile for a tiny app from scratch.

## Step 1: Create a tiny app

```
mkdir -p ~/course/myapp && cd ~/course/myapp
cat > index.js <<'EOF'
require('http').createServer((req, res) => res.end('Hello from my container!')).listen(3000);
EOF
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `index.js`, paste this, and save (`Ctrl+S`):

```
require('http').createServer((req, res) => res.end('Hello from my container!')).listen(3000);
```


## Step 2: Write the Dockerfile

```
cat > Dockerfile <<'EOF'
FROM node:20-slim
WORKDIR /app
COPY index.js .
EXPOSE 3000
CMD ["node", "index.js"]
EOF
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `Dockerfile`, paste this, and save (`Ctrl+S`):

```
FROM node:20-slim
WORKDIR /app
COPY index.js .
EXPOSE 3000
CMD ["node", "index.js"]
```


## Step 3: Build it

```
docker build -t myapp:v1 .
```


## Step 4: Run it

```
docker run -d --name myapp-container -p 3000:3000 myapp:v1
```


## Step 5: Test it

```
curl http://localhost:3000
```


## Sanity checks

- curl returns 'Hello from my container!'

Common places beginners get stuck: Forgetting to rebuild after changing source code (Docker doesn't auto-detect changes — rerun `docker build` explicitly every time.)

## Cleanup

```
docker rm -f myapp-container
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.
