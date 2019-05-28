Install Istio:
* Install `Custom Resource Definition` (CDR) for kubernetes: `kubectl apply -f crds.yml`
* Install istio within kubernetes: `kubectl apply -f istio.yml`

Install app deployment and service
* Blue deployment: `kubectl apply -f blue/deployment.yml`
* Green deployment: `kubectl apply -f green/deployment.yml`
* App service: `kubectl apply -f service.yml`

Configure blue green deployment:
* Install app gateway:`kubectl apply -f app-gateway.yml`
* Install destination rule: `kubectl apply -f des-rule.yml`