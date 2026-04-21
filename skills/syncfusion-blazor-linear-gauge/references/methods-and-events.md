# Methods and Events in Blazor Linear Gauge

## Table of Contents
- [Overview](#overview)
- [Methods](#methods)
  - [SetPointerValue](#setpointervalue)
  - [SetAnnotationValue](#setannotationvalue)
  - [RefreshAsync](#refreshasync)
  - [PrintAsync](#printasync)
- [Lifecycle Events](#lifecycle-events)
  - [OnLoad](#onload)
  - [Loaded](#loaded)
- [Rendering Events](#rendering-events)
  - [AxisLabelRendering](#axislabelrendering)
  - [AnnotationRendering](#annotationrendering)
- [User Interaction Events](#user-interaction-events)
  - [ValueChange](#valuechange)
  - [TooltipRendering](#tooltiprendering)
  - [OnDragStart](#ondragstart)
  - [OnDragEnd](#ondragend)
- [Utility Events](#utility-events)
  - [Resizing](#resizing)
- [Complete Examples](#complete-examples)

## Overview

The Blazor Linear Gauge provides a comprehensive set of methods and events to:

- **Methods:** Programmatically control the gauge (update values, refresh, print)
- **Lifecycle Events:** Hook into gauge initialization
- **Rendering Events:** Customize elements during rendering
- **User Interaction Events:** Respond to pointer drags and tooltip displays
- **Utility Events:** Handle responsive behavior

This guide covers all available methods and events with practical examples.

## Methods

Methods allow you to programmatically control the gauge after initialization.

### SetPointerValue

Update a pointer's value programmatically.

**Syntax:**
```csharp
gauge.SetPointerValue(axisIndex, pointerIndex, value);
```

**Parameters:**
- `axisIndex` (int): Zero-based index of the axis
- `pointerIndex` (int): Zero-based index of the pointer
- `value` (double): New pointer value

**Example:**

```razor
@using Syncfusion.Blazor.LinearGauge

<button @onclick="UpdatePointer">Update to 80</button>

<SfLinearGauge @ref="gauge">
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="50"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    SfLinearGauge gauge;
    
    private void UpdatePointer()
    {
        gauge.SetPointerValue(0, 0, 80);
    }
}
```

**Use case:** Real-time data updates (sensor readings, progress tracking, live metrics).

#### Multiple Pointers Example

```razor
@using Syncfusion.Blazor.LinearGauge

<button @onclick="UpdateMultiplePointers">Update All</button>

<SfLinearGauge @ref="gauge">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="30" Color="#3498db">
                </LinearGaugePointer>
                <LinearGaugePointer PointerValue="60" Color="#e74c3c">
                </LinearGaugePointer>
                <LinearGaugePointer PointerValue="90" Color="#2ecc71">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    SfLinearGauge gauge;
    
    private void UpdateMultiplePointers()
    {
        gauge.SetPointerValue(0, 0, 45);  // First pointer
        gauge.SetPointerValue(0, 1, 72);  // Second pointer
        gauge.SetPointerValue(0, 2, 95);  // Third pointer
    }
}
```

#### Timer-Based Updates

```razor
@using Syncfusion.Blazor.LinearGauge
@using System.Timers
@implements IDisposable

<SfLinearGauge @ref="gauge">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeAxisLabelStyle Format="{value}%">
            </LinearGaugeAxisLabelStyle>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="@currentValue" 
                                    Color="#007bff"
                                    AnimationDuration="500">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    SfLinearGauge gauge;
    private double currentValue = 0;
    private Timer timer;
    private Random random = new Random();
    
    protected override void OnInitialized()
    {
        timer = new Timer(2000);  // Update every 2 seconds
        timer.Elapsed += (sender, e) => UpdateValue();
        timer.Start();
    }
    
    private void UpdateValue()
    {
        currentValue = random.Next(0, 101);
        if (gauge != null)
        {
            gauge.SetPointerValue(0, 0, currentValue);
            StateHasChanged();
        }
    }
    
    public void Dispose()
    {
        timer?.Stop();
        timer?.Dispose();
    }
}
```

### SetAnnotationValue

Update an annotation's content dynamically.

**Syntax:**
```csharp
gauge.SetAnnotationValue(annotationIndex, axisIndex, content);
```

**Parameters:**
- `annotationIndex` (int): Zero-based index of the annotation
- `content` (string): New HTML content for the annotation
- `axisValue` (int): Zero-based index of the axis

**Example:**

```razor
@using Syncfusion.Blazor.LinearGauge

<button @onclick="UpdateAnnotation">Update Status</button>

<SfLinearGauge @ref="gauge">
    <LinearGaugeAnnotations>
        <LinearGaugeAnnotation AxisValue="0" ZIndex="1" 
                               Content="Status: OK">
        </LinearGaugeAnnotation>
    </LinearGaugeAnnotations>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="50"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    SfLinearGauge gauge;
    private bool isWarning = false;
    
    private void UpdateAnnotation()
    {
        isWarning = !isWarning;
        string content = isWarning 
            ? "Status: WARNING"
            : "Status: OK";
        
        gauge.SetAnnotationValue(0, content, 0);
    }
}
```

**Use case:** Dynamic status displays, live value labels, contextual messages.

#### Synchronized Annotation and Pointer

```razor
@using Syncfusion.Blazor.LinearGauge

<button @onclick="IncreaseValue">Increase</button>
<button @onclick="DecreaseValue">Decrease</button>

<SfLinearGauge @ref="gauge">
    <LinearGaugeAnnotations>
        <LinearGaugeAnnotation AxisValue="0"
                               ZIndex="1"
                               Content="50%">
        </LinearGaugeAnnotation>
    </LinearGaugeAnnotations>

    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="50">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

<style>
    .value-display {
        font-size: 24px;
        font-weight: bold;
        color: #007bff;
        text-align: center;
    }
</style>

@code {
    private SfLinearGauge gauge;
    private double currentValue = 50;

    private void IncreaseValue()
    {
        if (currentValue < 100)
        {
            currentValue += 10;
            UpdateGauge();
        }
    }

    private void DecreaseValue()
    {
        if (currentValue > 0)
        {
            currentValue -= 10;
            UpdateGauge();
        }
    }

    private void UpdateGauge()
    {
        gauge.SetPointerValue(0, 0, currentValue);
        gauge.SetAnnotationValue(0, currentValue.ToString()+ "%", 0);
    }
}
```

### RefreshAsync

Refresh the entire gauge to reflect property changes.

**Syntax:**
```csharp
await gauge.RefreshAsync();
```

**Use case:** After making multiple configuration changes, refresh to apply all updates at once.

**Example:**

```razor
@using Syncfusion.Blazor.LinearGauge

<button @onclick="ChangeTheme">Change Theme</button>

<SfLinearGauge @ref="gauge" Background="@backgroundColor">
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="60" Color="@pointerColor">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    SfLinearGauge gauge;
    private string backgroundColor = "white";
    private string pointerColor = "#007bff";
    
    private async Task ChangeTheme()
    {
        backgroundColor = backgroundColor == "white" ? "#2c3e50" : "white";
        pointerColor = pointerColor == "#007bff" ? "#e74c3c" : "#007bff";
        
        await gauge.RefreshAsync();
    }
}
```

### PrintAsync

Print the gauge.

**Syntax:**
```csharp
await gauge.PrintAsync();
```

**Prerequisites:**
- Set `AllowPrint="true"` on the gauge

**Example:**

```razor
@using Syncfusion.Blazor.LinearGauge

<button @onclick="PrintGauge">Print</button>

<SfLinearGauge @ref="gauge" AllowPrint="true">
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="75"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    SfLinearGauge gauge;
    
    private async Task PrintGauge()
    {
        await gauge.PrintAsync();
    }
}
```

## Lifecycle Events

Events that fire during gauge initialization and loading.

### OnLoad

Fires before the gauge is rendered. Use to modify settings or cancel rendering.

**Event Arguments:** `LoadEventArgs`
- `Cancel` (bool): Set to `true` to cancel rendering

**Example:**

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeEvents OnLoad="OnLoad"></LinearGaugeEvents>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="60"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private void OnLoad(Syncfusion.Blazor.LinearGauge.LoadEventArgs args)
    {
        Console.WriteLine("Gauge is about to load");
        
        // Optionally cancel rendering based on condition
        // args.Cancel = true;
    }
}
```

**Use case:** Conditional rendering, logging, performance tracking.

### Loaded

Fires after the gauge has been successfully rendered.

**Event Arguments:** `LoadedEventArgs`

**Example:**

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeEvents Loaded="OnLoaded"></LinearGaugeEvents>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="60"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private void OnLoaded(Syncfusion.Blazor.LinearGauge.LoadedEventArgs args)
    {
        Console.WriteLine("Gauge loaded successfully");
        // Perform post-load operations
    }
}
```

**Use case:** Initialize data fetching, start timers, trigger animations.

## Rendering Events

Customize elements during the rendering process.

### AxisLabelRendering

Fires before each axis label is rendered. Use to customize label text or appearance.

**Event Arguments:** `AxisLabelRenderEventArgs`
- `Text` (string): Label text (can be modified)
- `AxisValue` (double): The numeric value of the label
- `Axis` (LinearGaugeAxis): The axis configuration

**Example:**

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeEvents AxisLabelRendering="OnAxisLabelRendering">
    </LinearGaugeEvents>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeMajorTicks Interval="20"></LinearGaugeMajorTicks>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="65"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private void OnAxisLabelRendering(Syncfusion.Blazor.LinearGauge.AxisLabelRenderEventArgs args)
    {
        // Add degree symbol to all labels
        args.Text = args.Text + "°C";
    }
}
```

#### Conditional Label Styling

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeEvents AxisLabelRendering="OnAxisLabelRendering">
    </LinearGaugeEvents>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugeMajorTicks Interval="25"></LinearGaugeMajorTicks>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="85"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private void OnAxisLabelRendering(Syncfusion.Blazor.LinearGauge.AxisLabelRenderEventArgs args)
    {
        // Highlight critical values
        if (args.AxisValue >= 75)
        {
            args.Text = $"⚠️ {args.Text}";
        }
    }
}
```

**Use case:** Custom units, conditional formatting, multilingual labels.

### AnnotationRendering

Fires before each annotation is rendered. Use to modify annotation content or position.

**Event Arguments:** `AnnotationRenderEventArgs`
- `Content` (string): Annotation HTML content (can be modified)
- `Annotation` (LinearGaugeAnnotation): The annotation configuration

**Example:**

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeEvents AnnotationRendering="OnAnnotationRendering">
    </LinearGaugeEvents>
    <LinearGaugeAnnotations>
        <LinearGaugeAnnotation AxisValue="0" ZIndex="1" Content="Value">
        </LinearGaugeAnnotation>
    </LinearGaugeAnnotations>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="@currentValue">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private double currentValue = 60;
    
    private void OnAnnotationRendering(Syncfusion.Blazor.LinearGauge.AnnotationRenderEventArgs args)
    {
        // Update annotation with current value
        args.Content = currentValue.ToString() + "%";
    }
}
```

**Use case:** Dynamic annotation content, conditional styling, data-driven annotations.

## User Interaction Events

Respond to user interactions with the gauge.

### ValueChange

Fires when a pointer value changes due to user interaction (dragging) or programmatic update.

**Event Arguments:** `ValueChangeEventArgs`
- `Value` (double): New pointer value
- `PointerIndex` (int): Index of the pointer
- `AxisIndex` (int): Index of the axis

**Example:**

```razor
@using Syncfusion.Blazor.LinearGauge

<p>Current Value: @currentValue</p>

<SfLinearGauge>
    <LinearGaugeEvents ValueChange="OnValueChange"></LinearGaugeEvents>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="50" 
                                    EnableDrag="true"
                                    AnimationDuration="0">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private double currentValue = 50;
    
    private void OnValueChange(Syncfusion.Blazor.LinearGauge.ValueChangeEventArgs args)
    {
        currentValue = Math.Round(args.Value, 2);
    }
}
```

**Use case:** Live value tracking, validation, synchronizing with other components.

#### Multiple Pointer Tracking

```razor
@using Syncfusion.Blazor.LinearGauge

<div>
    <p>Min: @minValue | Max: @maxValue | Avg: @avgValue</p>
</div>

<SfLinearGauge>
    <LinearGaugeEvents ValueChange="OnValueChange"></LinearGaugeEvents>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="30" 
                                    EnableDrag="true"
                                    Color="#3498db">
                </LinearGaugePointer>
                <LinearGaugePointer PointerValue="70" 
                                    EnableDrag="true"
                                    Color="#e74c3c">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private double minValue = 30;
    private double maxValue = 70;
    private double avgValue = 50;
    
    private void OnValueChange(Syncfusion.Blazor.LinearGauge.ValueChangeEventArgs args)
    {
        if (args.PointerIndex == 0)
        {
            minValue = args.Value;
        }
        else if (args.PointerIndex == 1)
        {
            maxValue = args.Value;
        }
        
        avgValue = Math.Round((minValue + maxValue) / 2, 2);
    }
}
```

### TooltipRendering

Fires before a tooltip is rendered. Use to customize tooltip content or format.

**Event Arguments:** `TooltipRenderEventArgs`
- `Content` (string): Tooltip content (can be modified)
- `Pointer` (LinearGaugePointer): The pointer configuration

**Example:**

```razor
@using Syncfusion.Blazor.LinearGauge

<SfLinearGauge>
    <LinearGaugeEvents TooltipRendering="OnTooltipRendering">
    </LinearGaugeEvents>
    <LinearGaugeTooltipSettings Enable="true">
    </LinearGaugeTooltipSettings>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="65" EnableDrag="true">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private void OnTooltipRendering(Syncfusion.Blazor.LinearGauge.TooltipRenderEventArgs args)
    {
        args.Content = $"Temperature: {args.Content}°C";
    }
}
```

**Use case:** Custom tooltip formatting, additional context, unit conversion.

### OnDragStart

Fires when user starts dragging a pointer.

**Event Arguments:** `PointerDragEventArgs`
- `PointerIndex` (int): Index of the pointer being dragged
- `AxisIndex` (int): Index of the axis
- `CurrentValue` (double): Pointer value at drag start

**Example:**

```razor
@using Syncfusion.Blazor.LinearGauge

<p>Status: @dragStatus</p>

<SfLinearGauge>
    <LinearGaugeEvents OnDragStart="OnDragStart" 
                       OnDragEnd="OnDragEnd">
    </LinearGaugeEvents>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="50" EnableDrag="true">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private string dragStatus = "Ready";
    
    private void OnDragStart(Syncfusion.Blazor.LinearGauge.PointerDragEventArgs args)
    {
        dragStatus = $"Dragging from {args.CurrentValue}";
    }
    
    private void OnDragEnd(Syncfusion.Blazor.LinearGauge.PointerDragEventArgs args)
    {
        dragStatus = $"Dropped at {args.CurrentValue}";
    }
}
```

**Use case:** Validation, logging, UI feedback during drag operations.

### OnDragEnd

Fires when user stops dragging a pointer.

**Event Arguments:** `PointerDragEventArgs`
- `CurrentValue` (double): Final pointer value after drag

**Example:**

```razor
@using Syncfusion.Blazor.LinearGauge

<p>Final Value: @finalValue</p>

<SfLinearGauge>
    <LinearGaugeEvents OnDragEnd="OnDragEnd"></LinearGaugeEvents>
    <LinearGaugeAxes>
        <LinearGaugeAxis Minimum="0" Maximum="100">
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="50" EnableDrag="true">
                </LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private double finalValue = 50;
    
    private void OnDragEnd(Syncfusion.Blazor.LinearGauge.PointerDragEventArgs args)
    {
        finalValue = Math.Round(args.CurrentValue, 2);
        
        // Save to database or perform validation
        SaveValue(finalValue);
    }
    
    private void SaveValue(double value)
    {
        // Implementation
        Console.WriteLine($"Saved value: {value}");
    }
}
```

**Use case:** Persist changes, trigger calculations, validation before commit.

## Utility Events

### Resizing

Fires after the gauge is resized.

**Event Arguments:** `ResizeEventArgs`
- `CurrentSize` (Size): New gauge dimensions
- `PreviousSize` (Size): Previous gauge dimensions

**Example:**

```razor
@using Syncfusion.Blazor.LinearGauge

<p>Size: @gaugeSize</p>

<SfLinearGauge Width="100%" Height="400px">
    <LinearGaugeEvents Resizing="OnResizeCompleted">
    </LinearGaugeEvents>
    <LinearGaugeAxes>
        <LinearGaugeAxis>
            <LinearGaugePointers>
                <LinearGaugePointer PointerValue="60"></LinearGaugePointer>
            </LinearGaugePointers>
        </LinearGaugeAxis>
    </LinearGaugeAxes>
</SfLinearGauge>

@code {
    private string gaugeSize = "Initial";
    
    private void OnResizeCompleted(Syncfusion.Blazor.LinearGauge.ResizeEventArgs args)
    {
        gaugeSize = $"{args.CurrentSize.Width} x {args.CurrentSize.Height}";
        Console.WriteLine($"Resized from {args.PreviousSize.Width} x {args.PreviousSize.Height} to {args.CurrentSize.Width} x {args.CurrentSize.Height}");
    }
}
```

**Use case:** Responsive adjustments, layout recalculations, logging.

## Complete Examples

### Real-Time Monitoring Dashboard

```razor
@using Syncfusion.Blazor.LinearGauge

@using System.Timers
@implements IDisposable

<div class="dashboard">
    <h3>System Monitor</h3>
    <div class="metrics">
        <span>CPU: @cpuValue%</span>
        <span>Status: @status</span>
    </div>
    
    <SfLinearGauge @ref="gauge" Width="300px" Height="400px">
        <LinearGaugeEvents 
            ValueChange="OnValueChange"
            AxisLabelRendering="OnAxisLabelRendering">
        </LinearGaugeEvents>
        
        <LinearGaugeAnnotations>
            <LinearGaugeAnnotation AxisValue="0" ZIndex="1" Content="35%">
            </LinearGaugeAnnotation>
        </LinearGaugeAnnotations>
        
        <LinearGaugeAxes>
            <LinearGaugeAxis Minimum="0" Maximum="100">
                <LinearGaugePointers>
                    <LinearGaugePointer PointerValue="@cpuValue" 
                                        Color="@pointerColor"
                                        AnimationDuration="1000">
                    </LinearGaugePointer>
                </LinearGaugePointers>
                
                <LinearGaugeRanges>
                    <LinearGaugeRange Start="0" End="50" Color="#28a745">
                    </LinearGaugeRange>
                    <LinearGaugeRange Start="50" End="75" Color="#ffc107">
                    </LinearGaugeRange>
                    <LinearGaugeRange Start="75" End="100" Color="#dc3545">
                    </LinearGaugeRange>
                </LinearGaugeRanges>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </SfLinearGauge>
</div>

<style>
    .dashboard {
        padding: 20px;
        background: #f8f9fa;
        border-radius: 8px;
    }
    .metrics {
        display: flex;
        justify-content: space-between;
        margin-bottom: 15px;
        font-weight: 600;
    }
    .gauge-label {
        font-size: 32px;
        font-weight: bold;
        color: #007bff;
    }
</style>

@code {
    SfLinearGauge gauge;
    private double cpuValue = 35;
    private string status = "Normal";
    private string pointerColor = "#28a745";
    private Timer timer;
    private Random random = new Random();
    
    protected override void OnInitialized()
    {
        timer = new Timer(3000);
        timer.Elapsed += (sender, e) => UpdateCpuValue();
        timer.Start();
    }
    
    private void UpdateCpuValue()
    {
        cpuValue = random.Next(0, 101);

        UpdateStatus();
        
        if (gauge != null)
        {
            gauge.SetPointerValue(0, 0, cpuValue);
            gauge.SetAnnotationValue(0, cpuValue.ToString() + "%", 0);
        }
        StateHasChanged();
    }
    
    private void OnValueChange(Syncfusion.Blazor.LinearGauge.ValueChangeEventArgs args)
    {
        cpuValue = Math.Round(args.Value, 1);
        UpdateStatus();
    }
    
    private void OnAxisLabelRendering(Syncfusion.Blazor.LinearGauge.AxisLabelRenderEventArgs args)
    {
        args.Text = args.Text + "%";
    }
    
    private void UpdateStatus()
    {
        if (cpuValue < 50)
        {
            status = "Normal";
            pointerColor = "#28a745";
        }
        else if (cpuValue < 75)
        {
            status = "Warning";
            pointerColor = "#ffc107";
        }
        else
        {
            status = "Critical";
            pointerColor = "#dc3545";
        }
    }
    
    public void Dispose()
    {
        timer?.Stop();
        timer?.Dispose();
    }
}
```

### Interactive Temperature Controller

```razor
@using Syncfusion.Blazor.LinearGauge

<div class="controller">
    <h3>Temperature Control</h3>
    <p class="status">Target: @targetTemp°C | Drag Status: @dragStatus</p>
    
    <SfLinearGauge @ref="gauge">
        <LinearGaugeEvents 
            OnDragStart="OnDragStart"
            OnDragEnd="OnDragEnd"
            TooltipRendering="OnTooltipRendering">
        </LinearGaugeEvents>
        
        <LinearGaugeTooltipSettings Enable="true" 
                                    Format="{value}°C">
        </LinearGaugeTooltipSettings>
        
        <LinearGaugeAxes>
            <LinearGaugeAxis Minimum="0" Maximum="50">
                <LinearGaugeAxisLabelStyle Format="{value}°C">
                </LinearGaugeAxisLabelStyle>
                <LinearGaugePointers>
                    <LinearGaugePointer PointerValue="@targetTemp" 
                                        EnableDrag="true"
                                        AnimationDuration="500">
                    </LinearGaugePointer>
                </LinearGaugePointers>
            </LinearGaugeAxis>
        </LinearGaugeAxes>
    </SfLinearGauge>
    
    <div class="controls">
        <button @onclick="() => AdjustTemp(-5)">-5°</button>
        <button @onclick="() => AdjustTemp(5)">+5°</button>
        <button @onclick="Reset">Reset</button>
    </div>
</div>

<style>
    .controller {
        max-width: 400px;
        padding: 20px;
        background: white;
        border-radius: 8px;
        box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    }
    .status {
        color: #6c757d;
        font-size: 14px;
    }
    .controls {
        display: flex;
        gap: 10px;
        margin-top: 15px;
    }
    .controls button {
        flex: 1;
        padding: 10px;
        background: #007bff;
        color: white;
        border: none;
        border-radius: 4px;
        cursor: pointer;
    }
</style>

@code {
    SfLinearGauge gauge;
    private double targetTemp = 22;
    private string dragStatus = "Ready";
    private double initialTemp;
    
    private void OnDragStart(Syncfusion.Blazor.LinearGauge.PointerDragEventArgs args)
    {
        initialTemp = args.CurrentValue;
        dragStatus = "Adjusting...";
    }
    
    private void OnDragEnd(Syncfusion.Blazor.LinearGauge.PointerDragEventArgs args)
    {
        targetTemp = Math.Round(args.CurrentValue, 1);
        dragStatus = $"Changed from {initialTemp}°C to {targetTemp}°C";
        
        // Send to thermostat
        Console.WriteLine($"Setting temperature to {targetTemp}°C");
    }
    
    private void OnTooltipRendering(Syncfusion.Blazor.LinearGauge.TooltipRenderEventArgs args)
    {
        args.Content = $"Set to {args.Content}";
    }
    
    private void AdjustTemp(double delta)
    {
        targetTemp = Math.Max(0, Math.Min(50, targetTemp + delta));
        gauge.SetPointerValue(0, 0, targetTemp);
        dragStatus = $"Adjusted to {targetTemp}°C";
    }
    
    private void Reset()
    {
        targetTemp = 22;
        gauge.SetPointerValue(0, 0, targetTemp);
        dragStatus = "Reset to default";
    }
}
```

## Key Takeaways

**Methods:**
1. **SetPointerValue** - Update pointer values programmatically
2. **SetAnnotationValue** - Update annotation content dynamically
3. **RefreshAsync** - Refresh gauge after configuration changes
4. **PrintAsync** - Print the gauge

**Events:**
5. **OnLoad/Loaded** - Lifecycle hooks for initialization
6. **AxisLabelRendering** - Customize axis labels during render
7. **AnnotationRendering** - Modify annotations before display
8. **ValueChange** - Respond to pointer value changes
9. **TooltipRendering** - Customize tooltip content
10. **OnDragStart/OnDragEnd** - Handle pointer drag interactions
12. **Resizing** - Handle responsive behavior

Use these methods and events to create interactive, responsive, and data-driven gauge implementations.
