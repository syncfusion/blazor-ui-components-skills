# Upload and Download in Blazor FileManager

## Table of Contents
- [Overview](#overview)
- [File Upload Configuration](#file-upload-configuration)
- [Upload Settings Properties](#upload-settings-properties)
- [Directory Upload](#directory-upload)
- [Chunk Upload](#chunk-upload)
- [File Download](#file-download)
- [Upload Events](#upload-events)
- [Download Events](#download-events)
- [Complete Upload/Download Example](#complete-uploaddownload-example)

---

## Overview

The FileManager provides upload and download capabilities such as:

- Single and multiple file upload
- Directory (folder) selection
- Sequential or parallel uploads
- Chunked upload for large files
- ZIP download for multiple files
- Progress tracking
- Event-based customization

---

## File Upload Configuration

### Basic Upload Setup

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerAjaxSettings
        Url="/api/FileManager/FileOperations"
        UploadUrl="/api/FileManager/Upload"
        DownloadUrl="/api/FileManager/Download"
        GetImageUrl="/api/FileManager/GetImage">
    </FileManagerAjaxSettings>
</SfFileManager>
```

---

### Secure Upload Endpoint (Recommended Pattern)

```csharp
[HttpPost("upload")]
public async Task<IActionResult> UploadAsync(IFormFile file)
{
    if (file == null || file.Length == 0)
    {
        return BadRequest("No file uploaded.");
    }

    const long maxAllowedSize = 50 * 1024 * 1024; // 50 MB
    if (file.Length > maxAllowedSize)
    {
        return BadRequest("File size exceeds allowed limit.");
    }

    var basePath = Path.GetFullPath(Path.Combine(Directory.GetCurrentDirectory(), "Uploads"));
    Directory.CreateDirectory(basePath);

    var safeFileName = Path.GetFileName(file.FileName);
    var fullPath = Path.GetFullPath(Path.Combine(basePath, safeFileName));

    if (!fullPath.StartsWith(basePath, StringComparison.Ordinal))
    {
        return Unauthorized("Invalid file path.");
    }

    using var stream = new FileStream(fullPath, FileMode.CreateNew, FileAccess.Write, FileShare.None);
    await file.CopyToAsync(stream);

    return Ok(new { FileName = safeFileName });
}
```

---

## Upload Settings Properties

### FileManagerUploadSettings Properties

- `DirectoryUpload` – Enables selection of folders on the client
- `AllowedExtensions` – Allowed file extensions
- `MinFileSize` – Minimum file size
- `MaxFileSize` – Maximum file size (enforced client-side)
- `SequentialUpload` – Upload files sequentially
- `ChunkSize` – Chunk size for large uploads
- `AutoUpload` – Auto-start upload

```razor
<FileManagerUploadSettings
    DirectoryUpload="true"
    AllowedExtensions=".pdf,.doc,.docx,.xlsx"
    MaxFileSize="10485760"
    AutoUpload="true">
</FileManagerUploadSettings>
```

---

## Directory Upload

Directory selection can be enabled on the client using `DirectoryUpload`.

> ⚠️ **Security Constraint**
>
> Server-side handling of directory uploads must validate and canonicalize paths.
> Unsafe examples using client-controlled paths are intentionally omitted from this reference.

---

## Chunk Upload

### Enable Chunked Upload

```razor
<FileManagerUploadSettings
    ChunkSize="1048576"
    SequentialUpload="true">
</FileManagerUploadSettings>
```

### Safe Chunk Handling (Reference Pattern)

```csharp
var tempDirectory = Path.Combine(basePath, "temp");
Directory.CreateDirectory(tempDirectory);

var tempFilePath = Path.Combine(tempDirectory, uploadId + ".tmp");

using (var stream = new FileStream(tempFilePath, FileMode.Append, FileAccess.Write, FileShare.None))
{
    await chunk.CopyToAsync(stream);
}

File.Move(tempFilePath, finalPath, overwrite: false);
```

---

## File Download

### Secure Single File Download

```csharp
[HttpGet("download")]
public IActionResult Download(string fileName)
{
    var basePath = Path.GetFullPath(Path.Combine(Directory.GetCurrentDirectory(), "Uploads"));
    var safeFileName = Path.GetFileName(fileName);
    var fullPath = Path.GetFullPath(Path.Combine(basePath, safeFileName));

    if (!fullPath.StartsWith(basePath, StringComparison.Ordinal) || !System.IO.File.Exists(fullPath))
    {
        return NotFound();
    }

    return PhysicalFile(fullPath, "application/octet-stream", safeFileName);
}
```

---

## Upload Events

```razor
<FileManagerEvents ItemsUploading="OnItemsUploading" ItemsUploaded="OnItemsUploaded" />
```

---

## Download Events

```razor
<FileManagerEvents BeforeDownload="OnBeforeDownload" />
```

---

## Complete Upload/Download Example

```razor
<SfFileManager TValue="FileManagerDirectoryContent">
    <FileManagerUploadSettings
        DirectoryUpload="true"
        AllowedExtensions=".pdf,.doc,.docx"
        MaxFileSize="10485760"
        ChunkSize="1048576" />
</SfFileManager>
```

---

## Next Steps

- Review `events-and-callbacks.md` for event patterns
- Review `file-providers.md` for storage customization
- See `advanced-features.md` for filtering and pagination

---
