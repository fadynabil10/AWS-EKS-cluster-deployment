## 🚀 Imperative Approach: Amazon EKS Cluster Setup

Easily spin up your EKS cluster using `eksctl` in just a few steps! 👨‍💻

### 1. 🎯 Create the EKS Cluster
```bash
eksctl create cluster \
  --name=dev-eks-cluster \
  --region=us-east-1 \
  --zones=us-east-1a,us-east-1b \
  --without-nodegroup
```
---

Starts a new EKS cluster in the specified region without any node groups.

### 2. 📋 List Available EKS Clusters
```bash
eksctl get cluster
```
---

### 3. 🔒 Enable IAM OIDC Provider
```bash
eksctl utils associate-iam-oidc-provider \
  --region us-east-1 \
  --cluster dev-eks-cluster \
  --approve
```

---

### 4. 🛠️ Create On-Demand Node Group
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
Sets up a group of nodes using on-demand instances for flexible capacity.

### 5. 💸 Create Spot Node Group (Save Costs!)
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
A more cost-effective option by using spot instances.

### 6. 🔍 List Node Groups
```bash
eksctl get nodegroup --cluster=dev-eks-cluster
```
---
Displays the node groups associated with the cluster.

### 7. ❌ Delete the EKS Cluster
```bash
eksctl delete cluster dev-eks-cluster
```

Happy deploying! 🚀
---
💻 **Made with ❤️ by AWS Community Builder** | [Reach_Out_2_Me_@](https://www.linkedin.com/in/sarvar04/)
