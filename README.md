# just-aws-oidc-action

Assume AWS role and run justfile recipes.

## usage

```yml
  frontend:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Run cloudfront invalidation
        uses: chrispsheehan/just-aws-oidc-action@0.1.1
        with:
          aws_oidc_role_arn: ${{ vars.AWS_OIDC_ROLE_ARN }}
          just_action: frontend-refresh
```
