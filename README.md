# 🍽️ Containerizing a Node.js Application Using CI/CD Pipeline

An end-to-end DevOps pipeline that takes a Node.js web application (a Zomato-style food discovery app) from source code to a running, security-scanned Docker container — fully automated with Jenkins.

![Pipeline Passed](https://img.shields.io/badge/pipeline-passing-brightgreen)
![Docker](https://img.shields.io/badge/containerized-Docker-blue)
![Quality Gate](https://img.shields.io/badge/SonarQube-Quality%20Gate%20Passed-success)
![Security Scan](https://img.shields.io/badge/Trivy-Scanned-orange)

---

## 📌 Project Overview

This project demonstrates a **production-style CI/CD pipeline** built entirely with Jenkins, covering everything a real DevOps workflow needs:

- Automated build triggered on every code change
- Static code quality analysis with SonarQube
- Docker image build and containerization
- Vulnerability scanning of both the filesystem and the built image using Trivy
- Automated push to Docker Hub
- Automated deployment of the container

The goal was to simulate how a real engineering team ships a Node.js application safely and repeatably — not just "build and deploy," but **build, test, scan, and only then deploy.**

---

## 🧰 Tech Stack

| Category | Tool |
|---|---|
| CI/CD Orchestration | Jenkins |
| Language / Runtime | Node.js |
| Containerization | Docker |
| Code Quality | SonarQube |
| Security Scanning | Trivy (filesystem + image scan) |
| Image Registry | Docker Hub |
| Version Control | Git & GitHub |

---

## 🔄 Pipeline Architecture

```
 ┌─────────────────┐
 │  Tool Install    │  Install required CLI tools (Node, Docker, scanners)
 └────────┬─────────┘
          │
 ┌────────▼─────────┐
 │ Clean Workspace   │  Wipe previous build artifacts
 └────────┬─────────┘
          │
 ┌────────▼─────────┐
 │  Code (Checkout)  │  Pull latest source from GitHub
 └────────┬─────────┘
          │
 ┌────────▼─────────┐
 │  CQA (SonarQube)  │  Static code analysis & quality gate check
 └────────┬─────────┘
          │
 ┌────────▼─────────┐
 │   Build Code      │  Install dependencies & build the app
 └────────┬─────────┘
          │
 ┌────────▼─────────┐
 │   Docker Build     │  Build the container image
 └────────┬─────────┘
          │
 ┌────────▼─────────┐
 │ Trivy (Filesystem)│  Scan source/dependency files for vulnerabilities
 └────────┬─────────┘
          │
 ┌────────▼─────────┐
 │  Trivy Image Scan  │  Scan the built Docker image itself
 └────────┬─────────┘
          │
 ┌────────▼─────────┐
 │   Push to Docker   │  Publish image to Docker Hub
 │       Hub          │
 └────────┬─────────┘
          │
 ┌────────▼─────────┐
 │  Docker Deploy      │  Run the container from the published image
 └────────┬─────────┘
          │
 ┌────────▼─────────┐
 │  Post Actions       │  Notifications / cleanup / reporting
 └──────────────────┘
```

### Pipeline Performance (Actual Run)

| Stage | Time |
|---|---|
| Declarative: Tool Install | 182ms |
| Clean Workspace | 420ms |
| Code | 1s |
| CQA | 32s |
| Build code | 25s |
| Docker Build | 3min 29s |
| Trivy Files | 12s |
| Image Scan | 1min 52s |
| DockerHub Push | 1min 16s |
| Docker Deploy | 765ms |
| Post Actions | 335ms |
| **Total Runtime** | **~7min 53s** |

---

## 📸 Screenshots

> Add these images to a `screenshots/` folder in your repo, then they'll render automatically below.

**Jenkins Dashboard**
![Jenkins Dashboard](screenshots/jenkins-dashboard.png)

**Pipeline Stage View — Full Build Breakdown**
![Pipeline Stages](screenshots/pipeline-stages.png)

**SonarQube — Quality Gate Passed**
![SonarQube Quality Gate](screenshots/sonarqube-quality-gate.png)

**Application Running**
![App Screenshot](screenshots/app-running.png)

---

## ✅ Code Quality Results (SonarQube)

| Metric | Result |
|---|---|
| Quality Gate | ✅ **Passed** |
| Bugs | 1 |
| Vulnerabilities | 0 |
| Code Smells | 0 |
| Lines of Code | 1.3k |
| Languages Analyzed | CSS, JavaScript |

The pipeline is configured to fail the build automatically if the SonarQube Quality Gate does not pass, enforcing a baseline code quality standard before any image is even built.

---

## 🔒 Security Scanning (Trivy)

Trivy is run **twice** in this pipeline for defense-in-depth:

1. **Filesystem scan** — checks source code and dependency manifests (`package.json`, `package-lock.json`) for known vulnerable packages before the image is even built.
2. **Image scan** — scans the final built Docker image, catching vulnerabilities introduced by the base image or system-level packages.

This ensures vulnerabilities are caught early (shift-left security) rather than discovered only after deployment.

---

## 🐳 Docker Hub

The final image is automatically pushed to Docker Hub as part of the pipeline:

🔗 **[View on Docker Hub](https://hub.docker.com/r/krish951565/YOUR-IMAGE-NAME)**

Pull and run it locally:
```bash
docker pull krish951565/YOUR-IMAGE-NAME
docker run -p 3000:3000 krish951565/YOUR-IMAGE-NAME
```

---

## ⚙️ Running Locally (without Jenkins)

```bash
# Clone the repo
git clone https://github.com/Krish951565/Containerizing-NodeJS-Application-Using-CICD-PIPELINE.git
cd Containerizing-NodeJS-Application-Using-CICD-PIPELINE

# Install dependencies
npm install

# Run the app
npm start
```

Or run it fully containerized:
```bash
docker build -t nodejs-app .
docker run -p 3000:3000 nodejs-app
```

---

## 🚀 What This Project Demonstrates

- Designing a **multi-stage Jenkins declarative pipeline** from scratch
- Integrating **SonarQube** for automated code quality gating
- Using **Trivy** for both dependency and container image vulnerability scanning
- Automating **Docker image build, tag, and push** to a registry
- Automating **deployment** as the final pipeline stage
- Building a pipeline that **fails fast** on quality or security issues, rather than deploying blindly

---

## 🧗 Challenges & Learnings

- Configuring Jenkins to correctly authenticate with Docker Hub and SonarQube using stored credentials rather than hardcoded secrets
- Tuning Trivy scans to catch real vulnerabilities without excessive false-positive noise
- Structuring the pipeline as declarative stages so each step's timing and status is independently visible (as seen in the stage view above) for easier debugging

---

## 📄 License

This project is for educational and portfolio purposes.
