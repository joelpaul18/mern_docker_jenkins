# MERN Stack CI/CD Pipeline with Docker & Jenkins

A MERN (MongoDB, Express, React, Node.js) application containerized with Docker and deployed through an automated Jenkins CI/CD pipeline, triggered by GitHub webhooks on every push.

Forked and adapted from the original instructor repository, with Docker, Docker Compose, and Jenkins pipeline configuration added on top.

---

## Project Overview

This project demonstrates a complete, working CI/CD workflow:

**Code push to GitHub → GitHub webhook triggers Jenkins → Jenkins builds Docker images → Images pushed to Docker Hub → Containers deployed and running**

---

## Architecture

```
                 ┌──────────────┐
   git push  --> │   GitHub     │
                 └──────┬───────┘
                        │ webhook
                        ▼
                 ┌──────────────┐
                 │   Jenkins    │
                 │  Pipeline    │
                 └──────┬───────┘
                        │
        ┌───────────────┼───────────────┐
        ▼               ▼               ▼
   Clone Repo     Build Images     Push to Docker Hub
        │               │               │
        └───────────────┴───────────────┘
                        │
                        ▼
              docker compose up -d
                        │
        ┌───────────────┼───────────────┐
        ▼               ▼               ▼
   React Client    Express API      MongoDB
   (port 3000)     (port 5000)   (data volume)
```

---

## Tech Stack

- **Frontend:** React
- **Backend:** Node.js / Express
- **Database:** MongoDB
- **Containerization:** Docker, Docker Compose
- **CI/CD:** Jenkins (Declarative Pipeline)
- **Image Registry:** Docker Hub
- **Trigger:** GitHub Webhook (auto-builds on every push to `main`)

---

## CI/CD Pipeline Stages

1. **Clone** — Pulls the latest code from GitHub (`main` branch)
2. **Build** — Builds the React client and Express server images using Docker Compose, tagged with the Jenkins build number
3. **Push** — Logs in to Docker Hub using Jenkins-managed credentials and pushes both images
4. **Run** — Starts (or restarts) all services — client, server, and MongoDB — using Docker Compose

Each image is tagged with the Jenkins `BUILD_NUMBER` (e.g. `api-server:14`), so every build produces a uniquely identifiable, traceable image — not just `latest`.

---

## Project Structure

```
.
├── client/              # React frontend
├── server/              # Express backend
├── docker-compose.yml   # Multi-container orchestration
├── Jenkinsfile          # CI/CD pipeline definition
└── README.md
```

---

## How the Automated Pipeline Works

1. A push to the `main` branch triggers a GitHub webhook.
2. Jenkins picks up the change and runs the pipeline automatically — no manual trigger needed.
3. Docker images for the client and server are built and tagged with the current Jenkins build number.
4. Images are securely pushed to Docker Hub using Jenkins credentials (no secrets stored in code).
5. The updated containers are deployed with `docker compose up -d`.

---

## Key Learnings from This Project

- Setting up a Declarative Jenkins Pipeline with multiple stages
- Using Jenkins credentials binding to handle Docker Hub login securely
- Tagging Docker images by build number for version traceability instead of relying on `latest`
- Configuring GitHub webhooks to trigger builds automatically on push
- Orchestrating a multi-container MERN app with Docker Compose

---

## Author

**Joel Paul**
[GitHub](https://github.com/joelpaul18)
