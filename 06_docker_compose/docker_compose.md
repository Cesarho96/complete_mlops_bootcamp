# Docker Compose

Docker Compose is a tool for defining and running **multi-container applications**. Instead of starting each container manually, you describe all services in a single `docker-compose.yml` file and manage them together with simple commands.

---

## Why Docker Compose?

| Without Compose | With Compose |
|-----------------|-------------|
| Run `docker run` for every service | One file defines all services |
| Manage networks and volumes manually | Networking is automatic between services |
| Hard to reproduce the environment | `docker-compose.yml` is version-controlled |

---

## The `docker-compose.yml` File

This is the core of Docker Compose. Each entry under `services` becomes an independent container.

```yaml
version: "3.0"
services:
  web:
    image: web-app       # name of the image
    build: .             # build from the Dockerfile in the current directory
    ports:
      - "8000:5000"      # host_port:container_port

  redis:
    image: redis         # pull the official Redis image from Docker Hub
```

### Key fields explained

| Field | Description |
|-------|-------------|
| `version` | Compose file format version |
| `services` | List of containers to run |
| `image` | Docker image to use (built locally or pulled from Docker Hub) |
| `build` | Path to the `Dockerfile` used to build the image |
| `ports` | Port mapping — `host:container` |

> All services defined in the same `docker-compose.yml` share an internal network automatically. In this example, the `web` container can reach the Redis container simply by using the hostname `redis`.

---

## Project Structure

This example runs a Flask web app backed by Redis:

```
06_docker_compose/
├── app.py               # Flask application
├── Dockerfile           # Instructions to build the web image
├── requirements.txt     # Python dependencies
└── docker-compose.yml   # Service definitions
```

### `app.py` — Flask app with Redis hit counter

```python
import time
import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)

@app.route('/')
def hello():
    count = get_hit_count()
    return f'Hello Docker! This page has been visited {count} times.'
```

The app connects to Redis using the service name `redis` as the hostname — Docker Compose resolves it automatically on the shared internal network.

### `Dockerfile` — Web service image

```dockerfile
FROM python:3.7-alpine
WORKDIR /code
ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0
COPY . .
RUN pip install -r requirements.txt
EXPOSE 5000
CMD ["flask", "run"]
```

---

## Common Commands

### Start all services

Builds images if needed and starts all containers in the foreground:

```bash
docker compose up
```

Run in **detached mode** (background) with the `-d` flag:

```bash
docker compose up -d
```

Force a rebuild of images before starting:

```bash
docker compose up --build
```

---

### Stop all services

Stops running containers without removing them:

```bash
docker compose stop
```

Stop **and remove** containers, networks, and volumes created by `up`:

```bash
docker compose down
```

---

### Other useful commands

| Command | Description |
|---------|-------------|
| `docker compose ps` | List running services and their status |
| `docker compose logs` | Show logs from all services |
| `docker compose logs web` | Show logs from a specific service |
| `docker compose build` | Build (or rebuild) images without starting containers |
| `docker compose exec web sh` | Open a shell inside the `web` container |
| `docker compose restart` | Restart all services |

---

## Quick Reference

```bash
docker compose up -d        # Start all services in background
docker compose stop         # Stop all services
docker compose down         # Stop and remove containers + networks
docker compose ps           # Check status of services
docker compose logs -f      # Follow live logs from all services
```
