

This setup is to demo the use of efs.csi.aws.com provisioner using storage class and PVC.

First of all we need to finish following steps before using this manifests:

1) Create an IAM policy and role

2) Install the Amazon EFS driver

3) Create an Amazon EFS file system (This step includes creating Security Group; creating an Amazon EFS file system and creating mount targets.)


Requirement: Please follow this link and complete steps 1,2,3 as mentioned above. (https://docs.aws.amazon.com/eks/latest/userguide/efs-csi.html)

Steps:

    kubectl apply -f storageclass.yaml (This will create storage class named efs-sc)

    kubectl apply -f pvc.yaml (This will crate PVC named mongodb-pvc-efs using storage class efs-sc)

The above steps will dynamically provision PV which can be verified using kubectl get pv

    kubectl apply -f pod.yaml (This will create mongodb pd which mounts the previously generated pv)


Issue observed:
Unlike using ebs provisioner, to make the settings work, I need to perform following tweaks
a) Storage class needed to have "directoryPerms" section
b) In pod manifest, I needed to add command: ["mongod"] as without it, I was getting error from pod logs "chown: changing ownership of '/data/db': Operation not permitted"

Testing:

Exec inside the mongodb pod and run following commands to store value pj in db.

--> kubectl exec -it mongodb mongo

--> use mystore

--> db.foo.insert({name:'pj'})

--> db.foo.find() (This should give the field named pj)

--> exit

Now delete the pod and recreate it again. We would run the following commands to verify the field pj still exists even after deleting the pod

--> kubectl exec -it mongodb mongo

--> use mystore

--> db.foo.find() (This should give the field named pj)

--> exit
