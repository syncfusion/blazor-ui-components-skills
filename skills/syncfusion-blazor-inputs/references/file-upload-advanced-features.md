# Advanced Upload Features

## Table of Contents
- [Chunked Upload](#chunks)
- [Pause & Resume](#pause)
- [MemoryStream Processing](#memory)
- [Batch Operations](#batch)
- [Async/Await Patterns](#async)
- [Concurrent vs Sequential](#concurrent)
- [Error Recovery](#recovery)
- [Performance Optimization](#perf)

---

## Chunked Upload

Chunked upload breaks large files into smaller pieces (chunks), allowing reliable uploads over unreliable networks. If a chunk fails, only that chunk is retransmitted, not the entire file.

### Configuration

Enable chunking by setting `ChunkSize` in `UploaderAsyncSettings`:

```razor
<SfUploader ID="UploadFiles">
    <UploaderAsyncSettings 
        SaveUrl="api/upload/save" 
        RemoveUrl="api/upload/remove" 
        ChunkSize="500000"
        RetryCount="5" 
        RetryAfterDelay="3000">
    </UploaderAsyncSettings>
</SfUploader>
```

### Chunk Configuration Properties

| Property | Type | Purpose |
|----------|------|---------|
| `ChunkSize` | Double | Size of each chunk in bytes (0 = disabled) |
| `RetryCount` | Int32 | Number of retries after failed chunk |
| `RetryAfterDelay` | Double | Delay in milliseconds before retry attempt |

### Common Chunk Sizes

```csharp
public static class ChunkSizes
{
    public const long Size100KB = 102_400;      // 100 KB
    public const long Size500KB = 512_000;      // 500 KB
    public const long Size1MB = 1_048_576;      // 1 MB
    public const long Size5MB = 5_242_880;      // 5 MB
    public const long Size10MB = 10_485_760;    // 10 MB
}
```

### Backend Handler for Chunks

```csharp
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
            var fileName = Path.GetFileName(file.FileName);
            var filePath = Path.Combine(uploadsPath, fileName);

            using (var fileStream = new FileStream(
                filePath, 
                FileMode.Append, 
                FileAccess.Write))
            {
                await file.CopyToAsync(fileStream);
            }
        }
    }
    catch (Exception ex)
    {
        Response.StatusCode = 400;
        await Response.WriteAsync($"Upload failed: {ex.Message}");
    }
}
```

### Advantages
- Resumable for network interruptions
- Supports pause/resume
- Better for large files (100MB+)
- Retry mechanism built-in

### Important Notes
- Only activates for files larger than `ChunkSize`
- Works exclusively with asynchronous uploads
- Requires proper backend chunking handler

---

## Pause & Resume

Pause and resume functionality is only available when chunk upload is enabled.

### Methods

```csharp
public async Task PauseAsync()    // Pause upload
public async Task ResumeAsync()   // Resume paused upload
```

### Example: Manual Pause/Resume Control

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader @ref="uploader" AutoUpload="true">
    <UploaderAsyncSettings 
        SaveUrl="api/upload/save" 
        RemoveUrl="api/upload/remove" 
        ChunkSize="500000">
    </UploaderAsyncSettings>
    <UploaderEvents 
        OnResume="@OnResumeHandler" 
        Paused="@PausedHandler">
    </UploaderEvents>
</SfUploader>

<div class="mt-3">
    <button class="btn btn-warning" @onclick="PauseUpload">Pause Upload</button>
    <button class="btn btn-success" @onclick="ResumeUpload">Resume Upload</button>
</div>

@if (!string.IsNullOrEmpty(statusMessage))
{
    <div class="alert alert-info mt-3">@statusMessage</div>
}

@code {
    SfUploader uploader;
    string statusMessage = "";

    private async Task PauseUpload()
    {
        await uploader.PauseAsync();
        statusMessage = "Upload paused";
    }

    private async Task ResumeUpload()
    {
        await uploader.ResumeAsync();
        statusMessage = "Upload resumed";
    }

    private void OnResumeHandler(PauseResumeEventArgs args)
    {
        statusMessage = $"Resumed: {args.File.Name}";
    }

    private void PausedHandler(PauseResumeEventArgs args)
    {
        statusMessage = $"Paused: {args.File.Name}";
    }
}
```

---

## MemoryStream Processing

Process uploaded files in memory without saving to disk. Useful for transformations or immediate processing.

### Example: Image to Base64 Conversion

```razor
@using Syncfusion.Blazor.Inputs
@using System.IO

<SfUploader AutoUpload="true">
    <UploaderEvents ValueChange="@OnValueChange"></UploaderEvents>
</SfUploader>

@if (!string.IsNullOrEmpty(base64Image))
{
    <h4>Image Preview</h4>
    <img src="@($"data:image/jpeg;base64,{base64Image}")" 
         style="max-width: 300px; height: auto;" />
}

@code {
    private string base64Image;

    private async Task OnValueChange(UploadChangeEventArgs args)
    {
        base64Image = string.Empty;

        foreach (var fileEntry in args.Files)
        {
            if (fileEntry.FileInfo.Type.StartsWith("image/", 
                StringComparison.OrdinalIgnoreCase))
            {
                using var memoryStream = new MemoryStream();
                
                await fileEntry.File.OpenReadStream(long.MaxValue)
                    .CopyToAsync(memoryStream);
                
                byte[] imageBytes = memoryStream.ToArray();
                base64Image = Convert.ToBase64String(imageBytes);
                Console.WriteLine($"Image converted to Base64");
            }
        }
    }
}
```

### Example: PDF Content Reading

```razor
@using Syncfusion.Blazor.Inputs
@using System.IO
@using System.Text

<SfUploader AutoUpload="true">
    <UploaderEvents ValueChange="@OnValueChange"></UploaderEvents>
</SfUploader>

@if (fileContent != null)
{
    <h4>File Content Preview</h4>
    <pre>@fileContent</pre>
}

@code {
    private string fileContent;

    private async Task OnValueChange(UploadChangeEventArgs args)
    {
        fileContent = null;

        foreach (var fileEntry in args.Files)
        {
            if (fileEntry.FileInfo.Type == "text/plain" ||
                fileEntry.FileInfo.Name.EndsWith(".txt"))
            {
                using var memoryStream = new MemoryStream();
                
                await fileEntry.File.OpenReadStream(long.MaxValue)
                    .CopyToAsync(memoryStream);
                
                fileContent = Encoding.UTF8.GetString(
                    memoryStream.ToArray()
                );
                Console.WriteLine("Text file loaded");
            }
        }
    }
}
```

### Memory Considerations
- Use for smaller files (< 50 MB)
- For large files, consider streaming or chunking
- Monitor server memory usage
- Consider implementing cleanup

---

## Batch Operations

Handle multiple file uploads efficiently:

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader @ref="uploader" 
            AllowMultiple="true"
            SequentialUpload="true"
            AutoUpload="false">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderEvents Success="@OnSuccess" OnFailure="@OnFailure"></UploaderEvents>
</SfUploader>

<div class="mt-3">
    <button class="btn btn-primary" @onclick="@(async () => await uploader.UploadAsync())">
        Upload All
    </button>
</div>

<div class="mt-3">
    <h5>Progress: @uploadedCount / @totalCount</h5>
    <div class="alert alert-info">
        @foreach (var log in uploadLogs)
        {
            <p>@log</p>
        }
    </div>
</div>

@code {
    SfUploader uploader;
    List<string> uploadLogs = new();
    int uploadedCount = 0;
    int totalCount = 0;

    private void OnSuccess(SuccessEventArgs args)
    {
        uploadedCount++;
        uploadLogs.Add($"✓ {args.File.Name}");
        StateHasChanged();
    }

    private void OnFailure(FailureEventArgs args)
    {
        uploadLogs.Add($"✗ {args.File.Name}: {args.StatusText}");
        StateHasChanged();
    }
}
```

---

## Async/Await Patterns

Proper async patterns for file operations:

### Pattern 1: File Processing

```razor
@using System.IO

private async Task OnFileSelected(SelectedEventArgs args)
{
    foreach (var file in args.FilesData)
    {
        await ProcessFileAsync(file);
    }
}

private async Task ProcessFileAsync(FileInfo file)
{
    await Task.Delay(100); // Simulate processing
    Console.WriteLine($"Processing: {file.Name}");
}
```

### Pattern 2: Error Handling

```razor
private async Task OnValueChange(UploadChangeEventArgs args)
{
    try
    {
        foreach (var fileEntry in args.Files)
        {
            await HandleFileAsync(fileEntry);
        }
    }
    catch (Exception ex)
    {
        Console.WriteLine($"Error: {ex.Message}");
    }
}

private async Task HandleFileAsync(UploadingFile fileEntry)
{
    using var stream = await fileEntry.File.OpenReadStream(long.MaxValue);
    // Process stream
}
```

---

## Concurrent vs Sequential

### Sequential Upload (One at a time)

```razor
<SfUploader AllowMultiple="true" SequentialUpload="true">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
</SfUploader>
```
- Lower server load
- Guaranteed order
- Slower for multiple files
- Better for limited resources

### Concurrent Upload (All at once)

```razor
<SfUploader AllowMultiple="true" SequentialUpload="false">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
</SfUploader>
```
- Higher server load
- Faster for users
- Not guaranteed order
- Use with powerful servers

---

## Error Recovery

Implement robust error handling:

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader @ref="uploader" AutoUpload="true">
    <UploaderAsyncSettings 
        SaveUrl="api/upload/save"
        ChunkSize="500000"
        RetryCount="5"
        RetryAfterDelay="2000">
    </UploaderAsyncSettings>
    <UploaderEvents 
        OnFailure="@OnFailure"
        OnResume="@OnResume">
    </UploaderEvents>
</SfUploader>

<div>@statusMessage</div>

@code {
    SfUploader uploader;
    string statusMessage = "";
    int retryAttempt = 0;

    private void OnFailure(FailureEventArgs args)
    {
        statusMessage = $"Upload failed: {args.File.Name}";
        retryAttempt++;
        
        if (retryAttempt < 3)
        {
            statusMessage += $" (Retry {retryAttempt}/3)";
        }
    }

    private void OnResume(PauseResumeEventArgs args)
    {
        statusMessage = $"Retrying: {args.File.Name}";
        retryAttempt = 0;
    }
}
```

---

## Performance Optimization

### Optimization Strategies

1. **Chunk Size Selection**
   - Larger chunks: Faster but more memory
   - Smaller chunks: Better resume but more requests
   - Optimal: 500KB - 5MB range

2. **Sequential vs Concurrent**
   - Sequential: Conserves server resources
   - Concurrent: Better user experience (when server can handle it)

3. **File Validation**
   - Early validation reduces wasted uploads
   - Pre-check size and type before upload

4. **Progress Tracking**
   - Show progress to improve UX
   - Update only when necessary

### Example: Optimized Configuration

```razor
<SfUploader AllowMultiple="true"
            AllowedExtensions=".jpg,.png,.pdf"
            MaxFileSize="104857600">
    <UploaderAsyncSettings 
        SaveUrl="api/upload/save"
        ChunkSize="1048576"
        RetryCount="3"
        RetryAfterDelay="1000">
    </UploaderAsyncSettings>
    <UploaderEvents FileSelected="@OnFileSelected"></UploaderEvents>
</SfUploader>

@code {
    private void OnFileSelected(SelectedEventArgs args)
    {
        // Early validation prevents unnecessary uploads
        foreach (var file in args.FilesData)
        {
            if (file.Size > 104857600)
            {
                args.Cancel = true;
            }
        }
    }
}
```

---

## Complete Advanced Example

```razor
@page "/advanced-upload"
@using Syncfusion.Blazor.Inputs

<div class="container">
    <h3>Advanced File Upload</h3>

    <SfUploader @ref="uploader"
                AllowMultiple="true"
                AutoUpload="false"
                AllowedExtensions=".jpg,.png,.pdf"
                MaxFileSize="104857600">
        <UploaderAsyncSettings 
            SaveUrl="api/upload/save"
            ChunkSize="1048576"
            RetryCount="5">
        </UploaderAsyncSettings>
        <UploaderEvents 
            FileSelected="@OnFileSelected"
            OnUploadStart="@OnUploadStart"
            Success="@OnSuccess"
            OnFailure="@OnFailure"
            Paused="@OnPaused">
        </UploaderEvents>
    </SfUploader>

    <div class="mt-3">
        <button class="btn btn-primary" @onclick="@(async () => await uploader.UploadAsync())">
            Upload
        </button>
        <button class="btn btn-warning" @onclick="@(async () => await uploader.PauseAsync())">
            Pause
        </button>
        <button class="btn btn-success" @onclick="@(async () => await uploader.ResumeAsync())">
            Resume
        </button>
    </div>

    <div class="mt-4">
        <h5>Statistics</h5>
        <p>Total: @totalFiles | Uploaded: @uploadedCount | Failed: @failedCount</p>
    </div>

    <div class="mt-3">
        <h5>Activity Log</h5>
        <pre>@string.Join("\n", activityLog.TakeLast(10))</pre>
    </div>
</div>

@code {
    SfUploader uploader;
    List<string> activityLog = new();
    int totalFiles = 0;
    int uploadedCount = 0;
    int failedCount = 0;

    private void OnFileSelected(SelectedEventArgs args)
    {
        totalFiles = args.FilesData.Count;
        LogActivity($"Selected {totalFiles} files");
    }

    private void OnUploadStart(UploadingEventArgs args)
    {
        LogActivity($"Uploading: {args.FileData.Name}");
    }

    private void OnSuccess(SuccessEventArgs args)
    {
        uploadedCount++;
        LogActivity($"✓ Success: {args.File.Name}");
        StateHasChanged();
    }

    private void OnFailure(FailureEventArgs args)
    {
        failedCount++;
        LogActivity($"✗ Failed: {args.File.Name}");
        StateHasChanged();
    }

    private void OnPaused(PauseResumeEventArgs args)
    {
        LogActivity($"Paused: {args.File.Name}");
    }

    private void LogActivity(string message)
    {
        activityLog.Add($"[{DateTime.Now:HH:mm:ss}] {message}");
    }
}
```
