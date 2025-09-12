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

To see the application performance results in the Instana dashboard, you will first need an Instana account. Don't worry a [trial account](https://instana.com/trial?utm_source=github&utm_medium=robot_shop) is free.

---

## Build from Source
To optionally build from source (you will need a newish version of Docker to do this) use Docker Compose. Optionally edit the `.env` file to specify an alternative image registry and version tag; see the official [documentation](https://docs.docker.com/compose/env-file/) for more information.

To download the tracing module for Nginx, it needs a valid Instana agent key. Set this in the environment before starting the build.

```shell
$ export INSTANA_AGENT_KEY="<your agent key>"
Now build all the images.

shell
Copy code
$ docker-compose build
If you modified the .env file and changed the image registry, you need to push the images to that registry

shell
Copy code
$ docker-compose push
Run Locally
You can run it locally for testing.

If you did not build from source, don't worry all the images are on Docker Hub. Just pull down those images first using:

shell
Copy code
$ docker-compose pull
Fire up Stan's Robot Shop with:

shell
Copy code
$ docker-compose up
If you want to fire up some load as well:

shell
Copy code
$ docker-compose -f docker-compose.yaml -f docker-compose-load.yaml up
If you are running it locally on a Linux host you can also run the Instana agent locally, unfortunately the agent is currently not supported on Mac.

There is also only limited support on ARM architectures at the moment.

Marathon / DCOS
The manifests for robotshop are in the DCOS/ directory. These manifests were built using a fresh install of DCOS 1.11.0. They should work on a vanilla HA or single instance install.

You may install Instana via the DCOS package manager, instructions are here: https://github.com/dcos/examples/tree/master/instana-agent/1.9

Kubernetes
You can run Kubernetes locally using minikube or on one of the many cloud providers.

The Docker container images are all available on Docker Hub.

Install Stan's Robot Shop to your Kubernetes cluster using the Helm chart.

To deploy the Instana agent to Kubernetes, just use the helm chart.

Accessing the Store
If you are running the store locally via docker-compose up then, the store front is available on localhost port 8080 http://localhost:8080

If you are running the store on Kubernetes via minikube then, find the IP address of Minikube and the Node Port of the web service.

shell
Copy code
$ minikube ip
$ kubectl get svc web
If you are using a cloud Kubernetes / Openshift / Mesosphere then it will be available on the load balancer of that system.

Load Generation
A separate load generation utility is provided in the load-gen directory. This is not automatically run when the application is started. The load generator is built with Python and Locust. The build.sh script builds the Docker image, optionally taking push as the first argument to also push the image to the registry. The registry and tag settings are loaded from the .env file in the parent directory. The script load-gen.sh runs the image, it takes a number of command line arguments. You could run the container inside an orchestration system (K8s) as well if you want to, an example descriptor is provided in K8s directory. For End-user Monitoring, load is not automatically generated but by navigating through the Robotshop from the browser. For more details see the README in the load-gen directory.

Website Monitoring / End-User Monitoring
Docker Compose
To enable Website Monitoring / End-User Monitoring (EUM) see the official documentation for how to create a configuration. There is no need to inject the JavaScript fragment into the page, this will be handled automatically. Just make a note of the unique key and set the environment variable INSTANA_EUM_KEY and INSTANA_EUM_REPORTING_URL for the web image within docker-compose.yaml.

Kubernetes
The Helm chart for installing Stan's Robot Shop supports setting the key and endpoint URL required for website monitoring, see the README.

Prometheus
The cart and payment services both have Prometheus metric endpoints. These are accessible on /metrics. The cart service provides:

Counter of the number of items added to the cart

The payment services provide:

Counter of the number of items purchased

Histogram of the total number of items in each cart

Histogram of the total value of each cart

To test the metrics use:

shell
Copy code
$ curl http://<host>:8080/api/cart/metrics
$ curl http://<host>:8080/api/payment/metrics
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

Notes for Making Images Visible
Create a folder named screenshots/ in the root of your repository.

Add your images with these exact filenames:

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