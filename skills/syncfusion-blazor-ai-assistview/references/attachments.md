````markdown
# Attachments

## Table of Contents
- [Enabling Attachments](#enabling-attachments)
- [Attachment Settings Configuration](#attachment-settings-configuration)
- [File Type Restrictions](#file-type-restrictions)
- [File Size Limits](#file-size-limits)
- [Upload Configuration](#upload-configuration)
- [Attachment Events](#attachment-events)
- [Handling Attachment Clicks](#handling-attachment-clicks)
- [Custom Upload Logic](#custom-upload-logic)
- [Best Practices](#best-practices)

## Enabling Attachments

The AI AssistView component supports file attachments, allowing users to upload documents, images, and other files alongside their prompts. Enable attachments using the `AttachmentSettings` property:

```csharp
@using Syncfusion.Blazor.InteractiveChat

<div style="height: 400px; width: 650px;">
    <SfAIAssistView PromptRequested="@OnPromptRequested">
        <AssistViewAttachmentSettings 
            Enable="true"
            SaveUrl="api/upload"
            RemoveUrl="api/remove">
        </AssistViewAttachmentSettings>
    </SfAIAssistView>
</div>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        // Access attachments
        var attachments = args.Attachments;
        
        await Task.Delay(1000);
        args.Response = $"<div>Received {attachments?.Count ?? 0} attachment(s)</div>";
    }
}
```

## Attachment Settings Configuration

Configure attachment behavior with `AssistViewAttachmentSettings`:

```csharp
<SfAIAssistView PromptRequested="@OnPromptRequested">
    <AssistViewAttachmentSettings 
        Enable="true"
        AllowedFileTypes=".jpg,.png,.pdf,.docx"
        MaxFileSize="5000000"
        MaximumCount="5"
        SaveUrl="api/attachments/upload"
        RemoveUrl="api/attachments/remove">
    </AssistViewAttachmentSettings>
</SfAIAssistView>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Processing your request with attachments...</div>";
    }
}
```

### AttachmentSettings Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Enable` | `bool` | `false` | Enables or disables attachment feature |
| `AllowedFileTypes` | `string` | `""` | Comma-separated file extensions (e.g., ".jpg,.png,.pdf") |
| `MaxFileSize` | `double` | `2000000` | Maximum file size in bytes (default: 2MB) |
| `MaximumCount` | `int` | `10` | Maximum number of files per message |
| `SaveUrl` | `string` | `""` | Server endpoint for file uploads |
| `RemoveUrl` | `string` | `""` | Server endpoint for file deletions |

## File Type Restrictions

Restrict uploadable file types using the `AllowedFileTypes` property:

```csharp
<!-- Images only -->
<AssistViewAttachmentSettings 
    Enable="true"
    AllowedFileTypes=".jpg,.jpeg,.png,.gif,.webp"
    SaveUrl="api/upload">
</AssistViewAttachmentSettings>

<!-- Documents only -->
<AssistViewAttachmentSettings 
    Enable="true"
    AllowedFileTypes=".pdf,.docx,.xlsx,.txt"
    SaveUrl="api/upload">
</AssistViewAttachmentSettings>

<!-- Mixed types -->
<AssistViewAttachmentSettings 
    Enable="true"
    AllowedFileTypes=".jpg,.png,.pdf,.docx,.txt,.zip"
    SaveUrl="api/upload">
</AssistViewAttachmentSettings>
```

## File Size Limits

Control maximum file size with `MaxFileSize` (in bytes):

```csharp
<!-- 1 MB limit -->
<AssistViewAttachmentSettings 
    Enable="true"
    MaxFileSize="1000000"
    SaveUrl="api/upload">
</AssistViewAttachmentSettings>

<!-- 5 MB limit -->
<AssistViewAttachmentSettings 
    Enable="true"
    MaxFileSize="5000000"
    SaveUrl="api/upload">
</AssistViewAttachmentSettings>

<!-- 10 MB limit -->
<AssistViewAttachmentSettings 
    Enable="true"
    MaxFileSize="10000000"
    SaveUrl="api/upload">
</AssistViewAttachmentSettings>
```

**Size Calculation:**
- 1 MB = 1,000,000 bytes
- 5 MB = 5,000,000 bytes
- 10 MB = 10,000,000 bytes

## Upload Configuration

### Server-Side Upload Endpoint

Create an API controller to handle file uploads:

```csharp
[ApiController]
[Route("api/attachments")]
public class AttachmentsController : ControllerBase
{
    private readonly IWebHostEnvironment _environment;

    public AttachmentsController(IWebHostEnvironment environment)
    {
        _environment = environment;
    }

    [HttpPost("upload")]
    public async Task<IActionResult> Upload(IFormFile file)
    {
        if (file == null || file.Length == 0)
            return BadRequest("No file uploaded");

        // Validate file size (5MB limit)
        if (file.Length > 5000000)
            return BadRequest("File size exceeds limit");

        // Validate file type
        var allowedExtensions = new[] { ".jpg", ".png", ".pdf", ".docx" };
        var extension = Path.GetExtension(file.FileName).ToLowerInvariant();
        
        if (!allowedExtensions.Contains(extension))
            return BadRequest("File type not allowed");

        // Save file
        var uploadsFolder = Path.Combine(_environment.WebRootPath, "uploads");
        Directory.CreateDirectory(uploadsFolder);
        
        var fileName = $"{Guid.NewGuid()}{extension}";
        var filePath = Path.Combine(uploadsFolder, fileName);

        using (var stream = new FileStream(filePath, FileMode.Create))
        {
            await file.CopyToAsync(stream);
        }

        return Ok(new { fileName, filePath });
    }

    [HttpPost("remove")]
    public IActionResult Remove([FromBody] string fileName)
    {
        if (string.IsNullOrEmpty(fileName))
            return BadRequest("No file name provided");

        var filePath = Path.Combine(_environment.WebRootPath, "uploads", fileName);
        
        if (System.IO.File.Exists(filePath))
        {
            System.IO.File.Delete(filePath);
            return Ok();
        }

        return NotFound("File not found");
    }
}
```

### Client-Side Configuration

```csharp
<SfAIAssistView PromptRequested="@OnPromptRequested">
    <AssistViewAttachmentSettings 
        Enable="true"
        SaveUrl="/api/attachments/upload"
        RemoveUrl="/api/attachments/remove"
        AllowedFileTypes=".jpg,.png,.pdf,.docx"
        MaxFileSize="5000000"
        MaximumCount="3">
    </AssistViewAttachmentSettings>
</SfAIAssistView>
```

## Attachment Events

Handle attachment-related events to customize behavior:

```csharp
@using Syncfusion.Blazor.InteractiveChat
@using Syncfusion.Blazor.Inputs

<SfAIAssistView 
    PromptRequested="@OnPromptRequested"
    OnAttachmentUploadReady="@OnAttachmentUploadReady"
    AttachmentUploadChange="@OnAttachmentUploadChange"
    AttachmentUploadSuccess="@OnAttachmentUploadSuccess"
    AttachmentUploadFailed="@OnAttachmentUploadFailed"
    AttachmentRemoved="@OnAttachmentRemoved"
    AttachmentClick="@OnAttachmentClick">
    <AssistViewAttachmentSettings 
        Enable="true"
        SaveUrl="api/attachments/upload"
        RemoveUrl="api/attachments/remove">
    </AssistViewAttachmentSettings>
</SfAIAssistView>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Response with attachments processed</div>";
    }

    private void OnAttachmentUploadReady(AttachmentUploadReadyEventArgs args)
    {
        // Called before upload begins
        Console.WriteLine($"Preparing to upload {args.FilesData.Length} file(s)");
        
        // Add custom form data
        args.CustomFormData = new Dictionary<string, object>
        {
            { "userId", "user123" },
            { "category", "documents" }
        };
        
        // Cancel upload if needed
        // args.Cancel = true;
    }

    private void OnAttachmentUploadChange(UploadChangeEventArgs args)
    {
        // Called when files are selected/changed
        Console.WriteLine($"Files changed: {args.Files.Count}");
    }

    private void OnAttachmentUploadSuccess(SuccessEventArgs args)
    {
        // Called on successful upload
        Console.WriteLine($"Upload successful: {args.File.Name}");
    }

    private void OnAttachmentUploadFailed(FailureEventArgs args)
    {
        // Called on upload failure
        Console.WriteLine($"Upload failed: {args.File.Name} - {args.Response.StatusText}");
    }

    private void OnAttachmentRemoved(RemovingEventArgs args)
    {
        // Called when attachment is removed
        Console.WriteLine($"Attachment removed: {args.FilesData[0].Name}");
    }

    private void OnAttachmentClick(AttachmentClickEventArgs args)
    {
        // Called when attachment is clicked
        Console.WriteLine($"Attachment clicked: {args.SelectedFile.Name}");
    }
}
```

## Handling Attachment Clicks

Implement custom behavior when users click on attachments:

```csharp
@using Syncfusion.Blazor.InteractiveChat
@inject IJSRuntime JS

<SfAIAssistView 
    PromptRequested="@OnPromptRequested"
    AttachmentClick="@OnAttachmentClick">
    <AssistViewAttachmentSettings 
        Enable="true"
        SaveUrl="api/attachments/upload">
    </AssistViewAttachmentSettings>
</SfAIAssistView>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Processing your request...</div>";
    }

    private async Task OnAttachmentClick(AttachmentClickEventArgs args)
    {
        var file = args.SelectedFile;
        
        // Show file details
        Console.WriteLine($"File: {file.Name}");
        Console.WriteLine($"Size: {file.Size} bytes");
        Console.WriteLine($"Type: {file.Type}");
        
        // Option 1: Open file in new window
        await JS.InvokeVoidAsync("window.open", $"/uploads/{file.Name}", "_blank");
        
        // Option 2: Download file
        // await JS.InvokeVoidAsync("downloadFile", file.Name);
        
        // Option 3: Show preview modal
        // await ShowFilePreview(file);
    }
}
```

## Custom Upload Logic

Implement custom upload logic using the `OnAttachmentUploadReady` event:

```csharp
@using Syncfusion.Blazor.InteractiveChat
@inject HttpClient Http

<SfAIAssistView 
    PromptRequested="@OnPromptRequested"
    OnAttachmentUploadReady="@OnAttachmentUploadReady">
    <AssistViewAttachmentSettings 
        Enable="true"
        SaveUrl="api/attachments/upload">
    </AssistViewAttachmentSettings>
</SfAIAssistView>

@code {
    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "<div>Response content</div>";
    }

    private void OnAttachmentUploadReady(AttachmentUploadReadyEventArgs args)
    {
        // Add authentication token
        args.CurrentRequest = new Dictionary<string, object>
        {
            { "Authorization", "Bearer YOUR_TOKEN_HERE" }
        };

        // Add custom metadata
        args.CustomFormData = new Dictionary<string, object>
        {
            { "userId", GetCurrentUserId() },
            { "timestamp", DateTime.UtcNow.ToString("o") },
            { "sessionId", Guid.NewGuid().ToString() }
        };

        // Validate file before upload
        foreach (var file in args.FilesData)
        {
            if (file.Size > 5000000)
            {
                args.Cancel = true;
                Console.WriteLine($"File {file.Name} exceeds size limit");
                return;
            }

            // Check file content (example)
            if (IsVirusDetected(file))
            {
                args.Cancel = true;
                Console.WriteLine($"File {file.Name} failed security check");
                return;
            }
        }
    }

    private string GetCurrentUserId()
    {
        // Return current user ID
        return "user123";
    }

    private bool IsVirusDetected(FileInfo file)
    {
        // Implement virus scanning logic
        return false;
    }
}
```

## Best Practices

### 1. File Validation

Always validate files on both client and server:

```csharp
private void OnAttachmentUploadReady(AttachmentUploadReadyEventArgs args)
{
    var allowedExtensions = new[] { ".jpg", ".png", ".pdf", ".docx" };
    
    foreach (var file in args.FilesData)
    {
        var extension = Path.GetExtension(file.Name).ToLowerInvariant();
        
        if (!allowedExtensions.Contains(extension))
        {
            args.Cancel = true;
            ShowError($"File type {extension} not allowed");
            return;
        }
        
        if (file.Size > 5000000)
        {
            args.Cancel = true;
            ShowError($"File {file.Name} exceeds 5MB limit");
            return;
        }
    }
}
```

### 2. Security Considerations

- **Validate file types** on server, not just client
- **Scan for malware** before saving files
- **Store files outside web root** when possible
- **Use unique filenames** to prevent overwrites
- **Implement rate limiting** to prevent abuse

### 3. Error Handling

```csharp
private void OnAttachmentUploadFailed(FailureEventArgs args)
{
    var errorMessage = args.Response.StatusText;
    var fileName = args.File.Name;
    
    // Log error
    Logger.LogError($"Upload failed for {fileName}: {errorMessage}");
    
    // Show user-friendly message
    if (args.Response.StatusCode == 413)
    {
        ShowError("File is too large. Maximum size is 5MB.");
    }
    else if (args.Response.StatusCode == 415)
    {
        ShowError("File type not supported.");
    }
    else
    {
        ShowError("Upload failed. Please try again.");
    }
}
```

### 4. Progress Indication

Provide feedback during upload:

```csharp
private string uploadStatus = "";

private void OnAttachmentUploadReady(AttachmentUploadReadyEventArgs args)
{
    uploadStatus = $"Uploading {args.FilesData.Length} file(s)...";
    StateHasChanged();
}

private void OnAttachmentUploadSuccess(SuccessEventArgs args)
{
    uploadStatus = $"Successfully uploaded {args.File.Name}";
    StateHasChanged();
}
```

### 5. Cleanup

Remove temporary files when no longer needed:

```csharp
private void OnAttachmentRemoved(RemovingEventArgs args)
{
    // Call server to delete file
    foreach (var file in args.FilesData)
    {
        _ = DeleteFileFromServer(file.Name);
    }
}

private async Task DeleteFileFromServer(string fileName)
{
    try
    {
        await Http.DeleteAsync($"api/attachments/{fileName}");
    }
    catch (Exception ex)
    {
        Logger.LogError($"Failed to delete {fileName}: {ex.Message}");
    }
}
```

### 6. Accessibility

Ensure attachment functionality is accessible:

```csharp
<AssistViewAttachmentSettings 
    Enable="true"
    SaveUrl="api/upload"
    AllowedFileTypes=".jpg,.png,.pdf"
    MaxFileSize="5000000">
</AssistViewAttachmentSettings>

<!-- Add descriptive text for screen readers -->
<div class="sr-only">
    Supported file types: JPG, PNG, PDF. Maximum file size: 5MB.
</div>
```

## Complete Example

```csharp
@page "/ai-assistant-with-attachments"
@using Syncfusion.Blazor.InteractiveChat
@using Syncfusion.Blazor.Inputs

<div style="height: 500px; width: 700px;">
    <SfAIAssistView 
        PromptRequested="@OnPromptRequested"
        OnAttachmentUploadReady="@OnAttachmentUploadReady"
        AttachmentUploadSuccess="@OnAttachmentUploadSuccess"
        AttachmentUploadFailed="@OnAttachmentUploadFailed"
        AttachmentClick="@OnAttachmentClick">
        <AssistViewAttachmentSettings 
            Enable="true"
            AllowedFileTypes=".jpg,.png,.pdf,.docx"
            MaxFileSize="5000000"
            MaximumCount="3"
            SaveUrl="/api/attachments/upload"
            RemoveUrl="/api/attachments/remove">
        </AssistViewAttachmentSettings>
    </SfAIAssistView>
</div>

@if (!string.IsNullOrEmpty(statusMessage))
{
    <div class="alert alert-info mt-2">@statusMessage</div>
}

@code {
    private string statusMessage = "";

    private async Task OnPromptRequested(AssistViewPromptRequestedEventArgs args)
    {
        var attachmentCount = args.Attachments?.Count ?? 0;
        
        await Task.Delay(1000);
        
        if (attachmentCount > 0)
        {
            args.Response = $"<div><strong>Processing your request with {attachmentCount} attachment(s)...</strong></div>";
        }
        else
        {
            args.Response = "<div>Response to your prompt</div>";
        }
    }

    private void OnAttachmentUploadReady(AttachmentUploadReadyEventArgs args)
    {
        statusMessage = $"Uploading {args.FilesData.Length} file(s)...";
        
        // Add custom headers or metadata
        args.CustomFormData = new Dictionary<string, object>
        {
            { "userId", "user123" },
            { "timestamp", DateTime.UtcNow }
        };
    }

    private void OnAttachmentUploadSuccess(SuccessEventArgs args)
    {
        statusMessage = $"Successfully uploaded: {args.File.Name}";
    }

    private void OnAttachmentUploadFailed(FailureEventArgs args)
    {
        statusMessage = $"Upload failed: {args.File.Name} - {args.Response.StatusText}";
    }

    private void OnAttachmentClick(AttachmentClickEventArgs args)
    {
        statusMessage = $"Clicked attachment: {args.SelectedFile.Name}";
    }
}
```

````
