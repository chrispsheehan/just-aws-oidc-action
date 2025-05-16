# just-aws-oidc-action

Assume AWS role and run justfile recipes.

## usage

```yml
  backend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Deploy api
        uses: chrispsheehan/terragrunt-aws-oidc-action@0.2.0
        with:
          aws_oidc_role_arn: ${{ vars.AWS_OIDC_ROLE_ARN }}
          tg_directory: infra/live/dev/aws/api
```
