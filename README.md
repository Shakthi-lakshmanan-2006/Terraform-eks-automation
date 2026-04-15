# 🚀 Terraform EKS Project

> Provision a production-ready **Amazon EKS cluster** on AWS using Terraform — including VPC, subnets, IAM roles, node groups, and an EC2 client instance.

---

## 📐 Architecture Overview

```
AWS Region: ap-south-1
│
├── VPC (10.0.0.0/16)
│   ├── Public Subnet 1 (10.0.1.0/24) — ap-south-1a
│   ├── Public Subnet 2 (10.0.2.0/24) — ap-south-1b
│   ├── Internet Gateway
│   └── Route Table (0.0.0.0/0 → IGW)
│
├── Security Group (allow all ingress/egress)
│
├── EC2 Instance (t3.micro) — EKS Client
│
├── IAM Role — EKS Cluster Role
│   └── AmazonEKSClusterPolicy
│
├── EKS Cluster — my-eks-cluster
│
├── IAM Role — EKS Node Role
│   ├── AmazonEKSWorkerNodePolicy
│   ├── AmazonEC2ContainerRegistryReadOnly
│   └── AmazonEKS_CNI_Policy
│
└── EKS Node Group (t3.micro × 2 nodes)
```

---

## 📁 Project Structure

```
terraform-eks-project/
├── main.tf                  # All Terraform resources (VPC, EKS, IAM, etc.)
├── .terraform.lock.hcl      # Provider version lock file (tracked in Git)
├── .gitignore               # Excludes sensitive/generated files
└── README.md                # This file
```

---

## ✅ Prerequisites

Before you begin, make sure you have the following installed:

| Tool | Version | Link |
|------|---------|------|
| Terraform | >= 1.0 | [Install](https://developer.hashicorp.com/terraform/install) |
| AWS CLI | >= 2.x | [Install](https://docs.aws.amazon.com/cli/latest/userguide/install-cliv2.html) |
| kubectl | Latest | [Install](https://kubernetes.io/docs/tasks/tools/) |

Also configure your AWS credentials:

```bash
aws configure
# Enter your AWS Access Key ID, Secret Access Key, and region (ap-south-1)
```

---

## 🛠️ Usage

### 1. Clone the repository

```bash
git clone https://github.com/<your-username>/terraform-eks-project.git
cd terraform-eks-project
```

### 2. Initialize Terraform

Downloads the required provider plugins:

```bash
terraform init
```

### 3. Review the plan

```bash
terraform plan
```

### 4. Apply the configuration

```bash
terraform apply
```

Type `yes` when prompted. This will provision all AWS resources (~10–15 min for EKS cluster to be ready).

### 5. Connect to the EKS cluster

```bash
aws eks update-kubeconfig --region ap-south-1 --name my-eks-cluster
kubectl get nodes
```

### 6. Destroy all resources (when done)

```bash
terraform destroy
```

---

## ⚙️ Configuration

| Variable | Default | Description |
|----------|---------|-------------|
| `cluster_name` | `my-eks-cluster` | Name of the EKS cluster |

To change to a custom cluster name, edit `main.tf`:
```hcl
variable "cluster_name" {
  default = "your-custom-cluster-name"
}
```

---

## 🔐 Security Notes

- ⚠️ The security group currently allows **all inbound/outbound traffic**. Restrict this in production.
- 🔒 Never commit `terraform.tfstate` — it contains sensitive infrastructure details.
- 🔑 Ensure your AWS credentials are **never hardcoded** in `.tf` files; use `aws configure` or environment variables.

---

## 📦 Resources Created

| Resource | Name |
|----------|------|
| VPC | main-vpc |
| Public Subnet 1 | public-subnet-1 (ap-south-1a) |
| Public Subnet 2 | public-subnet-2 (ap-south-1b) |
| Internet Gateway | — |
| Security Group | allow_all |
| EC2 Instance | EKS-Client (t3.micro) |
| IAM Role | eks-role, eks-node-role |
| EKS Cluster | my-eks-cluster |
| Node Group | node-group (2× t3.micro) |

---

## 🧑‍💻 Author

> Built with ❤️ using Terraform + AWS EKS

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).
