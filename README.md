# LCloud (LocalCloud)

## Project Objective & Scope

**LCloud** is an open-source initiative to bring AWS sandbox environments to a Kubernetes (K8s) platform, ideally running OnPremise. The goal is to enable zero-cost development environments that can later be integrated into CI/CD pipelines and deployed to AWS accounts seamlessly. LCloud will emulate AWS services using open-source technologies, allowing developers to use familiar AWS CLI commands and a web interface identical to AWS, while running everything locally.

### Motivation

- **Cost Reduction:** Avoid cloud expenses for development and testing environments.
- **Portability:** Make it easy to move workloads from local to AWS, reusing scripts and workflows.
- **Agility:** Enable early integration with CI/CD pipelines and automated deployments to AWS.

### Strategy

- **AWS CLI & UI Compatibility:** Users interact with services using AWS CLI and a web UI mimicking AWS Console.
- **Service Emulation:** Each AWS service is mapped to an open-source equivalent on K8s:
  - **Athena:** PrestoDB as the SQL engine.
  - **EC2:** KubeVirt for virtual machines.
  - **S3:** MinIO for object storage.
  - **Lambda:** OpenFaaS or Knative for serverless functions.
  - **VPC:** K8s CNI plugins for virtual networking.
  - **Glue:** Apache Airflow or Spark for ETL orchestration.
  - **LakeFormation:** Open-source data catalog and access control.
  - **AppSync:** Hasura or Apollo for GraphQL APIs.
  - **CloudFormation:** Crossplane or Pulumi for infrastructure as code.

### Initial Scope

- Start with Lambda, EC2, VPC, Athena, Glue, LakeFormation, S3, AppSync, and CloudFormation.
- Ensure each service is modular, integrable, and behaves like its AWS counterpart.
- Design for easy extension to more AWS services in the future.

### Integration & User Experience

- **CLI & Web UI:** AWS CLI compatibility and a familiar web interface.
- **CI/CD Ready:** Local environments can be plugged into CI/CD pipelines for automated AWS deployment.
- **Developer Friendly:** Reuse AWS scripts, templates, and workflows.

---

## Routemap & Priorities


| Phase | Activity                                      | Priority | Owner(s)                | Status   | Notes                                  |
|-------|-----------------------------------------------|----------|-------------------------|----------|----------------------------------------|
| 1     | Project repository & license setup            | High     | Core Team               | Backlog  | Choose MIT/Apache 2.0 license          |
| 1     | Define architecture & service mapping         | High     | Core Team               | Backlog  | Draw diagrams, select open-source tech |
| 1     | AWS CLI compatibility layer                   | High     | Core Team               | Backlog  | Parse/translate AWS CLI commands       |
| 2     | S3 (MinIO) integration                       | High     | Core Team               | Backlog  | REST API, bucket/object operations     |
| 2     | EC2 (KubeVirt) integration                   | High     | hugo.gif@gmail.com      | To Do    | VM lifecycle, networking               |
| 2     | Lambda (OpenFaaS/Knative) integration        | High     | Core Team               | Backlog  | Function deployment/execution          |
| 2     | Athena (PrestoDB) integration                | Medium   | Core Team               | Backlog  | Query interface, data sources          |
| 2     | VPC (K8s CNI) integration                    | Medium   | Core Team               | Backlog  | Network isolation, subnets             |
| 3     | Glue (Airflow/Spark) integration             | Medium   | Core Team               | Backlog  | ETL jobs, scheduling                   |
| 3     | LakeFormation (Data Catalog) integration     | Medium   | Core Team               | Backlog  | Permissions, metadata                  |
| 3     | AppSync (Hasura/Apollo) integration          | Medium   | Core Team               | Backlog  | GraphQL endpoints                      |
| 3     | CloudFormation (Crossplane/Pulumi)           | Medium   | Core Team               | Backlog  | IaC templates, resource provisioning   |
| 4     | Unified Web UI (AWS Console-like)            | Medium   | Core Team               | Backlog  | React/Vue/Angular                      |
| 4     | CI/CD pipeline integration                   | Medium   | Core Team               | Backlog  | GitHub Actions, GitLab CI, etc.        |
| 5     | Documentation & tutorials                    | High     | Core Team               | Backlog  | README, examples, guides               |
| 5     | Community feedback & roadmap update          | High     | Core Team               | Backlog  | Issues, discussions, PRs               |

---

## Open Invitation

**This project is an open invitation:**

👉 If you have experience, ideas, or just curiosity, **join me!**  
👉 I want this to be a collaborative space where we can share progress, questions, and best practices.

Let’s build, learn, and share together!

**#Kubernetes #CloudNative #DevOps #OnPremise #DisasterRecovery #OpenS**

---

Ready to contribute? Fork, clone, and let’s get started!
