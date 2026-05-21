# Docker Image — Build, Run & Manage

## 1. Build an image

```bash
docker build -t <image-name> .
```

> The trailing `.` tells Docker to use the `Dockerfile` in the current directory — don't omit it.

Example:

```bash
docker build -t welcome-app .
```

---

## 2. Verify the image was created

```bash
docker images
```

---

## 3. Run the image as a container

```bash
docker run -p <host-port>:<container-port> <image-name>
```

Example (maps port 5000 on the host to port 5000 inside the container):

```bash
docker run -p 5000:5000 welcome-app
```

Useful flags:

| Flag | Description |
|------|-------------|
| `-d` | Run in detached mode (background) |
| `--name <name>` | Assign a custom name to the container |
| `--rm` | Automatically remove the container when it stops |

Example with flags:

```bash
docker run -d --name my-app -p 5000:5000 welcome-app
```

---

## 4. List running containers

```bash
docker ps
```

Add `-a` to also see stopped containers:

```bash
docker ps -a
```

---

## 5. Stop a container

```bash
docker stop <container-id or container-name>
```

Example:

```bash
docker stop 94266a2c9683
```

To kill a container immediately (without a graceful shutdown):

```bash
docker kill 94266a2c9683
```

---

## 6. Remove a container

```bash
docker rm <container-id or container-name>
```

To force-remove a running container in one step:

```bash
docker rm -f 94266a2c9683
```

---

## 7. Remove an image

```bash
docker rmi <image-name or image-id>
```
