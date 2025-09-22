Three Tier Architecture Deployment on AWS EKS

Stan's Robot Shop is a sample microservice application you can use as a sandbox to test and learn containerised application orchestration and monitoring techniques. It is not intended to be a comprehensive reference example of how to write a microservices application, although you will better understand some of those concepts by playing with Stan's Robot Shop. To be clear, the error handling is patchy and there is not any security built into the application.

You can get more detailed information from my blog post about this sample microservice application.

This sample microservice application has been built using these technologies:

    NodeJS (Express)
    Java (Spring Boot)
    Python (Flask)
    Golang
    PHP (Apache)
    MongoDB
    Redis
    MySQL (Maxmind data)
    RabbitMQ
    Nginx
    AngularJS (1.x)

The various services in the sample application already include all required Instana components installed and configured. The Instana components provide automatic instrumentation for complete end to end tracing, as well as complete visibility into time series metrics for all the technologies.

To see the application performance results in the Instana dashboard, you will first need an Instana account. Don't worry a trial account is free.
Build from Source

To optionally build from source (you will need a newish version of Docker to do this) use Docker Compose. Optionally edit the .env file to specify an alternative image registry and version tag; see the official documentation for more information.

To download the tracing module for Nginx, it needs a valid Instana agent key. Set this in the environment before starting the build.

$ export INSTANA_AGENT_KEY="<your agent key>"

Now build all the images.

$ docker-compose build

If you modified the .env file and changed the image registry, you need to push the images to that registry

$ docker-compose push

Run Locally

You can run it locally for testing.

If you did not build from source, don't worry all the images are on Docker Hub. Just pull down those images first using:

$ docker-compose pull

Fire up Stan's Robot Shop with:

$ docker-compose up

If you want to fire up some load as well:

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

$ minikube ip
$ kubectl get svc web

If you are using a cloud Kubernetes / Openshift / Mesosphere then it will be available on the load balancer of that system.
Load Generation

A separate load generation utility is provided in the load-gen directory. This is not automatically run when the application is started. The load generator is built with Python and Locust. The build.sh script builds the Docker image, optionally taking push as the first argument to also push the image to the registry. The registry and tag settings are loaded from the .env file in the parent directory. The script load-gen.sh runs the image, it takes a number of command line arguments. You could run the container inside an orchestration system (K8s) as well if you want to, an example descriptor is provided in K8s directory. For End-user Monitoring ,load is not automatically generated but by navigating through the Robotshop from the browser .For more details see the README in the load-gen directory.
Website Monitoring / End-User Monitoring
Docker Compose

To enable Website Monioring / End-User Monitoring (EUM) see the official documentation for how to create a configuration. There is no need to inject the JavaScript fragment into the page, this will be handled automatically. Just make a note of the unique key and set the environment variable INSTANA_EUM_KEY and INSTANA_EUM_REPORTING_URL for the web image within docker-compose.yaml.
Kubernetes

The Helm chart for installing Stan's Robot Shop supports setting the key and endpoint url required for website monitoring, see the README.
Prometheus

The cart and payment services both have Prometheus metric endpoints. These are accessible on /metrics. The cart service provides:

    Counter of the number of items added to the cart

The payment services provides:

    Counter of the number of items perchased
    Histogram of the total number of items in each cart
    Histogram of the total value of each cart

To test the metrics use:

$ curl http://<host>:8080/api/cart/metrics
$ curl http://<host>:8080/api/payment/metrics


# Project Screenshots

This repository contains screenshots demonstrating various pages, AWS resources, and monitoring tools used in the project. Each image highlights a specific component or feature. All images are stored in the `screenshots/` folder.

## Images

| Image | Description | Path |
|-------|-------------|------|
| al-page.png | Screenshot of the **AL (Application Load) page** showing load balancer details or configurations. | `screenshots/al-page.png` |
| aws-acm.png | Screenshot of **AWS ACM (Certificate Manager)** displaying SSL/TLS certificate configurations. | `screenshots/aws-acm.png` |
| aws-hosted-zone.png | Screenshot of **AWS Hosted Zone** in Route 53 showing DNS records and domain setup. | `screenshots/aws-hosted-zone.png` |
| cart-page.png | Screenshot of the **Cart page** in the application demonstrating shopping cart functionality. | `screenshots/cart-page.png` |
| eks-cluster.png | Screenshot of the **EKS Cluster** in AWS showing cluster overview and nodes. | `screenshots/eks-cluster.png` |
| grafana.png | Screenshot of the **Grafana dashboard** for monitoring metrics and visualizations. | `screenshots/grafana.png` |
| grafana-cluster.png | Screenshot of Grafana showing metrics specifically for the **cluster performance**. | `screenshots/grafana-cluster.png` |
| login-page.png | Screenshot of the **Login page** of the application showing authentication flow. | `screenshots/login-page.png` |
| order-page.png | Screenshot of the **Order page** demonstrating order placement and details. | `screenshots/order-page.png` |
| prometheus.png | Screenshot of the **Prometheus monitoring dashboard** for collecting metrics. | `screenshots/prometheus.png` |
| robot-page.png | Screenshot of the **Robot page** showcasing automation or robotic operations in the app. | `screenshots/robot-page.png` |

## Purpose

These images are intended to provide visual references for:

- AWS infrastructure setup
- Application pages and user interfaces
- Monitoring and observability tools like Grafana and Prometheus
