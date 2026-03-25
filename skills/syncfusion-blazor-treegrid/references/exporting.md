# Exporting & Print in Syncfusion Blazor TreeGrid

## Table of Contents
- [⚠️ CRITICAL - Export Enum Types](#️-critical---export-enum-types)
- [Excel Export](#excel-export)
- [PDF Export](#pdf-export)
- [CSV Export](#csv-export)
- [Print](#print)
- [Export with Custom Properties](#export-with-custom-properties)
- [Export Only Selected Rows](#export-only-selected-rows)
- [Custom Theme in Export](#custom-theme-in-export)
- [Export with Hidden Columns](#export-with-hidden-columns)
- [Export Method Reference](#export-method-reference)

---

**⚠️ Best Practice:** For basic exporting samples, enable only the export options (toolbar buttons and properties) without extra event handlers. Event handlers shown below are for **advanced customization only**.

---

## ⚠️ CRITICAL - Export Enum Types

### Common CS0266 Compilation Error

**DO NOT confuse `HierarchyExportMode` with `HierarchyGridPrintMode`** - these are different enum types that cause compilation errors if mixed:

```csharp
// ❌ WRONG - Causes CS0266: Cannot implicitly convert type error
var excelProps = new ExcelExportProperties() 
{ 
    HierarchyExportMode = HierarchyGridPrintMode.None  // Wrong enum type!
};

var pdfProps = new PdfExportProperties() 
{ 
    HierarchyExportMode = HierarchyGridPrintMode.All  // Wrong enum type!
};

// ✅ CORRECT - Use matching enum type for export properties
var excelProps = new ExcelExportProperties() 
{ 
    HierarchyExportMode = HierarchyExportMode.None  // Correct!
};

var pdfProps = new PdfExportProperties() 
{ 
    HierarchyExportMode = HierarchyExportMode.All  // Correct!
};
```

### Enum Usage Rules

| Property/Context | Correct Enum Type | Namespace |
|------------------|-------------------|-----------|
| `ExcelExportProperties.HierarchyExportMode` | `HierarchyExportMode` | `Syncfusion.Blazor.Grids` |
| `PdfExportProperties.HierarchyExportMode` | `HierarchyExportMode` | `Syncfusion.Blazor.Grids` |
| Print operations | `HierarchyGridPrintMode` | `Syncfusion.Blazor.Grids` |

### Available Enum Values

Both enum types have the same three values but are **NOT interchangeable**:

**HierarchyExportMode (for Excel/PDF export):**
```csharp
HierarchyExportMode.All       // Export all records (expanded + collapsed nodes)
HierarchyExportMode.Expanded  // Export only visible/expanded records
HierarchyExportMode.None      // Export current page only (flat, no hierarchy)
```

**HierarchyGridPrintMode (for print operations only):**
```csharp
HierarchyGridPrintMode.All       // Print all records
HierarchyGridPrintMode.Expanded  // Print only expanded records
HierarchyGridPrintMode.None      // Print current page only
```

### Complete Working Example

```razor
@page "/export-correct-enums"
@using Syncfusion.Blazor.TreeGrid
@using Syncfusion.Blazor.Grids

<button @onclick="ExportToExcel">Export Excel</button>
<button @onclick="ExportToPdf">Export PDF</button>

<SfTreeGrid @ref="TreeGridRef"
            DataSource="@TreeData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            TreeColumnIndex="1"
            AllowExcelExport="true"
            AllowPdfExport="true">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="ID" Width="80" IsPrimaryKey="true" />
        <TreeGridColumn Field="TaskName" HeaderText="Task" Width="200" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    private SfTreeGrid<TaskItem> TreeGridRef;

    private async Task ExportToExcel()
    {
        var excelProps = new ExcelExportProperties()
        {
            FileName = "Report.xlsx",
            // ✅ CORRECT: Use HierarchyExportMode enum
            HierarchyExportMode = HierarchyExportMode.All
        };
        await TreeGridRef.ExportToExcelAsync(excelProps);
    }

    private async Task ExportToPdf()
    {
        var pdfProps = new PdfExportProperties()
        {
            FileName = "Report.pdf",
            // ✅ CORRECT: Use HierarchyExportMode enum
            HierarchyExportMode = HierarchyExportMode.Expanded
        };
        await TreeGridRef.ExportToPdfAsync(pdfProps);
    }

    public class TaskItem
    {
        public int TaskId { get; set; }
        public int? ParentId { get; set; }
        public string TaskName { get; set; }
    }

    private List<TaskItem> TreeData = new()
    {
        new TaskItem { TaskId = 1, ParentId = null, TaskName = "Planning" },
        new TaskItem { TaskId = 2, ParentId = 1, TaskName = "Design" },
    };
}
```

---

## Excel Export

Enable and wire up Excel export with events:

```razor
<SfTreeGrid @ref="TreeRef"
            DataSource="@TreeData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            AllowExcelExport="true"
            Toolbar="@(new List<string> { "ExcelExport" })">
    <TreeGridEvents TValue="TaskItem"
                    OnToolbarClick="ToolbarClick"
                    OnExcelExport="OnExcelExportHandler"
                    ExcelQueryCellInfoEvent="OnExcelQueryCellInfoHandler" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="ID"        Width="80" IsPrimaryKey="true" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="200" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" />
        <TreeGridColumn Field="Priority" HeaderText="Priority"  Width="120" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    private SfTreeGrid<TaskItem> TreeRef;

    private async Task ToolbarClick(Syncfusion.Blazor.Navigations.ClickEventArgs args)
    {
        if (args.Item.Id.Contains("excelexport"))
            await TreeRef.ExportToExcelAsync();
    }

    /// <summary>
    /// Fires before Excel export starts
    /// </summary>
    private void OnExcelExportHandler(object args)
    {
        // TODO: Implement custom Excel export logic here
    }

    /// <summary>
    /// Fires for each cell during Excel export
    /// </summary>
    private void OnExcelQueryCellInfoHandler(ExcelQueryCellInfoEventArgs<TaskItem> args)
    {
        // TODO: Implement custom cell formatting for Excel export
    }
}
```

**Export with options:**
```csharp
var excelProps = new Syncfusion.Blazor.TreeGrid.TreeGridExcelExportProperties
{
    FileName = "TaskReport.xlsx",
    IsCollapsedStatePersist = true  // respect current expand/collapse state
};
await TreeRef.ExportToExcelAsync(excelProps);
```

---

## PDF Export

```razor
<SfTreeGrid @ref="TreeRef"
            AllowPdfExport="true"
            Toolbar="@(new List<string> { "PdfExport" })">
    <TreeGridEvents TValue="TaskItem"
                    OnToolbarClick="ToolbarClick"
                    OnPdfExport="OnPdfExportHandler"
                    PdfQueryCellInfoEvent="OnPdfQueryCellInfoHandler" />
    ...
</SfTreeGrid>

@code {
    private SfTreeGrid<TaskItem> TreeRef;

    private async Task ToolbarClick(Syncfusion.Blazor.Navigations.ClickEventArgs args)
    {
        if (args.Item.Id.Contains("pdfexport"))
            await TreeRef.ExportToPdfAsync();
    }

    /// <summary>
    /// Fires before PDF export starts
    /// </summary>
    private void OnPdfExportHandler(object args)
    {
        // TODO: Implement custom PDF export logic here
    }

    /// <summary>
    /// Fires for each cell during PDF export
    /// </summary>
    private void OnPdfQueryCellInfoHandler(PdfQueryCellInfoEventArgs<TaskItem> args)
    {
        // TODO: Implement custom cell formatting for PDF export
        // Example patterns:
        /*
        // Customize cell style in exported PDF
        if (args.Column.Field == "Priority" && args.Data.GetType().GetProperty("Priority")?.GetValue(args.Data)?.ToString() == "Critical")
        {
            args.Style = new PdfStyle
            {
                BackgroundColor = new PdfColor(255, 0, 0),
                TextBrush = new PdfSolidBrush(new PdfColor(255, 255, 255))
            };
        }
        
        // Format specific columns
        // if (args.Column.Field == "StartDate")
        // {
        //     args.Style = new PdfStyle { FontSize = 10 };
        // }
        
        // Highlight rows based on conditions
        // if (IsHighPriorityRow(args.Data))
        // {
        //     args.Style = new PdfStyle { BackgroundColor = new PdfColor(255, 255, 0) };
        // }
        */
    }
}
```

**Export with options:**
```csharp
var pdfProps = new Syncfusion.Blazor.TreeGrid.TreeGridPdfExportProperties
{
    FileName        = "TaskReport.pdf",
    PageSize        = Syncfusion.Blazor.Grids.PdfPageSize.A4,
    IncludeTemplateColumn = true
};
await TreeRef.ExportToPdfAsync(pdfProps);
```

---

## CSV Export

```csharp
await TreeRef.ExportToCsvAsync();

// With options
var csvProps = new Syncfusion.Blazor.TreeGrid.TreeGridExcelExportProperties
{
    FileName = "tasks.csv"
};
await TreeRef.ExportToCsvAsync(csvProps);
```

Add CSV to toolbar: `Toolbar="@(new List<string> { "CsvExport" })"`

---

## Print

```razor
<SfTreeGrid @ref="TreeRef"
            Toolbar="@(new List<string> { "Print" })">
    <TreeGridEvents TValue="TaskItem" OnToolbarClick="ToolbarClick" />
    ...
</SfTreeGrid>

@code {
    private SfTreeGrid<TaskItem> TreeRef;

    private async Task ToolbarClick(Syncfusion.Blazor.Navigations.ClickEventArgs args)
    {
        if (args.Item.Id.Contains("print"))
            await TreeRef.PrintAsync();
    }

    // Or call directly from any button
    private async Task PrintGrid() => await TreeRef.PrintAsync();
}
```

**Print settings:**
```razor
<SfTreeGrid PrintMode="PrintMode.CurrentPage" ...>
<!-- PrintMode: AllPages (default) | CurrentPage -->
```

---

## Export with Custom Properties

Customize column headers, data, and hierarchy in the export:

```csharp
var excelProps = new TreeGridExcelExportProperties
{
    FileName = "CustomExport.xlsx",
    // Export only specific columns by index
    Columns = new List<Syncfusion.Blazor.Grids.GridColumn>
    {
        new() { Field = "TaskName", HeaderText = "Task",     Width = 200 },
        new() { Field = "Duration", HeaderText = "Days",     Width = 100 },
        new() { Field = "Priority", HeaderText = "Priority", Width = 120 }
    }
};
await TreeRef.ExportToExcelAsync(excelProps);
```

---

## Export Only Selected Rows

```csharp
var excelProps = new TreeGridExcelExportProperties
{
    ExportType = ExportType.CurrentPage  // or SelectedRecords
};
await TreeRef.ExportToExcelAsync(excelProps);
```

| ExportType | Description |
|---|---|
| `AllPages` | Export entire data source (default) |
| `CurrentPage` | Export only the current page |
| `SelectedRecords` | Export only selected rows |

---

## Custom Theme in Export

Apply a custom theme to the Excel export:

```csharp
var theme = new ExcelTheme
{
    Header = new ExcelStyle
    {
        FontColor = "#FFFFFF",
        FontSize = 12,
        Bold = true,
        BackColor = "#1565C0"
    },
    Record = new ExcelStyle
    {
        FontColor = "#212121",
        FontSize = 11
    }
};

var excelProps = new TreeGridExcelExportProperties { Theme = theme };
await TreeRef.ExportToExcelAsync(excelProps);
```

---

## Export with Hidden Columns

By default, hidden columns are excluded. To include them:

```csharp
var excelProps = new TreeGridExcelExportProperties
{
    IncludeHiddenColumn = true
};
await TreeRef.ExportToExcelAsync(excelProps);
```

---

## Export Method Reference

| Method | Parameters | Description |
|--------|------------|-------------|
| `ExportToExcelAsync()` | None | Export to Excel with default settings |
| `ExportToExcelAsync(ExcelExportProperties)` | Custom properties | Export to Excel with custom options |
| `ExportToPdfAsync()` | None | Export to PDF with default settings |
| `ExportToPdfAsync(PdfExportProperties)` | Custom properties | Export to PDF with custom options |
| `ExportToCsvAsync()` | None | Export to CSV with default settings |
| `ExportToCsvAsync(TreeGridExcelExportProperties)` | Custom properties | Export to CSV with custom options |
| `PrintAsync()` | None | Print the TreeGrid |

---
