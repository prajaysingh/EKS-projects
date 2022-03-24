This demo creates a user which has only pod reader type access on EKS clusters.

1. Create the ClusterRole and ClusterRoleBinding as specified on "ClusterRole-ClusterRoleBinding.yaml" file and apply it.
   kubectl apply -f ClusterRole-ClusterRoleBinding.yaml

2. Create a test IAM user named test-user and attach the policy mentioned on "IAM-Role-Policy-for-user.json" file.
   This policy allows test-user to View nodes and View workloads for all clusters

3. Add the entry on aws-auth configmap as mentioned on "aws-auth-configmap-addition" file.

4. Test your setup
   
   export AWS_PROFILE=test

  aws sts get-caller-identity

  aws eks update-kubeconfig --name ManualCluster --region us-west-2

  k get po -A

  NAMESPACE     NAME                                            READY   STATUS    RESTARTS   AGE
  kube-system   aws-load-balancer-controller-5b54dbc757-ffh7h   1/1     Running   0          2d
  kube-system   aws-load-balancer-controller-5b54dbc757-lr4ct   1/1     Running   0          2d
  kube-system   aws-node-6zf5r                                  1/1     Running   0          16d
  kube-system   aws-node-msvc4                                  1/1     Running   0          16d
  kube-system   cluster-autoscaler-5896d74f8b-6f8pq             1/1     Running   1          16d
  kube-system   coredns-66fd5f54db-hllm9                        1/1     Running   0          16d
  kube-system   coredns-66fd5f54db-sj7nh                        1/1     Running   0          16d
  kube-system   ebs-csi-controller-59df778bd9-f6sn8             5/5     Running   0          16d
  kube-system   ebs-csi-controller-59df778bd9-knhxc             5/5     Running   1          16d
  kube-system   ebs-csi-node-4gw7h                              3/3     Running   0          16d
  kube-system   ebs-csi-node-sxdp2                              3/3     Running   0          16d
  kube-system   kube-proxy-g4zrh                                1/1     Running   0          16d
  kube-system   kube-proxy-xshlj                                1/1     Running   0          16d

  k get sa -A

  Error from server (Forbidden): serviceaccounts is forbidden: User "test-user" cannot list resource "serviceaccounts" in API group "" at the cluster scope
