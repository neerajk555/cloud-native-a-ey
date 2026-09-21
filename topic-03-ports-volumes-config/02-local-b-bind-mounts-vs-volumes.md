# Topic 3 / Local B: Bind Mounts vs. Named Volumes

A beginner-friendly, hands-on walkthrough for this topic, done entirely on your own Ubuntu VM - about 15
minutes, using VS Code and a terminal. Prerequisites: Local A complete.

Use both mount types and understand when to reach for which.

## Step 1: Create a host folder with a file

```
mkdir -p ~/course/bind-demo
echo 'hello from host' > ~/course/bind-demo/index.html
```


## Step 2: Bind-mount it into nginx

```
docker run -d --name bind-test -p 8082:80 -v ~/course/bind-demo:/usr/share/nginx/html nginx
```


## Step 3: Confirm your file is served, then edit it live

```
curl http://localhost:8082
echo 'edited!' > ~/course/bind-demo/index.html
curl http://localhost:8082
```

No restart needed — bind mounts reflect host changes immediately.


## Step 4: Now use a named volume instead

```
docker volume create my-vol
docker run -d --name vol-test -v my-vol:/data alpine sleep 300
docker exec vol-test sh -c 'echo persisted > /data/file.txt'
```


## Step 5: Prove the volume survives container removal

```
docker rm -f vol-test
docker run --rm -v my-vol:/data alpine cat /data/file.txt
```


## Sanity checks

- The file.txt content prints even though the original container is gone.

Common places beginners get stuck: Reversing the -v syntax order (It's `-v host_or_volume:container_path`, same order as ports.) Forgetting named volumes outlive containers (This is the whole point — but also means you must explicitly `docker volume rm` to actually delete the data.)

## Cleanup

```
docker rm -f bind-test
docker volume rm my-vol
rm -rf ~/course/bind-demo
```

Nothing here touches AWS, so there's no cost to worry about - just tidy up your local Docker/Kubernetes state
before moving on.
