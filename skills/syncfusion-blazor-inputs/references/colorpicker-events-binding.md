# ColorPicker - Events and Data Binding

## Table of Contents
- [Overview](#overview)
- [Value Binding](#value-binding)
- [ValueChange Event](#valuechange-event)
- [Selected Event](#selected-event)
- [OnOpen and OnClose Events](#onopen-and-onclose-events)
- [ModeSwitched Event](#modeswitched-event)
- [OnTileRender Event](#ontilerender-event)
- [Form Integration](#form-integration)
- [Real-Time Preview Patterns](#real-time-preview-patterns)

## Overview

The ColorPicker component provides comprehensive event handling for:
- Value changes and selection confirmation
- Popup open/close events
- Mode switching tracking
- Custom palette tile rendering
- Form validation integration
- Real-time color preview

## Value Binding

### Two-Way Binding

```razor
<SfColorPicker @bind-Value="@selectedColor">
</SfColorPicker>

<p>Selected Color: @selectedColor</p>

@code {
    private string selectedColor = "#3498db";
}
```

### One-Way Binding with Event

```razor
<SfColorPicker Value="@currentColor" 
               ValueChange="@OnColorChanged">
</SfColorPicker>

<p>Current Color: @currentColor</p>

@code {
    private string currentColor = "#e74c3c";
    
    private void OnColorChanged(ColorPickerEventArgs args)
    {
        currentColor = args.CurrentValue.Hex;
        Console.WriteLine($"Color changed to: {currentColor}");
    }
}
```

### Binding to Object Property

```razor
<SfColorPicker @bind-Value="@theme.PrimaryColor">
</SfColorPicker>

<p>Theme Primary Color: @theme.PrimaryColor</p>

@code {
    private ThemeSettings theme = new ThemeSettings { PrimaryColor = "#3498db" };
    
    public class ThemeSettings
    {
        public string PrimaryColor { get; set; }
        public string SecondaryColor { get; set; }
    }
}
```

## ValueChange Event

Fires when the color value changes (immediate on selection or on Apply button click if ShowButtons="true").

### Basic ValueChange Handler

```razor
<SfColorPicker @bind-Value="@color" 
               ValueChange="@OnValueChanged">
</SfColorPicker>

<div class="log">
    <p>Last changed: @lastChanged</p>
</div>

@code {
    private string color = "#008000";
    private string lastChanged = "Not yet";
    
    private void OnValueChanged(ColorPickerEventArgs args)
    {
        lastChanged = DateTime.Now.ToString("HH:mm:ss");
        Console.WriteLine($"New color: {args.CurrentValue.Hex}");
    }
}
```

### ColorPickerEventArgs Properties

```razor
<SfColorPicker @bind-Value="@color" 
               ValueChange="@OnColorChange"
               EnableOpacity="true">
</SfColorPicker>

<div class="color-info">
    <p>Hex: @colorInfo.Hex</p>
    <p>RGBA: @colorInfo.Rgba</p>
</div>

@code {
    private string color = "rgba(52, 152, 219, 0.8)";
    private ColorInfo colorInfo = new ColorInfo();
    
    private void OnColorChange(ColorPickerEventArgs args)
    {
        colorInfo.Hex = args.CurrentValue.Hex;
        colorInfo.Rgba = args.CurrentValue.Rgba;
    }
    
    private class ColorInfo
    {
        public string Hex { get; set; }
        public string Rgba { get; set; }
    }
}
```

### Async ValueChanged Handler

```razor
<SfColorPicker @bind-Value="@themeColor" 
               ValueChange="@OnThemeColorChanged">
</SfColorPicker>

<p>Status: @saveStatus</p>

@code {
    private string themeColor = "#2ecc71";
    private string saveStatus = "Ready";
    
    private async Task OnThemeColorChanged(ColorPickerEventArgs args)
    {
        saveStatus = "Saving...";
        
        // Save to database or API
        await SaveThemeColorAsync(args.CurrentValue.Hex);
        
        saveStatus = "Saved!";
        
        // Reset status after delay
        await Task.Delay(2000);
        saveStatus = "Ready";
    }
    
    private async Task SaveThemeColorAsync(string color)
    {
        // Simulate API call
        await Task.Delay(500);
        Console.WriteLine($"Saved color: {color}");
    }
}
```

## Selected Event

Fires when user confirms selection (with Apply button or immediate selection).

### Basic Selected Handler

```razor
<SfColorPicker @bind-Value="@color" 
               ShowButtons="true"
               Selected="@OnColorSelected">
</SfColorPicker>

<p>Confirmed Color: @confirmedColor</p>

@code {
    private string color = "#9b59b6";
    private string confirmedColor = "None";
    
    private void OnColorSelected(ColorPickerEventArgs args)
    {
        confirmedColor = args.CurrentValue.Hex;
        Console.WriteLine($"User confirmed: {confirmedColor}");
    }
}
```

### Selected vs ValueChange

```razor
<SfColorPicker @bind-Value="@color" 
               ShowButtons="true"
               ValueChange="@OnValueChanged"
               Selected="@OnSelected">
</SfColorPicker>

<div class="event-log">
    <p>ValueChanged: @valueChangeCount times</p>
    <p>Selected: @selectedCount times</p>
</div>

@code {
    private string color = "#f39c12";
    private int valueChangeCount = 0;
    private int selectedCount = 0;
    
    private void OnValueChanged(ColorPickerEventArgs args)
    {
        valueChangeCount++;
        Console.WriteLine("ValueChanged fired");
    }
    
    private void OnSelected(ColorPickerEventArgs args)
    {
        selectedCount++;
        Console.WriteLine("Selected fired - User clicked Apply");
    }
}
```

**Difference:**
- **ValueChange**: Fires on every color change (including preview)
- **Selected**: Fires only on Apply button click (if ShowButtons="true") or final selection

## OnOpen and OnClose Events

Track popup open and close events.

### Open Event

```razor
<SfColorPicker @bind-Value="@color" 
               OnOpen="@OnPickerOpen">
</SfColorPicker>

<p>Opened: @openCount times</p>

@code {
    private string color = "#1abc9c";
    private int openCount = 0;
    
    private void OnPickerOpen(BeforeOpenCloseEventArgs args)
    {
        openCount++;
        Console.WriteLine("ColorPicker opened");
        
        // Can cancel opening
        // args.Cancel = true;
    }
}
```

### Close Event

```razor
<SfColorPicker @bind-Value="@color" 
               OnClose="@OnPickerClose">
</SfColorPicker>

<p>Closed: @closeCount times</p>

@code {
    private string color = "#e67e22";
    private int closeCount = 0;
    
    private void OnPickerClose(BeforeOpenCloseEventArgs args)
    {
        closeCount++;
        Console.WriteLine("ColorPicker closed");
        
        // Can cancel closing
        // args.Cancel = true;
    }
}
```

### Conditional Open/Close

```razor
<SfColorPicker @bind-Value="@color" 
               OnOpen="@OnBeforeOpen"
               OnClose="@OnBeforeClose">
</SfColorPicker>

<button @onclick="() => allowInteraction = !allowInteraction">
    @(allowInteraction ? "Lock" : "Unlock") Picker
</button>

@code {
    private string color = "#16a085";
    private bool allowInteraction = true;
    
    private void OnBeforeOpen(BeforeOpenCloseEventArgs args)
    {
        if (!allowInteraction)
        {
            args.Cancel = true;
            Console.WriteLine("Opening prevented - Picker locked");
        }
    }
    
    private void OnBeforeClose(BeforeOpenCloseEventArgs args)
    {
        // Could validate selection before closing
        if (string.IsNullOrEmpty(color))
        {
            args.Cancel = true;
            Console.WriteLine("Closing prevented - No color selected");
        }
    }
}
```

### Opened Event (After Opening)

```razor
<SfColorPicker @bind-Value="@color" 
               Opened="@OnPickerOpened">
</SfColorPicker>

@code {
    private string color = "#c0392b";
    
    private void OnPickerOpened(OpenEventArgs args)
    {
        Console.WriteLine("ColorPicker has opened (after animation)");
        // Can perform actions after popup is fully displayed
    }
}
```

### PopupClosed Event

```razor
<SfColorPicker @bind-Value="@color" 
               PopupClosed="@OnPopupClosed">
</SfColorPicker>

@code {
    private string color = "#8e44ad";
    
    private void OnPopupClosed(Object args)
    {
        Console.WriteLine("Popup has closed completely");
        // Cleanup or logging after popup closes
    }
}
```

## ModeSwitched Event

Tracks when user switches between Picker and Palette modes.

### Basic ModeSwitched Handler

```razor
<SfColorPicker @bind-Value="@color" 
               ModeSwitcher="true"
               ModeSwitched="@OnModeSwitch">
</SfColorPicker>

<p>Current Mode: @currentMode</p>

@code {
    private string color = "#2ecc71";
    private string currentMode = "Picker";
    
    private void OnModeSwitch(ModeSwitchEventArgs args)
    {
        currentMode = args.Mode.ToString();
        Console.WriteLine($"Switched to: {currentMode}");
    }
}
```

### Track Mode Preferences

```razor
<SfColorPicker @bind-Value="@color" 
               ModeSwitcher="true"
               OnModeSwitch="@OnModeChange">
</SfColorPicker>

<div class="stats">
    <p>Picker Mode Used: @pickerUsage times</p>
    <p>Palette Mode Used: @paletteUsage times</p>
</div>

@code {
    private string color = "#f1c40f";
    private int pickerUsage = 0;
    private int paletteUsage = 0;
    
    private void OnModeChange(ModeSwitchEventArgs args)
    {
        if (args.Mode == ColorPickerMode.Picker)
            pickerUsage++;
        else
            paletteUsage++;
    }
}
```

## OnTileRender Event

Customize palette tiles before rendering.

### Custom Tile Styling

```razor
<SfColorPicker @bind-Value="@color" 
               Mode="ColorPickerMode.Palette"
               OnTileRender="@OnTileRenderHandler">
</SfColorPicker>

@code {
    private string color = "#3498db";
    
    private void OnTileRenderHandler(PaletteTileEventArgs args)
    {
        // Add custom class to specific colors
        if (args.Value == "#e74c3c")
        {
            args.Element.AddClass("favorite-color");
        }
        
        // Add tooltip
        args.Element.SetAttribute("title", $"Color: {args.Value}");
    }
}
```

### Mark Popular Colors

```razor
<SfColorPicker @bind-Value="@color" 
               Mode="ColorPickerMode.Palette"
               OnTileRender="@MarkPopularColors">
</SfColorPicker>

@code {
    private string color = "#9b59b6";
    private List<string> popularColors = new List<string> { "#3498db", "#e74c3c", "#2ecc71" };
    
    private void MarkPopularColors(PaletteTileEventArgs args)
    {
        if (popularColors.Contains(args.Value))
        {
            args.Element.AddClass("popular-tile");
            args.Element.SetAttribute("title", "Popular Color");
        }
    }
}

<style>
    .popular-tile {
        border: 2px solid gold !important;
        box-shadow: 0 0 5px rgba(255, 215, 0, 0.5);
    }
</style>
```

## Form Integration

### EditForm with Validation

```razor
<EditForm Model="@formModel" OnValidSubmit="@HandleSubmit">
    <DataAnnotationsValidator />
    
    <div class="form-group">
        <label>Brand Color:</label>
        <SfColorPicker @bind-Value="@formModel.BrandColor" 
                       Mode="ColorPickerMode.Palette">
        </SfColorPicker>
        <ValidationMessage For="@(() => formModel.BrandColor)" />
    </div>
    
    <div class="form-group">
        <label>Accent Color:</label>
        <SfColorPicker @bind-Value="@formModel.AccentColor" 
                       EnableOpacity="true">
        </SfColorPicker>
        <ValidationMessage For="@(() => formModel.AccentColor)" />
    </div>
    
    <button type="submit">Save Theme</button>
</EditForm>

@code {
    private ThemeFormModel formModel = new ThemeFormModel();
    
    private void HandleSubmit()
    {
        Console.WriteLine($"Brand: {formModel.BrandColor}, Accent: {formModel.AccentColor}");
        // Save to database
    }
    
    public class ThemeFormModel
    {
        [Required(ErrorMessage = "Brand color is required")]
        public string BrandColor { get; set; } = "#3498db";
        
        [Required(ErrorMessage = "Accent color is required")]
        public string AccentColor { get; set; } = "rgba(231, 76, 60, 0.8)";
    }
}
```

### Custom Validation

```razor
<EditForm Model="@colorForm" OnValidSubmit="@OnSubmit">
    <div class="form-group">
        <label>Background Color:</label>
        <SfColorPicker @bind-Value="@colorForm.BackgroundColor" 
                       ValueChange="@ValidateContrast">
        </SfColorPicker>
    </div>
    
    <div class="form-group">
        <label>Text Color:</label>
        <SfColorPicker @bind-Value="@colorForm.TextColor" 
                       ValueChange="@ValidateContrast">
        </SfColorPicker>
    </div>
    
    @if (!string.IsNullOrEmpty(validationMessage))
    {
        <div class="alert alert-warning">@validationMessage</div>
    }
    
    <button type="submit" disabled="@(!isValid)">Apply Colors</button>
</EditForm>

@code {
    private ColorFormModel colorForm = new ColorFormModel();
    private string validationMessage = "";
    private bool isValid = true;
    
    private void ValidateContrast(ColorPickerEventArgs args)
    {
        // Simplified contrast check
        bool bgIsLight = IsLightColor(colorForm.BackgroundColor);
        bool textIsLight = IsLightColor(colorForm.TextColor);
        
        if (bgIsLight == textIsLight)
        {
            validationMessage = "Warning: Poor contrast between background and text colors";
            isValid = false;
        }
        else
        {
            validationMessage = "";
            isValid = true;
        }
    }
    
    private bool IsLightColor(string color)
    {
        // Simplified lightness check (would need proper implementation)
        return color.Contains("255") || color.Contains("fff");
    }
    
    private void OnSubmit()
    {
        if (isValid)
        {
            Console.WriteLine($"Colors applied: BG={colorForm.BackgroundColor}, Text={colorForm.TextColor}");
        }
    }
    
    public class ColorFormModel
    {
        public string BackgroundColor { get; set; } = "#ffffff";
        public string TextColor { get; set; } = "#000000";
    }
}
```

## Real-Time Preview Patterns

### Live Background Update

```razor
@page "/live-background"
@using Syncfusion.Blazor.Inputs

<div class="config-panel">
    <h3>Choose Background Color</h3>
    <SfColorPicker @bind-Value="@bgColor" 
                   EnableOpacity="true"
                   ValueChange="@OnBackgroundChange">
    </SfColorPicker>
</div>

<div class="preview-area" style="background-color: @bgColor; min-height: 300px; padding: 40px; transition: background-color 0.3s ease;">
    <h2 style="color: white; text-shadow: 2px 2px 4px rgba(0,0,0,0.5);">
        Live Preview
    </h2>
    <p style="color: white;">Background color updates in real-time as you select</p>
</div>

@code {
    private string bgColor = "rgba(52, 152, 219, 0.9)";
    
    private void OnBackgroundChange(ColorPickerEventArgs args)
    {
        bgColor = args.CurrentValue.Rgba;
        StateHasChanged();
    }
}
```

### Multi-Element Theme Preview

```razor
@page "/theme-preview"
@using Syncfusion.Blazor.Inputs

<div class="theme-builder">
    <div class="controls">
        <div>
            <label>Primary:</label>
            <SfColorPicker @bind-Value="@primaryColor"></SfColorPicker>
        </div>
        <div>
            <label>Secondary:</label>
            <SfColorPicker @bind-Value="@secondaryColor"></SfColorPicker>
        </div>
        <div>
            <label>Accent:</label>
            <SfColorPicker @bind-Value="@accentColor"></SfColorPicker>
        </div>
    </div>
    
    <div class="preview">
        <button style="background-color: @primaryColor; color: white; border: none; padding: 10px 20px; border-radius: 4px;">
            Primary Button
        </button>
        <button style="background-color: @secondaryColor; color: white; border: none; padding: 10px 20px; border-radius: 4px;">
            Secondary Button
        </button>
        <button style="background-color: @accentColor; color: white; border: none; padding: 10px 20px; border-radius: 4px;">
            Accent Button
        </button>
    </div>
</div>

@code {
    private string primaryColor = "#3498db";
    private string secondaryColor = "#2ecc71";
    private string accentColor = "#e74c3c";
}
```

### Debounced Updates for Performance

```razor
<SfColorPicker @bind-Value="@color" 
               ValueChange="@OnColorChangeDebounced">
</SfColorPicker>

<p>Updates: @updateCount (debounced)</p>

@code {
    private string color = "#9b59b6";
    private int updateCount = 0;
    private System.Threading.Timer debounceTimer;
    
    private void OnColorChangeDebounced(ColorPickerEventArgs args)
    {
        debounceTimer?.Dispose();
        
        debounceTimer = new System.Threading.Timer(_ =>
        {
            InvokeAsync(() =>
            {
                updateCount++;
                // Perform expensive operation here
                Console.WriteLine($"Debounced update: {args.CurrentValue.Hex}");
                StateHasChanged();
            });
        }, null, 300, System.Threading.Timeout.Infinite);
    }
    
    public void Dispose()
    {
        debounceTimer?.Dispose();
    }
}
```

## Best Practices

1. **Use appropriate events**: ValueChanged for real-time, Selected for confirmation
2. **Async handlers**: Use async/await for I/O operations
3. **Debouncing**: Debounce frequent updates for performance
4. **Validation**: Validate colors in event handlers
5. **Preview**: Show real-time preview of color changes
6. **Error handling**: Handle event errors gracefully
7. **State management**: Call StateHasChanged() when needed
8. **Cleanup**: Dispose timers and resources
9. **User feedback**: Show loading states during async operations
10. **Accessibility**: Announce color changes to screen readers

## Related Topics

- **Getting Started**: Basic setup → [colorpicker-getting-started.md](colorpicker-getting-started.md)
- **Modes**: Configuration options → [colorpicker-modes-configuration.md](colorpicker-modes-configuration.md)
- **Customization**: Styling and accessibility → [colorpicker-customization-accessibility.md](colorpicker-customization-accessibility.md)
