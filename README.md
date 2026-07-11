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

**Jenkins** — Orchestrates the entire pipeline end to end. Every stage in this project (checkout, code analysis, build, security scans, image push, and deployment) is defined as a Jenkins declarative pipeline, so the whole workflow runs automatically whenever new code is pushed, with zero manual steps in between.

**Node.js** — The runtime the application itself is built on. It's the codebase being tested, analyzed, containerized, and deployed throughout this pipeline.

**Docker** — Packages the Node.js application and all its dependencies into a single, portable container image. This guarantees the app runs identically on any machine — the developer's laptop, the CI server, or production — eliminating "it works on my machine" issues.

**SonarQube** — Performs static code analysis on every build. It scans for bugs, code smells, and vulnerabilities, and enforces a Quality Gate — meaning if the code doesn't meet the defined quality bar, the pipeline can fail before a single Docker image is even built. This catches problems early, before they reach production.

**Trivy** — A security scanner used twice in this pipeline for layered protection: first on the raw filesystem/dependencies (catching vulnerable npm packages before the build), and again on the final Docker image (catching vulnerabilities introduced by the base image or OS-level packages). This is a "shift-left" security practice — finding issues as early as possible rather than after deployment.

**Docker Hub** — The registry where the final, scanned Docker image is automatically published after passing all quality and security checks. From there, it can be pulled and deployed anywhere.

**Git & GitHub** — Version control for the source code, and the trigger point for the entire pipeline. Jenkins is configured to detect changes pushed to GitHub and kick off a fresh build automatically.

---

## 🔄 Pipeline Architecture

```
 <img width="940" height="501" alt="image" src="https://github.com/user-attachments/assets/4509296b-baf9-4e64-b98f-e8cd5bdfdff6" />


```

### Pipeline Performance (Actual Run)


<img width="940" height="260" alt="image" src="https://github.com/user-attachments/assets/8968f690-a3bf-4de6-b2a6-f9f780d2d002" />



---

## ✅ Code Quality Results (SonarQube)

<img width="940" height="200" alt="image" src="https://github.com/user-attachments/assets/71feb9a6-06d0-448a-af7c-6ebbe13ef61a" />


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

<img width="1901" height="742" alt="image" src="https://github.com/user-attachments/assets/e8f0e7d3-9f67-49a5-9afc-89b247295009" />


🔗 **[View on Docker Hub](https://hub.docker.com/r/krish951565/YOUR-IMAGE-NAME)**

Pull and run it locally:
```bash
docker pull krish951565/YOUR-IMAGE-NAME
docker run -p 3000:3000 krish951565/YOUR-IMAGE-NAME
```
Final Result

After passing through every stage of the pipeline — code quality checks, security scans, containerization, and deployment — here's the application running live, served straight from the Docker container built and shipped by this CI/CD pipeline:

<img width="940" height="280" alt="image" src="https://github.com/user-attachments/assets/dbaff5a6-bdfc-449e-83e7-1e71b1a67ead" />


From a single git push, this pipeline took the code all the way to a running, scanned, production-style deployment — with no manual steps in between.
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
