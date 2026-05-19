---
name: syncfusion-blazor-3D-charts
description: Provides guidance for building and customizing Syncfusion Blazor 3D Charts. Trigger this skill whenever the user needs help with SfChart3D, 3D Column/Bar charts, axes, data binding, rotation, tilt, depth, legends, tooltips, or any configuration in Syncfusion.Blazor.Chart3D.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Data Visualization"
---

# Implementing Syncfusion Blazor 3D Charts

**NuGet:** `Syncfusion.Blazor.Chart3D` + `Syncfusion.Blazor.Themes`  
**Namespace:** `Syncfusion.Blazor.Chart3D`

A comprehensive skill for implementing and customizing Syncfusion's Blazor 3D Chart component. The 3D Chart provides stunning three-dimensional visualization for column, bar, and stacked chart types with rich customization options including rotation, tilt, depth, and various axis types.

## When to Use This Skill

Use this skill when you need to:
- Create three-dimensional charts in Blazor applications
- Visualize data with 3D column, bar, or stacked chart types
- Implement rotatable and tiltable 3D visualizations
- Configure category, numeric, datetime, or logarithmic axes for 3D charts
- Bind data from various sources (IEnumerable, remote data, dynamic objects)
- Customize 3D chart appearance (colors, depth, wall color, rotation)
- Add data labels, legends, and tooltips to 3D charts
- Configure axis labels, tick lines, and grid lines
- Enable selection, highlighting, and interaction in 3D charts
- Create multi-pane 3D chart layouts
- Export or print 3D charts
- Ensure WCAG accessibility compliance for 3D visualizations
- Troubleshoot 3D chart rendering or configuration issues

## Component Overview

**Package**: `Syncfusion.Blazor.Chart3D`  
**Component**: `SfChart3D`  
**Supported Chart Types**: Column, Bar, StackedColumn, StackedBar  
**Supported Axis Types**: Category, Numeric, DateTime, Logarithmic  
**Platforms**: Blazor Server, Blazor WebAssembly, Blazor Web App (.NET 8+)

The 3D Chart component renders data in three-dimensional space with configurable rotation, tilt, and depth for enhanced visual impact. It supports all standard chart features including multiple series, rich tooltips, legends, data labels, and extensive customization options.

## Documentation and Navigation Guide

### API Reference
📄 **Read:** [references/api-reference.md](references/api-reference.md)
- Complete API reference for Syncfusion.Blazor.Chart3D namespace
- SfChart3D component properties, methods, and events
- All chart series, axis, legend, and tooltip classes
- Data label, border, margin, and styling components
- Event argument classes for all chart events
- Enumerations (Chart3DSeriesType, ValueType, SelectionMode, etc.)
- Interface definitions and base classes
- Usage examples for common API scenarios
- Links to detailed online API documentation

### Events Reference
📄 **Read:** [references/events.md](references/events.md)
- Complete event reference for all 3D chart events
- Lifecycle events (OnCreated, OnResized)
- Rendering events (OnSeriesRender, OnPointRender, OnAxisLabelRender, etc.)
- User interaction events (OnPointClick, OnLegendClick, OnSelectionChanged)
- Mouse events (OnChartMouseMove, OnChartMouseClick, etc.)
- Export and data events (OnExported, OnAxisRangeRendered)
- Event argument properties and usage examples
- Complete working examples with multiple events
- Event handling best practices

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Complete installation and setup for Blazor Server, WebAssembly, and Web App
- NuGet package installation (Syncfusion.Blazor.Chart3D)
- Service registration and namespace imports
- Script and theme configuration
- Creating your first 3D chart with complete code example
- Data source setup and basic binding
- Render mode configuration for .NET 8+
- Troubleshooting common setup issues

### Chart Types
📄 **Read:** [references/chart-types.md](references/chart-types.md)
- Column charts - vertical 3D bars
- Bar charts - horizontal 3D bars
- Stacked Column charts - vertically stacked series
- Stacked Bar charts - horizontally stacked series
- Chart type configuration (Chart3DSeriesType enum)
- Column/bar spacing and width customization
- Grouped columns with GroupName property
- Multiple series in single chart
- Complete code examples for each type

### Data Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- List binding with IEnumerable objects
- ExpandoObject binding for dynamic data structures
- DynamicObject binding for runtime data
- Remote data binding with SfDataManager
- DataSource, XName, YName property mapping
- Complex data structures and nested properties
- Dynamic data updates and refresh
- Performance optimization tips

### Axis Configuration
📄 **Read:** [references/axes-configuration.md](references/axes-configuration.md)
- Category Axis - for discrete categories
- Numeric Axis - for continuous numeric data
- DateTime Axis - for time-series data
- Logarithmic Axis - for exponential scale data
- ValueType property configuration
- Axis range (Minimum, Maximum, Interval)
- Axis titles and title rotation
- Tick lines and grid lines customization
- Multiple axes support
- Complete examples for each axis type

### Axis Labels
📄 **Read:** [references/axis-labels.md](references/axis-labels.md)
- Label formatting (LabelFormat property)
- Custom label templates
- Label rotation (LabelRotationAngle)
- Label placement (BetweenTicks, OnTicks)
- Label style customization (font, color, size)
- Smart label collision handling
- Custom label text with events
- Edge label placement
- Multilevel labels

### Data Labels
📄 **Read:** [references/data-labels.md](references/data-labels.md)
- Enabling data labels on series
- Label position configuration
- Label template customization
- Label styling (font, color, background)
- Smart label positioning
- Format customization
- Conditional label visibility
- Complete implementation examples

### Legend
📄 **Read:** [references/legend.md](references/legend.md)
- Enabling and positioning legends
- Legend alignment options
- Series name customization
- Legend styling and appearance
- Interactive legend toggle
- Custom legend items and icons
- Legend templates
- Complete configuration examples

### Tooltip
📄 **Read:** [references/tooltip.md](references/tooltip.md)
- Enabling tooltips on hover
- Tooltip format customization
- Custom tooltip templates
- Tooltip styling (border, fill, opacity)
- Multi-point tooltip content
- Shared tooltips across series
- Complete implementation examples

### Appearance and Styling
📄 **Read:** [references/appearance.md](references/appearance.md)
- Chart background and wall color
- Series colors and color palettes
- Theme integration (Material, Bootstrap, Fluent, Tailwind)
- Custom CSS styling with CssClass
- Opacity and transparency effects
- Border styles and customization
- Animation configuration
- Visual effects and enhancements

### Chart Dimensions and 3D Effects
📄 **Read:** [references/chart-dimensions.md](references/chart-dimensions.md)
- Chart size configuration (Width, Height)
- Responsive sizing techniques
- 3D rotation (EnableRotation, RotationAngle)
- Tilt angle configuration (TiltAngle)
- Depth configuration (Depth property)
- Wall size customization
- Perspective and viewing angle
- Complete dimension examples

### Selection and Highlighting
📄 **Read:** [references/selection.md](references/selection.md)
- Selection modes (Point, Series, Cluster)
- Enabling selection functionality
- Selection patterns and visual styles
- Multi-selection support
- Selection events and handling
- Programmatic selection
- Highlight modes and effects
- Complete selection examples

### Multiple Panes
📄 **Read:** [references/multiple-panes.md](references/multiple-panes.md)
- Creating multi-pane chart layouts
- Row and column pane configuration
- Assigning axes to specific panes
- Distributing series across panes
- Pane border and background customization
- Synchronized pane behavior
- Use cases for multiple panes
- Complete multi-pane examples

### Print and Export
📄 **Read:** [references/print-export.md](references/print-export.md)
- Print functionality
- Export to image formats (PNG, JPEG, SVG)
- Export to PDF documents
- Export configuration options
- Custom export settings
- Print preview customization
- Complete export examples

### Accessibility
📄 **Read:** [references/accessibility.md](references/accessibility.md)
- WCAG 2.1 compliance overview
- Keyboard navigation support
- ARIA attributes and roles
- Screen reader compatibility
- Focus management
- High contrast mode support
- Color contrast requirements
- Accessible design patterns
- Testing accessibility

## Quick Start Example

```razor
@page "/3d-chart-demo"
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales Analysis" 
           WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <Chart3DPrimaryXAxis Title="Month" 
                         ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DPrimaryYAxis Title="Sales (in thousands)">
    </Chart3DPrimaryYAxis>
    
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>
    
    <Chart3DTooltipSettings Enable="true"></Chart3DTooltipSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData" 
                       Name="Sales" 
                       XName="Month" 
                       YName="SalesValue" 
                       Type="Chart3DSeriesType.Column">
            <Chart3DDataLabel Visible="true"></Chart3DDataLabel>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class SalesInfo
    {
        public string Month { get; set; }
        public double SalesValue { get; set; }
    }

    public List<SalesInfo> SalesData = new List<SalesInfo>
    {
        new SalesInfo { Month = "Jan", SalesValue = 35 },
        new SalesInfo { Month = "Feb", SalesValue = 28 },
        new SalesInfo { Month = "Mar", SalesValue = 34 },
        new SalesInfo { Month = "Apr", SalesValue = 32 },
        new SalesInfo { Month = "May", SalesValue = 40 },
        new SalesInfo { Month = "Jun", SalesValue = 32 },
        new SalesInfo { Month = "Jul", SalesValue = 35 }
    };
}
```

## Common Patterns

### Multiple Series Chart

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D WallColor="transparent" EnableRotation="true" RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@Data" Name="Series 1" XName="X" YName="Y1" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@Data" Name="Series 2" XName="X" YName="Y2" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class ChartData
    {
        public string X { get; set; }
        public double Y1 { get; set; }
        public double Y2 { get; set; }
    }

    public List<ChartData> Data = new List<ChartData>
    {
        new ChartData { X = "Jan", Y1 = 35, Y2 = 28 },
        new ChartData { X = "Feb", Y1 = 28, Y2 = 32 },
        new ChartData { X = "Mar", Y1 = 34, Y2 = 30 },
        new ChartData { X = "Apr", Y1 = 32, Y2 = 36 },
        new ChartData { X = "May", Y1 = 40, Y2 = 38 },
        new ChartData { X = "Jun", Y1 = 32, Y2 = 34 },
        new ChartData { X = "Jul", Y1 = 35, Y2 = 37 }
    };
}
```

### Stacked Column Chart

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D WallColor="transparent" EnableRotation="true" RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@Data" XName="X" YName="Y1" Type="Chart3DSeriesType.StackingColumn">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@Data" XName="X" YName="Y2" Type="Chart3DSeriesType.StackingColumn">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class StackingData
    {
        public string X { get; set; }
        public double Y1 { get; set; }
        public double Y2 { get; set; }
    }

    public List<StackingData> Data = new List<StackingData>
    {
        new StackingData { X = "Jan", Y1 = 20, Y2 = 15 },
        new StackingData { X = "Feb", Y1 = 18, Y2 = 12 },
        new StackingData { X = "Mar", Y1 = 25, Y2 = 14 },
        new StackingData { X = "Apr", Y1 = 22, Y2 = 16 },
        new StackingData { X = "May", Y1 = 28, Y2 = 18 },
        new StackingData { X = "Jun", Y1 = 24, Y2 = 14 },
        new StackingData { X = "Jul", Y1 = 26, Y2 = 17 }  
    };
}
```

### DateTime Axis Chart

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D WallColor="transparent" EnableRotation="true" RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.DateTime" 
                         LabelFormat="yyyy">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@TimeSeriesData" 
                       XName="Date" 
                       YName="Value" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class TimeSeriesPoint
    {
        public DateTime Date { get; set; }
        public double Value { get; set; }
    }

    public List<TimeSeriesPoint> TimeSeriesData = new List<TimeSeriesPoint>
    {
        new TimeSeriesPoint { Date = new DateTime(2018, 1, 1), Value = 120 },
        new TimeSeriesPoint { Date = new DateTime(2019, 1, 1), Value = 135 },
        new TimeSeriesPoint { Date = new DateTime(2020, 1, 1), Value = 150 },
        new TimeSeriesPoint { Date = new DateTime(2021, 1, 1), Value = 170 },
        new TimeSeriesPoint { Date = new DateTime(2022, 1, 1), Value = 160 },
        new TimeSeriesPoint { Date = new DateTime(2023, 1, 1), Value = 185 },
        new TimeSeriesPoint { Date = new DateTime(2024, 1, 1), Value = 200 }
    };
}
```

## Key Properties and APIs

### SfChart3D Component
**Main Properties**:
- **Title**: Chart title text
- **SubTitle**: Chart subtitle text
- **Width/Height**: Chart dimensions (string with units or percentage)
- **WallColor**: Background color of 3D walls
- **EnableRotation**: Enable mouse rotation (bool)
- **RotationAngle**: Initial rotation angle (0-360 degrees)
- **TiltAngle**: Tilt angle for 3D perspective (0-360 degrees)
- **Depth**: Depth of 3D rendering (numeric value)
- **Theme**: Chart theme (Material, Bootstrap, Fluent, Tailwind, etc.)
- **Background**: Background color for the chart
- **BackgroundImage**: Background image URL

**Methods**:
- `ExportAsync(ExportType, string)` - Export chart to PNG, JPEG, SVG, or PDF
- `PrintAsync()` - Print the chart
- `Refresh()` - Refresh the chart rendering

**Key Events**:
- `OnCreated` - Chart3DCreatedEventArgs
- `OnPointClick` - Chart3DPointClickEventArgs
- `OnSeriesRender` - Chart3DSeriesRenderEventArgs
- `OnPointRender` - Chart3DPointRenderEventArgs
- `OnLegendClick` - Chart3DLegendClickEventArgs
- `OnTooltipRender` - Chart3DTooltipRenderEventArgs
- `OnSelectionChanged` - Chart3DSelectionChangedEventArgs
- `OnAxisLabelRender` - Chart3DAxisLabelRenderEventArgs

### Chart3DSeries Properties
- **DataSource**: Data collection to bind
- **XName**: Property name for X-axis values
- **YName**: Property name for Y-axis values
- **Name**: Series display name for legend
- **Type**: Chart3DSeriesType enum (Column, Bar, StackingColumn, StackingBar)
- **Fill**: Series color
- **Opacity**: Series opacity (0-1)
- **ColumnSpacing**: Space between columns (0-1)
- **ColumnWidth**: Width of columns (0-1)
- **GroupName**: Group name for grouped columns
- **Visible**: Series visibility toggle

**Child Components**:
- `Chart3DDataLabel` - Data label configuration
- `Chart3DEmptyPointSettings` - Empty point handling
- `Chart3DSeriesBorder` - Border customization
- `Chart3DAnimation` - Animation settings

### Axis Components (Chart3DPrimaryXAxis, Chart3DPrimaryYAxis, Chart3DAxis)
**Core Properties**:
- **ValueType**: ValueType enum (Category, Numeric, DateTime, Double, DateTimeCategory, Logarithmic)
- **Title**: Axis title text
- **Minimum/Maximum**: Axis range bounds
- **Interval**: Spacing between axis values
- **LabelFormat**: Format string for labels (e.g., "yyyy", "C", "N2")
- **LabelRotationAngle**: Rotation angle for labels
- **EdgeLabelPlacement**: EdgeLabelPlacement enum (None, Shift, Hide)
- **LabelPlacement**: LabelPlacement enum (OnTicks, BetweenTicks)
- **RangePadding**: ChartRangePadding enum (Auto, None, Normal, Additional, Round)
- **IsInversed**: Invert axis direction
- **OpposedPosition**: Position axis on opposite side

**Child Components**:
- `Chart3DAxisLabelStyle` - Label font customization
- `Chart3DAxisTitleStyle` - Title font customization
- `Chart3DMajorGridLines` / `Chart3DMinorGridLines` - Grid line settings
- `Chart3DMajorTickLines` / `Chart3DMinorTickLines` - Tick line settings

### Legend Settings (Chart3DLegendSettings)
- **Visible**: Show/hide legend
- **Position**: LegendPosition enum (Top, Bottom, Left, Right, Custom)
- **Alignment**: Alignment enum (Near, Center, Far)
- **Height/Width**: Legend dimensions
- **ShapeHeight/ShapeWidth**: Icon dimensions
- **ToggleVisibility**: Enable series toggle on click
- **Mode**: Chart3DLegendMode enum (Series, Point)

**Child Components**:
- `Chart3DLegendTextStyle` - Text font style
- `Chart3DLegendBorder` - Border customization
- `Chart3DLegendMargin` - Margin settings

### Tooltip Settings (Chart3DTooltipSettings)
- **Enable**: Enable tooltip
- **Fill**: Background color
- **Format**: Format string for tooltip content
- **Shared**: Share tooltip across series
- **EnableMarker**: Show marker in tooltip
- **FadeOutDuration**: Fade out duration in ms
- **FadeOutMode**: Chart3DFadeOutMode enum (Click, Move)
- **Template**: RenderFragment for custom tooltip

**Child Components**:
- `Chart3DTooltipTextStyle` - Text font style
- `Chart3DTooltipBorder` - Border customization

### Selection Settings
- **SelectionMode**: SelectionMode enum (None, Point, Series, Cluster)
- **SelectionPattern**: SelectionPattern enum (None, Dots, DiagonalForward, Grid, Chessboard, etc.)
- **HighlightMode**: HighlightMode enum (None, Point, Series, Cluster)

Use `Chart3DSelectedDataIndexes` collection with `Chart3DSelectedDataIndex` to programmatically select points.

## Common Use Cases

### Business Dashboard Metrics
Display KPIs, sales data, or performance metrics with enhanced 3D visualization for executive dashboards.

### Financial Data Visualization
Render stock prices, market trends, or financial comparisons with datetime axes and multiple series.

### Scientific Data Presentation
Visualize experimental data, measurements, or research findings with logarithmic or numeric axes.

### Product Comparison
Compare product features, ratings, or sales across categories with grouped or stacked columns.

### Time-Series Analysis
Display trends over time with datetime axes and smooth animations for data changes.

### Regional Sales Reporting
Show sales by region, country, or territory with category axes and colorful series.

## Troubleshooting Tips

**Chart not rendering:**
- Verify `AddSyncfusionBlazor()` service registration in Program.cs
- Check namespace imports in _Imports.razor
- Ensure script reference is at end of body tag
- Confirm render mode for .NET 8+ projects

**Data not displaying:**
- Check XName and YName match property names exactly (case-sensitive)
- Verify DataSource is not null and contains data
- Ensure ValueType matches data type (Category for strings, Numeric for numbers)

**3D rotation not working:**
- Set EnableRotation="true"
- Ensure RotationAngle is between 0-360
- Check if mouse events are blocked by overlays

**Performance issues:**
- Limit data points (3D rendering is resource-intensive)
- Disable animations if not needed
- Use appropriate chart type for data volume

## Related Syncfusion Components

- **Chart** - 2D chart component with 30+ chart types
- **Stock Chart** - Financial charting with technical indicators
- **Circular Gauge** - Radial gauge visualization
- **Pivot Table** - Multi-dimensional data analysis
- **TreeMap** - Hierarchical data visualization
