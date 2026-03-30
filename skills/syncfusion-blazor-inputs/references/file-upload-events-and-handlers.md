# Events & Event Handlers

## Table of Contents
- [ValueChange Event](#valuechange)
- [FileSelected Event](#fileselected)
- [Created Event](#created)
- [OnFileListRender Event](#onfilelistrender)
- [OnUploadStart Event](#onuploadstart)
- [Success Event](#success)
- [OnFailure Event](#onfailure)
- [OnRemove Event](#onremove)
- [Event Handler Patterns](#patterns)
- [Complete Event Example](#complete)

---

## ValueChange Event

Fires when files are selected or removed from the uploader. Crucial for direct file processing in Blazor Server.

### ⚠️ Important Constraint
**Do NOT use `ValueChange` event with `UploaderAsyncSettings`.** These are mutually exclusive:
- **ValueChange**: For direct file access and processing (Blazor Server only)
- **UploaderAsyncSettings**: For server-side upload endpoints (use `Success`/`OnFailure` events instead)

### Use Cases
- Access file details and content directly
- Preview files before upload
- Save files directly to directory (Blazor Server)
- Process files in memory (MemoryStream)
- Convert files to Base64 or other formats

### Example: Save Files to Server

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
            foreach (var fileEntry in args.Files)
            {
                var uploadsFolder = Path.Combine(
                    Directory.GetCurrentDirectory(), 
                    "wwwroot", 
                    "uploads"
                );
                
                if (!Directory.Exists(uploadsFolder))
                    Directory.CreateDirectory(uploadsFolder);

                var filePath = Path.Combine(uploadsFolder, fileEntry.FileInfo.Name);

                await using (var fileStream = new FileStream(
                    filePath, 
                    FileMode.Create, 
                    FileAccess.Write))
                {
                    await fileEntry.File.OpenReadStream(long.MaxValue)
                        .CopyToAsync(fileStream);
                }
                
                Console.WriteLine($"File '{fileEntry.FileInfo.Name}' saved successfully");
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error saving file: {ex.Message}");
        }
    }
}
```

### Example: Convert Image to Base64

```razor
@using Syncfusion.Blazor.Inputs
@using System.IO

<SfUploader AutoUpload="true">
    <UploaderEvents ValueChange="@OnValueChange"></UploaderEvents>
</SfUploader>

@if (!string.IsNullOrEmpty(base64Image))
{
    <h4>Uploaded Image Preview</h4>
    <img src="@($"data:image/png;base64,{base64Image}")" style="max-width: 300px;" />
}

@code {
    private string base64Image;

    private async Task OnValueChange(UploadChangeEventArgs args)
    {
        base64Image = string.Empty;

        foreach (var fileEntry in args.Files)
        {
            if (fileEntry.FileInfo.Type.StartsWith("image/", StringComparison.OrdinalIgnoreCase))
            {
                using var memoryStream = new MemoryStream();
                await fileEntry.File.OpenReadStream(long.MaxValue)
                    .CopyToAsync(memoryStream);
                
                byte[] imageBytes = memoryStream.ToArray();
                base64Image = Convert.ToBase64String(imageBytes);
                Console.WriteLine($"Image loaded and converted to Base64");
            }
        }
    }
}
```

---

## FileSelected Event

Fires when files are chosen from file explorer **before** `ValueChange`. Provides opportunity for pre-validation and preventing invalid selections.

### Use Cases
- Prevent files larger than size limit
- Reject invalid file types
- Limit file count
- Show custom validation messages

### Example: File Size Validation

```razor
@using Syncfusion.Blazor.Inputs
@using System.Linq

<SfUploader>
    <UploaderEvents FileSelected="@OnFileSelected"></UploaderEvents>
</SfUploader>

<p>@validationMessage</p>

@code {
    private string validationMessage = "";
    private readonly long MaxFileSize = 1024 * 1024; // 1 MB

    private void OnFileSelected(SelectedEventArgs args)
    {
        validationMessage = "";
        
        foreach (var file in args.FilesData)
        {
            if (file.Size > MaxFileSize)
            {
                validationMessage += $"Error: '{file.Name}' exceeds 1 MB limit. ";
                args.Cancel = true; // Prevent file from being added
            }
        }
        
        if (!string.IsNullOrEmpty(validationMessage))
        {
            Console.WriteLine(validationMessage);
        }
    }
}
```

### Example: File Type Restriction

```razor
<SfUploader AllowedExtensions=".jpg,.png,.pdf">
    <UploaderEvents FileSelected="@OnFileSelected"></UploaderEvents>
</SfUploader>

@code {
    private void OnFileSelected(SelectedEventArgs args)
    {
        var allowedTypes = new[] { "jpg", "png", "pdf" };
        
        foreach (var file in args.FilesData)
        {
            var ext = Path.GetExtension(file.Name).ToLower().TrimStart('.');
            if (!allowedTypes.Contains(ext))
            {
                args.Cancel = true;
                Console.WriteLine($"File type '{ext}' not allowed");
            }
        }
    }
}
```

---

## Created Event

Fires after File Upload component is rendered and initialized. Good for setup, DOM manipulation, or JavaScript interop.

### Example: Initialize Component

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader>
    <UploaderEvents Created="@OnUploaderCreated"></UploaderEvents>
</SfUploader>

<p>@statusMessage</p>

@code {
    private string statusMessage = "Uploader not yet created.";

    private void OnUploaderCreated()
    {
        statusMessage = "Syncfusion File Uploader initialized successfully!";
        Console.WriteLine(statusMessage);
        // Can perform additional setup here
    }
}
```

---

## OnFileListRender Event

Allows customization of individual file list items before rendering. Useful for displaying additional information or custom actions.

### Example: Custom File Display

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader @ref="fileobj" AutoUpload="false">
    <UploaderEvents OnFileListRender="@OnFileListRenderHandler"></UploaderEvents>
    <UploaderAsyncSettings 
        SaveUrl="api/upload/save" 
        RemoveUrl="api/upload/remove">
    </UploaderAsyncSettings>
</SfUploader>

@code {
    SfUploader fileobj;
    
    private void OnFileListRenderHandler(FileListRenderingEventArgs args)
    {
        // Can modify file list display properties here
        Console.WriteLine($"File: {args.FileInfo.Name}, Size: {args.FileInfo.Size}");
    }
}
```

---

## OnUploadStart Event

Fires when upload starts for each file. Useful for logging, adding custom headers, or modifying request data.

### Example: Add Custom Headers

```razor
<SfUploader AutoUpload="true">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderEvents OnUploadStart="@OnUploadStart"></UploaderEvents>
</SfUploader>

@code {
    private void OnUploadStart(UploadingEventArgs args)
    {
        Console.WriteLine($"Upload starting for: {args.FileData.Name}");
        // Add custom headers or modify request
    }
}
```

---

## Success Event

Fires when file upload completes successfully. Use for user feedback or next steps.

### Example: Success Feedback

```razor
<SfUploader AutoUpload="true">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderEvents Success="@OnUploadSuccess"></UploaderEvents>
</SfUploader>

@if (!string.IsNullOrEmpty(successMessage))
{
    <div class="alert alert-success">@successMessage</div>
}

@code {
    private string successMessage = "";

    private void OnUploadSuccess(SuccessEventArgs args)
    {
        successMessage = $"✓ {args.File.Name} uploaded successfully ({args.File.Size} bytes)";
        Console.WriteLine(successMessage);
    }
}
```

---

## OnFailure Event

Fires when upload fails. Essential for error handling and user notification.

### Example: Error Handling

```razor
<SfUploader AutoUpload="true">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderEvents OnFailure="@OnUploadFailure"></UploaderEvents>
</SfUploader>

@if (!string.IsNullOrEmpty(errorMessage))
{
    <div class="alert alert-danger">@errorMessage</div>
}

@code {
    private string errorMessage = "";

    private void OnUploadFailure(FailureEventArgs args)
    {
        errorMessage = $"✗ {args.File.Name} failed: {args.StatusText}";
        Console.WriteLine(errorMessage);
    }
}
```

---

## OnRemove Event

Fires when user removes a file from upload list. Can log removal or update UI.

### Example: Track Removals

```razor
<SfUploader AutoUpload="false">
    <UploaderAsyncSettings 
        SaveUrl="api/upload/save"
        RemoveUrl="api/upload/remove">
    </UploaderAsyncSettings>
    <UploaderEvents OnRemove="@OnFileRemove"></UploaderEvents>
</SfUploader>

<p>@removalMessage</p>

@code {
    private string removalMessage = "";

    private void OnFileRemove(RemovingEventArgs args)
    {
        if (args.FilesData.Count > 0)
        {
            removalMessage = $"File removed: {args.FilesData[0].Name}";
            Console.WriteLine(removalMessage);
        }
    }
}
```

---

## BeforeUpload Event

Fires before upload starts. Allows cancellation or modification of upload request.

### Example: Cancel Upload Based on Condition

```razor
<SfUploader AutoUpload="true">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderEvents BeforeUpload="@OnBeforeUpload"></UploaderEvents>
</SfUploader>

@code {
    private void OnBeforeUpload(BeforeUploadEventArgs args)
    {
        // Cancel upload if specific condition is met
        if (args.FileData.Size > 10485760) // 10 MB
        {
            args.Cancel = true;
            Console.WriteLine($"Upload cancelled for: {args.FileData.Name}");
        }
    }
}
```

---

## BeforeRemove Event

Fires before file removal. Allows cancellation of remove operation.

### Example: Confirm Before Removal

```razor
<SfUploader AutoUpload="false">
    <UploaderAsyncSettings SaveUrl="api/upload/save" RemoveUrl="api/upload/remove"></UploaderAsyncSettings>
    <UploaderEvents BeforeRemove="@OnBeforeRemove"></UploaderEvents>
</SfUploader>

@code {
    private void OnBeforeRemove(BeforeRemoveEventArgs args)
    {
        // Optionally cancel removal
        Console.WriteLine($"About to remove: {args.FilesData[0].Name}");
    }
}
```

---

## OnChunkSuccess Event

Fires when a chunk upload completes successfully. Useful for tracking chunked upload progress.

### Example: Track Chunk Progress

```razor
<SfUploader AutoUpload="true">
    <UploaderAsyncSettings SaveUrl="api/upload/save" ChunkSize="1048576"></UploaderAsyncSettings>
    <UploaderEvents OnChunkSuccess="@OnChunkSuccess"></UploaderEvents>
</SfUploader>

@code {
    private void OnChunkSuccess(SuccessEventArgs args)
    {
        Console.WriteLine($"Chunk {args.ChunkIndex + 1} of {args.TotalChunk} uploaded");
    }
}
```

---

## OnChunkFailure Event

Fires when a chunk upload fails. Enables retry logic or error handling for specific chunks.

### Example: Handle Chunk Failure

```razor
<SfUploader AutoUpload="true">
    <UploaderAsyncSettings SaveUrl="api/upload/save" ChunkSize="1048576"></UploaderAsyncSettings>
    <UploaderEvents OnChunkFailure="@OnChunkFailure"></UploaderEvents>
</SfUploader>

@code {
    private void OnChunkFailure(FailureEventArgs args)
    {
        Console.WriteLine($"Chunk {args.ChunkIndex} failed: {args.StatusText}");
    }
}
```

---

## Progressing Event

Fires during upload to report progress. Useful for custom progress indicators.

### Example: Custom Progress Display

```razor
<SfUploader AutoUpload="true">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderEvents Progressing="@OnProgress"></UploaderEvents>
</SfUploader>

<div>Progress: @progressPercent%</div>

@code {
    private int progressPercent = 0;

    private void OnProgress(ProgressEventArgs args)
    {
        progressPercent = (int)args.Percent;
        StateHasChanged();
    }
}
```

---

## OnCancel Event

Fires when user cancels an ongoing upload.

### Example: Handle Upload Cancellation

```razor
<SfUploader AutoUpload="false" @ref="uploader">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderEvents OnCancel="@OnCancel"></UploaderEvents>
</SfUploader>

<button @onclick="CancelUpload">Cancel Upload</button>

@code {
    SfUploader uploader;

    private async Task CancelUpload()
    {
        await uploader.CancelAsync();
    }

    private void OnCancel(CancelEventArgs args)
    {
        Console.WriteLine($"Upload cancelled");
    }
}
```

---

## OnClear Event

Fires when clear button is clicked to remove all files.

### Example: Handle Clear Action

```razor
<SfUploader AutoUpload="false" @ref="uploader">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderEvents OnClear="@OnClear"></UploaderEvents>
</SfUploader>

@code {
    private void OnClear(ClearingEventArgs args)
    {
        Console.WriteLine("All files cleared");
    }
}
```

---

## Event Handler Patterns

### Pattern 1: Multiple Event Handling with Async Upload

```razor
<SfUploader AutoUpload="false" @ref="uploader">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderEvents 
        FileSelected="@OnFileSelected"
        OnUploadStart="@OnUploadStart"
        Success="@OnSuccess"
        OnFailure="@OnFailure">
    </UploaderEvents>
</SfUploader>

@code {
    private void OnFileSelected(SelectedEventArgs args) 
        => Console.WriteLine($"Files selected: {args.FilesData.Count}");

    private void OnUploadStart(UploadingEventArgs args) 
        => Console.WriteLine($"Uploading: {args.FileData.Name}");

    private void OnSuccess(SuccessEventArgs args) 
        => Console.WriteLine($"Success: {args.File.Name}");

    private void OnFailure(FailureEventArgs args) 
        => Console.WriteLine($"Failed: {args.File.Name} - {args.StatusText}");
}
```

### Pattern 1b: Direct File Access (Blazor Server Only)

```razor
<SfUploader AutoUpload="true" @ref="uploader">
    <UploaderEvents 
        FileSelected="@OnFileSelected"
        ValueChange="@OnValueChange">
    </UploaderEvents>
</SfUploader>

@code {
    private void OnFileSelected(SelectedEventArgs args) 
        => Console.WriteLine($"Files selected: {args.FilesData.Count}");

    private async Task OnValueChange(UploadChangeEventArgs args)
    {
        // Direct file processing without server endpoint
        foreach (var file in args.Files)
        {
            Console.WriteLine($"Processing: {file.FileInfo.Name}");
            // Access file.File.OpenReadStream() for content
        }
    }
}
```

### Pattern 2: State Management

```razor
<SfUploader AutoUpload="false" @ref="uploader">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderEvents Success="@OnSuccess" OnFailure="@OnFailure"></UploaderEvents>
</SfUploader>

<div>
    <p>Uploaded: @uploadedCount</p>
    <p>Failed: @failedCount</p>
</div>

@code {
    SfUploader uploader;
    int uploadedCount = 0;
    int failedCount = 0;

    private void OnSuccess(SuccessEventArgs args)
    {
        uploadedCount++;
        StateHasChanged();
    }

    private void OnFailure(FailureEventArgs args)
    {
        failedCount++;
        StateHasChanged();
    }
}
```

---

## Complete Example: Async Upload with Events

This example shows the correct way to use events with `UploaderAsyncSettings` (without ValueChange).

```razor
@page "/upload-events"
@using Syncfusion.Blazor.Inputs

<div class="container mt-4">
    <h3>File Upload with Async Events</h3>

    <SfUploader @ref="uploader" 
                AutoUpload="false"
                AllowMultiple="true"
                MaxFileSize="5242880">
        <UploaderAsyncSettings 
            SaveUrl="api/upload/save"
            RemoveUrl="api/upload/remove">
        </UploaderAsyncSettings>
        <UploaderEvents 
            FileSelected="@OnFileSelected"
            Created="@OnCreated"
            OnUploadStart="@OnUploadStart"
            Success="@OnSuccess"
            OnFailure="@OnFailure"
            OnRemove="@OnRemove">
        </UploaderEvents>
    </SfUploader>

    <div class="mt-3">
        <button class="btn btn-primary" @onclick="@(async () => await uploader.UploadAsync())">
            Upload
        </button>
        <button class="btn btn-secondary" @onclick="@(async () => await uploader.ClearAsync())">
            Clear
        </button>
    </div>

    <div class="mt-4">
        <h5>Event Log</h5>
        <div class="alert alert-info">
            @foreach (var log in eventLogs)
            {
                <p>@log</p>
            }
        </div>
    </div>

    <div class="mt-3">
        <h5>Statistics</h5>
        <p>Selected: @selectedCount | Uploaded: @uploadedCount | Failed: @failedCount</p>
    </div>
</div>

@code {
    SfUploader uploader;
    List<string> eventLogs = new();
    int selectedCount = 0;
    int uploadedCount = 0;
    int failedCount = 0;

    private void OnFileSelected(SelectedEventArgs args)
    {
        selectedCount = args.FilesData.Count;
        LogEvent($"FileSelected: {selectedCount} files");
    }

    private void OnCreated()
    {
        LogEvent("Component Created: Ready for upload");
    }

    private void OnUploadStart(UploadingEventArgs args)
    {
        LogEvent($"Upload Starting: {args.FileData.Name}");
    }

    private void OnSuccess(SuccessEventArgs args)
    {
        uploadedCount++;
        LogEvent($"✓ Success: {args.File.Name} ({args.File.Size} bytes)");
    }

    private void OnFailure(FailureEventArgs args)
    {
        failedCount++;
        LogEvent($"✗ Failed: {args.File.Name} - {args.StatusText}");
    }

    private void OnRemove(RemovingEventArgs args)
    {
        if (args.FilesData.Count > 0)
        {
            LogEvent($"Removed: {args.FilesData[0].Name}");
        }
    }

    private void LogEvent(string message)
    {
        eventLogs.Insert(0, $"[{DateTime.Now:HH:mm:ss}] {message}");
        if (eventLogs.Count > 10) eventLogs.RemoveAt(eventLogs.Count - 1);
        StateHasChanged();
    }
}
```

---

## Complete Example: Direct File Access (Blazor Server)

This example shows using `ValueChange` event for direct file processing (without UploaderAsyncSettings).

```razor
@page "/upload-direct"
@using Syncfusion.Blazor.Inputs
@using System.IO

<div class="container mt-4">
    <h3>Direct File Processing</h3>

    <SfUploader @ref="uploader" 
                AutoUpload="true"
                AllowMultiple="true"
                AllowedExtensions=".jpg,.png,.pdf"
                MaxFileSize="5242880">
        <UploaderEvents 
            FileSelected="@OnFileSelected"
            ValueChange="@OnValueChange">
        </UploaderEvents>
    </SfUploader>

    <div class="mt-4">
        <h5>Processing Log</h5>
        <div class="alert alert-info">
            @foreach (var log in processLogs)
            {
                <p>@log</p>
            }
        </div>
    </div>

    <div class="mt-3">
        <h5>Statistics</h5>
        <p>Files Processed: @processedCount</p>
    </div>
</div>

@code {
    SfUploader uploader;
    List<string> processLogs = new();
    int processedCount = 0;

    private void OnFileSelected(SelectedEventArgs args)
    {
        LogProcess($"Selected {args.FilesData.Count} files");
    }

    private async Task OnValueChange(UploadChangeEventArgs args)
    {
        try
        {
            foreach (var fileEntry in args.Files)
            {
                // Direct file processing
                var uploadsFolder = Path.Combine(
                    Directory.GetCurrentDirectory(), 
                    "wwwroot", 
                    "uploads"
                );
                
                if (!Directory.Exists(uploadsFolder))
                    Directory.CreateDirectory(uploadsFolder);

                var filePath = Path.Combine(uploadsFolder, fileEntry.FileInfo.Name);

                await using (var fileStream = new FileStream(
                    filePath, 
                    FileMode.Create, 
                    FileAccess.Write))
                {
                    await fileEntry.File.OpenReadStream(long.MaxValue)
                        .CopyToAsync(fileStream);
                }
                
                processedCount++;
                LogProcess($"✓ Saved: {fileEntry.FileInfo.Name} ({fileEntry.FileInfo.Size} bytes)");
            }
        }
        catch (Exception ex)
        {
            LogProcess($"✗ Error: {ex.Message}");
        }
    }

    private void LogProcess(string message)
    {
        processLogs.Insert(0, $"[{DateTime.Now:HH:mm:ss}] {message}");
        if (processLogs.Count > 10) processLogs.RemoveAt(processLogs.Count - 1);
        StateHasChanged();
    }
}
```
