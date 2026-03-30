# File Source & Input Methods

## Table of Contents
- [Drag and Drop Upload](#drag)
- [Form Integration](#form)
- [Direct File Picker](#picker)
- [Browser File Dialog](#dialog)
- [Directory Selection](#directory)
- [Multiple Input Methods](#multiple)
- [Accessibility Best Practices](#a11y)

---

## Drag and Drop Upload

Enhance user experience with drag-and-drop functionality (enabled by default).

### Basic Drag-and-Drop

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader AutoUpload="true">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
</SfUploader>

<style>
    /* Provide visual feedback for drag-over state */
    .e-uploader.e-dragging {
        background-color: #e7f3ff;
        border-color: #007bff;
    }
</style>
```

### Custom Drag-and-Drop Area

```razor
@using Syncfusion.Blazor.Inputs

<div class="drag-drop-area" @ondrop="@OnDrop" @ondragover="@OnDragOver" @ondragleave="@OnDragLeave">
    <SfUploader @ref="uploader" AutoUpload="false" ID="dragDropUploader">
        <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
        <UploaderTemplates>
            <Template>
                <div class="drag-drop-content">
                    <div class="icon">📁</div>
                    <h3>Drop files here or click to browse</h3>
                    <p class="info">Supported formats: JPG, PNG, PDF</p>
                </div>
            </Template>
        </UploaderTemplates>
    </SfUploader>
</div>

<style>
    .drag-drop-area {
        border: 3px dashed #007bff;
        border-radius: 8px;
        padding: 40px 20px;
        text-align: center;
        background-color: #f8f9fa;
        cursor: pointer;
        transition: all 0.3s;
    }

    .drag-drop-area.dragging-over {
        background-color: #e7f3ff;
        border-color: #0056b3;
        box-shadow: 0 0 10px rgba(0, 123, 255, 0.3);
    }

    .drag-drop-content .icon {
        font-size: 48px;
        margin-bottom: 10px;
    }

    .drag-drop-content .info {
        color: #6c757d;
        font-size: 12px;
    }
</style>

@code {
    SfUploader uploader;
    bool isDraggingOver = false;

    private void OnDragOver(DragEventArgs e)
    {
        e.preventDefault();
        isDraggingOver = true;
    }

    private void OnDragLeave(DragEventArgs e)
    {
        isDraggingOver = false;
    }

    private void OnDrop(DragEventArgs e)
    {
        e.preventDefault();
        isDraggingOver = false;
        // Files handled by uploader component
    }
}
```

---

## Form Integration

Integrate file upload into HTML forms:

### Basic Form Integration

```razor
@using Syncfusion.Blazor.Inputs

<form @onsubmit="@OnFormSubmit">
    <div class="form-group">
        <label for="name">Name:</label>
        <input type="text" id="name" class="form-control" @bind="formData.Name" />
    </div>

    <div class="form-group">
        <label>Upload Resume:</label>
        <SfUploader @ref="uploader" 
                    AutoUpload="false"
                    AllowMultiple="false"
                    AllowedExtensions=".pdf,.doc,.docx">
            <UploaderEvents ValueChange="@OnFileChange"></UploaderEvents>
        </SfUploader>
    </div>

    <button type="submit" class="btn btn-primary">Submit</button>
</form>

@code {
    SfUploader uploader;
    
    public class FormData
    {
        public string Name { get; set; }
        public string ResumeFile { get; set; }
    }

    FormData formData = new();

    private void OnFileChange(UploadChangeEventArgs args)
    {
        if (args.Files.Count > 0)
        {
            formData.ResumeFile = args.Files[0].FileName;
        }
    }

    private async Task OnFormSubmit()
    {
        if (string.IsNullOrEmpty(formData.Name) || string.IsNullOrEmpty(formData.ResumeFile))
        {
            Console.WriteLine("Please fill all fields");
            return;
        }

        // Upload files
        if (uploader != null)
        {
            await uploader.UploadAsync();
        }

        Console.WriteLine($"Form submitted: {formData.Name}");
    }
}
```

### EditForm Integration

```razor
@using System.ComponentModel.DataAnnotations
@using Syncfusion.Blazor.Inputs

<EditForm Model="@model" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <div class="form-group">
        <label>Email:</label>
        <SfTextBox @bind-Value="@model.Email"></SfTextBox>
        <ValidationMessage For="() => model.Email" />
    </div>

    <div class="form-group">
        <label>Document:</label>
        <SfUploader @ref="uploader" 
                    AutoUpload="false"
                    AllowMultiple="false">
            <UploaderEvents FileSelected="@OnFileSelected"></UploaderEvents>
        </SfUploader>
        <ValidationMessage For="() => model.DocumentName" />
    </div>

    <button type="submit" class="btn btn-primary">Submit</button>
</EditForm>

@code {
    public class SubmissionModel
    {
        [Required(ErrorMessage = "Email is required")]
        public string Email { get; set; }

        [Required(ErrorMessage = "Document is required")]
        public string DocumentName { get; set; }
    }

    SubmissionModel model = new();
    SfUploader uploader;

    private void OnFileSelected(SelectedEventArgs args)
    {
        if (args.FilesData.Count > 0)
        {
            model.DocumentName = args.FilesData[0].Name;
        }
    }

    private async Task HandleValidSubmit()
    {
        if (uploader != null)
        {
            await uploader.UploadAsync();
        }
        Console.WriteLine("Form submitted");
    }
}
```

---

## Direct File Picker

Trigger file picker programmatically:

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader @ref="uploader" AutoUpload="false" ID="fileUploader">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
</SfUploader>

<button class="btn btn-primary" @onclick="UploadFiles">
    Upload Files
</button>

@code {
    SfUploader uploader;

    private async Task UploadFiles()
    {
        if (uploader != null)
        {
            await uploader.UploadAsync();
        }
    }
}
```

---

## Browser File Dialog

Standard browser file selection dialog:

```razor
@using Syncfusion.Blazor.Inputs

<div>
    <p>File Selection Method: <strong>Browser Dialog</strong></p>
    
    <SfUploader AutoUpload="true"
                AllowMultiple="true"
                AllowedExtensions=".jpg,.png,.pdf">
        <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    </SfUploader>
</div>

<style>
    /* Browser dialog inherits system styling */
    /* No custom styling needed for native browser dialog */
</style>
```

---

## Directory Selection

Allow users to select entire directories:

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader AutoUpload="true" DirectoryUpload="true">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderTemplates>
        <Template>
            <div class="directory-upload-area">
                <div class="icon">📂</div>
                <h3>Upload Folder</h3>
                <p>Select a directory to upload all files within it</p>
            </div>
        </Template>
    </UploaderTemplates>
</SfUploader>

<style>
    .directory-upload-area {
        padding: 30px;
        text-align: center;
        border: 2px dashed #007bff;
        border-radius: 8px;
    }

    .directory-upload-area .icon {
        font-size: 48px;
    }
</style>
```

### Browser Compatibility
- **Chrome/Edge**: Full support
- **Firefox**: Full support
- **Safari**: Limited support
- **Mobile browsers**: Variable support

---

## Multiple Input Methods

Combine multiple file input methods:

```razor
@using Syncfusion.Blazor.Inputs

<div class="multi-input-upload">
    <div class="input-methods">
        <div class="method" @onclick="@(() => OpenMethod('drag'))">
            <div class="icon">🖱️</div>
            <p>Drag & Drop</p>
        </div>
        <div class="method" @onclick="@(() => OpenMethod('browse'))">
            <div class="icon">📂</div>
            <p>Browse</p>
        </div>
        <div class="method" @onclick="@(() => OpenMethod('folder'))">
            <div class="icon">📁</div>
            <p>Upload Folder</p>
        </div>
        <div class="method" @onclick="@(() => OpenMethod('url'))">
            <div class="icon">🌐</div>
            <p>From URL</p>
        </div>
    </div>

    <div class="upload-area">
        @if (selectedMethod == "drag")
        {
            <SfUploader @ref="uploader" AutoUpload="false">
                <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
            </SfUploader>
        }
        else if (selectedMethod == "folder")
        {
            <SfUploader @ref="uploader" AutoUpload="false" DirectoryUpload="true">
                <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
            </SfUploader>
        }
    </div>
</div>

<style>
    .multi-input-upload {
        max-width: 800px;
        margin: 20px auto;
    }

    .input-methods {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
        gap: 15px;
        margin-bottom: 30px;
    }

    .method {
        padding: 20px;
        border: 2px solid #dee2e6;
        border-radius: 8px;
        text-align: center;
        cursor: pointer;
        transition: all 0.3s;
    }

    .method:hover {
        border-color: #007bff;
        background-color: #e7f3ff;
    }

    .method.active {
        border-color: #007bff;
        background-color: #007bff;
        color: white;
    }

    .method .icon {
        font-size: 32px;
        margin-bottom: 10px;
    }

    .upload-area {
        padding: 20px;
        border: 2px dashed #007bff;
        border-radius: 8px;
    }
</style>

@code {
    SfUploader uploader;
    string selectedMethod = "drag";

    private void OpenMethod(string method)
    {
        selectedMethod = method;
        StateHasChanged();
    }
}
```

---

## Accessibility Best Practices

### WCAG Compliant Upload

```razor
@using Syncfusion.Blazor.Inputs

<div class="accessible-upload">
    <h2 id="upload-label">Upload Your Files</h2>
    
    <SfUploader aria-labelledby="upload-label"
                aria-describedby="upload-help"
                AllowMultiple="true"
                MaxFileSize="104857600">
        <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    </SfUploader>

    <p id="upload-help" class="help-text">
        Accepted formats: JPG, PNG, PDF. Maximum 100 MB per file.
        Use Tab to navigate, Enter to activate.
    </p>
</div>

<style>
    .help-text {
        font-size: 14px;
        color: #6c757d;
        margin-top: 10px;
    }

    /* Focus visible for keyboard navigation */
    :focus-visible {
        outline: 3px solid #007bff;
        outline-offset: 2px;
    }
</style>
```

### Keyboard Navigation

```razor
@using Syncfusion.Blazor.Inputs

<div class="keyboard-accessible-upload">
    <SfUploader @ref="uploader" 
                AutoUpload="false"
                role="region"
                aria-label="File upload area">
        <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    </SfUploader>

    <div class="keyboard-instructions">
        <p><strong>Keyboard Instructions:</strong></p>
        <ul>
            <li><kbd>Tab</kbd> - Move to upload area</li>
            <li><kbd>Enter</kbd> - Activate file browser</li>
            <li><kbd>Space</kbd> - Open selected file dialog</li>
            <li><kbd>Ctrl+A</kbd> - Select all files</li>
        </ul>
    </div>

    <div class="upload-buttons">
        <button class="btn btn-primary" 
                @onclick="@(async () => await uploader.UploadAsync())"
                aria-label="Upload selected files">
            Upload
        </button>
        <button class="btn btn-secondary" 
                @onclick="@(async () => await uploader.ClearAllAsync())"
                aria-label="Clear all selected files">
            Clear
        </button>
    </div>
</div>

<style>
    .keyboard-instructions {
        margin-top: 20px;
        padding: 15px;
        background-color: #f8f9fa;
        border-left: 4px solid #007bff;
    }

    kbd {
        background: #333;
        color: white;
        padding: 2px 6px;
        border-radius: 3px;
        font-family: monospace;
    }
</style>

@code {
    SfUploader uploader;
}
```

### Screen Reader Support

```razor
@using Syncfusion.Blazor.Inputs

<div class="sr-only-labels">
    <!-- Hidden labels for screen readers -->
    <label for="uploader-input" class="sr-only">
        Select files to upload. Supported formats: JPG, PNG, PDF.
    </label>

    <SfUploader ID="uploader-input"
                aria-describedby="upload-instructions"
                AutoUpload="false">
        <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    </SfUploader>

    <div id="upload-instructions" class="sr-only">
        <p>Files selected will be listed below with size and status.</p>
        <p>Use Upload button to proceed or Clear to remove selections.</p>
    </div>
</div>

<style>
    .sr-only {
        position: absolute;
        width: 1px;
        height: 1px;
        padding: 0;
        margin: -1px;
        overflow: hidden;
        clip: rect(0, 0, 0, 0);
        white-space: nowrap;
        border: 0;
    }
</style>
```

---

## Complete Multi-Method Upload Example

```razor
@page "/multi-input-upload"
@using Syncfusion.Blazor.Inputs

<div class="comprehensive-upload">
    <h1>File Upload Center</h1>

    <div class="upload-methods">
        <SfUploader @ref="uploader1" 
                    AutoUpload="true"
                    CssClass="method-card">
            <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
            <UploaderTemplates>
                <Template>
                    <div class="method-content">
                        <div class="method-icon">🖱️</div>
                        <h3>Drag & Drop</h3>
                        <p>Drop files here</p>
                    </div>
                </Template>
            </UploaderTemplates>
        </SfUploader>

        <SfUploader @ref="uploader2" 
                    AutoUpload="false"
                    DirectoryUpload="true"
                    CssClass="method-card">
            <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
            <UploaderTemplates>
                <Template>
                    <div class="method-content">
                        <div class="method-icon">📁</div>
                        <h3>Upload Folder</h3>
                        <p>Select directory</p>
                    </div>
                </Template>
            </UploaderTemplates>
        </SfUploader>
    </div>
</div>

<style>
    .comprehensive-upload {
        max-width: 1000px;
        margin: 40px auto;
        padding: 20px;
    }

    .upload-methods {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
        gap: 20px;
        margin-top: 30px;
    }

    .method-card {
        border: 2px solid #dee2e6;
        border-radius: 8px;
        padding: 20px;
    }

    .method-content {
        text-align: center;
    }

    .method-icon {
        font-size: 48px;
        margin-bottom: 10px;
    }
</style>

@code {
    SfUploader uploader1;
    SfUploader uploader2;
}
```
