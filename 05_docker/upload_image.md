# Uploading a Docker Image to Docker Hub

Docker Hub is a cloud-based registry where you can store and share Docker images. Follow the steps below to push a local image and pull it back from Docker Hub.

---

## Prerequisites

- Docker installed and running
- A [Docker Hub](https://hub.docker.com) account
- Your image must be tagged with your Docker Hub username (e.g., `username/image-name`)

---

## Step 1 — Log in to Docker Hub

```bash
docker login
```

Enter your Docker Hub **username** and **password** when prompted. On success you will see `Login Succeeded`.

---

## Step 2 — List your local images

```bash
docker images
```

This shows all images currently available on your machine.

---

## Step 3 — Build and tag the image

Build the image using the `username/repository-name` naming convention so Docker knows where to push it:

```bash
docker build -t cesar96/welcome-app .
```

> If the image already exists under a different name, you can retag it without rebuilding:
> ```bash
> docker tag welcome-app cesar96/welcome-app
> ```

---

## Step 4 — Push the image to Docker Hub

```bash
docker push cesar96/welcome-app:latest
```

- `:latest` is the default tag. Replace it with a specific version (e.g., `:v1.0`) to version your releases.
- The image will be publicly visible on your Docker Hub profile after the push completes.

---

## Step 5 — Pull the image from Docker Hub

To download the image on any machine:

```bash
docker pull cesar96/welcome-app:latest
```

---

## Step 6 — Run the image

```bash
docker run -d -p 5000:5000 cesar96/welcome-app
```

| Flag | Meaning |
|------|---------|
| `-d` | Run the container in detached (background) mode |
| `-p 5000:5000` | Map port 5000 on the host to port 5000 inside the container |

---

## Optional — Remove a local image

To free up disk space or force a clean rebuild:

```bash
docker image rm -f cesar96/welcome-app
```

---

## Quick Reference

| Task | Command |
|------|---------|
| Log in | `docker login` |
| Build & tag | `docker build -t username/image-name .` |
| Retag existing image | `docker tag old-name username/new-name` |
| Push to Docker Hub | `docker push username/image-name:tag` |
| Pull from Docker Hub | `docker pull username/image-name:tag` |
| Run container | `docker run -d -p host:container username/image-name` |
| Remove local image | `docker image rm -f image-name` |
