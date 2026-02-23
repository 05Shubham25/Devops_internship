# DD Task — MEAN Stack CRUD App

A full-stack CRUD application built with the MEAN stack (MongoDB, Express, Angular 15, Node.js), fully containerized with Docker and deployed on Azure VM with CI/CD via GitHub Actions.

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | Angular 15 + Nginx |
| Backend | Node.js + Express |
| Database | MongoDB 6.0 |
| Reverse Proxy | Nginx |
| Containerization | Docker + Docker Compose |
| CI/CD | GitHub Actions |
| Cloud | Azure VM (Ubuntu) |

## Project Structure

```
crud-dd-task-mean-app/
├── backend/          # Node.js + Express REST API
│   └── Dockerfile
├── frontend/         # Angular 15 app
│   ├── Dockerfile
│   └── nginx-frontend.conf
├── nginx.conf        # Reverse proxy config
├── docker-compose.yml
└── .env
```

## Features

- Create, Read, Update, Delete tutorials
- Search tutorials by title
- MongoDB for persistent storage
- All services containerized and networked via Docker

## Local Setup (Docker)

### Prerequisites
- Docker
- Docker Compose

### Run locally

```bash
# Clone the repo
git clone <repo-url>
cd crud-dd-task-mean-app

# Start all containers
docker compose up -d --build

# App will be available at:
http://localhost
```

### Container Overview

| Container | Port | Description |
|-----------|------|-------------|
| nginx | 80 | Reverse proxy (main entry point) |
| frontend | internal | Angular app served via Nginx |
| backend | 8080 | Node.js REST API |
| mongodb | 27017 | MongoDB database |

### Useful Commands

```bash
# View running containers
docker ps

# View logs
docker compose logs -f

# Stop all containers
docker compose down

# Stop and remove volumes (clears DB)
docker compose down -v
```

## Environment Variables

Create a `.env` file in the root:

```env
MONGO_INITDB_DATABASE=dd_db
APP_PORT=8080
FRONTEND_PORT=80
```

## Nginx Reverse Proxy

Nginx routes all traffic on port 80:
- `/api/*` → backend container (port 8080)
- `/*` → frontend container (Angular app)

## CI/CD Pipeline (GitHub Actions)

On every push to `main` branch:
1. Docker images are built (frontend + backend)
2. Images are pushed to Docker Hub
3. Azure VM pulls latest images and restarts containers via SSH

### GitHub Secrets Required

| Secret | Description |
|--------|-------------|
| `DOCKER_USERNAME` | Docker Hub username |
| `DOCKER_PASSWORD` | Docker Hub password/token |
| `AZURE_VM_HOST` | Azure VM public IP |
| `AZURE_VM_USER` | VM SSH username |
| `AZURE_SSH_KEY` | VM private SSH key |

## Azure VM Deployment

1. Create Ubuntu VM on Azure
2. Open port 80 in NSG (Network Security Group)
3. SSH into VM and install Docker:

```bash
sudo apt update && sudo apt install -y docker.io docker-compose-plugin
sudo usermod -aG docker $USER
```

4. Copy `docker-compose.yml` and `.env` to VM
5. Run:

```bash
docker compose up -d
```

App accessible at: `http://<VM-PUBLIC-IP>`

## API Endpoints

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/api/tutorials` | Get all tutorials |
| GET | `/api/tutorials/:id` | Get tutorial by ID |
| POST | `/api/tutorials` | Create tutorial |
| PUT | `/api/tutorials/:id` | Update tutorial |
| DELETE | `/api/tutorials/:id` | Delete tutorial |
| DELETE | `/api/tutorials` | Delete all tutorials |
| GET | `/api/tutorials?title=xyz` | Search by title |
