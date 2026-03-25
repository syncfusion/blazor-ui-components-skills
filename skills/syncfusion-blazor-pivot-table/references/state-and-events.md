# State Persistence & Events

## State Persistence

Automatically saves the current pivot table layout (field arrangement, sorting, filters, drill state) to browser local storage.

```razor
<!-- CRITICAL: Must set a unique ID for each pivot table on the page -->
<SfPivotView ID="pivot" TValue="ProductDetails" EnablePersistence="true">
    <PivotViewDataSourceSettings DataSource="@data">
        <!-- axis config -->
    </PivotViewDataSourceSettings>
</SfPivotView>
```

> **Warning:** Always set a unique `ID`. If two pivot tables share the same ID, they will override each other's persisted state.

**What is persisted:**
- Field axis arrangement (rows, columns, values, filters)
- Applied sorting and filters
- Expanded/collapsed drill state
- Active layout settings

---

## Programmatic Save & Load Layout

Save and restore the pivot state as a serialized string (e.g., store in a database per user).

```razor
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="SaveLayout">Save Layout</SfButton>
<SfButton OnClick="LoadLayout">Load Layout</SfButton>

<SfPivotView TValue="ProductDetails" @ref="pivot">
    <PivotViewDataSourceSettings DataSource="@data">
        <!-- axis config -->
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    SfPivotView<ProductDetails> pivot;
    private string savedLayout;

    private async void SaveLayout(MouseEventArgs e)
    {
        savedLayout = await pivot.GetPersistDataAsync();
        // store savedLayout to database / local state
    }

    private void LoadLayout(MouseEventArgs e)
    {
        pivot.LoadPersistDataAsync(savedLayout);
    }
}
```

| Method | Description |
|---|---|
| `GetPersistDataAsync()` | Returns current state as a JSON string |
| `LoadPersistDataAsync(string data)` | Restores pivot state from JSON string |

---

## Events Overview

All events are registered inside `<PivotViewEvents TValue="T">` component:

```razor
<SfPivotView TValue="ProductDetails">
    <!-- data source settings -->
    <PivotViewEvents TValue="ProductDetails"
        DataBound="OnDataBound"
        EnginePopulating="OnEnginePopulating"
        EnginePopulated="OnEnginePopulated"
        CellClick="OnCellClick"
        Drill="OnDrill">
    </PivotViewEvents>
</SfPivotView>
```

---

## Key Events Reference

### Data & Engine Events

| Event | Trigger | Args |
|---|---|---|
| `OnLoad` | Before data source settings are applied | `DataSourceSettings` |
| `DataBound` | After data source is populated | (none) |
| `EnginePopulating` | Before pivot engine recalculates | `DataSourceSettings` (modifiable) |
| `EnginePopulated` | After pivot engine finishes | `DataSourceSettings`, `PivotValues` |

```razor
<PivotViewEvents TValue="ProductDetails"
    EnginePopulating="OnEnginePopulating"
    DataBound="OnDataBound">
</PivotViewEvents>

@code {
    private void OnEnginePopulating(EnginePopulatingEventArgs args)
    {
        // Modify report before engine recalculates
        // args.DataSourceSettings...
    }

    private void OnDataBound()
    {
        // Data is ready — safe to read pivot values
    }
}
```

### Cell Interaction Events

| Event | Trigger | Key Args |
|---|---|---|
| `CellClick` | Any cell is clicked | `args.Data` (RowHeaders, ColumnHeaders, FormattedText, CssClass) |
| `CellSelecting` | Cell selection about to start | `args.Data`, `args.Cancel` |
| `CellSelected` | After cell selection completes | Selected cell data |

```razor
<PivotViewEvents TValue="ProductDetails" CellClick="OnCellClick"></PivotViewEvents>

@code {
    private void OnCellClick(CellClickEventArgs args)
    {
        if (args.Data.RowHeaders == "France")
        {
            args.Data.CssClass = "e-highlight"; // Apply custom CSS
        }
    }
}
```

### Drill Events

| Event | Trigger | Key Args |
|---|---|---|
| `Drill` | Member expanded or collapsed | `args.DrillInfo` (member name, axis, action) |
| `DrillThrough` | Value cell clicked for drill-through | `args.RowHeaders`, `args.ColumnHeaders`, `args.RawData` |
| `BeginDrillThrough` | Before drill-through dialog opens | `args.CellInfo`, `args.Type` |

```razor
<PivotViewEvents TValue="ProductDetails" Drill="OnDrill"></PivotViewEvents>

@code {
    private void OnDrill(DrillArgs<ProductDetails> args)
    {
        // args.DrillInfo.MemberName, args.DrillInfo.Axis
    }
}
```

### Export Events

| Event | Trigger |
|---|---|
| `ExportCompleted` | After export to PDF/Excel/CSV completes |
| `BeforeExport` | Before export begins (can modify export settings) |
| `ExcelQueryCellInfo` | Per-cell during Excel export (customize cell) |
| `ExcelHeaderQueryCellInfo` | Per-header-cell during Excel export |
| `PdfQueryCellInfo` | Per-cell during PDF export |
| `PdfHeaderQueryCellInfo` | Per-header-cell during PDF export |

### Chart Events

| Event | Trigger | Key Args |
|---|---|---|
| `ChartSeriesCreated` | After chart series render | `args.Cancel`, `args.Series` |

```razor
<PivotViewEvents TValue="ProductDetails"
    ChartSeriesCreated="OnChartSeriesCreated">
</PivotViewEvents>

@code {
    private void OnChartSeriesCreated(ChartSeriesCreatedEventArgs args)
    {
        args.Cancel = true; // Prevent chart series from rendering
    }
}
```

### Formatting Events

| Event | Trigger |
|---|---|
| `ConditionalFormatting` | When "Add Condition" is clicked in conditional formatting dialog |

### Toolbar & Report Events

| Event | Trigger |
|---|---|
| `ToolbarRendered` | Before toolbar renders (customize toolbar items) |
| `SaveReport` | When save report is triggered |
| `LoadReport` | When load report is triggered |
| `FetchReport` | When report list is fetched |
| `RenameReport` | When a report is renamed |
| `RemoveReport` | When a report is removed |
| `NewReport` | When new report is triggered |
| `OnActionBegin` | Before any toolbar action starts |
| `OnActionComplete` | After any toolbar action completes |
| `OnActionFailure` | When a toolbar action fails |

### Field List & Grouping Bar Events

| Event | Trigger |
|---|---|
| `FieldListRefreshed` | After field list UI refreshes |
| `FieldDragStart` | When field drag starts in grouping bar |
| `FieldDrop` | When field is dropped in grouping bar |
| `FieldDropped` | After field is dropped (both grouping bar & field list) |
| `FieldRemove` | Before a field is removed |
| `MemberEditorOpen` | When filter member editor opens |
| `AggregateMenuOpen` | When aggregation dropdown opens |
| `BeforeColumnsRender` | Before columns are framed for rendering |
| `HyperlinkCellClicked` | When a hyperlink cell is clicked |

---

## Complete Events Setup Pattern

```razor
<SfPivotView TValue="ProductDetails" @ref="pivot"
             AllowDrillThrough="true"
             AllowConditionalFormatting="true">
    <PivotViewDataSourceSettings DataSource="@data">
        <!-- axis config -->
    </PivotViewDataSourceSettings>
    <PivotViewEvents TValue="ProductDetails"
        OnLoad="OnLoad"
        DataBound="OnDataBound"
        EnginePopulating="OnEnginePopulating"
        EnginePopulated="OnEnginePopulated"
        CellClick="OnCellClick"
        Drill="OnDrill"
        DrillThrough="OnDrillThrough"
        ExportCompleted="OnExportCompleted">
    </PivotViewEvents>
</SfPivotView>
```
