# Localization & Accessibility

## Table of Contents
- [Locale Configuration](#locale)
- [Custom Text Labels](#labels)
- [WCAG 2.1 Compliance](#wcag)
- [Keyboard Navigation](#keyboard)
- [Screen Reader Support](#screenreader)
- [ARIA Attributes](#aria)

---

## Locale Configuration

The SfUploader component **does not have a `Locale` property**. Instead, localization is automatically determined by the application's current culture (system culture or application-level culture setting). The UI text, error messages, and tooltips will automatically adjust based on the culture configured in your Blazor application.

### How Localization Works in SfUploader

The component uses the CultureInfo set in your application to determine the language for:
- Button labels (Browse, Upload, Clear, Remove)
- Validation messages
- Error notifications
- Status messages

### Setting Application Culture

To configure the application's culture globally, set it in your Blazor app startup:

```csharp
// In Program.cs (Blazor Web App or WebAssembly)
using System.Globalization;

var builder = WebApplication.CreateBuilder(args);

// Set default culture
var cultureInfo = new CultureInfo("es-ES");
CultureInfo.DefaultThreadCurrentCulture = cultureInfo;
CultureInfo.DefaultThreadCurrentUICulture = cultureInfo;

// ... rest of configuration
```

### Dynamic Culture Switching at Runtime

To dynamically change the application culture:

```razor
@using System.Globalization
@inject NavigationManager Navigation

<div class="culture-selector">
    <label>Select Language:</label>
    <select @onchange="OnCultureChange">
        <option value="en-US">English</option>
        <option value="es-ES">Español</option>
        <option value="fr-FR">Français</option>
        <option value="de-DE">Deutsch</option>
        <option value="zh-CN">中文</option>
        <option value="ja-JP">日本語</option>
        <option value="pt-BR">Português</option>
        <option value="it-IT">Italiano</option>
    </select>
</div>

<SfUploader AutoUpload="false">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
</SfUploader>

@code {
    private void OnCultureChange(ChangeEventArgs e)
    {
        var culture = e.Value.ToString();
        var cultureInfo = new CultureInfo(culture);
        CultureInfo.DefaultThreadCurrentCulture = cultureInfo;
        CultureInfo.DefaultThreadCurrentUICulture = cultureInfo;
        
        // Reload page to apply new culture
        Navigation.NavigateTo(Navigation.Uri, forceLoad: true);
    }
}
```

**Note:** The SfUploader component will automatically display text in the configured culture. For custom labels and messages, implement them using your application's localization framework.

---

## Custom Text Labels

Override default text with custom labels:

### Custom Button Text

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader AutoUpload="false">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderTemplate>
        <SfTemplate>
            <div class="custom-upload">
                <h3>Upload Your Document</h3>
                <p>Select or drag your file here</p>
            </div>
        </SfTemplate>
    </UploaderTemplate>
</SfUploader>

<div class="custom-buttons">
    <button class="btn-upload">📤 Enviar Arquivo</button>
    <button class="btn-clear">🗑️ Limpar</button>
</div>
```

### Custom File Status Messages

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader @ref="uploader" AutoUpload="false">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderEvents 
        Success="@OnSuccess"
        OnFailure="@OnFailure">
    </UploaderEvents>
</SfUploader>

<div class="status-message">@statusText</div>

@code {
    SfUploader uploader;
    string statusText = "";

    private void OnSuccess(SuccessEventArgs args)
    {
        // Custom message
        statusText = $"✅ Arquivo '{args.File.Name}' enviado com sucesso!";
    }

    private void OnFailure(FailureEventArgs args)
    {
        // Custom message
        statusText = $"❌ Erro ao enviar '{args.File.Name}': {args.StatusText}";
    }
}
```

### Multilingual Custom Labels

```razor
@using Syncfusion.Blazor.Inputs

<div class="language-selector">
    <button @onclick="@(() => currentLanguage = "en")">English</button>
    <button @onclick="@(() => currentLanguage = "es")">Español</button>
    <button @onclick="@(() => currentLanguage = "fr")">Français</button>
</div>

<SfUploader AutoUpload="false">
    <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    <UploaderTemplates>
        <Template>
            <div class="upload-content">
                <h3>@GetLabel("title")</h3>
                <p>@GetLabel("description")</p>
            </div>
        </Template>
    </UploaderTemplates>
</SfUploader>

<style>
    .language-selector {
        margin-bottom: 20px;
    }

    .language-selector button {
        margin-right: 10px;
        padding: 8px 16px;
        border: 1px solid #ddd;
        cursor: pointer;
    }
</style>

@code {
    private string currentLanguage = "en";

    private Dictionary<string, Dictionary<string, string>> labels = new()
    {
        {
            "en", new Dictionary<string, string>
            {
                { "title", "Upload Files" },
                { "description", "Drag and drop or click to select" }
            }
        },
        {
            "es", new Dictionary<string, string>
            {
                { "title", "Cargar Archivos" },
                { "description", "Arrastra y suelta o haz clic para seleccionar" }
            }
        },
        {
            "fr", new Dictionary<string, string>
            {
                { "title", "Charger les Fichiers" },
                { "description", "Glissez-déposez ou cliquez pour sélectionner" }
            }
        }
    };

    private string GetLabel(string key)
    {
        return labels.ContainsKey(currentLanguage) &&
               labels[currentLanguage].ContainsKey(key)
            ? labels[currentLanguage][key]
            : key;
    }
}
```

---

## WCAG 2.1 Compliance

Ensure compliance with Web Content Accessibility Guidelines 2.1 Level AA.

### Semantic HTML

```razor
@using Syncfusion.Blazor.Inputs

<section aria-label="File upload section">
    <h2 id="upload-heading">Upload Documents</h2>
    
    <SfUploader aria-labelledby="upload-heading"
                aria-describedby="upload-instructions"
                role="region">
        <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    </SfUploader>

    <div id="upload-instructions" class="instructions">
        <p>Accepted formats: PDF, DOC, DOCX (Max 10MB)</p>
    </div>
</section>
```

### Color Contrast

```css
.upload-button {
    /* Ensure 4.5:1 contrast ratio */
    background-color: #003366;  /* Dark blue */
    color: #ffffff;             /* White */
    border: 2px solid #001a33;
}

.error-message {
    /* Red with sufficient contrast */
    background-color: #fde4e4;  /* Light pink */
    color: #cc0000;             /* Dark red - 4.5:1 ratio */
}

.success-message {
    /* Green with sufficient contrast */
    background-color: #e4f5e4;  /* Light green */
    color: #006600;             /* Dark green - 4.5:1 ratio */
}
```

### Focus Indicators

```css
.uploader:focus-visible,
.upload-button:focus-visible {
    outline: 3px solid #4d94ff;
    outline-offset: 2px;
}

/* Remove default outline */
*:focus {
    outline: none;
}

*:focus-visible {
    outline: 3px solid #4d94ff;
    outline-offset: 2px;
}
```

---

## Keyboard Navigation

Provide full keyboard access:

```razor
@using Syncfusion.Blazor.Inputs

<div class="keyboard-nav-upload">
    <h2>Keyboard-Accessible Upload</h2>

    <SfUploader @ref="uploader"
                @onkeydown="@OnKeyDown"
                AutoUpload="false"
                role="region"
                aria-label="File upload with keyboard navigation">
        <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
    </SfUploader>

    <div class="keyboard-instructions">
        <h3>Keyboard Shortcuts</h3>
        <table>
            <tr>
                <td><kbd>Tab</kbd></td>
                <td>Navigate between controls</td>
            </tr>
            <tr>
                <td><kbd>Enter</kbd> / <kbd>Space</kbd></td>
                <td>Activate focused control</td>
            </tr>
            <tr>
                <td><kbd>Ctrl</kbd> + <kbd>U</kbd></td>
                <td>Trigger upload</td>
            </tr>
            <tr>
                <td><kbd>Delete</kbd></td>
                <td>Remove selected files</td>
            </tr>
        </table>
    </div>
</div>

@code {
    SfUploader uploader;

    private async Task OnKeyDown(KeyboardEventArgs e)
    {
        if (e.CtrlKey && e.Key == "u")
        {
            e.preventDefault();
            await uploader.UploadAsync();
        }
        else if (e.Key == "Delete")
        {
            await uploader.ClearAllAsync();
        }
    }
}
```

---

## Screen Reader Support

Provide comprehensive screen reader support:

```razor
@using Syncfusion.Blazor.Inputs

<div class="sr-accessible-upload">
    <!-- Main landmark with label -->
    <section aria-label="File upload area">
        <h2 id="main-heading">Upload Your Files</h2>
        
        <!-- Primary uploader -->
        <SfUploader id="main-uploader"
                    aria-labelledby="main-heading"
                    aria-describedby="main-instructions"
                    aria-busy="false">
            <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
            <UploaderEvents OnUploadStart="@OnUploadStart" Success="@OnUploadEnd"></UploaderEvents>
        </SfUploader>

        <!-- Instructions (hidden visually but available to screen readers) -->
        <p id="main-instructions" class="sr-only">
            Use this form to upload documents. Select one or more files and click Upload.
            Files must be less than 10MB in size.
        </p>
    </section>

    <!-- Status announcements -->
    <div id="upload-status" class="sr-only" role="status" aria-live="polite">
        @statusAnnouncement
    </div>

    <!-- File list with descriptions -->
    <div class="file-list" role="list">
        @foreach (var file in uploadedFiles)
        {
            <div class="file-item" role="listitem" aria-label="@file.Name - @file.Size - @file.Status">
                @file.Name
            </div>
        }
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

    .file-list {
        margin-top: 20px;
    }

    .file-item {
        padding: 10px;
        border-bottom: 1px solid #ddd;
    }
</style>

@code {
    private List<FileInfo> uploadedFiles = new();
    private string statusAnnouncement = "";

    private class FileInfo
    {
        public string Name { get; set; }
        public string Size { get; set; }
        public string Status { get; set; }
    }

    private void OnUploadStart(UploadingEventArgs args)
    {
        statusAnnouncement = $"Starting upload of {args.FileData.Name}";
    }

    private void OnUploadEnd(SuccessEventArgs args)
    {
        statusAnnouncement = $"Successfully uploaded {args.File.Name}";
        uploadedFiles.Add(new FileInfo 
        { 
            Name = args.File.Name, 
            Size = $"{args.File.Size} bytes",
            Status = "Uploaded"
        });
    }
}
```

---

## ARIA Attributes

Use ARIA attributes for enhanced accessibility:

```razor
@using Syncfusion.Blazor.Inputs

<div class="aria-enhanced-upload">
    <!-- Main region with label -->
    <div role="region" aria-label="File upload form">
        <h2 id="form-title">Document Upload</h2>

        <!-- Uploader with descriptions -->
        <div aria-labelledby="form-title" 
             aria-describedby="format-info"
             class="uploader-wrapper">
            <SfUploader id="doc-uploader"
                        AllowedExtensions=".pdf,.doc,.docx"
                        MaxFileSize="10485760">
                <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
                <UploaderEvents Success="@OnSuccess" OnFailure="@OnFailure"></UploaderEvents>
            </SfUploader>
        </div>

        <!-- Format information -->
        <p id="format-info" class="format-note">
            Supported formats: PDF, DOC, DOCX (maximum 10 MB each)
        </p>

        <!-- Status indicator -->
        <div id="upload-status" 
             role="status" 
             aria-live="assertive" 
             aria-atomic="true"
             class="status-indicator">
            @statusMessage
        </div>
    </div>
</div>

<style>
    .uploader-wrapper {
        padding: 20px;
        border: 2px solid #ddd;
        border-radius: 4px;
    }

    .format-note {
        font-size: 14px;
        color: #666;
        margin-top: 10px;
    }

    .status-indicator {
        padding: 10px;
        margin-top: 15px;
        border-radius: 4px;
        min-height: 20px;
    }

    .status-indicator.success {
        background: #d4edda;
        color: #155724;
        border: 1px solid #c3e6cb;
    }

    .status-indicator.error {
        background: #f8d7da;
        color: #721c24;
        border: 1px solid #f5c6cb;
    }
</style>

@code {
    private string statusMessage = "";

    private void OnSuccess(SuccessEventArgs args)
    {
        statusMessage = $"✓ {args.File.Name} uploaded successfully";
    }

    private void OnFailure(FailureEventArgs args)
    {
        statusMessage = $"✗ {args.File.Name} failed to upload";
    }
}
```

---

## Complete Accessible Upload Example

```razor
@page "/accessible-upload"
@using Syncfusion.Blazor.Inputs

<div class="accessible-upload-app">
    <header>
        <h1>Accessible Document Upload Center</h1>
    </header>

    <main role="main">
        <!-- Language selector -->
        <div class="language-controls">
            <label for="lang-select">Language:</label>
            <select id="lang-select" @onchange="@((ChangeEventArgs e) => currentLang = e.Value.ToString())">
                <option value="en">English</option>
                <option value="es">Español</option>
                <option value="de">Deutsch</option>
            </select>
        </div>

        <!-- Upload form -->
        <section aria-label="File upload section" class="upload-section">
            <h2>@GetText("upload_heading")</h2>

            <SfUploader @ref="uploader"
                        id="main-uploader"
                        aria-describedby="upload-help"
                        AllowedExtensions=".pdf,.doc,.docx"
                        MaxFileSize="10485760">
                <UploaderAsyncSettings SaveUrl="api/upload/save"></UploaderAsyncSettings>
                <UploaderEvents Success="@OnSuccess" OnFailure="@OnFailure"></UploaderEvents>
            </SfUploader>

            <p id="upload-help">@GetText("upload_help")</p>
        </section>

        <!-- Status area -->
        <div role="status" aria-live="polite" class="upload-status">
            @statusMessage
        </div>
    </main>
</div>

<style>
    .accessible-upload-app {
        max-width: 800px;
        margin: 0 auto;
        padding: 20px;
        font-family: Arial, sans-serif;
    }

    .language-controls {
        margin-bottom: 20px;
    }

    .upload-section {
        padding: 20px;
        border: 2px solid #ddd;
        border-radius: 8px;
        margin: 20px 0;
    }

    .upload-status {
        padding: 15px;
        margin-top: 15px;
        border-radius: 4px;
        min-height: 20px;
    }

    .upload-status.success {
        background: #d4edda;
        color: #155724;
    }

    .upload-status.error {
        background: #f8d7da;
        color: #721c24;
    }
</style>

@code {
    SfUploader uploader;
    string currentLang = "en";
    string statusMessage = "";

    private Dictionary<string, Dictionary<string, string>> translations = new()
    {
        {
            "en", new Dictionary<string, string>
            {
                { "upload_heading", "Upload Documents" },
                { "upload_help", "Supported: PDF, DOC, DOCX (max 10MB)" }
            }
        },
        {
            "es", new Dictionary<string, string>
            {
                { "upload_heading", "Cargar Documentos" },
                { "upload_help", "Soportados: PDF, DOC, DOCX (máx 10MB)" }
            }
        },
        {
            "de", new Dictionary<string, string>
            {
                { "upload_heading", "Dokumente Hochladen" },
                { "upload_help", "Unterstützt: PDF, DOC, DOCX (max 10MB)" }
            }
        }
    };

    private string GetText(string key) =>
        translations.ContainsKey(currentLang) &&
        translations[currentLang].ContainsKey(key)
            ? translations[currentLang][key]
            : key;

    private void OnSuccess(SuccessEventArgs args)
    {
        statusMessage = $"✓ {args.File.Name} successfully uploaded";
    }

    private void OnFailure(FailureEventArgs args)
    {
        statusMessage = $"✗ Error uploading {args.File.Name}";
    }
}
```
