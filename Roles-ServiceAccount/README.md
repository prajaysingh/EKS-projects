create a role service-reader in namespace foo that has permission to read services.
k apply -f service-reader.yaml 

create a role binding that associates the role service-reader with service account default in foo namespace.
k apply -f service-reader-role-binding.yaml

Run a sample pod to test the access.
kubectl run test --image=luksa/kubectl-proxy -n foo
Exec into pod
kubectl exec -it test -n foo sh

Lets check if pod in this namespace can access the services get,list by running following command inside the container
curl localhost:8001/api/v1/namespaces/foo/services
 
Yes it does!!
