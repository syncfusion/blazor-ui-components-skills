# Docker Deployment in Blazor FileManager

## Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Configuration](#configuration)
- [Docker Deployment](#docker-deployment)
- [Configure the Client-Side File Manager Component](#configure-the-client-side-file-manager-component)
- [Troubleshooting](#troubleshooting)

---

## Overview

The Blazor FileManager is a component for managing files and folders in a web application. It provides a Windows Explorer-like interface for file operations.

**From `docker-deployment.md` (lines 1-40):**

The unified Syncfusion File Manager provider service supports both Azure Blob Storage and Amazon S3 through a single Docker image. The service exposes the following operations: read, details, search, image preview, download, upload, create, rename, copy, move, and delete.

- **Single image, two providers** - One Docker image covers Azure Blob Storage and Amazon S3
- **Environment-driven selection** - Switch the storage backend with the `FILEMANAGER_PROVIDER` variable
- **Unified client endpoints** - The same API paths work for both providers
- **Production-ready** - Pin to a specific image tag that matches your `Syncfusion.Blazor.FileManager` version

### Provider Selection

Select the storage provider by setting the `FILEMANAGER_PROVIDER` environment variable:

| Provider | `FILEMANAGER_PROVIDER` value | Storage settings |
|--|--|--|
| Azure Blob Storage | `azure` | `AZURE_*` variables |
| Amazon S3 | `amazon-s3` | `AWS_*` variables |

You can deploy the published image directly, or build a custom Docker image from the [Syncfusion File Manager providers repository](https://github.com/SyncfusionExamples/filemanager-providers).

> **Production Note**
>
> The example in this guide uses the `:latest` tag for brevity. For production deployments, pin to a specific image tag that matches the version of `Syncfusion.Blazor.FileManager` you install on the client. See the [image tags listing](https://hub.docker.com/r/syncfusion/filemanager-provider/tags) on Docker Hub.

---

## Prerequisites

Install [Docker](https://www.docker.com/products/container-runtime#/download) in your environment:

- **Windows** - [Docker Desktop for Windows](https://docs.docker.com/desktop/setup/install/windows-install/)
- **macOS** - [Docker Desktop for macOS](https://docs.docker.com/desktop/setup/install/mac-install/)
- **Linux** - [Docker Engine](https://docs.docker.com/engine/install/) with the Docker Compose plugin

Before starting the service, create and configure either an Azure Blob Storage account and container or an Amazon S3 bucket.

**From `docker-deployment.md` (lines 40-80):**

### Azure Blob Storage Requirements

**Purpose:** Cloud storage on Microsoft Azure

**You need:**

- An active Microsoft Azure subscription
- An Azure Storage account and a Blob container within it
- The storage account name and an access key with permissions to perform file and folder operations

See the [Azure Storage account creation guide](https://learn.microsoft.com/en-us/azure/storage/common/storage-account-create).

### Amazon S3 Requirements

**Purpose:** Cloud storage on Amazon Web Services

**You need:**

- An active AWS account
- An Amazon S3 bucket in a known AWS region
- An IAM access key ID and secret access key with permission to perform file and folder operations

See the [Amazon S3 bucket creation guide](https://docs.aws.amazon.com/AmazonS3/latest/userguide/create-bucket-overview.html).

---

## Configuration

`FILEMANAGER_PROVIDER` is required. The service fails to start if `FILEMANAGER_PROVIDER` is missing, empty, or set to any value other than `azure` or `amazon-s3`.

**From `docker-deployment.md` (lines 80-150):**

### Azure Blob Storage

**Best for:** Enterprises already standardized on Microsoft Azure

Set `FILEMANAGER_PROVIDER` to `azure`, then provide the following variables:

| Environment variable | Required | Description |
|--|--|--|
| `AZURE_ACCOUNT_NAME` | Yes | Azure Storage account name |
| `AZURE_ACCOUNT_KEY` | Yes | Azure Storage account key |
| `AZURE_BLOB_NAME` | Yes | Blob container name. The container must already exist |
| `AZURE_BLOB_PATH` | Yes | Full URL of the Blob container. Example: `https://<account>.blob.core.windows.net/<container>/` |
| `AZURE_FILE_PATH` | Yes | Full URL of the File Manager root path. The folder must exist on startup; the service does not auto-create it. Example: `https://<account>.blob.core.windows.net/<container>/<folder>` |

### Amazon S3

**Best for:** AWS-hosted workloads or S3-compatible storage

Set `FILEMANAGER_PROVIDER` to `amazon-s3`, then provide the following variables:

| Environment variable | Required | Description |
|--|--|--|
| `AWS_ACCESS_KEY_ID` | Yes | AWS IAM access key ID |
| `AWS_SECRET_ACCESS_KEY` | Yes | AWS IAM secret access key |
| `AWS_BUCKET_NAME` | Yes | S3 bucket name. The bucket must already exist |
| `AWS_BUCKET_REGION` | Yes | AWS region that contains the bucket. Example: `us-east-1` |

---

## Docker Deployment

The steps below use Compose. The same configuration works whether you rename the file to `docker-compose.yaml` or keep the `.yml` extension.

**From `docker-deployment.md` (lines 150-280):**

### Step 1: Pull the Unified Provider Image

**Purpose:** Download the latest published image from Docker Hub

```bash
docker pull syncfusion/filemanager-provider:latest
```

### Step 2: Create the `docker-compose.yml` File

**Purpose:** Define the provider service, its environment, and the host port mapping

Use the Compose configuration for the provider you selected. Provide only the matching storage settings.

#### Amazon S3

```yaml
services:
  filemanager-provider:
    image: syncfusion/filemanager-provider:latest
    environment:
      FILEMANAGER_PROVIDER: amazon-s3
      AWS_ACCESS_KEY_ID: YOUR_AWS_ACCESS_KEY_ID
      AWS_SECRET_ACCESS_KEY: YOUR_AWS_SECRET_ACCESS_KEY
      AWS_BUCKET_NAME: YOUR_AWS_BUCKET_NAME
      AWS_BUCKET_REGION: YOUR_AWS_BUCKET_REGION
    ports:
      - "5000:80"
```

#### Azure Blob Storage

```yaml
services:
  filemanager-provider:
    image: syncfusion/filemanager-provider:latest
    environment:
      FILEMANAGER_PROVIDER: azure
      AZURE_ACCOUNT_NAME: YOUR_AZURE_ACCOUNT_NAME
      AZURE_ACCOUNT_KEY: YOUR_AZURE_ACCOUNT_KEY
      AZURE_BLOB_NAME: YOUR_AZURE_BLOB_NAME
      AZURE_BLOB_PATH: https://<account>.blob.core.windows.net/<container>/
      AZURE_FILE_PATH: https://<account>.blob.core.windows.net/<container>/<folder>
    ports:
      - "5000:80"
```

### Step 3: Run the Container

**Purpose:** Start the provider service and confirm it is healthy

In a terminal, navigate to the directory containing `docker-compose.yml` and run:

```bash
docker compose up
```

When you are using the published image directly, omit the `--build` option. To run detached, add `-d`. To follow logs in the foreground, omit `-d` (default).

The service is available at `http://localhost:5000`. Verify that it is running by opening `http://localhost:5000/api/Test`. A successful response confirms that the service is ready to integrate with the File Manager component.

To stop the container, run:

```bash
docker compose down
```

---

## Configure the Client-Side File Manager Component

The unified service uses the same endpoints for both storage providers.

**From `docker-deployment.md` (lines 280-360):**

### Endpoint Mapping

**Purpose:** Point the Blazor client at the running Docker container

Set the [`FileManagerAjaxSettings`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.FileManager.FileManagerAjaxSettings.html) properties as follows:

| Property | Value |
|--|--|
| `Url` | `http://localhost:5000/api/FileManager/FileOperations` |
| `UploadUrl` | `http://localhost:5000/api/FileManager/Upload` |
| `DownloadUrl` | `http://localhost:5000/api/FileManager/Download` |
| `GetImageUrl` | `http://localhost:5000/api/FileManager/GetImage` |

### Razor Example

**Purpose:** Configure `FileManagerAjaxSettings` in a Blazor component

The following example shows a Blazor client that configures the `AjaxSettings` properties in a `.razor` file using the `SfFileManager` component:

```razor
@using Syncfusion.Blazor.FileManager

<div class="control-section">
    <SfFileManager TValue="FileManagerDirectoryContent" AllowDragAndDrop="true">
        <FileManagerAjaxSettings Url="http://localhost:5000/api/FileManager/FileOperations"
                                 UploadUrl="http://localhost:5000/api/FileManager/Upload"
                                 DownloadUrl="http://localhost:5000/api/FileManager/Download"
                                 GetImageUrl="http://localhost:5000/api/FileManager/GetImage">
        </FileManagerAjaxSettings>
    </SfFileManager>
</div>

<style>
    .control-section {
        margin: 0 auto;
        width: 80%;
    }
</style>
```

The server-side Controller does not need to handle the file operations because the unified Docker service handles them. The sample requires the File Manager theme styles. For package installation and CSS references, refer to the [Getting Started](getting-started.md) page.

---

## Troubleshooting

**From `docker-deployment.md` (lines 360-420):**

- **Wrong provider value** - Verify that `FILEMANAGER_PROVIDER` is set to exactly `azure` or `amazon-s3`
- **Missing or mismatched environment variables** - Verify that all required environment variables for the selected provider are present and correct. Do not mix Azure and Amazon S3 settings in the same configuration
- **Port mismatch** - Confirm that the port mapping in `docker-compose.yml` matches the host URL configured in `ajaxSettings`. For example, with `5000:80`, use `http://localhost:5000/`
- **Service fails to start or file operations fail** - Verify the storage account or bucket permissions and confirm that the configured region, container, and paths are correct
- **Intermittent S3 DNS errors** - If you see an intermittent **Name or service not known** error when connecting to the S3 endpoint (e.g. `your-bucket.s3.amazonaws.com`), add `dns: [8.8.8.8, 8.8.4.4, 1.1.1.1]` under the `filemanager` service in `docker-compose.yml` and recreate the container

> ⚠️ **Common Mistake**
>
> Mixing Azure and Amazon S3 environment variables in the same `docker-compose.yml` is a common cause of startup failures. Use only the variable group that matches your `FILEMANAGER_PROVIDER` value.

---

## Next Steps

After deploying the provider service, you can extend the File Manager component:

1. **Start with the basics** - Read [Getting Started](getting-started.md) for installation, NuGet setup, and the first component instance
2. **Pick a data source** - See [Data Binding](data-binding.md) to understand `FileManagerAjaxSettings` and other binding patterns
3. **Review in-process providers** - See [File Providers and Storage](file-providers.md) for Azure / S3 / Google Drive / SharePoint / Firebase / SQL / FTP configuration without Docker
4. **Customize the UI** - See [Customization](customization.md) to tailor the toolbar, context menu, and view modes
5. **Explore advanced scenarios** - Browse [Advanced Features](advanced-features.md) for virtualization, pagination, drag-and-drop, and accessibility
