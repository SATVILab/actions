# Pre-build Dev Container GitHub Action

**Pre-build Dev Container** is a GitHub Action that automates the process of building your development container image with optional caching. It simplifies the setup for projects using Docker-based development environments by pre-building the container image and updating the `devcontainer.json` file accordingly.

---

## 📋 TL;DR

To quickly set up the **Pre-build Dev Container** action in your repository:

1. **Copy the workflow template** below.
2. **Paste it** into your repository's `.github/workflows/` directory as `devcontainer-build.yml`.
3. **Customize the inputs** if necessary.
4. **Commit and push** the changes to your repository.
5. **Trigger the workflow** manually from the GitHub Actions tab if desired.

```yaml
# .github/workflows/devcontainer-build.yml

name: 'Pre-build Dev Container'

on:
  push:
    branches:
      - main
  workflow_dispatch:
    inputs:
      use_cache:
        description: 'Use cache when building the image'
        required: false
        default: 'true'

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Build Dev Container
        uses: SATVILab/build-devcontainer@v1.2.0
        with:
          use_cache: ${{ github.event.inputs.use_cache }}
```

It will run automatically when the repo is pushed to.

To rebuild from scratch (i.e. not use the cache), run the action manually with `use_cache` set to false.

---

## 📖 Table of Contents

- [Pre-build Dev Container GitHub Action](#pre-build-dev-container-github-action)
  - [📋 TL;DR](#-tldr)
  - [🔍 Description](#-description)
  - [⚙️ Prerequisites](#️-prerequisites)
  - [🔧 Inputs](#-inputs)
  - [🚀 Usage](#-usage)
    - [1. Copy the Workflow File](#1-copy-the-workflow-file)
    - [2. Customize the Workflow (Optional)](#2-customize-the-workflow-optional)
    - [3. Commit and Push](#3-commit-and-push)
    - [4. Trigger the Workflow Manually (Optional)](#4-trigger-the-workflow-manually-optional)
  - [❓ When to Disable Cache](#-when-to-disable-cache)
  - [🛠️ Example](#️-example)
  - [🐞 Troubleshooting](#-troubleshooting)
  - [📄 License](#-license)
  - [🙌 Acknowledgments](#-acknowledgments)

---

## 🔍 Description

The **Pre-build Dev Container** action automates the building of your development container image. It:

- **Builds the Docker image** for your development environment.
- **Uses caching** to speed up the build process by default.
- **Updates** or creates the `.devcontainer/prebuild/devcontainer.json` file with the new image.
- **Commits and pushes** changes back to your repository.

This action is especially useful for projects that use Visual Studio Code Remote Containers or GitHub Codespaces.

---

## ⚙️ Prerequisites

- **GitHub Repository:** Ensure you have a repository where you want to set up the pre-build action.
- **Dockerfile and devcontainer.json:** Your repository should have a `.devcontainer` directory with a `Dockerfile` and a `devcontainer.json` file.
- **GitHub Actions Permissions:** The default `GITHUB_TOKEN` provided by GitHub Actions should have permissions to push to your repository.

---

## 🔧 Inputs

| Input       | Description                                      | Required | Default |
| ----------- | ------------------------------------------------ | -------- | ------- |
| `use_cache` | Use cache when building the image (`true`/`false`)| No       | `true`  |
| `image_name`| Name of the image to build                       | No       | `ghcr.io/<owner>/<repo>` |

---

## 🚀 Usage

### 1. Copy the Workflow File

Create a new workflow file in your repository at `.github/workflows/devcontainer-build.yml` and paste the following content:

```yaml
name: 'Pre-build Dev Container'

on:
  push:
    branches:
      - main
  workflow_dispatch:
    inputs:
      use_cache:
        description: 'Use cache when building the image'
        required: false
        default: 'true'

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Pre-build Dev Container
        uses: your-username/pre-build-devcontainer-action@v1
        with:
          use_cache: ${{ github.event.inputs.use_cache }}
```

### 2. Customize the Workflow (Optional)

- **Specify a custom image name:**

  If you want to use a custom image name or rebuild from scratch, modify the `with` section:

  ```yaml
        with:
          use_cache: ${{ github.event.inputs.use_cache }}
          image_name: 'ghcr.io/your-custom-image-name'
  ```

### 3. Commit and Push

- **Commit** the new workflow file to your repository.
- **Push** the changes to the main branch.

### 4. Trigger the Workflow Manually (Optional)

- Go to the **Actions** tab in your repository.
- Select the **Pre-build Dev Container** workflow.
- Click **Run workflow**.
- Set `use_cache` to `false` if you want to build without using the cache.
- Click **Run workflow** to start the job.

---

## ❓ When to Disable Cache

- **Cache Invalidation:** If you've made changes to the base image or dependencies that are not picked up due to caching, set `use_cache` to `false`.
- **Debugging Build Issues:** Disabling the cache can help identify issues that are masked when using cached layers.
- **First-Time Build:** The first build might not have a cache available; in this case, the setting has no effect.

---

## 🛠️ Example

Here's an example of a customized workflow file:

```yaml
name: 'Pre-build Dev Container'

on:
  push:
    branches:
      - main
  workflow_dispatch:
    inputs:
      use_cache:
        description: 'Use cache when building the image'
        required: false
        default: 'false'  # Default to not using cache
      image_name:
        description: 'Custom image name'
        required: false
        default: 'ghcr.io/your-username/your-repo'

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Pre-build Dev Container
        uses: your-username/pre-build-devcontainer-action@v1
        with:
          use_cache: ${{ github.event.inputs.use_cache }}
          image_name: ${{ github.event.inputs.image_name }}
```

---

## 🐞 Troubleshooting

- **Authentication Errors:** Ensure the action has permission to push to your repository. The default `GITHUB_TOKEN` should suffice.
- **Docker Build Failures:** Check your `Dockerfile` and `devcontainer.json` for any errors.
- **Cache Not Working:** If caching isn't speeding up builds, ensure that the image name remains consistent between builds.

---

## 📄 License

This project is licensed under the terms of the [MIT License](LICENSE).

---

## 🙌 Acknowledgments

Developed by [Your Name](https://github.com/your-username). Inspired by the need to simplify development container setups. Contributions and feedback are welcome!

---

**Feel free to open issues or pull requests if you have suggestions or encounter any problems.**