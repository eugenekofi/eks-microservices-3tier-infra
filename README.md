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
docker-compose build

docker-compose push

docker-compose pull
docker-compose up

docker-compose -f docker-compose.yaml -f docker-compose-load.yaml up

minikube ip
kubectl get svc web

./build.sh [push]
./load-gen.sh <args>

curl http://<host>:8080/api/cart/metrics
curl http://<host>:8080/api/payment/metrics

aws-hosted-zone.png
aws-acm.png
eks-cluster.png
cart-page.png
login-page.png
order-page.png
robot-page.png
ai-page.png
git add screenshots/
git commit -m "Add portfolio screenshots"
git push

---

✅ **Key points to make images live:**

1. `README.md` must be at repo root.  
2. `screenshots/` folder must be at **repo root**, alongside `README.md`.  
3. Image filenames must match exactly (case-sensitive).  
4. Commit and push images to GitHub.  

---

If you want, I can **also add a “Live Demo / Portfolio” section at the top** with clickable links so it looks more polished for recruiters viewing your GitHub portfolio.  

Do you want me to do that next?

