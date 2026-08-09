Three-Tier Architecture Deployment on AWS EKS
================

A microservices-based e-commerce application deployed on Amazon EKS, demonstrating containerized application orchestration, networking, and cloud-native infrastructure management.

Project Overview
================

This project showcases a complete three-tier architecture deployed on AWS EKS, featuring multiple microservices communicating through REST APIs and message queues. The application demonstrates real-world deployment patterns including load balancing, service discovery, persistent storage, and auto-scaling.

Architecture
================

The application consists of the following services built with diverse technology stacks:

- **Frontend:** AngularJS (1.x) with Nginx
- **Backend Services:**
  - NodeJS (Express)
  - Java (Spring Boot)
  - Python (Flask)
  - Golang
  - PHP (Apache)
- **Data Layer:**
  - MongoDB
  - Redis
  - MySQL (Maxmind data)
- **Messaging:** RabbitMQ

Infrastructure & Deployment
================

This project was deployed using:

- AWS EKS — Managed Kubernetes cluster
- Helm Charts — Application packaging and deployment
- AWS ALB Ingress Controller — Internet-facing load balancing
- EBS CSI Driver — Persistent storage for stateful services
- IAM Roles for Service Accounts (IRSA) — Fine-grained pod-level permissions

Prerequisites
================

- AWS Account with EKS permissions
- `kubectl`, `eksctl`, `helm` CLI tools installed
- An existing EKS cluster with managed node groups
- AWS Load Balancer Controller installed

Deployment Steps
================

1. Clone the Repository

```bash
git clone https://github.com/krisha-manoj/3-tier-Application.git
cd 3-tier-Application
```

2. Create the Namespace

```bash
kubectl create namespace robot-shop
```

3. Deploy Using Helm

```bash
cd EKS/helm
helm install robot-shop --namespace robot-shop .
```

4. Apply the Ingress Resource

```bash
kubectl apply -f ingress.yaml
```

5. Access the Application

```bash
kubectl get ingress -n robot-shop
```

Access the application using the ALB DNS name from the ADDRESS field.

Running Locally (Docker Compose)
================

To run locally for testing:

```bash
docker-compose pull
docker-compose up
```

The storefront will be available at `http://localhost:8080`

To include load generation:

```bash
docker-compose -f docker-compose.yaml -f docker-compose-load.yaml up
```

Monitoring & Metrics
================

The cart and payment services expose Prometheus metric endpoints on `/metrics`:

- **Cart service:** Counter of items added to cart
- **Payment service:** Counter of items purchased, histogram of cart sizes and values

```bash
curl http://<host>:8080/api/cart/metrics
curl http://<host>:8080/api/payment/metrics
```

Load Generation
================

A separate load generation utility is provided in the `load-gen` directory, built with Python and Locust. See the README in that directory for details on running load tests against the deployed application.

With the help of this project, I learnt :
================

- Setting up EKS clusters with proper IAM roles and access entries
- Configuring AWS Load Balancer Controller with IRSA
- Managing security groups for ALB-to-pod communication
- Troubleshooting node registration and pod scheduling issues
- Handling multi-AZ deployments with proper subnet configuration

