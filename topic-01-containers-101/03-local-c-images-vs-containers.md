# Topic 1 / Local C: Proving Images and Containers Are Independent

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local A and B complete.

See with your own eyes that containers from the same image are completely independent of each other and of the image.

## Step 1: Create two containers from the same image

```
docker run -d --name c1 ubuntu sleep 300
docker run -d --name c2 ubuntu sleep 300
```


## Step 2: Change a file inside only c1

```
docker exec c1 sh -c 'echo hello > /tmp/note.txt'
```


## Step 3: Confirm c2 doesn't have it

```
docker exec c2 ls /tmp/
```

You should NOT see note.txt.


## Step 4: Confirm a brand-new container from the same image doesn't have it either

```
docker run --rm ubuntu ls /tmp/
```


## Sanity checks

- Only c1 shows note.txt — c2 and any new container from `ubuntu` don't, proving the image itself is untouched.

Common places beginners get stuck: Assuming containers from the same image share state (They never do by default — each gets its own independent writable layer on top of the shared read-only image.)

## Cleanup

```
docker rm -f c1 c2
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.
