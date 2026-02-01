🚀 Cloud-Native DevOps CI/CD Pipeline on Azure
📌 Project Overview

This project demonstrates an end-to-end DevOps implementation for deploying a containerized Node.js application on Microsoft Azure using a fully automated CI/CD pipeline.

The solution includes source control integration, containerization, private image registry, Kubernetes orchestration, and external exposure using Azure Load Balancer.

🏗️ Architecture Overview
Developer → GitHub → Jenkins → Docker → Azure Container Registry → AKS → Azure Load Balancer → End User

🔄 Workflow Explanation

Developer pushes code to GitHub.
GitHub webhook triggers Jenkins pipeline automatically.

Jenkins:

Pulls latest code
Builds Docker image
Tags image with build number
Pushes image to Azure Container Registry (ACR)
AKS pulls the latest image securely using Managed Identity.
Kubernetes Deployment creates pods.
Service of type LoadBalancer provisions Azure Public IP.
Application becomes accessible via browser.

🚀 Application Details

Technology: Node.js (Express)

Endpoints:
/ – Application homepage
/health – Health check endpoint

Application is containerized and deployed on Azure Kubernetes Service (AKS).

🛠️ Technologies Used

Cloud: Microsoft Azure
OS: Ubuntu Linux (Azure VM for Jenkins)
Version Control: Git & GitHub
CI/CD: Jenkins (Self-Managed)
Containerization: Docker
Container Registry: Azure Container Registry (ACR)
Orchestration: Azure Kubernetes Service (AKS)
Networking: Azure Load Balancer
CLI Tools: kubectl, Azure CLI

📂 Project Structure
nodejs-app/
│── app.js
│── package.json
│── Dockerfile
│── Jenkinsfile
│── deployment.yaml
│── service.yaml
│── README.md

🐳 Docker Implementation

The application is containerized using:

FROM node:18
WORKDIR /app
COPY . .
RUN npm install
CMD ["node", "app.js"]


This ensures portability and consistent runtime environment across deployments.

⚙️ CI/CD Pipeline (Jenkins)

Pipeline stages:
Clone Repository
Build Docker Image
Tag Image with Build Number
Push Image to ACR
Deploy Updated Image to AKS
Deployment is automated upon every code push.

☸ Kubernetes Deployment

Deployment manages pod lifecycle and replicas.
Service type: LoadBalancer
Azure automatically provisions:
Public IP
Azure Load Balancer
Backend configuration
Rolling updates ensure zero-downtime deployment.


🔐 Security Implementation

ACR integrated with AKS using Managed Identity.
No credentials stored inside Kubernetes manifests.
Private image registry for secure container storage.


🌍 Application Access

After deployment, the application is accessible via:

http://<Public-IP>


🧠 Key DevOps Concepts Demonstrated

End-to-End CI/CD Automation
Containerization Best Practices
Kubernetes Orchestration
ImagePullBackOff Troubleshooting
502 Debugging via Ingress/Service
Managed Identity Integration
Rolling Updates


📸 Screenshots (Add These)

Jenkins Successful Pipeline
Docker Image in ACR
kubectl get pods
kubectl get svc
Application Running in Browser


🎯 Future Enhancements

Helm Chart Implementation
GitOps with ArgoCD
Prometheus & Grafana Monitoring
Horizontal Pod Autoscaling

👨‍💻 Author

Bony Thomas
Cloud / DevOps Engineer
Open to DevOps & Cloud Opportunities
