
# Brain Tasks App – React Application Deployment on AWS EKS

## 1. Project Overview

This project demonstrates deploying a React application using Docker, Amazon ECR, Amazon EKS, Kubernetes, AWS CodeBuild and AWS CodePipeline.

The application is containerized using Docker and deployed to an Amazon EKS cluster. An AWS Network Load Balancer exposes the application publicly.

## 2. Technologies Used

- GitHub – Source code management
- Docker – Application containerization
- Amazon ECR – Docker image repository
- Amazon EKS – Kubernetes cluster
- Kubernetes – Application deployment and service management
- AWS CodeBuild – Docker image build and deployment automation
- AWS CodePipeline – CI/CD automation
- Nginx – Web server
- AWS Network Load Balancer – Public application access

## 3. Project Architecture

GitHub → AWS CodePipeline → AWS CodeBuild → Amazon ECR → Amazon EKS → Network Load Balancer → React Application

### Pipeline Stages

**Stage 1: Source**
- GitHub repository connected through AWS CodeConnections.
- Code changes pushed to the main branch trigger the pipeline.

**Stage 2: Build**
- AWS CodeBuild builds the Docker image.
- The image is tagged using the Git commit ID.
- The image is pushed to Amazon ECR.

**Stage 3: Deploy**
- AWS CodeBuild connects to the EKS cluster.
- Kubernetes deployment is updated with the latest Docker image.
- Kubernetes rollout status is verified.

## 4. AWS Configuration

| Configuration | Value |
|---|---|
| AWS Region | ap-south-1 |
| EKS Cluster | brain-tasks-eks |
| ECR Repository | brain-tasks-app |
| CodePipeline | brain-tasks-pipeline |
| Build Project | brain-tasks-build |
| Deploy Project | brain-tasks-deploy |
| Kubernetes Deployment | brain-tasks-deployment |
| Kubernetes Service | brain-tasks-service |
| Replicas | 2 |

## 5. Repository Structure

```text
Brain-Task-App/
│
├── Dockerfile
├── nginx.conf
├── buildspec-build.yml
├── buildspec-deploy.yml
├── README.md
├── dist/
│
└── k8s/
    ├── deployment.yaml
    └── service.yaml
```

## 6. Deployment Instructions

### Step 1: Clone the repository

```bash
git clone https://github.com/DhanyaSathasivam/Brain-Task-App.git
cd Brain-Task-App
```

### Step 2: Build the Docker image

```bash
docker build -t brain-tasks-app:v1 .
```

### Step 3: Run the application locally

```bash
docker run -d -p 3000:3000 brain-tasks-app:v1
```

Open:

http://localhost:3000

### Step 4: Deploy to Kubernetes

Configure kubectl to connect to the EKS cluster:

```bash
aws eks update-kubeconfig \
  --region ap-south-1 \
  --name brain-tasks-eks
```

Apply Kubernetes manifests:

```bash
kubectl apply -f k8s/deployment.yaml
kubectl apply -f k8s/service.yaml
```

### Step 5: Verify deployment

```bash
kubectl get deployments
kubectl get pods -o wide
kubectl get services
```

## 7. Application URL

http://k8s-default-braintas-88f6a38ee3-22b52d4c23b0a82b.elb.ap-south-1.amazonaws.com

## 8. Verification Results

- Kubernetes deployment: 2/2 replicas available.
- Both application pods: Running.
- Pod restarts: 0.
- LoadBalancer: Active.
- HTTP response: 200 OK.
- CodePipeline Source stage: Succeeded.
- CodePipeline Build stage: Succeeded.
- CodePipeline Deploy stage: Succeeded.

## 9. Screenshots

Project screenshots are included in the screenshots folder.

1. GitHub repository.
2. AWS CodePipeline showing Source, Build and Deploy success.
3. Amazon ECR Docker image.
4. EKS Kubernetes deployment and running pods.
5. Kubernetes LoadBalancer service.
6. React application running through the public LoadBalancer URL.

## 10. Conclusion

Successfully implemented an automated CI/CD pipeline to build, containerize and deploy a React application to Amazon EKS using Docker, Amazon ECR, Kubernetes, AWS CodeBuild and AWS CodePipeline.
