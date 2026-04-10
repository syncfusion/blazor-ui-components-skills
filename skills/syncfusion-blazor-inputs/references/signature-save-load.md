# Signature - Save and Load

Comprehensive guide to saving signatures in multiple formats, loading existing signatures, and integrating with backend services for the Syncfusion Blazor Signature component.

## Save Methods

### SaveAsync() Method

Save the signature to a file with specified format:

```razor
<SfSignature @ref="signatureRef"></SfSignature>

<button @onclick="SaveSignature">Save as PNG</button>

@code {
    private SfSignature signatureRef;
    
    private async Task SaveSignature()
    {
        await signatureRef.SaveAsync(SignatureFileType.Png, "signature.png");
    }
}
```

**Method Signature:**
```csharp
Task SaveAsync(SignatureFileType fileType = SignatureFileType.Png, string fileName = "Signature")
```

### File Format Options

#### PNG Format

Best for transparency and universal compatibility:

```razor
await signatureRef.SaveAsync(SignatureFileType.Png, "signature.png");
```

**Advantages:**
- Lossless compression
- Supports transparency
- Universal browser support
- Good for documents

**Use Cases:** Forms, documents, contracts, web display

#### JPEG Format

Smaller file size, no transparency:

```razor
await signatureRef.SaveAsync(SignatureFileType.Jpeg, "signature.jpg");
```

**Advantages:**
- Smaller file size
- Good compression
- Wide compatibility

**Disadvantages:**
- No transparency
- Lossy compression

**Use Cases:** Email attachments, archival storage

#### SVG Format

Vector format, infinitely scalable:

```razor
await signatureRef.SaveAsync(SignatureFileType.Svg, "signature.svg");
```

**Advantages:**
- Scalable without quality loss
- Small file size for simple signatures
- Editable in vector programs

**Use Cases:** Print documents, scalable signatures, graphic design

### Format Selector Example

```razor
<div class="save-options">
    <SfSignature @ref="signatureRef"></SfSignature>
    
    <div class="format-selector">
        <label>Save as:</label>
        <button @onclick="@(() => SaveAs(SignatureFileType.Png))">PNG</button>
        <button @onclick="@(() => SaveAs(SignatureFileType.Jpeg))">JPEG</button>
        <button @onclick="@(() => SaveAs(SignatureFileType.Svg))">SVG</button>
    </div>
    
    @if (!string.IsNullOrEmpty(saveMessage))
    {
        <div class="alert alert-success">@saveMessage</div>
    }
</div>

@code {
    private SfSignature signatureRef;
    private string saveMessage = "";
    
    private async Task SaveAs(SignatureFileType fileType)
    {
        string extension = fileType switch
        {
            SignatureFileType.Png => "png",
            SignatureFileType.Jpeg => "jpg",
            SignatureFileType.Svg => "svg",
            _ => "png"
        };
        
        string filename = $"signature-{DateTime.Now:yyyyMMdd-HHmmss}.{extension}";
        await signatureRef.SaveAsync(fileType, filename);
        saveMessage = $"Saved as {filename}";
    }
}
```

## SaveWithBackground Property

Control whether to include the background in saved file:

```razor
<!-- Include background image/color in saved file -->
<SfSignature BackgroundImage="images/letterhead.png"
             SaveWithBackground="true">
</SfSignature>

<!-- Save only the signature strokes (transparent background) -->
<SfSignature BackgroundImage="images/letterhead.png"
             SaveWithBackground="false">
</SfSignature>
```

**Default:** `true`

### Use Cases

**SaveWithBackground="true":**
- Letterhead documents
- Pre-printed forms
- Complete document with background
- Standalone signature with branding

**SaveWithBackground="false":**
- Overlay signature on different backgrounds later
- Transparent signature for flexibility
- Separate signature from document template

### Example with Toggle

```razor
<div class="signature-with-toggle">
    <label>
        <input type="checkbox" @bind="includeBackground" />
        Include background in saved file
    </label>
    
    <SfSignature @ref="signatureRef"
                 BackgroundImage="images/form-template.png"
                 SaveWithBackground="@includeBackground">
    </SfSignature>
    
    <button @onclick="SaveSignature">Save Signature</button>
</div>

@code {
    private SfSignature signatureRef;
    private bool includeBackground = true;
    
    private async Task SaveSignature()
    {
        string filename = includeBackground 
            ? "signature-with-background.png" 
            : "signature-transparent.png";
        
        await signatureRef.SaveAsync(SignatureFileType.Png, filename);
    }
}
```

## GetSignature() Method

Retrieve signature as Base64 string for server upload:

```razor
<SfSignature @ref="signatureRef"></SfSignature>

<button @onclick="UploadToServer">Upload Signature</button>

@code {
    private SfSignature signatureRef;
    
    private async Task UploadToServer()
    {
        // Get signature as Base64 string
        string signatureData = await signatureRef.GetSignatureAsync(SignatureFileType.Png);
        
        if (!string.IsNullOrEmpty(signatureData))
        {
            // Upload to server
            await SaveToDatabase(signatureData);
        }
    }
    
    private async Task SaveToDatabase(string base64Signature)
    {
        // Your server API call here
        // Example: await Http.PostAsJsonAsync("api/signatures", new { signature = base64Signature });
    }
}
```

### Format Options for GetSignature

```razor
// Get as PNG (Base64)
string pngData = await signatureRef.GetSignatureAsync(SignatureFileType.Png);

// Get as JPEG (Base64)
string jpegData = await signatureRef.GetSignatureAsync(SignatureFileType.Jpeg);

// Get as SVG (XML string)
string svgData = await signatureRef.GetSignatureAsync(SignatureFileType.Svg);
```

### Server Integration Example

```razor
@inject HttpClient Http

<SfSignature @ref="signatureRef"></SfSignature>

<button @onclick="SubmitSignature" disabled="@isSubmitting">
    @(isSubmitting ? "Submitting..." : "Submit Signature")
</button>

@if (submissionResult != null)
{
    <div class="alert alert-@(submissionResult.Success ? "success" : "danger")">
        @submissionResult.Message
    </div>
}

@code {
    private SfSignature signatureRef;
    private bool isSubmitting = false;
    private SubmissionResult submissionResult;
    
    private async Task SubmitSignature()
    {
        isSubmitting = true;
        
        try
        {
            // Get signature as Base64
            string signatureBase64 = await signatureRef.GetSignatureAsync(SignatureFileType.Png);
            
            if (string.IsNullOrEmpty(signatureBase64))
            {
                submissionResult = new SubmissionResult 
                { 
                    Success = false, 
                    Message = "Please provide a signature" 
                };
                return;
            }
            
            // Send to server
            var response = await Http.PostAsJsonAsync("api/signatures/upload", new 
            {
                SignatureData = signatureBase64,
                UserId = "user123",
                DocumentId = "doc456",
                Timestamp = DateTime.UtcNow
            });
            
            if (response.IsSuccessStatusCode)
            {
                var result = await response.Content.ReadFromJsonAsync<ApiResponse>();
                submissionResult = new SubmissionResult 
                { 
                    Success = true, 
                    Message = $"Signature saved with ID: {result.SignatureId}" 
                };
                
                await signatureRef.ClearAsync();
            }
            else
            {
                submissionResult = new SubmissionResult 
                { 
                    Success = false, 
                    Message = "Failed to upload signature" 
                };
            }
        }
        catch (Exception ex)
        {
            submissionResult = new SubmissionResult 
            { 
                Success = false, 
                Message = $"Error: {ex.Message}" 
            };
        }
        finally
        {
            isSubmitting = false;
        }
    }
    
    public class SubmissionResult
    {
        public bool Success { get; set; }
        public string Message { get; set; }
    }
    
    public class ApiResponse
    {
        public string SignatureId { get; set; }
    }
}
```

## Load() Method

Load an existing signature from Base64 string:

```razor
<SfSignature @ref="signatureRef"></SfSignature>

<button @onclick="LoadExistingSignature">Load Previous Signature</button>

@code {
    private SfSignature signatureRef;
    
    private async Task LoadExistingSignature()
    {
        // Base64 signature data from database or file
        string existingSignature = "data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAA...";
        
        await signatureRef.LoadAsync(existingSignature);
    }
}
```

### Loading from Server

```razor
@inject HttpClient Http

<SfSignature @ref="signatureRef"></SfSignature>

<button @onclick="LoadFromServer">Load Saved Signature</button>

@code {
    private SfSignature signatureRef;
    
    private async Task LoadFromServer()
    {
        try
        {
            // Fetch signature from server
            var response = await Http.GetAsync("api/signatures/user123");
            
            if (response.IsSuccessStatusCode)
            {
                var signatureData = await response.Content.ReadFromJsonAsync<SignatureDto>();
                
                if (!string.IsNullOrEmpty(signatureData.Base64Data))
                {
                    await signatureRef.LoadAsync(signatureData.Base64Data);
                }
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Failed to load signature: {ex.Message}");
        }
    }
    
    public class SignatureDto
    {
        public string Base64Data { get; set; }
        public DateTime CreatedAt { get; set; }
    }
}
```

## Clear() Method

Remove the signature from the canvas:

```razor
<SfSignature @ref="signatureRef"></SfSignature>

<button @onclick="ClearSignature">Clear</button>

@code {
    private SfSignature signatureRef;
    
    private async Task ClearSignature()
    {
        await signatureRef.ClearAsync();
    }
}
```

### Clear with Confirmation

```razor
<SfSignature @ref="signatureRef"></SfSignature>

<button @onclick="ClearWithConfirmation">Clear</button>

@code {
    private SfSignature signatureRef;
    
    private async Task ClearWithConfirmation()
    {
        bool confirmed = await JS.InvokeAsync<bool>(
            "confirm", 
            "Are you sure you want to clear the signature?"
        );
        
        if (confirmed)
        {
            await signatureRef.ClearAsync();
        }
    }
}
```

## Complete Save/Load Example

```razor
@page "/signature-saveload"
@inject HttpClient Http
@inject IJSRuntime JS

<div class="signature-manager">
    <h3>Signature Management</h3>
    
    <div class="signature-canvas-wrapper">
        <SfSignature @ref="signatureRef"
                     BackgroundColor="#FFFFFF"
                     StrokeColor="#000000"
                     MinStrokeWidth="0.5"
                     MaxStrokeWidth="2.0">
        </SfSignature>
    </div>
    
    <div class="action-bar">
        <div class="btn-group">
            <button class="btn btn-primary" @onclick="SaveLocally">
                <i class="bi bi-download"></i> Save Locally
            </button>
            
            <button class="btn btn-success" @onclick="UploadToServer">
                <i class="bi bi-cloud-upload"></i> Upload to Server
            </button>
        </div>
        
        <div class="btn-group">
            <button class="btn btn-info" @onclick="LoadFromServer">
                <i class="bi bi-cloud-download"></i> Load from Server
            </button>
            
            <button class="btn btn-warning" @onclick="ClearSignature">
                <i class="bi bi-eraser"></i> Clear
            </button>
        </div>
    </div>
    
    <div class="format-options">
        <label>Save Format:</label>
        <select @bind="selectedFormat" class="form-select">
            <option value="@SignatureFileType.Png">PNG (Recommended)</option>
            <option value="@SignatureFileType.Jpeg">JPEG (Smaller)</option>
            <option value="@SignatureFileType.Svg">SVG (Scalable)</option>
        </select>
    </div>
    
    @if (!string.IsNullOrEmpty(statusMessage))
    {
        <div class="alert alert-@(isError ? "danger" : "success") mt-3">
            @statusMessage
        </div>
    }
</div>

@code {
    private SfSignature signatureRef;
    private SignatureFileType selectedFormat = SignatureFileType.Png;
    private string statusMessage = "";
    private bool isError = false;
    
    private async Task SaveLocally()
    {
        try
        {
            string ext = selectedFormat switch
            {
                SignatureFileType.Jpeg => "jpg",
                SignatureFileType.Svg => "svg",
                _ => "png"
            };
            
            string filename = $"signature-{DateTime.Now:yyyyMMddHHmmss}.{ext}";
            await signatureRef.SaveAsync(selectedFormat, filename);
            
            SetStatus($"Signature saved as {filename}", false);
        }
        catch (Exception ex)
        {
            SetStatus($"Save failed: {ex.Message}", true);
        }
    }
    
    private async Task UploadToServer()
    {
        try
        {
            string signatureData = await signatureRef.GetSignatureAsync(SignatureFileType.Png);
            
            if (string.IsNullOrEmpty(signatureData))
            {
                SetStatus("Please draw a signature first", true);
                return;
            }
            
            var payload = new
            {
                SignatureBase64 = signatureData,
                UserId = "current-user-id",
                Timestamp = DateTime.UtcNow
            };
            
            var response = await Http.PostAsJsonAsync("api/signatures", payload);
            
            if (response.IsSuccessStatusCode)
            {
                SetStatus("Signature uploaded successfully", false);
            }
            else
            {
                SetStatus("Upload failed", true);
            }
        }
        catch (Exception ex)
        {
            SetStatus($"Upload error: {ex.Message}", true);
        }
    }
    
    private async Task LoadFromServer()
    {
        try
        {
            var response = await Http.GetAsync("api/signatures/current-user-id");
            
            if (response.IsSuccessStatusCode)
            {
                var data = await response.Content.ReadFromJsonAsync<SignatureResponse>();
                await signatureRef.LoadAsync(data.SignatureBase64);
                SetStatus("Signature loaded from server", false);
            }
            else
            {
                SetStatus("No signature found on server", true);
            }
        }
        catch (Exception ex)
        {
            SetStatus($"Load error: {ex.Message}", true);
        }
    }
    
    private async Task ClearSignature()
    {
        bool confirmed = await JS.InvokeAsync<bool>("confirm", "Clear the signature?");
        if (confirmed)
        {
            await signatureRef.ClearAsync();
            SetStatus("Signature cleared", false);
        }
    }
    
    private void SetStatus(string message, bool error)
    {
        statusMessage = message;
        isError = error;
    }
    
    public class SignatureResponse
    {
        public string SignatureBase64 { get; set; }
        public DateTime CreatedAt { get; set; }
    }
}
```

## Database Storage Strategies

### SQL Server Example

```csharp
// Entity Framework model
public class UserSignature
{
    public int Id { get; set; }
    public string UserId { get; set; }
    public string SignatureBase64 { get; set; }
    public DateTime CreatedAt { get; set; }
    public string FileFormat { get; set; } // "PNG", "JPEG", "SVG"
}

// Save to database
public async Task SaveSignature(string userId, string signatureBase64)
{
    var signature = new UserSignature
    {
        UserId = userId,
        SignatureBase64 = signatureBase64,
        CreatedAt = DateTime.UtcNow,
        FileFormat = "PNG"
    };
    
    _context.UserSignatures.Add(signature);
    await _context.SaveChangesAsync();
}

// Load from database
public async Task<string> GetSignature(string userId)
{
    var signature = await _context.UserSignatures
        .Where(s => s.UserId == userId)
        .OrderByDescending(s => s.CreatedAt)
        .FirstOrDefaultAsync();
    
    return signature?.SignatureBase64;
}
```

### Azure Blob Storage Example

```csharp
// Save to Azure Blob Storage
public async Task<string> SaveToBlob(string signatureBase64, string userId)
{
    var blobClient = _blobContainerClient.GetBlobClient($"{userId}/signature.png");
    
    byte[] signatureBytes = Convert.FromBase64String(
        signatureBase64.Replace("data:image/png;base64,", "")
    );
    
    using (var stream = new MemoryStream(signatureBytes))
    {
        await blobClient.UploadAsync(stream, overwrite: true);
    }
    
    return blobClient.Uri.ToString();
}
```

## Best Practices

1. **Format Selection**: Use PNG for most cases, JPEG for smaller files
2. **Server Upload**: Use GetSignatureAsync() + HTTP POST for server storage
3. **Validation**: Always check if signature is empty before saving
4. **Error Handling**: Wrap save/load operations in try-catch blocks
5. **User Feedback**: Show clear status messages for all operations
6. **Confirmation**: Ask for confirmation before clearing signatures
7. **Storage**: Store Base64 in database or convert to binary for file storage
8. **Compression**: Consider compressing large signatures before database storage
9. **Security**: Validate signatures server-side, don't trust client data
10. **Audit Trail**: Store timestamp and user ID with each signature

## Troubleshooting

**Save not working:**
- Check if signature canvas has content
- Verify file format is correctly specified
- Ensure browser allows file downloads

**GetSignature returns empty:**
- User hasn't drawn anything yet
- Component not fully initialized
- Check for JavaScript errors in console

**Load fails:**
- Verify Base64 string format is correct
- Ensure data URI includes proper prefix
- Check for corrupted or invalid image data
