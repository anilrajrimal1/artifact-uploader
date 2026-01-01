# Build Artifact Uploader

[![GitHub release (latest by date)](https://img.shields.io/github/v/release/anilrajrimal1/artifact-uploader)](https://github.com/anilrajrimal1/artifact-uploader/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A GitHub Action that zips and uploads build artifacts to an AWS S3 bucket, simplifying CI/CD workflows by automating artifact storage.

## Overview

This action ensures seamless integration of build artifacts into your CI/CD pipeline by packaging and storing them in an S3 bucket.

## Features

- Compresses build artifacts into a ZIP file
- Uploads artifacts securely to AWS S3
- Optimized for CI/CD workflows

## Prerequisites

Before using this action, ensure that:

- You have an AWS account.
- You have an s3 bucket created.
- You have created an IAM Role that:
   - Trusts GitHub’s OIDC provider
   - Has permission to access s3 bucket.
- Your GitHub workflow has:

```yaml
permissions:
  id-token: write
  contents: read
```

### AWS Setup (OIDC – One Time)
1. Create an OIDC provider in AWS IAM:
  - Provider URL: https://token.actions.githubusercontent.com
  - Audience: sts.amazonaws.com

2. Create an IAM Role with trust policy similar to:
```json
{
  "Effect": "Allow",
  "Principal": {
    "Federated": "arn:aws:iam::<ACCOUNT_ID>:oidc-provider/token.actions.githubusercontent.com"
  },
  "Action": "sts:AssumeRoleWithWebIdentity",
  "Condition": {
    "StringEquals": {
      "token.actions.githubusercontent.com:aud": "sts.amazonaws.com"
    },
    "StringLike": {
      "token.actions.githubusercontent.com:sub": "repo:<OWNER>/<REPO>:*"
    }
  }
}
```

3. Attach an IAM policy allowing s3 read/write.

## Usage

To use this action in your GitHub workflow, add the following step to your `workflow.yml`:

```yaml
name: Build and Push Artifacts to S3

on:
  push:
    branches:
      - master
  workflow_dispatch:
  
permissions:
  id-token: write
  contents: read

jobs:
  build:
    name: Build and Push
    runs-on: self-hosted
    steps:
      - name: Upload Build Artifact to S3
        uses: anilrajrimal1/artifact-uploader@v1
        with:
          aws-role-arn: ${{ vars.AWS_ROLE_ARN }}
          aws-region: ${{ env.AWS_REGION }}
          project-name: ${{ env.PROJECT_NAME }}
          s3-base-path: "s3://your-bucket-name"
          source-directory: "./dist"
```

## Inputs

| Name                   | Required | Default      | Description                                     |
|------------------------|----------|--------------|-------------------------------------------------|
| `aws-role-arn`         | ✅ Yes   | -            | IAM Role ARN to assume via GitHub OIDC          |
| `aws-region`           | ✅ Yes   | `ap-south-1` | AWS region where the S3 bucket is located       |
| `project-name`         | ✅ Yes   | -            | Project name used for organizing artifacts      |
| `s3-base-path`         | ✅ Yes   | -            | Base S3 path where artifacts will be stored     |
| `source-directory`     | ❌ No    | `dist`       | Directory containing files to zip and upload    |

## How It Works

1. Use AWS OIDC for login.
2. Creates a ZIP archive of the specified `source-directory`.
3. Uploads the ZIP file to the designated S3 path.
4. Outputs the final S3 location of the uploaded artifact.

## Security Notes

- Never hardcode sensitive credentials in workflow files.
- This action does not use AWS access keys
- Credentials are short-lived and issued per workflow run
- Recommended by both GitHub and AWS

## Contributing

Contributions are welcome! Feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Author

Anil Raj Rimal

## Acknowledgements

- [AWS S3](https://aws.amazon.com/s3/) for providing scalable storage solutions.
