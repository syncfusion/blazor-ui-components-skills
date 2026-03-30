# File Validation & Constraints

## Table of Contents
- [AllowedExtensions Validation](#extensions)
- [File Size Validation](#size)
- [File Count Validation](#count)
- [Custom Validation Function](#custom)
- [Cancel Invalid Uploads](#cancel)
- [Preventing Invalid Selections](#prevent)
- [Error Messages](#errors)
- [Validation Patterns](#patterns)
- [Form Integration](#form)

---

## AllowedExtensions Validation

The `AllowedExtensions` property filters selected or dropped files against specified file types. Matching is case-insensitive.

### Basic Configuration

```razor
<SfUploader AllowedExtensions=".jpg,.jpeg,.png,.gif" />
```

### Grouped Extension Sets

**Documents Only**:
```razor
<SfUploader AllowedExtensions=".pdf,.doc,.docx,.xls,.xlsx,.ppt,.pptx" />
```

**Images Only**:
```razor
<SfUploader AllowedExtensions=".jpg,.jpeg,.png,.gif,.bmp,.svg,.webp" />
```

**Media Files**:
```razor
<SfUploader AllowedExtensions=".mp4,.mov,.avi,.mkv,.mp3,.wav,.flac,.aac" />
```

**Archive Files**:
```razor
<SfUploader AllowedExtensions=".zip,.rar,.7z,.tar,.gz" />
```

### MIME Type Validation (Server-Side)

```csharp
private static bool IsValidMimeType(string fileName, string mimeType)
{
    var validTypes = new Dictionary<string, string[]>
    {
        { ".pdf", new[] { "application/pdf" } },
        { ".jpg", new[] { "image/jpeg" } },
        { ".png", new[] { "image/png" } },
        { ".docx", new[] { "application/vnd.openxmlformats-officedocument.wordprocessingml.document" } }
    };

    var ext = Path.GetExtension(fileName).ToLower();
    return validTypes.TryGetValue(ext, out var types) && types.Contains(mimeType);
}
```

### Important Notes
- **Client-side filtering only**: Always validate server-side
- **Case-insensitive**: `.jpg` matches `.JPG`
- **Include leading dot**: Use `.jpg` not `jpg`
- **Security**: Never trust client-side validation alone

---

## File Size Validation

Validate files based on size (in bytes) to prevent empty or oversized uploads.

### Size Configuration

```razor
<!-- Minimum 1 KB, Maximum 10 MB -->
<SfUploader MinFileSize="1024" MaxFileSize="10485760" />
```

### Common Size Limits

```razor
<!-- 1 MB limit -->
<SfUploader MaxFileSize="1048576" />

<!-- 5 MB limit -->
<SfUploader MaxFileSize="5242880" />

<!-- 10 MB limit -->
<SfUploader MaxFileSize="10485760" />

<!-- 50 MB limit -->
<SfUploader MaxFileSize="52428800" />

<!-- 100 MB limit -->
<SfUploader MaxFileSize="104857600" />
```

### Size Conversion Helpers

```csharp
public static class FileSizeHelper
{
    public const long OneKB = 1_024;
    public const long OneMB = 1_048_576;
    public const long OneGB = 1_073_741_824;

    public static long ToBytes(double megabytes) => (long)(megabytes * OneMB);
    
    public static string FormatBytes(long bytes)
    {
        return bytes switch
        {
            < OneKB => $"{bytes} B",
            < OneMB => $"{bytes / (double)OneKB:F2} KB",
            < OneGB => $"{bytes / (double)OneMB:F2} MB",
            _ => $"{bytes / (double)OneGB:F2} GB"
        };
    }
}
```

### Usage with Validation

```razor
<SfUploader MaxFileSize="@FileSizeHelper.ToBytes(5)">
    <!-- Allows files up to 5 MB -->
</SfUploader>
```

### Example: Size Validation with Message

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader AllowMultiple="true" 
            MinFileSize="1024"
            MaxFileSize="10485760">
    <UploaderEvents FileSelected="@OnFileSelected"></UploaderEvents>
</SfUploader>

<div>@sizeMessage</div>

@code {
    private string sizeMessage = "";

    private void OnFileSelected(SelectedEventArgs args)
    {
        sizeMessage = "";
        foreach (var file in args.FilesData)
        {
            if (file.Size < 1024)
            {
                sizeMessage += $"'{file.Name}' is too small. ";
                args.Cancel = true;
            }
            else if (file.Size > 10485760)
            {
                sizeMessage += $"'{file.Name}' exceeds 10 MB limit. ";
                args.Cancel = true;
            }
        }
    }
}
```

---

## File Count Validation

Limit number of files when `AllowMultiple="true"`:

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader AllowMultiple="true">
    <UploaderEvents FileSelected="@OnFileSelected"></UploaderEvents>
</SfUploader>

<div>@countMessage</div>

@code {
    private string countMessage = "";
    private const int MaxFiles = 5;

    private void OnFileSelected(SelectedEventArgs args)
    {
        countMessage = "";
        if (args.FilesData.Count > MaxFiles)
        {
            countMessage = $"Maximum {MaxFiles} files allowed";
            args.Cancel = true;
        }
    }
}
```

---

## Custom Validation Function

Combine multiple validation criteria:

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader AllowMultiple="true">
    <UploaderEvents FileSelected="@OnFileSelected"></UploaderEvents>
</SfUploader>

<div>@validationMessage</div>

@code {
    private string validationMessage = "";

    private void OnFileSelected(SelectedEventArgs args)
    {
        validationMessage = "";
        
        foreach (var file in args.FilesData)
        {
            var validationError = ValidateFile(file);
            if (!string.IsNullOrEmpty(validationError))
            {
                validationMessage += $"{file.Name}: {validationError} ";
                args.Cancel = true;
            }
        }
    }

    private string ValidateFile(FileInfo file)
    {
        // Check file type
        var allowedExtensions = new[] { ".jpg", ".png", ".pdf" };
        var ext = Path.GetExtension(file.Name).ToLower();
        if (!allowedExtensions.Contains(ext))
            return "Invalid file type";

        // Check file size
        if (file.Size < 1024)
            return "File too small";
        
        if (file.Size > 5242880) // 5 MB
            return "File exceeds 5 MB";

        // Check file name
        if (file.Name.Length > 255)
            return "File name too long";

        return null; // Valid
    }
}
```

---

## Cancel Invalid Uploads

Use `args.Cancel = true` to prevent invalid files:

```razor
<SfUploader AllowedExtensions=".jpg,.png">
    <UploaderEvents FileSelected="@OnFileSelected"></UploaderEvents>
</SfUploader>

@code {
    private void OnFileSelected(SelectedEventArgs args)
    {
        foreach (var file in args.FilesData)
        {
            if (!IsValidImage(file.Name))
            {
                args.Cancel = true;
                Console.WriteLine($"Rejected: {file.Name}");
            }
        }
    }

    private bool IsValidImage(string fileName)
    {
        var ext = Path.GetExtension(fileName).ToLower();
        return ext == ".jpg" || ext == ".jpeg" || ext == ".png";
    }
}
```

---

## Preventing Invalid Selections

Provide real-time feedback:

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader @ref="uploader">
    <UploaderEvents FileSelected="@OnFileSelected"></UploaderEvents>
</SfUploader>

<div class="validation-feedback">
    @if (!string.IsNullOrEmpty(validationFeedback))
    {
        <div class="alert alert-danger">@validationFeedback</div>
    }
</div>

@code {
    SfUploader uploader;
    private string validationFeedback = "";

    private void OnFileSelected(SelectedEventArgs args)
    {
        validationFeedback = "";
        var invalidFiles = new List<string>();

        foreach (var file in args.FilesData)
        {
            if (!IsValidForUpload(file))
            {
                invalidFiles.Add(file.Name);
            }
        }

        if (invalidFiles.Any())
        {
            validationFeedback = $"Invalid files: {string.Join(", ", invalidFiles)}";
            args.Cancel = true;
        }
    }

    private bool IsValidForUpload(FileInfo file)
    {
        var validExtensions = new[] { ".jpg", ".png", ".pdf" };
        var ext = Path.GetExtension(file.Name).ToLower();
        
        return validExtensions.Contains(ext) && file.Size <= 5242880;
    }
}
```

---

## Error Messages

Display meaningful error messages:

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader AllowMultiple="true" 
            AllowedExtensions=".jpg,.png,.pdf"
            MaxFileSize="5242880">
    <UploaderEvents FileSelected="@OnFileSelected"></UploaderEvents>
</SfUploader>

@if (!string.IsNullOrEmpty(errorMessage))
{
    <div class="alert alert-warning mt-3">@errorMessage</div>
}

@code {
    private string errorMessage = "";

    private void OnFileSelected(SelectedEventArgs args)
    {
        errorMessage = GetErrorMessage(args.FilesData);
        if (!string.IsNullOrEmpty(errorMessage))
            args.Cancel = true;
    }

    private string GetErrorMessage(List<FileInfo> files)
    {
        var errors = new List<string>();

        foreach (var file in files)
        {
            var ext = Path.GetExtension(file.Name).ToLower();
            
            if (ext != ".jpg" && ext != ".png" && ext != ".pdf")
                errors.Add($"'{file.Name}': Invalid file type. Use JPG, PNG, or PDF.");
            
            if (file.Size > 5242880)
                errors.Add($"'{file.Name}': File exceeds 5 MB limit.");
            
            if (file.Size == 0)
                errors.Add($"'{file.Name}': File is empty.");
        }

        return errors.Any() ? string.Join(" ", errors) : null;
    }
}
```

---

## Validation Patterns

### Pattern 1: Simple Type Validation

```razor
<SfUploader AllowedExtensions=".jpg,.png,.gif">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
</SfUploader>
```

### Pattern 2: Size + Type Validation

```razor
<SfUploader AllowedExtensions=".pdf,.doc,.docx"
            MaxFileSize="52428800">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
</SfUploader>
```

### Pattern 3: Comprehensive Validation

```razor
<SfUploader AllowMultiple="true"
            AllowedExtensions=".jpg,.png,.pdf"
            MinFileSize="1024"
            MaxFileSize="10485760">
    <UploaderEvents FileSelected="@OnFileSelected"></UploaderEvents>
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
</SfUploader>

@code {
    private void OnFileSelected(SelectedEventArgs args)
    {
        foreach (var file in args.FilesData)
        {
            if (!ValidateFile(file))
                args.Cancel = true;
        }
    }

    private bool ValidateFile(FileInfo file)
    {
        // Multi-criteria validation
        return ValidateExtension(file.Name) && 
               ValidateSize(file.Size) && 
               ValidateName(file.Name);
    }

    private bool ValidateExtension(string name) => 
        name.EndsWith(".jpg") || name.EndsWith(".png") || name.EndsWith(".pdf");

    private bool ValidateSize(long size) => 
        size >= 1024 && size <= 10485760;

    private bool ValidateName(string name) => 
        !name.Contains("..") && name.Length <= 255;
}
```

---

## Form Integration

Validate within EditForm:

```razor
@using System.ComponentModel.DataAnnotations
@using Syncfusion.Blazor.Inputs

<EditForm Model="@employee" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    
    <div class="form-group">
        <label>Name:</label>
        <SfTextBox @bind-Value="@employee.Name"></SfTextBox>
        <ValidationMessage For="() => employee.Name" />
    </div>

    <div class="form-group">
        <label>Resume:</label>
        <SfUploader @ref="uploader" 
                    AllowMultiple="false" 
                    AutoUpload="false"
                    AllowedExtensions=".pdf">
            <UploaderEvents FileSelected="@OnFileSelected"></UploaderEvents>
        </SfUploader>
        <ValidationMessage For="() => employee.ResumeFile" />
    </div>

    <button type="submit" class="btn btn-primary">Submit</button>
</EditForm>

@code {
    [Required(ErrorMessage = "Please upload a resume")]
    public class Employee
    {
        [Required]
        public string Name { get; set; }
        
        [Required(ErrorMessage = "Resume is required")]
        public string ResumeFile { get; set; }
    }

    Employee employee = new();
    SfUploader uploader;

    private void OnFileSelected(SelectedEventArgs args)
    {
        if (args.FilesData.Count > 0)
        {
            employee.ResumeFile = args.FilesData[0].Name;
        }
    }

    private void HandleValidSubmit()
    {
        Console.WriteLine("Form submitted successfully");
    }
}
```

---

## Server-Side Validation Checklist

Always implement server-side validation:

- ✓ Validate file type (check MIME type, not just extension)
- ✓ Validate file size
- ✓ Scan for malware (consider antivirus scanning)
- ✓ Sanitize file names (remove special characters)
- ✓ Check file content (read file header, not just extension)
- ✓ Restrict upload location (use secure directory)
- ✓ Limit concurrent uploads
- ✓ Log upload attempts for security audit
