# User Interaction

## Table of Contents
- [Overview](#overview)
- [Tooltips](#tooltips)
- [Pointer Dragging](#pointer-dragging)
- [Range Dragging](#range-dragging)
- [Events](#events)
- [Common Use Cases](#common-use-cases)

## Overview

The CircularGauge provides rich user interaction capabilities including tooltips for displaying information, draggable pointers and ranges for user input, and comprehensive event handling for custom behaviors.

## Tooltips

Tooltips display additional information when hovering over gauge elements. The `CircularGaugeTooltipSettings` component with properties like `Enable`, `Fill`, `Format`, and `ShowAtMousePosition` controls tooltip behavior.

### Pointer Tooltips

The `CircularGaugeTooltipSettings` component with the `Enable` property allows tooltips to show pointer values on hover:

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="65">
                </CircularGaugePointer>
            </CircularGaugePointers>
            <CircularGaugeTooltipSettings Enable="true">
            </CircularGaugeTooltipSettings>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Custom Tooltip Styling

The `CircularGaugeTooltipSettings` component properties including `Fill`, `EnableAnimation`, `Format`, `CircularGaugeTooltipBorder`, and `CircularGaugeTooltipTextStyle` customize tooltip appearance:

```cshtml
@using Syncfusion.Blazor.CircularGauge

<CircularGaugeTooltipSettings 
    Enable="true"
    Fill="lightgray"
    EnableAnimation="true"
    Format="Speed: {value} km/h">
    <CircularGaugeTooltipBorder Color="blue" Width="2">
    </CircularGaugeTooltipBorder>
    <CircularGaugeTooltipTextStyle 
        Color="blue"
        FontStyle="italic"
        FontWeight="bold"
        Size="15px"
        FontFamily="Arial">
    </CircularGaugeTooltipTextStyle>
</CircularGaugeTooltipSettings>
```

**Tooltip Properties:**
- `Enable`: Enable/disable tooltip
- `Fill`: Background color
- `EnableAnimation`: Animate tooltip appearance
- `Format`: Custom format string (use `{value}` placeholder)
- `ShowAtMousePosition`: Show at cursor position (default: false)

### Tooltip at Mouse Position

The `ShowAtMousePosition` property in `CircularGaugeTooltipSettings` controls tooltip positioning:

```cshtml
@using Syncfusion.Blazor.CircularGauge

<CircularGaugeTooltipSettings 
    Enable="true" 
    ShowAtMousePosition="true">
</CircularGaugeTooltipSettings>
```

**Default Behavior:** Tooltip appears on the axis  
**With `ShowAtMousePosition="true"`:** Tooltip follows cursor

### Range Tooltips

The `CircularGaugeRangeTooltipSettings` component with properties like `Fill` and `Format` displays information about ranges:

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeTooltipSettings Enable="true" Type="@TooltipType">
        <CircularGaugeRangeTooltipSettings 
            Fill="#FF6B6B"
            Format="Range: {start} - {end}">
            <CircularGaugeRangeTooltipBorder Color="#C92A2A" Width="1">
            </CircularGaugeRangeTooltipBorder>
        </CircularGaugeRangeTooltipSettings>
    </CircularGaugeTooltipSettings>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="50" Color="#30B32D">
                </CircularGaugeRange>
                <CircularGaugeRange Start="50" End="100" Color="#FF6B6B">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

@code {
    public string[] TooltipType = new string[] { "Range", "Pointer" };
}
```

### Annotation Tooltips

The `CircularGaugeAnnotationTooltipSettings` component with properties like `Format` and `Fill` shows tooltips for annotations:

```cshtml
@using Syncfusion.Blazor.CircularGauge

<CircularGaugeTooltipSettings Enable="true" Type="@TooltipType">
    <CircularGaugeAnnotationTooltipSettings 
        Format="Annotation: {value}"
        Fill="#E3F2FD">
    </CircularGaugeAnnotationTooltipSettings>
</CircularGaugeTooltipSettings>

@code {
    public string[] TooltipType = new string[] { "Annotation", "Pointer" };
}
```

### Combined Tooltips (Pointers, Ranges, Annotations)

The `CircularGaugeTooltipSettings` component with the `Type` property can combine multiple tooltip types such as pointers, ranges, and annotations:

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge EnablePointerDrag="true">
    <CircularGaugeTooltipSettings Enable="true" Type="@TooltipType">
        <CircularGaugeRangeTooltipSettings Fill="#FFE0B2">
        </CircularGaugeRangeTooltipSettings>
        <CircularGaugeAnnotationTooltipSettings Format="Info: {value}">
        </CircularGaugeAnnotationTooltipSettings>
    </CircularGaugeTooltipSettings>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="70">
                </CircularGaugePointer>
            </CircularGaugePointers>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="50">
                </CircularGaugeRange>
                <CircularGaugeRange Start="50" End="100">
                </CircularGaugeRange>
            </CircularGaugeRanges>
            <CircularGaugeAnnotations>
                <CircularGaugeAnnotation Angle="180">
                    <ContentTemplate>
                        <div>Center Label</div>
                    </ContentTemplate>
                </CircularGaugeAnnotation>
            </CircularGaugeAnnotations>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

@code {
    public string[] TooltipType = new string[] { "Range", "Annotation", "Pointer" };
}
```

## Pointer Dragging

The `EnablePointerDrag` property on the `SfCircularGauge` component allows users to adjust values by dragging pointers.

### Basic Pointer Dragging

The `EnablePointerDrag` property set to true on the `SfCircularGauge` component enables pointer dragging functionality:

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge EnablePointerDrag="true">
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="50">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Set `EnablePointerDrag="true"` on the `SfCircularGauge` component.**

### Handling Drag Events

The `CircularGaugeEvents` component with properties like `OnDragStart`, `OnDrag`, and `OnDragEnd` responds to drag actions:

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge EnablePointerDrag="true">
    <CircularGaugeEvents 
        OnDragStart="DragStart"
        OnDrag="DragMove"
        OnDragEnd="DragEnd">
    </CircularGaugeEvents>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="@pointerValue">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

<div>Current Value: @pointerValue</div>

@code {
    private double pointerValue = 50;
    
    void DragStart(Syncfusion.Blazor.CircularGauge.PointerDragEventArgs args)
    {
        Console.WriteLine($"Drag started at: {args.CurrentValue}");
    }
    
    void DragMove(Syncfusion.Blazor.CircularGauge.PointerDragEventArgs args)
    {
        pointerValue = args.CurrentValue;
    }
    
    void DragEnd(Syncfusion.Blazor.CircularGauge.PointerDragEventArgs args)
    {
        Console.WriteLine($"Final value: {args.CurrentValue}");
        // Save to database, update state, etc.
    }
}
```

**Event Arguments (`IPointerDragEventArgs`):**
- `AxisIndex`: Index of the axis
- `PointerIndex`: Index of the pointer
- `CurrentValue`: Current pointer value
- `PreviousValue`: Previous value (OnDragMove only)

### Interactive Gauge with Validation

The `CircularGaugeEvents` component with `OnDragEnd` handler combined with `EnablePointerDrag` property validates and processes user input:

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge EnablePointerDrag="true">
    <CircularGaugeEvents OnDragEnd="ValidateAndSave">
    </CircularGaugeEvents>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugePointers>
                <CircularGaugePointer Value="@temperature">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

<div>
    <p>Temperature: @temperature°C</p>
    <p style="color: @statusColor">@statusMessage</p>
</div>

@code {
    private double temperature = 22;
    private string statusMessage = "";
    private string statusColor = "black";
    
    void ValidateAndSave(Syncfusion.Blazor.CircularGauge.PointerDragEventArgs.PointerDragEventArgs args)
    {
        temperature = args.CurrentValue;
        
        if (temperature < 18)
        {
            statusMessage = "Too cold!";
            statusColor = "blue";
        }
        else if (temperature > 26)
        {
            statusMessage = "Too hot!";
            statusColor = "red";
        }
        else
        {
            statusMessage = "Comfortable";
            statusColor = "green";
        }
        
        // Save to backend
        // await SaveTemperature(temperature);
    }
}
```

## Range Dragging

The `EnableRangeDrag` property on the `SfCircularGauge` component allows users to adjust range boundaries by dragging.

### Enable Range Dragging

The `EnableRangeDrag` property set to true on the `SfCircularGauge` component enables range dragging to define threshold boundaries:

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge EnableRangeDrag="true">
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="50">
                </CircularGaugePointer>
            </CircularGaugePointers>
            <CircularGaugeRanges>
                <CircularGaugeRange 
                    Start="0" 
                    End="80" 
                    Radius="108%" 
                    Color="#30B32D" 
                    StartWidth="8" 
                    EndWidth="8">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Use Case:** Allow users to define threshold ranges dynamically (e.g., "safe zone" boundaries).

## Events

The `CircularGaugeEvents` component provides comprehensive event handling with properties like `OnLoad`, `Loaded`, `OnDragStart`, `OnDrag`, `OnDragEnd`, and mouse events for custom behaviors.

### Available Events

#### Lifecycle Events

**OnLoad**
- Triggers before rendering the gauge
- First event fired
- Use for initial setup

```cshtml
@using Syncfusion.Blazor.CircularGauge

<CircularGaugeEvents OnLoad="OnLoadHandler">
</CircularGaugeEvents>

@code {
    void OnLoadHandler()
    {
        Console.WriteLine("Gauge is loading...");
    }
}
```

**Loaded**
- Triggers after gauge is fully loaded
- Use for post-render operations

```cshtml
@using Syncfusion.Blazor.CircularGauge

<CircularGaugeEvents Loaded="LoadedHandler">
</CircularGaugeEvents>

@code {
    void LoadedHandler()
    {
        Console.WriteLine("Gauge loaded successfully!");
    }
}
```

**AnimationCompleted**
- Triggers after animation completes
- Use for chaining operations

```cshtml
@using Syncfusion.Blazor.CircularGauge

<CircularGaugeEvents AnimationCompleted="AnimationDone">
</CircularGaugeEvents>

@code {
    void AnimationDone()
    {
        Console.WriteLine("Animation finished");
    }
}
```

#### Drag Events

**OnDragStart**
- Triggers when drag begins
- Access initial pointer value

```cshtml
@using Syncfusion.Blazor.CircularGauge

<CircularGaugeEvents OnDragStart="DragStartHandler">
</CircularGaugeEvents>

@code {
    void DragStartHandler(IPointerDragEventArgs args)
    {
        Console.WriteLine($"Started dragging pointer {args.PointerIndex}");
    }
}
```

**OnDragMove**
- Triggers during drag
- Real-time value updates

```cshtml
@using Syncfusion.Blazor.CircularGauge

<CircularGaugeEvents OnDragMove="DragMoveHandler">
</CircularGaugeEvents>

@code {
    void DragMoveHandler(IPointerDragEventArgs args)
    {
        Console.WriteLine($"Current: {args.CurrentValue}, Previous: {args.PreviousValue}");
    }
}
```

**OnDragEnd**
- Triggers when drag completes
- Final value persistence

```cshtml
@using Syncfusion.Blazor.CircularGauge

<CircularGaugeEvents OnDragEnd="DragEndHandler">
</CircularGaugeEvents>

@code {
    async Task DragEndHandler(IPointerDragEventArgs args)
    {
        await SaveValue(args.CurrentValue);
    }
}
```

#### Mouse Events

**OnGaugeMouseDown**
- Triggers on mouse down

**OnGaugeMouseUp**
- Triggers on mouse up

**OnGaugeMouseMove**
- Triggers on mouse move

**OnGaugeMouseLeave**
- Triggers when mouse leaves gauge

```cshtml
@using Syncfusion.Blazor.CircularGauge

<CircularGaugeEvents 
    OnGaugeMouseMove="MouseMoveHandler"
    OnGaugeMouseLeave="MouseLeaveHandler">
</CircularGaugeEvents>

@code {
    void MouseMoveHandler(MouseEventArgs args)
    {
        Console.WriteLine($"Mouse at: {args.X}, {args.Y}");
    }
    
    void MouseLeaveHandler(MouseEventArgs args)
    {
        Console.WriteLine("Mouse left gauge");
    }
}
```

#### Rendering Events

**AnnotationRendering**
- Triggers before each annotation renders
- Customize annotations dynamically

```cshtml
@using Syncfusion.Blazor.CircularGauge

<CircularGaugeEvents AnnotationRendering="CustomizeAnnotation">
</CircularGaugeEvents>

@code {
    void CustomizeAnnotation(IAnnotationRenderEventArgs args)
    {
        // Modify annotation content or style
        args.Content = $"Value: {pointerValue}";
    }
}
```

**OnRadiusCalculate**
- Triggers before radius calculation
- Customize gauge radius dynamically

```cshtml
@using Syncfusion.Blazor.CircularGauge

<CircularGaugeEvents OnRadiusCalculate="CalculateRadius">
</CircularGaugeEvents>

@code {
    void CalculateRadius(RadiusEventArgs args)
    {
        // Custom radius logic
    }
}
```

### Complete Event Example

The `CircularGaugeEvents` component properties including `OnLoad`, `Loaded`, `OnDragStart`, `OnDrag`, `OnDragEnd`, and `AnimationCompleted` can be combined for comprehensive event handling:

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge EnablePointerDrag="true">
    <CircularGaugeEvents 
        OnLoad="OnLoad"
        Loaded="Loaded"
        OnDragStart="DragStart"
        OnDrag="DragMove"
        OnDragEnd="DragEnd"
        AnimationCompleted="AnimationComplete">
    </CircularGaugeEvents>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="@currentValue">
                    <CircularGaugePointerAnimation Enable="true" Duration="1000">
                    </CircularGaugePointerAnimation>
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

<div>
    <p>Status: @status</p>
    <p>Value: @currentValue</p>
</div>

@code {
    private double currentValue = 50;
    private string status = "Initializing...";
    
    void OnLoad()
    {
        status = "Loading gauge...";
    }
    
    void Loaded()
    {
        status = "Gauge ready";
    }
    
    void DragStart(Syncfusion.Blazor.CircularGauge.PointerDragEventArgs args)
    {
        status = "Adjusting value...";
    }
    
    void DragMove(Syncfusion.Blazor.CircularGauge.PointerDragEventArgs args)
    {
        currentValue = args.CurrentValue;
    }
    
    async Task DragEnd(Syncfusion.Blazor.CircularGauge.PointerDragEventArgs args)
    {
        status = "Saving...";
        await Task.Delay(500); // Simulate API call
        status = $"Saved: {args.CurrentValue}";
    }
    
    void AnimationComplete()
    {
        status = "Animation done";
    }
}
```

## Common Use Cases

### Temperature Control

The `EnablePointerDrag` property with `CircularGaugeEvents` and `CircularGaugeTooltipSettings` enables interactive temperature control:

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge EnablePointerDrag="true">
    <CircularGaugeEvents OnDragEnd="UpdateTemperature">
    </CircularGaugeEvents>
    <CircularGaugeTooltipSettings Enable="true" Format="{value}°C">
    </CircularGaugeTooltipSettings>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="16" Maximum="30" StartAngle="270" EndAngle="90">
            <CircularGaugeAxisLabelStyle Format="{value}°C">
            </CircularGaugeAxisLabelStyle>
            <CircularGaugePointers>
                <CircularGaugePointer Value="@targetTemp" Color="#007DD1">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

@code {
    private double targetTemp = 22;
    
    async Task UpdateTemperature(Syncfusion.Blazor.CircularGauge.PointerDragEventArgs args)
    {
        targetTemp = Math.Round(args.CurrentValue, 1);
        await SendToThermostat(targetTemp);
    }
    
    async Task SendToThermostat(double temp)
    {
        // API call to thermostat
        await Task.CompletedTask;
    }
}
```

### Volume Control

The `EnablePointerDrag` property with `CircularGaugeEvents` and pointer `Type` property enables volume adjustment:

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge EnablePointerDrag="true">
    <CircularGaugeEvents OnDragMove="AdjustVolume">
    </CircularGaugeEvents>
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugePointers>
                <CircularGaugePointer Value="@volume" Type="PointerType.RangeBar">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

@code {
    private double volume = 50;
    
    void AdjustVolume(Syncfusion.Blazor.CircularGauge.PointerDragEventArgs args)
    {
        volume = args.CurrentValue;
        // Update audio system
    }
}
```

### Progress Indicator with Tooltips

The `CircularGaugeTooltipSettings` component with `Enable`, `Format`, and `ShowAtMousePosition` properties displays progress information:

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeTooltipSettings 
        Enable="true" 
        Format="Progress: {value}%"
        ShowAtMousePosition="true">
    </CircularGaugeTooltipSettings>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="75" Type="PointerType.RangeBar">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Best Practices

### Tooltips
- Enable tooltips for informational gauges
- Use custom formats with units
- Style tooltips to match your theme
- Position at mouse for dense dashboards

### Dragging
- Enable for user input scenarios (settings, controls)
- Disable animation when dragging is enabled
- Validate values on DragEnd
- Provide visual feedback during drag
- Save changes asynchronously

### Events
- Use OnDragMove sparingly (fires frequently)
- Debounce rapid updates
- Handle errors in async event handlers
- Provide user feedback for actions

### Performance
- Disable tooltips for gauges with rapid updates
- Use OnDragEnd for persistence, not OnDragMove
- Limit event logging in production
- Batch updates when possible

## Troubleshooting

**Tooltip not showing:**
- Verify `Enable="true"` in CircularGaugeTooltipSettings
- Check pointer/range is within visible area
- Ensure element isn't covered by other components

**Dragging not working:**
- Set `EnablePointerDrag="true"` on SfCircularGauge (not on pointer)
- Check pointer is within axis bounds
- Verify no CSS preventing mouse events

**Events not firing:**
- Ensure CircularGaugeEvents component is added
- Check event handler method signature matches
- Verify no JavaScript errors in console

**Drag performance issues:**
- Avoid heavy operations in OnDragMove
- Use OnDragEnd for saving/API calls
- Disable animation during drag
- Debounce frequent updates
