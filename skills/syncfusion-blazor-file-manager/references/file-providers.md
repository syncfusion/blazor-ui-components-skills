# File Providers and Storage in Blazor FileManager

## Table of Contents

- [Overview](#overview)
- [Physical File Provider](#physical-file-provider)
- [Azure Blob Storage](#azure-blob-storage)
- [Amazon S3](#amazon-s3)
- [Google Drive](#google-drive)
- [SharePoint Online](#sharepoint-online)
- [Firebase Real-time Database](#firebase-real-time-database)
- [SQL Database](#sql-database)
- [FTP Provider](#ftp-provider)

## Overview

File providers allow the FileManager to work with different storage backends. Each provider handles:

- File operations (read, create, delete, rename, etc.)
- Authentication and authorization
- Data transmission and caching
- Error handling

**From various provider documentation (15+ files):**

Choose a provider based on:
- Storage location (local, cloud, database)
- Authentication method (API keys, OAuth, SQL)
- Performance requirements
- Scalability needs

## Physical File Provider

**Best for:** Local file system, on-premises storage

**Features:**
- Direct file system access
- No external authentication needed
- Fast performance for local files
- Complete file operation support

### Setup

From `physical-file-system-provider.md`:

1. **Create Models folder** in server project
2. **Create Controller**

### Example Controller

```csharp
using Microsoft.AspNetCore.Mvc;
using Syncfusion.EJ2.FileManager.Base;
using Syncfusion.EJ2.FileManager.PhysicalFileProvider;

namespace YourApp.Server.Controllers
{
    [Route("api/[controller]")]
    public class FileManagerController : Controller
    {
        public PhysicalFileProvider operation;
        public string basePath;
        string root = "wwwroot\\Files";

        public FileManagerController(IWebHostEnvironment hostingEnvironment)
        {
            this.basePath = hostingEnvironment.ContentRootPath;
            this.operation = new PhysicalFileProvider();
            this.operation.RootFolder(this.basePath + "\\" + this.root);
        }

        [Route("FileOperations")]
        public object FileOperations([FromBody] FileManagerDirectoryContent args)
        {
            switch (args.Action)
            {
                case "read":
                    return this.operation.ToCamelCase(this.operation.GetFiles(args.Path, args.ShowHiddenItems));
                case "delete":
                    return this.operation.ToCamelCase(this.operation.Delete(args.Path, args.Names));
                case "copy":
                    return this.operation.ToCamelCase(this.operation.Copy(args.Path, args.TargetPath, args.Names, args.RenameFiles, args.TargetData));
                case "move":
                    return this.operation.ToCamelCase(this.operation.Move(args.Path, args.TargetPath, args.Names, args.RenameFiles, args.TargetData));
                case "details":
                    return this.operation.ToCamelCase(this.operation.Details(args.Path, args.Names));
                case "create":
                    return this.operation.ToCamelCase(this.operation.Create(args.Path, args.Name));
                case "search":
                    return this.operation.ToCamelCase(this.operation.Search(args.Path, args.SearchString, args.ShowHiddenItems, args.CaseSensitive));
                case "rename":
                    return this.operation.ToCamelCase(this.operation.Rename(args.Path, args.Name, args.NewName));
            }
            return null;
        }

        [Route("Upload")]
        [DisableRequestSizeLimit]
        public IActionResult Upload(string path, long size, IList<IFormFile> uploadFiles, string action)
        {
            FileManagerResponse uploadResponse = operation.Upload(path, uploadFiles, action, size, null);
            if (uploadResponse.Error != null)
            {
                Response.Clear();
                Response.ContentType = "application/json; charset=utf-8";
                Response.StatusCode = Convert.ToInt32(uploadResponse.Error.Code);
            }
            return Content("");
        }

        [Route("Download")]
        public IActionResult Download(string downloadInput)
        {
            FileManagerDirectoryContent args = JsonConvert.DeserializeObject<FileManagerDirectoryContent>(downloadInput);
            return operation.Download(args.Path, args.Names, args.Data);
        }

        [Route("GetImage")]
        public IActionResult GetImage(FileManagerDirectoryContent args)
        {
            return this.operation.GetImage(args.Path, args.Id, false, null, null);
        }
    }
}
```

## Azure Blob Storage

**Best for:** Cloud-based storage, enterprise applications

**Features:**
- Scalable cloud storage
- Access control with SAS tokens
- Integration with Azure services
- Automatic backups

### Setup

From `azure-cloud-file-system-provider.md`:

1. **Create Azure Storage Account**
2. **Install NuGet package**
   ```
   Install-Package Syncfusion.Blazor.FileManager.AzureProvider
   ```
3. **Configure connection string** in `appsettings.json`
4. **Create controller** with AzureProvider

### Example Configuration

```json
{
  "AzureStorageConfig": {
    "AccountName": "your-storage-account-name",
    "AccountKey": "your-storage-account-key",
    "BlobContainerName": "filemanager-files"
  }
}
```

### Example Controller

```csharp
using Syncfusion.EJ2.FileManager.AzureProvider;

[Route("api/[controller]")]
public class AzureProviderController : Controller
{
    private AzureFileProvider operation;

    public AzureProviderController(IConfiguration config)
    {
        var account = config["AzureStorageConfig:AccountName"];
        var key = config["AzureStorageConfig:AccountKey"];
        var container = config["AzureStorageConfig:BlobContainerName"];
        
        this.operation = new AzureFileProvider(account, key, container);
    }

    [Route("FileOperations")]
    public object FileOperations([FromBody] FileManagerDirectoryContent args)
    {
        switch (args.Action)
        {
            case "read":
                return this.operation.ToCamelCase(this.operation.GetFiles(args.Path, args.ShowHiddenItems));
            // ... other operations
        }
        return null;
    }
}
```

## Amazon S3

**Best for:** AWS-based applications, distributed storage

**Features:**
- AWS S3 integration
- Bucket-based organization
- IAM authentication
- CDN integration possible

### Setup

From `amazon-S3-cloud-file-provider.md`:

1. **Create AWS S3 Bucket**
2. **Install NuGet package**
   ```
   Install-Package Syncfusion.Blazor.FileManager.AmazonS3Provider
   ```
3. **Configure AWS credentials**
4. **Create controller** with AmazonS3Provider

### Example Configuration

```json
{
  "AWSConfig": {
    "AccessKey": "your-access-key",
    "SecretKey": "your-secret-key",
    "BucketName": "your-bucket-name",
    "Region": "us-east-1"
  }
}
```

## Google Drive

**Best for:** Google Workspace integration, collaborative storage

**Features:**
- Google Drive API integration
- OAuth authentication
- Shared drive support
- Google Workspace integration

### Setup

From `Google-Drive-file-system-provider.md`:

1. **Set up Google Cloud Project**
2. **Create OAuth 2.0 credentials**
3. **Install NuGet package**
   ```
   Install-Package Syncfusion.Blazor.FileManager.GoogleDriveProvider
   ```
4. **Configure authentication**

### Configuration

```json
{
  "GoogleDriveConfig": {
    "ClientId": "your-client-id",
    "ClientSecret": "your-client-secret",
    "FolderId": "root"
  }
}
```

## SharePoint Online

**Best for:** Microsoft 365 environments, enterprise document management

**Features:**
- SharePoint Online integration
- Office 365 authentication
- Document library support
- Permission-based access control

### Setup

From `sharePoint-file-provider.md`:

1. **Register app in Azure AD**
2. **Get OAuth credentials**
3. **Install NuGet package**
   ```
   Install-Package Syncfusion.Blazor.FileManager.SharePointProvider
   ```
4. **Configure SharePoint connection**

### Configuration

```json
{
  "SharePointConfig": {
    "TenantId": "your-tenant-id",
    "ClientId": "your-client-id",
    "ClientSecret": "your-client-secret",
    "SiteUrl": "url",
    "FolderId": "Document Library"
  }
}
```

## Firebase Real-time Database

**Best for:** Real-time collaborative applications, mobile apps

**Features:**
- Real-time data synchronization
- Firebase Authentication
- Offline support
- Scalable NoSQL storage

### Setup

From `Firebase-Real-time-Database-file-system-provider.md`:

1. **Create Firebase project**
2. **Set up authentication**
3. **Configure database rules**
4. **Use Firebase SDK**

### Example Configuration

```csharp
var options = new FirebaseOptions
{
    Credential = GoogleCredential.FromFile("path/to/serviceAccountKey.json"),
    DatabaseUrl = "url"
};

FirebaseApp.Create(options);
```

## SQL Database

**Best for:** Complex queries, reporting, integration with existing databases

**Features:**
- SQL Server, MySQL, PostgreSQL support
- Complex queries and filtering
- Transaction support
- Existing database integration

### Setup

From `SQL-database-file-system-provider.md`:

1. **Create database tables** for file metadata
2. **Install NuGet package**
   ```
   Install-Package Syncfusion.Blazor.FileManager.SQLProvider
   ```
3. **Configure connection string**
4. **Create provider implementation**

### Example Controller

```csharp
using Syncfusion.EJ2.FileManager.SQLProvider;

[Route("api/[controller]")]
public class SQLProviderController : Controller
{
    private SQLFileProvider operation;

    public SQLProviderController(IConfiguration config)
    {
        string connection = config.GetConnectionString("DefaultConnection");
        this.operation = new SQLFileProvider(connection);
    }

    [Route("FileOperations")]
    public object FileOperations([FromBody] FileManagerDirectoryContent args)
    {
        switch (args.Action)
        {
            case "read":
                return this.operation.ToCamelCase(this.operation.GetFiles(args.Path, args.ShowHiddenItems));
            // ... other operations
        }
        return null;
    }
}
```

## FTP Provider

**Best for:** Legacy systems, FTP-based file storage

**Features:**
- FTP server integration
- Secure SFTP support
- Remote file access
- Cross-platform compatibility

### Setup

From `File-Transfer-Protocol-file-system-provider.md`:

1. **Configure FTP server connection**
2. **Set up credentials**
3. **Create FTP provider**

### Example Configuration

```csharp
var ftpConfig = new FTPConfig
{
    Host = "ftp.example.com",
    Port = 21,
    Username = "username",
    Password = "password",
    RootPath = "/public_html",
    UseSSL = true
};
```

## Provider Selection Guide

| Provider | Storage Type | Scale | Cost | Ease | Best For |
|--|--|--|--|--|--|
| **Physical** | Local/Network | Small-Medium | None | Easy | On-premises, simple setups |
| **Azure** | Cloud | Large | Medium | Medium | Azure environments |
| **S3** | Cloud | Large | Low | Medium | AWS environments |
| **Google Drive** | Cloud | Medium | Free* | Easy | Google Workspace |
| **SharePoint** | Cloud | Large | Included | Hard | Microsoft 365 |
| **Firebase** | Cloud | Medium | Free* | Medium | Real-time apps |
| **SQL** | Database | Large | Varies | Hard | Complex queries |
| **FTP** | Remote | Small-Medium | Low | Medium | Legacy systems |

*Free tier available

## Common Provider Patterns

### Pattern 1: Multi-Provider Setup

```csharp
public IFileProvider GetProvider(string providerType)
{
    return providerType switch
    {
        "physical" => new PhysicalFileProvider(),
        "azure" => new AzureFileProvider(config),
        "s3" => new AmazonS3FileProvider(config),
        "sql" => new SQLFileProvider(config),
        _ => throw new NotSupportedException()
    };
}
```

### Pattern 2: Provider with Caching

```csharp
var baseProvider = new AzureFileProvider(config);
var cachedProvider = new CachedFileProvider(baseProvider);
```

### Pattern 3: Provider with Logging

```csharp
public class LoggedFileProvider : IFileProvider
{
    private readonly IFileProvider _inner;
    private readonly ILogger _logger;

    public async Task<FileManagerResponse> GetFilesAsync(string path)
    {
        _logger.Log($"Getting files from {path}");
        return await _inner.GetFilesAsync(path);
    }
}
```

## Next Steps

- See [Getting Started](getting-started.md) for provider setup
- Check [File Operations](file-operations.md) for operation details
- Review [Data Binding](data-binding.md) for integration patterns
