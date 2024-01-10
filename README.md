# AWS_EKS

**Create an EKS cluster and deploy 2048 game into that cluster**

**Step 1: Create an EKS cluster**


1.	Name: <yourname>-eks-cluster-1
2.	Use K8S version 1.25
3.	Create an IAM role 'eks-cluster-role' with 1 policy attached:
-	 AmazonEKSClusterPolicy
4.	Create another IAM role 'eks-node-grp-role' with 3 policies attached: 
5.	(Allows EC2 instances to call AWS services on your behalf.)
-	AmazonEKSWorkerNodePolicy
-	AmazonEC2ContainerRegistryReadOnly
-	AmazonEKS_CNI_Policy
6.	Choose default VPC, Choose 2 or 3 subnets
7.	Choose a security group which open the ports 22, 80, 8080
8.	cluster endpoint access: public

Click 'Create'. This process will take 10-12 minutes. Wait till your cluster shows up as Active. 


**Step 2 Add Node Groups to our cluster**

1.	Now, lets add the worker nodes where the pods can run
2.	Open the cluster > Compute > Add NodeGrp
3.	Name: <yourname>-eks-nodegrp-1 
4.	Select the role you already created
5.	Leave default values for everything else
6.	AMI - choose the default 1 (Amazon Linux 2)
7.	change desired/minimum/maximum to 1 (from 2)
8.	Enable SSH access. Choose a security group which allwos 22, 80, 8080
9.	Choose default values for other fields 
10.	Click on create



# Step 3: Authenticate to this cluster**


**Open cloudshell Type on your AWS CLI window**
To get your account and user id details
    aws sts get-caller-identity


**Create a  kubeconfig file where it stores the credentials for EKS:**
Kubeconfig configuration allows you to connect to your cluster using the kubectl command line.

    **aws eks update-kubeconfig --region <<region-code >> --name <<my-cluster>>**
    ex: aws eks update-kubeconfig --region us-east-1 --name unus-eks-cluster-1 


**To Check the pods**
    kubectl get nodes

**Install nano editor in cloudshell. We will need this in the next task**
    sudo yum install nano -y



# Step 4: Create a new POD in EKS for the 2048 game**

**Apply the config file to create the pod**
    kubectl apply -f 2048-pod.yaml
    #pod/2048-pod created

 **View the newly created pod**
    kubectl get pods


# Step 5: Setup Load Balancer Service


**Apply the config file**

    kubectl apply -f 2048-svc.yaml

**View details of the modified service**

    kubectl describe svc 2048-svc


Go to EC2 console, get the DNS name of ELB and paste the DNS into address bar of the browser.
It will show the 2048 game. You can play. (need to wait for 2-3 minutes for the setup to be complete)



# Clean up all the resources created in the task

kubectl get pods
kubectl delete -f 2048-pod.yaml

kubectl get services
kubectl delete -f mygame-svc.yaml




