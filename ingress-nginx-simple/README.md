This demo creates a NLB (Network Load Balancer) to expose the NGINX Ingress controller behind a Service.

Steps:

Follow Steps 1 and 2 to create a nginx ingress contoller and NLB

1) curl  https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v1.0.4/deploy/static/provider/aws/deploy.yaml --output nginx-ingress.yaml
2) k apply -f nginx-ingress.yaml

Check if ingress-nginx-controller pod is running by using kubectl get po -n ingress-nginx.

Check in console if NLB is created

If the above check is valid, we are good to deploy sample application:

3) kubectl apply -f ingress-nginx-backend-service-apple.yaml (This create apple service and backend pod)
4) kubectl apply -f ingress-nginx-backend-service-banana.yaml (This create banana service and backend pod)
5) kubectl apply -f ingress-resource-nginx.yaml (This will create ingress resource to expose previous created services. Replace <NLB-DNS-NAME> with your NLB's.
  This ingress performs path based routing).
