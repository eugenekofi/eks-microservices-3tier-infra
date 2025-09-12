1. Architecture Overview


Figure 1: Three-tier microservices architecture.

2. AWS EKS Cluster


Figure 2: EKS cluster hosting all microservices.

3. Networking & Ingress


Figure 3: AWS Route 53 hosted zone.


Figure 4: Kubernetes ingress configuration.

4. Certificates & Security


Figure 5: SSL certificates managed by AWS ACM.

5. Monitoring with Instana


Figure 6: Instana dashboard showing end-to-end traces.

6. Microservices Overview
Cart Service


Handles shopping cart operations.

Order Service


Handles purchase orders.

Login / User Service


Handles user login and authentication.

7. Build & Run Locally
Build from Source (Optional)
export INSTANA_AGENT_KEY="<your-agent-key>"
docker-compose build
docker-compose push  # optional if using custom registry

Run from Docker Hub
docker-compose pull
docker-compose up
docker-compose -f docker-compose.yaml -f docker-compose-load.yaml up


Web frontend: http://localhost:8080

⚠️ Instana agent only supported on Linux; limited support on ARM and Mac.

8. Kubernetes / Helm Deployment
Deploy Instana Agent
helm repo add instana https://agents.instana.io/helm
helm repo update
helm install instana-agent instana/agent \
  --set agentKey=<your-agent-key> \
  --namespace instana --create-namespace

Deploy Robot Shop
helm repo add robotshop https://path-to-robotshop-helm-chart
helm repo update

helm install robotshop robotshop/stan-robot-shop \
  --namespace robotshop --create-namespace \
  --set instana.eumKey=<your-eum-key> \
  --set instana.eumReportingUrl=<instana-eum-url>

Access the Store
kubectl get svc -n robotshop


NodePort: http://<node-ip>:<node-port>

LoadBalancer: AWS ELB DNS name

9. Load Generation

A separate load generator is provided in the load-gen directory (Python + Locust):

cd load-gen
./build.sh push
./load-gen.sh --host=http://<robotshop-web-service-url> --users 100 --spawn-rate 10


Optional end-user monitoring (EUM) through web navigation.

10. Prometheus Metrics

Cart Service: /metrics

Counter: number of items added

Payment Service: /metrics

Counter: number of items purchased

Histogram: number of items per cart

Histogram: total value of each cart

kubectl port-forward svc/cart 8080:8080 -n robotshop
curl http://localhost:8080/api/cart/metrics

kubectl port-forward svc/payment 8081:8080 -n robotshop
curl http://localhost:8081/api/payment/metrics

11. Summary

Multi-language microservices on AWS EKS

Instana monitoring with end-to-end tracing

Kubernetes Ingress, ACM SSL, and Route 53 Hosted Zone

Realistic workflows: Cart, Orders, Login

Optional load generation for performance testing

Images in Repo
images/
    acm.png
    ai.png
    cart.png
    cluster.png
    hostedzone.png
    ingress.png
    login.png
    order.png
    robot.png