# Deploy repository to Server Action

## Description

This composite action automates the deployment of a GitHub repository to a specified server. It's designed to facilitate consistent and automated deployment processes, such as updating web applications, synchronizing files, or deploying updates to server-based applications. The action allows for deployment from any specified branch to a designated directory on the server.

## Functionality

- **Checkout Repository**: Clones the specified branch of the repository into the runner's workspace.
- **Prepare Repository Directory**: Creates a directory and copies the repository contents into it, preserving the structure for zipping.
- **Create Unique ZIP File**: Packages the directory into a uniquely named ZIP file, preventing overwriting and aiding in deployment tracking.
- **Secure Copy (SCP) to Server**: Transfers the ZIP file to the remote server using SCP.
- **Prepare Server and Deploy**: Connects to the server via SSH to:
  - Install `zip` and `unzip` if not present.
  - Remove the existing target directory for a clean deployment.
  - Extract the ZIP contents into the target directory.
  - Remove the ZIP file post-extraction.

## Inputs

- `branch`: Branch of the repository to deploy.
- `server_host`: Hostname or IP address of the server.
- `server_user`: Username for SSH access.
- `server_password`: Password for SSH access.
- `server_port`: SSH port of the server.
- `target_directory`: Target directory on the server for deployment.
- `repo_directory`: Directory name for placing repository files before zipping.

## Usage

This action can be included in any workflow within the same repository. It's particularly useful for scenarios like continuous deployment, routine updates, or synchronized deployments across multiple environments. Set up the necessary inputs, including server details and directory paths, to utilize this action.

To use this action, create a `.github/workflows/deploy_to_server.yml` (or similar) file in your repository:

```yaml
name: Deploy Workflow

on:
  pull_request:
    types:
      - opened
      - synchronize

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:

      - name: Deploy to Server
        uses: NIOMIND-srl-sb/deploy-repository-to-server-action@v1.0.0
        with:
          branch: ${{ github.event.inputs.branch }}
          server_host: ${{ secrets.SERVER_HOST }}
          server_user: ${{ secrets.SERVER_USER }}
          server_password: ${{ secrets.SERVER_PASSWORD }}
          server_port: ${{ secrets.SERVER_PORT }}
          target_directory: ${{ secrets.target_directory }}
```
