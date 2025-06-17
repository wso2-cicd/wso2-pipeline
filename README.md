# wso2-pipeline

## Centralized CI/CD Orchestration for WSO2 Products

This repository serves as the **central GitOps-style control plane** for deploying APIs, policies, and related artifacts to WSO2 API Manager environments using GitHub Actions.

It works in conjunction with:
- [**`wso2-workflows`**](https://github.com/wso2-cicd/wso2-workflows): A library of reusable GitHub Action workflows.
- **Individual API/policy repositories**: Each containing versioned API artifacts and configurations.

Together, they enable **secure**, **automated**, and **auditable** deployments across `Dev`, `QA`, `UAT`, and `Prod` environments.

---

## Prerequisites

To successfully run CI/CD workflows from this repository, ensure the following setup is complete:

### Self-Hosted GitHub Runner

A self-hosted runner is required to securely deploy to internal WSO2 environments.

#### Setup Instructions

1. Follow [GitHub's official guide](https://docs.github.com/en/actions/hosting-your-own-runners/adding-self-hosted-runners) to configure a self-hosted runner.
2. Ensure the runner has network access to your WSO2 environments (e.g., VPN, VPC, firewall).
3. Install the following tools on the runner:
   - [`apictl`](https://apim.docs.wso2.com/en/latest/install-and-setup/setup/api-controller/getting-started-with-wso2-api-controller/)
   - `jq`
   - `gh` (GitHub CLI)
   - `git`
   - `zip`
   - `unzip`
4. Configure WSO2 environments in `apictl`:
    ```bash
    apictl add env dev  --apim https://localhost:9444
    apictl add env qa   --apim https://localhost:9445
    apictl add env uat  --apim https://localhost:9446
    apictl add env prod --apim https://localhost:9447
    ```

---

### 🔐 Required GitHub Secrets

Set the following secrets at the repository level (or at the organization level for broader reuse):

| Secret Name                   | Description                                               |
|-------------------------------|-----------------------------------------------------------|
| `WSO2_GIT_BOT_USERNAME`       | GitHub username of the bot/account used in automation     |
| `WSO2_GIT_BOT_EMAIL`          | Email address of the GitHub bot account                   |
| `WSO2_GIT_BOT_PAT`            | Personal Access Token with `repo` and `workflow` scopes   |
| `WSO2_APIM_DEV_ADMIN_USERNAME`| Admin username for Dev environment (used by `apictl`)     |
| `WSO2_APIM_DEV_ADMIN_PASSWORD`| Admin password for Dev environment                        |
| `WSO2_APIM_QA_ADMIN_USERNAME` | Admin username for QA environment                         |
| `WSO2_APIM_QA_ADMIN_PASSWORD` | Admin password for QA environment                         |

> You can also store these secrets in a centralized secret manager like **HashiCorp Vault** and fetch them at runtime.

---

## 📚 Related Repositories

- [`wso2-workflows`](https://github.com/wso2-cicd/wso2-workflows) – Reusable GitHub Actions workflows for API and policy deployments.
- Individual API Repos (e.g., `api-identity-sys`, `api-ecommerce-exp`)
- Policy Repos (`policy-mediation`, `policy-rate-limiting`)

---

For more information, see [Part 1: CI/CD for WSO2 Products](https://medium.com/@rajkumar.rajaratnam/ci-cd-for-wso2-products-part-1-deploying-apis-to-wso2-api-manager-using-api-controller-b64fa90b4472)
