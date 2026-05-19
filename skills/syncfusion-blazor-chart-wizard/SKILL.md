---
name: syncfusion-blazor-chart-wizard
description: "Implements the Syncfusion Blazor Chart Wizard for guided chart creation. Use this when building interactive chart builders that require step-by-step configuration, data binding, and export features. Covers chart customization, theming, and accessibility."
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Data Visualization"
---

# Implementing Syncfusion Blazor Chart Wizard

**NuGet:** `Syncfusion.Blazor.ChartWizard` + `Syncfusion.Blazor.Themes`  
**Namespace:** `Syncfusion.Blazor.ChartWizard`

The Syncfusion Blazor Chart Wizard is a comprehensive, user-friendly component that guides users through interactive chart creation and customization. It provides a dialog-based interface with property panels, multiple chart types, data binding, theming, accessibility features, and export capabilities.

## When to Use This Skill

Use this skill when:

- **Interactive Chart Creation**: Building applications that allow users to create and customize charts interactively
- **Data Visualization Tools**: Implementing dashboard builders, report generators, or analytics interfaces
- **Guided Chart Configuration**: Providing step-by-step chart setup with visual property panels
- **Multi-Format Export**: Requiring chart export to PNG, JPEG, SVG, PDF, CSV, or XLSX formats
- **Theme-Aware Charts**: Building charts that adapt to application themes (Material, Fluent, Bootstrap, Tailwind, etc.)
- **Accessible Visualizations**: Creating WCAG 2.2 AA compliant charts with keyboard navigation and screen reader support
- **Chart State Persistence**: Saving and loading chart configurations for user preferences or templates
- **Blazor Applications**: Implementing in Blazor Server, WebAssembly, or Web App architectures

## Component Overview

The Chart Wizard component provides:

- **Interactive Chart Builder**: Step-by-step guided interface for chart creation
- **15+ Chart Types**: Line, Column, Bar, Area, Pie, Scatter, and stacked variations
- **Property Panel**: Real-time chart customization with visual controls
- **Data Binding**: Support for IEnumerable, List, and ObservableCollection data sources
- **Multi-Series Support**: Create charts with multiple data series and categories
- **Export Capabilities**: Export to image (PNG, JPEG, SVG), document (PDF), and data formats (CSV, XLSX)
- **Serialization**: Save and load complete chart configurations as JSON
- **Theming**: 20+ built-in themes with consistent styling
- **Accessibility**: Full WCAG 2.2 AA compliance with keyboard navigation
- **RTL Support**: Right-to-left layout for international applications

## Documentation and Navigation Guide

### Getting Started

Choose the appropriate setup guide based on your Blazor application type:

#### **Blazor Server App Setup**
📄 **Read:** [references/getting-started.md](references/getting-started.md)

Complete setup instructions for Blazor Server applications, including:
- Visual Studio, VS Code, and .NET CLI project creation
- NuGet package installation (Syncfusion.Blazor.ChartWizard)
- Namespace imports and service registration
- Script reference configuration
- Component implementation
- Data model creation and binding
- Theme configuration
- Complete working example with Olympics data
- Troubleshooting common setup issues

#### **Blazor WebAssembly (WASM) App Setup**
📄 **Read:** [references/getting-started-wasm.md](references/getting-started-wasm.md)

Complete setup instructions for Blazor WASM applications, including:
- WASM-specific project creation across IDEs
- Package installation with dependency restoration
- Configuration for client-side rendering
- Component setup and data binding
- Theme configuration for WASM
- Complete functional WASM example
- Performance optimization considerations

#### **Blazor Web App Setup**
📄 **Read:** [references/getting-started-web-app.md](references/getting-started-web-app.md)

Complete setup instructions for Blazor Web Apps with render modes, including:
- Interactive render mode configuration (Server, WebAssembly, Auto)
- Interactivity location setup (Global, Per page/component)
- Client vs Server project package management
- Render mode-specific component implementation
- Theme management across render modes
- Complete Web App example with Auto mode
- Best practices for choosing render modes

### Working with Data

📄 **Read:** [references/working-with-data.md](references/working-with-data.md)

Comprehensive data binding and field configuration, including:
- ChartSettings configuration and properties
- DataSource binding (IEnumerable<Object>)
- CategoryFields configuration for x-axis grouping
- SeriesFields for multi-series data mapping
- SeriesType selection (15+ chart types)
- Single-series vs multi-series chart patterns
- List<T> data binding examples
- ObservableCollection for dynamic data
- Data model design best practices
- Field mapping and ordering
- Complete examples with Olympics and sales data
- Troubleshooting data display issues

### Appearance and Customization

📄 **Read:** [references/appearance-customization.md](references/appearance-customization.md)

Visual customization and layout configuration, including:
- Width and Height properties (percentage and pixel-based)
- Responsive design patterns
- Theme property configuration (20+ built-in themes)
  - Material, Material3, MaterialDark, Material3Dark
  - Fluent, Fluent2, FluentDark, Fluent2Dark, Fluent2HighContrast
  - Bootstrap, Bootstrap4, Bootstrap5, BootstrapDark, Bootstrap5Dark
  - Tailwind, Tailwind3, TailwindDark, Tailwind3Dark
  - Fabric, FabricDark
  - HighContrast, HighContrastLight
- Theme stylesheet prerequisites and inclusion
- EnableRtl for right-to-left layout support
- PropertyPanelExpanded for initial panel state control
- Combined appearance configuration examples
- Theme consistency best practices
- Troubleshooting theme and sizing issues

### Accessibility

📄 **Read:** [references/accessibility.md](references/accessibility.md)

Comprehensive accessibility compliance and features, including:
- WCAG 2.2 AA level support
- Section 508 compliance
- WAI-ARIA attributes and roles
  - Data labels, legends, axis titles
  - Chart titles and series points
  - ARIA roles: img, button, region
  - ARIA attributes: label, hidden, pressed
- Screen reader support
- Complete keyboard navigation
  - Focus management (Alt+J, Tab, Shift+Tab)
  - Data point navigation (Arrow keys)
  - Series selection (Enter, Space)
  - Legend navigation and toggling
  - Zoom and pan controls
  - Print shortcut (Ctrl+P)
- Color contrast compliance
- Mobile device support
- Axe-core validation integration
- Testing guidelines and best practices

### Serialization

📄 **Read:** [references/serialization.md](references/serialization.md)

Chart state save and load functionality, including:
- SaveChart() method - serialize to JSON string
- LoadChartAsync() method - restore from JSON
- Complete chart state capture (settings, series, axes, titles, styles)
- Storage options (database, file, LocalStorage, session state)
- Save and load button implementation
- Component reference setup (@ref)
- State management patterns
- Version compatibility considerations
- Use cases: user preferences, dashboard configs, report templates
- Error handling and validation
- Complete working examples

### Print and Export

📄 **Read:** [references/print-export.md](references/print-export.md)

Export charts to multiple formats, including:
- Supported formats: PNG, JPEG, SVG, PDF, CSV, XLSX, PRINT
- ChartExportSettings declarative configuration
  - FileName property
  - Width and Height for image/PDF exports
  - Orientation (Portrait/Landscape) for PDF/print
- Export button integration
- Print functionality via browser dialog
- Exporting event for runtime customization
  - ChartExportingEventArgs properties
  - Dynamic file naming
  - Conditional export cancellation
  - Runtime dimension adjustment
  - Orientation control
- Format-specific considerations
- Best practices for resolution and file naming
- Complete examples with event handling
- Troubleshooting export issues

### API Reference

📄 **Read:** [references/api-reference.md](references/api-reference.md)

Complete API documentation for all classes, properties, methods, and events, including:
- SfChartWizard class
  - Width, Height, Theme properties
  - EnableRtl, PropertyPanelExpanded
  - Exporting event
  - SaveChart(), LoadChartAsync() methods
- ChartSettings class
  - Datasource, CategoryFields, SeriesFields
  - SeriesType property with 15+ types
- ChartExportSettings class
  - FileName, Width, Height, Orientation
- ChartExportingEventArgs class
  - Event arguments for export customization
- ChartWizardSeriesType enum
  - All available series types
- PageOrientation enum
  - Portrait and Landscape options

## Quick Start Example

Here's a minimal example to get you started with the Chart Wizard component:

```cshtml
@using Syncfusion.Blazor.ChartWizard

<SfChartWizard Width="100%" Height="500px">
    <ChartSettings DataSource="@Data"
                   CategoryFields="@CategoryList"
                   SeriesFields="@SeriesList"
                   SeriesType="ChartWizardSeriesType.Column">
    </ChartSettings>
</SfChartWizard>

@code {
    public class OlympicsData
    {
        public string Country { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }

    private readonly List<string> CategoryList = new() { "Country" };
    private readonly List<string> SeriesList = new() { "Gold", "Silver", "Bronze" };

    private List<OlympicsData> Data = new()
    {
        new OlympicsData { Country = "USA",   Gold = 40, Silver = 44, Bronze = 42 },
        new OlympicsData { Country = "China", Gold = 40, Silver = 27, Bronze = 24 },
        new OlympicsData { Country = "Japan", Gold = 20, Silver = 12, Bronze = 13 }
    };
}
```

## Common Patterns

### Single-Series Chart

```cshtml
@using Syncfusion.Blazor.ChartWizard

<SfChartWizard Width="100%" Height="500px">
    <ChartSettings DataSource="@SalesData"
                   CategoryFields="@CategoryList"
                   SeriesFields="@SeriesList"
                   SeriesType="ChartWizardSeriesType.Line">
    </ChartSettings>
</SfChartWizard>

@code {
    public class SalesInfo
    {
        public string Month { get; set; }
        public double Sales { get; set; }
    }

    private List<SalesInfo> SalesData = new()
    {
        new SalesInfo { Month = "Jan", Sales = 100 },
        new SalesInfo { Month = "Feb", Sales = 120 },
        new SalesInfo { Month = "Mar", Sales = 140 }
    };

    private readonly List<string> CategoryList = new() { "Month" };
    private readonly List<string> SeriesList = new() { "Sales" };
}
```

### Multi-Series with Theme

```cshtml
@using Syncfusion.Blazor.ChartWizard

<SfChartWizard Theme="Theme.Fluent2" Width="100%" Height="500px">
    <ChartSettings DataSource="@OlympicsData"
                   CategoryFields="@CategoryList"
                   SeriesFields="@SeriesList"
                   SeriesType="ChartWizardSeriesType.Bar">
    </ChartSettings>
</SfChartWizard>

@code {
    private readonly List<string> CategoryList = new() { "Country" };
    private readonly List<string> SeriesList = new() { "Gold", "Silver", "Bronze" };

    public class OlympicsDataModel
    {
        public string Country { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }

    private List<OlympicsDataModel> OlympicsData = new()
    {
        new OlympicsDataModel { Country = "USA",   Gold = 40, Silver = 44, Bronze = 42 },
        new OlympicsDataModel { Country = "China", Gold = 40, Silver = 27, Bronze = 24 },
        new OlympicsDataModel { Country = "Japan", Gold = 20, Silver = 12, Bronze = 13 }
    };
}
```

### With Export Configuration

```cshtml
@using Syncfusion.Blazor.ChartWizard

<SfChartWizard Width="100%" Height="500px">
    <ChartSettings DataSource="@ReportData"
                   CategoryFields="@CategoryList"
                   SeriesFields="@SeriesList"
                   SeriesType="ChartWizardSeriesType.Column">

        <ChartExportSettings FileName="QuarterlyReport"
                             Width="1024"
                             Height="768"
                             Orientation="Syncfusion.Blazor.ChartWizard.PageOrientation.Landscape" />
    </ChartSettings>
</SfChartWizard>

@code {
    private readonly List<string> CategoryList = new() { "Quarter" };
    private readonly List<string> SeriesList = new() { "Revenue", "Profit" };

    public class ReportItem
    {
        public string Quarter { get; set; }
        public double Revenue { get; set; }
        public double Profit { get; set; }
    }

    private List<ReportItem> ReportData = new()
    {
        new ReportItem { Quarter = "Q1", Revenue = 120000, Profit = 45000 },
        new ReportItem { Quarter = "Q2", Revenue = 150000, Profit = 60000 },
        new ReportItem { Quarter = "Q3", Revenue = 170000, Profit = 75000 },
        new ReportItem { Quarter = "Q4", Revenue = 200000, Profit = 90000 }
    };
}
```

### With Property Panel Collapsed

```cshtml
@using Syncfusion.Blazor.ChartWizard

<SfChartWizard Width="90%"
               Height="600px"
               PropertyPanelExpanded="false"
               Theme="Theme.Material3">

    <ChartSettings DataSource="@MyData"
                   CategoryFields="@CategoryFields"
                   SeriesFields="@SeriesFields"
                   SeriesType="ChartWizardSeriesType.Area">
    </ChartSettings>

</SfChartWizard>

@code {
    private readonly List<string> CategoryFields = new() { "CategoryName" };
    private readonly List<string> SeriesFields = new() { "Value1", "Value2" };

    public class MyItem
    {
        public string CategoryName { get; set; }
        public double Value1 { get; set; }
        public double Value2 { get; set; }
    }

    private List<MyItem> MyData = new()
    {
        new MyItem { CategoryName = "A", Value1 = 30, Value2 = 20 },
        new MyItem { CategoryName = "B", Value1 = 45, Value2 = 35 },
        new MyItem { CategoryName = "C", Value1 = 50, Value2 = 40 }
    };
}
```

## Key Properties

### SfChartWizard Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| **Width** | string | "100%" | Width of the component (px or %) |
| **Height** | string | "100%" | Height of the component (px or %) |
| **Theme** | Theme | Fluent2 | Visual theme (Material, Fluent, Bootstrap, Tailwind, etc.) |
| **EnableRtl** | bool | false | Enable right-to-left layout |
| **PropertyPanelExpanded** | bool | true | Show property panel on initial render |

### ChartSettings Properties

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| **DataSource** | IEnumerable<object> | Yes | Data collection for the chart |
| **CategoryFields** | List<string> | Yes | Field names for x-axis categories |
| **SeriesFields** | List<string> | Yes | Field names for data series |
| **SeriesType** | ChartWizardSeriesType | No | Chart type (Line, Column, Bar, Area, Pie, etc.) |

### ChartExportSettings Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| **FileName** | string | "" | Name for exported file (without extension) |
| **Width** | double | NaN | Export width in pixels |
| **Height** | double | NaN | Export height in pixels |
| **Orientation** | PageOrientation | Portrait | Page orientation for PDF/print |

## Common Use Cases

### Business Analytics Dashboard

```cshtml
@using Syncfusion.Blazor.ChartWizard

<SfChartWizard Width="100%"
               Height="600px"
               Theme="Theme.Bootstrap5">

    <ChartSettings DataSource="@QuarterlySales"
                   CategoryFields="@CategoryFields"
                   SeriesFields="@SeriesFields"
                   SeriesType="ChartWizardSeriesType.StackingColumn">

        <ChartExportSettings FileName="QuarterlyAnalysis"
                             Orientation="Syncfusion.Blazor.ChartWizard.PageOrientation.Landscape" />
    </ChartSettings>

</SfChartWizard>

@code {
    private readonly List<string> CategoryFields = new() { "Quarter", "Region" };
    private readonly List<string> SeriesFields = new() { "Revenue", "Profit", "Expenses" };

    public class QuarterlySale
    {
        public string Quarter { get; set; }
        public string Region { get; set; }
        public double Revenue { get; set; }
        public double Profit { get; set; }
        public double Expenses { get; set; }
    }

    private List<QuarterlySale> QuarterlySales = new()
    {
        new QuarterlySale { Quarter = "Q1", Region = "North", Revenue = 150000, Profit = 50000, Expenses = 30000 },
        new QuarterlySale { Quarter = "Q2", Region = "North", Revenue = 170000, Profit = 60000, Expenses = 35000 },
        new QuarterlySale { Quarter = "Q3", Region = "South", Revenue = 160000, Profit = 55000, Expenses = 32000 },
        new QuarterlySale { Quarter = "Q4", Region = "South", Revenue = 180000, Profit = 65000, Expenses = 38000 }
    };
}
```

### Olympics Medal Comparison

```cshtml
@using Syncfusion.Blazor.ChartWizard

<SfChartWizard Theme="Theme.Material3"
               PropertyPanelExpanded="true"
               Width="100%"
               Height="500px">

    <ChartSettings DataSource="@OlympicsMedals"
                   CategoryFields="@CategoryFields"
                   SeriesFields="@SeriesFields"
                   SeriesType="ChartWizardSeriesType.Bar">
    </ChartSettings>

</SfChartWizard>

@code {
    private readonly List<string> CategoryFields = new() { "Country" };
    private readonly List<string> SeriesFields = new() { "Gold", "Silver", "Bronze" };

    public class OlympicsMedalInfo
    {
        public string Country { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }

    private List<OlympicsMedalInfo> OlympicsMedals = new()
    {
        new OlympicsMedalInfo { Country = "USA",   Gold = 40, Silver = 44, Bronze = 42 },
        new OlympicsMedalInfo { Country = "China", Gold = 40, Silver = 27, Bronze = 24 },
        new OlympicsMedalInfo { Country = "Japan", Gold = 20, Silver = 12, Bronze = 13 }
    };
}
```

### Sales Trend Visualization

```cshtml
@using Syncfusion.Blazor.ChartWizard

<SfChartWizard Width="90%"
               Height="500px">

    <ChartSettings DataSource="@MonthlySales"
                   CategoryFields="@CategoryFields"
                   SeriesFields="@SeriesFields"
                   SeriesType="ChartWizardSeriesType.Line">
    </ChartSettings>

</SfChartWizard>

@code {
    private readonly List<string> CategoryFields = new() { "Month" };
    private readonly List<string> SeriesFields = new() { "CurrentYear", "PreviousYear" };

    public class SalesRecord
    {
        public string Month { get; set; }
        public double CurrentYear { get; set; }
        public double PreviousYear { get; set; }
    }

    private List<SalesRecord> MonthlySales = new()
    {
        new SalesRecord { Month = "Jan", CurrentYear = 12000, PreviousYear = 10000 },
        new SalesRecord { Month = "Feb", CurrentYear = 13500, PreviousYear = 11000 },
        new SalesRecord { Month = "Mar", CurrentYear = 15000, PreviousYear = 12500 }
    };
}
```

### Multi-Category Population Data

```cshtml
@using Syncfusion.Blazor.ChartWizard

<SfChartWizard EnableRtl="false"
               Theme="Theme.Fluent2"
               Width="100%"
               Height="500px">

    <ChartSettings DataSource="@CityPopulation"
                   CategoryFields="@CategoryFields"
                   SeriesFields="@SeriesFields"
                   SeriesType="ChartWizardSeriesType.Column">
    </ChartSettings>

</SfChartWizard>

@code {
    private readonly List<string> CategoryFields = new() { "City", "Country" };
    private readonly List<string> SeriesFields = new() { "Population" };

    public class PopulationInfo
    {
        public string City { get; set; }
        public string Country { get; set; }
        public double Population { get; set; }
    }

    private List<PopulationInfo> CityPopulation = new()
    {
        new PopulationInfo { City = "Tokyo",       Country = "Japan",  Population = 37.4 },
        new PopulationInfo { City = "Delhi",       Country = "India",  Population = 31.0 },
        new PopulationInfo { City = "Shanghai",    Country = "China",  Population = 27.0 },
        new PopulationInfo { City = "São Paulo",   Country = "Brazil", Population = 22.0 }
    };
}
```

## Available Chart Types (SeriesType)

- **Line** - Line chart with connected data points
- **StackingLine** - Stacked line chart
- **StackingLine100** - 100% stacked line chart
- **Column** - Vertical bar chart
- **StackingColumn** - Stacked column chart
- **StackingColumn100** - 100% stacked column chart
- **Bar** - Horizontal bar chart
- **StackingBar** - Stacked bar chart
- **StackingBar100** - 100% stacked bar chart
- **Area** - Area chart with filled regions
- **StackingArea** - Stacked area chart
- **StackingArea100** - 100% stacked area chart
- **Scatter** - Scatter plot with individual markers
- **Pie** - Circular pie chart for proportions

## Prerequisites

- .NET SDK 6.0 or later
- Syncfusion Blazor license (trial or commercial)
- Blazor Server, WebAssembly, or Web App project
- Visual Studio 2022, VS Code, or .NET CLI

## Package Requirements

```bash
dotnet add package Syncfusion.Blazor.ChartWizard
```

## Namespace Imports

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.ChartWizard
```

## Service Registration

```csharp
// Program.cs
builder.Services.AddSyncfusionBlazor();
```

## Script Reference

```html
<!-- App.razor or _Host.cshtml -->
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
```

## Next Steps

1. Choose your Blazor application type (Server, WASM, or Web App)
2. Read the appropriate getting started guide
3. Configure data binding with your data models
4. Customize appearance with themes and properties
5. Implement export and serialization as needed
6. Ensure accessibility compliance for production use

## Related Components

- **Syncfusion Blazor Charts** - For programmatic chart creation without wizard interface
- **Syncfusion Blazor DataGrid** - For tabular data visualization
- **Syncfusion Blazor Dashboard Layout** - For arranging multiple charts

## Support Resources

- [Official Documentation](https://blazor.syncfusion.com/documentation/chart-wizard/getting-started)
- [API Reference](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.ChartWizard.html)
- [Live Demos](https://blazor.syncfusion.com/demos/chart-wizard/default-functionalities)
- [Community Forums](https://www.syncfusion.com/forums/blazor)
