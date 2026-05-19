# Blazor Chart Wizard API Reference

Complete API reference for the Syncfusion Blazor Chart Wizard component, including all classes, properties, methods, events, and enums.

## Table of Contents

- [Overview](#overview)
- [Namespace](#namespace)
- [SfChartWizard Class](#sfchartwizard-class)
  - [Properties](#sfchartwizard-properties)
  - [Methods](#sfchartwizard-methods)
  - [Events](#sfchartwizard-events)
- [ChartSettings Class](#chartsettings-class)
  - [Properties](#chartsettings-properties)
- [ChartExportSettings Class](#chartexportsettings-class)
  - [Properties](#chartexportsettings-properties)
- [ChartExportingEventArgs Class](#chartexportingeventargs-class)
  - [Properties](#chartexportingeventargs-properties)
- [ChartWizardSeriesType Enum](#chartwizardseriestype-enum)
- [PageOrientation Enum](#pageorientation-enum)
- [Theme Enum](#theme-enum)

## Overview

The Syncfusion Blazor Chart Wizard provides a user-friendly interface that guides users through the process of creating and customizing charts step-by-step. It simplifies chart creation by offering options for chart types, data selection, and formatting in an organized sequence.

## Namespace

```csharp
Syncfusion.Blazor.ChartWizard
```

**Assembly:** `Syncfusion.Blazor.dll`

## SfChartWizard Class

The main component class that renders the Chart Wizard interface.

**Inheritance:** `object` → `ComponentBase` → `OwningComponentBase` → `SfOwningComponentBase` → `SfBaseComponent` → `SfDataBoundComponent` → **`SfChartWizard`**

**Implements:** `IComponent`, `IHandleEvent`, `IHandleAfterRender`, `IDisposable`

### SfChartWizard Properties

#### Width

Gets or sets the width of the Chart Wizard component.

```csharp
[Parameter]
public string Width { get; set; }
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| Width | string | "100%" | Width of the component (e.g., "800px", "50%") |

**Remarks:** Accepts input as either pixel (`"800px"`) or percentage (`"100%"`). If specified as `"100%"`, the chart wizard renders to the full width of its parent element.

**Example:**
```cshtml
<SfChartWizard Width="500px" />
```

#### Height

Gets or sets the height of the Chart Wizard component.

```csharp
[Parameter]
public string Height { get; set; }
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| Height | string | "100%" | Height of the component (e.g., "600px", "75%") |

**Remarks:** Accepts input as either pixel (`"600px"`) or percentage (`"100%"`). If specified as `"100%"`, the chart wizard renders to the full height of its parent element.

**Example:**
```cshtml
<SfChartWizard Height="400px" />
```

#### Theme

Gets or sets the theme styles for the components within the Chart Wizard.

```csharp
[Parameter]
public Theme Theme { get; set; }
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| Theme | Theme | Fluent2 | Visual theme (Material, Fluent, Bootstrap, Tailwind, etc.) |

**Remarks:** This property applies a consistent visual theme across all sub-components. Available themes include Material, Material3, MaterialDark, Material3Dark, Fluent, Fluent2, FluentDark, Fluent2Dark, Fluent2HighContrast, Bootstrap, Bootstrap4, Bootstrap5, BootstrapDark, Bootstrap5Dark, Tailwind, TailwindDark, Tailwind3, Tailwind3Dark, Fabric, FabricDark, HighContrast, and HighContrastLight.

**Example:**
```cshtml
<SfChartWizard Theme="Theme.Bootstrap5" />
```

#### EnableRtl

Gets or sets a value indicating whether right-to-left (RTL) rendering is enabled.

```csharp
[Parameter]
public bool EnableRtl { get; set; }
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| EnableRtl | bool | false | Enable right-to-left layout for RTL languages |

**Remarks:** Enabling RTL layout is useful for supporting right-to-left languages such as Arabic, Hebrew, or Persian. It affects the alignment and flow direction of the component's content.

**Example:**
```cshtml
<SfChartWizard EnableRtl="true" />
```

#### PropertyPanelExpanded

Gets or sets a value indicating whether to display the property panel on initial rendering.

```csharp
[Parameter]
public bool PropertyPanelExpanded { get; set; }
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| PropertyPanelExpanded | bool | true | Show property panel on initial render |

**Remarks:** The property panel serves as an interactive configurator within the Chart Wizard. Users can utilize this panel to dynamically configure chart settings such as series types, data axes, legend appearance, and data labels.

**Example:**
```cshtml
<SfChartWizard PropertyPanelExpanded="true" />
```

#### ChildContent

Gets or sets the content of the UI element.

```csharp
[Parameter]
public RenderFragment? ChildContent { get; set; }
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| ChildContent | RenderFragment | null | Child content for nested components like ChartSettings |

**Remarks:** This property is typically used to define nested configuration components like `ChartSettings`.

### SfChartWizard Methods

#### SaveChart()

Serializes the current state of the Chart Wizard component into a JSON string.

```csharp
public string SaveChart()
```

**Returns:** `string` - A JSON representation of the complete chart state, including all configuration settings and layout details.

**Remarks:** The serialized output includes all chart elements, their properties, layout information, series configurations, axes, legends, styling, and other runtime state data. The resulting JSON string can later be passed to `LoadChartAsync()` to fully restore the chart.

**Example:**
```csharp
SfChartWizard chartWizard = new SfChartWizard();
string chartJson = chartWizard.SaveChart();
```

#### LoadChartAsync(string)

Asynchronously loads chart configuration data from a JSON string and applies it to the current Chart Wizard instance.

```csharp
public Task LoadChartAsync(string data)
```

**Parameters:**
- `data` (string) - A non-empty string containing the JSON representation of the chart data. Must not be null or empty.

**Returns:** `Task` - A task that represents the asynchronous operation.

**Remarks:** This method performs asynchronous deserialization of chart data and updates the internal state of the Chart Wizard component. All existing configuration values are overwritten by the data contained in the JSON payload. If the provided JSON does not match the expected structure, the method may throw a deserialization exception.

**Example:**
```csharp
SfChartWizard chartWizard = new SfChartWizard();
await chartWizard.LoadChartAsync(chartJson);
```

### SfChartWizard Events

#### Exporting

Gets or sets an EventCallback that fires when the chart export action is initiated.

```csharp
[Parameter]
public EventCallback<ChartExportingEventArgs> Exporting { get; set; }
```

**Event Args:** `ChartExportingEventArgs`

**Remarks:** This event is raised just before the chart content is exported, providing an opportunity to intervene in the process. You can modify export parameters, adjust properties like file name or resolution, or cancel the export by setting `args.Cancel = true`.

**Example:**
```cshtml
<SfChartWizard Exporting="OnExporting">
    <ChartSettings ... />
</SfChartWizard>

@code {
    private void OnExporting(ChartExportingEventArgs args)
    {
        args.FileName = "CustomFileName";
        args.Width = 1024;
        args.Height = 768;
    }
}
```

## ChartSettings Class

Provides options to customize the appearance and behavior of the chart rendered within the Chart Wizard.

**Inheritance:** `object` → `ComponentBase` → `OwningComponentBase` → `SfOwningComponentBase` → `SfBaseComponent` → `SfDataBoundComponent` → **`ChartSettings`**

**Implements:** `IComponent`, `IHandleEvent`, `IHandleAfterRender`, `IDisposable`

**Remarks:** This component is used within the `SfChartWizard` to define specific properties governing the chart's visual representation and data mapping.

### ChartSettings Properties

#### Datasource

Gets or sets the data source for the chart.

```csharp
[Parameter]
public IEnumerable<object>? Datasource { get; set; }
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| Datasource | IEnumerable<object> | null | Data collection for the chart |

**Remarks:** This data source will be used to populate the chart series. Each object in the collection typically contains fields that correspond to the `CategoryFields` and `SeriesFields` properties.

**Example:**
```cshtml
<ChartSettings Datasource="@ChartData" />
```

#### CategoryFields

Gets or sets the data fields used to group data along the category axis.

```csharp
[Parameter]
public List<string>? CategoryFields { get; set; }
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| CategoryFields | List<string> | null | Field names for x-axis categories |

**Remarks:** These field names are used to categorize the data points along the chart's primary axis (x-axis). Each string corresponds to a field name in the `Datasource`.

**Example:**
```cshtml
<ChartSettings CategoryFields="@(new List<string> { "Region" })" />
```

#### SeriesFields

Gets or sets the list of field names from the Datasource that will be mapped to individual series in the chart.

```csharp
[Parameter]
public List<string>? SeriesFields { get; set; }
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| SeriesFields | List<string> | null | Field names for data series |

**Remarks:** Each item in this list will typically generate a separate series in the chart, mapping data columns to visual series. Each string corresponds to a field name in the `Datasource`.

**Example:**
```cshtml
<ChartSettings SeriesFields="@(new List<string> { "Sales", "Profit" })" />
```

#### SeriesType

Gets or sets the series type used in the chart to render the data.

```csharp
[Parameter]
public ChartWizardSeriesType SeriesType { get; set; }
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| SeriesType | ChartWizardSeriesType | Line | Chart type (Line, Column, Bar, Area, Pie, etc.) |

**Remarks:** This property determines the visual representation of the data in the generated chart. Available types include Line, StackingLine, StackingLine100, Column, StackingColumn, StackingColumn100, Bar, StackingBar, StackingBar100, Area, StackingArea, StackingArea100, Scatter, and Pie.

**Example:**
```cshtml
<ChartSettings SeriesType="ChartWizardSeriesType.Column" />
```

#### ChildContent

Sets and gets the content of the UI element.

```csharp
[Parameter]
public RenderFragment? ChildContent { get; set; }
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| ChildContent | RenderFragment | null | Child content for nested components like ChartExportSettings |

## ChartExportSettings Class

Provides options to customize the export behavior of the chart within the Chart Wizard.

**Inheritance:** `object` → `ComponentBase` → `OwningComponentBase` → `SfOwningComponentBase` → `SfBaseComponent` → `SfDataBoundComponent` → **`ChartExportSettings`**

**Implements:** `IComponent`, `IHandleEvent`, `IHandleAfterRender`, `IDisposable`

**Remarks:** This component is used within the `SfChartWizard` to configure export-specific properties such as file name, image dimensions, and PDF orientation.

### ChartExportSettings Properties

#### FileName

Gets or sets the name for the exported file.

```csharp
[Parameter]
public string FileName { get; set; }
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| FileName | string | "" | Name for exported file (without extension) |

**Remarks:** The specified name will be used as the filename when the chart is exported. The component will automatically append the appropriate file extension based on the export type.

**Example:**
```cshtml
<ChartExportSettings FileName="SalesReport" />
```

#### Width

Gets or sets the width of the exported image dimension.

```csharp
[Parameter]
public double Width { get; set; }
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| Width | double | double.NaN | Export width in pixels |

**Remarks:** This property specifies the width of the chart when it is exported. If the value is set to `double.NaN` (the default), the exported file will have the chart's default rendered width. Note that this property is not applicable for XLSX and CSV export formats.

**Example:**
```cshtml
<ChartExportSettings Width="1024" />
```

#### Height

Gets or sets the height of the exported image dimension.

```csharp
[Parameter]
public double Height { get; set; }
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| Height | double | double.NaN | Export height in pixels |

**Remarks:** This property specifies the height of the chart when it is exported. If the value is set to `double.NaN` (the default), the exported file will have the chart's default rendered height. Note that this property is not applicable for XLSX and CSV export formats.

**Example:**
```cshtml
<ChartExportSettings Height="768" />
```

#### Orientation

Gets or sets the orientation to export the PDF file.

```csharp
[Parameter]
public PageOrientation Orientation { get; set; }
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| Orientation | PageOrientation | Portrait | Page orientation for PDF/print (Portrait or Landscape) |

**Remarks:** This property is applicable only when exporting the chart to a PDF format. The available orientations are `Portrait` (vertical) and `Landscape` (horizontal).

**Example:**
```cshtml
<ChartExportSettings Orientation="PageOrientation.Landscape" />
```

#### ChildContent

Sets and gets the content of the UI element.

```csharp
[Parameter]
public RenderFragment? ChildContent { get; set; }
```

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| ChildContent | RenderFragment | null | Child content |

## ChartExportingEventArgs Class

Provides information about the chart export event when `Exporting` is triggered.

**Inheritance:** `object` → **`ChartExportingEventArgs`**

### ChartExportingEventArgs Properties

#### FileName

Gets or sets the filename to use for the exported file.

```csharp
public string FileName { get; set; }
```

**Type:** `string`

**Description:** Sets the filename to use for the exported file (without extension). The component will append the appropriate extension for the selected export type.

#### Cancel

Gets or sets a value indicating whether to cancel the export operation.

```csharp
public bool Cancel { get; set; }
```

**Type:** `bool`

**Default:** `false`

**Description:** Set to `true` to cancel the export operation.

#### Width

Gets or sets the width in pixels to use for the exported output.

```csharp
public double Width { get; set; }
```

**Type:** `double`

**Description:** Sets width in pixels to use for the exported output (when supported by the export type). Useful to control output resolution.

#### Height

Gets or sets the height in pixels to use for the exported output.

```csharp
public double Height { get; set; }
```

**Type:** `double`

**Description:** Sets height in pixels to use for the exported output (when supported by the export type).

#### Orientation

Gets or sets the page orientation for printable exports.

```csharp
public PageOrientation Orientation { get; set; }
```

**Type:** `PageOrientation`

**Description:** Sets page orientation for printable exports (`Portrait` or `Landscape`). Applies primarily to PDF/Print workflows.

## ChartWizardSeriesType Enum

Specifies the various series types available for rendering data within the chart.

**Namespace:** `Syncfusion.Blazor.ChartWizard`

### Values

| Value | Description |
|-------|-------------|
| **Line** | Renders the chart using line series |
| **StackingLine** | Renders the chart using StackingLine series, where data points are stacked cumulatively |
| **StackingLine100** | Renders the chart using 100% StackingLine series, stacked to represent a percentage of the total |
| **Column** | Renders the chart using Column series, with vertical bars representing data |
| **StackingColumn** | Renders the chart using StackingColumn series, where columns are stacked cumulatively |
| **StackingColumn100** | Renders the chart using 100% StackingColumn series, stacked to represent a percentage |
| **Bar** | Renders the chart using Bar series, with horizontal bars representing data |
| **StackingBar** | Renders the chart using StackingBar series, where bars are stacked cumulatively |
| **StackingBar100** | Renders the chart using 100% StackingBar series, stacked to represent a percentage |
| **Area** | Renders the chart using Area series, with the area beneath the line filled |
| **StackingArea** | Renders the chart using StackingArea series, where areas are stacked cumulatively |
| **StackingArea100** | Renders the chart using 100% StackingArea series, stacked to represent a percentage |
| **Scatter** | Renders the chart using Scatter series, with individual data points plotted as markers |
| **Pie** | Renders the chart using Pie chart, representing proportions of a whole |

**Example:**
```cshtml
<ChartSettings SeriesType="ChartWizardSeriesType.Column" />
```

## PageOrientation Enum

Specifies the orientation of a PDF page for the Chart Wizard component.

**Namespace:** `Syncfusion.Blazor.ChartWizard`

### Values

| Value | Description |
|-------|-------------|
| **Portrait** | Specifies that the PDF page is oriented vertically |
| **Landscape** | Specifies that the PDF page is oriented horizontally |

**Example:**
```cshtml
<ChartExportSettings Orientation="PageOrientation.Landscape" />
```

## Theme Enum

Specifies the visual theme for the Chart Wizard component.

**Namespace:** `Syncfusion.Blazor`

### Values

| Value | Description |
|-------|-------------|
| **Material** | Renders the component with Material theme |
| **Material3** | Renders the component with Material 3 theme |
| **MaterialDark** | Renders the component with Material dark theme |
| **Material3Dark** | Renders the component with Material 3 dark theme |
| **Fluent** | Renders the component with Fluent theme |
| **Fluent2** | Renders the component with Fluent 2 theme (default) |
| **FluentDark** | Renders the component with Fluent dark theme |
| **Fluent2Dark** | Renders the component with Fluent 2 dark theme |
| **Fluent2HighContrast** | Renders the component with Fluent 2 high contrast theme |
| **Bootstrap** | Renders the component with Bootstrap theme |
| **Bootstrap4** | Renders the component with Bootstrap 4 theme |
| **Bootstrap5** | Renders the component with Bootstrap 5 theme |
| **BootstrapDark** | Renders the component with Bootstrap dark theme |
| **Bootstrap5Dark** | Renders the component with Bootstrap 5 dark theme |
| **Tailwind** | Renders the component with Tailwind theme |
| **TailwindDark** | Renders the component with Tailwind dark theme |
| **Tailwind3** | Renders the component with Tailwind 3 theme |
| **Tailwind3Dark** | Renders the component with Tailwind 3 dark theme |
| **Fabric** | Renders the component with Fabric theme |
| **FabricDark** | Renders the component with Fabric dark theme |
| **HighContrast** | Renders the component with high contrast theme |
| **HighContrastLight** | Renders the component with high contrast light theme |

**Example:**
```cshtml
<SfChartWizard Theme="Theme.Material3" />
```

## Complete Usage Example

Here's a complete example demonstrating the use of multiple APIs:

```cshtml
@using Syncfusion.Blazor.ChartWizard

<SfChartWizard @ref="ChartWizard"
               Width="100%"
               Height="600px"
               Theme="Theme.Fluent2"
               EnableRtl="false"
               PropertyPanelExpanded="true"
               Exporting="OnExporting">
    <ChartSettings DataSource="@Data"
                   CategoryFields="@(new List<string> { "Country" })"
                   SeriesFields="@(new List<string> { "Gold", "Silver", "Bronze" })"
                   SeriesType="ChartWizardSeriesType.Column">
        <ChartExportSettings FileName="OlympicsMedals"
                           Width="1024"
                           Height="768"
                           Orientation="Syncfusion.Blazor.ChartWizard.PageOrientation.Landscape" />
    </ChartSettings>
</SfChartWizard>

<button @onclick="SaveChartState">Save Chart</button>
<button @onclick="LoadChartState">Load Chart</button>

@code {
    private SfChartWizard? ChartWizard;
    private string? savedChartJson;

    private List<OlympicsData> Data = new()
    {
        new OlympicsData { Country = "USA", Gold = 40, Silver = 44, Bronze = 42 },
        new OlympicsData { Country = "China", Gold = 40, Silver = 27, Bronze = 24 },
        new OlympicsData { Country = "Japan", Gold = 20, Silver = 12, Bronze = 13 }
    };

    private void OnExporting(ChartExportingEventArgs args)
    {
        // Customize export
        args.FileName = $"OlympicsMedals_{DateTime.Now:yyyyMMdd}";
        args.Width = 1280;
        args.Height = 720;
    }

    private void SaveChartState()
    {
        if (ChartWizard != null)
        {
            savedChartJson = ChartWizard.SaveChart();
        }
    }

    private async Task LoadChartState()
    {
        if (ChartWizard != null && !string.IsNullOrEmpty(savedChartJson))
        {
            await ChartWizard.LoadChartAsync(savedChartJson);
        }
    }

    public class OlympicsData
    {
        public string? Country { get; set; }
        public int Gold { get; set; }
        public int Silver { get; set; }
        public int Bronze { get; set; }
    }
}
```

## Additional Resources

- [Syncfusion Blazor Documentation](https://blazor.syncfusion.com/documentation/)
- [Chart Wizard Component Demo](https://blazor.syncfusion.com/demos/chart-wizard/)
- [API Documentation Online](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.ChartWizard.html)
