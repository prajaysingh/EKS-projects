This demo is for using single EFS with EKS Fargate. In this setup two applications have their own separate directories with separate permissions in a single EFS by making use of 2 access points.

For this to happen:

1) You should create two access points in EFS pointing to two different paths and permissions. In my setup I have created 2 access points with following configuration

    a) Name: app1
    
    
       Root directory path: /app1
       POSIX user:
       
        User id: 2000
        
        Group id: 2000
        
       Root directory creation permissions:
       
         User id: 2000
         
         Group id: 2000
         
         Permissions: 0750
         

    b) Name: app2
    
    
       Root directory path: /app2
       POSIX user:
       
         User id: 3000
         
         Group id: 3000
         
       Root directory creation permissions:
       
         User id: 3000
         
         Group id: 3000
         
         Permissions: 0750
         
   
   2) Apply the manifests that creates storage-class,pvc,pv,pod for 2 different applications. Replace filesystem id and access point id with your values in PersistentVolume section
   
        kubectl apply -f App1.yml
        
        kubectl apply -f App2.yml
        
        
   3) For verification, you can exec to any pod and see the permission is matching or not using ls -la /data/
   
   
   4) You can also mount the EFS to your EC2 instance to do some testing using following command. This will mount EFS to directory ~/efs-mount-point
      Replace <file-system-id> and <aws-region> with your values.
    
        mkdir ~/efs-mount-point
    
        sudo mount -t nfs -o nfsvers=4.1,rsize=1048576,wsize=1048576,hard,timeo=600,retrans=2,noresvport <file-system-id>.efs.<aws-region>.amazonaws.com:/   ~/efs-mount-point   
    
        df -kh --> This should show your efs mounted to the directory
    
  
   5) Change your user id to match app1 for instance
    
        sudo useradd -u 2000 user1
    
        sudo su -s /bin/bash user1 --> this will change your terminal to point to user1
    
        cd ~/efs-mount-point/
    
        ls -la --> You should see 2 directory app1 and app2
    
        cd app1 --> success 
    
        cd app2 --> failure as invalid permission
    
        
        
