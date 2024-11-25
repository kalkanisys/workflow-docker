# workflow-docker

## Using the Deploy Reusable Workflow

This repository contains a reusable GitHub Actions workflow defined in the [`deploy.yml`](#file:deploy.yml-context) file. This workflow can be used to deploy your application to different environments using Docker.

### Inputs

The workflow accepts the following inputs:

- `ENV`: The environment to deploy to (required).
- `QEMU_ARCH`: The QEMU architecture to use (default: `amd64`).
- `DOWNLOAD_TASKFILE`: Whether to download the taskfile (default: `true`).
- `TASKFILE_VERSION`: The version of the deployer taskfile (default: `main`).

### Secrets

The workflow accepts the following secrets:

- `ACTION_GITHUB_TOKEN`: GitHub token (optional).

### Example Usage

To use this reusable workflow in your repository, create a new workflow file (e.g., `.github/workflows/deploy.yml`) and include the following content:

```yaml
name: Deploy

on:
  push:
    branches:
      - main

jobs:
  deploy:
    uses: kalkani/workflow-docker/.github/workflows/deploy.yml@main
    with:
      ENV: "prod"
      QEMU_ARCH: "amd64"
      DOWNLOAD_TASKFILE: false
      TASKFILE_VERSION: "main"
    secrets:
      ACTION_GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
```

This example triggers the deploy workflow on every push to the `main` branch, deploying the application to the `prod` environment.

## Using Taskfile.yml for Deployment

To deploy your application on a remote server using `docker-compose.deploy.yml` and `deploy.yml`, follow these steps:

1. **Download the Taskfile.yml**: Ensure you have the `Taskfile.yml` in your project directory.

2. **Prepare `docker-compose.deploy.yml`**: Create and configure the `docker-compose.deploy.yml` file with the containers you want to deploy.

3. **Create Environment-Specific Deploy Files**: Create `deploy.{{ENV}}.yml` files with different variable values to customize your deployment. For example, you can set `PROJECT`, `REMOTE_HOST`, and other items as needed.

4. **Run the Deployment Task**: Execute the deployment task with the appropriate environment variable.

```sh
ENV={{ENV}} task deploy
```

This command will use the `Taskfile.yml` to deploy your application based on the specified environment configuration.
