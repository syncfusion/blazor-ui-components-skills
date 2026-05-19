---
name: syncfusion-blazor-range-selectors
description: Implement Syncfusion Blazor RangeNavigator for interactive data range selection and chart navigation. Trigger when users mention range selector, range navigator, SfRangeNavigator, Syncfusion.Blazor.Charts.RangeNavigator, time-series filtering, date range picker for charts, data zooming, slider navigation, thumb-based range selection, period selector, or chart data range selection for large data.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Data Visualization"
---

# Implementing Range Selectors

**NuGet:** `Syncfusion.Blazor.Charts` + `Syncfusion.Blazor.Themes` (or `Syncfusion.Blazor.RangeNavigator` for individual package)  
**Namespace:** `Syncfusion.Blazor.Charts`

A comprehensive skill for implementing Syncfusion Blazor Range Selector (RangeNavigator) components for data range selection and chart navigation. The Range Selector enables users to select a specific range from a large data collection using draggable thumbs, providing an intuitive way to filter and navigate through time-series or numeric data.

## When to Use This Skill

Use this skill immediately when you need to:
- Enable range selection in charts with draggable thumbs
- Filter large time-series datasets by date range
- Navigate through financial data or stock prices
- Create chart zoom/pan controls with visual feedback
- Implement dashboard filtering based on data ranges
- Add period selector buttons (1M, 3M, 6M, YTD, 1Y, All) for quick navigation
- Display data trends with area, line, or stepline series
- Build interactive data exploration interfaces
- Filter data for drill-down analysis
- Create responsive range selection controls for Blazor Server, WebAssembly, or Web App
- Enable synchronized filtering across multiple charts
- Implement lightweight chart navigation for performance-critical scenarios
- Provide visual context for selected data ranges

## Component Overview

The **Syncfusion Blazor Range Selector** (`SfRangeNavigator`) is a specialized control designed for data range selection and navigation. It combines:

- **Draggable Thumbs**: Left and right handles for selecting range boundaries
- **Visual Series**: Line, Area, or StepLine visualization of data trends
- **Period Selector**: Quick preset buttons via `RangeNavigatorPeriodSelectorSettings`, `RangeNavigatorPeriods`, and `RangeNavigatorPeriod`
- **Value Types**: Support for DateTime, Numeric, and Logarithmic data
- **Interactive Selection**: Click labels or drag thumbs to update range
- **Data Binding**: Local and remote data source integration
- **Customization**: Extensive styling, theming, and formatting options using `RangeNavigatorBorder`, `RangeNavigatorMargin`, `RangeNavigatorStyleSettings`, `RangeNavigatorThumbSettings`, and tooltip settings

**Key Capabilities:**
- **Range Selection Methods**: Drag thumbs, tap labels, or set programmatically
- **Series Types**: Line (default), Area, StepLine
- **Value Binding**: One-way and two-way binding support
- **Integration**: Works with period selectors and other charts
- **Export**: PNG, JPEG, SVG, PDF export functionality
- **Accessibility**: WCAG compliant with keyboard navigation

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

Start here for installation, setup, and your first range selector. Covers:
- Installing Syncfusion.Blazor.RangeNavigator NuGet package
- Blazor Server, WebAssembly, and Web App setup
- Service registration and theme configuration
- Basic SfRangeNavigator implementation with sample data
- Project structure and script references
- Troubleshooting common setup issues

### Core Configuration

#### Range Selection and Values

📄 **Read:** [references/range-configuration.md](references/range-configuration.md)

Configure range selection behavior and value binding:
- Value property for start and end values
- One-way and two-way binding patterns
- Value types (DateTime, Numeric, Logarithmic)
- Thumb dragging for range selection
- Label tapping for quick selection
- Programmatic range updates and validation

#### Series Types

📄 **Read:** [references/series-types.md](references/series-types.md)

Choose and customize series visualization:
- Line series for trend lines
- Area series for filled regions
- StepLine series for discrete data
- Series customization (colors, width, fill)
- Multiple series support

### Data and Integration

#### Data Binding

📄 **Read:** [references/data-binding.md](references/data-binding.md)

Configure data sources for the range selector:
- Local data sources (List, Array, ExpandoObject)
- Remote data binding with SfDataManager
- DateTime data handling and formatting
- Numeric and logarithmic scale data
- Data refresh and dynamic updates

#### Period Selector Integration

📄 **Read:** [references/period-selector-integration.md](references/period-selector-integration.md)

Add period selector buttons for quick navigation:
- Predefined period buttons (1M, 3M, 6M, YTD, 1Y, All)
- Custom period configuration
- Integration with range changes
- Event handling for period selection
- Styling and positioning

### Customization and Styling

#### Axis Customization

📄 **Read:** [references/axis-customization.md](references/axis-customization.md)

Customize axis, grid, and labels:
- Grid line configuration (major and minor)
- Tick customization (size, color, position)
- Label formatting and rotation
- Interval types (Years, Months, Days, Hours, Minutes)
- Logarithmic axis support
- RTL (Right-to-Left) support

#### Visual Customization

📄 **Read:** [references/visual-customization.md](references/visual-customization.md)

Control appearance, themes, and layout:
- Chart dimensions (Width, Height, Margin)
- Theme selection (Material, Bootstrap, Fluent, Tailwind, Fabric, Highcontrast)
- Tooltip configuration and templates
- Lightweight rendering mode for performance
- Custom styling and color schemes
- Responsive design patterns

### Advanced Features

#### Export, Events, and Accessibility

📄 **Read:** [references/export-events-accessibility.md](references/export-events-accessibility.md)

Handle exports, events, and ensure accessibility:
- **Export**: PNG, JPEG, SVG, PDF export functionality
- **Events**: Changed, Loaded, TooltipRender event handling
- **Accessibility**: WCAG compliance, keyboard navigation, screen readers, high contrast themes
- ARIA attributes and testing checklist

## Quick Start Example

Here's a minimal range selector for time-series data filtering with event handling:

```razor
@using Syncfusion.Blazor.Charts

@{
    DateTime[] range = SelectedRange as DateTime[] ?? new DateTime[] { DateTime.Now, DateTime.Now };
}
<h3>Stock Price Range Selector</h3>
<p>Selected Range: @range[0].ToShortDateString() to @range[1].ToShortDateString()</p>

<SfRangeNavigator @bind-Value="@SelectedRange" 
                  ValueType="RangeValueType.DateTime"
                  LabelFormat="MMM-yy"
                  IntervalType="RangeIntervalType.Months">
        <RangeNavigatorEvents Changed="OnRangeChanged"></RangeNavigatorEvents>
    <RangeNavigatorRangeTooltipSettings Enable="true"></RangeNavigatorRangeTooltipSettings>
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close" 
                              Type="RangeNavigatorType.Area"
                              Fill="#3F51B5">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }
    
    public object SelectedRange = new DateTime[] 
    { 
        new DateTime(2020, 01, 01), 
        new DateTime(2021, 01, 01) 
    };
    
    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2018, 01, 01), Close = 35 },
        new StockInfo { Date = new DateTime(2019, 01, 01), Close = 42 },
        new StockInfo { Date = new DateTime(2020, 01, 01), Close = 48 },
        new StockInfo { Date = new DateTime(2021, 01, 01), Close = 56 },
        new StockInfo { Date = new DateTime(2022, 01, 01), Close = 62 }
    };
    
    private void OnRangeChanged(ChangedEventArgs args)
    {
        // Handle range change event
        Console.WriteLine($"Range changed: {args.Start} to {args.End}");
    }
}
```

**What this creates:**
- Area chart showing stock price trend with blue fill (#3F51B5)
- Draggable thumbs for range selection
- Initially selected range: Jan 2020 to Jan 2021
- Month labels with "MMM-yy" format
- Tooltip showing values on hover
- Event handler responding to range changes
- Two-way binding with @bind-Value directive

## Common Use Cases

### 1. Stock Price Filtering with Period Selector

Filter stock data with quick period buttons:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-Value="@SelectedRange" 
                  ValueType="RangeValueType.DateTime"
                  IntervalType="RangeIntervalType.Months"
                  LabelFormat="MMM yy">
    <RangeNavigatorRangeTooltipSettings Enable="true" DisplayMode="TooltipDisplayMode.Always">
    </RangeNavigatorRangeTooltipSettings>
    <RangeNavigatorPeriodSelectorSettings>
        <RangeNavigatorPeriods>
            <RangeNavigatorPeriod Text="1M" Interval="1" IntervalType="RangeIntervalType.Months"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Text="3M" Interval="3" IntervalType="RangeIntervalType.Months"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Text="6M" Interval="6" IntervalType="RangeIntervalType.Months"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Text="YTD"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Text="1Y" Interval="1" IntervalType="RangeIntervalType.Years"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Text="All"></RangeNavigatorPeriod>
        </RangeNavigatorPeriods>
    </RangeNavigatorPeriodSelectorSettings>
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" XName="Date" YName="Close" Type="RangeNavigatorType.Area">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@{
    DateTime[] range = SelectedRange as DateTime[] ?? new DateTime[] { DateTime.Now, DateTime.Now };
}
<p>Selected Range: @range[0].ToShortDateString() to @range[1].ToShortDateString()</p>

@code {
    public object SelectedRange = new DateTime[] 
    { 
        new DateTime(2022, 01, 01), 
        new DateTime(2023, 01, 01) 
    };
    
    public List<StockInfo> StockData = GetStockData();
    
    private static List<StockInfo> GetStockData()
    {
        // Sample data generation
        var data = new List<StockInfo>();
        var startDate = new DateTime(2020, 01, 01);
        var random = new Random();
        
        for (int i = 0; i < 100; i++)
        {
            data.Add(new StockInfo 
            { 
                Date = startDate.AddDays(i * 10), 
                Close = 100 + random.Next(-20, 20) 
            });
        }
        
        return data;
    }

    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }
}
```

### 2. Dashboard with Range-Based Filtering

Synchronize chart data with range selector:

```razor
@using Syncfusion.Blazor.Charts

<div class="dashboard-container">
    <h3>Sales Dashboard</h3>
    <SfRangeNavigator Value="@SelectedRange"
                      ValueType="RangeValueType.DateTime"
                      Height="140px">

        <RangeNavigatorEvents Changed="OnRangeChanged" />

        <RangeNavigatorSeriesCollection>
            <RangeNavigatorSeries DataSource="@SalesDetails"
                                  XName="Date"
                                  YName="Revenue"
                                  Type="RangeNavigatorType.Area" />
        </RangeNavigatorSeriesCollection>

    </SfRangeNavigator>
    <SfChart Height="300px">
        <ChartPrimaryXAxis ValueType="Syncfusion.Blazor.Charts.ValueType.DateTime"
                           Minimum="@SelectedRange[0]"
                           Maximum="@SelectedRange[1]" />

        <ChartSeriesCollection>
            <ChartSeries DataSource="@FilteredData"
                         XName="Date"
                         YName="Revenue"
                         Type="ChartSeriesType.Column" />
        </ChartSeriesCollection>
    </SfChart>
</div>

@code {
    public DateTime[] SelectedRange =
    {
        new DateTime(2023, 01, 01),
        new DateTime(2023, 06, 30)
    };

    public List<SalesData> SalesDetails = new();
    public List<SalesData> FilteredData = new();

    protected override void OnInitialized()
    {
        SalesDetails = GetSalesData();
        ApplyFilter();
    }

    private void OnRangeChanged(ChangedEventArgs args)
    {
        if (args.Start is DateTime start && args.End is DateTime end)
        {
            SelectedRange = new[] { start, end };
            ApplyFilter();
        }
    }

    private void ApplyFilter()
    {
        FilteredData = SalesDetails
            .Where(d => d.Date >= SelectedRange[0] && d.Date <= SelectedRange[1])
            .ToList();
    }

    private List<SalesData> GetSalesData()
    {
        var data = new List<SalesData>();
        var random = new Random();
        var start = new DateTime(2023, 01, 01);
        decimal revenue = 15000;

        for (int i = 0; i < 180; i++)
        {
            revenue += random.Next(-1500, 2000);

            data.Add(new SalesData
            {
                Date = start.AddDays(i),
                Revenue = Math.Max(revenue, 5000)
            });
        }

        return data;
    }

    public class SalesData
    {
        public DateTime Date { get; set; }
        public decimal Revenue { get; set; }
    }
}
```

### 3. Lightweight Mode for Performance

Optimize for large datasets:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator Value="@SelectedRange" 
                  ValueType="RangeValueType.DateTime"
                  EnableGrouping="true"
                  GroupBy="RangeIntervalType.Months"
                  AllowSnapping="true">
    <RangeNavigatorRangeTooltipSettings Enable="true"></RangeNavigatorRangeTooltipSettings>
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@LargeDataset" 
                              XName="Timestamp" 
                              YName="Value"
                              Type="RangeNavigatorType.Line">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public DateTime[] SelectedRange;
    public List<DataPoint> LargeDataset = GenerateLargeDataset(10000);

    public class DataPoint
    {
        public DateTime Timestamp { get; set; }
        public double Value { get; set; }
    }
    
    private static List<DataPoint> GenerateLargeDataset(int count)
    {
        // Generate large dataset efficiently
        return Enumerable.Range(0, count)
            .Select(i => new DataPoint 
            { 
                Timestamp = DateTime.Now.AddHours(-count + i), 
                Value = Math.Sin(i * 0.1) * 100 
            })
            .ToList();
    }
}
```

## Key Properties Reference

### Essential Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `Value` | DateTime[] / double[] | Selected range [start, end] | null |
| `ValueType` | RangeValueType | Data type (DateTime, Double, Logarithmic) | Double |
| `DataSource` | object | Data collection for series | null |
| `IntervalType` | RangeIntervalType | Axis interval (Auto, Years, Months, Days, etc.) | Auto |
| `LabelFormat` | string | Label display format | null |
| `Width` | string | Component width | "100%" |
| `Height` | string | Component height | "80px" |
| `Theme` | Theme | Visual theme (Material, Bootstrap5, Fluent, Tailwind, Fabric, HighContrast) | Material |
| `Interval` | int | Axis interval value | 1 |
| `LogBase` | double | Base for logarithmic axis | 10 |
| `Orientation` | Orientation | Horizontal or Vertical | Horizontal |

### Series Configuration

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `Type` | RangeNavigatorType | Series type (Line, Area, StepLine) | Line |
| `XName` | string | X-axis data field name | null |
| `YName` | string | Y-axis data field name | null |
| `Fill` | string | Series fill color | null |
| `Width` | double | Series line width | 1 |
| `Opacity` | double | Series opacity (0-1) | 1 |
| `Name` | string | Series name/label | null |

### Visual Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `EnableGrouping` | bool | Enable data grouping | false |
| `AllowSnapping` | bool | Snap thumbs to data points | false |
| `EnableRtl` | bool | Right-to-Left support | false |
| `TabIndex` | int | Tab index for keyboard navigation | 0 |

### Period Selector

| Property | Type | Description |
|----------|------|-------------|
| `RangeNavigatorPeriodSelectorSettings` | Component | Period selector configuration (Enabled, Height, Position) |
| `RangeNavigatorPeriods` | Collection | Period button collection container |
| `RangeNavigatorPeriod` | Item | Individual period selector button (Text, Interval, IntervalType, Selected) |

### Tooltip Configuration

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `RangeNavigatorRangeTooltipSettings.Enable` | bool | Enable/disable tooltip | true |
| `RangeNavigatorRangeTooltipSettings.DisplayMode` | TooltipDisplayMode | Display mode (OnDemand, Always) | OnDemand |
| `RangeNavigatorRangeTooltipSettings.Format` | string | Tooltip content format template | null |

### Axis Customization

| Property | Type | Description |
|----------|------|-------------|
| `RangeNavigatorMargin` | Component | Margin (Left, Right, Top, Bottom) |
| `RangeNavigatorBorder` | Component | Border (Color, Width, DashArray) |
| `RangeNavigatorMajorGridLines` | Component | Gridlines (Width, Color, DashArray) |
| `RangeNavigatorMajorTickLines` | Component | Tick lines (Width, Color, Height) |
| `RangeNavigatorLabelStyle` | Component | Label styling (Color, FontFamily, Size, FontWeight) |

### Thumb Configuration

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `RangeNavigatorThumbSettings.Type` | ThumbType | Thumb shape (Circle, Rectangle) | Circle |
| `RangeNavigatorThumbSettings.Fill` | string | Thumb fill color | "#f3f3f3" |
| `RangeNavigatorThumbSettings.Size` | double | Thumb size in pixels | 15 |

## Events Reference

Key events for handling Range Navigator interactions:

| Event | Event Args | Description |
|-------|-----------|-------------|
| `Changed` | `ChangedEventArgs` | Fires when range selection changes (start, end, value) |
| `Loaded` | `RangeLoadedEventArgs` | Fires after component loads completely |
| `Resizing` | `RangeResizeEventArgs` | Fires while dragging thumbs |
| `Resized` | `RangeResizeEventArgs` | Fires after thumb drag completes |
| `Rendering` | `RangeSelectorRenderEventArgs` | Fires before selector renders |
| `TooltipRender` | `RangeTooltipRenderEventArgs` | Fires before tooltip renders |
| `LabelRender` | `RangeLabelRenderEventArgs` | Fires when labels render |

**Example Event Handler:**
```csharp
private void OnRangeChanged(ChangedEventArgs args)
{
    var startValue = args.Start;  // DateTime or double
    var endValue = args.End;      // DateTime or double
    var selectedData = args.SelectedData;  // Filtered data
    StateHasChanged();
}
```

## Methods Reference

Key methods for programmatic control:

| Method | Return Type | Description |
|--------|------------|-------------|
| `ExportAsync(ExportType, string)` | `Task` | Export as PNG, JPEG, SVG, or PDF |
| `Print()` | `Task` | Print the Range Navigator |
| `Refresh()` | `Task` | Refresh/redraw component |
| `GetVisibleRangeModel()` | `VisibleRangeModel` | Get current visible range |

## Implementation Workflow

When implementing a range selector, follow this sequence:

1. **Start with [Getting Started](references/getting-started.md)** for installation and basic setup
2. **Configure range values** from [Range Configuration](references/range-configuration.md) for selection behavior
3. **Choose series type** using [Series Types](references/series-types.md) for visualization
4. **Set up data binding** from [Data Binding](references/data-binding.md) for your data structure — when using remote data prefer trusted/internal APIs or mocked data; see [Data Binding - Security Considerations](references/data-binding.md)
5. **Add period selector** (optional) with [Period Selector Integration](references/period-selector-integration.md)
6. **Customize axis** with [Axis Customization](references/axis-customization.md) for proper labeling
7. **Apply visual styling** from [Visual Customization](references/visual-customization.md)
8. **Add export and events** using [Export, Events, and Accessibility](references/export-events-accessibility.md)
9. **Review [API Reference](references/api-reference.md)** for complete API documentation with all classes, properties, and enums

For questions or issues, refer to the troubleshooting sections in each reference file or consult [Export, Events, and Accessibility](references/export-events-accessibility.md) for event handling patterns and accessibility requirements.
