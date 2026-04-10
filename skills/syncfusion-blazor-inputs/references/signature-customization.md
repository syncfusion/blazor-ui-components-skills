# Signature - Customization and Styling

Learn how to customize the appearance, behavior, and responsiveness of the Syncfusion Blazor Signature component with styling, accessibility features, and device-specific optimizations.

## Disabled State

Prevent any signature input while showing the canvas:

```razor
<SfSignature Disabled="true"></SfSignature>
```

### Conditional Disable

```razor
<SfSignature Disabled="@isProcessing"></SfSignature>

<button @onclick="ProcessSignature" disabled="@isProcessing">
    @(isProcessing ? "Processing..." : "Process Signature")
</button>

@code {
    private bool isProcessing = false;
    
    private async Task ProcessSignature()
    {
        isProcessing = true;
        await Task.Delay(2000); // Simulate processing
        isProcessing = false;
    }
}
```

## IsReadOnly State

Allow viewing but prevent modification of existing signature:

```razor
<SfSignature IsReadOnly="true"></SfSignature>
```

### View Mode Toggle

```razor
<div class="signature-viewer">
    <label>
        <input type="checkbox" @bind="isViewMode" />
        View Only Mode
    </label>
    
    <SfSignature @ref="signatureRef"
                 IsReadOnly="@isViewMode">
    </SfSignature>
</div>

@code {
    private SfSignature signatureRef;
    private bool isViewMode = false;
}
```

**Difference from Disabled:**
- `Disabled`: Component appears grayed out, non-interactive
- `IsReadOnly`: Component looks normal, displays signature but won't accept input

## Canvas Size Customization

### Using CSS

```razor
<div class="signature-canvas-wrapper">
    <SfSignature></SfSignature>
</div>

<style>
    .signature-canvas-wrapper {
        width: 600px;
        height: 300px;
        border: 2px solid #007bff;
        border-radius: 8px;
        padding: 10px;
        background-color: #f8f9fa;
    }
    
    .signature-canvas-wrapper canvas {
        width: 100% !important;
        height: 100% !important;
        display: block;
    }
</style>
```

### Responsive Canvas

```razor
<div class="responsive-signature">
    <SfSignature></SfSignature>
</div>

<style>
    .responsive-signature {
        width: 100%;
        max-width: 800px;
        height: 300px;
        margin: 0 auto;
    }
    
    @media (max-width: 768px) {
        .responsive-signature {
            height: 200px;
        }
    }
    
    .responsive-signature canvas {
        width: 100%;
        height: 100%;
        border: 1px solid #ddd;
        border-radius: 4px;
    }
</style>
```

## HtmlAttributes

Apply custom HTML attributes to the wrapper element:

```razor
<SfSignature HtmlAttributes="@customAttrs"></SfSignature>

@code {
    private Dictionary<string, object> customAttrs = new Dictionary<string, object>
    {
        { "id", "signature-canvas" },
        { "data-testid", "user-signature" },
        { "role", "img" },
        { "aria-label", "Signature input area" }
    };
}
```

## Custom Styling Examples

### Professional Document Signature

```razor
<div class="professional-signature">
    <div class="signature-header">
        <h5>Authorized Signature</h5>
        <p class="text-muted">Sign within the box below</p>
    </div>
    
    <div class="signature-box">
        <SfSignature @ref="signatureRef"
                     StrokeColor="#003366"
                     BackgroundColor="#FFFFFF"
                     MinStrokeWidth="0.5"
                     MaxStrokeWidth="2.0">
        </SfSignature>
    </div>
    
    <div class="signature-footer">
        <small class="text-muted">
            By signing, you agree to the terms and conditions
        </small>
    </div>
</div>

<style>
    .professional-signature {
        max-width: 600px;
        margin: 20px auto;
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }
    
    .signature-header {
        margin-bottom: 15px;
    }
    
    .signature-header h5 {
        margin: 0;
        color: #333;
        font-weight: 600;
    }
    
    .signature-box {
        border: 2px solid #003366;
        border-radius: 8px;
        padding: 15px;
        background: linear-gradient(to bottom, #ffffff 0%, #f8f9fa 100%);
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    }
    
    .signature-box canvas {
        display: block;
        width: 100%;
        background-color: white;
        border-radius: 4px;
    }
    
    .signature-footer {
        margin-top: 10px;
        text-align: center;
    }
</style>

@code {
    private SfSignature signatureRef;
}
```

### Digital Pad Style

```razor
<div class="digital-pad">
    <div class="pad-screen">
        <SfSignature StrokeColor="#00FF00"
                     BackgroundColor="#000000"
                     MinStrokeWidth="1.0"
                     MaxStrokeWidth="3.0">
        </SfSignature>
    </div>
    <div class="pad-controls">
        <button @onclick="ClearSignature">Clear</button>
        <button @onclick="SaveSignature">Accept</button>
    </div>
</div>

<style>
    .digital-pad {
        width: 400px;
        background: linear-gradient(145deg, #2a2a2a, #1a1a1a);
        border-radius: 20px;
        padding: 20px;
        box-shadow: 0 10px 30px rgba(0, 0, 0, 0.5);
    }
    
    .pad-screen {
        border: 4px solid #333;
        border-radius: 10px;
        background-color: #000;
        padding: 10px;
        box-shadow: inset 0 2px 10px rgba(0, 0, 0, 0.8);
    }
    
    .pad-screen canvas {
        display: block;
        width: 100%;
        background-color: #000;
    }
    
    .pad-controls {
        display: flex;
        justify-content: space-around;
        margin-top: 15px;
        gap: 10px;
    }
    
    .pad-controls button {
        flex: 1;
        padding: 12px;
        background: linear-gradient(145deg, #3a3a3a, #2a2a2a);
        border: 2px solid #444;
        border-radius: 8px;
        color: #fff;
        font-weight: bold;
        cursor: pointer;
        transition: all 0.3s;
    }
    
    .pad-controls button:hover {
        background: linear-gradient(145deg, #4a4a4a, #3a3a3a);
        transform: translateY(-2px);
    }
</style>

@code {
    private async Task ClearSignature() { }
    private async Task SaveSignature() { }
}
```

### Tablet/Mobile Optimized

```razor
<div class="mobile-signature">
    <div class="instruction-banner">
        <i class="bi bi-pencil-square"></i>
        <span>Sign with your finger or stylus</span>
    </div>
    
    <SfSignature @ref="signatureRef"
                 StrokeColor="#000000"
                 BackgroundColor="#FFFEF7"
                 MinStrokeWidth="2.5"
                 MaxStrokeWidth="5.0"
                 Velocity="0.8">
    </SfSignature>
    
    <div class="mobile-actions">
        <button class="btn-clear" @onclick="Clear">
            <i class="bi bi-arrow-counterclockwise"></i> Clear
        </button>
        <button class="btn-done" @onclick="Done">
            <i class="bi bi-check-lg"></i> Done
        </button>
    </div>
</div>

<style>
    .mobile-signature {
        width: 100%;
        height: 100vh;
        display: flex;
        flex-direction: column;
        background-color: #f5f5f5;
    }
    
    .instruction-banner {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        padding: 20px;
        text-align: center;
        font-size: 18px;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 10px;
    }
    
    .mobile-signature canvas {
        flex: 1;
        width: 100% !important;
        height: auto !important;
        touch-action: none; /* Prevent scrolling while signing */
    }
    
    .mobile-actions {
        display: flex;
        padding: 15px;
        gap: 10px;
        background: white;
        box-shadow: 0 -2px 10px rgba(0, 0, 0, 0.1);
    }
    
    .mobile-actions button {
        flex: 1;
        padding: 18px;
        border: none;
        border-radius: 8px;
        font-size: 16px;
        font-weight: 600;
        cursor: pointer;
        display: flex;
        align-items: center;
        justify-content: center;
        gap: 8px;
    }
    
    .btn-clear {
        background-color: #6c757d;
        color: white;
    }
    
    .btn-done {
        background-color: #28a745;
        color: white;
    }
</style>

@code {
    private SfSignature signatureRef;
    
    private async Task Clear() 
    {
        await signatureRef.ClearAsync();
    }
    
    private async Task Done() 
    {
        await signatureRef.SaveAsync(SignatureFileType.Png, "signature.png");
    }
}
```

## Theme Integration

### Bootstrap Theme

```razor
<div class="card">
    <div class="card-header bg-primary text-white">
        <h5 class="mb-0">Signature Required</h5>
    </div>
    <div class="card-body">
        <SfSignature></SfSignature>
    </div>
    <div class="card-footer">
        <button class="btn btn-primary">Submit</button>
        <button class="btn btn-secondary">Clear</button>
    </div>
</div>
```

### Material Design Style

```razor
<div class="material-signature-card">
    <div class="mdc-card">
        <div class="signature-area">
            <SfSignature StrokeColor="#6200EE"
                         BackgroundColor="#FFFFFF">
            </SfSignature>
        </div>
        <div class="mdc-card-actions">
            <button class="mdc-button mdc-button--raised">
                <span class="mdc-button__label">Save</span>
            </button>
        </div>
    </div>
</div>

<style>
    .material-signature-card {
        max-width: 600px;
        margin: 20px auto;
    }
    
    .mdc-card {
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.12), 0 2px 8px rgba(0,0,0,0.08);
        background: white;
        overflow: hidden;
    }
    
    .signature-area {
        padding: 24px;
        min-height: 250px;
    }
    
    .mdc-card-actions {
        padding: 16px;
        display: flex;
        justify-content: flex-end;
        border-top: 1px solid rgba(0,0,0,0.12);
    }
    
    .mdc-button--raised {
        background-color: #6200EE;
        color: white;
        padding: 10px 24px;
        border: none;
        border-radius: 4px;
        cursor: pointer;
        font-weight: 500;
        text-transform: uppercase;
        letter-spacing: 0.0892857143em;
    }
</style>
```

## Accessibility Features

### ARIA Attributes

```razor
<div role="region" aria-labelledby="signature-heading">
    <h3 id="signature-heading">Signature Input</h3>
    
    <SfSignature HtmlAttributes="@ariaAttrs"></SfSignature>
    
    <div id="signature-instructions" class="sr-only">
        Use mouse, touch, or stylus to draw your signature
    </div>
</div>

@code {
    private Dictionary<string, object> ariaAttrs = new Dictionary<string, object>
    {
        { "role", "img" },
        { "aria-label", "Signature canvas" },
        { "aria-describedby", "signature-instructions" },
        { "aria-required", "true" }
    };
}
```

### Keyboard Accessibility

Provide keyboard alternatives for touch-only actions:

```razor
<div class="accessible-signature">
    <SfSignature @ref="signatureRef"></SfSignature>
    
    <div class="keyboard-controls" role="toolbar" aria-label="Signature controls">
        <button @onclick="ClearSignature" 
                aria-label="Clear signature"
                tabindex="0">
            Clear (Ctrl+Z)
        </button>
        
        <button @onclick="SaveSignature" 
                aria-label="Save signature"
                tabindex="0">
            Save (Ctrl+S)
        </button>
    </div>
</div>

@code {
    private SfSignature signatureRef;
    
    private async Task ClearSignature() 
    {
        await signatureRef.ClearAsync();
    }
    
    private async Task SaveSignature() 
    {
        await signatureRef.SaveAsync(SignatureFileType.Png, "signature.png");
    }
}
```

## Device-Specific Optimizations

### Touch Device Detection

```razor
@inject IJSRuntime JS

<SfSignature MinStrokeWidth="@minStroke"
             MaxStrokeWidth="@maxStroke"
             Velocity="@velocity">
</SfSignature>

@code {
    private double minStroke = 0.5;
    private double maxStroke = 2.0;
    private double velocity = 0.7;
    
    protected override async Task OnInitializedAsync()
    {
        bool isTouchDevice = await JS.InvokeAsync<bool>(
            "eval", 
            "'ontouchstart' in window || navigator.maxTouchPoints > 0"
        );
        
        if (isTouchDevice)
        {
            // Optimize for touch
            minStroke = 2.0;
            maxStroke = 5.0;
            velocity = 0.8;
        }
    }
}
```

### Stylus Support

```razor
<div class="stylus-optimized">
    <p class="instruction">
        <i class="bi bi-pen"></i>
        @(hasStylus ? "Stylus detected - precision mode" : "Touch or mouse mode")
    </p>
    
    <SfSignature MinStrokeWidth="@(hasStylus ? 0.3 : 1.5)"
                 MaxStrokeWidth="@(hasStylus ? 1.5 : 4.0)">
    </SfSignature>
</div>

@code {
    private bool hasStylus = false;
    
    protected override async Task OnInitializedAsync()
    {
        // Detect stylus support (simplified)
        hasStylus = await JS.InvokeAsync<bool>(
            "eval",
            "window.PointerEvent !== undefined"
        );
    }
}
```

## Responsive Design Patterns

### Adaptive Layout

```razor
<div class="adaptive-signature">
    <SfSignature></SfSignature>
</div>

<style>
    .adaptive-signature {
        width: 100%;
        height: 250px;
        padding: 10px;
    }
    
    /* Tablet */
    @media (min-width: 768px) and (max-width: 1024px) {
        .adaptive-signature {
            height: 300px;
            max-width: 700px;
            margin: 0 auto;
        }
    }
    
    /* Desktop */
    @media (min-width: 1025px) {
        .adaptive-signature {
            height: 350px;
            max-width: 800px;
            margin: 0 auto;
        }
    }
    
    /* Mobile landscape */
    @media (max-width: 767px) and (orientation: landscape) {
        .adaptive-signature {
            height: 180px;
        }
    }
</style>
```

## Best Practices

1. **Canvas Size**: Provide adequate space (min 300x200px) for comfortable signing
2. **Touch Targets**: Make buttons at least 44x44px for touch devices
3. **Stroke Width**: Increase for touch devices (2.0-5.0), decrease for mouse (0.5-2.0)
4. **Contrast**: Ensure sufficient contrast between stroke and background (WCAG AA: 4.5:1)
5. **Instructions**: Provide clear visual instructions for first-time users
6. **Feedback**: Show immediate visual feedback for all interactions
7. **Responsive**: Test on multiple devices and orientations
8. **Accessibility**: Include ARIA labels and keyboard alternatives
9. **Performance**: Optimize canvas size for target devices
10. **State Management**: Clearly indicate disabled/readonly states

## Troubleshooting

**Canvas not sized correctly:**
- Set explicit width/height on parent container
- Use `!important` on canvas CSS if needed
- Check for conflicting CSS rules

**Touch not working on mobile:**
- Add `touch-action: none` to prevent scroll interference
- Ensure viewport meta tag is set correctly
- Test on actual device (not just simulators)

**Signature appears pixelated:**
- Canvas auto-scales based on device pixel ratio
- Ensure parent container has adequate dimensions
- Avoid excessive CSS transformations
