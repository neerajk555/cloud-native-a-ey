# Topic 2 / Local C: Multi-Stage Builds

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local B complete.

Shrink a final image by separating build-time tools from the runtime image.

## Step 1: Build a single-stage version and check its size

```
cd ~/course/myapp
cat > Dockerfile <<'EOF'
FROM node:20
WORKDIR /app
COPY index.js .
CMD ["node", "index.js"]
EOF
docker build -t myapp:single .
docker images myapp:single
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `Dockerfile`, paste this, and save (`Ctrl+S`):

```
FROM node:20
WORKDIR /app
COPY index.js .
CMD ["node", "index.js"]
```


## Step 2: Rewrite as multi-stage

```
cat > Dockerfile <<'EOF'
FROM node:20 AS build
WORKDIR /app
COPY index.js .

FROM node:20-slim
WORKDIR /app
COPY --from=build /app/index.js .
CMD ["node", "index.js"]
EOF
docker build -t myapp:multi .
docker images myapp:multi
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `Dockerfile`, paste this, and save (`Ctrl+S`):

```
FROM node:20 AS build
WORKDIR /app
COPY index.js .

FROM node:20-slim
WORKDIR /app
COPY --from=build /app/index.js .
CMD ["node", "index.js"]
```


## Step 3: Compare

```
docker images | grep myapp
```


## Sanity checks

- myapp:multi is noticeably smaller than myapp:single.

Common places beginners get stuck: Forgetting `--from=build` (Without it, COPY pulls from your host machine, not the earlier build stage — you'd get a file-not-found error.)

## Cleanup

```
docker rmi myapp:single myapp:multi
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.
