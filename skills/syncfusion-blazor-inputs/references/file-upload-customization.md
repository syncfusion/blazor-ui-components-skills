# Customization & Styling

## Table of Contents
- [Custom File List Template](#template)
- [CSS Class Customization](#css)
- [Theme Integration](#theme)
- [Custom Button Text with UploaderButtons](#custom-button-text-with-uploaderbuttons)
- [Custom Button Styling](#button)
- [Dark Mode Support](#darkmode)
- [Responsive Design](#responsive)
- [Custom Icons](#icons)

---

## Custom File List Template

Customize how files appear in the upload list:

### Basic Custom Template

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader AutoUpload="false">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderTemplates>
        <Template>
            <div style="padding: 20px;">
                <h3>Drag and drop files here</h3>
                <p>or click to select files</p>
            </div>
        </Template>
    </UploaderTemplates>
</SfUploader>
```

### Advanced File List Template

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader @ref="uploader" AutoUpload="false">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderTemplates>
        <Template>
            <div class="custom-upload-container">
                <div class="upload-icon">📁</div>
                <h3>Upload Your Files</h3>
                <p class="text-muted">Drag and drop or click to browse</p>
                <p class="file-restrictions">Maximum file size: 100 MB</p>
            </div>
        </Template>
    </UploaderTemplates>
</SfUploader>

<style>
    .custom-upload-container {
        text-align: center;
        padding: 40px 20px;
        border: 2px dashed #007bff;
        border-radius: 8px;
        background-color: #f8f9fa;
        cursor: pointer;
    }

    .custom-upload-container:hover {
        background-color: #e9ecef;
        border-color: #0056b3;
    }

    .upload-icon {
        font-size: 48px;
        margin-bottom: 10px;
    }

    .file-restrictions {
        font-size: 12px;
        color: #6c757d;
        margin: 10px 0 0 0;
    }
</style>

@code {
    SfUploader uploader;
}
```

---

## CSS Class Customization

Override default styles with CSS classes:

### Custom Container Styling

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader CssClass="custom-uploader">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
</SfUploader>

<style>
    /* Custom uploader styling */
    .custom-uploader {
        border: 2px solid #007bff;
        border-radius: 8px;
        padding: 20px;
    }

    .custom-uploader .e-file-select {
        background-color: #007bff;
        color: white;
        border: none;
        padding: 10px 20px;
        border-radius: 4px;
        cursor: pointer;
    }

    .custom-uploader .e-file-select:hover {
        background-color: #0056b3;
    }
</style>
```

### File List Item Styling

```razor
<style>
    /* Style uploaded file list items */
    .e-upload .e-file-list-item {
        padding: 12px;
        margin-bottom: 8px;
        border-left: 4px solid #007bff;
        background-color: #f8f9fa;
    }

    .e-upload .e-file-list-item:hover {
        background-color: #e9ecef;
    }

    /* Success state */
    .e-file-list-item.e-success {
        border-left-color: #28a745;
        background-color: #d4edda;
    }

    /* Error state */
    .e-file-list-item.e-error {
        border-left-color: #dc3545;
        background-color: #f8d7da;
    }
</style>
```

---

## Theme Integration

Syncfusion provides built-in themes:

### Bootstrap 5 Theme

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
```

### Material Design 3 Theme

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/material3.css" rel="stylesheet" />
</head>
```

### Fluent 2 Theme

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/fluent2.css" rel="stylesheet" />
</head>
```

### Tailwind CSS Theme

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />
</head>
```

---

## Custom Button Styling

### Styled Upload Button

```razor
@using Syncfusion.Blazor.Inputs

<div class="upload-container">
    <SfUploader @ref="uploader" 
                AutoUpload="false"
                CssClass="styled-uploader">
        <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    </SfUploader>

    <div class="custom-buttons mt-3">
        <button class="btn btn-primary" @onclick="@(async () => await uploader.UploadAsync())">
            <span class="icon">📤</span> Upload Files
        </button>
        <button class="btn btn-secondary" @onclick="@(async () => await uploader.ClearAllAsync())">
            <span class="icon">🗑️</span> Clear All
        </button>
    </div>
</div>

<style>
    .styled-uploader {
        border: 2px dashed #007bff;
        border-radius: 8px;
        padding: 30px;
        text-align: center;
        background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
    }

    .custom-buttons {
        display: flex;
        gap: 10px;
        justify-content: center;
    }

    .custom-buttons .btn {
        display: flex;
        align-items: center;
        gap: 8px;
        padding: 10px 20px;
        border-radius: 4px;
        font-weight: 500;
    }

    .custom-buttons .btn-primary {
        background-color: #007bff;
        color: white;
        border: none;
    }

    .custom-buttons .btn-primary:hover {
        background-color: #0056b3;
    }

    .custom-buttons .btn-secondary {
        background-color: #6c757d;
        color: white;
        border: none;
    }

    .custom-buttons .btn-secondary:hover {
        background-color: #545b62;
    }

    .btn .icon {
        font-size: 16px;
    }
</style>

@code {
    SfUploader uploader;
}
```

---

## Dark Mode Support

### Dark Theme with CSS Variables

```razor
@using Syncfusion.Blazor.Inputs

<div class="@(isDarkMode ? "dark-mode" : "light-mode")">
    <button @onclick="@(() => isDarkMode = !isDarkMode)">
        Toggle Dark Mode
    </button>

    <SfUploader CssClass="adaptive-uploader">
        <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    </SfUploader>
</div>

<style>
    :root {
        --uploader-bg: #ffffff;
        --uploader-border: #dee2e6;
        --uploader-text: #333333;
        --uploader-hover-bg: #f8f9fa;
    }

    .dark-mode {
        --uploader-bg: #1e1e1e;
        --uploader-border: #404040;
        --uploader-text: #e0e0e0;
        --uploader-hover-bg: #2d2d2d;
    }

    .adaptive-uploader {
        background-color: var(--uploader-bg);
        border: 2px solid var(--uploader-border);
        color: var(--uploader-text);
        padding: 20px;
        border-radius: 8px;
    }

    .adaptive-uploader:hover {
        background-color: var(--uploader-hover-bg);
    }
</style>

@code {
    bool isDarkMode = false;
}
```

---

## Responsive Design

### Mobile-First Responsive Layout

```razor
@using Syncfusion.Blazor.Inputs

<div class="responsive-upload-container">
    <SfUploader AllowMultiple="true" 
                AutoUpload="false"
                CssClass="responsive-uploader">
        <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    </SfUploader>

    <div class="responsive-buttons">
        <button class="btn btn-upload">Upload</button>
        <button class="btn btn-clear">Clear</button>
    </div>
</div>

<style>
    .responsive-upload-container {
        margin: 20px auto;
        max-width: 600px;
        padding: 20px;
    }

    .responsive-uploader {
        padding: 20px;
        border: 2px dashed #007bff;
        border-radius: 8px;
        text-align: center;
    }

    .responsive-buttons {
        display: grid;
        gap: 10px;
        margin-top: 20px;
    }

    /* Mobile: Stack buttons vertically */
    @media (max-width: 576px) {
        .responsive-upload-container {
            padding: 10px;
        }

        .responsive-uploader {
            padding: 15px;
        }

        .responsive-buttons {
            grid-template-columns: 1fr;
        }

        .btn {
            width: 100%;
            padding: 12px;
        }
    }

    /* Tablet: Two columns */
    @media (min-width: 577px) and (max-width: 992px) {
        .responsive-buttons {
            grid-template-columns: 1fr 1fr;
        }
    }

    /* Desktop: Wider container */
    @media (min-width: 993px) {
        .responsive-upload-container {
            max-width: 800px;
        }
    }
</style>
```

---

## Custom Button Text with UploaderButtons

Customize the text displayed on uploader action buttons using the `UploaderButtons` component.

### Basic Button Customization

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader AutoUpload="false">
    <UploaderAsyncSettings SaveUrl="api/upload/save" RemoveUrl="api/upload/remove"></UploaderAsyncSettings>
    <UploaderButtons Browse="Choose Files" 
                     Upload="Start Upload" 
                     Clear="Remove All">
    </UploaderButtons>
</SfUploader>
```

### Localized Button Text

```razor
@using Syncfusion.Blazor.Inputs

<h3>English Upload</h3>
<SfUploader AutoUpload="false">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderButtons Browse="Browse" Upload="Upload" Clear="Clear"></UploaderButtons>
</SfUploader>

<h3>Spanish Upload</h3>
<SfUploader AutoUpload="false">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderButtons Browse="Seleccionar" Upload="Cargar" Clear="Limpiar"></UploaderButtons>
</SfUploader>

<h3>French Upload</h3>
<SfUploader AutoUpload="false">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderButtons Browse="Parcourir" Upload="Télécharger" Clear="Effacer"></UploaderButtons>
</SfUploader>
```

### Dynamic Button Text

```razor
@using Syncfusion.Blazor.Inputs

<div class="language-selector">
    <button @onclick="@(() => SetLanguage("en"))">English</button>
    <button @onclick="@(() => SetLanguage("es"))">Español</button>
    <button @onclick="@(() => SetLanguage("fr"))">Français</button>
</div>

<SfUploader AutoUpload="false">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderButtons Browse="@browseText" 
                     Upload="@uploadText" 
                     Clear="@clearText">
    </UploaderButtons>
</SfUploader>

@code {
    private string browseText = "Browse";
    private string uploadText = "Upload";
    private string clearText = "Clear";

    private Dictionary<string, Dictionary<string, string>> buttonTexts = new()
    {
        {
            "en", new Dictionary<string, string>
            {
                { "browse", "Browse" },
                { "upload", "Upload" },
                { "clear", "Clear" }
            }
        },
        {
            "es", new Dictionary<string, string>
            {
                { "browse", "Seleccionar" },
                { "upload", "Cargar" },
                { "clear", "Limpiar" }
            }
        },
        {
            "fr", new Dictionary<string, string>
            {
                { "browse", "Parcourir" },
                { "upload", "Télécharger" },
                { "clear", "Effacer" }
            }
        }
    };

    private void SetLanguage(string lang)
    {
        if (buttonTexts.ContainsKey(lang))
        {
            browseText = buttonTexts[lang]["browse"];
            uploadText = buttonTexts[lang]["upload"];
            clearText = buttonTexts[lang]["clear"];
        }
    }
}
```

### Properties

| Property | Type | Description |
|----------|------|-------------|
| `Browse` | String | Text for the browse/select files button |
| `Upload` | String | Text for the upload button |
| `Clear` | String | Text for the clear/remove all button |

---

## Custom Icons

### Icon Integration

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader AutoUpload="false" CssClass="icon-uploader">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderTemplates>
        <Template>
            <div class="upload-area-with-icons">
                <div class="icon-large">📤</div>
                <h3>Upload Files</h3>
                <p>Drag files or <span class="link">browse</span></p>
                <div class="file-icons">
                    <span title="Images">🖼️</span>
                    <span title="Documents">📄</span>
                    <span title="Videos">🎬</span>
                    <span title="Archives">📦</span>
                </div>
            </div>
        </Template>
    </UploaderTemplates>
</SfUploader>

<style>
    .upload-area-with-icons {
        padding: 40px 20px;
        text-align: center;
    }

    .icon-large {
        font-size: 64px;
        margin-bottom: 15px;
        animation: bounce 2s infinite;
    }

    @keyframes bounce {
        0%, 100% { transform: translateY(0); }
        50% { transform: translateY(-10px); }
    }

    .file-icons {
        display: flex;
        justify-content: center;
        gap: 15px;
        margin-top: 20px;
        font-size: 24px;
    }

    .file-icons span {
        cursor: help;
        transition: transform 0.2s;
    }

    .file-icons span:hover {
        transform: scale(1.2);
    }

    .link {
        color: #007bff;
        text-decoration: underline;
        cursor: pointer;
    }
</style>
```

---

## Complete Customization Example

```razor
@page "/custom-upload"
@using Syncfusion.Blazor.Inputs

<div class="@(isDarkMode ? "custom-upload-dark" : "custom-upload-light")">
    <div class="upload-section">
        <h2>Advanced File Upload</h2>
        
        <div class="theme-toggle">
            <button @onclick="@(() => isDarkMode = !isDarkMode)">
                @(isDarkMode ? "☀️ Light" : "🌙 Dark")
            </button>
        </div>

        <SfUploader @ref="uploader"
                    AllowMultiple="true"
                    AutoUpload="false"
                    MaxFileSize="104857600"
                    CssClass="premium-uploader">
            <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
            <UploaderTemplates>
                <Template>
                    <div class="premium-upload-area">
                        <div class="upload-icon">🚀</div>
                        <h3>Upload Your Files</h3>
                        <p>Drag and drop or click to select</p>
                        <div class="supported-types">
                            Supported: Images, Documents, Videos
                        </div>
                    </div>
                </Template>
            </UploaderTemplates>
        </SfUploader>

        <div class="action-buttons">
            <button class="btn btn-primary" @onclick="@(async () => await uploader.UploadAsync())">
                📤 Upload
            </button>
            <button class="btn btn-secondary" @onclick="@(async () => await uploader.ClearAllAsync())">
                🗑️ Clear
            </button>
        </div>
    </div>
</div>

<style>
    :root {
        --primary: #007bff;
        --primary-dark: #0056b3;
        --bg: #ffffff;
        --text: #333333;
        --border: #dee2e6;
    }

    .custom-upload-dark {
        --primary: #4da3ff;
        --primary-dark: #0d6efd;
        --bg: #1e1e1e;
        --text: #e0e0e0;
        --border: #404040;
    }

    .upload-section {
        max-width: 600px;
        margin: 40px auto;
        padding: 30px;
        background: var(--bg);
        color: var(--text);
        border-radius: 12px;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }

    .theme-toggle {
        text-align: right;
        margin-bottom: 20px;
    }

    .premium-uploader {
        border: 2px dashed var(--primary);
        border-radius: 8px;
        padding: 20px;
        margin: 20px 0;
        background: linear-gradient(to right, rgba(0, 123, 255, 0.05), rgba(0, 123, 255, 0.1));
    }

    .premium-upload-area {
        text-align: center;
        padding: 20px;
    }

    .upload-icon {
        font-size: 48px;
        margin-bottom: 10px;
        animation: pulse 2s infinite;
    }

    @keyframes pulse {
        0%, 100% { opacity: 1; }
        50% { opacity: 0.7; }
    }

    .supported-types {
        font-size: 12px;
        color: #6c757d;
        margin-top: 10px;
    }

    .action-buttons {
        display: flex;
        gap: 10px;
        justify-content: center;
        margin-top: 20px;
    }

    .btn {
        padding: 10px 20px;
        border: none;
        border-radius: 4px;
        cursor: pointer;
        font-weight: 500;
        transition: all 0.3s;
    }

    .btn-primary {
        background: var(--primary);
        color: white;
    }

    .btn-primary:hover {
        background: var(--primary-dark);
    }

    .btn-secondary {
        background: #6c757d;
        color: white;
    }

    .btn-secondary:hover {
        background: #545b62;
    }
</style>

@code {
    SfUploader uploader;
    bool isDarkMode = false;
}
```
