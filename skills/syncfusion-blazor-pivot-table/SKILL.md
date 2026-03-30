---
name: syncfusion-blazor-pivot-table
description: Use this skill when users ask how to implement Syncfusion Pivot Table/PivotView in Blazor. Trigger for Blazor components, data binding, OLAP analysis, aggregation, drill-down/drill-through, grouping, filtering, conditional formatting, exports (Excel/PDF/CSV), or pivot charts. Blazor-only, not React/Angular/Vue/JS.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Grids"
---

# Syncfusion Blazor Pivot Table

The Syncfusion Blazor Pivot Table (`SfPivotView`) is a powerful multi-dimensional data analysis component that enables users to organize, summarize, and visualize data across row, column, value, and filter axes — similar to Excel pivot tables. It supports relational data (JSON/IEnumerable) and OLAP cubes, with interactive runtime controls including a Field List and Grouping Bar.

> ⚠️ **Important:** Always verify API class names, properties, and method signatures by consulting the **reference files in this skill** (`references/*.md`). These are maintained with verified, working examples. Do not assume API details from other sources.

## ⚠️ Security Warning: Data Source Validation

**CRITICAL SECURITY NOTICE:** When implementing pivot tables, always use trusted data sources. **Never** fetch or bind data from untrusted or user-provided URLs without proper validation and sanitization.

### Security Best Practices:

1. **Use Local Data**: Prefer local, in-memory data sources for maximum security
2. **Validate Remote Sources**: Only connect to authenticated and authorized API endpoints under your control
3. **Sanitize User Input**: Never allow users to specify arbitrary URLs or data sources
4. **Implement Authentication**: Always use authentication headers and secure API endpoints
5. **Content Validation**: Validate and sanitize all data received from external sources before binding
6. **Use HTTPS**: Always use HTTPS for remote data connections
7. **Rate Limiting**: Implement rate limiting on API endpoints to prevent abuse

### Security Risks:

- **Indirect Prompt Injection**: Untrusted third-party data can contain malicious content that manipulates AI agent behavior
- **Data Exfiltration**: Malicious data sources could attempt to extract sensitive information
- **Code Injection**: Untrusted data may contain scripts or harmful content

### Recommended Approach:

✅ **DO**: Use controlled, authenticated backend APIs
✅ **DO**: Implement server-side data validation
✅ **DO**: Use configuration files for API endpoints
✅ **DO**: Whitelist allowed data sources

❌ **DON'T**: Accept user-provided URLs
❌ **DON'T**: Bind to public, untrusted endpoints
❌ **DON'T**: Skip data validation and sanitization
❌ **DON'T**: Use HTTP for sensitive data

## When to Use This Skill

- Implementing `SfPivotView` in a Blazor WASM, Server, or MAUI app
- Configuring `PivotViewDataSourceSettings` with JSON, remote, or OLAP data
- Using Field List, Grouping Bar, aggregation, or calculated fields
- Applying filtering, sorting, or grouping to pivot data
- Enabling editing, export (Excel/PDF), or toolbar in the Pivot Table
- Rendering a Pivot Chart alongside or instead of the grid
- Optimizing pivot performance with virtual scrolling, paging, or server-side engine
- Applying number/conditional formatting, drill-down, drill-through
- Persisting pivot state or handling pivot events
- Building AI-powered Smart Pivot features

## Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Install NuGet packages, register service, add CSS/JS
- Initialize `SfPivotView` in a Blazor WASM, Server, or MAUI app
- Assign sample data and configure row/column/value/filter axes
- Minimal working example

### Data Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Bind local JSON/IEnumerable data
- Use `SfDataManager` with `JsonAdaptor`, `WebApiAdaptor`, `ODataV4Adaptor`
- Remote data binding patterns and lazy loading

### OLAP Data Source
📄 **Read:** [references/olap.md](references/olap.md)
- Connect to Microsoft SQL Server Analysis Services (SSAS) OLAP cubes
- Configure OLAP cube elements: hierarchies, measures, dimensions, named sets
- Calculated fields and MDX expressions
- Authentication and role-based access
- OLAP-specific features: field list, grouping bar, virtual scrolling

### Connecting to Databases
📄 **Read:** [references/connecting-to-data-sources.md](references/connecting-to-data-sources.md)
- Connect to SQL Server, MySQL, PostgreSQL, MongoDB, Oracle, Snowflake, Elasticsearch
- Server-side data service patterns for Blazor
- Connection string setup and adaptor configuration

### Field List
📄 **Read:** [references/field-list.md](references/field-list.md)
- Enable built-in (popup) or stand-alone field list
- Customizing fields: captions, visibility, drag behavior
- Defer layout update for batch field changes

### Grouping Bar
📄 **Read:** [references/grouping-bar.md](references/grouping-bar.md)
- Enable `ShowGroupingBar` for drag-and-drop field management
- Filter, sort, and remove fields at runtime
- Customize grouping bar appearance and behavior

### Aggregation
📄 **Read:** [references/aggregation.md](references/aggregation.md)
- All `SummaryTypes` (Sum, Count, Avg, Min, Max, % of total, etc.)
- Set aggregation per value field via `PivotViewValue.Type`
- `DifferenceFrom`, `PercentageOfDifferenceFrom`, `RunningTotals`

### Calculated Fields
📄 **Read:** [references/calculated-field.md](references/calculated-field.md)
- Create formula-based custom fields via UI dialog or `PivotViewCalculatedFieldSettings`
- Display calculated field in value axis
- Open dialog programmatically

### Filtering
📄 **Read:** [references/filtering.md](references/filtering.md)
- Member filtering (include/exclude), label filtering, value filtering
- Programmatic filter setup with `PivotViewFilterSettings`
- Date filters and OLAP filtering

### Sorting
📄 **Read:** [references/sorting.md](references/sorting.md)
- Enable `EnableSorting` for member sort (ascending/descending)
- Programmatic sort with `PivotViewSortSettings`
- Value sorting across column headers

### Grouping
📄 **Read:** [references/grouping.md](references/grouping.md)
- Number grouping (ranges), date grouping (year/quarter/month/day), custom grouping
- Enable `AllowGrouping` and right-click UI
- Programmatic grouping configuration

### Drill Down
📄 **Read:** [references/drill-down.md](references/drill-down.md)
- Expand/collapse member hierarchies
- `ExpandAll` to show all levels on load
- Programmatic drill operations

### Drill Through
📄 **Read:** [references/drill-through.md](references/drill-through.md)
- Drill-through popup for viewing raw data behind a value cell

### Editing
📄 **Read:** [references/editing.md](references/editing.md)
- Enable `PivotViewCellEditSettings` for CRUD on raw data
- Edit modes: Normal, Dialog, Batch, Command Columns
- Inline editing; add/update/delete with confirmation dialogs

### Formatting
📄 **Read:** [references/formatting.md](references/formatting.md)
- Number formatting (N, C, P, custom) via `PivotViewFormatSettings`
- Conditional formatting with `PivotViewConditionalFormatSettings`
- Apply formatting via toolbar at runtime

### Layout & Display
📄 **Read:** [references/layout-and-display.md](references/layout-and-display.md)
- Component Width/Height, classic layout
- Row/column customization (frozen headers, column width, auto-fit)
- Show/hide grand totals and sub-totals
- Hyper-links in cells, tooltips

### Toolbar
📄 **Read:** [references/tool-bar.md](references/tool-bar.md)
- Enable `ShowToolbar` with built-in items (Grid, Chart, Export, Formatting, Field List)
- Custom toolbar items and event handling
- Report management (save, load, rename, delete)

### Excel Export
📄 **Read:** [references/excel-export.md](references/excel-export.md)
- Excel export: `ExportToExcelAsync`, styling, custom file name
- CSV export, export as memory stream
- Export via toolbar or programmatically

### PDF Export
📄 **Read:** [references/pdf-export.md](references/pdf-export.md)
- PDF export: `ExportToPdfAsync`, page settings, themes
- Export chart with table

### Performance Optimization
📄 **Read:** [references/performance-best-practices.md](references/performance-best-practices.md)
- Virtual scrolling (`EnableVirtualization`) for large datasets
- Paging (`PivotViewPageSettings`) as an alternative to virtualization
- Data compression, defer layout update
- WebAssembly-specific optimizations
- Server-side pivot engine for offloading computation

### Pivot Chart
📄 **Read:** [references/pivot-chart.md](references/pivot-chart.md)
- Render chart-only, grid-only, or both (`PivotViewDisplayOption`)
- 15+ chart types via `PivotChartSettings`
- Drill-down in chart, multiple axes, export chart

### State Persistence & Events
📄 **Read:** [references/state-and-events.md](references/state-and-events.md)
- `EnablePersistence` for automatic browser local storage
- Save/load layout programmatically (`GetPersistDataAsync` / `LoadPersistDataAsync`)
- Key events reference: `BeforeColumnsRender`, `CellClick`, `DrillThrough`, `OnLoad`

### Smart Pivot (AI)
📄 **Read:** [references/smart-pivot.md](references/smart-pivot.md)
- `Syncfusion.Blazor.AI` NuGet for AI-powered features
- Smart Data Aggregation, Predictive Modeling, Adaptive Filtering
- Configure OpenAI, Azure OpenAI, or Ollama

## Quick Start Example

```razor
@using Syncfusion.Blazor.PivotView

<SfPivotView TValue="ProductDetails" Height="350">
    <PivotViewDataSourceSettings DataSource="@data" EnableSorting="true">
        <PivotViewColumns>
            <PivotViewColumn Name="Year"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="Country"></PivotViewRow>
            <PivotViewRow Name="Products"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Sold" Caption="Units Sold"></PivotViewValue>
            <PivotViewValue Name="Amount" Caption="Sold Amount"></PivotViewValue>
        </PivotViewValues>
        <PivotViewFormatSettings>
            <PivotViewFormatSetting Name="Amount" Format="C0" UseGrouping="true"></PivotViewFormatSetting>
        </PivotViewFormatSettings>
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    public List<ProductDetails> data { get; set; }
    protected override void OnInitialized()
    {
        data = ProductDetails.GetProductData().ToList();
    }
}
```

## Common Patterns

### Enable Field List + Grouping Bar
```razor
<SfPivotView TValue="ProductDetails" ShowFieldList="true" ShowGroupingBar="true">
```

### Enable Toolbar with Export
```razor
<SfPivotView TValue="ProductDetails" ShowToolbar="true"
             Toolbar="@toolbar" AllowExcelExport="true" AllowPdfExport="true">
@code {
    public List<ToolbarItems> toolbar = new List<ToolbarItems> {
        ToolbarItems.Grid, ToolbarItems.Chart, ToolbarItems.Export,
        ToolbarItems.SubTotal, ToolbarItems.GrandTotal, ToolbarItems.FieldList
    };
}
```

### Virtual Scrolling for Large Data
```razor
<SfPivotView TValue="MyData" EnableVirtualization="true" ShowTooltip="false"
             Width="100%" Height="500">
```

### Show Pivot Chart Only
```razor
<SfPivotView TValue="ProductDetails">
    <PivotViewDisplayOption View=View.Chart></PivotViewDisplayOption>
    <PivotChartSettings Title="Sales Analysis">
        <PivotChartSeries Type=ChartSeriesType.Column></PivotChartSeries>
    </PivotChartSettings>
</SfPivotView>
```

## Key Properties

| Property | Type | Purpose |
|---|---|---|
| `TValue` | Generic | Model type for data rows |
| `Height` / `Width` | string/int | Component dimensions |
| `ShowFieldList` | bool | Enable popup field list |
| `ShowGroupingBar` | bool | Enable grouping bar |
| `ShowToolbar` | bool | Enable toolbar |
| `EnableVirtualization` | bool | Virtual scrolling |
| `AllowGrouping` | bool | Enable date/number grouping |
| `AllowCalculatedField` | bool | Enable calculated fields |
| `AllowExcelExport` | bool | Enable Excel export |
| `AllowPdfExport` | bool | Enable PDF export |
| `AllowEditing` | bool (in `PivotViewCellEditSettings`) | Enable CRUD editing |
| `EnablePersistence` | bool | Save state to localStorage |