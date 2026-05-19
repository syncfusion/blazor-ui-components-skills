# Events and Accessibility

This comprehensive guide covers event handling for interactive Bullet Charts and accessibility features to ensure WCAG compliance and inclusive design.

## Table of Contents
- [Events Overview](#events-overview)
- [Component Lifecycle Events](#component-lifecycle-events)
- [User Interaction Events](#user-interaction-events)
- [Rendering Events](#rendering-events)
- [Event Handling Patterns](#event-handling-patterns)
- [Accessibility Features](#accessibility-features)
- [WCAG Compliance](#wcag-compliance)
- [Testing Checklist](#testing-checklist)
- [Best Practices](#best-practices)

## Events Overview

Bullet Chart events allow you to respond to user interactions and component lifecycle changes. All events are configured through the `BulletChartEvents` component.

### Basic Event Setup

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota" 
               Title="Sales Performance">
    <BulletChartEvents Loaded="OnChartLoaded"></BulletChartEvents>
    <BulletChartRangeCollection>
        <BulletChartRange End="200"></BulletChartRange>
        <BulletChartRange End="350"></BulletChartRange>
        <BulletChartRange End="500"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class SalesMetric
    {
        public double Sales { get; set; }
        public double Quota { get; set; }
    }
    
    public List<SalesMetric> SalesData = new List<SalesMetric>
    {
        new SalesMetric { Sales = 380, Quota = 350 }
    };
    
    private void OnChartLoaded(EventArgs args)
    {
        Console.WriteLine("Chart has finished loading");
    }
}
```

## Component Lifecycle Events

### Loaded Event

Triggers after the chart completes rendering. Use for initialization tasks, logging, or triggering dependent operations.

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@PerformanceData" 
               ValueField="Score" 
               TargetField="Goal" 
               CategoryField="Metric"
               Height="400">
    <BulletChartEvents Loaded="OnLoadedHandler"></BulletChartEvents>
    <BulletChartRangeCollection>
        <BulletChartRange End="40"></BulletChartRange>
        <BulletChartRange End="70"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class PerformanceMetric
    {
        public string Metric { get; set; }
        public double Score { get; set; }
        public double Goal { get; set; }
    }
    
    public List<PerformanceMetric> PerformanceData = new List<PerformanceMetric>
    {
        new PerformanceMetric { Metric = "Sales", Score = 88, Goal = 85 },
        new PerformanceMetric { Metric = "Quality", Score = 76, Goal = 80 },
        new PerformanceMetric { Metric = "Support", Score = 92, Goal = 90 }
    };
    
    private void OnLoadedHandler(EventArgs args)
    {
        // Log chart load time
        Console.WriteLine($"Chart loaded at: {DateTime.Now:HH:mm:ss.fff}");
        
        // Trigger analytics
        // await AnalyticsService.TrackEvent("BulletChartViewed");
        
        // Update status indicator
        isChartReady = true;
        StateHasChanged();
    }
    
    private bool isChartReady = false;
}
```

### OnPrintComplete Event

Triggers before the chart is sent to the printer. Use to customize print output or track print actions.

```razor
@using Syncfusion.Blazor.Charts

<button class="btn btn-primary" @onclick="PrintChart">Print Chart</button>

<SfBulletChart @ref="bulletChartRef" 
               DataSource="@RevenueData" 
               ValueField="Revenue" 
               TargetField="Target" 
               Title="Revenue Performance">
    <BulletChartEvents OnPrintComplete="OnPrintCompleteHandler"></BulletChartEvents>
    <BulletChartRangeCollection>
        <BulletChartRange End="150"></BulletChartRange>
        <BulletChartRange End="250"></BulletChartRange>
        <BulletChartRange End="300"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    private SfBulletChart<RevenueMetric> bulletChartRef;
    
    public class RevenueMetric
    {
        public double Revenue { get; set; }
        public double Target { get; set; }
    }
    
    public List<RevenueMetric> RevenueData = new List<RevenueMetric>
    {
        new RevenueMetric { Revenue = 270, Target = 250 }
    };
    
    private async Task PrintChart()
    {
        await bulletChartRef.PrintAsync();
    }
    
    private void OnPrintCompleteHandler(PrintEventArgs args)
    {
        // Log print action
        Console.WriteLine($"Chart printed at: {DateTime.Now}");
        
        // Track analytics
        // await AnalyticsService.TrackPrintEvent("BulletChart", "Revenue");
        
        // Optional: Cancel print
        // args.Cancel = true;
    }
}
```

## User Interaction Events

### PointerClick Event

Fires when user clicks or taps on the actual bar or target marker. Useful for drill-down scenarios or detail views.

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@DepartmentData" 
               ValueField="Performance" 
               TargetField="Target" 
               CategoryField="Department"
               Height="400">
    <BulletChartEvents PointerClick="OnPointerClickHandler"></BulletChartEvents>
    <BulletChartRangeCollection>
        <BulletChartRange End="40"></BulletChartRange>
        <BulletChartRange End="70"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@if (!string.IsNullOrEmpty(selectedDepartment))
{
    <div class="alert alert-info mt-3">
        <strong>Selected:</strong> @selectedDepartment<br />
        <strong>Performance:</strong> @selectedPerformance<br />
        <strong>Target:</strong> @selectedTarget
    </div>
}

@code {
    public class DepartmentMetric
    {
        public string Department { get; set; }
        public double Performance { get; set; }
        public double Target { get; set; }
    }
    
    public List<DepartmentMetric> DepartmentData = new List<DepartmentMetric>
    {
        new DepartmentMetric { Department = "Sales", Performance = 88, Target = 85 },
        new DepartmentMetric { Department = "Marketing", Performance = 76, Target = 80 },
        new DepartmentMetric { Department = "Support", Performance = 92, Target = 90 }
    };
    
    private string selectedDepartment = "";
    private double selectedPerformance = 0;
    private double selectedTarget = 0;
    
    private void OnPointerClickHandler(BulletChartPointEventArgs args)
    {
        selectedDepartment = args.CategoryName;
        selectedPerformance = args.Value;
        selectedTarget = args.Target[0];
        
        Console.WriteLine($"Clicked: {selectedDepartment} - Performance: {selectedPerformance}");
        
        // Navigate to detail page
        // NavigationManager.NavigateTo($"/department/{selectedDepartment}");
    }
}
```

### Drill-Down Navigation Pattern

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@RegionalSales" 
               ValueField="Sales" 
               TargetField="Quota" 
               CategoryField="Region"
               Height="400">
    <BulletChartEvents PointerClick="OnRegionClick"></BulletChartEvents>
    <BulletChartRangeCollection>
        <BulletChartRange End="200"></BulletChartRange>
        <BulletChartRange End="350"></BulletChartRange>
        <BulletChartRange End="500"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    [Inject] private NavigationManager NavManager { get; set; }
    
    public class RegionalSalesData
    {
        public string Region { get; set; }
        public double Sales { get; set; }
        public double Quota { get; set; }
    }
    
    public List<RegionalSalesData> RegionalSales = new List<RegionalSalesData>
    {
        new RegionalSalesData { Region = "North", Sales = 380, Quota = 350 },
        new RegionalSalesData { Region = "South", Sales = 420, Quota = 400 },
        new RegionalSalesData { Region = "East", Sales = 310, Quota = 350 }
    };
    
    private void OnRegionClick(BulletChartPointEventArgs args)
    {
        var region = args.CategoryName;
        NavManager.NavigateTo($"/sales-details/{region}");
    }
}
```

## Rendering Events

### TooltipRender Event

Customize tooltip content or cancel tooltip display before rendering.

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota" 
               CategoryField="Product"
               Height="400">
    <BulletChartEvents TooltipRender="OnTooltipRenderHandler"></BulletChartEvents>
    <BulletChartTooltip TValue="SalesMetric" Enable="true"></BulletChartTooltip>
    <BulletChartRangeCollection>
        <BulletChartRange End="200"></BulletChartRange>
        <BulletChartRange End="350"></BulletChartRange>
        <BulletChartRange End="500"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class SalesMetric
    {
        public string Product { get; set; }
        public double Sales { get; set; }
        public double Quota { get; set; }
    }
    
    public List<SalesMetric> SalesData = new List<SalesMetric>
    {
        new SalesMetric { Product = "Product A", Sales = 380, Quota = 350 },
        new SalesMetric { Product = "Product B", Sales = 420, Quota = 400 },
        new SalesMetric { Product = "Product C", Sales = 310, Quota = 350 }
    };
    
    private void OnTooltipRenderHandler(BulletChartTooltipEventArgs args)
    {
        // Customize tooltip text
        var achievement = (args.Value / args.Target[0]) * 100;
        args.Text = new string[]
        {
            $"Sales: ${args.Value}K",
            $"Quota: ${args.Target[0]}K",
            $"Achievement: {achievement:F1}%"
        };
        
        // Conditionally hide tooltip
        // if (args.Value < 100) args.Cancel = true;
        
        Console.WriteLine($"Tooltip displayed for value: {args.Value}");
    }
}
```

### LegendRender Event

Modify legend items before they are rendered.

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@PerformanceData" 
               ValueField="Score" 
               TargetField="Goal" 
               Height="350">
    <BulletChartEvents LegendRender="OnLegendRenderHandler"></BulletChartEvents>
    <BulletChartLegendSettings Visible="true"></BulletChartLegendSettings>
    <BulletChartRangeCollection>
        <BulletChartRange End="40" Name="Poor" Color="#d32f2f"></BulletChartRange>
        <BulletChartRange End="70" Name="Satisfactory" Color="#ffa726"></BulletChartRange>
        <BulletChartRange End="100" Name="Excellent" Color="#66bb6a"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class PerformanceMetric
    {
        public double Score { get; set; }
        public double Goal { get; set; }
    }
    
    public List<PerformanceMetric> PerformanceData = new List<PerformanceMetric>
    {
        new PerformanceMetric { Score = 75, Goal = 70 }
    };
    
    private void OnLegendRenderHandler(BulletChartLegendRenderEventArgs args)
    {
        // Add emoji indicators to legend text
        if (args.Text == "Poor")
            args.Text = "🔴 Poor Performance";
        else if (args.Text == "Satisfactory")
            args.Text = "🟡 Satisfactory";
        else if (args.Text == "Excellent")
            args.Text = "🟢 Excellent";
        
        // Change legend shape
        // args.Shape = LegendShape.Diamond;
        
        // Modify legend color
        // args.Fill = "#custom-color";
        
        Console.WriteLine($"Rendering legend: {args.Text}");
    }
}
```

## Event Handling Patterns

### Comprehensive Event Logging

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@MetricsData" 
               ValueField="Value" 
               TargetField="Target" 
               CategoryField="Metric"
               Height="400">
    <BulletChartEvents Loaded="OnLoaded"
                       PointerClick="OnPointerClick"
                       TooltipRender="OnTooltipRender"
                       LegendRender="OnLegendRender"
                       OnPrintComplete="OnPrintComplete"></BulletChartEvents>
    <BulletChartTooltip TValue="MetricData" Enable="true"></BulletChartTooltip>
    <BulletChartLegendSettings Visible="true"></BulletChartLegendSettings>
    <BulletChartRangeCollection>
        <BulletChartRange End="40" Name="Low"></BulletChartRange>
        <BulletChartRange End="70" Name="Medium"></BulletChartRange>
        <BulletChartRange End="100" Name="High"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

<div class="mt-3">
    <h4>Event Log</h4>
    <ul>
        @foreach (var log in eventLogs.TakeLast(10))
        {
            <li>@log</li>
        }
    </ul>
</div>

@code {
    public class MetricData
    {
        public string Metric { get; set; }
        public double Value { get; set; }
        public double Target { get; set; }
    }
    
    public List<MetricData> MetricsData = new List<MetricData>
    {
        new MetricData { Metric = "Sales", Value = 88, Target = 85 },
        new MetricData { Metric = "Quality", Value = 76, Target = 80 }
    };
    
    private List<string> eventLogs = new();
    
    private void OnLoaded(EventArgs args)
    {
        LogEvent("Loaded", "Chart finished loading");
    }
    
    private void OnPointerClick(BulletChartPointEventArgs args)
    {
        LogEvent("PointerClick", $"Clicked on {args.CategoryName}");
    }
    
    private void OnTooltipRender(BulletChartTooltipEventArgs args)
    {
        LogEvent("TooltipRender", $"Tooltip shown for value: {args.Value}");
    }
    
    private void OnLegendRender(BulletChartLegendRenderEventArgs args)
    {
        LogEvent("LegendRender", $"Rendering legend: {args.Text}");
    }
    
    private void OnPrintComplete(PrintEventArgs args)
    {
        LogEvent("PrintComplete", "Chart sent to printer");
    }
    
    private void LogEvent(string eventName, string details)
    {
        var timestamp = DateTime.Now.ToString("HH:mm:ss.fff");
        eventLogs.Add($"[{timestamp}] {eventName}: {details}");
        StateHasChanged();
    }
}
```

### Asynchronous Event Handlers

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="Sales" 
               TargetField="Quota">
    <BulletChartEvents PointerClick="OnPointerClickAsync"></BulletChartEvents>
    <BulletChartRangeCollection>
        <BulletChartRange End="200"></BulletChartRange>
        <BulletChartRange End="350"></BulletChartRange>
        <BulletChartRange End="500"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    [Inject] private HttpClient Http { get; set; }
    
    public class SalesMetric
    {
        public double Sales { get; set; }
        public double Quota { get; set; }
    }
    
    public List<SalesMetric> SalesData = new List<SalesMetric>
    {
        new SalesMetric { Sales = 380, Quota = 350 }
    };
    
    private async Task OnPointerClickAsync(BulletChartPointEventArgs args)
    {
        try
        {
            // Fetch detailed data from API
            var detailUrl = $"api/sales-detail?value={args.Value}";
            var response = await Http.GetAsync(detailUrl);
            
            if (response.IsSuccessStatusCode)
            {
                var data = await response.Content.ReadAsStringAsync();
                Console.WriteLine($"Fetched details: {data}");
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error fetching details: {ex.Message}");
        }
    }
}
```

## Accessibility Features

### Keyboard Navigation

The Bullet Chart supports full keyboard navigation for accessibility.

**Keyboard Shortcuts:**

| Key Combination | Windows | Mac | Action |
|----------------|---------|-----|--------|
| Focus | `Alt + J` | `⌥ + J` | Move focus to chart |
| Next Element | `Tab` | `Tab` | Move to next interactive element |
| Previous Element | `Shift + Tab` | `⇧ + Tab` | Move to previous element |
| Navigate Legend Right | `→` or `↑` | `→` or `↑` | Move to next legend item |
| Navigate Legend Left | `←` or `↓` | `←` or `↓` | Move to previous legend item |
| Print | `Ctrl + P` | `⌘ + P` | Print chart |

### ARIA Attributes

The Bullet Chart automatically includes proper ARIA attributes for screen readers:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@AccessibleData" 
               ValueField="Score" 
               TargetField="Goal" 
               Title="Performance Score (Accessible)"
               Subtitle="Q1 2026 Results">
    <BulletChartRangeCollection>
        <BulletChartRange End="40" Name="Below Expectations"></BulletChartRange>
        <BulletChartRange End="70" Name="Meeting Expectations"></BulletChartRange>
        <BulletChartRange End="100" Name="Exceeding Expectations"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class AccessibleMetric
    {
        public double Score { get; set; }
        public double Goal { get; set; }
    }
    
    public List<AccessibleMetric> AccessibleData = new List<AccessibleMetric>
    {
        new AccessibleMetric { Score = 85, Goal = 75 }
    };
}
```

**Generated ARIA Attributes:**
- `role="img"` - Identifies chart as image
- `role="button"` - For interactive legend items
- `role="region"` - For chart container
- `aria-label` - Descriptive labels for chart elements
- `aria-pressed` - State for toggle elements

### Screen Reader Support

Enhance screen reader experience with descriptive text:

```razor
@using Syncfusion.Blazor.Charts

<div role="region" aria-label="Sales Performance Dashboard">
    <h2 id="chartTitle">Q1 2026 Sales Results</h2>
    
    <div aria-describedby="chartTitle chartDescription">
        <SfBulletChart DataSource="@SalesData" 
                       ValueField="Sales" 
                       TargetField="Quota" 
                       Title="Sales Performance">
            <BulletChartRangeCollection>
                <BulletChartRange End="200" Name="Below Target"></BulletChartRange>
                <BulletChartRange End="350" Name="On Target"></BulletChartRange>
                <BulletChartRange End="500" Name="Above Target"></BulletChartRange>
            </BulletChartRangeCollection>
        </SfBulletChart>
    </div>
    
    <div id="chartDescription" class="sr-only">
        This bullet chart shows actual sales of $380,000 against a target quota of $350,000,
        indicating performance above target in the green zone.
    </div>
</div>

<style>
    .sr-only {
        position: absolute;
        width: 1px;
        height: 1px;
        padding: 0;
        margin: -1px;
        overflow: hidden;
        clip: rect(0, 0, 0, 0);
        white-space: nowrap;
        border-width: 0;
    }
</style>

@code {
    public class SalesMetric
    {
        public double Sales { get; set; }
        public double Quota { get; set; }
    }
    
    public List<SalesMetric> SalesData = new List<SalesMetric>
    {
        new SalesMetric { Sales = 380, Quota = 350 }
    };
}
```

### High Contrast Themes

Support users with visual impairments using high contrast themes:

```razor
@using Syncfusion.Blazor

<div class="theme-selector mb-3">
    <label>
        <input type="checkbox" @onchange="ToggleHighContrast" />
        Enable High Contrast Mode
    </label>
</div>

<SfBulletChart DataSource="@PerformanceData" 
               Theme="@CurrentTheme"
               ValueField="Score" 
               TargetField="Goal" 
               Title="Accessible Performance Chart">
    <BulletChartRangeCollection>
        <BulletChartRange End="40" Color="@(isHighContrast ? "#d50000" : "#d32f2f")"></BulletChartRange>
        <BulletChartRange End="70" Color="@(isHighContrast ? "#ff6f00" : "#ffa726")"></BulletChartRange>
        <BulletChartRange End="100" Color="@(isHighContrast ? "#00c853" : "#66bb6a")"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    private bool isHighContrast = false;
    private Theme CurrentTheme => isHighContrast ? Theme.HighContrast : Theme.Bootstrap5;
    
    public class PerformanceMetric
    {
        public double Score { get; set; }
        public double Goal { get; set; }
    }
    
    public List<PerformanceMetric> PerformanceData = new List<PerformanceMetric>
    {
        new PerformanceMetric { Score = 75, Goal = 70 }
    };
    
    private void ToggleHighContrast(ChangeEventArgs e)
    {
        isHighContrast = (bool)e.Value;
    }
}
```

## WCAG Compliance

### WCAG 2.2 Level AA Requirements

**1. Color Contrast (1.4.3)**

Ensure sufficient contrast ratios:

```razor
<!-- Good: High contrast colors -->
<BulletChartRangeCollection>
    <BulletChartRange End="40" Color="#c62828"></BulletChartRange>  <!-- Contrast ratio: 7.2:1 -->
    <BulletChartRange End="70" Color="#f57c00"></BulletChartRange>  <!-- Contrast ratio: 5.8:1 -->
    <BulletChartRange End="100" Color="#2e7d32"></BulletChartRange> <!-- Contrast ratio: 6.4:1 -->
</BulletChartRangeCollection>

<!-- Avoid: Low contrast -->
<BulletChartRangeCollection>
    <BulletChartRange End="40" Color="#ffcdd2"></BulletChartRange>  <!-- Poor contrast -->
    <BulletChartRange End="70" Color="#fff9c4"></BulletChartRange>
    <BulletChartRange End="100" Color="#c8e6c9"></BulletChartRange>
</BulletChartRangeCollection>
```

**2. Non-Text Contrast (1.4.11)**

Ensure UI components are distinguishable:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@Data">
    <!-- Good: Clear borders and high contrast -->
    <BulletChartValueBorder Color="#1976d2" Width="3"></BulletChartValueBorder>
    <BulletChartTargetBorder Color="#d32f2f" Width="4"></BulletChartTargetBorder>
    <!-- Chart configuration -->
</SfBulletChart>
```

**3. Keyboard Accessible (2.1.1)**

All functionality available via keyboard - automatically supported by Bullet Chart.

**4. Focus Visible (2.4.7)**

Ensure focus indicators are visible:

```html
<style>
    /* Custom focus styles for enhanced visibility */
    .e-bulletchart .e-bullet-legend-focus {
        outline: 3px solid #1976d2 !important;
        outline-offset: 2px;
    }
</style>
```

**5. Meaningful Sequence (1.3.2)**

Provide logical reading order:

```razor
@using Syncfusion.Blazor.Charts

<div class="dashboard-section">
    <h2>Sales Dashboard</h2>
    <p class="chart-description">
        The following chart compares actual sales performance against quarterly targets.
    </p>
    
    <SfBulletChart DataSource="@SalesData" 
                   Title="Q1 Sales Performance">
        <!-- Chart configuration -->
    </SfBulletChart>
    
    <div class="chart-summary">
        <p>Actual Sales: $380K | Target: $350K | Achievement: 108.6%</p>
    </div>
</div>
```

### Accessible Form Pattern

```razor
@using Syncfusion.Blazor.Charts

<form>
    <fieldset>
        <legend>Performance Metrics Configuration</legend>
        
        <div class="form-group">
            <label for="targetValue">Target Value:</label>
            <input type="number" id="targetValue" @bind="targetValue" 
                   aria-describedby="targetHelp" />
            <small id="targetHelp">Enter the target performance value (0-100)</small>
        </div>
        
        <button type="button" @onclick="UpdateChart" 
                aria-label="Update performance chart with new target">
            Update Chart
        </button>
    </fieldset>
</form>

<SfBulletChart DataSource="@ChartData" 
               ValueField="Score" 
               TargetField="Target">
    <!-- Chart configuration -->
</SfBulletChart>

@code {
    private double targetValue = 75;
    
    public class ChartMetric
    {
        public double Score { get; set; }
        public double Target { get; set; }
    }
    
    public List<ChartMetric> ChartData = new();
    
    private void UpdateChart()
    {
        ChartData = new List<ChartMetric>
        {
            new ChartMetric { Score = 85, Target = targetValue }
        };
    }
}
```

## Testing Checklist

### Keyboard Navigation Testing

- [ ] Tab key navigates to chart
- [ ] Arrow keys navigate between legend items
- [ ] Enter/Space activates legend toggles
- [ ] Escape key closes tooltips
- [ ] Tab order is logical and predictable
- [ ] Focus indicators are visible
- [ ] No keyboard traps

### Screen Reader Testing

Test with NVDA, JAWS, or VoiceOver:

- [ ] Chart title is announced
- [ ] Chart type is identified ("Bullet Chart")
- [ ] Data values are readable
- [ ] Legend items are announced with names
- [ ] Tooltip content is conveyed
- [ ] State changes are announced
- [ ] Instructions/descriptions are available

### Color and Contrast

- [ ] Minimum contrast ratio: 4.5:1 (text)
- [ ] Minimum contrast ratio: 3:1 (non-text)
- [ ] Information not conveyed by color alone
- [ ] Chart is usable in grayscale
- [ ] High contrast mode works correctly

### Responsive and Zoom

- [ ] Chart scales with browser zoom (up to 200%)
- [ ] Text remains readable at 200% zoom
- [ ] Touch targets minimum 44x44px (mobile)
- [ ] Chart adapts to mobile viewports
- [ ] No horizontal scrolling required

### Testing Script Example

```csharp
// Automated accessibility test using Playwright
public class BulletChartA11yTests
{
    [Fact]
    public async Task Chart_Should_Have_Accessible_Name()
    {
        // Arrange
        await Page.GotoAsync("/bullet-chart");
        
        // Act
        var chart = await Page.QuerySelectorAsync("[role='img']");
        var ariaLabel = await chart.GetAttributeAsync("aria-label");
        
        // Assert
        Assert.NotNull(ariaLabel);
        Assert.Contains("Performance", ariaLabel);
    }
    
    [Fact]
    public async Task Chart_Should_Be_Keyboard_Accessible()
    {
        // Arrange
        await Page.GotoAsync("/bullet-chart");
        
        // Act
        await Page.Keyboard.PressAsync("Tab");
        var focusedElement = await Page.EvaluateAsync<string>("document.activeElement.tagName");
        
        // Assert
        Assert.NotNull(focusedElement);
    }
    
    [Fact]
    public async Task Legend_Items_Should_Have_Sufficient_Contrast()
    {
        // Test using axe-core or similar library
        var violations = await Page.EvaluateAsync<object>("axe.run()");
        Assert.Empty(violations);
    }
}
```

## Best Practices

### Event Handling

1. **Keep handlers lightweight** - Avoid heavy computations in event handlers
2. **Use async for I/O** - Make event handlers async when calling APIs
3. **Handle exceptions** - Always include try-catch in event handlers
4. **Avoid memory leaks** - Unsubscribe from events when components dispose

### Accessibility

1. **Provide text alternatives** - Always include titles and descriptions
2. **Test with real users** - Get feedback from users with disabilities
3. **Support keyboard fully** - Never rely solely on mouse/touch
4. **Use semantic HTML** - Wrap charts in appropriate landmarks
5. **Provide data tables** - Offer tabular alternative for complex charts

## Troubleshooting

### Issue: Events not firing

**Solutions:**
1. Verify event name spelling (case-sensitive)
2. Check that `BulletChartEvents` component is properly nested
3. Ensure handler method signature matches event type
4. Look for JavaScript console errors

### Issue: Keyboard navigation not working

**Solutions:**
1. Verify chart has received focus (use Alt+J)
2. Check for conflicting keyboard shortcuts
3. Ensure chart is not disabled
4. Test in different browsers

### Issue: Screen reader not announcing content

**Solutions:**
1. Add explicit ARIA labels
2. Provide descriptive titles
3. Include hidden screen reader text
4. Test with multiple screen readers
5. Check that chart container has `role="region"`