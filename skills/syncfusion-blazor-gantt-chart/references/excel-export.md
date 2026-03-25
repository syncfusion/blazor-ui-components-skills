# Excel Export

## Table of Contents
- [When to Read](#when-to-read)
- [Key Properties and Features](#key-properties-and-features)
- [Basic Excel Export](#basic-excel-export)
- [CSV Export](#csv-export)
- [Export Properties Configuration](#export-properties-configuration)
- [Custom File Name](#custom-file-name)
- [Include Hidden Columns](#include-hidden-columns)
- [Export Specific Columns](#export-specific-columns)
- [Export Selected Rows](#export-selected-rows)
- [Export with Theme](#export-with-theme)
- [Custom Cell Styling](#custom-cell-styling)
- [Export Events](#export-events)
- [Best Practices](#best-practices)
- [Common Scenarios](#common-scenarios)
- [Troubleshooting](#troubleshooting)

---

## When to Read

Use this reference when you need to:
- Export Gantt task data to `.xlsx` (Excel) or `.csv` format
- Customize file names, columns, themes, and cell styles
- Export filtered/selected rows rather than all data
- Handle export events for pre/post-export logic

---

## Key Properties and Features

### Component-Level Properties
- `AllowExcelExport` — Enables Excel/CSV export functionality on `SfGantt`
- `ExportToExcelAsync()` — Exports to `.xlsx` Excel format
- `ExportToCsvAsync(ExcelExportProperties?)` — Exports to `.csv` format

### ExcelExportProperties
- `FileName` — Custom output file name (include `.xlsx` or `.csv` extension)
- `IncludeHiddenColumn` — Whether to include hidden columns (default: `false`)
- `Columns` — Specific `List<GanttColumn>` to export (overrides visible columns)
- `DataSource` — Custom data collection to export (overrides full dataset)
- `Theme` — Predefined theme: `ExcelTheme` object with header/record/caption styles
- `Header` — Custom header rows above the column headers
- `Footer` — Custom footer rows below the data

### Events
- `ExcelExporting` — Triggered before export starts (cancellable via `args.Cancel`)
- `ExcelExported` — Triggered after export completes
- `ExcelQueryCellInfo` — Customize individual cell appearance during export
- `ExcelHeaderQueryCellInfo` — Customize column header cells during export

---

## Basic Excel Export

Enable Excel export and connect it to a toolbar button:

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Navigations

<SfGantt @ref="Gantt"
         DataSource="@TaskCollection"
         Height="450px"
         Width="900px"
         AllowExcelExport="true"
         Toolbar="@ToolbarItems">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate"
                     EndDate="EndDate" Duration="Duration" Progress="Progress"
                     Dependency="Predecessor" ParentID="ParentID">
    </GanttTaskFields>
    <GanttEvents OnToolbarClick="ToolbarClickHandler" TValue="TaskData" />
</SfGantt>

@code {
    private SfGantt<TaskData> Gantt;
    private List<TaskData> TaskCollection { get; set; }
    private List<object> ToolbarItems = new List<object>()
    {
        new ToolbarItem() { Text = "Excel Export", TooltipText = "Excel Export",
                           Id = "ExcelExport", PrefixIcon = "e-excelexport" }
    };

    protected override void OnInitialized()
    {
        TaskCollection = GetTaskCollection();
    }

    public async void ToolbarClickHandler(ClickEventArgs args)
    {
        if (args.Item.Id == "ExcelExport")
        {
            await Gantt.ExportToExcelAsync();
        }
    }

    public class TaskData
    {
        public int TaskID { get; set; }
        public string TaskName { get; set; }
        public DateTime StartDate { get; set; }
        public DateTime? EndDate { get; set; }
        public string Duration { get; set; }
        public int Progress { get; set; }
        public int? ParentID { get; set; }
        public string Predecessor { get; set; }
    }

    public static List<TaskData> GetTaskCollection()
    {
        return new List<TaskData>()
        {
            new TaskData() { TaskID = 1, TaskName = "Project initiation",
                            StartDate = new DateTime(2022, 04, 05), EndDate = new DateTime(2022, 04, 08) },
            new TaskData() { TaskID = 2, TaskName = "Identify Site location",
                            StartDate = new DateTime(2022, 04, 05), Duration = "0", Progress = 30, ParentID = 1 },
            new TaskData() { TaskID = 3, TaskName = "Perform soil test",
                            StartDate = new DateTime(2022, 04, 05), Duration = "4", Progress = 40,
                            ParentID = 1, Predecessor = "2" }
        };
    }
}
```

> **Prerequisite**: `AllowExcelExport="true"` must be set on `SfGantt`. Without it, `ExportToExcelAsync()` has no effect.

---

## CSV Export

Export as comma-separated values using `ExportToCsvAsync()`:

```razor
<SfGantt @ref="Gantt" DataSource="@TaskCollection" AllowExcelExport="true"
         Toolbar="@(new List<string>() { "ExcelExport", "CsvExport" })">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttEvents OnToolbarClick="ToolbarClickHandler" TValue="TaskData" />
</SfGantt>

@code {
    private SfGantt<TaskData> Gantt;

    public async void ToolbarClickHandler(ClickEventArgs args)
    {
        if (args.Item.Id == "ExcelExport")
            await Gantt.ExportToExcelAsync();
        else if (args.Item.Id == "CsvExport")
            await Gantt.ExportToCsvAsync();
    }
}
```

---

## Export Properties Configuration

Pass an `ExcelExportProperties` object to configure the export:

```csharp
public async Task ExportWithOptions()
{
    ExcelExportProperties exportProperties = new ExcelExportProperties();
    exportProperties.FileName = "ProjectSchedule.xlsx";
    exportProperties.IncludeHiddenColumn = true;
    await Gantt.ExportToExcelAsync(exportProperties);
}
```

---

## Custom File Name

```csharp
ExcelExportProperties exportProperties = new ExcelExportProperties();
exportProperties.FileName = $"GanttExport_{DateTime.Now:yyyy-MM-dd}.xlsx";
await Gantt.ExportToExcelAsync(exportProperties);
```

> Use `.xlsx` for Excel and `.csv` for CSV exports. Omitting the extension may cause incorrect file associations.

---

## Include Hidden Columns

By default, hidden columns are excluded from export. To include them:

```csharp
ExcelExportProperties exportProperties = new ExcelExportProperties();
exportProperties.IncludeHiddenColumn = true;
await Gantt.ExportToExcelAsync(exportProperties);
```

---

## Export Specific Columns

Override the exported column set by specifying a `Columns` list:

```csharp
ExcelExportProperties exportProperties = new ExcelExportProperties();
exportProperties.Columns = new List<GanttColumn>
{
    new GanttColumn { Field = "TaskID",    HeaderText = "ID",        Width = "50" },
    new GanttColumn { Field = "TaskName",  HeaderText = "Task Name", Width = "200" },
    new GanttColumn { Field = "StartDate", HeaderText = "Start",     Width = "100" },
    new GanttColumn { Field = "EndDate",   HeaderText = "End",       Width = "100" },
    new GanttColumn { Field = "Duration",  HeaderText = "Duration",  Width = "80" },
    new GanttColumn { Field = "Progress",  HeaderText = "Progress",  Width = "80" }
};
await Gantt.ExportToExcelAsync(exportProperties);
```

---

## Export Selected Rows

Export only the rows currently selected in the Gantt:

```csharp
var selectedRecords = await Gantt.GetSelectedRecordsAsync();
ExcelExportProperties exportProperties = new ExcelExportProperties();
exportProperties.DataSource = selectedRecords;
await Gantt.ExportToExcelAsync(exportProperties);
```

---

## Export with Theme

Apply a predefined Excel theme to styled exports:

```csharp
ExcelExportProperties exportProperties = new ExcelExportProperties();
exportProperties.Theme = new ExcelTheme()
{
    Header = new ExcelStyle()
    {
        FontColor = "#FFFFFF",
        FontName = "Calibri",
        FontSize = 12,
        Bold = true,
        BackColor = "#0070C0"
    },
    Record = new ExcelStyle()
    {
        FontColor = "#000000",
        FontName = "Calibri",
        FontSize = 11
    },
    Caption = new ExcelStyle()
    {
        FontColor = "#000000",
        Bold = true,
        BackColor = "#D9E1F2"
    }
};
await Gantt.ExportToExcelAsync(exportProperties);
```

---

## Custom Cell Styling

Use `ExcelQueryCellInfo` to apply per-cell styles during export:

```razor
<SfGantt @ref="Gantt" DataSource="@TaskCollection" AllowExcelExport="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttEvents ExcelQueryCellInfo="ExcelQueryCellInfoHandler"
                 ExcelHeaderQueryCellInfo="ExcelHeaderCellHandler"
                 TValue="TaskData" />
</SfGantt>

@code {
    private SfGantt<TaskData> Gantt;

    public void ExcelQueryCellInfoHandler(ExcelQueryCellInfoEventArgs<TaskData> args)
    {
        if (args.Column.Field == "Progress" && args.Data.Progress < 50)
        {
            args.Style = new ExcelStyle()
            {
                BackColor = "#FFCCCC",
                FontColor = "#CC0000",
                Bold = true
            };
        }
    }

    public void ExcelHeaderCellHandler(ExcelHeaderQueryCellInfoEventArgs args)
    {
        args.Style = new ExcelStyle()
        {
            BackColor = "#003366",
            FontColor = "#FFFFFF",
            Bold = true,
            FontSize = 12
        };
    }
}
```

---

## Export Events

### ExcelExporting (Before Export)

Triggered before export starts — cancel or modify options:

```razor
<SfGantt DataSource="@TaskCollection" AllowExcelExport="true">
    <GanttEvents ExcelExporting="OnExcelExporting"
                 ExcelExported="OnExcelExported"
                 TValue="TaskData" />
</SfGantt>

@code {
    public void OnExcelExporting(ExcelExportEventArgs args)
    {
        if (TaskCollection.Count > 5000)
            args.Cancel = true; // Cancel for very large datasets
    }

    public void OnExcelExported(ExcelExportedEventArgs args)
    {
        Console.WriteLine("Excel export completed successfully.");
    }
}
```

---

## Best Practices

### File Naming
- Include date/project identifiers: `$"Project_{name}_{DateTime.Now:yyyy-MM-dd}.xlsx"`
- Avoid special characters (`, :, /, \, ?, *, |, <, >`) in file names
- Use `.xlsx` for Excel and `.csv` for CSV — don't mix extensions

### Column Selection
- Hide or omit auxiliary/internal columns before export
- Use business-friendly `HeaderText` for downstream consumers

### Performance
- For 10k+ rows: use `DataSource` to export only filtered/visible data
- Combine with filtering before export so users export only what they need
- Avoid complex `ExcelQueryCellInfo` logic on every cell for large datasets

### Data Quality
- Ensure numeric fields (Duration, Progress) are `int`/`double` types — not strings
- Normalize date formats for the target audience's locale

---

## Common Scenarios

### Export Current Filtered View

```csharp
var filteredRecords = await Gantt.GetFilteredRecordsAsync();
ExcelExportProperties props = new ExcelExportProperties();
props.DataSource = filteredRecords;
await Gantt.ExportToExcelAsync(props);
```

### Export Summary (Parent Tasks Only)

```csharp
var summaryTasks = TaskCollection.Where(t => t.ParentID == null).ToList();
ExcelExportProperties props = new ExcelExportProperties();
props.DataSource = summaryTasks;
props.FileName = "ProjectSummary.xlsx";
await Gantt.ExportToExcelAsync(props);
```

### Export with Custom Header Row

```csharp
ExcelExportProperties exportProperties = new ExcelExportProperties();
exportProperties.Header = new ExcelHeader()
{
    HeaderRows = 1,
    Rows = new List<ExcelRow>()
    {
        new ExcelRow()
        {
            Index = 1,
            Cells = new List<ExcelCell>()
            {
                new ExcelCell()
                {
                    Index = 1,
                    ColSpan = 5,
                    Value = "Weekly Project Status Report",
                    Style = new ExcelStyle()
                    {
                        FontColor = "#FFFFFF",
                        BackColor = "#0070C0",
                        Bold = true,
                        FontSize = 16,
                        HAlign = ExcelHAlign.Center
                    }
                }
            }
        }
    }
};
await Gantt.ExportToExcelAsync(exportProperties);
```

### Progress Color Coding

```csharp
public void ExcelQueryCellInfoHandler(ExcelQueryCellInfoEventArgs<TaskData> args)
{
    if (args.Column.Field == "Progress")
    {
        args.Style = args.Data.Progress switch
        {
            100     => new ExcelStyle { BackColor = "#C6EFCE", FontColor = "#276221" },  // Green
            >= 75   => new ExcelStyle { BackColor = "#FFEB9C", FontColor = "#9C6500" },  // Yellow
            _       => new ExcelStyle { BackColor = "#FFC7CE", FontColor = "#9C0006" }   // Red
        };
    }
}
```

---

## Troubleshooting

**Issue**: Export does nothing / no file downloaded  
**Solution**: Verify `AllowExcelExport="true"` is set on `SfGantt`. Check browser console for errors.

**Issue**: Hidden columns are not included  
**Solution**: Set `exportProperties.IncludeHiddenColumn = true` explicitly.

**Issue**: Export contains wrong columns  
**Solution**: Set `exportProperties.Columns` to explicitly define the columns to include.

**Issue**: Date values look incorrect in Excel  
**Solution**: Verify source data uses `DateTime` types, not strings. Check culture/locale settings.

**Issue**: Large export is slow or crashes browser  
**Solution**: Use `DataSource` to export a filtered/paged subset.

**Issue**: Custom theme not applying  
**Solution**: Ensure `ExcelTheme` styles use valid hex color strings (`#RRGGBB`).

**Issue**: `ExcelQueryCellInfo` not firing  
**Solution**: Confirm the event is wired in `GanttEvents`: `ExcelQueryCellInfo="ExcelQueryCellInfoHandler"`.

---
