# Build Artifact Uploader

## Description
**Build Artifact Uploader** is a GitHub Action that zips and uploads a build artifact to an AWS S3 bucket. This action helps streamline CI/CD workflows by automating the process of artifact storage.

## Usage
To use this action in your GitHub workflow, add the following step in your `workflow.yml`:

```yaml
- name: Build Artifact Uploader
  uses: anilrajrimal1/artifact-uploader@v1.0.0
  with:
    aws-access-key-id: ${{ secrets.AWS_ACCESS_KEY_ID }}
    aws-secret-access-key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    aws-region: ${{ env.AWS_REGION }}
    project-name: ${{ env.PROJECT_NAME }}
    s3-base-path: "s3://your-bucket-name"
    source-directory: "./dist"
```

## Inputs
| Name | Required | Default | Description |
|------|----------|---------|-------------|
| `aws-access-key-id` | ✅ | - | AWS access key ID |
| `aws-secret-access-key` | ✅ | - | AWS secret access key |
| `aws-region` | ✅ | `ap-south-1` | AWS region |
| `project-name` | ✅ | - | Name of the project |
| `s3-base-path` | ✅ | - | Base S3 path for uploads |
| `source-directory` | ❌ | `dist` | Directory to zip and upload |

## How It Works
1. Configures AWS credentials.
2. Creates a ZIP archive of the specified `source-directory`.
3. Uploads the ZIP file to the specified S3 path.
4. Outputs the final S3 location of the uploaded artifact.
