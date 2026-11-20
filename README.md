# preview-deployment

A static HTML page deployed to Amazon S3 from GitHub Actions. The workflow signs in to AWS with OpenID Connect, so the repo stores no long-lived AWS keys.

## Live page

https://test-preview-deployment-elshanx.s3.eu-north-1.amazonaws.com/index.html

## How it deploys

Every push to `main` runs `.github/workflows/deploy.yml`, which:

1. Checks out the repo.
2. Requests a short-lived OIDC token from GitHub and exchanges it for temporary AWS credentials by assuming an IAM role, `preview-deployment-gha`.
3. Copies `index.html` to the S3 bucket.

The role's trust policy only accepts tokens from this repo on the `main` branch. Its permissions policy only allows listing the bucket, writing objects, and deleting objects in it.

## Files

- `index.html`: the whole site.
- `.github/workflows/deploy.yml`: the deploy pipeline.
