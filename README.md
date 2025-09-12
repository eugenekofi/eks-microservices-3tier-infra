# eks-microservices-3tier-loadbalancer-ingress-infra
Infrastructure setup for deploying a 3-tier microservices application on Amazon EKS. This project demonstrates production-grade practices including AWS Load Balancer Controller, Ingress routing, custom domain integration with GoDaddy, and TLS/HTTPS termination using ACM.

# Three Tier Architecture Deployment on AWS EKS

Stan's Robot Shop is a sample microservice application you can use as a sandbox to test and learn containerised application orchestration and monitoring techniques. It is not intended to be a comprehensive reference example of how to write a microservices application, although you will better understand some of those concepts by playing with Stan's Robot Shop. To be clear, the error handling is patchy and there is not any security built into the application.

You can get more detailed information from my [blog post](https://www.instana.com/blog/stans-robot-shop-sample-microservice-application/) about this sample microservice application.

This sample microservice application has been built using these technologies:

- NodeJS ([Express](http://expressjs.com/))
- Java ([Spring Boot](https://spring.io/))
- Python ([Flask](http://flask.pocoo.org))
- Golang
- PHP (Apache)
- MongoDB
- Redis
- MySQL ([Maxmind](http://www.maxmind.com) data)
- RabbitMQ
- Nginx
- AngularJS (1.x)

The various services in the sample application already include all required Instana components installed and configured. The Instana components provide automatic instrumentation for complete end-to-end [tracing](https://docs.instana.io/core_concepts/tracing/), as well as complete visibility into time series metrics for all the technologies.

To see the application performance results in the Instana dashboard, you will first need an Instana account. Don't worry, a [trial account](https://instana.com/trial?utm_source=github&utm_medium=robot_shop) is free.

---

## Build from Source
To optionally build from source (you will need a newish version of Docker to do this) use Docker Compose. Optionally edit the `.env` file to specify an alternative image registry and version tag; see the official [documentation](https://docs.docker.com/compose/env-file/) for more information.

To download the tracing module for Nginx, it needs a valid Instana agent key. Set this in the environment before starting the build:

```bash
export INSTANA_AGENT_KEY="<your agent key>"
Now build all the images:

bash
Copy code
docker-compose build
If you modified the .env file and changed the image registry, you need to push the images to that registry:

bash
Copy code
docker-compose push
Run Locally
You can run it locally for testing. If you did not build from source, all the images are on Docker Hub. Pull them first:

bash
Copy code
docker-compose pull
Fire up Stan's Robot Shop:

bash
Copy code
docker-compose up
Fire up with load generation:

bash
Copy code
docker-compose -f docker-compose.yaml -f docker-compose-load.yaml up
Kubernetes / DCOS / Other Platforms
Marathon / DCOS: Manifests in DCOS/. Works on DCOS 1.11.0.

Kubernetes: Use minikube or cloud provider. Install with Helm chart in K8s/helm/.

Instana Agent: Install via Helm for Kubernetes or DCOS package manager.

Accessing the Store
Local: http://localhost:8080

Minikube: Use Minikube IP + NodePort:

bash
Copy code
minikube ip
kubectl get svc web
Cloud K8s: Access via LoadBalancer URL.

Load Generation
Provided in load-gen/. Built with Python & Locust. Build & run:

bash
Copy code
./build.sh [push]
./load-gen.sh <args>
Website Monitoring / End-User Monitoring
Docker Compose: Set INSTANA_EUM_KEY & INSTANA_EUM_REPORTING_URL in docker-compose.yaml.

Kubernetes: Configure via Helm chart.

Prometheus Metrics
Cart & payment services expose metrics on /metrics:

bash
Copy code
curl http://<host>:8080/api/cart/metrics
curl http://<host>:8080/api/payment/metrics
Screenshots
Here’s a visual tour of the AWS infrastructure and application pages.
(All images are located in the screenshots/ folder at the root of the repo.)

AWS Infrastructure Pages
Hosted Zone	AWS ACM	EKS Cluster

Descriptions:

Hosted Zone: Shows DNS records in Route 53 for your custom domain.

AWS ACM: Displays TLS/SSL certificate details for HTTPS termination.

EKS Cluster: Overview of the deployed Amazon EKS cluster with nodes, namespaces, and workloads.

Application Pages
Cart Page	Login Page	Order Page

Descriptions:

Cart Page: Items in the user cart, pricing, and checkout workflow.

Login Page: Mobile/desktop-friendly login form for user authentication.

Order Page: Displays order history, status, and detailed information for each order.

Robot Page	AI Page

Descriptions:

Robot Page: Interface for robot management, automation, or monitoring dashboard.

AI Page: Interface showing AI-powered analytics, recommendations, or simulations.

Notes for Images
Create a folder screenshots/ at repo root.

Add these images exactly:

pgsql
Copy code
aws-hosted-zone.png
aws-acm.png
eks-cluster.png
cart-page.png
login-page.png
order-page.png
robot-page.png
ai-page.png