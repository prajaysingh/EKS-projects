This demo is for using EFS with EKS Fargate.

I followed this blog https://aws.amazon.com/blogs/aws/new-aws-fargate-for-amazon-eks-now-supports-amazon-efs/

Notes:

1) No need of EFS CSI driver as it is installed in the Fargate stack and support for EFS is provided out of the box.

2) Check if CSIDriver Object is installed by default

    k get csidriver
    
    NAME              ATTACHREQUIRED   PODINFOONMOUNT   STORAGECAPACITY   TOKENREQUESTS   REQUIRESREPUBLISH   MODES        AGE
    
    efs.csi.aws.com   false            false            false             <unset>         false               Persistent   78d

    If not installed you need to create csi object as mentioned in blog.

3) You need to create EFS in console for this to work and get the filesystem id. This is required when create pv manifest.
 
4) Make sure the VPC for EFS and Fargate Cluster is same

5) The Security group for file system mount target (In EFS console go to Network Section) doesnot have inbound on port 2049 for task security group (I used cluster security group)
    Hence I modified Mount target SG to include:
    Type: Custom TCP or NFS 
    Protocol: TCP
    Port Range: 2049
    Source: Cluster SG.
