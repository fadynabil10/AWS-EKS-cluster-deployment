
# Amazon EKS Cluster Setup and Management with eksctl

This guide provides steps to create, manage, and delete an Amazon EKS cluster using `eksctl`. Follow these steps to set up a cluster with an IAM OIDC provider, a managed Spot instance node group, and to manage your EKS cluster resources effectively.

## Table of Contents
1. [Create the EKS Cluster](#1-create-the-eks-cluster)
2. [List Available EKS Clusters](#2-list-available-eks-clusters)
3. [Enable IAM OIDC Provider for EKS Cluster](#3-enable-iam-oidc-provider-for-eks-cluster)
4. [Create the Spot Instance Node Group](#4-create-the-spot-instance-node-group)
5. [List Associated Node Groups with EKS Cluster](#5-list-associated-node-groups-with-eks-cluster)
6. [Delete the EKS Cluster](#6-delete-the-eks-cluster)

---

### 1. Create the EKS Cluster

To create an EKS cluster named `dev-eks-cluster` in the `us-east-1` region without a node group, run:

```bash
eksctl create cluster \
   --name=dev-eks-cluster \
   --region=us-east-1 \
   --zones=us-east-1a,us-east-1b \
   --without-nodegroup
```

### 2. List Available EKS Clusters

To list all available EKS clusters in your AWS account:

```bash
eksctl get cluster
```

### 3. Enable IAM OIDC Provider for EKS Cluster

Enable the IAM OIDC provider for your EKS cluster, which allows Kubernetes service accounts to assume AWS IAM roles securely. Update the command with your cluster’s name and region.

```bash
eksctl utils associate-iam-oidc-provider \
    --region us-east-1 \
    --cluster dev-eks-cluster \
    --approve
```

### 4. Create the Spot Instance Node Group

This command creates a **managed node group** named `on-demand-ng` in the `dev-eks-cluster` using `t2.micro` and `t2.small` instance types. It provisions 2 nodes, with a minimum of 1 and a maximum of 5 nodes, each with 20 GB of gp3 encrypted storage. SSH access is enabled with the specified SSH key pair (`my-keypair`). The node group is fully managed by Amazon EKS, which handles updates and health monitoring. It also grants necessary permissions for the AWS Load Balancer Controller, Auto Scaling Groups, and full access to ECR.

```bash
eksctl create nodegroup \
     --cluster dev-eks-cluster \
     --name on-demand-ng \
     --instance-types t2.micro,t2.small \
     --nodes 2 \
     --nodes-min 1 \
     --nodes-max 5 \
     --node-volume-size 20 \
     --node-volume-type gp3 \
     --node-volume-encrypted \
     --ssh-access \
     --ssh-public-key my-keypair \
     --managed \
     --alb-ingress-access \
     --asg-access \
     --full-ecr-access
```

### 5. List Associated Node Groups with EKS Cluster

To list all node groups associated with the `dev-eks-cluster`:

```bash
eksctl get nodegroup --cluster=dev-eks-cluster
```

### 6. Delete the EKS Cluster

To delete the `dev-eks-cluster` and all associated resources:

```bash
eksctl delete cluster dev-eks-cluster
```

---

Save this file with a `.md` extension (for example, `README.md`), and it’s ready to be uploaded to GitHub as part of a repository for documenting your EKS cluster setup and management process.
