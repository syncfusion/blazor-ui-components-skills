---
name: syncfusion-blazor-smith-charts
description: Implement Syncfusion Blazor Smith Chart for RF circuit analysis and transmission line impedance matching. Use this when visualizing impedance/admittance data, reflection coefficients, S-parameters, or antenna tuning in Blazor applications. This skill covers circular grid representations and RF network parameter analysis.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
  category: "Data Visualization"
---

# Implementing Smith Charts

**NuGet:** `Syncfusion.Blazor.Charts` + `Syncfusion.Blazor.Themes` (or `Syncfusion.Blazor.SmithChart` for individual package)
**Namespace:** `Syncfusion.Blazor.Charts`

The Syncfusion Blazor Smith Chart (`SfSmithChart`) is a specialized charting component designed for visualizing transmission line impedance, RF circuit parameters, and antenna tuning data. It displays complex impedance or admittance on a circular grid, making it essential for RF/microwave engineering applications. The component lives in the `Syncfusion.Blazor.Charts` namespace and exposes the Smith Chart API surface for series, axes, legends, titles, subtitles, tooltips, export/print, and rendering events.

## When to Use This Skill

Use this skill when the user needs to:

- **Visualize impedance matching** for transmission lines and RF circuits
- **Plot reflection coefficients** (S-parameters) on a Smith Chart
- **Analyze antenna tuning** and resonance characteristics
- **Display multiple transmission lines** or circuit configurations
- **Customize circular and radial axes** for impedance/admittance visualization
- **Add markers and data labels** to highlight critical impedance points
- **Enable legends** to differentiate between multiple series
- **Show tooltips** with resistance and reactance values
- **Export Smith Charts** to image formats or print for documentation
- **Handle RF engineering data** with resistance and reactance coordinates

## Component Overview

The Smith Chart component renders data points as a series connected by lines on a circular coordinate system. Key capabilities include:

- **Two-axis system:** Horizontal axis (straight line) and radial axis (circular path)
- **Series visualization:** Plot multiple transmission lines or circuit configurations
- **Markers and data labels:** Highlight specific impedance points with customizable shapes and labels
- **Smart labels:** Automatically position labels to avoid overlaps with connector lines
- **Legend support:** Display series names with toggle visibility
- **Interactive tooltips:** Show resistance/reactance values on hover
- **Export and print:** Save charts as PNG, JPEG, SVG, or PDF
- **Customization:** Full control over colors, fonts, borders, and styling
- **Responsive design:** Adapts to container dimensions

**Component:** `SfSmithChart` (Syncfusion.Blazor.Charts namespace)

**Core API surface:**
- `SfSmithChart` for the main chart
- `RenderType` for impedance or admittance rendering
- `SmithChartEvents` for event wiring
- `SmithChartSeriesCollection` and `SmithChartSeries` for data binding
- `SmithChartHorizontalAxis` and `SmithChartRadialAxis` for axis configuration
- `SmithChartLegendSettings` for legend placement and formatting
- `SmithChartTitle` and `SmithChartSubtitle` for chart headings
- `SmithChartSeriesMarker`, `SmithChartSeriesDatalabel`, `SmithChartSeriesDataLabelBorder`, and `SmithChartSeriesTooltip` for point annotations
- `SmithChartBorder` and `SmithChartMargin` for layout and container styling

**Common enum types:**
- `RenderType`
- `SmithChartAlignment`
- `SmithChartLabelIntersectAction`
- `AxisLabelPosition`
- `LegendPosition`
- `Shape`
- `ExportType`

## Documentation and Navigation Guide

### API Reference

📄 **Read:** [references/api-reference.md](references/api-reference.md)

Use this as the authoritative Smith Chart API summary for:
- `SfSmithChart` properties and methods
- Smith Chart events and event args
- Nested components, classes, and enums

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

Start here for installation and initial setup:
- Installing Syncfusion.Blazor.SmithChart NuGet package
- Setting up Blazor Server, WebAssembly, or Web App projects
- Registering services and theme references
- Creating your first Smith Chart with basic data binding
- Troubleshooting common setup issues

### Series and Data Binding

📄 **Read:** [references/series-and-data-binding.md](references/series-and-data-binding.md)

Learn how to bind data and configure series:
- Structuring data with Resistance and Reactance properties
- Configuring SmithChartSeries with Fill, Opacity, Width, Visible
- Adding multiple series for comparison
- Customizing individual series appearance
- Handling complex transmission line data

### Axes Configuration

📄 **Read:** [references/axes-configuration.md](references/axes-configuration.md)

Configure horizontal and radial axes:
- Understanding horizontal axis (straight line) vs radial axis (circular)
- Customizing axis labels (position, font, intersection handling)
- Styling axis lines and grid appearance
- Controlling label visibility and placement (Inside/Outside)
- Font customization (FontFamily, FontWeight, FontStyle, Size, Color)

### Markers and Data Labels

📄 **Read:** [references/markers-and-data-labels.md](references/markers-and-data-labels.md)

Add visual markers and data labels to series points:
- Enabling and customizing marker shapes (Circle, Rectangle, Triangle)
- Configuring marker size, color, opacity, and borders
- Displaying data labels with resistance/reactance values
- Using smart labels with connector lines to avoid overlaps
- Creating custom data label templates

### Legend Configuration

📄 **Read:** [references/legend-configuration.md](references/legend-configuration.md)

Display and customize legends for series identification:
- Positioning legends (Top, Bottom, Left, Right, Custom)
- Customizing legend shapes, colors, and borders
- Configuring legend alignment and sizing
- Adding legend titles with custom styling
- Enabling toggle visibility to show/hide series
- Organizing legends in rows and columns

### Tooltips

📄 **Read:** [references/tooltips.md](references/tooltips.md)

Enable interactive tooltips for data exploration:
- Showing resistance and reactance values on hover
- Customizing tooltip appearance (fill color, opacity, borders)
- Creating custom tooltip templates
- Accessing SmithChartPoint data in templates

### Chart Dimensions and Titles

📄 **Read:** [references/chart-dimensions-and-titles.md](references/chart-dimensions-and-titles.md)

Configure chart size, titles, and layout:
- Setting chart width and height (fixed or responsive)
- Adding and styling chart titles and subtitles
- Configuring margins and padding
- Customizing background colors
- Creating responsive Smith Charts

### Export, Print, Events, and Accessibility

📄 **Read:** [references/export-print-events-accessibility.md](references/export-print-events-accessibility.md)

Export charts, handle events, and ensure accessibility:
- Exporting to PNG, JPEG, SVG, and PDF formats
- Printing Smith Charts for documentation
- Handling component events (Loaded, SeriesRender, AxisLabelRendering, LegendRendering, TitleRendering, SubtitleRendering, TextRendering, TooltipRender, OnPrintComplete, OnExportComplete, SizeChanged)
- Implementing WCAG-compliant accessible charts
- Supporting keyboard navigation and screen readers
- Testing accessibility compliance

## Quick Start Example

Here's a basic Smith Chart showing a transmission line:

```razor
@page "/smith-chart-basic"
@using Syncfusion.Blazor.Charts

<h3>Transmission Line Impedance</h3>

<SfSmithChart>
    <SmithChartSeriesCollection>
        <SmithChartSeries Name="Transmission Line 1" 
                          DataSource="@TransmissionData" 
                          Reactance="Reactance" 
                          Resistance="Resistance">
            <SmithChartSeriesMarker Visible="true"></SmithChartSeriesMarker>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }
    
    public List<SmithChartData> TransmissionData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10, Reactance = 25 },
        new SmithChartData { Resistance = 6, Reactance = 4.5 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 },
        new SmithChartData { Resistance = 2, Reactance = 1.2 },
        new SmithChartData { Resistance = 1, Reactance = 0.8 },
        new SmithChartData { Resistance = 0, Reactance = 0.2 }
    };
}
```

## Common Use Cases

### RF Impedance Matching Analysis

```razor
@using Syncfusion.Blazor.Charts

<SfSmithChart Height="600px" Width="100%">
    <SmithChartTitle Text="Antenna Impedance Matching"></SmithChartTitle>
    <SmithChartLegendSettings Visible="true" Position="@Syncfusion.Blazor.Charts.LegendPosition.Bottom">
    </SmithChartLegendSettings>
    <SmithChartSeriesCollection>
        <SmithChartSeries Name="Before Matching" 
                          DataSource="@BeforeData" 
                          Reactance="Reactance" 
                          Resistance="Resistance"
                          Fill="#FF6B6B">
            <SmithChartSeriesMarker Visible="true" Shape="@Syncfusion.Blazor.Charts.Shape.Circle"></SmithChartSeriesMarker>
            <SmithChartSeriesTooltip Visible="true"></SmithChartSeriesTooltip>
        </SmithChartSeries>
        <SmithChartSeries Name="After Matching" 
                          DataSource="@AfterData" 
                          Reactance="Reactance" 
                          Resistance="Resistance"
                          Fill="#4ECDC4">
            <SmithChartSeriesMarker Visible="true" Shape="@Syncfusion.Blazor.Charts.Shape.Diamond"></SmithChartSeriesMarker>
            <SmithChartSeriesTooltip Visible="true"></SmithChartSeriesTooltip>
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }

    public List<SmithChartData> BeforeData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10,  Reactance = 25 },
        new SmithChartData { Resistance = 6,   Reactance = 4.5 },
        new SmithChartData { Resistance = 3.5, Reactance = 1.6 },
        new SmithChartData { Resistance = 2,   Reactance = 1.2 },
        new SmithChartData { Resistance = 1,   Reactance = 0.8 },
        new SmithChartData { Resistance = 0,   Reactance = 0.2 }
    };

    public List<SmithChartData> AfterData = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 2.5, Reactance = 1.2 },
        new SmithChartData { Resistance = 2, Reactance = 0.8 },
        new SmithChartData { Resistance = 1.6, Reactance = 0.5 },
        new SmithChartData { Resistance = 1.3, Reactance = 0.3 },
        new SmithChartData { Resistance = 1.1, Reactance = 0.1 },
        new SmithChartData { Resistance = 1, Reactance = 0 }
    };
}
```

### Multiple Frequency Response Analysis

```razor
@using Syncfusion.Blazor.Charts

<SfSmithChart>
    <SmithChartTitle Text="Frequency Sweep: 100MHz - 1GHz"></SmithChartTitle>
    <SmithChartLegendSettings Visible="true" Position="@Syncfusion.Blazor.Charts.LegendPosition.Right">
    </SmithChartLegendSettings>
    <SmithChartSeriesCollection>
        <SmithChartSeries Name="100MHz" DataSource="@Freq100Data" 
                          Reactance="Reactance" Resistance="Resistance" Width="2">
        </SmithChartSeries>
        <SmithChartSeries Name="500MHz" DataSource="@Freq500Data" 
                          Reactance="Reactance" Resistance="Resistance" Width="2">
        </SmithChartSeries>
        <SmithChartSeries Name="1GHz" DataSource="@Freq1000Data" 
                          Reactance="Reactance" Resistance="Resistance" Width="2">
        </SmithChartSeries>
    </SmithChartSeriesCollection>
</SfSmithChart>

@code {
    public class SmithChartData
    {
        public double? Resistance { get; set; }
        public double? Reactance { get; set; }
    }

    public List<SmithChartData> Freq100Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 10,  Reactance = 20 },
        new SmithChartData { Resistance = 8,   Reactance = 12 },
        new SmithChartData { Resistance = 6,   Reactance = 6 },
        new SmithChartData { Resistance = 4,   Reactance = 3 },
        new SmithChartData { Resistance = 2,   Reactance = 1.5 },
        new SmithChartData { Resistance = 1,   Reactance = 0.8 }
    };

    public List<SmithChartData> Freq500Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 8,   Reactance = 15 },
        new SmithChartData { Resistance = 6,   Reactance = 8 },
        new SmithChartData { Resistance = 4.5, Reactance = 4 },
        new SmithChartData { Resistance = 3,   Reactance = 2 },
        new SmithChartData { Resistance = 1.8, Reactance = 1 },
        new SmithChartData { Resistance = 1.2, Reactance = 0.4 }
    };

    public List<SmithChartData> Freq1000Data = new List<SmithChartData>
    {
        new SmithChartData { Resistance = 6,   Reactance = 10 },
        new SmithChartData { Resistance = 4.5, Reactance = 5 },
        new SmithChartData { Resistance = 3,   Reactance = 2.5 },
        new SmithChartData { Resistance = 2,   Reactance = 1.2 },
        new SmithChartData { Resistance = 1.2, Reactance = 0.5 },
        new SmithChartData { Resistance = 1, Reactance = 0 }
    };
}
```

## Key Properties

| Property | Type | Description | When to Use |
|----------|------|-------------|-------------|
| `SmithChartSeriesCollection` | Collection | Container for all series | Always required to display data |
| `DataSource` | IEnumerable | List of impedance data points | Bind your resistance/reactance data |
| `Resistance` | string | Property name for resistance values | Map to your data model property |
| `Reactance` | string | Property name for reactance values | Map to your data model property |
| `RenderType` | Enum | Choose impedance or admittance rendering | Switch the Smith Chart coordinate interpretation |
| `Radius` | string | Radius of the Smith Chart plot area | Control the circular plot size |
| `ElementSpacing` | string | Spacing between chart elements | Tweak compact or dense layouts |
| `Background` | string | Chart background color | Match the host UI theme |
| `Theme` | string | Built-in Smith Chart theme | Keep visual styling consistent |
| `ID` | string | Chart element identifier | Useful for testing or scripting |
| `SmithChartSeriesMarker` | Component | Marker configuration | Highlight data points visually |
| `SmithChartSeriesTooltip` | Component | Tooltip configuration | Show values on hover |
| `SmithChartLegendSettings` | Component | Legend configuration | Identify multiple series |
| `SmithChartHorizontalAxis` | Component | Horizontal axis settings | Customize straight-line axis |
| `SmithChartRadialAxis` | Component | Radial axis settings | Customize circular axis |
| `Width` | string | Chart width (px or %) | Control chart dimensions |
| `Height` | string | Chart height (px or %) | Control chart dimensions |

## Implementation Workflow

1. **Install Package** → Add Syncfusion.Blazor.SmithChart NuGet package
2. **Register Services** → Add services in Program.cs/Startup.cs
3. **Add Theme** → Reference Syncfusion theme CSS
4. **Prepare Data** → Structure data with Resistance and Reactance properties
5. **Add Component** → Place `<SfSmithChart>` in your Razor page
6. **Bind Series** → Configure `SmithChartSeries` with DataSource
7. **Customize** → Add markers, tooltips, legends, and styling
8. **Test** → Verify data visualization and interactivity

## Edge Cases and Considerations

- **Data Range:** Smith Chart represents normalized impedance (typically 0-∞). Ensure data is appropriately normalized.
- **Series Visibility:** With many overlapping series, use distinct colors and toggle visibility via legend.
- **Null Values:** Use nullable double (double?) for Resistance and Reactance to handle missing data gracefully.
- **Performance:** Large datasets (>1000 points per series) may impact rendering. Consider data sampling.
- **Responsive Design:** Use percentage-based Width/Height or container queries for responsive layouts.
- **Accessibility:** Always enable tooltips and ARIA labels for screen reader support in RF engineering applications.

## When NOT to Use This Component

- **General XY Plotting:** Use regular Chart component for standard Cartesian plots
- **Financial Data:** Use Stock Chart for time-series financial data
- **Non-RF Data:** Smith Charts are specialized for impedance/admittance; use appropriate chart types for other domains
- **3D Impedance:** Smith Charts are 2D; complex 3D RF visualization requires different approaches
