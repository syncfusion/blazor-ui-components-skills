# Customizing Appearance in the Blazor Chart Wizard Component

## Table of Contents

1. [Overview](#overview)
2. [Appearance Properties Overview](#appearance-properties-overview)
3. [Width and Height Configuration](#width-and-height-configuration)
   - [Percentage-Based Sizing](#percentage-based-sizing)
   - [Pixel-Based Sizing](#pixel-based-sizing)
   - [Responsive Design Considerations](#responsive-design-considerations)
4. [Theme Customization](#theme-customization)
   - [Available Themes](#available-themes)
   - [Applying Themes](#applying-themes)
   - [Theme Scope](#theme-scope)
5. [Right-to-Left (RTL) Support](#right-to-left-rtl-support)
   - [EnableRtl Property](#enablertl-property)
   - [RTL Layout Behavior](#rtl-layout-behavior)
6. [Property Panel Control](#property-panel-control)
   - [PropertyPanelExpanded Property](#propertypanelexpanded-property)
   - [Default Behavior](#default-behavior)
7. [Complete Implementation Examples](#complete-implementation-examples)
   - [Basic Width and Height](#basic-width-and-height)
   - [Theme Application](#theme-application)
   - [RTL Implementation](#rtl-implementation)
   - [Property Panel Configuration](#property-panel-configuration)
   - [Combined Configuration](#combined-configuration)
8. [Best Practices](#best-practices)
9. [Troubleshooting](#troubleshooting)
   - [Common Issues](#common-issues)
   - [Solutions](#solutions)

---

## Overview

This guide explains how to tailor the appearance of the `Chart Wizard` component in Blazor. Discover the available properties and see practical examples for each customization option.

The Chart Wizard provides comprehensive appearance customization options including:
- **Flexible sizing** with percentage and pixel values
- **Multiple built-in themes** for different visual styles
- **RTL support** for internationalization
- **Property panel control** for optimized layouts

---

## Appearance Properties Overview

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Width` | string | "100%" | Sets the width of the Chart Wizard (e.g., "800px", "50%"). |
| `Height` | string | "100%" | Sets the height of the Chart Wizard (e.g., "600px", "75%"). |
| `Theme` | Theme | Material | Sets the visual theme for the component and its sub-components. |
| `EnableRtl` | bool | false | Enables right-to-left layout for RTL languages. |
| `PropertyPanelExpanded` | bool | true | Shows or hides the property panel on initial render. |

---

## Width and Height Configuration

You can control the size of the `Chart Wizard` by specifying the `Width` and `Height` properties. Use pixel values for fixed sizing or percentages for responsive layouts.

### Percentage-Based Sizing

Percentage values make the Chart Wizard responsive to its container size, ideal for flexible layouts.

```cshtml
@using Syncfusion.Blazor.ChartWizard

<div class="control-section">
    <SfChartWizard Width="60%" Height="80%">
        <ChartSettings DataSource="@OlympicsDataSource"
                       CategoryFields="@categories"
                       SeriesType="ChartWizardSeriesType.Bar"
                       SeriesFields="@chartSeries">
        </ChartSettings>
    </SfChartWizard>
</div>

@code {
    private readonly List<string> chartSeries = new() { "Gold", "Silver", "Bronze" };
    private readonly List<string> categories = new() { "Country", "CountryCode" };

    public class OlympicsData
    {
        public string? Country { get; set; }
        public string? CountryCode { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }

    private readonly List<OlympicsData> OlympicsDataSource = new()
    {
        new OlympicsData { Country = "USA", CountryCode = "USA", Gold = 40, Silver = 44, Bronze = 42 },
        new OlympicsData { Country = "China", CountryCode = "CHN", Gold = 40, Silver = 27, Bronze = 24 },
        new OlympicsData { Country = "Great Britain", CountryCode = "GBR", Gold = 14, Silver = 22, Bronze = 29 },
        new OlympicsData { Country = "France", CountryCode = "FRA", Gold = 16, Silver = 26, Bronze = 22 },
        new OlympicsData { Country = "Australia", CountryCode = "AUS", Gold = 18, Silver = 19, Bronze = 16 },
        new OlympicsData { Country = "Japan", CountryCode = "JPN", Gold = 20, Silver = 12, Bronze = 13 },
        new OlympicsData { Country = "Italy", CountryCode = "ITA", Gold = 12, Silver = 13, Bronze = 15 },
        new OlympicsData { Country = "Netherlands", CountryCode = "NLD", Gold = 15, Silver = 7,  Bronze = 12 },
        new OlympicsData { Country = "Germany", CountryCode = "DEU", Gold = 12, Silver = 13, Bronze = 8  },
        new OlympicsData { Country = "South Korea", CountryCode = "KOR", Gold = 13, Silver = 9,  Bronze = 10 }
    };
}
```

**Key Benefits:**
- Automatically adjusts to container size
- Ideal for responsive web applications
- Works well with CSS Grid and Flexbox layouts

### Pixel-Based Sizing

Pixel values provide precise, fixed dimensions regardless of the container size.

```cshtml
@using Syncfusion.Blazor.ChartWizard

<div class="control-section">
    <SfChartWizard Width="650px" Height="400px">
        <ChartSettings DataSource="@OlympicsDataSource"
                       CategoryFields="@categories"
                       SeriesType="ChartWizardSeriesType.Bar"
                       SeriesFields="@chartSeries">
        </ChartSettings>
    </SfChartWizard>
</div>

@code {
    private readonly List<string> chartSeries = new() { "Gold", "Silver", "Bronze" };
    private readonly List<string> categories = new() { "Country", "CountryCode" };

    public class OlympicsData
    {
        public string? Country { get; set; }
        public string? CountryCode { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }

    private readonly List<OlympicsData> OlympicsDataSource = new()
    {
        new OlympicsData { Country = "USA", CountryCode = "USA", Gold = 40, Silver = 44, Bronze = 42 },
        new OlympicsData { Country = "China", CountryCode = "CHN", Gold = 40, Silver = 27, Bronze = 24 },
        new OlympicsData { Country = "Great Britain", CountryCode = "GBR", Gold = 14, Silver = 22, Bronze = 29 },
        new OlympicsData { Country = "France", CountryCode = "FRA", Gold = 16, Silver = 26, Bronze = 22 },
        new OlympicsData { Country = "Australia", CountryCode = "AUS", Gold = 18, Silver = 19, Bronze = 16 },
        new OlympicsData { Country = "Japan", CountryCode = "JPN", Gold = 20, Silver = 12, Bronze = 13 },
        new OlympicsData { Country = "Italy", CountryCode = "ITA", Gold = 12, Silver = 13, Bronze = 15 },
        new OlympicsData { Country = "Netherlands", CountryCode = "NLD", Gold = 15, Silver = 7,  Bronze = 12 },
        new OlympicsData { Country = "Germany", CountryCode = "DEU", Gold = 12, Silver = 13, Bronze = 8  },
        new OlympicsData { Country = "South Korea", CountryCode = "KOR", Gold = 13, Silver = 9,  Bronze = 10 }
    };
}
```

**Key Benefits:**
- Consistent size across all screen resolutions
- Predictable layout behavior
- Easier to align with fixed-size elements

### Responsive Design Considerations

Combine both approaches for optimal responsive design:

```cshtml
<!-- Responsive container with max dimensions -->
<div style="max-width: 1200px; width: 100%; margin: 0 auto;">
    <SfChartWizard Width="100%" Height="600px">
        <ChartSettings DataSource="@OlympicsDataSource"
                       CategoryFields="@categories"
                       SeriesType="ChartWizardSeriesType.Bar"
                       SeriesFields="@chartSeries">
        </ChartSettings>
    </SfChartWizard>
</div>
```

---

## Theme Customization

The `Theme` property applies a built-in visual style to the chart. Choose from available themes to match the application's look and feel.

### Available Themes

The Chart Wizard supports the following themes:

| Theme | Enum Value | Description |
|-------|------------|-------------|
| **Material** | `Theme.Material` | Google's Material Design (default) |
| **Material 3** | `Theme.Material3` | Latest Material Design 3 |
| **Bootstrap** | `Theme.Bootstrap` | Twitter Bootstrap style |
| **Bootstrap 4** | `Theme.Bootstrap4` | Bootstrap 4 styling |
| **Bootstrap 5** | `Theme.Bootstrap5` | Bootstrap 5 styling |
| **Fabric** | `Theme.Fabric` | Microsoft Office Fabric |
| **Fluent** | `Theme.Fluent` | Microsoft Fluent Design |
| **Tailwind** | `Theme.Tailwind` | Tailwind CSS style |
| **High Contrast** | `Theme.HighContrast` | Accessibility-focused high contrast |

### Applying Themes

```cshtml
@using Syncfusion.Blazor.ChartWizard

<div class="control-section">
    <SfChartWizard Theme="Theme.Material3">
        <ChartSettings DataSource="@OlympicsDataSource"
                       CategoryFields="@categories"
                       SeriesType="ChartWizardSeriesType.Bar"
                       SeriesFields="@chartSeries">
        </ChartSettings>
    </SfChartWizard>
</div>

@code {
    private readonly List<string> chartSeries = new() { "Gold", "Silver", "Bronze" };
    private readonly List<string> categories = new() { "Country", "CountryCode" };

    public class OlympicsData
    {
        public string? Country { get; set; }
        public string? CountryCode { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }

    private readonly List<OlympicsData> OlympicsDataSource = new()
    {
        new OlympicsData { Country = "USA", CountryCode = "USA", Gold = 40, Silver = 44, Bronze = 42 },
        new OlympicsData { Country = "China", CountryCode = "CHN", Gold = 40, Silver = 27, Bronze = 24 },
        new OlympicsData { Country = "Great Britain", CountryCode = "GBR", Gold = 14, Silver = 22, Bronze = 29 },
        new OlympicsData { Country = "France", CountryCode = "FRA", Gold = 16, Silver = 26, Bronze = 22 },
        new OlympicsData { Country = "Australia", CountryCode = "AUS", Gold = 18, Silver = 19, Bronze = 16 },
        new OlympicsData { Country = "Japan", CountryCode = "JPN", Gold = 20, Silver = 12, Bronze = 13 },
        new OlympicsData { Country = "Italy", CountryCode = "ITA", Gold = 12, Silver = 13, Bronze = 15 },
        new OlympicsData { Country = "Netherlands", CountryCode = "NLD", Gold = 15, Silver = 7,  Bronze = 12 },
        new OlympicsData { Country = "Germany", CountryCode = "DEU", Gold = 12, Silver = 13, Bronze = 8  },
        new OlympicsData { Country = "South Korea", CountryCode = "KOR", Gold = 13, Silver = 9,  Bronze = 10 }
    };
}
```

### Theme Scope

**Important Note:**
> The `Theme` property applies the selected theme to the chart area, not the entire Chart Wizard UI. For a consistent look across the whole component, refer to the theme section in Getting Started documentation.

To apply theme globally to all Syncfusion components:

```cshtml
<!-- In _Host.cshtml or App.razor -->
<head>
    <!-- Material 3 Theme -->
    <link href="_content/Syncfusion.Blazor.Themes/material3.css" rel="stylesheet" />
    
    <!-- OR Bootstrap 5 Theme -->
    <!-- <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" /> -->
</head>
```

---

## Right-to-Left (RTL) Support

### EnableRtl Property

Set `EnableRtl` to `true` to support right-to-left languages such as Arabic or Hebrew. This option automatically adjusts the alignment of headers, panels, and controls for RTL layouts.

```cshtml
@using Syncfusion.Blazor.ChartWizard

<div class="control-section">
    <SfChartWizard EnableRtl="true">
        <ChartSettings DataSource="@OlympicsDataSource"
                       CategoryFields="@categories"
                       SeriesType="ChartWizardSeriesType.Bar"
                       SeriesFields="@chartSeries">
        </ChartSettings>
    </SfChartWizard>
</div>

@code {
    private readonly List<string> chartSeries = new() { "Gold", "Silver", "Bronze" };
    private readonly List<string> categories = new() { "Country", "CountryCode" };

    public class OlympicsData
    {
        public string? Country { get; set; }
        public string? CountryCode { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }

    private readonly List<OlympicsData> OlympicsDataSource = new()
    {
        new OlympicsData { Country = "USA", CountryCode = "USA", Gold = 40, Silver = 44, Bronze = 42 },
        new OlympicsData { Country = "China", CountryCode = "CHN", Gold = 40, Silver = 27, Bronze = 24 },
        new OlympicsData { Country = "Great Britain", CountryCode = "GBR", Gold = 14, Silver = 22, Bronze = 29 },
        new OlympicsData { Country = "France", CountryCode = "FRA", Gold = 16, Silver = 26, Bronze = 22 },
        new OlympicsData { Country = "Australia", CountryCode = "AUS", Gold = 18, Silver = 19, Bronze = 16 },
        new OlympicsData { Country = "Japan", CountryCode = "JPN", Gold = 20, Silver = 12, Bronze = 13 },
        new OlympicsData { Country = "Italy", CountryCode = "ITA", Gold = 12, Silver = 13, Bronze = 15 },
        new OlympicsData { Country = "Netherlands", CountryCode = "NLD", Gold = 15, Silver = 7,  Bronze = 12 },
        new OlympicsData { Country = "Germany", CountryCode = "DEU", Gold = 12, Silver = 13, Bronze = 8  },
        new OlympicsData { Country = "South Korea", CountryCode = "KOR", Gold = 13, Silver = 9,  Bronze = 10 }
    };
}
```

### RTL Layout Behavior

When `EnableRtl` is true:
- Property panel appears on the left side
- Chart area appears on the right side
- Text alignment adjusts automatically
- Menu and dropdown items align right
- Scrollbars appear on the left

---

## Property Panel Control

### PropertyPanelExpanded Property

The `PropertyPanelExpanded` property controls whether the property panel is open when the Chart Wizard first loads. Set it to `false` to start with the panel collapsed, giving more space to the chart area.

```cshtml
@using Syncfusion.Blazor.ChartWizard

<div class="control-section">
    <SfChartWizard PropertyPanelExpanded="false">
        <ChartSettings DataSource="@OlympicsDataSource"
                       CategoryFields="@categories"
                       SeriesType="ChartWizardSeriesType.Bar"
                       SeriesFields="@chartSeries">
        </ChartSettings>
    </SfChartWizard>
</div>

@code {
    private readonly List<string> chartSeries = new() { "Gold", "Silver", "Bronze" };
    private readonly List<string> categories = new() { "Country", "CountryCode" };

    public class OlympicsData
    {
        public string? Country { get; set; }
        public string? CountryCode { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }

    private readonly List<OlympicsData> OlympicsDataSource = new()
    {
        new OlympicsData { Country = "USA", CountryCode = "USA", Gold = 40, Silver = 44, Bronze = 42 },
        new OlympicsData { Country = "China", CountryCode = "CHN", Gold = 40, Silver = 27, Bronze = 24 },
        new OlympicsData { Country = "Great Britain", CountryCode = "GBR", Gold = 14, Silver = 22, Bronze = 29 },
        new OlympicsData { Country = "France", CountryCode = "FRA", Gold = 16, Silver = 26, Bronze = 22 },
        new OlympicsData { Country = "Australia", CountryCode = "AUS", Gold = 18, Silver = 19, Bronze = 16 },
        new OlympicsData { Country = "Japan", CountryCode = "JPN", Gold = 20, Silver = 12, Bronze = 13 },
        new OlympicsData { Country = "Italy", CountryCode = "ITA", Gold = 12, Silver = 13, Bronze = 15 },
        new OlympicsData { Country = "Netherlands", CountryCode = "NLD", Gold = 15, Silver = 7,  Bronze = 12 },
        new OlympicsData { Country = "Germany", CountryCode = "DEU", Gold = 12, Silver = 13, Bronze = 8  },
        new OlympicsData { Country = "South Korea", CountryCode = "KOR", Gold = 13, Silver = 9,  Bronze = 10 }
    };
}
```

### Default Behavior

- **`PropertyPanelExpanded="true"` (default):** Property panel is visible on initial load
- **`PropertyPanelExpanded="false"`:** Property panel is collapsed on initial load, showing only the chart
- Users can toggle the panel visibility at runtime regardless of the initial state

---

## Complete Implementation Examples

### Basic Width and Height

```cshtml
@using Syncfusion.Blazor.ChartWizard

<div class="container-fluid">
    <div class="row">
        <div class="col-md-12">
            <h3>Sales Performance Dashboard</h3>
            <SfChartWizard Width="100%" Height="500px">
                <ChartSettings DataSource="@OlympicsDataSource"
                               CategoryFields="@categories"
                               SeriesType="ChartWizardSeriesType.Column"
                               SeriesFields="@chartSeries">
                </ChartSettings>
            </SfChartWizard>
        </div>
    </div>
</div>

@code {
    private readonly List<string> chartSeries = new() { "Gold", "Silver", "Bronze" };
    private readonly List<string> categories = new() { "Country", "CountryCode" };

    public class OlympicsData
    {
        public string? Country { get; set; }
        public string? CountryCode { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }

    private readonly List<OlympicsData> OlympicsDataSource = new()
    {
        new OlympicsData { Country = "USA", CountryCode = "USA", Gold = 40, Silver = 44, Bronze = 42 },
        new OlympicsData { Country = "China", CountryCode = "CHN", Gold = 40, Silver = 27, Bronze = 24 },
        new OlympicsData { Country = "Great Britain", CountryCode = "GBR", Gold = 14, Silver = 22, Bronze = 29 },
        new OlympicsData { Country = "France", CountryCode = "FRA", Gold = 16, Silver = 26, Bronze = 22 },
        new OlympicsData { Country = "Australia", CountryCode = "AUS", Gold = 18, Silver = 19, Bronze = 16 },
        new OlympicsData { Country = "Japan", CountryCode = "JPN", Gold = 20, Silver = 12, Bronze = 13 },
        new OlympicsData { Country = "Italy", CountryCode = "ITA", Gold = 12, Silver = 13, Bronze = 15 },
        new OlympicsData { Country = "Netherlands", CountryCode = "NLD", Gold = 15, Silver = 7,  Bronze = 12 },
        new OlympicsData { Country = "Germany", CountryCode = "DEU", Gold = 12, Silver = 13, Bronze = 8  },
        new OlympicsData { Country = "South Korea", CountryCode = "KOR", Gold = 13, Silver = 9,  Bronze = 10 }
    };
}
```

### Theme Application

```cshtml
@using Syncfusion.Blazor.ChartWizard

<div class="control-section">
    <div class="theme-selector">
        <label>Select Theme: </label>
        <select @onchange="OnThemeChange">
            <option value="Material">Material</option>
            <option value="Material3">Material 3</option>
            <option value="Bootstrap5">Bootstrap 5</option>
            <option value="Fluent">Fluent</option>
            <option value="Tailwind">Tailwind</option>
            <option value="HighContrast">High Contrast</option>
        </select>
    </div>
    
    <SfChartWizard Theme="@selectedTheme" Width="100%" Height="600px">
        <ChartSettings DataSource="@OlympicsDataSource"
                       CategoryFields="@categories"
                       SeriesType="ChartWizardSeriesType.Bar"
                       SeriesFields="@chartSeries">
        </ChartSettings>
    </SfChartWizard>
</div>

@code {
    private Theme selectedTheme = Theme.Material3;
    private readonly List<string> chartSeries = new() { "Gold", "Silver", "Bronze" };
    private readonly List<string> categories = new() { "Country", "CountryCode" };

    private void OnThemeChange(ChangeEventArgs e)
    {
        selectedTheme = e.Value?.ToString() switch
        {
            "Material" => Theme.Material,
            "Material3" => Theme.Material3,
            "Bootstrap5" => Theme.Bootstrap5,
            "Fluent" => Theme.Fluent,
            "Tailwind" => Theme.Tailwind,
            "HighContrast" => Theme.HighContrast,
            _ => Theme.Material
        };
    }

    public class OlympicsData
    {
        public string? Country { get; set; }
        public string? CountryCode { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }

    private readonly List<OlympicsData> OlympicsDataSource = new()
    {
        new OlympicsData { Country = "USA", CountryCode = "USA", Gold = 40, Silver = 44, Bronze = 42 },
        new OlympicsData { Country = "China", CountryCode = "CHN", Gold = 40, Silver = 27, Bronze = 24 },
        new OlympicsData { Country = "Great Britain", CountryCode = "GBR", Gold = 14, Silver = 22, Bronze = 29 },
        new OlympicsData { Country = "France", CountryCode = "FRA", Gold = 16, Silver = 26, Bronze = 22 },
        new OlympicsData { Country = "Australia", CountryCode = "AUS", Gold = 18, Silver = 19, Bronze = 16 },
        new OlympicsData { Country = "Japan", CountryCode = "JPN", Gold = 20, Silver = 12, Bronze = 13 },
        new OlympicsData { Country = "Italy", CountryCode = "ITA", Gold = 12, Silver = 13, Bronze = 15 },
        new OlympicsData { Country = "Netherlands", CountryCode = "NLD", Gold = 15, Silver = 7,  Bronze = 12 },
        new OlympicsData { Country = "Germany", CountryCode = "DEU", Gold = 12, Silver = 13, Bronze = 8  },
        new OlympicsData { Country = "South Korea", CountryCode = "KOR", Gold = 13, Silver = 9,  Bronze = 10 }
    };
}
```

### RTL Implementation

```cshtml
@using Syncfusion.Blazor.ChartWizard

<div class="control-section">
    <div class="toolbar">
        <label>
            <input type="checkbox" @bind="enableRtl" />
            Enable Right-to-Left Layout
        </label>
    </div>
    
    <SfChartWizard EnableRtl="@enableRtl" Width="100%" Height="600px">
        <ChartSettings DataSource="@OlympicsDataSource"
                       CategoryFields="@categories"
                       SeriesType="ChartWizardSeriesType.Bar"
                       SeriesFields="@chartSeries">
        </ChartSettings>
    </SfChartWizard>
</div>

@code {
    private bool enableRtl = false;
    private readonly List<string> chartSeries = new() { "Gold", "Silver", "Bronze" };
    private readonly List<string> categories = new() { "Country", "CountryCode" };

    public class OlympicsData
    {
        public string? Country { get; set; }
        public string? CountryCode { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }

    private readonly List<OlympicsData> OlympicsDataSource = new()
    {
        new OlympicsData { Country = "USA", CountryCode = "USA", Gold = 40, Silver = 44, Bronze = 42 },
        new OlympicsData { Country = "China", CountryCode = "CHN", Gold = 40, Silver = 27, Bronze = 24 },
        new OlympicsData { Country = "Great Britain", CountryCode = "GBR", Gold = 14, Silver = 22, Bronze = 29 },
        new OlympicsData { Country = "France", CountryCode = "FRA", Gold = 16, Silver = 26, Bronze = 22 },
        new OlympicsData { Country = "Australia", CountryCode = "AUS", Gold = 18, Silver = 19, Bronze = 16 },
        new OlympicsData { Country = "Japan", CountryCode = "JPN", Gold = 20, Silver = 12, Bronze = 13 },
        new OlympicsData { Country = "Italy", CountryCode = "ITA", Gold = 12, Silver = 13, Bronze = 15 },
        new OlympicsData { Country = "Netherlands", CountryCode = "NLD", Gold = 15, Silver = 7,  Bronze = 12 },
        new OlympicsData { Country = "Germany", CountryCode = "DEU", Gold = 12, Silver = 13, Bronze = 8  },
        new OlympicsData { Country = "South Korea", CountryCode = "KOR", Gold = 13, Silver = 9,  Bronze = 10 }
    };
}
```

### Property Panel Configuration

```cshtml
@using Syncfusion.Blazor.ChartWizard

<div class="control-section">
    <div class="toolbar">
        <button class="btn btn-primary" @onclick="TogglePropertyPanel">
            @(propertyPanelExpanded ? "Hide" : "Show") Property Panel
        </button>
    </div>
    
    <SfChartWizard PropertyPanelExpanded="@propertyPanelExpanded" 
                   Width="100%" 
                   Height="600px">
        <ChartSettings DataSource="@OlympicsDataSource"
                       CategoryFields="@categories"
                       SeriesType="ChartWizardSeriesType.Bar"
                       SeriesFields="@chartSeries">
        </ChartSettings>
    </SfChartWizard>
</div>

@code {
    private bool propertyPanelExpanded = true;
    private readonly List<string> chartSeries = new() { "Gold", "Silver", "Bronze" };
    private readonly List<string> categories = new() { "Country", "CountryCode" };

    private void TogglePropertyPanel()
    {
        propertyPanelExpanded = !propertyPanelExpanded;
    }

    public class OlympicsData
    {
        public string? Country { get; set; }
        public string? CountryCode { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }

    private readonly List<OlympicsData> OlympicsDataSource = new()
    {
        new OlympicsData { Country = "USA", CountryCode = "USA", Gold = 40, Silver = 44, Bronze = 42 },
        new OlympicsData { Country = "China", CountryCode = "CHN", Gold = 40, Silver = 27, Bronze = 24 },
        new OlympicsData { Country = "Great Britain", CountryCode = "GBR", Gold = 14, Silver = 22, Bronze = 29 },
        new OlympicsData { Country = "France", CountryCode = "FRA", Gold = 16, Silver = 26, Bronze = 22 },
        new OlympicsData { Country = "Australia", CountryCode = "AUS", Gold = 18, Silver = 19, Bronze = 16 },
        new OlympicsData { Country = "Japan", CountryCode = "JPN", Gold = 20, Silver = 12, Bronze = 13 },
        new OlympicsData { Country = "Italy", CountryCode = "ITA", Gold = 12, Silver = 13, Bronze = 15 },
        new OlympicsData { Country = "Netherlands", CountryCode = "NLD", Gold = 15, Silver = 7,  Bronze = 12 },
        new OlympicsData { Country = "Germany", CountryCode = "DEU", Gold = 12, Silver = 13, Bronze = 8  },
        new OlympicsData { Country = "South Korea", CountryCode = "KOR", Gold = 13, Silver = 9,  Bronze = 10 }
    };
}
```

### Combined Configuration

```cshtml
@using Syncfusion.Blazor.ChartWizard

<div class="control-section">
    <div class="configuration-panel">
        <h4>Chart Wizard Configuration</h4>
        
        <div class="config-group">
            <label>Width:</label>
            <input type="text" @bind="chartWidth" placeholder="e.g., 100% or 800px" />
        </div>
        
        <div class="config-group">
            <label>Height:</label>
            <input type="text" @bind="chartHeight" placeholder="e.g., 600px" />
        </div>
        
        <div class="config-group">
            <label>Theme:</label>
            <select @bind="selectedTheme">
                <option value="Material">Material</option>
                <option value="Material3">Material 3</option>
                <option value="Bootstrap5">Bootstrap 5</option>
                <option value="Fluent">Fluent</option>
                <option value="Tailwind">Tailwind</option>
            </select>
        </div>
        
        <div class="config-group">
            <label>
                <input type="checkbox" @bind="enableRtl" />
                Enable RTL
            </label>
        </div>
        
        <div class="config-group">
            <label>
                <input type="checkbox" @bind="propertyPanelExpanded" />
                Property Panel Expanded
            </label>
        </div>
    </div>
    
    <SfChartWizard Width="@chartWidth" 
                   Height="@chartHeight"
                   Theme="@GetTheme()"
                   EnableRtl="@enableRtl"
                   PropertyPanelExpanded="@propertyPanelExpanded">
        <ChartSettings DataSource="@OlympicsDataSource"
                       CategoryFields="@categories"
                       SeriesType="ChartWizardSeriesType.Bar"
                       SeriesFields="@chartSeries">
        </ChartSettings>
    </SfChartWizard>
</div>

<style>
    .configuration-panel {
        background: #f5f5f5;
        padding: 15px;
        margin-bottom: 20px;
        border-radius: 4px;
    }
    
    .config-group {
        margin-bottom: 10px;
    }
    
    .config-group label {
        display: inline-block;
        min-width: 150px;
        font-weight: bold;
    }
    
    .config-group input[type="text"],
    .config-group select {
        padding: 5px;
        width: 200px;
    }
</style>

@code {
    private string chartWidth = "100%";
    private string chartHeight = "600px";
    private string selectedTheme = "Material3";
    private bool enableRtl = false;
    private bool propertyPanelExpanded = true;
    
    private readonly List<string> chartSeries = new() { "Gold", "Silver", "Bronze" };
    private readonly List<string> categories = new() { "Country", "CountryCode" };

    private Theme GetTheme()
    {
        return selectedTheme switch
        {
            "Material" => Theme.Material,
            "Material3" => Theme.Material3,
            "Bootstrap5" => Theme.Bootstrap5,
            "Fluent" => Theme.Fluent,
            "Tailwind" => Theme.Tailwind,
            _ => Theme.Material
        };
    }

    public class OlympicsData
    {
        public string? Country { get; set; }
        public string? CountryCode { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }

    private readonly List<OlympicsData> OlympicsDataSource = new()
    {
        new OlympicsData { Country = "USA", CountryCode = "USA", Gold = 40, Silver = 44, Bronze = 42 },
        new OlympicsData { Country = "China", CountryCode = "CHN", Gold = 40, Silver = 27, Bronze = 24 },
        new OlympicsData { Country = "Great Britain", CountryCode = "GBR", Gold = 14, Silver = 22, Bronze = 29 },
        new OlympicsData { Country = "France", CountryCode = "FRA", Gold = 16, Silver = 26, Bronze = 22 },
        new OlympicsData { Country = "Australia", CountryCode = "AUS", Gold = 18, Silver = 19, Bronze = 16 },
        new OlympicsData { Country = "Japan", CountryCode = "JPN", Gold = 20, Silver = 12, Bronze = 13 },
        new OlympicsData { Country = "Italy", CountryCode = "ITA", Gold = 12, Silver = 13, Bronze = 15 },
        new OlympicsData { Country = "Netherlands", CountryCode = "NLD", Gold = 15, Silver = 7,  Bronze = 12 },
        new OlympicsData { Country = "Germany", CountryCode = "DEU", Gold = 12, Silver = 13, Bronze = 8  },
        new OlympicsData { Country = "South Korea", CountryCode = "KOR", Gold = 13, Silver = 9,  Bronze = 10 }
    };
}
```

---

## Best Practices

1. **Use percentage for responsive designs:**
   ```cshtml
   <SfChartWizard Width="100%" Height="80vh">
   ```

2. **Use pixels for fixed layouts:**
   ```cshtml
   <SfChartWizard Width="1200px" Height="800px">
   ```

3. **Match theme with your application:**
   ```csharp
   // If using Bootstrap
   <SfChartWizard Theme="Theme.Bootstrap5">
   
   // If using Material Design
   <SfChartWizard Theme="Theme.Material3">
   ```

4. **Enable RTL for international applications:**
   ```csharp
   @inject IStringLocalizer<YourComponent> Localizer
   
   <SfChartWizard EnableRtl="@IsRtlCulture()">
   
   @code {
       private bool IsRtlCulture()
       {
           var culture = CultureInfo.CurrentCulture;
           return culture.TextInfo.IsRightToLeft;
       }
   }
   ```

5. **Start with property panel collapsed for presentation mode:**
   ```cshtml
   <SfChartWizard PropertyPanelExpanded="false">
   ```

6. **Set minimum dimensions to prevent rendering issues:**
   ```cshtml
   <div style="min-width: 600px; min-height: 400px;">
       <SfChartWizard Width="100%" Height="100%">
       </SfChartWizard>
   </div>
   ```

---

## Troubleshooting

### Common Issues

#### Issue 1: Chart Not Visible or Clipped

**Symptoms:**
- Chart Wizard appears empty or partially visible
- Content is cut off

**Causes:**
- Container has no defined height
- Percentage height without parent height
- CSS conflicts

**Solutions:**
```cshtml
<!-- Ensure parent container has height -->
<div style="height: 100vh;">
    <SfChartWizard Width="100%" Height="100%">
    </SfChartWizard>
</div>

<!-- OR use fixed pixel height -->
<SfChartWizard Width="100%" Height="600px">
</SfChartWizard>

<!-- OR use viewport units -->
<SfChartWizard Width="100%" Height="80vh">
</SfChartWizard>
```

#### Issue 2: Theme Not Applied

**Symptoms:**
- Chart appears with default styling
- Theme property has no effect

**Causes:**
- Theme CSS not loaded
- Incorrect theme CSS file
- CSS loading order issues

**Solutions:**
```cshtml
<!-- In _Host.cshtml or index.html -->
<head>
    <!-- Load theme CSS before other styles -->
    <link href="_content/Syncfusion.Blazor.Themes/material3.css" rel="stylesheet" />
    
    <!-- Your custom styles -->
    <link href="css/app.css" rel="stylesheet" />
</head>

@code {
    // Ensure Theme enum is used correctly
    <SfChartWizard Theme="Theme.Material3">
}
```

#### Issue 3: RTL Not Working Properly

**Symptoms:**
- Layout doesn't flip to RTL
- Some elements still appear LTR

**Causes:**
- EnableRtl not set
- CSS overriding RTL styles
- Browser cache

**Solutions:**
```cshtml
<!-- Verify EnableRtl is set -->
<SfChartWizard EnableRtl="true">
</SfChartWizard>

<!-- Check CSS specificity -->
<style>
    /* Avoid overriding RTL styles */
    .e-chartwizard {
        /* Don't set direction or text-align here */
    }
</style>

<!-- Clear cache and refresh -->
@code {
    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            // Force re-render
            StateHasChanged();
        }
    }
}
```

#### Issue 4: Property Panel State Not Updating

**Symptoms:**
- PropertyPanelExpanded changes don't reflect in UI
- Panel stays open/closed despite property changes

**Causes:**
- Property binding issues
- Component not re-rendering

**Solutions:**
```csharp
// Use proper binding
<SfChartWizard @bind-PropertyPanelExpanded="panelExpanded">
</SfChartWizard>

@code {
    private bool panelExpanded = true;
    
    private void TogglePanel()
    {
        panelExpanded = !panelExpanded;
        StateHasChanged(); // Force UI update
    }
}
```

#### Issue 5: Responsive Sizing Issues

**Symptoms:**
- Chart doesn't resize with window
- Percentages not working as expected

**Causes:**
- Parent container constraints
- CSS Flexbox/Grid conflicts

**Solutions:**
```cshtml
<!-- Use proper container structure -->
<div class="chart-container">
    <SfChartWizard Width="100%" Height="100%">
    </SfChartWizard>
</div>

<style>
    .chart-container {
        width: 100%;
        height: 600px;
        /* OR */
        height: calc(100vh - 100px); /* Subtract header/footer */
        position: relative;
    }
</style>

@code {
    // Handle window resize
    private async Task OnWindowResize()
    {
        await InvokeAsync(StateHasChanged);
    }
}
```

#### Issue 6: Theme Switching Causes Rendering Issues

**Symptoms:**
- Chart appears broken after theme change
- Colors mismatch after switching

**Causes:**
- CSS not fully loaded
- Component state not updated

**Solutions:**
```csharp
private Theme currentTheme = Theme.Material3;
private bool isLoading = false;

private async Task ChangeTheme(Theme newTheme)
{
    isLoading = true;
    StateHasChanged();
    
    // Allow time for CSS to load
    await Task.Delay(200);
    
    currentTheme = newTheme;
    isLoading = false;
    StateHasChanged();
}

<div>
    @if (isLoading)
    {
        <p>Loading theme...</p>
    }
    else
    {
        <SfChartWizard Theme="@currentTheme">
        </SfChartWizard>
    }
</div>
```

#### Issue 7: Width/Height Properties Not Accepting Values

**Symptoms:**
- Error when setting Width or Height
- Values ignored

**Causes:**
- Invalid format
- Missing units

**Solutions:**
```csharp
// Valid formats
Width="100%"      // Percentage
Width="800px"     // Pixels
Width="50vw"      // Viewport width
Height="600px"    // Pixels
Height="80vh"     // Viewport height

// Invalid formats
Width="800"       // Missing unit
Width="50"        // Missing unit
Height="100%"     // Needs parent with defined height
```
