# CC_Lab-6

A containerized backend deployment project focused on Docker networking, load balancing, and CI/CD automation using Jenkins.

## Overview

This repository demonstrates a simple distributed web service built with multiple backend containers behind an NGINX load balancer. The goal is to show how a backend service can be packaged in Docker, scaled horizontally, and exposed through a reverse proxy.

## Architecture

The project includes:

- a C++ backend service running on port 8080
- two backend containers (`backend1` and `backend2`)
- an NGINX reverse proxy configured to distribute requests
- a Jenkins pipeline that builds the Docker image and deploys the containers

### Core components

- `CC_LAB-6/backend/app.cpp`  
  A minimal HTTP server that responds with the hostname of the backend instance.

- `CC_LAB-6/backend/Dockerfile`  
  Builds the backend container image.

- `CC_LAB-6/nginx/default.conf`  
  Configures NGINX upstream load balancing with `ip_hash` routing to two backend servers.

- `CC_LAB-6/Jenkinsfile`  
  Automates the build and deployment pipeline using Docker commands.

## How it works

The Jenkins pipeline performs the following steps:

1. Builds the backend Docker image
2. Creates a Docker network
3. Starts two backend containers on the same network
4. Starts an NGINX container and loads the proxy configuration
5. Routes traffic to the backend instances through NGINX

This setup is a practical example of:

- containerization
- service scaling
- reverse proxying
- Docker networking
- automated deployment with CI/CD

## Project structure

```text
CC_Lab-6/
├── CC_LAB-6/
│   ├── backend/
│   │   ├── Dockerfile
│   │   └── app.cpp
│   ├── nginx/
│   │   └── default.conf
│   ├── Dockerfile.jenkins
│   └── Jenkinsfile
└── README.md
```

## Prerequisites

- Docker
- Jenkins (for automation pipeline execution)
- Linux/macOS environment or a Docker-capable host

## Local run

Build the backend image:

```bash
docker build -t backend-app CC_LAB-6/backend
```

Create a network:

```bash
docker network create app-network
```

Run backend containers:

```bash
docker run -d --name backend1 --hostname backend1 --network app-network backend-app
docker run -d --name backend2 --hostname backend2 --network app-network backend-app
```

Run NGINX and load the config:

```bash
docker run -d --name nginx-lb --network app-network -p 80:80 nginx
docker cp CC_LAB-6/nginx/default.conf nginx-lb:/etc/nginx/conf.d/default.conf
docker exec nginx-lb nginx -s reload
```

Then open:

```bash
http://localhost
```

## Notes

This project is intended as a lab/demo for understanding how deployment orchestration and load balancing operate in a Docker-based environment. It is lightweight and easy to run locally for experimentation.

## Summary

`CC_Lab-6` showcases the basics of deploying multiple backend instances behind a load balancer, with Jenkins automating the deployment flow. It is an excellent hands-on example of Docker networking and CI/CD-based container deployment.
