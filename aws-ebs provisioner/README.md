This setup is to demo the use of in-tree aws-ebs provisioner using storage class and PVC.

Steps:

1) kubectl apply -f storageclass.yaml (This will create storage class named test-gp2)

2) kubectl apply -f pvc.yaml (This will crate PVC named mongodb-pvc-aws-ebs using storage class test-gp2)

The above steps will dynamically provision PV which can be verified using kubectl get pv

3) kubectl apply -f pod.yaml (This will create mongodb pd which mounts the previously generated pv)



Testing:

Exec inside the mongodb pod and run following commands to store value pj in db.

--> kubectl exec -it mongodb mongo

--> use mystore

--> db.foo.insert({name:'pj'})

--> db.foo.find() (This should give the field named pj)

--> exit

Now delte the pod and recreate it again. We would run the following commands to verify the field pj still exists even after deleting the pod

--> kubectl exec -it mongodb mongo

--> use mystore

--> db.foo.find() (This should give the field named pj)

--> exit
