# Advanced Features

## Table of Contents
- [Overview](#overview)
- [Methods](#methods)
- [Accessibility](#accessibility)
- [Real-Time Data Updates](#real-time-data-updates)
- [Placing Gauges Inside Other Components](#placing-gauges-inside-other-components)
- [Performance Optimization](#performance-optimization)
- [Testing and Debugging](#testing-and-debugging)

## Overview

This guide covers advanced CircularGauge features including programmatic control via methods, accessibility compliance, real-time data integration, embedding gauges in complex layouts, and performance optimization techniques.

## Methods

The CircularGauge provides methods for programmatic control of gauge behavior and values.

### SetPointerValueAsync

Update pointer values programmatically with animation.

**Syntax:**
```csharp
await gaugeInstance.SetPointerValueAsync(axisIndex, pointerIndex, value);
```

**Example:**
```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge @ref="gauge">
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="@pointerValue">
                    <CircularGaugePointerAnimation Enable="true" Duration="1000">
                    </CircularGaugePointerAnimation>
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

<button @onclick="UpdatePointer">Update Pointer</button>

@code {
    SfCircularGauge gauge;
    private double pointerValue = 50;
    
    async Task UpdatePointer()
    {
        double newValue = new Random().Next(0, 100);
        await gauge.SetPointerValueAsync(0, 0, newValue);
        pointerValue = newValue;
    }
}
```

**Parameters:**
- `axisIndex` (int): Zero-based index of the axis (0 for first axis)
- `pointerIndex` (int): Zero-based index of the pointer (0 for first pointer)
- `value` (double): New pointer value

**Use Cases:**
- Real-time sensor data updates
- User input validation and adjustment
- Animated value transitions
- Dashboard updates from API calls

### SetAnnotationValueAsync

Update annotation content dynamically.

**Syntax:**
```csharp
await gaugeInstance.SetAnnotationValueAsync(axisIndex, annotationIndex, content);
```

**Example:**
```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge @ref="gauge">
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="@currentValue">
                </CircularGaugePointer>
            </CircularGaugePointers>
            <CircularGaugeAnnotations>
                <CircularGaugeAnnotation Angle="0" Radius="0%">
                    <ContentTemplate>
                        <div id="annotation-value">@currentValue%</div>
                    </ContentTemplate>
                </CircularGaugeAnnotation>
            </CircularGaugeAnnotations>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

<button @onclick="UpdateAnnotation">Update Label</button>

@code {
    SfCircularGauge gauge;
    private double currentValue = 65;
    
    async Task UpdateAnnotation()
    {
        currentValue = new Random().Next(0, 100);
        await gauge.SetAnnotationValueAsync(0, 0, $"<div>{currentValue}%</div>");
        await gauge.SetPointerValueAsync(0, 0, currentValue);
    }
}
```

**Parameters:**
- `axisIndex` (int): Zero-based index of the axis
- `annotationIndex` (int): Zero-based index of the annotation
- `content` (string): New HTML content for the annotation

**Use Cases:**
- Dynamic label updates
- Status message changes
- Custom formatted values
- Conditional content display

### SetRangeValue

Adjust range boundaries programmatically.

**Syntax:**
```csharp
gaugeInstance.SetRangeValue(axisIndex, rangeIndex, start, end);
```

**Example:**
```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge @ref="gauge">
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeRanges>
                <CircularGaugeRange 
                    Start="@redZoneStart" 
                    End="@redZoneEnd" 
                    Color="#FF6B6B">
                </CircularGaugeRange>
            </CircularGaugeRanges>
            <CircularGaugePointers>
                <CircularGaugePointer Value="75">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

<button @onclick="AdjustRedZone">Adjust Danger Zone</button>

@code {
    SfCircularGauge gauge;
    private double redZoneStart = 70;
    private double redZoneEnd = 100;
    
    void AdjustRedZone()
    {
        redZoneStart = 60;
        redZoneEnd = 100;
        gauge.SetRangeValue(0, 0, redZoneStart, redZoneEnd);
    }
}
```

**Parameters:**
- `axisIndex` (int): Zero-based index of the axis
- `rangeIndex` (int): Zero-based index of the range
- `start` (double): New start value
- `end` (double): New end value

**Use Cases:**
- Dynamic threshold adjustments
- User-defined safe zones
- Conditional range updates
- Configuration-based ranges

### RefreshAsync

Force gauge re-render when container size changes.

**Syntax:**
```csharp
await gaugeInstance.RefreshAsync();
```

**Example:**
```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge @ref="gauge" Width="@gaugeWidth" Height="@gaugeHeight">
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="65">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

<button @onclick="ResizeGauge">Resize Gauge</button>

@code {
    SfCircularGauge gauge;
    private string gaugeWidth = "300px";
    private string gaugeHeight = "300px";
    
    async Task ResizeGauge()
    {
        gaugeWidth = "500px";
        gaugeHeight = "500px";
        StateHasChanged();
        await Task.Delay(100); // Wait for DOM update
        await gauge.RefreshAsync();
    }
}
```

**Use Cases:**
- Responsive design updates
- Container resize handling
- Tab/Dialog visibility changes
- Dynamic layout adjustments

### Complete Methods Example

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge @ref="gauge">
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugePointers>
                <CircularGaugePointer Value="@temperature">
                    <CircularGaugePointerAnimation Enable="true" Duration="500">
                    </CircularGaugePointerAnimation>
                </CircularGaugePointer>
            </CircularGaugePointers>
            <CircularGaugeRanges>
                <CircularGaugeRange 
                    Start="@safeStart" 
                    End="@safeEnd" 
                    Color="#30B32D">
                </CircularGaugeRange>
                <CircularGaugeRange 
                    Start="@dangerStart" 
                    End="@dangerEnd" 
                    Color="#FF6B6B">
                </CircularGaugeRange>
            </CircularGaugeRanges>
            <CircularGaugeAnnotations>
                <CircularGaugeAnnotation Angle="0" Radius="0%">
                    <ContentTemplate>
                        <div>@temperature°C</div>
                    </ContentTemplate>
                </CircularGaugeAnnotation>
            </CircularGaugeAnnotations>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

<div>
    <button @onclick="SimulateSensorUpdate">Simulate Sensor</button>
    <button @onclick="AdjustThresholds">Adjust Thresholds</button>
</div>

@code {
    SfCircularGauge gauge;
    private double temperature = 22;
    private double safeStart = 18, safeEnd = 26;
    private double dangerStart = 26, dangerEnd = 100;
    
    async Task SimulateSensorUpdate()
    {
        temperature = new Random().Next(15, 35);
        await gauge.SetPointerValueAsync(0, 0, temperature);
        await gauge.SetAnnotationValueAsync(0, 0, $"<div>{temperature}°C</div>");
    }
    
    void AdjustThresholds()
    {
        safeStart = 20;
        safeEnd = 24;
        dangerStart = 24;
        dangerEnd = 100;
        gauge.SetRangeValue(0, 0, safeStart, safeEnd);
        gauge.SetRangeValue(0, 1, dangerStart, dangerEnd);
    }
}
```

## Accessibility

The CircularGauge is designed for accessibility compliance.

### WCAG 2.2 AA Compliance

**Standards Supported:**
- **WCAG 2.2 Level AA**: Yes
- **Section 508**: Partial support
- **Screen Reader Support**: Yes
- **Right-to-Left Support**: Yes
- **Color Contrast**: Must be manually ensured
- **Keyboard Navigation**: Not applicable (read-only visualization)

### Screen Reader Support

The gauge automatically includes WAI-ARIA attributes:

```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="75" Description="Temperature gauge at 75 degrees">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Generated HTML includes:**
- `role="region"`: Identifies gauge as a landmark
- `aria-label`: Provides accessible name
- `aria-describedby`: Links to description text

### Best Practices for Accessibility

**1. Provide Meaningful Titles**
```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge Title="Server CPU Usage">
    <CircularGaugeTitleStyle Size="20px" />
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="65">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**2. Ensure Color Contrast**
- Use high-contrast colors (4.5:1 ratio minimum)
- Don't rely solely on color for information
- Add text labels alongside color indicators

```cshtml
<CircularGaugeRange Start="0" End="50" Color="#30B32D">
</CircularGaugeRange>
<CircularGaugeAnnotation Angle="45" Radius="70%">
    <ContentTemplate>
        <div>Safe (0-50)</div>
    </ContentTemplate>
</CircularGaugeAnnotation>
```

**3. Add Text Alternatives**
```cshtml
@using Syncfusion.Blazor.CircularGauge
<div aria-label="Temperature gauge showing 22 degrees Celsius, within safe range of 18-26 degrees">
    <SfCircularGauge>
        <CircularGaugeAxes>
            <CircularGaugeAxis Minimum="0" Maximum="40">
                <CircularGaugePointers>
                    <CircularGaugePointer Value="22">
                    </CircularGaugePointer>
                </CircularGaugePointers>
            </CircularGaugeAxis>
        </CircularGaugeAxes>
    </SfCircularGauge>
</div>
<p class="sr-only">Current temperature: 22°C (Safe range: 18-26°C)</p>
```

**4. Support RTL for International Users**
```cshtml
@using Syncfusion.Blazor.CircularGauge
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="60">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Real-Time Data Updates

Integrate CircularGauge with live data sources.

### SignalR Integration

```cshtml
@using Microsoft.AspNetCore.SignalR.Client
@using Syncfusion.Blazor.CircularGauge
@inject NavigationManager Navigation
@implements IAsyncDisposable

<SfCircularGauge @ref="gauge">
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugeAxisLabelStyle Format="{value}%">
            </CircularGaugeAxisLabelStyle>
            <CircularGaugePointers>
                <CircularGaugePointer Value="@cpuUsage">
                    <CircularGaugePointerAnimation Enable="true" Duration="300">
                    </CircularGaugePointerAnimation>
                </CircularGaugePointer>
            </CircularGaugePointers>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="0" End="60" Color="#30B32D">
                </CircularGaugeRange>
                <CircularGaugeRange Start="60" End="85" Color="#FFC107">
                </CircularGaugeRange>
                <CircularGaugeRange Start="85" End="100" Color="#FF6B6B">
                </CircularGaugeRange>
            </CircularGaugeRanges>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

<div>CPU Usage: @cpuUsage%</div>

@code {
    SfCircularGauge gauge;
    private HubConnection hubConnection;
    private double cpuUsage = 0;
    
    protected override async Task OnInitializedAsync()
    {
        hubConnection = new HubConnectionBuilder()
            .WithUrl(Navigation.ToAbsoluteUri("/serverhub"))
            .Build();
        
        hubConnection.On<double>("ReceiveCPUUpdate", async (usage) =>
        {
            cpuUsage = usage;
            await gauge.SetPointerValueAsync(0, 0, cpuUsage);
            StateHasChanged();
        });
        
        await hubConnection.StartAsync();
    }
    
    public async ValueTask DisposeAsync()
    {
        if (hubConnection is not null)
        {
            await hubConnection.DisposeAsync();
        }
    }
}
```

### Timer-Based Updates

```cshtml
@implements IDisposable
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge @ref="gauge">
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="120">
            <CircularGaugePointers>
                <CircularGaugePointer Value="@speed">
                    <CircularGaugePointerAnimation Enable="true" Duration="500">
                    </CircularGaugePointerAnimation>
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

@code {
    SfCircularGauge gauge;
    private System.Threading.Timer timer;
    private double speed = 0;
    private Random random = new Random();
    
    protected override void OnInitialized()
    {
        timer = new System.Threading.Timer(async _ =>
        {
            speed = random.Next(40, 100);
            await InvokeAsync(async () =>
            {
                await gauge.SetPointerValueAsync(0, 0, speed);
                StateHasChanged();
            });
        }, null, 0, 2000); // Update every 2 seconds
    }
    
    public void Dispose()
    {
        timer?.Dispose();
    }
}
```

### HTTP Polling

```cshtml
@inject HttpClient Http
@implements IDisposable
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge @ref="gauge">
    <CircularGaugeAxes>
        <CircularGaugeAxis Minimum="0" Maximum="100">
            <CircularGaugePointers>
                <CircularGaugePointer Value="@temperatureValue">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

@code {
    SfCircularGauge gauge;
    private System.Threading.Timer timer;
    private double temperatureValue = 20;
    
    protected override void OnInitialized()
    {
        timer = new System.Threading.Timer(async _ =>
        {
            var data = await Http.GetFromJsonAsync<SensorData>("api/sensors/temperature");
            temperatureValue = data.Value;
            await InvokeAsync(async () =>
            {
                await gauge.SetPointerValueAsync(0, 0, temperatureValue);
                StateHasChanged();
            });
        }, null, 0, 5000); // Poll every 5 seconds
    }
    
    public void Dispose()
    {
        timer?.Dispose();
    }
    
    public class SensorData
    {
        public double Value { get; set; }
    }
}
```

## Placing Gauges Inside Other Components

Special considerations when embedding gauges in dynamic containers.

### Dashboard Layout

Use boolean flag and `RefreshAsync` for proper rendering:

```cshtml
@using Syncfusion.Blazor.Layouts
@using Syncfusion.Blazor.CircularGauge

<SfDashboardLayout AllowResizing="true" Columns="20">
    <DashboardLayoutEvents Created="OnCreated" OnWindowResize="OnResize">
    </DashboardLayoutEvents>
    <DashboardLayoutPanels>
        <DashboardLayoutPanel Row="0" Column="0" SizeX="10" SizeY="5">
            <HeaderTemplate><div>CPU Usage</div></HeaderTemplate>
            <ContentTemplate>
                    <SfCircularGauge @ref="gauge" Width="100%" Height="100%">
                        <CircularGaugeAxes>
                            <CircularGaugeAxis>
                                <CircularGaugePointers>
                                    <CircularGaugePointer Value="65">
                                    </CircularGaugePointer>
                                </CircularGaugePointers>
                            </CircularGaugeAxis>
                        </CircularGaugeAxes>
                    </SfCircularGauge>
            </ContentTemplate>
        </DashboardLayoutPanel>
    </DashboardLayoutPanels>
</SfDashboardLayout>

@code {
    SfCircularGauge gauge;
    private System.Threading.Timer resizeTimer;
    
    async void OnCreated()
    {
        await Task.Yield();
    }
    
    async Task OnResize(ResizeArgs args)
    {
        if (resizeTimer != null)
        {
            resizeTimer.Dispose();
        }
        resizeTimer = new System.Threading.Timer(async _ =>
        {
            await InvokeAsync(async () =>
            {
                await gauge.RefreshAsync();
            });
        }, null, 500, System.Threading.Timeout.Infinite);
    }

    
    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            await Task.Delay(50);
            if (gauge != null)
            {
                await gauge.RefreshAsync();
            }
        }
    }

}
```

### Tab Component

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.CircularGauge

<SfTab>
    <TabEvents Created="OnTabCreated"></TabEvents>
    <TabItems>
        <TabItem>
            <ChildContent>
                <TabHeader Text="Gauge"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                @if (isInitialRender)
                {
                    <SfCircularGauge>
                        <CircularGaugeAxes>
                            <CircularGaugeAxis>
                                <CircularGaugePointers>
                                    <CircularGaugePointer Value="75">
                                    </CircularGaugePointer>
                                </CircularGaugePointers>
                            </CircularGaugeAxis>
                        </CircularGaugeAxes>
                    </SfCircularGauge>
                }
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private bool isInitialRender = false;
    
    void OnTabCreated()
    {
        isInitialRender = true;
    }
}
```

### Dialog Component

```cshtml
@using Syncfusion.Blazor.Popups
@using Syncfusion.Blazor.CircularGauge

<SfDialog @bind-Visible="dialogVisible" Width="400px" Height="400px" EnableResize="true">
    <DialogEvents Opened="OnDialogOpened" Closed="OnDialogClosed" OnResizeStop="OnDialogResize">
    </DialogEvents>
    <DialogTemplates>
        <Header>Gauge</Header>
        <Content>
            @if (isInitialRender)
            {
                <SfCircularGauge @ref="gauge" Width="100%" Height="100%">
                    <CircularGaugeAxes>
                        <CircularGaugeAxis>
                            <CircularGaugePointers>
                                <CircularGaugePointer Value="50">
                                </CircularGaugePointer>
                            </CircularGaugePointers>
                        </CircularGaugeAxis>
                    </CircularGaugeAxes>
                </SfCircularGauge>
            }
        </Content>
    </DialogTemplates>
</SfDialog>

@code {
    SfCircularGauge gauge;
    private bool dialogVisible = true;
    private bool isInitialRender = false;
    
    void OnDialogOpened()
    {
        isInitialRender = true;
    }
    
    void OnDialogClosed()
    {
        isInitialRender = false;
    }
    
    async Task OnDialogResize(Microsoft.AspNetCore.Components.Web.MouseEventArgs args)
    {
        await Task.Delay(100);
        await gauge.RefreshAsync();
    }
}
```

### Accordion Component

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.CircularGauge

<SfAccordion>
    <AccordionEvents Expanded="OnAccordionExpanded">
    </AccordionEvents>
    <AccordionItems>
        <AccordionItem Expanded="true">
            <HeaderTemplate>Gauge Panel</HeaderTemplate>
            <ContentTemplate>
                    <SfCircularGauge @ref="gauge">
                        <CircularGaugeAxes>
                            <CircularGaugeAxis>
                                <CircularGaugePointers>
                                    <CircularGaugePointer Value="80">
                                    </CircularGaugePointer>
                                </CircularGaugePointers>
                            </CircularGaugeAxis>
                        </CircularGaugeAxes>
                    </SfCircularGauge>
            </ContentTemplate>
        </AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    SfCircularGauge gauge;
    
    async Task OnAccordionExpanded(ExpandedEventArgs args)
    {
        await Task.Delay(100);
        await gauge.RefreshAsync();
    }
}
```

## Performance Optimization

### Disable Unnecessary Features

```cshtml
@using Syncfusion.Blazor.CircularGauge
<!-- Disable animation for rapidly updating gauges -->
<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="20">
                    <CircularGaugePointerAnimation Enable="false">
                    </CircularGaugePointerAnimation>
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Debounce Rapid Updates

```csharp
private System.Threading.Timer debounceTimer;
private double pendingValue;

void UpdateGaugeValue(double newValue)
{
    pendingValue = newValue;
    
    if (debounceTimer != null)
    {
        debounceTimer.Dispose();
    }
    
    debounceTimer = new System.Threading.Timer(async _ =>
    {
        await InvokeAsync(async () =>
        {
            await gauge.SetPointerValueAsync(0, 0, pendingValue);
            StateHasChanged();
        });
    }, null, 300, System.Threading.Timeout.Infinite); // 300ms debounce
}
```

### Use Appropriate Axis Ranges

```cshtml
@using Syncfusion.Blazor.CircularGauge
<!-- Bad: Excessive precision -->
<CircularGaugeAxis Minimum="0" Maximum="100">
    <CircularGaugeAxisMajorTicks Interval="1" />
    <CircularGaugeAxisMinorTicks Interval="0.1" />
</CircularGaugeAxis>

<!-- Good: Reasonable precision -->
<CircularGaugeAxis Minimum="0" Maximum="100">
    <CircularGaugeAxisMajorTicks Interval="10" />
    <CircularGaugeAxisMinorTicks Interval="5" />
</CircularGaugeAxis>
```

## Testing and Debugging

### Unit Testing Gauge Updates

```csharp
[Fact]
public async Task UpdatePointer_ShouldChangeValue()
{
    // Arrange
    var ctx = new TestContext();
    var component = ctx.RenderComponent<GaugeComponent>();
    var gauge = component.Find("SfCircularGauge");
    
    // Act
    await component.Instance.UpdatePointerValue(75);
    
    // Assert
    Assert.Equal(75, component.Instance.CurrentValue);
}
```

### Browser DevTools Inspection

```cshtml
@using Syncfusion.Blazor.CircularGauge

<!-- Add ID for debugging -->
<SfCircularGauge ID="debugGauge">
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="@debugValue">
                </CircularGaugePointer>
            </CircularGaugePointers>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

@code {
    private double debugValue = 50;
    
    protected override void OnAfterRender(bool firstRender)
    {
        Console.WriteLine($"Gauge rendered with value: {debugValue}");
    }
}
```

### Common Debugging Scenarios

**Gauge not rendering:**
- Check browser console for errors
- Verify Syncfusion license is configured
- Ensure CSS is loaded
- Verify component reference is not null

**Values not updating:**
- Check `SetPointerValueAsync` is awaited
- Verify `StateHasChanged()` is called
- Ensure axis min/max accommodate values
- Check for JavaScript errors

**Performance issues:**
- Disable animation for real-time data
- Reduce update frequency
- Simplify gauge design (fewer ticks, ranges)
- Profile with browser DevTools

## Best Practices Summary

### Methods
- Always await async methods
- Use SetPointerValueAsync for animated updates
- Call RefreshAsync after container resize
- Index parameters are zero-based

### Accessibility
- Provide meaningful titles
- Ensure 4.5:1 color contrast
- Add text alternatives
- Support RTL for international users

### Real-Time Data
- Use SignalR for server push
- Debounce rapid updates (300-500ms)
- Disable animation for frequent updates
- Handle connection errors gracefully

### Container Integration
- Use boolean flag for initial render
- Call Task.Yield() before setting flag
- Refresh on resize/expand events
- Add 100-500ms delay before refresh

### Performance
- Disable unnecessary animations
- Use reasonable axis precision
- Debounce rapid updates
- Profile and optimize as needed
