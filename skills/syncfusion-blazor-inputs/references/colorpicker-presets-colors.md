# ColorPicker - Preset Colors and Palettes

## Table of Contents
- [Overview](#overview)
- [Default Preset Colors](#default-preset-colors)
- [Custom Preset Colors](#custom-preset-colors)
- [ShowRecentColors](#showrecentcolors)
- [NoColor Option](#nocolor-option)
- [Organizing Color Categories](#organizing-color-categories)
- [Use Cases and Examples](#use-cases-and-examples)

## Overview

The ColorPicker component supports preset color palettes in Palette mode, allowing you to:
- Use default Material Design color palette
- Define custom color sets organized by categories
- Track recently selected colors automatically
- Enable "no color" option for clearing selection

## Default Preset Colors

When using Palette mode without custom configuration, the ColorPicker displays a default set of Material Design colors.

### Basic Palette with Defaults

```razor
<SfColorPicker @bind-Value="@color" 
               Mode="ColorPickerMode.Palette">
</SfColorPicker>

@code {
    private string color = "#e74c3c";
}
```

**Default Palette Includes:**
- Primary Material Design colors
- Standard web colors
- Color variations (shades and tints)
- Recent colors section (if enabled)

## Custom Preset Colors

Define your own color palette using the `PresetColors` property with a dictionary of color groups.

### Basic Custom Palette

```razor
<SfColorPicker @bind-Value="@color" 
               Mode="ColorPickerMode.Palette"
               PresetColors="@customPalette">
</SfColorPicker>

@code {
    private string color = "#e74c3c";
    
    private Dictionary<string, string[]> customPalette = new Dictionary<string, string[]>
    {
        { "Basic", new string[] { "#ffffff", "#000000", "#ff0000", "#00ff00", "#0000ff" } },
        { "Grays", new string[] { "#f8f9fa", "#e9ecef", "#dee2e6", "#adb5bd", "#6c757d" } }
    };
}
```

### Brand Color Palette

```razor
<SfColorPicker @bind-Value="@brandColor" 
               Mode="ColorPickerMode.Palette"
               PresetColors="@brandPalette"
               Columns="6">
</SfColorPicker>

@code {
    private string brandColor = "#e74c3c";
    
    private Dictionary<string, string[]> brandPalette = new Dictionary<string, string[]>
    {
        { 
            "Primary Brand Colors", 
            new string[] { "#e74c3c", "#3498db", "#2ecc71", "#f39c12", "#9b59b6", "#1abc9c" }
        },
        { 
            "Secondary Colors", 
            new string[] { "#ecf0f1", "#95a5a6", "#34495e", "#7f8c8d", "#bdc3c7", "#2c3e50" }
        },
        { 
            "Accent Colors", 
            new string[] { "#e67e22", "#16a085", "#27ae60", "#8e44ad", "#c0392b", "#d35400" }
        }
    };
}
```

### Comprehensive Color System

```razor
@page "/theme-colors"
@using Syncfusion.Blazor.Inputs

<h3>Theme Color System</h3>

<div class="color-selector">
    <label>Select Theme Color:</label>
    <SfColorPicker @bind-Value="@themeColor" 
                   Mode="ColorPickerMode.Palette"
                   PresetColors="@themeColors"
                   Columns="8"
                   ShowRecentColors="true">
    </SfColorPicker>
</div>

<div class="preview-section">
    <h4>Selected Color: @themeColor</h4>
    <div style="background-color: @themeColor; width: 100%; height: 100px; border-radius: 5px;"></div>
</div>

@code {
    private string themeColor = "#3498db";
    
    private Dictionary<string, string[]> themeColors = new Dictionary<string, string[]>
    {
        { 
            "Primary", 
            new string[] 
            { 
                "#e3f2fd", "#bbdefb", "#90caf9", "#64b5f6", 
                "#42a5f5", "#2196f3", "#1e88e5", "#1976d2" 
            }
        },
        { 
            "Success", 
            new string[] 
            { 
                "#e8f5e9", "#c8e6c9", "#a5d6a7", "#81c784", 
                "#66bb6a", "#4caf50", "#43a047", "#388e3c" 
            }
        },
        { 
            "Warning", 
            new string[] 
            { 
                "#fff3e0", "#ffe0b2", "#ffcc80", "#ffb74d", 
                "#ffa726", "#ff9800", "#fb8c00", "#f57c00" 
            }
        },
        { 
            "Danger", 
            new string[] 
            { 
                "#ffebee", "#ffcdd2", "#ef9a9a", "#e57373", 
                "#ef5350", "#f44336", "#e53935", "#d32f2f" 
            }
        },
        { 
            "Neutral", 
            new string[] 
            { 
                "#fafafa", "#f5f5f5", "#eeeeee", "#e0e0e0", 
                "#bdbdbd", "#9e9e9e", "#757575", "#616161" 
            }
        }
    };
}
```

## ShowRecentColors

Automatically track and display recently selected colors for quick reuse.

### Enable Recent Colors (Default)

```razor
<SfColorPicker @bind-Value="@color" 
               Mode="ColorPickerMode.Palette"
               ShowRecentColors="true">
</SfColorPicker>

@code {
    private string color = "#3498db";
}
```

**Features:**
- Tracks last 10-12 selected colors
- Displayed in separate section at top of palette
- Persists during component lifecycle
- Updates automatically on color selection

### Disable Recent Colors

```razor
<SfColorPicker @bind-Value="@color" 
               Mode="ColorPickerMode.Palette"
               ShowRecentColors="false">
</SfColorPicker>

@code {
    private string color = "#e74c3c";
}
```

**When to disable:**
- Limited palette space
- Formal/restricted color selection
- Single-use scenarios
- Brand consistency enforcement

### Recent Colors with Custom Palette

```razor
<SfColorPicker @bind-Value="@selectedColor" 
               Mode="ColorPickerMode.Palette"
               PresetColors="@limitedPalette"
               ShowRecentColors="true"
               Columns="5">
</SfColorPicker>

@code {
    private string selectedColor = "#2ecc71";
    
    private Dictionary<string, string[]> limitedPalette = new Dictionary<string, string[]>
    {
        { 
            "Approved Colors", 
            new string[] 
            { 
                "#e74c3c", "#3498db", "#2ecc71", "#f39c12", "#9b59b6",
                "#1abc9c", "#e67e22", "#34495e", "#95a5a6", "#ecf0f1"
            }
        }
    };
}
```

## NoColor Option

Allow users to select "no color" or clear the current selection.

### Enable NoColor Option

```razor
<SfColorPicker @bind-Value="@color" 
               Mode="ColorPickerMode.Palette"
               NoColor="true">
</SfColorPicker>

@code {
    private string color = "#008000";
}
```

**Features:**
- Displays "No Color" tile in palette
- Clicking sets value to empty string
- Useful for optional color selections
- Allows clearing/resetting color

### NoColor with Custom Palette

```razor
@page "/optional-color"
@using Syncfusion.Blazor.Inputs

<h3>Optional Background Color</h3>

<SfColorPicker @bind-Value="@backgroundColor" 
               Mode="ColorPickerMode.Palette"
               PresetColors="@backgroundPalette"
               NoColor="true"
               ShowButtons="true">
</SfColorPicker>

<div class="preview-box" style="@GetBackgroundStyle()">
    @if (string.IsNullOrEmpty(backgroundColor))
    {
        <p>No background color selected</p>
    }
    else
    {
        <p>Background: @backgroundColor</p>
    }
</div>

@code {
    private string backgroundColor = "";
    
    private Dictionary<string, string[]> backgroundPalette = new Dictionary<string, string[]>
    {
        { 
            "Light Backgrounds", 
            new string[] { "#ffffff", "#f8f9fa", "#e9ecef", "#dee2e6" }
        },
        { 
            "Colored Backgrounds", 
            new string[] { "#e3f2fd", "#f3e5f5", "#e8f5e9", "#fff3e0" }
        }
    };
    
    private string GetBackgroundStyle()
    {
        return string.IsNullOrEmpty(backgroundColor) 
            ? "border: 2px dashed #ccc; padding: 20px; min-height: 100px;" 
            : $"background-color: {backgroundColor}; padding: 20px; min-height: 100px;";
    }
}
```

### Use Cases for NoColor

1. **Optional styling**: Background colors, borders, highlights
2. **Reset functionality**: Allow users to clear selection
3. **Default states**: Indicate no customization applied
4. **Conditional rendering**: Show/hide elements based on color presence
5. **Form fields**: Optional color inputs in forms

## Organizing Color Categories

Structure your color palette into logical categories for better organization and usability.

### UI Component Colors

```razor
<SfColorPicker @bind-Value="@componentColor" 
               Mode="ColorPickerMode.Palette"
               PresetColors="@uiColors"
               Columns="6">
</SfColorPicker>

@code {
    private string componentColor = "#007bff";
    
    private Dictionary<string, string[]> uiColors = new Dictionary<string, string[]>
    {
        { "Bootstrap Primary", new string[] { "#007bff", "#0056b3", "#004085" } },
        { "Bootstrap Success", new string[] { "#28a745", "#1e7e34", "#155724" } },
        { "Bootstrap Danger", new string[] { "#dc3545", "#bd2130", "#a71d2a" } },
        { "Bootstrap Warning", new string[] { "#ffc107", "#e0a800", "#d39e00" } },
        { "Bootstrap Info", new string[] { "#17a2b8", "#117a8b", "#0c5460" } },
        { "Bootstrap Dark", new string[] { "#343a40", "#23272b", "#1d2124" } }
    };
}
```

### Data Visualization Colors

```razor
<SfColorPicker @bind-Value="@chartColor" 
               Mode="ColorPickerMode.Palette"
               PresetColors="@chartColors"
               Columns="8">
</SfColorPicker>

@code {
    private string chartColor = "#3498db";
    
    private Dictionary<string, string[]> chartColors = new Dictionary<string, string[]>
    {
        { 
            "Series 1", 
            new string[] { "#3498db", "#2980b9", "#1f618d", "#154360" }
        },
        { 
            "Series 2", 
            new string[] { "#e74c3c", "#c0392b", "#a93226", "#922b21" }
        },
        { 
            "Series 3", 
            new string[] { "#2ecc71", "#27ae60", "#229954", "#1e8449" }
        },
        { 
            "Series 4", 
            new string[] { "#f39c12", "#e67e22", "#d68910", "#b9770e" }
        }
    };
}
```

### Semantic Color System

```razor
<SfColorPicker @bind-Value="@semanticColor" 
               Mode="ColorPickerMode.Palette"
               PresetColors="@semanticPalette"
               Columns="5">
</SfColorPicker>

@code {
    private string semanticColor = "#2ecc71";
    
    private Dictionary<string, string[]> semanticPalette = new Dictionary<string, string[]>
    {
        { "Status - Success", new string[] { "#d4edda", "#c3e6cb", "#28a745", "#155724", "#0b2e13" } },
        { "Status - Info", new string[] { "#d1ecf1", "#bee5eb", "#17a2b8", "#0c5460", "#062c33" } },
        { "Status - Warning", new string[] { "#fff3cd", "#ffeaa7", "#ffc107", "#856404", "#533f03" } },
        { "Status - Danger", new string[] { "#f8d7da", "#f5c6cb", "#dc3545", "#721c24", "#491217" } }
    };
}
```

## Use Cases and Examples

### Example 1: E-Commerce Product Colors

```razor
@page "/product-colors"
@using Syncfusion.Blazor.Inputs

<h3>Select Product Color</h3>

<SfColorPicker @bind-Value="@productColor" 
               Mode="ColorPickerMode.Palette"
               PresetColors="@productPalette"
               Columns="6"
               ShowRecentColors="false">
</SfColorPicker>

<div class="product-preview">
    <div class="product-image" style="background-color: @productColor;">
        Product Preview
    </div>
    <p>Selected: @GetColorName(productColor)</p>
</div>

@code {
    private string productColor = "#000000";
    
    private Dictionary<string, string[]> productPalette = new Dictionary<string, string[]>
    {
        { 
            "Classic Colors", 
            new string[] { "#000000", "#ffffff", "#808080", "#c0c0c0" }
        },
        { 
            "Popular Colors", 
            new string[] { "#003366", "#8b0000", "#006400", "#4b0082" }
        },
        { 
            "Trendy Colors", 
            new string[] { "#ff69b4", "#00ced1", "#ffd700", "#ff4500" }
        }
    };
    
    private string GetColorName(string hex)
    {
        var colorNames = new Dictionary<string, string>
        {
            { "#000000", "Black" }, { "#ffffff", "White" },
            { "#808080", "Gray" }, { "#c0c0c0", "Silver" },
            { "#003366", "Navy Blue" }, { "#8b0000", "Dark Red" },
            { "#006400", "Forest Green" }, { "#4b0082", "Indigo" },
            { "#ff69b4", "Hot Pink" }, { "#00ced1", "Turquoise" },
            { "#ffd700", "Gold" }, { "#ff4500", "Orange Red" }
        };
        
        return colorNames.ContainsKey(hex.ToLower()) ? colorNames[hex.ToLower()] : hex;
    }
}
```

### Example 2: Dashboard Widget Customization

```razor
@page "/dashboard-widgets"
@using Syncfusion.Blazor.Inputs

<h3>Widget Color Customization</h3>

<div class="widget-config">
    <label>Widget Background:</label>
    <SfColorPicker @bind-Value="@widgetBg" 
                   Mode="ColorPickerMode.Palette"
                   PresetColors="@widgetPalette"
                   NoColor="true"
                   Columns="5">
    </SfColorPicker>
</div>

<div class="widget-preview" style="@GetWidgetStyle()">
    <h4>Dashboard Widget</h4>
    <p>This is a preview of your widget</p>
</div>

@code {
    private string widgetBg = "#f8f9fa";
    
    private Dictionary<string, string[]> widgetPalette = new Dictionary<string, string[]>
    {
        { "Light Themes", new string[] { "#ffffff", "#f8f9fa", "#e9ecef", "#dee2e6", "#ced4da" } },
        { "Dark Themes", new string[] { "#212529", "#343a40", "#495057", "#6c757d", "#adb5bd" } },
        { "Colored Themes", new string[] { "#e3f2fd", "#f3e5f5", "#e8f5e9", "#fff3e0", "#fce4ec" } }
    };
    
    private string GetWidgetStyle()
    {
        if (string.IsNullOrEmpty(widgetBg))
            return "border: 2px dashed #ccc; padding: 30px; min-height: 150px;";
        
        return $"background-color: {widgetBg}; padding: 30px; min-height: 150px; border-radius: 8px; box-shadow: 0 2px 4px rgba(0,0,0,0.1);";
    }
}
```

### Example 3: Text Highlighter

```razor
@page "/text-highlighter"
@using Syncfusion.Blazor.Inputs

<h3>Text Highlighter Tool</h3>

<div class="highlighter-config">
    <label>Highlight Color:</label>
    <SfColorPicker @bind-Value="@highlightColor" 
                   Mode="ColorPickerMode.Palette"
                   PresetColors="@highlighterPalette"
                   ShowRecentColors="true"
                   Columns="6">
    </SfColorPicker>
</div>

<div class="text-content">
    <p>
        Select text and it will be highlighted with the chosen color.
        <span style="background-color: @highlightColor; padding: 2px 4px;">
            This text is highlighted
        </span>
        in the selected color.
    </p>
</div>

@code {
    private string highlightColor = "#ffff00";
    
    private Dictionary<string, string[]> highlighterPalette = new Dictionary<string, string[]>
    {
        { 
            "Standard Highlighters", 
            new string[] { "#ffff00", "#00ff00", "#00ffff", "#ff69b4", "#ffa500", "#ee82ee" }
        },
        { 
            "Pastel Highlighters", 
            new string[] { "#ffffcc", "#ccffcc", "#ccffff", "#ffccff", "#ffddcc", "#e6ccff" }
        }
    };
}
```

## Best Practices

1. **Limit palette size**: 20-30 colors max for quick selection
2. **Logical grouping**: Organize by purpose, not arbitrary categories
3. **Use meaningful names**: Category names should describe color purpose
4. **Consider color blindness**: Include sufficient contrast and variety
5. **Recent colors**: Enable for frequently changing selections
6. **NoColor option**: Include when color is optional
7. **Column count**: Match layout to available space
8. **Brand consistency**: Use PresetColors to enforce brand guidelines
9. **Documentation**: Comment color meanings in code
10. **Testing**: Validate colors work in light and dark themes

## Related Topics

- **Modes**: Picker vs Palette configuration → [colorpicker-modes-configuration.md](colorpicker-modes-configuration.md)
- **Events**: Handle color selection → [colorpicker-events-binding.md](colorpicker-events-binding.md)
- **Customization**: Advanced styling → [colorpicker-customization-accessibility.md](colorpicker-customization-accessibility.md)
