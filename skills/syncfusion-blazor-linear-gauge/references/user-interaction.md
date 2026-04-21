# User Interaction in Blazor Linear Gauge

## Table of Contents
- [Overview](#overview)
- [Tooltips](#tooltips)
  - [Enable Tooltips](#enable-tooltips)
  - [Format Tooltips](#format-tooltips)
  - [Customize Tooltip Appearance](#customize-tooltip-appearance)
  - [Conditional Tooltips](#conditional-tooltips)
- [Pointer Drag](#pointer-drag)
  - [Enable Dragging](#enable-dragging)
  - [Drag Events](#drag-events)
  - [Constrain Drag Range](#constrain-drag-range)
  - [Multiple Draggable Pointers](#multiple-draggable-pointers)
- [Mouse and Touch Events](#mouse-and-touch-events)
- [Practical Examples](#practical-examples)

## Overview

The Blazor Linear Gauge provides interactive features that allow users to:

- **View tooltips** when hovering over pointers
- **Drag pointers** to change values dynamically
- **Respond to mouse and touch events** for custom interactions

These features enable creating interactive dashboards, adjustment controls, and data input interfaces.

## Tooltips

Tooltips display pointer values when users hover over or interact with pointers.

### Enable Tooltips

Use the `LinearGaugeTooltipSettings` component with `Enable="true"`.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeTooltipSettings Enable="true">
    </LinearGaugeTooltipSettings>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="65"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Hovering over the pointer displays its value in a tooltip.

**Default behavior:**
- Tooltip shows the pointer value
- Appears on hover (desktop) or touch (mobile)
- Follows the pointer position

### Format Tooltips

Customize tooltip text using the `Format` property.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeTooltipSettings Enable="true" Format="{value}°C">
    </LinearGaugeTooltipSettings>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="50">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="22"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Tooltip displays "22°C" instead of just "22".

**Format placeholders:**
- `{value}` - The pointer value

#### Percentage Format

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeTooltipSettings Enable="true" Format="Progress: {value}%">
    </LinearGaugeTooltipSettings>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="75"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Tooltip displays "Progress: 75%".

#### Currency Format

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeTooltipSettings Enable="true" Format="${value}">
    </LinearGaugeTooltipSettings>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="1000">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="350"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** Tooltip displays "$350".

### Customize Tooltip Appearance

Style tooltips using tooltip border and text properties.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeTooltipSettings Enable="true" 
                                Format="{value}°C"
                                Fill="#007bff">
        <LinearGaugeTooltipBorder Color="#0056b3" Width="2">
        </LinearGaugeTooltipBorder>
        <LinearGaugeTooltipTextStyle Color="white" 
                                     FontWeight="bold" 
                                     Size="14px">
        </LinearGaugeTooltipTextStyle>
    </LinearGaugeTooltipSettings>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="50">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="28"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Tooltip Properties:**
- `Fill` - Background color
- `LinearGaugeTooltipBorder.Color` - Border color
- `LinearGaugeTooltipBorder.Width` - Border thickness
- `LinearGaugeTooltipTextStyle.Color` - Text color
- `LinearGaugeTooltipTextStyle.Size` - Font size
- `LinearGaugeTooltipTextStyle.FontWeight` - Font weight

#### Dark Theme Tooltip

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeTooltipSettings Enable="true" 
                                Format="Value: {value}"
                                Fill="#212529">
        <LinearGaugeTooltipBorder Color="#495057" Width="1">
        </LinearGaugeTooltipBorder>
        <LinearGaugeTooltipTextStyle Color="#f8f9fa" 
                                     Size="13px">
        </LinearGaugeTooltipTextStyle>
    </LinearGaugeTooltipSettings>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="60"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

#### Transparent Tooltip

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeTooltipSettings Enable="true" 
                                Format="{value}%"
                                Fill="rgba(0, 123, 255, 0.9)">
        <LinearGaugeTooltipBorder Width="0">
        </LinearGaugeTooltipBorder>
        <LinearGaugeTooltipTextStyle Color="white" 
                                     FontWeight="600">
        </LinearGaugeTooltipTextStyle>
    </LinearGaugeTooltipSettings>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="82"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

### Conditional Tooltips

Use the `TooltipRendering` event to customize tooltip content dynamically.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeEvents TooltipRendering="OnTooltipRendering">
    </LinearGaugeEvents>
    <LinearGaugeTooltipSettings Enable="true">
    </LinearGaugeTooltipSettings>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="@currentValue" 
                                    EnableDrag="true">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private double currentValue = 60;
    
    private void OnTooltipRendering(Syncfusion.Blazor.LinearGauge.TooltipRenderEventArgs args)
    {
        double value = double.Parse(args.Content);
        
        if (value < 33)
        {
            args.Content = $"Low: {value}%";
        }
        else if (value < 66)
        {
            args.Content = $"Medium: {value}%";
        }
        else
        {
            args.Content = $"High: {value}%";
        }
    }
}
```

**Use case:** Status-based tooltips, conditional formatting, contextual information.

#### Temperature-Based Tooltip

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeEvents TooltipRendering="OnTooltipRendering">
    </LinearGaugeEvents>
    <LinearGaugeTooltipSettings Enable="true">
    </LinearGaugeTooltipSettings>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="-20" Maximum="50">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="@temperature" 
                                    EnableDrag="true">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private double temperature = 22;
    
    private void OnTooltipRendering(Syncfusion.Blazor.LinearGauge.TooltipRenderEventArgs args)
    {
        double temp = double.Parse(args.Content);
        string status = "";
        
        if (temp < 0)
            status = "❄️ Freezing";
        else if (temp < 15)
            status = "🧊 Cold";
        else if (temp < 25)
            status = "😊 Comfortable";
        else if (temp < 35)
            status = "🌡️ Warm";
        else
            status = "🔥 Hot";
        
        args.Content = $"{status}: {temp}°C";
    }
}
```

## Pointer Drag

Enable users to change pointer values by dragging with mouse or touch.

### Enable Dragging

Set `EnableDrag="true"` on the pointer.

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="50" 
                                    EnableDrag="true">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>
```

**Result:** User can click and drag the pointer to change its value.

**Default behavior:**
- Pointer follows mouse/touch position
- Value updates in real-time
- Constrained to axis minimum and maximum

#### Draggable Bar Pointer

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeContainer Width="30">
        <LinearGaugeAxes>
            <LinearGaugeAxis Minimum="0" Maximum="100">
                <LinearGaugePointers>
                    <LinearGaugePointer PointerValue="65" 
                                        Type="Syncfusion.Blazor.LinearGauge.Point.Bar"
                                        Width="30"
                                        EnableDrag="true"
                                        Color="#007bff">
                    </LinearGaugePointer>
                </LinearGaugePointers>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </LinearGaugeContainer>
</SfLinearGauge>
```

**Result:** User can drag the bar to adjust the value, like a volume control.

### Drag Events

Respond to drag interactions with events: `OnDragStart`, `OnDragEnd`.

```razor
@using Syncfusion.Blazor.LinearGauge

<p>Status: @dragStatus</p>
<p>Current Value: @currentValue</p>

<SfLinearGauge>
    <LinearGaugeEvents 
        OnDragStart="OnDragStart"
        OnDragEnd="OnDragEnd">
    </LinearGaugeEvents>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="@currentValue" 
                                    EnableDrag="true"
                                    AnimationDuration="0">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private double currentValue = 50;
    private string dragStatus = "Ready";
    
    private void OnDragStart(Syncfusion.Blazor.LinearGauge.PointerDragEventArgs args)
    {
        dragStatus = "Dragging started";
    }
    
    private void OnDragEnd(Syncfusion.Blazor.LinearGauge.PointerDragEventArgs args)
    {
        currentValue = Math.Round(args.CurrentValue, 1);
        dragStatus = $"Drag ended at: {currentValue}";
    }
}
```

**Events:**
- `OnDragStart` - Fires when drag begins
- `OnDragEnd` - Fires when drag completes (use for saving/validation)

### Constrain Drag Range

Use axis `Minimum` and `Maximum` to limit the drag range.

```razor
@using Syncfusion.Blazor.LinearGauge

<p>Temperature (10-30°C): @temperature°C</p>

<SfLinearGauge>
    <LinearGaugeEvents OnDragEnd="OnDragEnd">
    </LinearGaugeEvents>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="10" Maximum="30">
            <LinearGaugeAxisLabelStyle Format="{value}°C">
            </LinearGaugeAxisLabelStyle>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="@temperature" 
                                    EnableDrag="true">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private double temperature = 20;
    
    private void OnDragEnd(Syncfusion.Blazor.LinearGauge.PointerDragEventArgs args)
    {
        temperature = Math.Round(args.CurrentValue, 1);
        // Value is automatically constrained between 10 and 30
    }
}
```

**Result:** Pointer cannot be dragged below 10 or above 30.

#### Custom Validation

```razor
@using Syncfusion.Blazor.LinearGauge

<p>Volume (0-100): @volume</p>
@if (validationMessage != "")
{
    <p style="color: red;">@validationMessage</p>
}

<SfLinearGauge>
    <LinearGaugeEvents OnDragEnd="OnDragEnd">
    </LinearGaugeEvents>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="@volume" 
                                    EnableDrag="true">
                </LinearGaugePointer>
            </LinearGaugePointers>
            
            <LinearGaugeRanges>
                <LinearGaugeRange Start="80" End="100" Color="#dc3545">
                </LinearGaugeRange>
            </LinearGaugeRanges>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private double volume = 50;
    private string validationMessage = "";
    
    private void OnDragEnd(Syncfusion.Blazor.LinearGauge.PointerDragEventArgs args)
    {
        volume = Math.Round(args.CurrentValue, 0);
        
        if (volume > 80)
        {
            validationMessage = "Warning: Volume is very high!";
        }
        else
        {
            validationMessage = "";
        }
    }
}
```

### Multiple Draggable Pointers

Allow multiple pointers to be dragged independently.

```razor
@using Syncfusion.Blazor.LinearGauge

<div>
    <p>Min: @minValue | Max: @maxValue | Range: @(maxValue - minValue)</p>
</div>

<SfLinearGauge>
    <LinearGaugeEvents OnDragEnd="OnDragEnd">
    </LinearGaugeEvents>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <!-- Minimum pointer -->
                <LinearGaugePointer PointerValue="@minValue" 
                                    EnableDrag="true"
                                    Color="#3498db"
                                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Circle">
                </LinearGaugePointer>
                
                <!-- Maximum pointer -->
                <LinearGaugePointer PointerValue="@maxValue" 
                                    EnableDrag="true"
                                    Color="#e74c3c"
                                    MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Diamond">
                </LinearGaugePointer>
            </LinearGaugePointers>
            
            <!-- Highlighted range between min and max -->
            <LinearGaugeRanges>
                <LinearGaugeRange Start="@minValue" 
                                  End="@maxValue" 
                                  Color="#2ecc71"
                                  StartWidth="10"
                                  EndWidth="10">
                </LinearGaugeRange>
            </LinearGaugeRanges>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private double minValue = 30;
    private double maxValue = 70;
    
    private void OnDragEnd(Syncfusion.Blazor.LinearGauge.PointerDragEventArgs args)
    {
        if (args.PointerIndex == 0)
        {
            // Minimum pointer
            minValue = Math.Round(args.CurrentValue, 0);
            
            // Ensure min doesn't exceed max
            if (minValue > maxValue)
            {
                minValue = maxValue;
            }
        }
        else if (args.PointerIndex == 1)
        {
            // Maximum pointer
            maxValue = Math.Round(args.CurrentValue, 0);
            
            // Ensure max doesn't go below min
            if (maxValue < minValue)
            {
                maxValue = minValue;
            }
        }
    }
}
```

**Use case:** Range selectors, min/max controls, comparison tools.

## Mouse and Touch Events

The Linear Gauge automatically supports both mouse and touch interactions.

### Mouse Support

- **Hover:** Displays tooltips on pointer hover
- **Click and Drag:** Drag pointers with mouse
- **Scroll:** Page scrolls normally (gauge doesn't interfere)

### Touch Support

- **Tap:** Shows tooltip on touch
- **Touch and Drag:** Drag pointers with finger
- **Multi-touch:** Supports dragging multiple pointers on multi-touch devices

### Cross-Platform Example

```razor
@using Syncfusion.Blazor.LinearGauge

<p>Interaction: @interactionType</p>
<p>Value: @currentValue</p>

<SfLinearGauge>
    <LinearGaugeEvents 
        OnDragStart="OnDragStart"
        OnDragEnd="OnDragEnd">
    </LinearGaugeEvents>
    <LinearGaugeTooltipSettings Enable="true" Format="{value}%">
    </LinearGaugeTooltipSettings>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="@currentValue" 
                                    EnableDrag="true"
                                    AnimationDuration="300">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private double currentValue = 50;
    private string interactionType = "Ready";
    
    private void OnDragStart(Syncfusion.Blazor.LinearGauge.PointerDragEventArgs args)
    {
        interactionType = "Interacting (Mouse or Touch)";
    }
    
    private void OnDragEnd(Syncfusion.Blazor.LinearGauge.PointerDragEventArgs args)
    {
        currentValue = Math.Round(args.CurrentValue, 0);
        interactionType = $"Updated via interaction";
    }
}
```

**Result:** Works seamlessly on desktop (mouse) and mobile (touch) without code changes.

## Practical Examples

### Temperature Range Selector

```razor
@using Syncfusion.Blazor.LinearGauge

<div class="temperature-selector">
    <h3>🌡️ Preferred Temperature Range</h3>
    <div class="range-display">
        <div>Min: <strong>@minTemp°C</strong></div>
        <div>Max: <strong>@maxTemp°C</strong></div>
        <div>Range: <strong>@(maxTemp - minTemp)°C</strong></div>
    </div>
    
    <SfLinearGauge Width="100%" Height="400px">
        <LinearGaugeEvents 
            OnDragEnd="OnDragEnd"
            TooltipRendering="OnTooltipRendering">
        </LinearGaugeEvents>
        <LinearGaugeTooltipSettings Enable="true">
        </LinearGaugeTooltipSettings>
        
        <LinearGaugeAxes>
            <LinearGaugeAxis Minimum="10" Maximum="35">
                <LinearGaugeAxisLabelStyle Format="{value}°C">
                </LinearGaugeAxisLabelStyle>
                
                <LinearGaugeMajorTicks Interval="5"></LinearGaugeMajorTicks>
                <LinearGaugeMinorTicks Interval="1"></LinearGaugeMinorTicks>
                
                <LinearGaugePointers>
                    <!-- Minimum temperature pointer -->
                    <LinearGaugePointer PointerValue="@minTemp" 
                                        EnableDrag="true"
                                        Color="#3498db"
                                        MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.Triangle"
                                        Placement="Syncfusion.Blazor.LinearGauge.Placement.Near"
                                        AnimationDuration="0">
                    </LinearGaugePointer>
                    
                    <!-- Maximum temperature pointer -->
                    <LinearGaugePointer PointerValue="@maxTemp" 
                                        EnableDrag="true"
                                        Color="#e74c3c"
                                        MarkerType="Syncfusion.Blazor.LinearGauge.MarkerType.InvertedTriangle"
                                        Placement="Syncfusion.Blazor.LinearGauge.Placement.Near"
                                        AnimationDuration="0">
                    </LinearGaugePointer>
                </LinearGaugePointers>
                
                <!-- Comfort zone range -->
                <LinearGaugeRanges>
                    <LinearGaugeRange Start="@minTemp" 
                                      End="@maxTemp" 
                                      Color="#2ecc71"
                                      StartWidth="15"
                                      EndWidth="15">
                    </LinearGaugeRange>
                </LinearGaugeRanges>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </SfLinearGauge>
    
    <div class="status">
        @statusMessage
    </div>
</div>

<style>
    .temperature-selector {
        max-width: 350px;
        padding: 25px;
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        border-radius: 12px;
        color: white;
    }
    .temperature-selector h3 {
        margin-top: 0;
        text-align: center;
    }
    .range-display {
        display: flex;
        justify-content: space-around;
        margin-bottom: 20px;
        background: rgba(255,255,255,0.2);
        padding: 15px;
        border-radius: 8px;
    }
    .status {
        text-align: center;
        margin-top: 15px;
        padding: 10px;
        background: rgba(255,255,255,0.2);
        border-radius: 6px;
        font-weight: 600;
    }
</style>

@code {
    private double minTemp = 18;
    private double maxTemp = 24;
    private string statusMessage = "Adjust your comfort zone";
    
    private void OnDragEnd(Syncfusion.Blazor.LinearGauge.PointerDragEventArgs args)
    {
        if (args.PointerIndex == 0)
        {
            minTemp = Math.Round(args.CurrentValue, 1);
            if (minTemp > maxTemp) minTemp = maxTemp;
        }
        else
        {
            maxTemp = Math.Round(args.CurrentValue, 1);
            if (maxTemp < minTemp) maxTemp = minTemp;
        }
        
        double range = maxTemp - minTemp;

        if (range < 3)
            statusMessage = "Narrow range - very specific!";
        else if (range < 6)
            statusMessage = "Comfortable range";
        else
            statusMessage = "Wide range - flexible!";
    }
    
    private void OnTooltipRendering(Syncfusion.Blazor.LinearGauge.TooltipRenderEventArgs args)
    {
        double temp = double.Parse(args.Content);
        args.Content = $"{temp}°C";
    }
}
```

## Key Takeaways

1. **Tooltips:** Enable with `LinearGaugeTooltipSettings`, customize format and appearance
2. **Format:** Use `{value}` placeholder with custom text (e.g., "{value}°C")
3. **Styling:** Customize tooltip colors, borders, and fonts
4. **Conditional Tooltips:** Use `TooltipRendering` event for dynamic content
5. **Pointer Drag:** Enable with `EnableDrag="true"` on pointers
6. **Drag Events:** Use `OnDragStart`, `OnDragEnd` for interaction logic
7. **Live Updates:** Set `AnimationDuration="0"` for instant feedback during drag
8. **Range Constraints:** Axis min/max automatically constrain drag values
9. **Multiple Pointers:** Each pointer can be independently draggable
10. **Cross-Platform:** Works seamlessly with mouse (desktop) and touch (mobile)

User interaction features make Linear Gauge ideal for interactive controls, settings panels, and data input interfaces.
