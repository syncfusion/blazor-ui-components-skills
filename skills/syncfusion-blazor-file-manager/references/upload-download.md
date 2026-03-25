# Upload and Download in Blazor FileManager

## Table of Contents

- [Overview](#overview)
- [File Upload Configuration](#file-upload-configuration)
- [Upload Settings Properties](#upload-settings-properties)
- [Directory Upload](#directory-upload)
- [Chunk Upload](#chunk-upload)
- [File Download](#file-download)
- [Large File Handling](#large-file-handling)
- [Upload Events](#upload-events)
- [Download Events](#download-events)
- [Complete Upload/Download Example](#complete-uploaddownload-example)

## Overview

The FileManager provides comprehensive file upload and download capabilities with:

- Single and multiple file upload
- Directory (folder) upload
- Sequential or parallel uploads
- Chunked upload for large files
- ZIP download for multiple files
- Progress tracking
- Event-based customization

## File Upload Configuration

### Basic Upload Setup

From `upload.md`:

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

### Upload Endpoint

The backend upload endpoint handles file operations:

```csharp
[Route("Upload")]
[DisableRequestSizeLimit]
public IActionResult Upload(string path, long size, IList<IFormFile> uploadFiles, string action)
{
    try
    {
        FileManagerResponse uploadResponse;
        foreach (var file in uploadFiles)
        {
            var folders = (file.FileName).Split('/');
            // Handle directory upload
            if (folders.Length > 1)
            {
                for (var i = 0; i < folders.Length - 1; i++)
                {
                    string newDirectoryPath = Path.Combine(basePath + path, folders[i]);
                    if (!Directory.Exists(newDirectoryPath))
                    {
                        Directory.CreateDirectory(newDirectoryPath);
                    }
                    path += folders[i] + "/";
                }
            }
        }
        uploadResponse = operation.Upload(path, uploadFiles, action, size, null);
        if (uploadResponse.Error != null)
        {
            Response.Clear();
            Response.ContentType = "application/json; charset=utf-8";
            Response.StatusCode = Convert.ToInt32(uploadResponse.Error.Code);
            Response.HttpContext.Features.Get<IHttpResponseFeature>().ReasonPhrase = uploadResponse.Error.Message;
        }
    }
    catch (Exception e)
    {
        ErrorDetails er = new ErrorDetails();
        er.Message = e.Message.ToString();
        er.Code = "417";
        Response.Clear();
        Response.ContentType = "application/json; charset=utf-8";
        Response.StatusCode = Convert.ToInt32(er.Code);
        return Content("");
    }
    return Content("");
}
```

## Upload Settings Properties

### FileManagerUploadSettings Properties

From `upload.md`:

| Property | Type | Default | Purpose |
|--|--|--|--|
| `DirectoryUpload` | bool | false | Enable/disable folder upload |
| `AllowedExtensions` | string | "*" | Comma-separated allowed file extensions (e.g., ".pdf,.doc,.docx") |
| `MinFileSize` | long | 0 | Minimum file size in bytes |
| `MaxFileSize` | long | long.MaxValue | Maximum file size in bytes |
| `SequentialUpload` | bool | false | Upload files sequentially instead of parallel |
| `ChunkSize` | long | 0 | File chunk size for chunked upload (0 = disabled) |
| `AutoUpload` | bool | true | Auto-upload files when dropped/selected |
| `AutoClose` | bool | false | Auto-close upload dialog after upload completes |
| `UploadMode` | UploadMode | FormSubmit | Upload method (FormSubmit or HttpClient) |

### Basic Upload Settings Configuration

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerUploadSettings DirectoryUpload="true" 
                               AllowedExtensions=".pdf,.doc,.docx,.xlsx"
                               MaxFileSize="10485760"
                               AutoUpload="true">
    </FileManagerUploadSettings>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

### AllowedExtensions Example

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerUploadSettings AllowedExtensions=".pdf,.jpg,.png,.docx,.xlsx"
                               MaxFileSize="5242880">
    </FileManagerUploadSettings>
    <!-- Rest of configuration -->
</SfFileManager>
```

### UploadMode Configuration

The `UploadMode` property defines the method used to perform the upload operation. Choose between `FormSubmit` (default) or `HttpClient`:

```razor
@using Syncfusion.Blazor.FileManager
@using System.Net.Http.Headers

<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerUploadSettings UploadMode="UploadMode.HttpClient"></FileManagerUploadSettings>
    <FileManagerEvents TValue="FileManagerDirectoryContent" OnSend="OnBeforeSend"></FileManagerEvents>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    private IApiAuthTokenService ApiAuthTokenService { get; set; }
    
    private async Task OnBeforeSend(BeforeSendEventArgs args)
    {
        var token = await ApiAuthTokenService.GetToken();
        args.HttpClientInstance.DefaultRequestHeaders.Add("Authorization", "Bearer " + token);
    }
}
```

**UploadMode Options:**
- `FormSubmit` (default): Uses traditional form submission for uploads
- `HttpClient`: Uses HttpClient instance for more control over headers and authorization

## Directory Upload

### Enable Directory Upload

From `upload.md`:

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerUploadSettings DirectoryUpload="true">
    </FileManagerUploadSettings>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

### Handle Directory Structure in Upload Endpoint

```csharp
[Route("Upload")]
[DisableRequestSizeLimit]
public IActionResult Upload(string path, long size, IList<IFormFile> uploadFiles, string action)
{
    try
    {
        foreach (var file in uploadFiles)
        {
            var folders = file.FileName.Split('/');
            
            // Create subdirectories for uploaded folder structure
            if (folders.Length > 1)
            {
                for (int i = 0; i < folders.Length - 1; i++)
                {
                    string directoryPath = Path.Combine(basePath + path, folders[i]);
                    if (!Directory.Exists(directoryPath))
                    {
                        Directory.CreateDirectory(directoryPath);
                    }
                    path += folders[i] + "/";
                }
            }
            
            // Save file
            string filePath = Path.Combine(basePath + path, file.FileName);
            using (FileStream filestream = System.IO.File.Create(filePath))
            {
                file.CopyTo(filestream);
                filestream.Flush();
            }
        }
        return Content("");
    }
    catch (Exception e)
    {
        // Error handling
        return BadRequest(e.Message);
    }
}
```

## Chunk Upload

### Enable Chunked Upload

From `upload.md`:

Configure `ChunkSize` for large files:

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerUploadSettings ChunkSize="1048576"
                               SequentialUpload="true">
    </FileManagerUploadSettings>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

**Note:** ChunkSize of 1048576 = 1 MB chunks

### Handle Chunked Upload in Backend

```csharp
[Route("Upload")]
[DisableRequestSizeLimit]
public IActionResult Upload(string path, long size, IList<IFormFile> uploadFiles, 
    string action, long chunkStartByte = 0, string chunkFile = null)
{
    try
    {
        foreach (var file in uploadFiles)
        {
            string filePath = Path.Combine(basePath + path, file.FileName);
            
            // Append chunk to existing file
            if (chunkStartByte > 0 || !string.IsNullOrEmpty(chunkFile))
            {
                using (FileStream filestream = System.IO.File.Create(filePath, 
                    (int)file.Length, FileOptions.SequentialScan))
                {
                    file.CopyTo(filestream);
                }
            }
            else
            {
                // First chunk - create new file
                using (FileStream filestream = System.IO.File.Create(filePath))
                {
                    file.CopyTo(filestream);
                }
            }
        }
        return Content("");
    }
    catch (Exception e)
    {
        return BadRequest(e.Message);
    }
}
```

## File Download

### Programmatic Download

```razor
<SfButton OnClick="DownloadFiles">Download Selected</SfButton>

<SfFileManager @ref="FileManager" TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    SfFileManager<FileManagerDirectoryContent> FileManager;

    public async Task DownloadFiles()
    {
        if (FileManager?.SelectedItems.Count > 0)
        {
            // Public method: DownloadFilesAsync(IList<FileManagerDirectoryContent> selectedItems)
            await FileManager.DownloadFilesAsync(FileManager.SelectedItems);
        }
    }
}
```

### Download Single File

```csharp
[Route("Download")]
public FileStreamResult Download(string downloadInput)
{
    // Parse download input (file name or path)
    FileStream fileStream = System.IO.File.OpenRead(Path.Combine(basePath, downloadInput));
    return File(fileStream, "APPLICATION/octet-stream", downloadInput);
}
```

### ZIP Multiple Files

```csharp
[Route("Download")]
public FileStreamResult Download(string downloadInput)
{
    try
    {
        // Parse file list from downloadInput
        string[] filesToDownload = JsonConvert.DeserializeObject<string[]>(downloadInput);
        
        if (filesToDownload.Length == 1)
        {
            FileStream fileStream = System.IO.File.OpenRead(Path.Combine(basePath, filesToDownload[0]));
            return File(fileStream, "APPLICATION/octet-stream", 
                Path.GetFileName(filesToDownload[0]));
        }
        else
        {
            // Create ZIP archive
            MemoryStream zipStream = new MemoryStream();
            using (ZipArchive archive = new ZipArchive(zipStream, ZipArchiveMode.Create, true))
            {
                foreach (var file in filesToDownload)
                {
                    string fullPath = Path.Combine(basePath, file);
                    if (System.IO.File.Exists(fullPath))
                    {
                        archive.CreateEntryFromFile(fullPath, Path.GetFileName(file));
                    }
                }
            }
            zipStream.Position = 0;
            return File(zipStream, "application/zip", "download.zip");
        }
    }
    catch (Exception e)
    {
        // Error handling
        throw;
    }
}
```

## Large File Handling

### Configure Large File Support

From `upload.md`:

Add to Program.cs or Startup.cs:

```csharp
// Configure request size limit
services.Configure<FormOptions>(options =>
{
    options.ValueLengthLimit = int.MaxValue;
    options.MultipartBodyLengthLimit = long.MaxValue;
    options.MemoryBufferThreshold = int.MaxValue;
});

// Configure Kestrel for large requests
services.Configure<KestrelServerOptions>(options =>
{
    options.Limits.MaxRequestBodySize = null;  // No limit
});
```

### Large File Upload Endpoint

```csharp
[Route("Upload")]
[DisableRequestSizeLimit]
[RequestFormLimits(ValueLengthLimit = int.MaxValue, 
    MultipartBodyLengthLimit = long.MaxValue)]
public IActionResult Upload(string path, long size, IList<IFormFile> uploadFiles, string action)
{
    // Upload implementation with chunking support
    try
    {
        foreach (var file in uploadFiles)
        {
            string filePath = Path.Combine(basePath + path, file.FileName);
            using (FileStream filestream = System.IO.File.Create(filePath))
            {
                file.CopyTo(filestream);
            }
        }
        return Content("");
    }
    catch (Exception e)
    {
        return StatusCode(413, "File too large");
    }
}
```

## Upload Events

### ItemsUploading Event

Triggered before upload starts - useful for validation:

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerEvents TValue="FileManagerDirectoryContent" ItemsUploading="OnItemsUploading">
    </FileManagerEvents>
    <FileManagerUploadSettings AllowedExtensions=".pdf,.doc,.docx"
                               MaxFileSize="5242880">
    </FileManagerUploadSettings>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    public void OnItemsUploading(ItemsUploadingEventArgs<FileManagerDirectoryContent> args)
    {
        // Validate before upload
        foreach (var file in args.FileDetails)
        {
            if (file.Size > 10485760)  // 10 MB
            {
                args.Cancel = true;
                Console.WriteLine("File exceeds size limit");
                break;
            }
        }
    }
}
```

### ItemsUploaded Event

Triggered after successful upload:

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerEvents TValue="FileManagerDirectoryContent" ItemsUploaded="OnItemsUploaded">
    </FileManagerEvents>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    public void OnItemsUploaded(ItemsUploadedEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"Uploaded {args.FileDetails.Count} files");
        foreach (var file in args.FileDetails)
        {
            Console.WriteLine($"File: {file.Name}, Size: {file.Size}");
        }
    }
}
```

### UploadListCreated Event

Triggered when upload list is created:

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerEvents TValue="FileManagerDirectoryContent" UploadListCreated="OnUploadListCreated">
    </FileManagerEvents>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    public void OnUploadListCreated(UploadListCreatedEventArgs args)
    {
        Console.WriteLine($"Upload list created with {args.FileDetails.Count} files");
    }
}
```

## Download Events

### BeforeDownload Event

Triggered before download - useful for preventing certain file types:

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerEvents TValue="FileManagerDirectoryContent" BeforeDownload="OnBeforeDownload">
    </FileManagerEvents>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    public void OnBeforeDownload(BeforeDownloadEventArgs<FileManagerDirectoryContent> args)
    {
        // Prevent download of executable files
        string[] restrictedExtensions = { ".exe", ".bat", ".cmd", ".com" };
        
        foreach (var file in args.FileDetails)
        {
            string extension = Path.GetExtension(file.Name).ToLower();
            if (restrictedExtensions.Contains(extension))
            {
                args.Cancel = true;
                Console.WriteLine($"Download blocked for {file.Name}");
                break;
            }
        }
    }
}
```

### BeforeImageLoad Event

Triggered before image preview is loaded:

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerEvents TValue="FileManagerDirectoryContent" BeforeImageLoad="OnBeforeImageLoad">
    </FileManagerEvents>
    <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                             UploadUrl="/api/FileManager/Upload"
                             DownloadUrl="/api/FileManager/Download"
                             GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>

@code {
    public void OnBeforeImageLoad(BeforeImageLoadEventArgs args)
    {
        Console.WriteLine($"Loading image: {args.File}");
    }
}
```

## Complete Upload/Download Example

```razor
@using Syncfusion.Blazor.FileManager
@using Syncfusion.Blazor.Buttons

<div style="height: 600px;">
    <SfFileManager @ref="FileManager" TValue="FileManagerDirectoryContent">
        
        <FileManagerUploadSettings DirectoryUpload="true"
                                   AllowedExtensions=".pdf,.doc,.docx,.xlsx,.jpg,.png"
                                   MaxFileSize="10485760"
                                   SequentialUpload="false"
                                   ChunkSize="1048576"
                                   AutoUpload="true">
        </FileManagerUploadSettings>

        <FileManagerEvents TValue="FileManagerDirectoryContent"
                          ItemsUploading="OnItemsUploading"
                          ItemsUploaded="OnItemsUploaded"
                          BeforeDownload="OnBeforeDownload">
        </FileManagerEvents>

        <FileManagerAjaxSettings Url="/api/FileManager/FileOperations"
                                 UploadUrl="/api/FileManager/Upload"
                                 DownloadUrl="/api/FileManager/Download"
                                 GetImageUrl="/api/FileManager/GetImage">
        </FileManagerAjaxSettings>
    </SfFileManager>
</div>

<div style="margin-top: 20px;">
    <SfButton OnClick="DownloadSelected">Download Selected Files</SfButton>
    <span style="margin-left: 20px;">Selected: @(FileManager?.SelectedItems.Count ?? 0) files</span>
</div>

@code {
    SfFileManager<FileManagerDirectoryContent> FileManager;

    public void OnItemsUploading(ItemsUploadingEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"Uploading {args.FileDetails.Count} files");
    }

    public void OnItemsUploaded(ItemsUploadedEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"Successfully uploaded {args.FileDetails.Count} files");
    }

    public void OnBeforeDownload(BeforeDownloadEventArgs<FileManagerDirectoryContent> args)
    {
        Console.WriteLine($"Downloading {args.FileDetails.Count} files");
    }

    public async Task DownloadSelected()
    {
        if (FileManager?.SelectedItems.Count > 0)
        {
            await FileManager.DownloadFilesAsync(FileManager.SelectedItems);
        }
    }
}
```

## Next Steps

- Check [Events and Callbacks](events-and-callbacks.md) for event patterns
- Review [File Providers](file-providers.md) for provider-specific upload handling
- See [Advanced Features](advanced-features.md) for pagination and filtering
