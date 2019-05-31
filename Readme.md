# Example blue green deployment using Istio
In this example, I use NodePort for IstioGateway so in somecase, you shoud change NodePort to LoadBalancer.<br>
Install Istio:
* Create `Custom Resource Definition` (CDR) for kubernetes: `kubectl apply -f crds.yml`
* Create `istio-system` namespace: `kubectl create namespace istio-system`
* Create Kiali secrets: `kubectl apply -f kia-secrets.yml`, username and password are encoded in base64, both username and password is admin
* Install istio within kubernetes: in this part, you have two chossen, use my Istio file or create your istio file via helm
  ** Use istio file within project `kubectl apply -f istio.yml`
  ** Create new istio file via helm template, you have to download istio at [https://istio.io/docs/setup/kubernetes/download/](https://istio.io/docs/setup/kubernetes/download/)
  ```shell
  helm template --set kiali.enabled=true install/kubernetes/helm/istio --name istio --namespace istio-system --values install/kubernetes/helm/istio/values-istio-demo.yaml > istio.yml
  ```

Install app deployment and service
* Create blue-green namespace: `kubectl apply -f namespace.yml`
* Blue deployment: `kubectl apply -f blue/deployment.yml`
* Green deployment: `kubectl apply -f green/deployment.yml`
* App service: `kubectl apply -f service.yml`

Configure blue green deployment:
* Install app gateway:`kubectl apply -f app-gateway.yml`
* Create destination rule: `kubectl apply -f des-rule.yml`
* Install virtual service: `kubectl apply -f virtual-service.yml`, host field in virtual services is service name

Access Kiali dashboard: Using portfoward forward traffice to localhost
```json
kubectl -n istio-system port-forward $(kubectl -n istio-system get pod -l app=kiali -o jsonpath='{.items[0].metadata.name}') 20001:20001
```
access http://localhost:20001/kiali