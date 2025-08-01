**GitHub README section**:

### 📊 Architecture Diagram

    A[GitHub Repo] --> B[AWS CodePipeline]
    B --> C[AWS CodeBuild]
    C --> D[Elastic Beanstalk (Staging)]
    D --> E[Manual Approval]
    E --> F[Elastic Beanstalk (Production)]

### 🔐 IAM Roles & Security

| Role Name                       | Used By                  | Purpose                                                                |
|---------------------------------|--------------------------|------------------------------------------------------------------------|
| `AWS-CodePipeline-Service-Role` | CodePipeline             | Orchestrates pipeline actions across AWS services                      |
| `CodeBuild-Service-Role`        | CodeBuild                | Accesses source artifacts, logs, and build environments                |
| `ElasticBeanstalk-Service-Role` | Elastic Beanstalk        | Manages environments and deployments                                   |
| `ElasticBeanstalk-EC2-Role`     | EC2 instances            | Pulls code, writes logs, and monitors health                           |
| `Manual Approval IAM Policy`    | IAM user/group           | Restricts who can approve production deployments                       |

All roles follow **least privilege** principles and are scoped to specific resources. Secrets are stored securely using **AWS Secrets Manager** or **SSM Parameter Store**, and artifacts are encrypted in **S3 using KMS**.

---

### ✅ Key Features

- 🔄 Automated build/test/deploy pipeline triggered by GitHub commits
- 🛑 Manual approval gate before production deployment
- 🔐 Secure IAM role separation for each service
- 📈 CloudWatch logging and monitoring
- 🔒 Artifact encryption and rollback support
- 💰 Cost-efficient and scalable deployment strategy

---

### 📁 Example IAM Policy Snippet

```json
{
  "Effect": "Allow",
  "Action": [
    "codebuild:StartBuild",
    "elasticbeanstalk:*",
    "s3:GetObject",
    "codepipeline:PutApprovalResult"
  ],
  "Resource": "*"
}
```

---

### 📌 Deployment Flow Summary

```plaintext
GitHub Commit → CodePipeline Triggered
→ CodeBuild Builds & Tests → Deploy to Staging (Elastic Beanstalk)
→ Manual Approval → Deploy to Production (Elastic Beanstalk)

