This is a demo application which is used for testing the use of single AWS Application Load Balancer (ALB) ingress for two different services in two different namespaces.
Prerequisite:
1) You need to have AWS account
2) ALB controller needs to be installed first. You can follow this link for that  (https://docs.aws.amazon.com/eks/latest/userguide/aws-load-balancer-controller.html)

Steps:
1) Create two namespaces in your EKS cluster:
  a) kubectl create ns apple
  b) kubectl create ns banana
  
 Steps 2 and 3 will create apple and banana service and pod for each of those service.
 
 2) Run: kubectl apply -f ingress-alb-backend-service-apple.yaml
 3) Run: kubectl apply -f ingress-alb-backend-service-banana.yaml
 
 Step 4 and 5 will create ingress resource for apple and banana namespace.It uses path based routing for the ingress resource to go to different service based on different http path.
 
 4) Run: kubectl apply -f ingress-resource-alb-apple.yaml
 5) Run: kubectl apply -f ingress-resource-alb-banana.yaml
 
 Verification:
 kubectl get ingress -A 
 
 This should yield single ALB for two different namespaces.
