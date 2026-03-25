# Column Interactions

## Table of Contents
- [When to Use](#when-to-use)
- [Column Reordering](#column-reordering)
- [Column Resizing](#column-resizing)
- [Frozen Columns](#frozen-columns)
- [Freeze Specific Columns](#freeze-specific-columns)
- [Freeze Direction Left or Right](#freeze-direction-left-or-right)
- [Draggable Freeze Line](#draggable-freeze-line)
- [Column Menu](#column-menu)
- [Disable Column Menu for Specific Column](#disable-column-menu-for-specific-column)
- [Column Chooser](#column-chooser)
- [Key Properties and APIs](#key-properties-and-apis)
- [Common Scenarios and Decisions](#common-scenarios-and-decisions)
- [Troubleshooting](#troubleshooting)

---

## When to Use

Guide the user to this reference when they need to:
- Allow users to drag columns to reorder them
- Allow users to resize columns by dragging the column edge
- Keep certain columns visible while scrolling horizontally (frozen columns)
- Show a column menu (sort asc/desc, autofit, column chooser, filter) on column headers
- Let users show/hide columns via a column chooser panel

---

## Column Reordering

When the user wants to allow dragging column headers to change their order, set `AllowReordering="true"` on `SfGantt`. Individual columns can opt out by setting `AllowReordering="false"` on a specific `GanttColumn`.

```razor
<SfGantt DataSource="@TaskCollection" Height="450px" Width="700px" AllowReordering="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttColumns>
        <GanttColumn Field="TaskID" Width="100"></GanttColumn>
        <GanttColumn Field="TaskName" Width="250" AllowReordering="false"></GanttColumn>
        <GanttColumn Field="StartDate" Width="150"></GanttColumn>
        <GanttColumn Field="Duration" Width="120"></GanttColumn>
        <GanttColumn Field="Progress" Width="120"></GanttColumn>
    </GanttColumns>
</SfGantt>
```

---

## Column Resizing

When the user wants to allow resizing columns by dragging the right edge of the column header, set `AllowResizing="true"` on `SfGantt`. Use `MinWidth` and `MaxWidth` on `GanttColumn` to constrain resize boundaries.

```razor
<SfGantt DataSource="@TaskCollection" Height="450px" Width="700px" AllowResizing="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttColumns>
        <GanttColumn Field="TaskID" Width="100"></GanttColumn>
        <GanttColumn Field="TaskName" Width="250" MinWidth="100" MaxWidth="400"></GanttColumn>
        <GanttColumn Field="Duration" Width="120" AllowResizing="false"></GanttColumn>
        <GanttColumn Field="Progress" Width="120"></GanttColumn>
    </GanttColumns>
</SfGantt>
```

> Double-clicking the column resizer handle auto-fits the column width to content. `AllowResizing` must be `true` for this to work.

---

## Frozen Columns

When the user wants the first N columns to stay fixed while scrolling horizontally, set `FrozenColumns` to the number of columns to freeze. Use `GanttSplitterSettings` to give the grid section enough space for scrolling.

```razor
<SfGantt DataSource="@TaskCollection" Height="450px" Width="700px" FrozenColumns="2">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttSplitterSettings Position="70%"></GanttSplitterSettings>
</SfGantt>

@code {
    private List<TaskData> TaskCollection { get; set; }
    protected override void OnInitialized()
    {
        TaskCollection = new List<TaskData>
        {
            new TaskData { TaskID = 1, TaskName = "Project initiation", StartDate = new DateTime(2022, 04, 05), EndDate = new DateTime(2022, 04, 21) },
            new TaskData { TaskID = 2, TaskName = "Identify site location", StartDate = new DateTime(2022, 04, 05), Duration = "0", Progress = 30, ParentID = 1 },
            new TaskData { TaskID = 3, TaskName = "Perform soil test", StartDate = new DateTime(2022, 04, 05), Duration = "4", Progress = 40, ParentID = 1 },
            new TaskData { TaskID = 4, TaskName = "Project estimation", StartDate = new DateTime(2022, 04, 06), EndDate = new DateTime(2022, 04, 21) },
            new TaskData { TaskID = 5, TaskName = "Develop floor plan", StartDate = new DateTime(2022, 04, 06), Duration = "3", Progress = 30, ParentID = 4 }
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
        public int? ParentID { get; set; }
    }
}
```

> `FrozenColumns` and `IsFrozen` (per-column) cannot be used together — they are incompatible.

---

## Freeze Specific Columns

When the user wants to freeze individual columns (not just the first N), set `IsFrozen="true"` on the specific `GanttColumn` entries instead of using `FrozenColumns` on `SfGantt`.

```razor
<SfGantt DataSource="@TaskCollection" Height="450px" Width="700px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttColumns>
        <GanttColumn Field="TaskID" IsFrozen="true"></GanttColumn>
        <GanttColumn Field="TaskName" IsFrozen="true"></GanttColumn>
        <GanttColumn Field="StartDate"></GanttColumn>
        <GanttColumn Field="EndDate"></GanttColumn>
        <GanttColumn Field="Duration"></GanttColumn>
        <GanttColumn Field="Progress"></GanttColumn>
    </GanttColumns>
    <GanttSplitterSettings Position="70%"></GanttSplitterSettings>
</SfGantt>
```

---

## Freeze Direction Left or Right

When the user wants to freeze a column on the right side of the grid (not just left), set both `IsFrozen="true"` and `Freeze="FreezeDirection.Right"` on the `GanttColumn`.

```razor
<SfGantt DataSource="@TaskCollection" Height="450px" Width="700px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttColumns>
        <GanttColumn Field="TaskID" IsFrozen="true"
                     Freeze="Syncfusion.Blazor.Grids.FreezeDirection.Left"></GanttColumn>
        <GanttColumn Field="TaskName" IsFrozen="true"
                     Freeze="Syncfusion.Blazor.Grids.FreezeDirection.Right"></GanttColumn>
        <GanttColumn Field="StartDate"></GanttColumn>
        <GanttColumn Field="EndDate"></GanttColumn>
        <GanttColumn Field="Duration"></GanttColumn>
        <GanttColumn Field="Progress"></GanttColumn>
    </GanttColumns>
    <GanttSplitterSettings Position="50%"></GanttSplitterSettings>
</SfGantt>
```

---

## Draggable Freeze Line

When the user wants end users to drag the freeze separator to interactively add or remove frozen columns, set `AllowFreezeLineMoving="true"` on `SfGantt`.

```razor
<SfGantt DataSource="@TaskCollection" Height="450px" Width="700px"
         AllowFreezeLineMoving="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttColumns>
        <GanttColumn Field="TaskID" IsFrozen="true"
                     Freeze="Syncfusion.Blazor.Grids.FreezeDirection.Left"></GanttColumn>
        <GanttColumn Field="TaskName" IsFrozen="true"
                     Freeze="Syncfusion.Blazor.Grids.FreezeDirection.Right"></GanttColumn>
        <GanttColumn Field="StartDate"></GanttColumn>
        <GanttColumn Field="Duration"></GanttColumn>
        <GanttColumn Field="Progress"></GanttColumn>
    </GanttColumns>
    <GanttSplitterSettings Position="70%"></GanttSplitterSettings>
</SfGantt>
```

---

## Column Menu

When the user wants a menu icon on column headers that gives quick access to sort ascending/descending, autofit, column chooser, and filter, set `ShowColumnMenu="true"` on `SfGantt`. Also enable the required companion properties for each menu action to be active.

**Built-in column menu items:**

| Menu Item | Requires |
|---|---|
| Sort Ascending | `AllowSorting="true"` |
| Sort Descending | `AllowSorting="true"` |
| Auto Fit | `AllowResizing="true"` |
| Auto Fit All | `AllowResizing="true"` |
| Column Chooser | `ShowColumnChooser="true"` |
| Filter | `AllowFiltering="true"` |

```razor
<SfGantt DataSource="@TaskCollection" Height="450px" Width="700px"
         AllowResizing="true" ShowColumnMenu="true"
         AllowFiltering="true" AllowSorting="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
</SfGantt>

@code {
    private List<TaskData> TaskCollection { get; set; }
    protected override void OnInitialized()
    {
        TaskCollection = new List<TaskData>
        {
            new TaskData { TaskID = 1, TaskName = "Project initiation", StartDate = new DateTime(2022, 04, 05), EndDate = new DateTime(2022, 04, 21) },
            new TaskData { TaskID = 2, TaskName = "Identify site location", StartDate = new DateTime(2022, 04, 05), Duration = "0", Progress = 30, ParentID = 1 },
            new TaskData { TaskID = 3, TaskName = "Perform soil test", StartDate = new DateTime(2022, 04, 05), Duration = "4", Progress = 40, ParentID = 1 },
            new TaskData { TaskID = 4, TaskName = "Project estimation", StartDate = new DateTime(2022, 04, 06), EndDate = new DateTime(2022, 04, 21) },
            new TaskData { TaskID = 5, TaskName = "Develop floor plan", StartDate = new DateTime(2022, 04, 06), Duration = "3", Progress = 30, ParentID = 4 }
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
        public int? ParentID { get; set; }
    }
}
```

---

## Disable Column Menu for Specific Column

When the user wants the column menu to appear on most columns but not on a specific one, set `ShowColumnMenu="false"` on that `GanttColumn`.

```razor
<SfGantt DataSource="@TaskCollection" Height="450px" Width="700px"
         ShowColumnMenu="true" AllowSorting="true" AllowFiltering="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttColumns>
        <GanttColumn Field="TaskID" Width="100" ShowColumnMenu="false"></GanttColumn>
        <GanttColumn Field="TaskName" Width="250"></GanttColumn>
        <GanttColumn Field="StartDate" Width="150"></GanttColumn>
        <GanttColumn Field="Duration" Width="120"></GanttColumn>
        <GanttColumn Field="Progress" Width="120"></GanttColumn>
    </GanttColumns>
</SfGantt>
```

---

## Column Chooser

When the user wants a UI panel that lets end users toggle column visibility, set `ShowColumnChooser="true"` on `SfGantt`. Add a `ColumnChooser` toolbar item to expose it via the toolbar.

```razor
<SfGantt DataSource="@TaskCollection" Height="450px" Width="700px"
         ShowColumnChooser="true" Toolbar="@(new List<Object>() { "ColumnChooser" })">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
</SfGantt>
```

---

## Key Properties and APIs

| Property / Method | Where Used | Purpose |
|---|---|---|
| `AllowReordering` | `SfGantt` / `GanttColumn` | Enables column drag-to-reorder |
| `AllowResizing` | `SfGantt` / `GanttColumn` | Enables column resize via edge drag |
| `MinWidth` / `MaxWidth` | `GanttColumn` | Constrains resize boundaries |
| `FrozenColumns` | `SfGantt` | Freezes the first N columns |
| `IsFrozen` | `GanttColumn` | Freezes a specific column (use instead of `FrozenColumns`) |
| `Freeze` | `GanttColumn` | `FreezeDirection.Left` or `FreezeDirection.Right` — requires `IsFrozen="true"` |
| `AllowFreezeLineMoving` | `SfGantt` | Draggable separator to interactively change frozen columns |
| `ShowColumnMenu` | `SfGantt` / `GanttColumn` | Enables header column menu (global or per-column off) |
| `ShowColumnChooser` | `SfGantt` | Shows column chooser panel |
| `AllowSorting` | `SfGantt` | Required for Sort Ascending/Descending menu items to work |
| `AllowFiltering` | `SfGantt` | Required for Filter menu item to work |

---

## Common Scenarios and Decisions

**User wants to let users drag columns to reorder them**
→ Set `AllowReordering="true"` on `SfGantt`. Add `AllowReordering="false"` to locked columns.

**User wants to let users resize columns**
→ Set `AllowResizing="true"` on `SfGantt`. Use `MinWidth`/`MaxWidth` to constrain individual columns.

**User wants the first 2 columns to stay fixed when scrolling**
→ Set `FrozenColumns="2"` on `SfGantt`. Do not combine with `IsFrozen` on columns.

**User wants to freeze a column on the right side**
→ Set `IsFrozen="true"` and `Freeze="FreezeDirection.Right"` on the `GanttColumn`.

**User wants a sort/filter/autofit menu on column headers**
→ Set `ShowColumnMenu="true"` and also enable `AllowSorting`, `AllowFiltering`, `AllowResizing` as needed.

**User wants to hide the column menu on the ID column only**
→ Set `ShowColumnMenu="false"` on that specific `GanttColumn`.

**User wants users to toggle column visibility through a UI**
→ Set `ShowColumnChooser="true"` and add `"ColumnChooser"` to the `Toolbar` list.

**User wants users to drag the freeze line to change which columns are frozen**
→ Set `AllowFreezeLineMoving="true"` on `SfGantt`.

---

## Troubleshooting

**Column menu icon does not appear**
- Confirm `ShowColumnMenu="true"` is set on `SfGantt` (not on a child component)
- The icon appears on hover over the column header — check that the column has enough width to display it

**Sort Ascending/Descending menu items are grayed out**
- `AllowSorting="true"` must be set on `SfGantt` for these items to be active

**Filter menu item is grayed out**
- `AllowFiltering="true"` must be set on `SfGantt`

**AutoFit / AutoFit All menu items do nothing**
- `AllowResizing="true"` must be set on `SfGantt`

**Frozen columns and `IsFrozen` conflict**
- Do not use `FrozenColumns` on `SfGantt` and `IsFrozen` on `GanttColumn` at the same time — pick one approach

**Right-to-left (RTL) layout broken with frozen columns**
- Frozen columns do not support RTL mode — remove frozen columns when RTL is enabled
