# Signature - Getting Started

Learn how to install, configure, and implement the Syncfusion Blazor Signature component (SfSignature) for capturing handwritten signatures in your Blazor applications.

## Installation

### Install NuGet Package

Add the Syncfusion.Blazor.Inputs package to your Blazor project:

```bash
dotnet add package Syncfusion.Blazor.Inputs
```

Or via Package Manager Console:

```powershell
Install-Package Syncfusion.Blazor.Inputs
```

### Register Syncfusion Services

In `Program.cs`, register the Syncfusion Blazor services:

```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

// Register Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();
```

### Add Namespace Imports

Add the required namespace in `_Imports.razor`:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Inputs
```

## CSS and Script References

### Add Theme Stylesheet

Add the Syncfusion theme CSS in your `App.razor`, `_Layout.cshtml`, or `index.html`:

```html
<head>
    <!-- Syncfusion Blazor Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
```

Available themes:
- `bootstrap5.css` - Bootstrap 5 theme
- `material.css` - Material Design
- `fluent.css` - Fluent UI
- `tailwind.css` - Tailwind CSS

### Add Script Reference

Add the Syncfusion Blazor script at the end of `<body>`:

```html
<body>
    <!-- Your content -->
    
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
</body>
```

## Basic Signature Implementation

### Minimal Example

Create a basic signature pad:

```razor
@page "/signature-demo"
@using Syncfusion.Blazor.Inputs

<h3>Signature Pad</h3>

<SfSignature></SfSignature>
```

### With Clear Button

Add functionality to clear the signature:

```razor
@page "/signature-clear"
@using Syncfusion.Blazor.Inputs

<h3>Sign Below</h3>

<SfSignature @ref="signatureRef"></SfSignature>

<button @onclick="ClearSignature">Clear Signature</button>

@code {
    private SfSignature signatureRef;
    
    private async Task ClearSignature()
    {
        await signatureRef.ClearAsync();
    }
}
```

### Setting Canvas Size

Control the signature pad dimensions with CSS:

```razor
<div class="signature-wrapper">
    <SfSignature></SfSignature>
</div>

<style>
    .signature-wrapper {
        width: 600px;
        height: 300px;
    }
    
    .signature-wrapper canvas {
        border: 1px solid #ccc;
        border-radius: 4px;
    }
</style>
```

## Basic Configuration

### Customizing Stroke Appearance

```razor
<SfSignature StrokeColor="#0000FF"
             BackgroundColor="#F5F5F5"
             MinStrokeWidth="0.5"
             MaxStrokeWidth="2.5">
</SfSignature>
```

### Save and Clear Actions

```razor
@page "/signature-actions"
@using Syncfusion.Blazor.Inputs

<div class="signature-container">
    <label>Your Signature:</label>
    
    <SfSignature @ref="signatureRef"
                 BackgroundColor="#FFFFFF">
    </SfSignature>
    
    <div class="actions">
        <button class="btn btn-primary" @onclick="SaveSignature">Save</button>
        <button class="btn btn-secondary" @onclick="ClearSignature">Clear</button>
    </div>
    
    @if (!string.IsNullOrEmpty(statusMessage))
    {
        <div class="alert alert-info mt-2">@statusMessage</div>
    }
</div>

<style>
    .signature-container {
        max-width: 600px;
        margin: 20px auto;
    }
    
    .signature-container canvas {
        border: 2px solid #007bff;
        border-radius: 4px;
        display: block;
        width: 100%;
    }
    
    .actions {
        margin-top: 10px;
        display: flex;
        gap: 10px;
    }
</style>

@code {
    private SfSignature signatureRef;
    private string statusMessage = "";
    
    private async Task SaveSignature()
    {
        await signatureRef.SaveAsync("signature.png", SignatureFileType.Png);
        statusMessage = "Signature saved as signature.png";
    }
    
    private async Task ClearSignature()
    {
        await signatureRef.ClearAsync();
        statusMessage = "Signature cleared";
    }
}
```

## Touch and Mouse Support

The Signature component automatically supports both touch and mouse input:

### Touch Input (Mobile/Tablets)
- Single finger touch for drawing
- Natural handwriting feel with pressure simulation
- Optimized for mobile browsers

### Mouse Input (Desktop)
- Click and drag to draw
- Smooth line rendering
- Works across all modern browsers

### Example with Device Detection

```razor
<div class="signature-info">
    <p>@deviceInfo</p>
</div>

<SfSignature @ref="signatureRef" Created="@OnCreated"></SfSignature>

@code {
    private SfSignature signatureRef;
    private string deviceInfo = "";
    
    private void OnCreated()
    {
        // Component supports both automatically
        deviceInfo = "Use mouse or touch to sign";
    }
}
```

## Platform-Specific Setup

### Blazor Server

No additional configuration needed after service registration.

### Blazor WebAssembly

Ensure the Syncfusion.Blazor.Inputs package is added to the client project.

### Blazor Web App (.NET 8+)

For interactive components, add render mode:

```razor
@page "/signature"
@rendermode InteractiveServer

<SfSignature></SfSignature>
```

Or use `@rendermode InteractiveWebAssembly` or `@rendermode InteractiveAuto`.

## Complete Working Example

```razor
@page "/signature-complete"
@using Syncfusion.Blazor.Inputs

<div class="container">
    <div class="card">
        <div class="card-header">
            <h3>Digital Signature</h3>
        </div>
        
        <div class="card-body">
            <p class="text-muted">Please sign in the box below:</p>
            
            <div class="signature-box">
                <SfSignature @ref="signatureRef"
                             StrokeColor="#000000"
                             BackgroundColor="#FFFFFF"
                             MinStrokeWidth="0.5"
                             MaxStrokeWidth="2.0"
                             Changed="@OnSignatureChanged">
                </SfSignature>
            </div>
            
            <div class="action-buttons mt-3">
                <button class="btn btn-success" 
                        @onclick="SaveSignature"
                        disabled="@(!hasSignature)">
                    <i class="bi bi-check-circle"></i> Accept & Save
                </button>
                
                <button class="btn btn-outline-secondary" 
                        @onclick="ClearSignature"
                        disabled="@(!hasSignature)">
                    <i class="bi bi-arrow-counterclockwise"></i> Clear
                </button>
            </div>
            
            @if (isSaved)
            {
                <div class="alert alert-success mt-3">
                    <i class="bi bi-check-circle-fill"></i> Signature saved successfully!
                </div>
            }
        </div>
    </div>
</div>

<style>
    .signature-box {
        border: 2px dashed #6c757d;
        border-radius: 8px;
        padding: 10px;
        background-color: #f8f9fa;
    }
    
    .signature-box canvas {
        display: block;
        width: 100%;
        border-radius: 4px;
    }
    
    .action-buttons {
        display: flex;
        gap: 10px;
    }
</style>

@code {
    private SfSignature signatureRef;
    private bool hasSignature = false;
    private bool isSaved = false;
    
    private void OnSignatureChanged(SignatureChangeEventArgs args)
    {
        Console.WriteLine($"Action: {args.ActionName}");
        isSaved = false;
    }
    
    private async Task SaveSignature()
    {
        await signatureRef.SaveAsync();
        isSaved = true;
    }
    
    private async Task ClearSignature()
    {
        await signatureRef.ClearAsync();
        hasSignature = false;
        isSaved = false;
    }
}
```

## Canvas Rendering

The Signature component uses HTML5 Canvas for rendering:

### Canvas Features
- High-quality stroke rendering
- Smooth line interpolation
- Automatic scaling for responsive layouts
- Hardware acceleration support

### Browser Compatibility
- Chrome, Edge, Firefox, Safari (latest versions)
- Mobile browsers (iOS Safari, Chrome Mobile)
- No plugin or Flash required

## Next Steps

- **Drawing Configuration**: Customize stroke width, colors, and background → [signature-drawing-configuration.md](signature-drawing-configuration.md)
- **Save/Load**: Learn to save in different formats and load existing signatures → [signature-save-load.md](signature-save-load.md)
- **Events**: Handle signature changes and validation → [signature-events.md](signature-events.md)
- **Customization**: Apply styling and responsive design → [signature-customization.md](signature-customization.md)

## Troubleshooting

**Signature not rendering:**
- Verify `AddSyncfusionBlazor()` is called in Program.cs
- Check namespace imports in _Imports.razor
- Ensure script reference is at end of `<body>`
- Verify canvas has non-zero dimensions

**Touch input not working:**
- Ensure viewport meta tag is set correctly for mobile
- Check for CSS that might block touch events
- Test on actual device (simulators may not work perfectly)

**Canvas size issues:**
- Set explicit width/height on parent container
- Use CSS to control canvas dimensions
- Ensure responsive layout doesn't collapse canvas
