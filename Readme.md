# Example blue green deployment using Istio
In this example, I use NodePort for IstioGateway so in somecase, you shoud change NodePort to LoadBalancer.<br>
Install Istio:
* Create `Custom Resource Definition` (CDR) for kubernetes: `kubectl apply -f crds.yml`
* Create Kiali secrets: `kubectl apply -f kia-secrets.yml`, username and password are encoded in base64, both username and password is admin
* Install istio within kubernetes: `kubectl apply -f istio.yml`

Install app deployment and service
* Create blue-green namespace: `kubectl apply -f namespace.yml`
* Blue deployment: `kubectl apply -f blue/deployment.yml`
* Green deployment: `kubectl apply -f green/deployment.yml`
* App service: `kubectl apply -f service.yml`

Configure blue green deployment:
* Install app gateway:`kubectl apply -f app-gateway.yml`
* Create destination rule: `kubectl apply -f des-rule.yml`
* Install virtual service: `kubectl apply -f virtual-service.yml`, host field in virtual services is service name