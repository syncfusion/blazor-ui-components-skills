# Column Basics and Formatting

## Table of Contents
- [When to Use](#when-to-use)
- [Define Columns with GanttColumns](#define-columns-with-ganttcolumns)
- [Column Types](#column-types)
- [Number Formatting](#number-formatting)
- [Date Formatting](#date-formatting)
- [AutoFit Columns](#autofit-columns)
- [Show or Hide Columns](#show-or-hide-columns)
- [Change Tree Column Position](#change-tree-column-position)
- [Responsive Columns with HideAtMedia](#responsive-columns-with-hideatmedia)
- [Control Column-Level Actions](#control-column-level-actions)
- [Key Properties and APIs](#key-properties-and-apis)
- [Common Scenarios and Decisions](#common-scenarios-and-decisions)
- [Troubleshooting](#troubleshooting)

---

## When to Use

Guide the user to this reference when they need to:
- Define which task fields appear as grid columns and in what order
- Apply number or date formatting to column values
- Show or hide columns programmatically or via column chooser
- Auto-fit column widths to content
- Move the tree expand/collapse icons to a different column
- Hide columns at specific screen widths (responsive)
- Enable or disable sorting, filtering, editing, resizing, or reordering per column

---

## Define Columns with GanttColumns

When the user wants explicit control over grid columns (fields, headers, widths, alignment), define `GanttColumn` entries inside `GanttColumns`. If `GanttColumns` is omitted, the component auto-generates columns from `GanttTaskFields`.

To add columns not in `GanttTaskFields` (e.g., custom `Status`, `WorkersCount`), include them as additional `GanttColumn` entries.

> If `Field` does not exist in the data model, the column renders empty.

```razor
@using Syncfusion.Blazor.Gantt

<SfGantt DataSource="@TaskCollection" Height="450px" Width="700px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttColumns>
        <GanttColumn Field="TaskID" Width="100"></GanttColumn>
        <GanttColumn Field="TaskName" HeaderText="Job Name" Width="200"></GanttColumn>
        <GanttColumn Field="StartDate" HeaderText="Start Date" Width="150" Format="d"
                     TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right">
        </GanttColumn>
        <GanttColumn Field="Duration" HeaderText="Duration" Width="120"></GanttColumn>
        <GanttColumn Field="Progress" HeaderText="Progress" Width="120" Format="N2"
                     Type="Syncfusion.Blazor.Grids.ColumnType.Integer">
        </GanttColumn>
        <!-- Custom field not in GanttTaskFields -->
        <GanttColumn Field="Status" HeaderText="Status" Width="150"></GanttColumn>
    </GanttColumns>
</SfGantt>

@code {
    private List<TaskData> TaskCollection { get; set; }
    protected override void OnInitialized()
    {
        TaskCollection = new List<TaskData>
        {
            new TaskData { TaskID = 1, TaskName = "Project initiation", StartDate = new DateTime(2022, 04, 05), EndDate = new DateTime(2022, 04, 21), Status = "Progress" },
            new TaskData { TaskID = 2, TaskName = "Identify site location", StartDate = new DateTime(2022, 04, 05), Duration = "0", Progress = 30, ParentID = 1, Status = "Progress" },
            new TaskData { TaskID = 3, TaskName = "Perform soil test", StartDate = new DateTime(2022, 04, 05), Duration = "4", Progress = 40, ParentID = 1, Status = "Hold" },
            new TaskData { TaskID = 4, TaskName = "Project estimation", StartDate = new DateTime(2022, 04, 06), EndDate = new DateTime(2022, 04, 21), Status = "Progress" },
            new TaskData { TaskID = 5, TaskName = "Develop floor plan", StartDate = new DateTime(2022, 04, 06), Duration = "3", Progress = 30, ParentID = 4, Status = "PostPoned" }
        };
    }

    public class TaskData
    {
        public int TaskID { get; set; }
        public string TaskName { get; set; }
        public DateTime StartDate { get; set; }
        public DateTime? EndDate { get; set; }
        public string Duration { get; set; }
        public int Progress { get; set; }
        public string Status { get; set; }
        public int? ParentID { get; set; }
    }
}
```

---

## Column Types

When the user needs explicit type handling for formatting or filtering, set the `Type` property using `Syncfusion.Blazor.Grids.ColumnType`.

**Supported types:**

| Type | Use for |
|---|---|
| `String` | Text fields (default) |
| `Integer` / `Double` / `Decimal` / `Long` | Numeric fields |
| `Boolean` | True/false checkbox display |
| `Date` | Date-only values |
| `DateTime` | Date and time values |
| `DateOnly` | `DateOnly` model properties (additional columns only) |
| `TimeOnly` | `TimeOnly` model properties (additional columns only) |
| `Checkbox` | Dedicated checkbox column |

> `DateOnly` and `TimeOnly` types are supported only for **additional columns** (not GanttTaskFields-mapped columns).

```razor
<GanttColumn Field="StartDate" HeaderText="Start Date"
             Format="dd/MM/yyyy hh:mm"
             Type="Syncfusion.Blazor.Grids.ColumnType.DateTime"
             Width="150">
</GanttColumn>
<GanttColumn Field="Progress" HeaderText="Progress"
             Type="Syncfusion.Blazor.Grids.ColumnType.Integer"
             Format="P2" Width="150">
</GanttColumn>
```

---

## Number Formatting

When the user wants to control how numeric values display (decimal places, currency, percentage), use the `Format` property.

| Format | Description | Example output |
|---|---|---|
| `N` / `N2` | Numeric with decimal places | `1,234.56` |
| `C` / `C2` | Currency | `$1,234.56` |
| `P` / `P2` | Percentage (input 0–1 range) | `45.00%` |

> Format strings apply to **display only** — the underlying data value is unchanged.

```razor
<SfGantt DataSource="@TaskCollection" Height="450px" Width="700px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttColumns>
        <GanttColumn Field="TaskID" Width="150" Format="N2"></GanttColumn>
        <GanttColumn Field="TaskName" HeaderText="Job Name" Width="250"></GanttColumn>
        <GanttColumn Field="Progress" Format="P2" Width="200"></GanttColumn>
    </GanttColumns>
</SfGantt>
```

---

## Date Formatting

When the user wants to control the date display format in columns, set `Format` to a date pattern string.

| Format string | Output |
|---|---|
| `"d"` | Short date: `4/5/2022` |
| `"dd/MM/yyyy"` | `05/04/2022` |
| `"MM/dd/yyyy"` | `04/05/2022` |
| `"dd.MM.yyyy"` | `05.04.2022` |
| `"dd/MM/yyyy hh:mm tt"` | `05/04/2022 12:00 AM` |

```razor
<SfGantt DataSource="@TaskCollection" Height="450px" Width="700px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttColumns>
        <GanttColumn Field="TaskID" Width="100"></GanttColumn>
        <GanttColumn Field="TaskName" HeaderText="Job Name" Width="250"></GanttColumn>
        <GanttColumn Field="StartDate" Format="@DateFormat"></GanttColumn>
    </GanttColumns>
</SfGantt>

@code {
    private string DateFormat = "MM/dd/yyyy";
}
```

---

## AutoFit Columns

When the user wants column widths to automatically fit their content, enable `AllowResizing="true"` (users can double-click the column header resizer) or call `AutoFitColumnsAsync` programmatically.

**Programmatic autofit on initial load** — call it in the `DataBound` event:

```razor
<SfGantt @ref="Gantt" DataSource="@TaskCollection" Height="450px" Width="700px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttEvents DataBound="DataBoundHandler" TValue="TaskData"></GanttEvents>
</SfGantt>

@code {
    private SfGantt<TaskData> Gantt;

    private async void DataBoundHandler(object args)
    {
        // Auto-fit specific columns by field name
        await Gantt.AutoFitColumnsAsync(new string[] { "TaskName", "StartDate", "EndDate" });
    }
}
```

---

## Show or Hide Columns

When the user needs to toggle column visibility programmatically, use `ShowColumnsAsync` and `HideColumnsAsync`. Pass the header text or field name and specify which identifier is used.

```razor
<button @onclick="ShowColumns">Show</button>
<button @onclick="HideColumns">Hide</button>

<SfGantt @ref="Gantt" DataSource="@TaskCollection" Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttColumns>
        <GanttColumn Field="TaskID" Width="100"></GanttColumn>
        <GanttColumn Field="TaskName" HeaderText="Task Name" Width="250"></GanttColumn>
        <GanttColumn Field="Duration"></GanttColumn>
        <GanttColumn Field="Progress"></GanttColumn>
    </GanttColumns>
</SfGantt>

@code {
    public SfGantt<TaskData> Gantt;
    private string[] columns = { "TaskName", "Duration" };

    // Hide by field name
    public async Task HideColumns() => await Gantt.HideColumnsAsync(columns, "Field");

    // Show by field name
    public async Task ShowColumns() => await Gantt.ShowColumnsAsync(columns, "Field");
}
```

**Second parameter options:**
- `"Field"` — identify columns by their `Field` property
- `"HeaderText"` — identify columns by their `HeaderText` property

---

## Change Tree Column Position

When the user wants the expand/collapse tree icons to appear in a column other than the first, set `TreeColumnIndex` to the 0-based index of the desired column. Default is `0`.

```razor
<SfGantt DataSource="@TaskCollection" TreeColumnIndex="2" Height="450px" Width="700px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
</SfGantt>
```

---

## Responsive Columns with HideAtMedia

When the user wants columns to automatically hide or show based on screen width (responsive layout), use the `HideAtMedia` property with a CSS media query string.

```razor
<GanttColumns>
    <!-- Hide TaskID column when browser width >= 700px -->
    <GanttColumn Field="TaskID" Width="150" HideAtMedia="(min-width: 700px)"></GanttColumn>
    <GanttColumn Field="TaskName" HeaderText="Job Name" Width="250"></GanttColumn>
    <GanttColumn Field="StartDate"></GanttColumn>
    <!-- Hide Duration column when browser width >= 500px -->
    <GanttColumn Field="Duration" HideAtMedia="(min-width: 500px)"></GanttColumn>
</GanttColumns>
```

---

## Control Column-Level Actions

When the user wants to lock certain columns from being edited, sorted, filtered, resized, or reordered, set the corresponding `Allow*` property to `false` on the `GanttColumn`. These are per-column overrides; the global setting on `SfGantt` must also be enabled for the feature to work.

```razor
<SfGantt DataSource="@TaskCollection" Height="450px" Width="900px"
         AllowSorting="true" AllowFiltering="true" AllowReordering="true" AllowResizing="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttColumns>
        <GanttColumn Field="TaskID" Width="100" AllowResizing="false"></GanttColumn>
        <GanttColumn Field="TaskName" AllowReordering="false" Width="250"></GanttColumn>
        <GanttColumn Field="StartDate" AllowEditing="false"></GanttColumn>
        <GanttColumn Field="Duration" AllowSorting="false"></GanttColumn>
        <GanttColumn Field="Progress" AllowFiltering="false"></GanttColumn>
    </GanttColumns>
    <GanttEditSettings AllowEditing="true"></GanttEditSettings>
</SfGantt>
```

---

## Key Properties and APIs

| Property / Method | Where Used | Purpose |
|---|---|---|
| `Field` | `GanttColumn` | Maps column to task model property — required |
| `HeaderText` | `GanttColumn` | Column header label |
| `Width` | `GanttColumn` | Column width in px or % |
| `TextAlign` | `GanttColumn` | `Left`, `Center`, `Right` |
| `Format` | `GanttColumn` | Date/number format string |
| `Type` | `GanttColumn` | `ColumnType` enum for explicit data type |
| `Visible` | `GanttColumn` | `false` hides column (still exists in model) |
| `HideAtMedia` | `GanttColumn` | CSS media query to auto-hide on screen size |
| `TreeColumnIndex` | `SfGantt` | 0-based index of column showing tree icons (default: `0`) |
| `AllowResizing` | `SfGantt` / `GanttColumn` | Enables column resize via drag |
| `AllowReordering` | `SfGantt` / `GanttColumn` | Enables column reorder via drag |
| `AllowEditing` | `GanttColumn` | Per-column editing permission |
| `AllowSorting` | `GanttColumn` | Per-column sorting permission |
| `AllowFiltering` | `GanttColumn` | Per-column filtering permission |
| `ShowColumnsAsync(cols, by)` | `SfGantt` method | Show columns by `"Field"` or `"HeaderText"` |
| `HideColumnsAsync(cols, by)` | `SfGantt` method | Hide columns by `"Field"` or `"HeaderText"` |
| `AutoFitColumnsAsync(fields)` | `SfGantt` method | Auto-fit column width to content |

---

## Common Scenarios and Decisions

**User wants to define which columns appear and in what order**
→ Add `GanttColumns` with explicit `GanttColumn` entries. Order of entries = column order.

**User wants to display a custom field not in GanttTaskFields**
→ Add a `GanttColumn` with the custom `Field` name. It must exist in the task model.

**User wants to format a date column as `MM/dd/yyyy`**
→ Set `Format="MM/dd/yyyy"` on the `GanttColumn`. Works for `Date` and `DateTime` types.

**User wants to show progress as a percentage**
→ Set `Format="P2"` and `Type="ColumnType.Integer"` on the Progress column.

**User wants column widths to auto-fit content on load**
→ Call `AutoFitColumnsAsync(new string[] { "Field1", "Field2" })` inside the `DataBound` event.

**User wants to hide a column dynamically from code**
→ Call `Gantt.HideColumnsAsync(new string[] { "FieldName" }, "Field")`.

**User wants certain columns to not be editable**
→ Set `AllowEditing="false"` on the specific `GanttColumn` (global `AllowEditing` on `GanttEditSettings` must still be `true`).

**User wants tree icons on the second column instead of the first**
→ Set `TreeColumnIndex="1"` on `SfGantt`.

---

## Troubleshooting

**Column renders empty**
- Confirm `Field` matches the exact property name in the C# model (case-sensitive)
- If the field is custom (not in `GanttTaskFields`), ensure the property exists in the task class

**Format not applying**
- Confirm `Type` is set correctly — date formats require `ColumnType.Date` or `ColumnType.DateTime`
- Format strings apply to display only; check the model value is the correct C# type

**`AutoFitColumnsAsync` has no effect**
- Call it in the `DataBound` event, not `OnInitialized` — data must be loaded first
- `AllowResizing="true"` is required on `SfGantt` for the autofit gesture on double-click

**`ShowColumnsAsync` / `HideColumnsAsync` not working**
- Ensure the second parameter matches how you identify columns: `"Field"` or `"HeaderText"`
- Values are case-sensitive; match exactly as defined on the `GanttColumn`
