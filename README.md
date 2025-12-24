# NodeGoat - DevOps CI/CD Project

This Project demonstrates a complete **DevOps CI/CD pipeline** using Jenkins, Docker, and Docker Compose on a Linux VM.

The application used is **OWASP NodeGoat**, a deliberately vulnerable Node.js application, deployed as a multi-container setup.

---

## Tech Stack
- Jenkins (Declarative Pipeline)
- Docker & Docker Compose
- Node.js (NodeGoat)
- MongoDB
- Linux (Ubuntu VM)
- GitHub

---

## Architecture Overview

Developer -> GitHub -> Jenkins Pipeline -> Docker Build -> Docker Compose Deploy -> Application

- Jenkins runs as a controller
- A separate Jenkins agent VM is used for Docker operations
- Application and database run as seperate containers

---

## CI/CD Pipeline Flow

1. Developer pushe code to GitHub
2. Jenkins Pipeline is triggered
3. Jenkins checks out code on Docker-enabled agent
4. Docker image is built
5. Existing containers are stopped safely
6. Application is deployed using Docker Compose
7. Deployment is verified via a health check

---

## Jenkinsfile Highlights

- Uses a dedicated Docker-enabled Jenkins agent
- Idempotent deployment ('docker compose down || true')
- Clear logging for each stage
- Non-blocking health check to avoid flaky builds

---

## How to Run Locally

```bash
docker compose up -d

