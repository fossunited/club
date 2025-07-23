## Docker Playbook

### Prerequisites
- Install Docker
- Verify Docker is running correctly
- docker --version
- docker run hello-world
- For Podman users
- podman --version
- podman run hello-world

### Beginner Session Outline
- Understanding Docker Fundamentals
- What are containers vs virtual machines
- Virtual Machines (VMs)
- Run a complete operating system including the kernel
- Each VM requires its own OS installation (several GBs)
- Hardware-level virtualization through hypervisors (VMware, VirtualBox, Hyper-V)
- More resource intensive (CPU, memory, storage)
- Containers
- Share the host OS kernel
- Only contain application and its dependencies (MBs to few GBs)
- OS-level virtualization through container runtime

### When to Use What?
- Use VMs when: You need complete OS isolation, running different OS types, or strong security boundaries.
- Use Containers when: You need fast deployment, microservices, consistent environments, or efficient resource usage.
- Docker architecture: Images, Containers, Registry
- Docker Images
- Read-only templates containing application code, runtime, libraries, and dependencies
- Built in layers - each instruction in Dockerfile creates a layer
- Immutable - once built, they don't change
- Can be shared and reused
- Identified by repository:tag (e.g., nginx:latest, python:3.9-slim)

### Docker Containers
- Running instances of Docker images
- Add a writable layer on top of the image layers
- Isolated processes running on the host
- Can be started, stopped, moved, and deleted
- Each container gets its own filesystem, networking, and isolated process tree
- Docker Registry
- Storage and distribution system for Docker images
- Docker Hub: Default public registry (hub.docker.com)
- Private registries: Harbor, GitLab Container Registry, AWS ECR, etc.
- Images are pushed to and pulled from registries
- The Docker Daemon and Client
- Docker Daemon (dockerd)
- Background service running on the host
- Manages Docker objects (images, containers, networks, volumes)
- Listens for Docker API requests
- Handles container lifecycle operations
- Can communicate with other Docker daemons
- Docker Client (docker)
- Command-line interface (CLI) tool
- How users interact with Docker
- Sends commands to Docker daemon via REST API
- Can communicate with local or remote daemons
- Docker Engine API
- RESTful API that Docker daemon exposes
- Used by Docker CLI and other tools
- Enables programmatic control of Docker
- Understanding Container Lifecycle
- Pull or build an image
- Create and run a container from the image
- Stop, start, or restart containers as needed
- Remove containers and images when done

### Advanced Session
- Creating Your First Docker Image
- Understanding Dockerfiles
- Before creating an image, you need a Dockerfile. A Dockerfile is a text file containing instructions to build a Docker image.
- Create a Simple Dockerfile
- Create a file named 'Dockerfile' (no extension)
- FROM ubuntu
- CMD ["echo", "Hello world"]
- Build an Image from Dockerfile
- Build image from Dockerfile in current directory

- docker build -f path-to-dockerfile -t image-name:tag .

### Note:
The period (.) at the end is the build context - the directory containing files needed for the build.

If Dockerfile is in the current directory: docker build -t my-hello:v1 .

- Run Your Custom Image
  First, check the images on your machine
- docker images
- Run your custom image
- docker run my-hello:v1
- Push Image to Docker Hub

### Docker Networking Basics
- docker network ls
- docker network create myapp-network
- docker run -d --name web --network myapp-network nginx
- docker run -d --name api --network myapp-network myapi:latest

### Volume Management
- Create a named volume
    docker volume create mydata

- Run container with volume
    docker run -d -v mydata:/data nginx

- Bind mount from host
    docker run -d -v $(pwd)/data:/app/data myapp:latest

### Introduction to Docker Compose
Create a docker-compose.yml file:
version: '3.8'
services:
  web:
    image: nginx
    ports:
      - "80:80"
    volumes:
      - ./html:/usr/share/nginx/html
  
  app:
    build: .
    ports:
      - "5000:5000"
    environment:
      - DEBUG=true
    depends_on:
      - db
  
  db:
    image: postgres:13
    environment:
      POSTGRES_PASSWORD: secretpassword
    volumes:
      - db-data:/var/lib/postgresql/data

volumes:
  db-data:

- Start all services
docker-compose up -d

- View logs
docker-compose logs -f

- Stop all services
docker-compose down


### Expert Session

- Dockerizing a Real Application

Best Practices for Production Images

1. Use specific base image versions (not latest)
2. Multi-stage builds for smaller images

Dockerfile: 
- Build stage
FROM node:16-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

- Runtime stage
FROM node:16-alpine
WORKDIR /app
COPY --from=builder /app/node_modules ./node_modules
COPY . .
EXPOSE 3000
USER node
CMD ["node", "server.js"]

3. Run as non-root user:
RUN addgroup -g 1001 -S nodejs
RUN adduser -S nodejs -u 1001
USER nodejs

4. Use .dockerignore to exclude unnecessary files:
node_modules
npm-debug.log
.git
.env
.DS_Store
*.md
.gitignore
Security Scanning
- Scan image for vulnerabilities
docker scan myapp:latest

- Use minimal base images
FROM alpine:3.16
- or distroless images
FROM gcr.io/distroless/nodejs
Container Orchestration with Docker Swarm
- Initialize swarm
docker swarm init

- Deploy a stack
docker stack deploy -c docker-compose.yml myapp

- Scale services
docker service scale myapp_web=3

- Update service with zero downtime
docker service update --image myapp:v2 myapp_web

### Superhuman Session
Performance Optimization
Resource Management
- Set CPU and memory limits
docker run -d \
  --cpus="1.5" \
  --memory="1g" \
  --memory-reservation="750m" \
  nginx

- Monitor resource usage
docker stats

- System cleanup
docker system prune -a
docker volume prune
Build Optimization
dockerfile# Leverage build cache by ordering commands correctly
FROM node:16-alpine

- Dependencies change less frequently
COPY package*.json ./
RUN npm ci

- Application code changes frequently
COPY . .
Advanced Volume Management

Volume Types and Use Cases
1. Named Volumes: Best for persistent data
docker volume create postgres-data
docker run -v postgres-data:/var/lib/postgresql/data postgres

2. Bind Mounts: Best for development
docker run -v $(pwd)/src:/app/src myapp

3. tmpfs Mounts: Best for sensitive temporary data
docker run --tmpfs /tmp:rw,noexec,nosuid,size=100m myapp
Volume Drivers and Plugins
- Use different volume drivers
docker volume create --driver local \
  --opt type=nfs \
  --opt o=addr=nfs-server,rw \
  --opt device=:/path/to/dir \
  nfs-volume
Docker Internals
Namespaces: Process, Network, Mount, UTS, IPC, User
Control Groups (cgroups): Resource limiting and accounting
Union File Systems: OverlayFS, AUFS
Container Runtime: containerd, runc
Custom Registry Setup
- Run local registry
docker run -d -p 5000:5000 \
  --restart=always \
  --name registry \
  -v registry-data:/var/lib/registry \
  registry:2

- Push to local registry
docker tag myapp:latest localhost:5000/myapp:latest
docker push localhost:5000/myapp:latest

### Hands-on Exercises
- Beginner Exercise
Pull and run three different images (nginx, redis, ubuntu)
List all containers and images
Stop and remove all containers
Remove at least one image

- Advanced Exercise
Create a simple Python Flask application
Write a Dockerfile for it
Build and run the image
Push it to Docker Hub

- Expert Exercise
Create a multi-container application with Docker Compose (web app + database)
Implement health checks
Use environment-specific configurations
Set up logging and monitoring

### Superhuman Exercise

Optimize a Dockerfile to reduce image size by 50%
Implement a blue-green deployment strategy
Set up a private registry with TLS
Create a custom volume plugin


**Credits : Udhayaprakasha V / D3 Community**

### References
https://docs.docker.com/get-started/
https://docs.docker.com/develop/dev-best-practices
https://docs.docker.com/docker-hub/repos
https://medium.com/p/8798892a0e88
https://github.com/hadolint/hadolint
https://github.com/docker-slim/docker-slim
https://github.com/aquasecurity/trivy
https://github.com/wagoodman/dive
https://docs.docker.com/compose
https://docs.docker.com/develop/security-best-practices/
https://labs.play-with-docker.com/
https://podman.io/docs
https://cheatsheetseries.owasp.org/cheatsheets/Docker_Security_Cheat_Sheet.html
https://docs.docker.com/engine/storage/volumes/
https://docs.docker.com/compose/
