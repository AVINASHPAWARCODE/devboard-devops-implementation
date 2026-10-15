# DevBoard — DevOps Implementation

A hands-on DevOps project focused on containerization, production frontend serving, container networking, dependency maintenance, and Docker image security.

This project extends the existing DevBoard application with my own DevOps configuration and improvements. The original application was created by [LondheShubham153](https://github.com/LondheShubham153/devboard-starter).

## Project Overview

DevBoard is a web application with a React frontend, a Go backend API, and a PostgreSQL database. My focus is on improving how the application is built, connected, run, and verified using Docker.

### Architecture

```text
                 Browser
                    |
                    v
             Nginx Frontend
              Port 4173
                    |
              /api requests
                    |
                    v
               Go Backend
              Port 8080
                    |
                    v
                PostgreSQL
                Port 5432
```

The frontend is exposed on host port `8080`, while the backend health endpoint is available on host port `8081` through Docker Compose.

## Tech Stack

- **Frontend:** React, Vite
- **Backend:** Go
- **Database:** PostgreSQL
- **Containers:** Docker, Docker Compose
- **Web Server & Reverse Proxy:** Nginx
- **Security Scanning:** Docker Scout
- **Version Control:** Git and GitHub

## My DevOps Contributions

### 1. Production Frontend Container

- Replaced the Vite preview runtime with Nginx to serve the production frontend build.
- Used a multi-stage Docker build to separate build-time dependencies from the final frontend image.
- Used `npm ci` for lockfile-based dependency installation.
- Configured Nginx to serve the frontend and proxy `/api/` requests to the backend.

### 2. Container Networking and Troubleshooting

- Investigated an Nginx startup failure caused by the backend hostname not resolving in a standalone container.
- Connected the container to the appropriate Docker network and restarted it.
- Verified frontend availability and the backend API health endpoint.

### 3. Dependency Maintenance

- Updated selected Go dependencies and frontend packages.
- Ran backend tests, frontend tests, build, and lint checks during development.
- Reviewed dependency audit findings and considered compatibility when updating packages.

### 4. Docker Image Security

- Scanned the frontend image using Docker Scout.
- Changed the frontend runtime to an Nginx Alpine-slim image.
- Updated available Alpine packages during the build.
- Rebuilt and rescanned the frontend image; Docker Scout reported zero vulnerable packages detected in that scan.

**Security note:** This result applies to the scanned frontend image at that time. It does not establish that the entire application is vulnerability-free. Backend dependency findings require separate verification.

## Run Locally

### Prerequisites

- Docker Desktop
- Git

Node.js, Go, and PostgreSQL do not need to be installed separately when using the provided Docker Compose setup.

### 1. Clone the repository

```bash
git clone https://github.com/AVINASHPAWARCODE/devboard-devops-implementation.git
cd devboard-devops-implementation
```

### 2. Create the environment file

On Windows PowerShell:

```powershell
Copy-Item .env.example .env
```

Review `.env` locally and use appropriate values for your environment. Never commit real credentials or secrets.

### 3. Build and start the application

```bash
docker compose up -d --build
```

### 4. Verify the application

Open the frontend:

http://localhost:8080

Check the backend health endpoint:

```powershell
Invoke-RestMethod http://localhost:8081/health
```

Check the frontend API proxy:

```powershell
Invoke-RestMethod http://localhost:8080/api/health
```

The exact response depends on the backend implementation.

### 5. Stop the application

```bash
docker compose down
```

This stops and removes the Compose containers and network. Named database volumes are normally preserved unless explicitly removed.

## Testing and Verification

The following checks were performed during development:

- Backend Go tests
- Frontend tests and production build
- Frontend lint checks
- Frontend HTTP response verification
- Backend health endpoint verification through the frontend proxy
- Docker Scout scan of the rebuilt frontend image

## Current Scope and Future Improvements

This repository currently focuses on Docker, Nginx, container networking, dependency updates, and frontend image security.

Planned improvements include:

- GitHub Actions CI pipeline
- Automated image builds and registry publishing
- Infrastructure provisioning with Terraform
- AWS deployment
- Monitoring with Prometheus and Grafana

These are planned improvements and should not be considered implemented until they are completed and tested.

## Learning Outcomes

Through this project, I practised:

- Building and running multi-container applications
- Configuring Nginx as a frontend server and API reverse proxy
- Troubleshooting Docker networking and DNS resolution
- Maintaining application dependencies
- Scanning and improving Docker image security
- Documenting setup, verification, and troubleshooting steps

## Acknowledgements

The original DevBoard application is available at:

https://github.com/LondheShubham153/devboard-starter

This repository documents my DevOps-related modifications and learning work on top of that application.


