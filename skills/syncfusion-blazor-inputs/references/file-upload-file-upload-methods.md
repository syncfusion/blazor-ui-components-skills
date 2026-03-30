# Upload Methods & Behavior

## Table of Contents
- [Asynchronous Upload](#async)
- [Synchronous Upload](#sync)
- [Save URL Configuration](#save-url)
- [Remove URL Configuration](#remove-url)
- [Manual Upload Button](#manual)
- [Upload Method Reference](#methods)
- [Upload Success/Failure](#results)
- [Backend API Implementation](#backend)

---

## Asynchronous Upload

Asynchronous upload processes files on the server without blocking the user interface. This is the recommended approach for web applications.

### Configuration

```razor
<SfUploader AutoUpload="true">
    <UploaderAsyncSettings 
        SaveUrl="https://your-api.com/api/upload/save" 
        RemoveUrl="https://your-api.com/api/upload/remove">
    </UploaderAsyncSettings>
</SfUploader>
```

### How It Works

1. User selects file
2. Component sends HTTP POST request to `SaveUrl`
3. Server processes file asynchronously
4. UI remains responsive during upload
5. File removal handled by `RemoveUrl`

### Advantages
- Non-blocking UI experience
- Supports large files
- Better user experience
- Progress tracking possible
- Can pause/resume with chunking

### Use Case
Most modern web applications use async uploads for better responsiveness.

---

## Synchronous Upload

Synchronous upload is simpler but less common. Files are processed directly in the `ValueChange` event without server-side API.

```razor
@using Syncfusion.Blazor.Inputs
@using System.IO

<SfUploader AutoUpload="true">
    <UploaderEvents ValueChange="@OnChange"></UploaderEvents>
</SfUploader>

@code {
    private async Task OnChange(UploadChangeEventArgs args)
    {
        try
        {
            foreach (var file in args.Files)
            {
                var uploadsFolder = Path.Combine(
                    Directory.GetCurrentDirectory(), 
                    "wwwroot", 
                    "uploads"
                );
                
                if (!Directory.Exists(uploadsFolder))
                {
                    Directory.CreateDirectory(uploadsFolder);
                }

                var filePath = Path.Combine(uploadsFolder, file.FileInfo.Name);

                await using (var fileStream = new FileStream(
                    filePath, 
                    FileMode.Create, 
                    FileAccess.Write))
                {
                    await file.File.OpenReadStream(long.MaxValue)
                        .CopyToAsync(fileStream);
                }
                
                Console.WriteLine($"File saved: {file.FileInfo.Name}");
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error: {ex.Message}");
        }
    }
}
```

### Important Considerations
- **Blazor Server**: Files saved on server
- **Blazor WebAssembly**: Requires backend API
- Requires file system write permissions
- Not recommended for large files

---

## Save URL Configuration

The `SaveUrl` property points to your server API endpoint that handles file uploads.

### Basic Configuration

```razor
<SfUploader>
    <UploaderAsyncSettings SaveUrl="api/upload/save">
    </UploaderAsyncSettings>
</SfUploader>
```

### Full URL Example

```razor
<SfUploader>
    <UploaderAsyncSettings 
        SaveUrl="https://myapi.example.com/api/FileUpload/Save">
    </UploaderAsyncSettings>
</SfUploader>
```

### CORS Configuration (if cross-origin)

In your API `Program.cs`:

```csharp
// Enable CORS
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowUpload", builder =>
    {
        builder.AllowAnyOrigin()
               .AllowAnyMethod()
               .AllowAnyHeader();
    });
});

app.UseCors("AllowUpload");
```

### Backend API Implementation

```csharp
[ApiController]
[Route("api/[controller]")]
public class UploadController : ControllerBase
{
    private readonly IHostingEnvironment _hostingEnv;

    public UploadController(IHostingEnvironment env)
    {
        _hostingEnv = env;
    }

    [HttpPost("[action]")]
    public async Task Save(IList<IFormFile> UploadFiles)
    {
        try
        {
            var uploadsPath = Path.Combine(
                _hostingEnv.ContentRootPath, 
                "uploads"
            );
            
            if (!Directory.Exists(uploadsPath))
                Directory.CreateDirectory(uploadsPath);

            foreach (var file in UploadFiles)
            {
                if (file.Length > 0)
                {
                    var fileName = Path.GetFileName(file.FileName);
                    var filePath = Path.Combine(uploadsPath, fileName);

                    using (var fileStream = new FileStream(
                        filePath, 
                        FileMode.Create, 
                        FileAccess.Write))
                    {
                        await file.CopyToAsync(fileStream);
                    }
                }
            }
        }
        catch (Exception ex)
        {
            Response.StatusCode = 400;
            await Response.WriteAsync($"Upload failed: {ex.Message}");
        }
    }
}
```

---

## Remove URL Configuration

The `RemoveUrl` property handles file deletion. It's optional but recommended for complete file management.

### Configuration

```razor
<SfUploader>
    <UploaderAsyncSettings 
        SaveUrl="api/upload/save"
        RemoveUrl="api/upload/remove">
    </UploaderAsyncSettings>
</SfUploader>
```

### Backend Remove Handler

```csharp
[HttpPost("[action]")]
public void Remove(IList<IFormFile> UploadFiles)
{
    try
    {
        var uploadsPath = Path.Combine(
            _hostingEnv.ContentRootPath, 
            "uploads"
        );

        foreach (var file in UploadFiles)
        {
            var filePath = Path.Combine(uploadsPath, file.FileName);
            
            if (System.IO.File.Exists(filePath))
            {
                System.IO.File.Delete(filePath);
                Console.WriteLine($"File deleted: {file.FileName}");
            }
        }
    }
    catch (Exception ex)
    {
        Response.StatusCode = 400;
        await Response.WriteAsync($"Deletion failed: {ex.Message}");
    }
}
```

---

## Manual Upload Button

Control upload timing with `AutoUpload="false"` and trigger manually:

```razor
<SfUploader @ref="uploader" AutoUpload="false">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
</SfUploader>

<button class="btn btn-primary" @onclick="UploadFiles">Upload</button>
<button class="btn btn-secondary" @onclick="ClearFiles">Clear</button>

@code {
    SfUploader uploader;

    private async Task UploadFiles()
    {
        if (uploader != null)
        {
            await uploader.UploadAsync();
        }
    }

    private async Task ClearFiles()
    {
        if (uploader != null)
        {
            await uploader.ClearAllAsync();
        }
    }
}
```

---

## Upload Method Reference

### Key Methods

| Method | Purpose | Example |
|--------|---------|---------|
| `UploadAsync()` | Manually trigger upload | `await uploader.UploadAsync();` |
| `ClearAllAsync()` | Clear selected files | `await uploader.ClearAllAsync();` |
| `PauseAsync()` | Pause active upload (chunked) | `await uploader.PauseAsync();` |
| `ResumeAsync()` | Resume paused upload (chunked) | `await uploader.ResumeAsync();` |
| `RemoveAsync()` | Remove specific file | `await uploader.RemoveAsync(selectedFiles);` |

### Usage Example

```razor
<SfUploader @ref="uploader" AutoUpload="false">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
</SfUploader>

<button @onclick="@(async () => await uploader.UploadAsync())">Upload</button>
<button @onclick="@(async () => await uploader.ClearAllAsync())">Clear</button>
<button @onclick="@(async () => await uploader.PauseAsync())">Pause</button>
<button @onclick="@(async () => await uploader.ResumeAsync())">Resume</button>

@code {
    SfUploader uploader;
}
```

---

## Upload Success/Failure

Handle upload results with events:

```razor
<SfUploader @ref="uploader" AutoUpload="true">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderEvents 
        Success="@OnUploadSuccess"
        OnFailure="@OnUploadFailure">
    </UploaderEvents>
</SfUploader>

<div>@statusMessage</div>

@code {
    SfUploader uploader;
    string statusMessage = "";

    private void OnUploadSuccess(SuccessEventArgs args)
    {
        statusMessage = $"Upload successful: {args.File.Name}";
        Console.WriteLine(statusMessage);
    }

    private void OnUploadFailure(FailureEventArgs args)
    {
        statusMessage = $"Upload failed: {args.File.Name} - {args.StatusText}";
        Console.WriteLine(statusMessage);
    }
}
```

### Event Arguments

**SuccessEventArgs**
- `File` - FileInfo object containing file details
  - `File.Name` - Name of uploaded file
  - `File.Size` - Size in bytes
- `Response` - Server response data
- `StatusText` - HTTP status text
- `ChunkIndex` - Current chunk index (for chunked uploads)
- `TotalChunk` - Total number of chunks

**FailureEventArgs**
- `File` - FileInfo object of failed file
  - `File.Name` - Name of file that failed
- `StatusText` - Error message/details
- `Response` - Server response
- `RetryFiles` - Array of files to retry

---

## Backend

### File Size Limits

Set server-side limits in `Program.cs`:

```csharp
builder.Services.Configure<FormOptions>(options =>
{
    options.MultipartBodyLengthLimit = 104857600; // 100 MB
    options.ValueLengthLimit = 104857600;
    options.MemoryBufferThreshold = 104857600;
});
```

### Security Considerations

1. **Validate File Type** (server-side):
```csharp
var allowedExtensions = new[] { ".jpg", ".png", ".pdf" };
var ext = Path.GetExtension(file.FileName).ToLower();
if (!allowedExtensions.Contains(ext))
    throw new InvalidOperationException("File type not allowed");
```

2. **Validate File Size**:
```csharp
if (file.Length > 104857600) // 100 MB
    throw new InvalidOperationException("File too large");
```

3. **Sanitize File Names**:
```csharp
var fileName = Path.GetFileName(file.FileName);
fileName = Regex.Replace(fileName, @"[^\w\s.-]", "");
```

4. **Store Outside Web Root**:
```csharp
// Store in secure location, not wwwroot
var uploadPath = Path.Combine("/secure/uploads", fileName);
```

---

## Complete Example

```razor
@page "/upload"
@using Syncfusion.Blazor.Inputs

<div class="container">
    <h3>File Upload with Manual Trigger</h3>
    
    <SfUploader @ref="uploader" 
                AutoUpload="false"
                AllowMultiple="true">
        <UploaderAsyncSettings 
            SaveUrl="api/upload/save"
            RemoveUrl="api/upload/remove">
        </UploaderAsyncSettings>
        <UploaderEvents 
            Success="@OnSuccess"
            OnFailure="@OnFailure">
        </UploaderEvents>
    </SfUploader>

    <div class="mt-3">
        <button class="btn btn-primary" @onclick="Upload">Upload Files</button>
        <button class="btn btn-secondary" @onclick="Clear">Clear</button>
    </div>

    @if (!string.IsNullOrEmpty(message))
    {
        <div class="alert alert-info mt-3">@message</div>
    }
</div>

@code {
    SfUploader uploader;
    string message = "";

    private async Task Upload() => await uploader.UploadAsync();
    private async Task Clear() => await uploader.ClearAllAsync();

    private void OnSuccess(SuccessEventArgs args) 
        => message = $"✓ {args.File.Name} uploaded successfully";

    private void OnFailure(FailureEventArgs args) 
        => message = $"✗ {args.File.Name} failed: {args.StatusText}";
}
```
