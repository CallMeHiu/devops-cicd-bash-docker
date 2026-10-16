# DevOps CI/CD Automation with Bash, Docker and GitHub Actions

![CI/CD](https://img.shields.io/badge/CI%2FCD-GitHub%20Actions-blue)
![Docker](https://img.shields.io/badge/Container-Docker-2496ED)
![Python](https://img.shields.io/badge/Python-3.12-3776AB)
![Automation](https://img.shields.io/badge/Automation-Bash-4EAA25)

## Overview

This project demonstrates a practical CI/CD workflow for a Python web application using Bash scripting, Docker, GitHub Actions, and GitHub Container Registry (GHCR).

The objective is to automate application testing, validate shell scripts, build a container image, verify the build, and publish the image only after the CI job succeeds.

## Architecture

Developer → GitHub → GitHub Actions CI → Automated Tests → Bash Automation → Docker Build → GHCR

The publishing job runs only after the test job succeeds and only for pushes to the main branch or version tags.

## Tech Stack

- Linux / Bash
- Python 3.12
- Flask
- pytest
- Docker
- GitHub Actions
- GitHub Container Registry
- ShellCheck
- Git

## Key Features

- Automated Python tests using pytest.
- Reusable Bash scripts for testing and Docker builds.
- Bash prerequisite checks and explicit error handling.
- Docker image build and inspection.
- Non-root container execution.
- Docker health checks.
- Automated CI on pushes and pull requests.
- Automated image publishing to GHCR.
- Version-based image tags and commit SHA tags.

## Repository Structure

```text
.
├── app/
│   ├── __init__.py
│   └── main.py
├── tests/
│   └── test_app.py
├── scripts/
│   ├── test.sh
│   ├── build.sh
│   └── ci.sh
├── .github/
│   └── workflows/
│       └── ci-cd.yml
├── Dockerfile
├── .dockerignore
├── .gitignore
├── requirements.txt
├── requirements-dev.txt
└── README.md
```

## Prerequisites

- Python 3.12
- Git
- Docker Engine or Docker Desktop
- A GitHub account for remote CI/CD execution

## Run Locally

Create a virtual environment and install dependencies:

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements-dev.txt
```

Run the tests:

```bash
python -m pytest -v
```

Run the complete local CI workflow:

```bash
chmod +x scripts/*.sh
./scripts/ci.sh local
```

Run the container:

```bash
docker run -d \
  --name devops-cicd-demo \
  -p 5000:5000 \
  devops-cicd-demo:local
```

Verify the application:

```bash
curl -fsS http://127.0.0.1:5000/
curl -fsS http://127.0.0.1:5000/health
```

Clean up:

```bash
docker rm -f devops-cicd-demo
```

## CI/CD Workflow

1. Check out the repository.
2. Set up Python.
3. Install application and development dependencies.
4. Run automated tests.
5. Perform shell script analysis.
6. Execute the local CI automation script.
7. Build the Docker image.
8. Publish the image to GHCR after the test job succeeds.

Pull requests run validation but do not publish images.

## Container Security

- The application runs as a non-root user.
- A `.dockerignore` file reduces unnecessary build context.
- GitHub Actions uses the built-in `GITHUB_TOKEN` for registry authentication.
- Publishing permissions are limited to the publishing job.
- Secrets and credentials must not be committed to the repository.

## Engineering Decisions

- Bash scripts are reused between local development and CI.
- Test failures stop the pipeline before publishing.
- Docker builds are separated from registry publishing.
- Version tags and commit SHA tags help identify image versions.
- A health endpoint provides a basic runtime verification mechanism.

## Future Improvements

- Add Docker image vulnerability scanning.
- Add test coverage reporting.
- Pin dependencies using a lock file.
- Add SBOM generation and image provenance.
- Extend publishing to AWS Elastic Container Registry (ECR).
- Deploy the image to an AWS environment using least-privilege IAM.
- Introduce a staging environment and deployment approval gates.

## Learning Outcomes

This project demonstrates hands-on practice with Bash automation, CI/CD design, automated testing, Docker image lifecycle management, GitHub Actions permissions, container health checks, and reproducible troubleshooting.

## Disclaimer

This is a portfolio and learning project. It is not intended to be a production-ready deployment platform without additional security review, dependency pinning, vulnerability scanning, monitoring, and deployment controls.
