# Terraform lab

This folder teaches Infrastructure as Code by provisioning the AWS VPC and EKS cluster used by the production-style LabTrack deployment.

The module-based approach keeps the lesson focused on Terraform concepts: providers, variables, modules, state, plan/apply, outputs and dependency graphs.

> Cost warning: an EKS cluster and NAT gateway can incur AWS charges. Destroy classroom infrastructure when finished.

```bash
terraform init
terraform fmt -recursive
terraform validate
terraform plan -out=labtrack.tfplan
terraform apply labtrack.tfplan
aws eks update-kubeconfig --region ap-southeast-2 --name labtrack-eks
kubectl get nodes
terraform destroy -auto-approve
```

I tested the full lifecycle in `ap-southeast-2`. The plan contained 54 resources, the EKS cluster became Active, and its two `t3.small` workers became Ready. The final destroy removed all 53 resources in Terraform state. A follow-up check returned no LabTrack EKS cluster, VPC, or active NAT gateway.

IRSA is disabled because the new AWS account experience blocks creation of an IAM OpenID Connect provider. No AWS credentials or Terraform state files are committed.
