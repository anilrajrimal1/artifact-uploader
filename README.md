# Build Artifact Uploader

[![GitHub release (latest by date)](https://img.shields.io/github/v/release/anilrajrimal1/artifact-uploader)](https://github.com/anilrajrimal1/artifact-uploader/releases)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

A GitHub Action that zips and uploads build artifacts to an AWS S3 bucket, simplifying CI/CD workflows by automating artifact storage.

## 📌 Overview

This action ensures seamless integration of build artifacts into your CI/CD pipeline by packaging and storing them in an S3 bucket.

## ✨ Features

- 📦 Compresses build artifacts into a ZIP file
- ☁️ Uploads artifacts securely to AWS S3
- 🔄 Optimized for CI/CD workflows
- 🛡️ Secure authentication with AWS credentials

## Usage

To use this action in your GitHub workflow, add the following step to your `workflow.yml`:

```yaml
- name: Upload Build Artifact to S3
  uses: anilrajrimal1/artifact-uploader@v1.0.0
  with:
    aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    aws-region: ${{ env.AWS_REGION }}
    project-name: ${{ env.PROJECT_NAME }}
    s3-base-path: "s3://your-bucket-name"
    source-directory: "./dist"
```

## 🔧 Inputs

| Name                   | Required | Default      | Description                                     |
|------------------------|----------|--------------|-------------------------------------------------|
| `aws-access-key-id`    | ✅ Yes   | -            | AWS access key ID for authentication           |
| `aws-secret-access-key`| ✅ Yes   | -            | AWS secret access key for authentication       |
| `aws-region`          | ✅ Yes   | `ap-south-1` | AWS region where the S3 bucket is located      |
| `project-name`        | ✅ Yes   | -            | Project name used for organizing artifacts     |
| `s3-base-path`        | ✅ Yes   | -            | Base S3 path where artifacts will be stored   |
| `source-directory`    | ❌ No    | `dist`       | Directory containing files to zip and upload  |

## 🔄 How It Works

1. Configures AWS credentials securely.
2. Creates a ZIP archive of the specified `source-directory`.
3. Uploads the ZIP file to the designated S3 path.
4. Outputs the final S3 location of the uploaded artifact.

## Security Notes

- Store your `AWS_ACCESS_KEY_ID` and `AWS_SECRET_ACCESS_KEY` as GitHub Secrets.
- Never hardcode sensitive credentials in workflow files.
- Uses AWS best practices for secure authentication.

## Requirements

- GitHub Actions runner with AWS CLI installed.
- AWS creds with proper permissions for S3

## Contributing

Contributions are welcome! Feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Author

Anil Raj Rimal

## Acknowledgements

- [AWS S3](https://aws.amazon.com/s3/) for providing scalable storage solutions.
