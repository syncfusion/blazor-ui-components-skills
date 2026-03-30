# File Upload Configuration

## Table of Contents
- [ID Property](#id)
- [AllowedExtensions](#extensions)
- [AllowMultiple](#multiple)
- [AutoUpload](#autoupload)
- [SequentialUpload](#sequential)
- [DirectoryUpload](#directory)
- [MaxFileSize](#maxfilesize)
- [MinFileSize](#minfilesize)
- [Enabled State](#enabled)
- [Name Property](#name)
- [Configuration Examples](#examples)

---

## ID Property

The `ID` property provides a unique identifier for the FileUpload component. This is essential for referencing the component in JavaScript, CSS, and server-side operations.

```razor
<SfUploader ID="myFileUploadComponent" name="UploadFiles" />
```

**When to use:**
- Referencing component in JavaScript interop
- Styling specific upload instances
- Server-side parameter matching (name attribute)

**Note:** The `ID` value must be unique across your application. When using `AsyncSettings`, the `name` property must match the controller's POST method parameter name.

---

## AllowedExtensions

The `AllowedExtensions` property specifies which file types can be uploaded. This provides client-side validation and file type filtering.

```razor
<SfUploader AllowedExtensions=".jpg,.jpeg,.png,.gif" />
```

### Common Extension Sets

**Images:**
```razor
AllowedExtensions=".jpg,.jpeg,.png,.gif,.bmp,.svg"
```

**Documents:**
```razor
AllowedExtensions=".pdf,.doc,.docx,.xls,.xlsx,.ppt,.pptx"
```

**Media:**
```razor
AllowedExtensions=".mp4,.mov,.avi,.mp3,.wav,.flac"
```

**Multiple Types:**
```razor
AllowedExtensions=".pdf,.jpg,.png,.docx"
```

**Important Notes:**
- Multiple extensions separated by commas
- Include the leading dot (`.jpg` not `jpg`)
- Matching is case-insensitive
- Always perform server-side validation for security

---

## AllowMultiple

The `AllowMultiple` property determines whether users can select one or multiple files at once.

```razor
<!-- Single file only -->
<SfUploader AllowMultiple="false" />

<!-- Multiple files allowed -->
<SfUploader AllowMultiple="true" />
```

### Use Cases

| Setting | Use Case |
|---------|----------|
| `false` | Profile picture upload, single document submission |
| `true` | Batch image uploads, multiple PDF uploads |

**Default:** `true` (multiple file selection enabled)

---

## AutoUpload

The `AutoUpload` property controls whether files upload immediately after selection or wait for manual upload button click.

```razor
<!-- Automatic upload -->
<SfUploader AutoUpload="true" />

<!-- Manual upload button required -->
<SfUploader AutoUpload="false" />
```

### Comparison

| Setting | Behavior | Use When |
|---------|----------|----------|
| `true` | Uploads immediately after selection | Quick submissions, user convenience |
| `false` | User clicks upload button | Batch uploads, file review needed |

**Default:** `true`

**Example with Manual Upload:**
```razor
<SfUploader @ref="uploader" AutoUpload="false">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
</SfUploader>

<button @onclick="UploadFiles">Upload Selected Files</button>

@code {
    SfUploader uploader;
    
    private async Task UploadFiles()
    {
        // Trigger upload manually
        await uploader.UploadAsync();
    }
}
```

---

## SequentialUpload

The `SequentialUpload` property (only active when `AllowMultiple="true"`) determines upload order for multiple files.

```razor
<!-- Upload all files concurrently -->
<SfUploader AllowMultiple="true" SequentialUpload="false" />

<!-- Upload files one at a time -->
<SfUploader AllowMultiple="true" SequentialUpload="true" />
```

### Comparison

| Setting | Behavior | Server Load | Use When |
|---------|----------|------------|----------|
| `false` | All files upload simultaneously | High | Powerful servers, fast connections |
| `true` | Files upload one at a time | Low | Limited server resources, mobile users |

**Default:** `false`

**Note:** Useful for managing server load and ensuring orderly processing of uploaded files.

---

## DirectoryUpload

The `DirectoryUpload` property enables users to select and upload entire directories instead of individual files.

```razor
<SfUploader DirectoryUpload="true" />
```

### Features
- Select entire folder structure
- Maintains folder hierarchy
- Useful for bulk uploads
- Browser support varies

**Default:** `false`

**Use Cases:**
- Backup folder uploads
- Project folder uploads
- Archive extraction preparation

---

## MaxFileSize

The `MaxFileSize` property sets the maximum allowed file size in **bytes**. Prevents users from uploading files exceeding this limit.

```razor
<!-- 10 MB limit -->
<SfUploader MaxFileSize="10485760" />

<!-- 100 MB limit -->
<SfUploader MaxFileSize="104857600" />

<!-- 1 GB limit -->
<SfUploader MaxFileSize="1073741824" />
```

### Size Conversion Reference
- 1 KB = 1,024 bytes
- 1 MB = 1,048,576 bytes (1024 × 1024)
- 10 MB = 10,485,760 bytes
- 100 MB = 104,857,600 bytes
- 1 GB = 1,073,741,824 bytes

**Important:**
- Server infrastructure (Kestrel/IIS) imposes additional limits
- Align client limits with server configuration


**Server Configuration Example (ASP.NET Core):**
```csharp
// Program.cs
builder.Services.Configure<FormOptions>(options =>
{
    options.MultipartBodyLengthLimit = 104857600; // 100 MB
});
```

---

## MinFileSize

The `MinFileSize` property sets the minimum required file size in **bytes**. Prevents uploading empty or very small files.

```razor
<!-- Minimum 1 KB -->
<SfUploader MinFileSize="1024" />

<!-- Minimum 5 MB -->
<SfUploader MinFileSize="5242880" />
```

### Common Scenarios
- Avoid empty file uploads: `MinFileSize="1"`
- Ensure meaningful data: `MinFileSize="1024"` (1 KB)

**Default:** 0 bytes (no minimum)

---

## Enabled State

The `Enabled` property controls whether the FileUpload component is interactive or disabled.

```razor
<!-- Component is active -->
<SfUploader Enabled="true" />

<!-- Component is disabled -->
<SfUploader Enabled="false" />
```

### Dynamic Enable/Disable

```razor
<SfUploader @ref="uploader" Enabled="@isEnabled" />

<button @onclick="@(() => isEnabled = !isEnabled)">Toggle Upload</button>

@code {
    SfUploader uploader;
    bool isEnabled = true;
}
```

**Use Cases:**
- Disable until form validation passes
- Disable during payment processing
- Disable based on user permissions

---

## Name Property

The `name` attribute in the HTML input element must match the server-side POST method parameter name.

```razor
<!-- Component name must match controller parameter -->
<SfUploader name="UploadFiles" />
```

**Server-Side Match:**
```csharp
[HttpPost("[action]")]
public void Save(IList<IFormFile> UploadFiles)
{
    // 'UploadFiles' must match name attribute above
    foreach (var file in UploadFiles)
    {
        // Process file
    }
}
```

**Note:** If `ID` differs from desired `name`, use `htmlAttributes` property:
```razor
<SfUploader ID="fileUploadId" name="DocumentFiles" />
```

---

## Configuration Examples

### Example 1: Strict Image Upload
```razor
<SfUploader 
    AutoUpload="true"
    AllowMultiple="false"
    AllowedExtensions=".jpg,.jpeg,.png"
    MaxFileSize="5242880"
    ID="profilePictureUpload"
    name="ProfilePicture">
    <UploaderAsyncSettings SaveUrl="api/profile/upload"></UploaderAsyncSettings>
</SfUploader>
```

### Example 2: Batch Document Upload
```razor
<SfUploader 
    AutoUpload="false"
    AllowMultiple="true"
    SequentialUpload="true"
    AllowedExtensions=".pdf,.doc,.docx"
    MaxFileSize="52428800"
    MinFileSize="1024"
    ID="documentBatchUpload"
    name="Documents">
    <UploaderAsyncSettings SaveUrl="api/documents/save"></UploaderAsyncSettings>
</SfUploader>
```

### Example 3: Flexible File Upload with Directory Support
```razor
<SfUploader 
    AutoUpload="false"
    AllowMultiple="true"
    DirectoryUpload="true"
    AllowedExtensions=".jpg,.pdf,.docx,.xlsx"
    MaxFileSize="104857600"
    ID="flexibleUpload"
    name="FlexibleFiles">
    <UploaderAsyncSettings SaveUrl="api/files/save"></UploaderAsyncSettings>
</SfUploader>
```

### Example 4: Progressive Upload Control
```razor
<SfUploader 
    @ref="uploader"
    AutoUpload="false"
    AllowMultiple="true"
    Enabled="@uploadEnabled"
    AllowedExtensions=".jpg,.png,.pdf"
    MaxFileSize="10485760">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
</SfUploader>

<button @onclick="@(() => uploadEnabled = !uploadEnabled)">
    Toggle Upload: @(uploadEnabled ? "Enabled" : "Disabled")
</button>

@code {
    SfUploader uploader;
    bool uploadEnabled = true;
}
```

---

## Key Configuration Checklist

Before deploying file upload feature, verify:
- ✓ `AllowedExtensions` matches security policy
- ✓ `MaxFileSize` aligns with server limits
- ✓ `name` attribute matches server parameter
- ✓ `AutoUpload` matches user workflow
- ✓ `AllowMultiple` matches use case
- ✓ Server-side validation implemented
- ✓ Upload directory has write permissions
- ✓ API endpoint properly secured
