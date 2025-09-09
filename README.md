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

```just
lambda-get-version:
    #!/usr/bin/env bash
    aws lambda get-alias \
        --function-name "$FUNCTION_NAME" --name "$ALIAS_NAME" \
        --query 'FunctionVersion' --output text
```

```yaml
jobs:
  run-just:
    runs-on: ubuntu-latest
    permissions:
      id-token: write
      contents: read

    steps:
      - uses: actions/checkout@v4

      - name: get lambda version
        id: lambda-get-version
        uses: chrispsheehan/just-aws-oidc-action@0.1.2
        env:
          FUNCTION_NAME: dev-lambda-function
          ALIAS_NAME: dev
        with:
          aws_oidc_role_arn: ${{ env.AWS_OIDC_ROLE_ARN }}
          just_action: lambda-get-version

      - name: read output from script
        run: |
          echo "Script output: ${{ steps.lambda-get-version.outputs.just_outputs }}"
          VERSION="${{ steps.lambda-get-version.outputs.just_outputs }}"
          echo "Parsed VERSION=$VERSION"
