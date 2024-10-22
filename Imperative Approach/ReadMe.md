## 🚀 Imperative Approach: Amazon EKS Cluster Setup

Welcome to the Amazon EKS Cluster Setup Guide! This document outlines the steps to create and manage an Amazon Elastic Kubernetes Service (EKS) cluster using the `eksctl` command-line tool. Whether you're launching a new cluster or managing existing node groups, this guide provides a straightforward, imperative approach to streamline your Kubernetes deployment on AWS. Let's get started! 🚀

Easily spin up your EKS cluster using `eksctl` in just a few steps! 👨‍💻
---
### 1. 🎯 Create the EKS Cluster
Starts a new EKS cluster in the specified region without any node groups.
```bash
eksctl create cluster \
  --name=dev-eks-cluster \
  --region=us-east-1 \
  --zones=us-east-1a,us-east-1b \
  --without-nodegroup
```
---

### 2. 📋 List Available EKS Clusters
```bash
eksctl get cluster
```
---

### 3. 🔒 Enable IAM OIDC Provider
Links IAM roles with the EKS cluster to manage authentication.
```bash
eksctl utils associate-iam-oidc-provider \
  --region us-east-1 \
  --cluster dev-eks-cluster \
  --approve
```

---

### 4. 🛠️ Create On-Demand Node Group
Sets up a group of nodes using on-demand instances for flexible capacity.
```bash
eksctl create nodegroup \
  --cluster dev-eks-cluster \
  --name on-demand-ng \
  --instance-types t2.micro,t2.small \
  --nodes 2 \
  --nodes-min 1 \
  --nodes-max 5 \
  --node-volume-size 20 \
  --ssh-access \
  --ssh-public-key test \
  --managed \
  --alb-ingress-access \
  --asg-access \
  --full-ecr-access
```

---

### 5. 💸 Create Spot Node Group (Save Costs!)
A more cost-effective option by using spot instances.
```bash
eksctl create nodegroup \
  --cluster dev-eks-cluster \
  --name spot-ng \
  --instance-types t2.micro,t2.small \
  --nodes 2 \
  --nodes-min 1 \
  --nodes-max 5 \
  --node-volume-size 20 \
  --ssh-access \
  --ssh-public-key test \
  --managed \
  --spot \
  --alb-ingress-access \
  --asg-access \
  --full-ecr-access
```

---

### 6. 🔍 List Node Groups
Displays the node groups associated with the cluster.
```bash
eksctl get nodegroup --cluster=dev-eks-cluster
```
---


### 7. ❌ Delete the EKS Cluster
Remove the EKS cluster when you're done.
```bash
eksctl delete cluster dev-eks-cluster
```
🎉 That’s it! Your EKS cluster will be up and running in no time. If you need further details or a step-by-step guide, check out this awesome article: [Spin Up an Amazon EKS Cluster in Minutes](https://dev.to/aws-builders/spin-up-an-amazon-eks-cluster-in-minutes-2666-temp-slug-667873?preview=205922cdaf933b01455dba5ef201bc8b1be8e0ab2767205d094c9878ebf54720f80151d0251fb19d9e37d26216e61ac73799e52d91fbe23c37b97530)

Happy deploying! 🚀
---
💻 **Made with ❤️ by AWS Community Builder** | [Reach_Out_2_Me_@](https://www.linkedin.com/in/sarvar04/)
