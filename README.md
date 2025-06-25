# Execute Just Command with AWS OIDC

This GitHub Action sets up [`just`](https://github.com/casey/just), authenticates to AWS via OIDC, and runs a specified **just recipe** — useful for clean, repeatable, script-based workflows in infrastructure, DevOps, and CI/CD pipelines.

---

## 🚀 Features

- Installs a specific version of [`just`](https://github.com/casey/just)
- Configures AWS credentials using GitHub OIDC
- Executes any `just` command (recipe)
- Captures and returns the final line of output as an action output

---

## 📥 Inputs

| Name               | Description                                      | Required | Default      |
|--------------------|--------------------------------------------------|----------|--------------|
| `just_version`     | Version of `just` to install                     | ❌        | `1.0.0`      |
| `aws_region`       | AWS region                                       | ❌        | `eu-west-2`  |
| `aws_oidc_role_arn`| ARN of the IAM role to assume via OIDC           | ✅        | —            |
| `just_action`      | The `just` recipe to execute                     | ✅        | —            |

---

## 📤 Outputs

| Name           | Description                                |
|----------------|--------------------------------------------|
| `just_outputs` | Output of the `just` command (last line)   |

---

## 🛠 Example Usage

```yaml
jobs:
  run-just:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read

    steps:
      - uses: actions/checkout@v4

      - name: Execute a Just recipe
        uses: your-org/just-oidc-action@main
        with:
          just_version: "1.14.0"
          aws_oidc_role_arn: arn:aws:iam::123456789012:role/github-oidc-role
          just_action: deploy-api
