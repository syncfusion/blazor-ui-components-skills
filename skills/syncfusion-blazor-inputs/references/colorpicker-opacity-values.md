# ColorPicker - Opacity and Value Formats

This guide covers opacity/alpha channel support, value format handling, and transparent color configuration.

## Overview

The ColorPicker component supports:
- **Opacity control**: Alpha channel for transparency (0-1 or 0-100%)
- **Multiple value formats**: Hex, RGB, RGBA, HSV, HSVA
- **Format preservation**: Maintains input format in output
- **Transparent colors**: Full support for semi-transparent colors

## EnableOpacity Property

Enable alpha channel control with an opacity slider.

### Basic Opacity Support

```razor
<SfColorPicker @bind-Value="@color" 
               EnableOpacity="true">
</SfColorPicker>

@code {
    private string color = "rgba(52, 152, 219, 0.7)";
}
```

**Features:**
- Opacity slider appears in picker
- Value includes alpha channel
- Output format: rgba() or hsva()
- Slider range: 0 (transparent) to 1 (opaque)

### Opacity with Picker Mode

```razor
<SfColorPicker @bind-Value="@overlayColor" 
               Mode="ColorPickerMode.Picker"
               EnableOpacity="true">
</SfColorPicker>

<div class="preview-overlay" style="background-color: @overlayColor; width: 200px; height: 100px;">
    Overlay Preview
</div>

@code {
    private string overlayColor = "rgba(231, 76, 60, 0.5)";
}
```

### Opacity with Palette Mode

```razor
<SfColorPicker @bind-Value="@highlightColor" 
               Mode="ColorPickerMode.Palette"
               EnableOpacity="true">
</SfColorPicker>

@code {
    private string highlightColor = "rgba(46, 204, 113, 0.3)";
}
```

**Note:** Opacity slider appears below palette in popup.

## Value Formats

The ColorPicker supports multiple color value formats.

### Hex Format (Default)

```razor
<SfColorPicker @bind-Value="@hexColor">
</SfColorPicker>

@code {
    private string hexColor = "#3498db"; // 6-digit hex
}
```

**Supported hex formats:**
- `#RGB` - 3-digit shorthand (e.g., `#f00` = red)
- `#RRGGBB` - 6-digit standard (e.g., `#ff0000` = red)
- `#RRGGBBAA` - 8-digit with alpha (e.g., `#ff0000cc` = semi-transparent red)

**Note:** Hex format without `EnableOpacity` doesn't include alpha.

### RGB Format

```razor
<SfColorPicker @bind-Value="@rgbColor">
</SfColorPicker>

@code {
    private string rgbColor = "rgb(52, 152, 219)";
}
```

**Format:** `rgb(red, green, blue)`
- Red: 0-255
- Green: 0-255
- Blue: 0-255

### RGBA Format (with opacity)

```razor
<SfColorPicker @bind-Value="@rgbaColor" 
               EnableOpacity="true">
</SfColorPicker>

@code {
    private string rgbaColor = "rgba(52, 152, 219, 0.8)";
}
```

**Format:** `rgba(red, green, blue, alpha)`
- Red: 0-255
- Green: 0-255
- Blue: 0-255
- Alpha: 0.0-1.0 (decimal)

### HSV Format

```razor
<SfColorPicker @bind-Value="@hsvColor">
</SfColorPicker>

@code {
    private string hsvColor = "hsv(204, 76%, 86%)";
}
```

**Format:** `hsv(hue, saturation, value)`
- Hue: 0-360 degrees
- Saturation: 0-100%
- Value (brightness): 0-100%

### HSVA Format (with opacity)

```razor
<SfColorPicker @bind-Value="@hsvaColor" 
               EnableOpacity="true">
</SfColorPicker>

@code {
    private string hsvaColor = "hsva(204, 76%, 86%, 0.7)";
}
```

**Format:** `hsva(hue, saturation, value, alpha)`
- Hue: 0-360 degrees
- Saturation: 0-100%
- Value: 0-100%
- Alpha: 0.0-1.0

## Reading and Setting Opacity Values

### Extract Alpha from RGBA

```razor
<SfColorPicker @bind-Value="@color" 
               EnableOpacity="true"
               ValueChange="@OnColorChanged">
</SfColorPicker>

<p>Color: @color</p>
<p>Opacity: @opacity%</p>

@code {
    private string color = "rgba(52, 152, 219, 0.7)";
    private double opacity = 70;
    
    private void OnColorChanged(ColorPickerEventArgs args)
    {
        color = args.CurrentValue.Rgba;
        
        // Extract opacity from rgba string
        if (color.StartsWith("rgba"))
        {
            var parts = color.Replace("rgba(", "").Replace(")", "").Split(',');
            if (parts.Length == 4 && double.TryParse(parts[3].Trim(), out double alpha))
            {
                opacity = Math.Round(alpha * 100, 0);
            }
        }
    }
}
```

### Programmatically Set Opacity

```razor
<SfColorPicker @bind-Value="@color" 
               EnableOpacity="true">
</SfColorPicker>

<div class="opacity-controls">
    <button @onclick="() => SetOpacity(1.0)">100% Opaque</button>
    <button @onclick="() => SetOpacity(0.75)">75%</button>
    <button @onclick="() => SetOpacity(0.5)">50%</button>
    <button @onclick="() => SetOpacity(0.25)">25%</button>
    <button @onclick="() => SetOpacity(0.0)">0% (Transparent)</button>
</div>

@code {
    private string color = "rgba(52, 152, 219, 0.8)";
    
    private void SetOpacity(double alpha)
    {
        // Parse current RGB values
        if (color.StartsWith("rgba"))
        {
            var parts = color.Replace("rgba(", "").Replace(")", "").Split(',');
            if (parts.Length >= 3)
            {
                color = $"rgba({parts[0].Trim()}, {parts[1].Trim()}, {parts[2].Trim()}, {alpha})";
            }
        }
    }
}
```

### Dynamic Opacity Slider

```razor
@page "/opacity-demo"
@using Syncfusion.Blazor.Inputs

<h3>Color with Dynamic Opacity</h3>

<div class="controls">
    <label>Base Color:</label>
    <SfColorPicker @bind-Value="@baseColor" 
                   Mode="ColorPickerMode.Palette">
    </SfColorPicker>
    
    <label>Opacity: @opacityLevel%</label>
    <input type="range" min="0" max="100" @bind="opacityLevel" @bind:event="oninput" />
</div>

<div class="preview" style="background-color: @GetColorWithOpacity(); width: 300px; height: 150px; border: 1px solid #ccc;">
    Color Preview
</div>

<p>Resulting Color: @GetColorWithOpacity()</p>

@code {
    private string baseColor = "#3498db";
    private int opacityLevel = 80;
    
    private string GetColorWithOpacity()
    {
        // Convert hex to RGB
        var hex = baseColor.TrimStart('#');
        var r = Convert.ToInt32(hex.Substring(0, 2), 16);
        var g = Convert.ToInt32(hex.Substring(2, 2), 16);
        var b = Convert.ToInt32(hex.Substring(4, 2), 16);
        var alpha = opacityLevel / 100.0;
        
        return $"rgba({r}, {g}, {b}, {alpha})";
    }
}
```

## Transparent Color Handling

### Fully Transparent Colors

```razor
<SfColorPicker @bind-Value="@transparentColor" 
               EnableOpacity="true">
</SfColorPicker>

<div style="background: linear-gradient(45deg, #ccc 25%, transparent 25%, transparent 75%, #ccc 75%, #ccc), 
            linear-gradient(45deg, #ccc 25%, white 25%, white 75%, #ccc 75%, #ccc);
            background-size: 20px 20px;
            background-position: 0 0, 10px 10px;">
    <div style="background-color: @transparentColor; width: 200px; height: 100px;">
        Transparent Preview (Checkered background shows transparency)
    </div>
</div>

@code {
    private string transparentColor = "rgba(52, 152, 219, 0.0)";
}
```

### Gradient with Transparency

```razor
<SfColorPicker @bind-Value="@gradientColor" 
               EnableOpacity="true">
</SfColorPicker>

<div style="background: linear-gradient(to right, transparent, @gradientColor, transparent); 
            width: 300px; height: 100px; border: 1px solid #ccc;">
    Gradient from transparent to color to transparent
</div>

@code {
    private string gradientColor = "rgba(155, 89, 182, 0.9)";
}
```

## Use Cases

### Use Case 1: Overlay Effects

```razor
@page "/overlay-builder"
@using Syncfusion.Blazor.Inputs

<h3>Overlay Color Builder</h3>

<div class="config">
    <label>Overlay Color:</label>
    <SfColorPicker @bind-Value="@overlayColor" 
                   EnableOpacity="true"
                   Mode="ColorPickerMode.Picker">
    </SfColorPicker>
</div>

<div class="demo-container" style="position: relative; width: 400px; height: 300px; background: url('sample-image.jpg') center/cover;">
    <div class="overlay" style="position: absolute; top: 0; left: 0; right: 0; bottom: 0; background-color: @overlayColor;">
        <div style="position: relative; z-index: 1; color: white; padding: 20px;">
            <h2>Overlay Content</h2>
            <p>Text over image with colored overlay</p>
        </div>
    </div>
</div>

@code {
    private string overlayColor = "rgba(0, 0, 0, 0.5)";
}
```

### Use Case 2: Text Highlighter

```razor
@page "/text-highlighter"
@using Syncfusion.Blazor.Inputs

<h3>Text Highlighter</h3>

<div class="controls">
    <label>Highlight Color:</label>
    <SfColorPicker @bind-Value="@highlightColor" 
                   EnableOpacity="true"
                   Mode="ColorPickerMode.Palette"
                   PresetColors="@highlightPalette">
    </SfColorPicker>
</div>

<div class="text-editor">
    <p>
        This is regular text. 
        <span style="background-color: @highlightColor; padding: 2px 4px;">
            This text is highlighted
        </span>
        with the selected color and opacity.
    </p>
</div>

@code {
    private string highlightColor = "rgba(255, 255, 0, 0.4)";
    
    private Dictionary<string, string[]> highlightPalette = new Dictionary<string, string[]>
    {
        { 
            "Highlighters", 
            new string[] 
            { 
                "rgba(255, 255, 0, 0.4)",   // Yellow
                "rgba(0, 255, 0, 0.3)",     // Green
                "rgba(255, 165, 0, 0.4)",   // Orange
                "rgba(255, 105, 180, 0.3)", // Pink
                "rgba(0, 255, 255, 0.3)"    // Cyan
            }
        }
    };
}
```

### Use Case 3: Shadow and Glow Effects

```razor
@page "/shadow-effects"
@using Syncfusion.Blazor.Inputs

<h3>Shadow & Glow Builder</h3>

<div class="controls">
    <label>Shadow Color:</label>
    <SfColorPicker @bind-Value="@shadowColor" 
                   EnableOpacity="true">
    </SfColorPicker>
</div>

<div class="preview-area">
    <div class="box-with-shadow" style="@GetBoxStyle()">
        Element with Shadow
    </div>
</div>

@code {
    private string shadowColor = "rgba(0, 0, 0, 0.3)";
    
    private string GetBoxStyle()
    {
        return $"width: 200px; height: 100px; background: white; " +
               $"box-shadow: 0 4px 8px {shadowColor}; " +
               $"padding: 20px; border-radius: 8px; " +
               $"display: flex; align-items: center; justify-content: center;";
    }
}
```

### Use Case 4: Glass Morphism Effect

```razor
@page "/glassmorphism"
@using Syncfusion.Blazor.Inputs

<h3>Glassmorphism Effect</h3>

<div class="controls">
    <label>Glass Color:</label>
    <SfColorPicker @bind-Value="@glassColor" 
                   EnableOpacity="true"
                   Mode="ColorPickerMode.Picker">
    </SfColorPicker>
</div>

<div class="demo-background" style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 50px;">
    <div class="glass-card" style="@GetGlassStyle()">
        <h4 style="margin: 0 0 10px 0; color: white;">Glass Card</h4>
        <p style="margin: 0; color: rgba(255, 255, 255, 0.9);">
            Glassmorphism effect with customizable background color and opacity
        </p>
    </div>
</div>

@code {
    private string glassColor = "rgba(255, 255, 255, 0.15)";
    
    private string GetGlassStyle()
    {
        return $"background: {glassColor}; " +
               $"backdrop-filter: blur(10px); " +
               $"border: 1px solid rgba(255, 255, 255, 0.18); " +
               $"border-radius: 12px; " +
               $"padding: 30px; " +
               $"box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.37);";
    }
}
```

### Use Case 5: Data Visualization with Transparency

```razor
@page "/chart-transparency"
@using Syncfusion.Blazor.Inputs

<h3>Chart Area Colors with Transparency</h3>

<div class="series-config">
    <div>
        <label>Series 1:</label>
        <SfColorPicker @bind-Value="@series1Color" EnableOpacity="true"></SfColorPicker>
    </div>
    <div>
        <label>Series 2:</label>
        <SfColorPicker @bind-Value="@series2Color" EnableOpacity="true"></SfColorPicker>
    </div>
</div>

<div class="chart-preview" style="position: relative; height: 200px; border: 1px solid #ccc;">
    <div style="position: absolute; bottom: 0; left: 0; width: 80%; height: 60%; background: @series1Color;"></div>
    <div style="position: absolute; bottom: 0; left: 20%; width: 80%; height: 80%; background: @series2Color;"></div>
</div>

<p>Notice how overlapping areas blend when transparency is used</p>

@code {
    private string series1Color = "rgba(52, 152, 219, 0.6)";
    private string series2Color = "rgba(231, 76, 60, 0.6)";
}
```

## Format Conversion Utilities

### Helper Methods for Format Conversion

```razor
@code {
    // Convert hex to rgba
    private string HexToRgba(string hex, double alpha = 1.0)
    {
        hex = hex.TrimStart('#');
        var r = Convert.ToInt32(hex.Substring(0, 2), 16);
        var g = Convert.ToInt32(hex.Substring(2, 2), 16);
        var b = Convert.ToInt32(hex.Substring(4, 2), 16);
        return $"rgba({r}, {g}, {b}, {alpha})";
    }
    
    // Extract alpha from rgba
    private double GetAlphaFromRgba(string rgba)
    {
        if (rgba.StartsWith("rgba"))
        {
            var parts = rgba.Replace("rgba(", "").Replace(")", "").Split(',');
            if (parts.Length == 4 && double.TryParse(parts[3].Trim(), out double alpha))
            {
                return alpha;
            }
        }
        return 1.0;
    }
    
    // Get RGB components
    private (int r, int g, int b) GetRgbFromRgba(string rgba)
    {
        if (rgba.StartsWith("rgba") || rgba.StartsWith("rgb"))
        {
            var parts = rgba.Replace("rgba(", "").Replace("rgb(", "").Replace(")", "").Split(',');
            if (parts.Length >= 3)
            {
                int.TryParse(parts[0].Trim(), out int r);
                int.TryParse(parts[1].Trim(), out int g);
                int.TryParse(parts[2].Trim(), out int b);
                return (r, g, b);
            }
        }
        return (0, 0, 0);
    }
}
```

## Best Practices

1. **Enable opacity when needed**: Only enable for transparency use cases
2. **Provide preview**: Show visual representation of transparent colors
3. **Use rgba for opacity**: RGBA format is most compatible
4. **Test on backgrounds**: Verify transparent colors on various backgrounds
5. **Accessibility**: Ensure sufficient contrast even with transparency
6. **Performance**: Minimize frequent opacity changes in animations
7. **Default values**: Provide sensible opacity defaults (0.7-0.9)
8. **User guidance**: Explain opacity slider purpose
9. **Format consistency**: Stick to one format throughout application
10. **Browser support**: Verify rgba support in target browsers

## Related Topics

- **Modes**: Configuration options → [colorpicker-modes-configuration.md](colorpicker-modes-configuration.md)
- **Events**: Handle value changes → [colorpicker-events-binding.md](colorpicker-events-binding.md)
- **Customization**: Advanced styling → [colorpicker-customization-accessibility.md](colorpicker-customization-accessibility.md)
