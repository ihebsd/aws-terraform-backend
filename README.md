# aws-terraform-backend

Bootstrap infrastructure for Terraform remote state on AWS.

This repository provisions:

- An S3 bucket for Terraform state storage
- A DynamoDB table for state locking
- Required security configurations (encryption, versioning)

---

## 📌 Purpose

This project initializes the remote backend infrastructure required for Terraform deployments on AWS.

It creates:

- 🪣 S3 bucket → stores the Terraform state file  
- 🗄 DynamoDB table → prevents concurrent state modifications (state locking)

This repository is meant to be executed once per AWS account/environment.

---

## 🏗 Architecture

Terraform (local backend)
        ↓
Creates
        ↓
- S3 Bucket (remote state storage)
- DynamoDB Table (state locking)

After creation, other Terraform projects can reference this backend.

---

## ⚙️ Requirements

- Terraform >= 1.5
- AWS CLI configured
- AWS account
- Proper IAM permissions

---

## 🚀 Usage

### 1️⃣ Clone the repository

```bash
git clone https://github.com/<your-org>/aws-terraform-backend.git
cd aws-terraform-backend
```

### 2️⃣ Initialize Terraform

```bash
terraform init
```

### 3️⃣ Review the plan

```bash
terraform plan
```

### 4️⃣ Apply

```bash
terraform apply
```

---

## 🔐 Important Notes

- This project should initially use a **local backend**
- Do NOT configure the S3 backend in this repo before it is created
- After creation, other Terraform projects can reference the generated S3 and DynamoDB resources

Example backend configuration for other projects:

```hcl
terraform {
  backend "s3" {
    bucket         = "your-terraform-state-bucket"
    key            = "env/terraform.tfstate"
    region         = "eu-west-1"
    dynamodb_table = "terraform-locks"
    encrypt        = true
  }
}
```

---

## 📁 Suggested Structure

```
aws-terraform-backend/
│
├── main.tf
├── variables.tf
├── outputs.tf
├── versions.tf
└── README.md
```

---

## 🛡 Best Practices

- Enable bucket versioning
- Enable server-side encryption (SSE)
- Restrict public access
- Use IAM roles (OIDC) instead of access keys in CI/CD
- Separate environments (dev/staging/prod)

---

## 📌 Next Steps

After this backend is provisioned:

- Configure remote backend in other Terraform projects
- Integrate with GitHub Actions using OIDC
- Implement environment separation