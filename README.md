# Dockerized Multi-Service Project

## Overview
This project demonstrates a Dockerized multi-service architecture with:
- A Go (Golang) HTTP API
- A Python Flask API (using uv)
- An Nginx reverse proxy for unified access and routing

All services are containerized and orchestrated using Docker Compose, making it easy to build, run, and scale the stack.

---

## Architecture
- **service_1**: Go HTTP API (listens on port 8001)
- **service_2**: Python Flask API (listens on port 8002, managed by uv)
- **nginx**: Reverse proxy, listens on port 8080 and routes requests to the appropriate backend service

Nginx strips the `/service1` and `/service2` prefixes and forwards requests to the correct backend endpoints.

---

## Prerequisites
- [Docker](https://www.docker.com/products/docker-desktop) (Desktop or Engine)
- [Docker Compose](https://docs.docker.com/compose/)

---

## Project Structure
```
.
├── docker-compose.yml
├── nginx
│   ├── nginx.conf
│   └── Dockerfile
├── service_1
│   ├── main.go
│   └── Dockerfile
├── service_2
│   ├── app.py
│   ├── pyproject.toml
│   ├── uv.lock
│   ├── Dockerfile
│   └── .dockerignore
└── README.md
```

---

## Running the Project
1. **Clone the repository** (if not already):
   ```bash
   git clone <repo-url>
   cd <repo-directory>
   ```
2. **Build and start all services:**
   ```bash
   docker-compose up --build
   ```
   This will build all images and start the containers.

3. **Access the APIs via Nginx reverse proxy:**
   - Go service: [http://localhost:8080/service1/ping](http://localhost:8080/service1/ping)
   - Python service: [http://localhost:8080/service2/ping](http://localhost:8080/service2/ping)

4. **Stop the services:**
   Press `CTRL+C` in the terminal, or run:
   ```bash
   docker-compose down
   ```

---

## Endpoints
### Go Service (service_1)
- `/ping` → `{ "status": "ok", "service": "1" }`
- `/hello` → `{ "message": "Hello from Service 1" }`

### Python Service (service_2)
- `/ping` → `{ "status": "ok", "service": "2" }`
- `/hello` → `{ "message": "Hello from Service 2" }`

**Access via Nginx:**
- `/service1/ping`, `/service1/hello` → Go service
- `/service2/ping`, `/service2/hello` → Python service

---

## Nginx Reverse Proxy
- Listens on port 8080
- Routes `/service1/*` to the Go service (strips `/service1` prefix)
- Routes `/service2/*` to the Python service (strips `/service2` prefix)
- Logs requests with timestamp and path

---

## Troubleshooting
- **Docker image pull/authentication issues:**
  - Make sure you are logged in to Docker Hub: `docker login`
  - Ensure a stable internet connection
- **Port conflicts:**
  - Make sure ports 8080, 8001, and 8002 are not in use by other applications
- **API not responding:**
  - Check container logs: `docker logs <container_name>`
  - Ensure Nginx is running and correctly configured

---

## License
This project is for educational and demonstration purposes.
