# 🚀 Brain Tasks App – End-to-End DevOps Deployment

## 📌 Project Overview

This project demonstrates a complete **DevOps pipeline** to deploy a React application into a **production-ready Kubernetes environment (AWS EKS)** using modern DevOps tools.

It covers:

* Application containerization
* CI/CD pipeline automation
* Kubernetes deployment
* Monitoring and logging

---

## 🏗️ Architecture

```
GitHub → CodePipeline → CodeBuild → DockerHub → AWS EKS → LoadBalancer → Browser
                                      ↓
                                 CloudWatch Logs
```

---

## 🧰 Tech Stack

| Category         | Tools Used                      |
| ---------------- | ------------------------------- |
| Source Control   | GitHub                          |
| CI/CD            | AWS CodePipeline, AWS CodeBuild |
| Containerization | Docker                          |
| Registry         | DockerHub                       |
| Orchestration    | Kubernetes (AWS EKS)            |
| Monitoring       | AWS CloudWatch                  |
| Application      | React (Vite build)              |

---

## 📦 Application Setup

### 🔹 Clone Repository

```bash
git clone https://github.com/Vennilavanguvi/Brain-Tasks-App.git
cd Brain-Tasks-App
```

### 🔹 Application Runs On

```
Port: 3000 (development)
Port: 80 (containerized production)
```

---

## 🐳 Dockerization

### 🔹 Dockerfile

* Multi-stage build used for optimized image
* Static files served via Nginx

### 🔹 Build Image

```bash
docker build -t brain-app .
```

### 🔹 Run Container

```bash
docker run -d -p 3000:80 brain-app
```

---

## 📦 Docker Registry (DockerHub)

### 🔹 Tag Image

```bash
docker tag brain-app <your-username>/brain-app:latest
```

### 🔹 Push Image

```bash
docker push <your-username>/brain-app:latest
```

---

## ☸️ Kubernetes Deployment (EKS)

### 🔹 Create EKS Cluster

```bash
eksctl create cluster \
--name brain-cluster \
--region ap-south-1 \
--nodegroup-name workers \
--node-type t3.medium \
--nodes 2
```

---

### 🔹 Deployment YAML

* Creates pods with Docker image

### 🔹 Service YAML

* Type: LoadBalancer
* Exposes application publicly

---

### 🔹 Deploy Application

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```

---

### 🔹 Verify Deployment

```bash
kubectl get pods
kubectl get svc
```

---

### 🌐 Access Application

```
http://<EXTERNAL-IP>
```

---

## ⚙️ CI/CD Pipeline

### 🔹 CodePipeline Stages

1. **Source**

   * GitHub repository connected via CodeConnections

2. **Build (CodeBuild)**

   * Docker image build
   * Push to DockerHub

3. **Deploy (CodeBuild)**

   * Update kubeconfig
   * Deploy to EKS using kubectl

---

## 📄 buildspec.yml

```yaml
version: 0.2

phases:
  install:
    commands:
      - echo "Starting build process"

  pre_build:
    commands:
      - echo "Logging into DockerHub"
      - printf "%s" "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin

  build:
    commands:
      - echo "Building Docker image"
      - docker build -t brain-app .
      - docker tag brain-app $DOCKER_USERNAME/brain-app:latest

  post_build:
    commands:
      - echo "Pushing Docker image"
      - docker push $DOCKER_USERNAME/brain-app:latest

      - echo "Updating kubeconfig"
      - aws eks update-kubeconfig --region ap-south-1 --name brain-cluster

      - echo "Deploying to Kubernetes"
      - kubectl apply -f deployment.yaml
      - kubectl apply -f service.yaml
```

---

## 📊 Monitoring (CloudWatch)

### 🔹 Build & Deploy Logs

* Integrated with CloudWatch Logs
* Log group:

```
/aws/codebuild/brain-build
```

### 🔹 View Logs

* CodeBuild → Build history → View logs
* CloudWatch → Log groups

---

### 🔹 Application Logs

```bash
kubectl logs <pod-name>
```

---

### 🔹 Alerts (Optional)

* CloudWatch Alarms configured for:

  * High CPU usage
  * Build failures

---

## 🔐 Security Best Practices

* Used **DockerHub Personal Access Token**
* Avoided hardcoding credentials
* Used environment variables in CodeBuild

---

## 📸 Screenshots 

* CodePipeline success  
* CodeBuild logs
* Kubernetes pods running
* LoadBalancer external IP
* Application UI in browser
* CloudWatch logs

---

## 📌 Submission Details

* **GitHub Repo:** (Add your repo link)
* **LoadBalancer URL:** (Add external URL)

---

## 🧠 Key Learnings

* End-to-end CI/CD pipeline setup
* Docker image lifecycle
* Kubernetes deployment on EKS
* CloudWatch monitoring & debugging
* Handling real-world DevOps issues

---

## 🚀 Future Improvements

* Use AWS ECR instead of DockerHub
* Add Prometheus + Grafana monitoring
* Implement auto-scaling (HPA)
* Use Terraform for infrastructure automation

---

## 🙌 Conclusion

This project demonstrates a **production-ready DevOps workflow** integrating CI/CD, containerization, Kubernetes, and monitoring using AWS services.

---
