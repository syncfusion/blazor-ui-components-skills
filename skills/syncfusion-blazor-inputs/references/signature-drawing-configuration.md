# Signature - Drawing Configuration

Comprehensive guide to configuring stroke appearance, colors, backgrounds, and drawing behavior of the Syncfusion Blazor Signature component.

## Stroke Width Configuration

### MinStrokeWidth Property

Set the minimum pen thickness:

```razor
<!-- Thin, precise lines -->
<SfSignature MinStrokeWidth="0.3"></SfSignature>

<!-- Standard thin lines -->
<SfSignature MinStrokeWidth="0.5"></SfSignature>

<!-- Thicker minimum for bold signatures -->
<SfSignature MinStrokeWidth="1.0"></SfSignature>
```

**Default:** `0.5`  
**Range:** `0.1` - `10.0`  
**Use Cases:**
- Fine signatures: `0.3` - `0.5`
- Standard signatures: `0.5` - `1.0`
- Bold signatures: `1.0` - `2.0`

### MaxStrokeWidth Property

Set the maximum pen thickness:

```razor
<!-- Subtle variation -->
<SfSignature MaxStrokeWidth="1.5"></SfSignature>

<!-- Standard variation -->
<SfSignature MaxStrokeWidth="2.0"></SfSignature>

<!-- High variation for expressive signatures -->
<SfSignature MaxStrokeWidth="3.5"></SfSignature>
```

**Default:** `2.0`  
**Range:** `0.1` - `10.0`  
**Use Cases:**
- Consistent thickness: Set min ≈ max
- Natural variation: Max = Min × 3-4
- Touch-friendly: `3.0` - `5.0`

### Combined Stroke Width Configuration

```razor
<!-- Consistent thickness (pen-like) -->
<SfSignature MinStrokeWidth="1.0"
             MaxStrokeWidth="1.2">
</SfSignature>

<!-- Natural handwriting feel -->
<SfSignature MinStrokeWidth="0.5"
             MaxStrokeWidth="2.0">
</SfSignature>

<!-- Bold, expressive signatures -->
<SfSignature MinStrokeWidth="1.5"
             MaxStrokeWidth="4.0">
</SfSignature>

<!-- Mobile/touch optimized -->
<SfSignature MinStrokeWidth="2.0"
             MaxStrokeWidth="5.0">
</SfSignature>
```

### Stroke Width Comparison

```razor
<div class="row">
    <div class="col-md-4">
        <h5>Fine (0.3 - 1.0)</h5>
        <SfSignature MinStrokeWidth="0.3" MaxStrokeWidth="1.0"></SfSignature>
    </div>
    
    <div class="col-md-4">
        <h5>Standard (0.5 - 2.0)</h5>
        <SfSignature MinStrokeWidth="0.5" MaxStrokeWidth="2.0"></SfSignature>
    </div>
    
    <div class="col-md-4">
        <h5>Bold (1.0 - 4.0)</h5>
        <SfSignature MinStrokeWidth="1.0" MaxStrokeWidth="4.0"></SfSignature>
    </div>
</div>
```

## Stroke Color

### StrokeColor Property

Customize the ink color:

```razor
<!-- Black ink (default) -->
<SfSignature StrokeColor="#000000"></SfSignature>

<!-- Blue ink -->
<SfSignature StrokeColor="#0000FF"></SfSignature>

<!-- Dark blue for professional look -->
<SfSignature StrokeColor="#003366"></SfSignature>

<!-- RGB format -->
<SfSignature StrokeColor="rgb(0, 51, 102)"></SfSignature>

<!-- RGBA with transparency -->
<SfSignature StrokeColor="rgba(0, 0, 0, 0.8)"></SfSignature>
```

**Default:** `"#000000"` (black)  
**Formats:** Hex (#RRGGBB), RGB, RGBA, named colors

### Color Selector Implementation

```razor
<div class="signature-with-colors">
    <div class="color-selector">
        <label>Ink Color:</label>
        <button @onclick="@(() => SetColor("#000000"))" class="color-btn black"></button>
        <button @onclick="@(() => SetColor("#0000FF"))" class="color-btn blue"></button>
        <button @onclick="@(() => SetColor("#FF0000"))" class="color-btn red"></button>
        <button @onclick="@(() => SetColor("#008000"))" class="color-btn green"></button>
    </div>
    
    <SfSignature @ref="signatureRef"
                 StrokeColor="@currentColor">
    </SfSignature>
</div>

<style>
    .color-selector {
        margin-bottom: 10px;
        display: flex;
        align-items: center;
        gap: 8px;
    }
    
    .color-btn {
        width: 30px;
        height: 30px;
        border: 2px solid #ccc;
        border-radius: 4px;
        cursor: pointer;
    }
    
    .color-btn.black { background: #000000; }
    .color-btn.blue { background: #0000FF; }
    .color-btn.red { background: #FF0000; }
    .color-btn.green { background: #008000; }
    
    .color-btn:hover {
        border-color: #007bff;
    }
</style>

@code {
    private SfSignature signatureRef;
    private string currentColor = "#000000";
    
    private async Task SetColor(string color)
    {
        currentColor = color;
        await signatureRef.ClearAsync(); // Clear when changing color
    }
}
```

## Background Configuration

### BackgroundColor Property

Set the canvas background color:

```razor
<!-- White background (default) -->
<SfSignature BackgroundColor="#FFFFFF"></SfSignature>

<!-- Light gray for better contrast -->
<SfSignature BackgroundColor="#F5F5F5"></SfSignature>

<!-- Beige/cream for document feel -->
<SfSignature BackgroundColor="#FFF8DC"></SfSignature>

<!-- Transparent background -->
<SfSignature BackgroundColor="transparent"></SfSignature>
```

**Default:** `"#FFFFFF"`  
**Use Cases:**
- White: Standard documents
- Light gray: Better contrast on white pages
- Transparent: Overlay on existing images
- Custom colors: Branding requirements

### BackgroundImage Property

Add a background image (letterhead, watermark, form):

```razor
<!-- Company letterhead -->
<SfSignature BackgroundImage="images/letterhead.png"
             StrokeColor="#000080">
</SfSignature>

<!-- Form template -->
<SfSignature BackgroundImage="data:image/png;base64,iVBORw0KG..."
             BackgroundColor="transparent">
</SfSignature>

<!-- Watermark -->
<SfSignature BackgroundImage="images/watermark.png">
</SfSignature>
```

**Formats:** URL, Data URI (Base64)  
**Image Types:** PNG, JPEG, SVG

### Background Image Example

```razor
@page "/signature-letterhead"
@using Syncfusion.Blazor.Inputs

<div class="letterhead-signature">
    <h3>Sign on Letterhead</h3>
    
    <SfSignature @ref="signatureRef"
                 BackgroundImage="images/company-letterhead.png"
                 BackgroundColor="#FFFFFF"
                 StrokeColor="#003366"
                 MinStrokeWidth="0.8"
                 MaxStrokeWidth="2.5"
                 SaveWithBackground="true">
    </SfSignature>
    
    <div class="actions mt-3">
        <button class="btn btn-primary" @onclick="SaveWithLetterhead">
            Save Signed Document
        </button>
        <button class="btn btn-secondary" @onclick="ClearSignature">
            Clear
        </button>
    </div>
</div>

@code {
    private SfSignature signatureRef;
    
    private async Task SaveWithLetterhead()
    {
        // Background will be included in saved file
        await signatureRef.SaveAsync(SignatureFileType.Png, "signed-document.png");
    }
    
    private async Task ClearSignature()
    {
        await signatureRef.ClearAsync();
    }
}
```

## Velocity Property

Control stroke smoothness and responsiveness:

```razor
<!-- Low velocity - slower, smoother lines -->
<SfSignature Velocity="0.3"></SfSignature>

<!-- Standard velocity -->
<SfSignature Velocity="0.7"></SfSignature>

<!-- High velocity - faster, more responsive -->
<SfSignature Velocity="1.0"></SfSignature>
```

**Default:** `0.7`  
**Range:** `0.1` - `1.0`

**Effects:**
- **Low (0.1 - 0.4):** Smoother curves, less responsive, good for slow signatures
- **Medium (0.5 - 0.7):** Balanced smoothness and responsiveness
- **High (0.8 - 1.0):** Very responsive, follows input closely, may be less smooth

### Velocity Comparison

```razor
<div class="velocity-comparison">
    <div>
        <label>Smooth (0.3)</label>
        <SfSignature Velocity="0.3"></SfSignature>
    </div>
    
    <div>
        <label>Balanced (0.7)</label>
        <SfSignature Velocity="0.7"></SfSignature>
    </div>
    
    <div>
        <label>Responsive (1.0)</label>
        <SfSignature Velocity="1.0"></SfSignature>
    </div>
</div>
```

## Combined Configuration Examples

### Professional Signature Pad

```razor
<SfSignature StrokeColor="#003366"
             BackgroundColor="#FFFFFF"
             MinStrokeWidth="0.5"
             MaxStrokeWidth="2.0"
             Velocity="0.7">
</SfSignature>
```

**Best For:** Business documents, contracts, formal agreements

### Touch-Optimized Signature

```razor
<SfSignature StrokeColor="#000000"
             BackgroundColor="#F5F5F5"
             MinStrokeWidth="2.0"
             MaxStrokeWidth="4.5"
             Velocity="0.8">
</SfSignature>
```

**Best For:** Mobile devices, tablets, touch screens

### Fine Detail Signature

```razor
<SfSignature StrokeColor="#000000"
             BackgroundColor="#FFFFFF"
             MinStrokeWidth="0.3"
             MaxStrokeWidth="1.0"
             Velocity="0.5">
</SfSignature>
```

**Best For:** Detailed signatures, mouse input, precise control

### Letterhead Document Signature

```razor
<SfSignature BackgroundImage="images/letterhead.png"
             StrokeColor="#000080"
             MinStrokeWidth="0.8"
             MaxStrokeWidth="2.5"
             Velocity="0.7"
             SaveWithBackground="true">
</SfSignature>
```

**Best For:** Official documents, contracts with letterhead

## Dynamic Configuration

### User-Adjustable Settings

```razor
<div class="signature-config">
    <div class="settings">
        <div class="setting-group">
            <label>Stroke Width: @strokeWidth</label>
            <input type="range" min="0.5" max="5.0" step="0.5" 
                   @bind="strokeWidth" @bind:event="oninput" />
        </div>
        
        <div class="setting-group">
            <label>Velocity: @velocity.ToString("F1")</label>
            <input type="range" min="0.1" max="1.0" step="0.1" 
                   @bind="velocity" @bind:event="oninput" />
        </div>
        
        <div class="setting-group">
            <label>Ink Color:</label>
            <input type="color" @bind="inkColor" @bind:event="oninput" />
        </div>
    </div>
    
    <SfSignature StrokeColor="@inkColor"
                 MinStrokeWidth="@(strokeWidth * 0.5)"
                 MaxStrokeWidth="@strokeWidth"
                 Velocity="@velocity">
    </SfSignature>
</div>

@code {
    private double strokeWidth = 2.0;
    private double velocity = 0.7;
    private string inkColor = "#000000";
}
```

### Device-Specific Configuration

```razor
<SfSignature MinStrokeWidth="@GetMinStrokeWidth()"
             MaxStrokeWidth="@GetMaxStrokeWidth()"
             Velocity="@GetVelocity()">
</SfSignature>

@code {
    [Inject] private IJSRuntime JS { get; set; }
    
    private bool isMobile = false;
    
    protected override async Task OnInitializedAsync()
    {
        // Detect mobile device (simplified)
        isMobile = await JS.InvokeAsync<bool>("isMobileDevice");
    }
    
    private double GetMinStrokeWidth() => isMobile ? 2.0 : 0.5;
    private double GetMaxStrokeWidth() => isMobile ? 5.0 : 2.0;
    private double GetVelocity() => isMobile ? 0.8 : 0.7;
}
```

## Best Practices

1. **Stroke Width**: Use larger strokes (2.0+) for touch devices
2. **Color Contrast**: Ensure stroke color contrasts well with background
3. **Velocity**: Start with default (0.7), adjust based on user feedback
4. **Background Images**: Optimize size (< 1MB) for better performance
5. **Touch Optimization**: Increase min/max stroke width by 2-3x for mobile
6. **Consistency**: Keep settings consistent across signature fields in the same form
7. **Testing**: Test on actual devices (mouse, touch, stylus) before deployment

## Performance Considerations

- Keep background images under 1MB
- Use compressed formats (JPEG for photos, PNG for graphics)
- Avoid complex background images that may slow rendering
- Test stroke smoothness on target devices
