Game 2048 on AWS EKS (Fargate) — Setup Steps



This document explains how the Game 2048 Kubernetes application was deployed on AWS EKS with Fargate using Terraform and Kubernetes manifests.



1\. Prerequisites



Before starting, ensure you have the following installed:



AWS CLI

&nbsp;(configured with your credentials)



kubectl



Terraform



Helm

&nbsp;(optional, if using Helm charts)



Git (for version control)



2\. Clone the Repository

git clone <your-github-repo-url>

cd terraform-ci-cd-infrastructure

3\. Terraform Infrastructure Deployment



Terraform is used to create the VPC, subnets, security groups, and EKS cluster.



Initialize Terraform



terraform init



Check the Plan



terraform plan



Apply the Plan



terraform apply



This will create:



VPC with public/private subnets



Security groups



EKS cluster on Fargate



IAM roles for Kubernetes service accounts



✅ Make sure Terraform shows “Apply complete!”



4\. Configure kubectl



After EKS creation, update kubectl configuration:



aws eks --region <your-region> update-kubeconfig --name <cluster-name>



Check nodes:



kubectl get nodes



You should see multiple Fargate nodes (Ready status)



5\. Deploy the Application



Create a Namespace



kubectl create namespace game-2048



Apply Deployment and Service



kubectl apply -f k8s/deployment.yaml -n game-2048

kubectl apply -f k8s/service.yaml -n game-2048



Apply Ingress



kubectl apply -f k8s/ingress.yaml -n game-2048

6\. Verify Deployment



Check Pods



kubectl get pods -n game-2048



Check Services



kubectl get svc -n game-2048



Check Ingress / ALB



kubectl get ingress -n game-2048



Open the ADDRESS shown in the Ingress output to access the app in your browser.



7\. Screenshots for Portfolio



Capture these screenshots for documentation:



kubectl-nodes.png → kubectl get nodes



pods-running.png → kubectl get pods -n game-2048



alb-created.png → AWS Console → EC2 → Load Balancers → show the ALB



ingress-output.png → kubectl get ingress -n game-2048



Crop to show only relevant sections; highlight key info like pod statuses, node statuses, and ALB URL.



8\. Cleanup (Optional)



To remove all resources and avoid costs:



terraform destroy



Be careful — this will delete the entire EKS cluster and associated resources.



9\. Notes / Observations



Multiple pods → expected (replicas for high availability)



Multiple nodes → normal with Fargate (each pod group gets a virtual node)



Ingress automatically provisions ALB for external access

