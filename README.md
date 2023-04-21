# Amazon-EKS-cluster
Creating an EKS Cluster in 15 Minutes: An AWS QuickStart Guide for Kubernetes Deployment

Amazon Elastic Kubernetes Service (EKS) is a managed Kubernetes service that simplifies the process of deploying, managing, and scaling containerized applications.


It consists of a control plane, which manages the Kubernetes API server and other core Kubernetes components, and worker nodes, which run the containerized workloads. However, the downside is that you have no access to the master nodes and are unable to customize the control plane in any way.

This will be the start of many more article on this topic. I plan to produce further content integrating CI/CD to the EKS clusters and expanding on different tools you could use. To further enhance your cluster.

Requirements:

AWS Account with IAM Permissions

EKS pricing starts at around $0.10 per hour. In this cluster setup we will try to keep the price as low as possible. Also make sure to delete the cluster afterwards. For more information check the documentation. This makes it so that EKS is easy to set up and use, making it a popular choice for those who want to practice Kubernetes.

Example of the pricing calculator below

Create an IAM role for your EKS Cluster.
To create your Amazon EKS cluster role in the IAM console
Open the IAM console at https://console.aws.amazon.com/iam/.

Choose Roles, then Create role.

Under Trusted entity type, select AWS service.

<img width="1226" alt="Screenshot 2023-04-21 at 19 43 53" src="https://user-images.githubusercontent.com/67044030/233724186-12c0bf7a-6343-4fb4-a1c7-a0fe48ff54b8.png">

From the Use cases for other AWS services dropdown list, choose EKS.

Choose EKS — Cluster for your use case, and then choose Next.

<img width="1226" alt="Screenshot 2023-04-21 at 19 45 04" src="https://user-images.githubusercontent.com/67044030/233724428-ea3a4f56-cfb1-489d-842c-be63fa19cf36.png">

On the Add permissions tab, choose Next.
<img width="1238" alt="Screenshot 2023-04-21 at 19 46 13" src="https://user-images.githubusercontent.com/67044030/233724555-75a69afc-a02a-4e35-9d99-2f001c789794.png">

For Role name, enter a unique name for your role, such as eksClusterRole.

<img width="1210" alt="Screenshot 2023-04-21 at 19 47 13" src="https://user-images.githubusercontent.com/67044030/233724716-516c54bd-20fe-477a-9b28-20f196d4f35c.png">

For Description, enter descriptive text such as Amazon EKS — Cluster role.

Choose Create role.

<img width="937" alt="Screenshot 2023-04-21 at 19 47 41" src="https://user-images.githubusercontent.com/67044030/233724965-3cb74450-3c12-4044-99a9-ef6d8d93b5e6.png">

2. Configuring the EKS Cluster

Choose Name

<img width="1204" alt="Screenshot 2023-04-21 at 19 49 21" src="https://user-images.githubusercontent.com/67044030/233725258-84ad2285-5401-4426-a748-26bc0c93563c.png">

Stick with Kubernetes Version 1.25 unless you need to try a different version

Select your cluster service role which you just create.

Click next


Configure the networking. We are going to keep really simple here for now just to test the cluster.

Select Default VPC

For the subnets we will pick any two.

<img width="1269" alt="Screenshot 2023-04-21 at 20 17 35" src="https://user-images.githubusercontent.com/67044030/233725472-1e246c70-b570-4f62-9b42-51868a5a3555.png">

And again for the Security groups pick the default security group.

Leave rest as default and click on Next


For the other steps you can leave as default. Your final page you should look like this:


Click Create

Now the EKS cluster can take around 10 minutes for it to start. In the meantime we can configure our node IAM Role.

3. Create the Node IAM Role

Similar to how we done before the node also needs permissions to make AWS APIs calls on your behalf. Before we can configure a node we need the permissions for it.

To create your Amazon EKS node role in the IAM console

Open the IAM console at https://console.aws.amazon.com/iam/.

In the left navigation pane, choose Roles.

On the Roles page, choose Create role.

On the Select trusted entity page, do the following:

In the Trusted entity type section, choose AWS service.

Under Use case, choose EC2.

Choose Next.

On the Add permissions page, do the following:

In the Filter policies box, enter AmazonEKSWorkerNodePolicy.

Select the check box to the left of AmazonEKSWorkerNodePolicy in the search results.

Choose Clear filters.

In the Filter policies box, enter AmazonEC2ContainerRegistryReadOnly.

Select the check box to the left of AmazonEC2ContainerRegistryReadOnly in the search results.

Choose Clear filters.

In the Filter policies box, enter AmazonEKS_CNI_Policy .

Select the check box to the left of AmazonEKS_CNI_Policy in the search results

Choose Next.

On the Name, review, and create page, do the following:

For Role name, enter a unique name for your role, such as AmazonEKSNodeRole.

For Description, replace the current text with descriptive text such as Amazon EKS — Node role.

Under Add tags (Optional), add metadata to the role by attaching tags as key–value pairs. For more information about using tags in IAM, see Tagging IAM Entities in the IAM User Guide.

Choose Create role.

4. Configure the Node

Click on your cluster, go to compute and Add node group.

Give it a name and select the Node IAM role you just created.

Leave the rest as default and Click on next.


For our demo purposes we will just keep this to two nodes. Leave the rest as default.


Select your two subnets you picked for your VPC earlier.


Go to next and click on Create.

Now give it a few minute and your node should be up and running.

We can now create a pod and test our cluster. Lets try this in AWS Cloud Shell

Go the AWS Cloud Shell Icon (near the alarm bell icon). It should start up the AWS CLI


Run this command to add your cluster.

Replace example_region with the name of the region for like us-east-1.

Replace cluster_name with the name of the cluster like Demo. Be aware it is caps lock sensitive.

aws eks --region example_region update-kubeconfig --name cluster_name
To see your nodes that you created do the command below

Kubectl get nodes 
To create your own pod in the CLI with a personal label.

kubectl run firstpod --image=nginx --labels awseks=done
To view your pod and your label.

kubectl get pod 
kubectl get pod --show-labels
Check out the Kubernetes documentation to implement other deployments and designs you would like to test out.

