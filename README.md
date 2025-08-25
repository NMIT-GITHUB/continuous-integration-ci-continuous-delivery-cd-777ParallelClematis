[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/hcpUEYOj)
[![Open in Visual Studio Code](https://classroom.github.com/assets/open-in-vscode-2e0aaae1b6195c2367325f4f02e2d04e9abb55f0b24a779b69b11b9e10269abc.svg)](https://classroom.github.com/online_ide?assignment_repo_id=20159698&assignment_repo_type=AssignmentRepo)
# CICD_EK_WORKSHOP
Hands-on introduction to CI/CD with GitHub Actions, delivered by Prof. Mads Nyborg (Business Academy Copenhagen). Learn to build and extend YAML workflows for automated builds, tests, and deployments directly in your repositories.

# CI/CD with GitHub Actions

## What is CI/CD?
- **Continuous Integration (CI):**  
  Every change pushed to the repository is automatically built and tested. The goal is to catch bugs early and keep the main branch deployable.  

- **Continuous Delivery/Deployment (CD):**  
  Approved changes are automatically prepared or deployed to staging/production environments.  

---

## Why CI/CD with GitHub?
- Automates building, testing, and deploying.  
- Provides fast feedback inside pull requests.  
- Runs on GitHub-hosted virtual machines (Linux, Windows, macOS).  
- Easy to configure using **YAML files** in your repo.  

---

## GitHub Actions Basics

- **Workflow:**  
  Automated process defined in a `.yml` file inside `/.github/workflows/`.  

- **Job:**  
  A collection of steps that run on the same virtual machine (runner).  

- **Step:**  
  Either a shell command or a pre-built action from the GitHub Marketplace.  

- **Runner:**  
  A server provided by GitHub (Ubuntu, Windows, macOS) that executes jobs. Each run starts on a fresh VM.  

---

## Example: Simple Workflow

```yaml
name: Hello and Goodbye Workflow

on:
  push:
    branches:
      - main
  workflow_dispatch:   # Allows manual trigger

jobs:
  hello:
    runs-on: ubuntu-latest
    steps:
      - name: Say Hello
        run: echo "Hello world"

  goodbye:
    runs-on: ubuntu-latest
    steps:
      - name: Say Goodbye
        run: echo "Goodbye"
```

- Runs automatically when code is pushed to `main`.  
- Can also be started manually from the GitHub Actions tab.  

---

## Example: Node.js Workflow

Add scripts to your repo:  

**hello.js**
```js
console.log("Hello from JavaScript!");
```

**goodbye.js**
```js
console.log("Goodbye from JavaScript!");
```

Workflow file: `/.github/workflows/test.yml`  

```yaml
name: Hello and Goodbye with Node.js

on:
  push:
    branches: [ main ]
  workflow_dispatch:

jobs:
  hello:
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository
        uses: actions/checkout@v4
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 20
      - name: Run Hello script
        run: node hello.js

  goodbye:
    needs: hello   # Ensures hello runs first
    runs-on: ubuntu-latest
    steps:
      - name: Check out repository
        uses: actions/checkout@v4
      - name: Set up Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 20
      - name: Run Goodbye script
        run: node goodbye.js
```

---

## Key Takeaways
- Workflows live in `.github/workflows/*.yml`.  
- Each workflow can contain multiple jobs.  
- Jobs run on runners, and can depend on each other (`needs`).  
- Actions (like `actions/checkout` or `actions/setup-node`) save time by reusing community scripts.  
- YAML syntax defines everything: triggers, jobs, steps, environments.  

---
