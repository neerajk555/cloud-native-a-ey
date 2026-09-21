# Topic 2 / Local B: Layer Caching and Rebuild Speed

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local A complete.

See layer caching in action, and understand why instruction order in a Dockerfile matters.

## Step 1: Add a dependency to make caching visible

```
cd ~/course/myapp
cat > package.json <<'EOF'
{"name": "myapp", "version": "1.0.0"}
EOF
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `package.json`, paste this, and save (`Ctrl+S`):

```
{"name": "myapp", "version": "1.0.0"}
```


## Step 2: Rewrite the Dockerfile with a slow step, in the SLOW order

```
cat > Dockerfile <<'EOF'
FROM node:20-slim
WORKDIR /app
COPY . .
RUN sleep 5
CMD ["node", "index.js"]
EOF
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `Dockerfile`, paste this, and save (`Ctrl+S`):

```
FROM node:20-slim
WORKDIR /app
COPY . .
RUN sleep 5
CMD ["node", "index.js"]
```


## Step 3: Build twice, timing each

```
time docker build -t myapp:slow1 .
# now change index.js slightly (any small edit)
echo '// comment' >> index.js
time docker build -t myapp:slow1 .
```

The second build re-runs `sleep 5` too, because `COPY . .` came BEFORE it, so any source change invalidates everything after.


## Step 4: Now fix the order

```
cat > Dockerfile <<'EOF'
FROM node:20-slim
WORKDIR /app
COPY package.json .
RUN sleep 5
COPY . .
CMD ["node", "index.js"]
EOF
```


**Using VS Code instead:** rather than the heredoc above, you can do this directly in the editor.

In VS Code's Explorer, create a new file named `Dockerfile`, paste this, and save (`Ctrl+S`):

```
FROM node:20-slim
WORKDIR /app
COPY package.json .
RUN sleep 5
COPY . .
CMD ["node", "index.js"]
```


## Step 5: Build twice again

```
time docker build -t myapp:slow2 .
echo '// another comment' >> index.js
time docker build -t myapp:slow2 .
```

This time the second build skips the `sleep 5` layer entirely, since only package.json (unchanged) affects it.


## Sanity checks

- The second build in the fixed-order version is noticeably faster than the second build in the slow-order version.

Common places beginners get stuck: Putting COPY . . before slow/expensive steps (Any source change then invalidates the cache for everything after it — always copy dependency manifests first, install/build, THEN copy the rest of the source.)

## Cleanup

```
docker rmi myapp:v1 myapp:slow1 myapp:slow2
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.
