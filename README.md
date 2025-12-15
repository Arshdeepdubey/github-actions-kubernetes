# 🚀 GitHub Actions CI/CD with Kubernetes Deployment

Automate your CI/CD pipeline using GitHub Actions and deploy your application seamlessly to a Kubernetes cluster.

![GitHub Actions and Kubernetes](https://your-image-url.com/github-actions-kubernetes.png)

## 📋 Table of Contents

- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
  - [1. Fork or Clone the Repository](#1-fork-or-clone-the-repository)
  - [2. Project Structure](#2-project-structure)
  - [3. Configure Docker and Kubernetes Secrets](#3-configure-docker-and-kubernetes-secrets)
  - [4. Set Up GitHub Actions Secrets](#4-set-up-github-actions-secrets)
- [Usage](#usage)
  - [1. Build and Push Docker Image](#1-build-and-push-docker-image)
  - [2. Deploy to Kubernetes](#2-deploy-to-kubernetes)
  - [3. Test the Application](#3-test-the-application)
- [Step-by-Step Commands](#step-by-step-commands)
- [Contributing](#contributing)
- [License](#license)

---

## Introduction

This repository contains a sample Node.js application that demonstrates how to set up a CI/CD pipeline using GitHub Actions to build and deploy to a Kubernetes cluster. By following this guide, you'll learn how to automate your deployments and ensure your applications are always up-to-date.

## Prerequisites

Before you begin, make sure you have the following installed and set up:

- **Git**: For cloning and managing repositories.
- **Docker**: To build and manage container images.
- **Node.js and NPM**: For running the Node.js application.
- **kubectl**: Kubernetes command-line tool to interact with your cluster.
- **A Kubernetes Cluster**: Can be local (Minikube) or cloud-based (GKE, EKS, AKS).
- **GitHub Account**: To fork the repository and set up GitHub Actions.
- **Docker Hub Account**: To push Docker images.

## Getting Started

### 1. Fork or Clone the Repository

#### **Option A: Fork the Repository**

1. Navigate to the [repository](https://github.com/your-username/github-actions-kubernetes).
2. Click on the **"Fork"** button at the top right to create a copy under your GitHub account.

#### **Option B: Clone the Repository**

```bash
git clone https://github.com/your-username/github-actions-kubernetes.git
```

Change directory into the project folder:

```bash
cd github-actions-kubernetes
```

### 2. Project Structure

Here's the layout of the repository:

```
github-actions-kubernetes/
├── .github/
│   └── workflows/
│       └── ci-cd.yaml          # GitHub Actions workflow
├── .gitignore                  # Files and directories to ignore in Git
├── README.md                   # Project documentation
├── app.js                      # Node.js application code
├── package.json                # NPM configuration file
├── package-lock.json           # NPM lock file
├── Dockerfile                  # Docker configuration
├── deployment.yaml             # Kubernetes Deployment manifest
└── service.yaml                # Kubernetes Service manifest
```

### 3. Configure Docker and Kubernetes Secrets

#### **Docker Hub**

1. **Log In to Docker Hub:**

   ```bash
   docker login
   ```

2. **Create a Personal Access Token (Recommended):**

  - Go to Docker Hub > **Account Settings** > **Security**.
  - Click **New Access Token**, name it, and copy the token.

#### **Kubernetes Config**

1. **Get Your Kubeconfig File:**

  - Typically located at `$HOME/.kube/config`.

2. **Base64 Encode the Kubeconfig:**

   ```bash
   cat $HOME/.kube/config | base64 | pbcopy  # For macOS
   ```

   **Note:** On Windows, you can use:

   ```bash
   type %USERPROFILE%\.kube\config | base64 | clip
   ```

### 4. Set Up GitHub Actions Secrets

1. **Navigate to Your Forked Repository on GitHub.**

2. **Go to Settings > Secrets and variables > Actions.**

3. **Add the Following Secrets:**

  - **`DOCKERHUB_USERNAME`**: Your Docker Hub username.
  - **`DOCKERHUB_TOKEN`**: Your Docker Hub password or access token.
  - **`KUBE_CONFIG_DATA`**: The base64 encoded kubeconfig file.

## Usage

### 1. Build and Push Docker Image

**Note:** The GitHub Actions workflow will automate these steps, but you can also do them manually.

#### **Build the Docker Image:**

```bash
docker build -t your-dockerhub-username/hello-kubernetes:latest .
```

#### **Push the Image to Docker Hub:**

```bash
docker push your-dockerhub-username/hello-kubernetes:latest
```

### 2. Deploy to Kubernetes

**Apply Kubernetes Manifests:**

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

### 3. Test the Application

1. **Get the Service IP:**

   ```bash
   kubectl get services
   ```

2. **Access the Application:**

  - Navigate to the external IP in your browser.
  - You should see **"Hello, Kubernetes!"** displayed.

## Step-by-Step Commands

Here's a consolidated list of commands to set up and run the project.

### **Clone the Repository**

```bash
git clone https://github.com/your-username/github-actions-kubernetes.git
cd github-actions-kubernetes
```

### **Install Dependencies**

```bash
npm install
```

### **Run the Application Locally**

```bash
node app.js
```

Visit `http://localhost:3000` to test.

### **Build the Docker Image**

```bash
docker build -t your-dockerhub-username/hello-kubernetes:latest .
```

### **Run the Docker Container Locally**

```bash
docker run -p 3000:3000 your-dockerhub-username/hello-kubernetes:latest
```

### **Push the Docker Image**

```bash
docker push your-dockerhub-username/hello-kubernetes:latest
```

### **Deploy to Kubernetes**

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

### **Check Deployment Status**

```bash
kubectl get deployments
kubectl get services
kubectl get pods
```

### **Set Up GitHub Actions Secrets**

1. **Access Repository Settings on GitHub.**

2. **Add Secrets:**

  - **DOCKERHUB_USERNAME**
  - **DOCKERHUB_TOKEN**
  - **KUBE_CONFIG_DATA**

### **Trigger GitHub Actions Workflow**

- **Push Changes to GitHub:**

  ```bash
  git add .
  git commit -m "Your commit message"
  git push origin main
  ```

- The GitHub Actions workflow will automatically run on pushes to the `main` branch.

## Contributing

Contributions are welcome! Please open an issue or submit a pull request for any improvements or suggestions.

### **Steps to Contribute:**

1. **Fork the Repository.**

2. **Create a New Branch:**

   ```bash
   git checkout -b feature/your-feature-name
   ```

3. **Make Your Changes and Commit:**

   ```bash
   git commit -m "Add your message"
   ```

4. **Push to Your Forked Repository:**

   ```bash
   git push origin feature/your-feature-name
   ```

5. **Open a Pull Request on GitHub.**

## License

This project is licensed under the Apache License Version 2.0- see the [LICENSE](LICENSE) file for details.

---

**Feel free to reach out if you have any questions or need further assistance!**

# 😊 Happy Coding!

---

## Additional Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Kubernetes Documentation](https://kubernetes.io/docs/home/)
- [Docker Documentation](https://docs.docker.com/)

---

**Note:** Replace `your-username` and `your-dockerhub-username` with your actual GitHub and Docker Hub usernames respectively.# trigger
