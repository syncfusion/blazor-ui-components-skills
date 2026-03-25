# Sorting Operations

## Table of Contents
- [Overview](#overview)
- [When to Read](#when-to-read)
- [Key Properties and Features](#key-properties-and-features)
- [Basic Sorting Setup](#basic-sorting-setup)
- [Initial Sorting (Predefined Sort)](#initial-sorting-predefined-sort)
- [Multi-Column Sorting](#multi-column-sorting)
- [Sort Direction](#sort-direction)
- [Programmatic Sorting](#programmatic-sorting)
- [Disable Sorting Per Column](#disable-sorting-per-column)
- [Sorting Events](#sorting-events)
- [Sorting Custom Columns](#sorting-custom-columns)
- [Hierarchical Sorting Behavior](#hierarchical-sorting-behavior)
- [Touch Interaction](#touch-interaction)
- [Best Practices](#best-practices)
- [Common Scenarios](#common-scenarios)
- [Troubleshooting](#troubleshooting)
- [Advanced: Server-Side Sorting](#advanced-server-side-sorting)

---

## Overview

The Syncfusion Blazor Gantt Chart component provides powerful sorting functionality to arrange task data in ascending or descending order based on column values. Sorting helps users organize and analyze data by task name, date, progress, or any custom field.

## When to Read
- You need to enable column-based sorting in your Gantt Chart
- You want to implement multi-column sorting with CTRL key
- You need to understand initial sort configuration
- You require programmatic sorting or custom sort logic
- You want to handle sorting events for custom behavior

## Key Properties and Features

### Sorting Properties
- `AllowSorting` — Enables sorting functionality (default: false)
- `GanttSortSettings` — Configure initial sort state
- `GanttSortDescriptors` — Collection of sort descriptors for multi-column sorting
- `GanttColumn.AllowSorting` — Enable/disable sorting per column

### Sort Descriptor Properties
- `Field` — Column field name to sort by
- `Direction` — Sort direction (Ascending or Descending)

### Methods
- `SortByColumnAsync(field, direction, isMultiSort)` — Sort specific column programmatically
- `ClearSortingAsync()` — Remove all applied sorting

### Events
- `Sorting` — Triggered before sorting is applied (cancellable)
- `Sorted` — Triggered after sorting is applied

## Basic Sorting Setup

Enable sorting by setting `AllowSorting="true"`:

```razor
@using Syncfusion.Blazor.Gantt

<SfGantt DataSource="@TaskCollection" Height="450px" Width="700px" AllowSorting="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" 
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
</SfGantt>

@code {
    private List<TaskData> TaskCollection { get; set; }

    protected override void OnInitialized()
    {
        TaskCollection = GetTaskCollection();
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

    public static List<TaskData> GetTaskCollection()
    {
        return new List<TaskData>()
        {
            new TaskData() { TaskID = 1, TaskName = "Project initiation", StartDate = new DateTime(2022, 04, 05), EndDate = new DateTime(2022, 04, 08) },
            new TaskData() { TaskID = 2, TaskName = "Identify Site location", StartDate = new DateTime(2022, 04, 05), Duration = "0", Progress = 30, ParentID = 1 },
            new TaskData() { TaskID = 3, TaskName = "Perform soil test", StartDate = new DateTime(2022, 04, 05), Duration = "4", Progress = 40, ParentID = 1 },
            new TaskData() { TaskID = 4, TaskName = "Soil test approval", StartDate = new DateTime(2022, 04, 05), Duration = "0", Progress = 30, ParentID = 1 },
            new TaskData() { TaskID = 5, TaskName = "Project estimation", StartDate = new DateTime(2022, 04, 06), EndDate = new DateTime(2022, 04, 08) },
            new TaskData() { TaskID = 6, TaskName = "Develop floor plan for estimation", StartDate = new DateTime(2022, 04, 06), Duration = "3", Progress = 30, ParentID = 5 }
        };
    }
}
```

**User Interaction:**
- Click column header to sort ascending
- Click again to toggle to descending
- Click a third time to remove sorting

## Initial Sorting (Predefined Sort)

Configure sorting during initial render using `GanttSortSettings`:

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Grids

<SfGantt DataSource="@TaskCollection" Height="450px" Width="700px" AllowSorting="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" 
                     Duration="Duration" Progress="Progress" ParentID="ParentID" />
    <GanttSortSettings>
        <GanttSortDescriptors>
            <GanttSortDescriptor Field="TaskID" Direction="SortDirection.Descending" />
            <GanttSortDescriptor Field="TaskName" Direction="SortDirection.Ascending" />
        </GanttSortDescriptors>
    </GanttSortSettings>
</SfGantt>
```

This example sorts by TaskID (descending) first, then by TaskName (ascending).

## Multi-Column Sorting

Users can sort by multiple columns by holding the **CTRL** key (Windows/Linux) or **CMD** key (macOS) while clicking column headers.

To remove a column from multi-sort, hold **SHIFT** and click the column header.

### Programmatic Multi-Column Sorting

```csharp
var sortDescriptors = new List<GanttSortDescriptor>
{
    new GanttSortDescriptor { Field = "Priority", Direction = SortDirection.Descending },
    new GanttSortDescriptor { Field = "StartDate", Direction = SortDirection.Ascending }
};
```

## Sort Direction

The `SortDirection` enum defines sorting order:

```csharp
public enum SortDirection
{
    Ascending,   // A → Z, 0 → 9, oldest → newest
    Descending   // Z → A, 9 → 0, newest → oldest
}
```

## Programmatic Sorting

### Sort by Column

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Buttons

<SfButton CssClass="e-primary" @onclick="Sorting" style="margin-bottom: 16px;">
    Sort TaskName Column
</SfButton>

<SfGantt @ref="GanttInstance" DataSource="@TaskCollection" Height="450px" Width="700px" AllowSorting="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
</SfGantt>

@code {
    public SfGantt<TaskData> GanttInstance;

    public void Sorting()
    {
        // Parameters: columnName, direction, isMultiSort
        GanttInstance.SortByColumnAsync("TaskName", Syncfusion.Blazor.Grids.SortDirection.Descending, false);
    }
}
```

**Method Parameters:**
- `columnName` — Field name of the column to sort
- `direction` — SortDirection.Ascending or SortDirection.Descending
- `isMultiSort` — true to add to existing sort, false to replace all sorts

### Clear All Sorting

```razor
@using Syncfusion.Blazor.Buttons

<SfButton CssClass="e-primary" @onclick="ClearSorting" style="margin-bottom: 16px;">
    Clear Sorting
</SfButton>

<SfGantt @ref="Gantt" DataSource="@TaskCollection" AllowSorting="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttSortSettings>
        <GanttSortDescriptors>
            <GanttSortDescriptor Field="TaskID" Direction="SortDirection.Descending" />
        </GanttSortDescriptors>
    </GanttSortSettings>
</SfGantt>

@code {
    public SfGantt<TaskData> Gantt;

    public void ClearSorting()
    {
        Gantt.ClearSortingAsync();
    }
}
```

## Disable Sorting Per Column

Prevent sorting on specific columns:

```razor
<SfGantt DataSource="@TaskCollection" AllowSorting="true">
    <GanttColumns>
        <GanttColumn Field="TaskID" HeaderText="Task ID" AllowSorting="false" />
        <GanttColumn Field="TaskName" HeaderText="Task Name" />
        <GanttColumn Field="StartDate" HeaderText="Start Date" />
    </GanttColumns>
</SfGantt>
```

When `AllowSorting="false"` is set on a column, users cannot sort by clicking that column's header.

## Sorting Events

### Sorting Event (Before Sort)

Triggered before sorting is applied. Can be cancelled:

```csharp
public void SortingHandler(SortingEventArgs args)
{
    if (args.ColumnName == "TaskID")
    {
        args.Cancel = true; // Cancel sorting for TaskID column
        Console.WriteLine($"Sorting on '{args.ColumnName}' is disabled.");
    }
}
```

### Sorted Event (After Sort)

Triggered after sorting is applied:

```csharp
public void SortedHandler(SortedEventArgs args)
{
    Console.WriteLine($"Sorted by '{args.ColumnName}' in '{args.Direction}' order.");
}
```

### Complete Example with Events

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Grids

<label style="color: red; display: block; margin-bottom: 12px; text-align: center;">
    @SortMessage
</label>

<SfGantt DataSource="@TaskCollection" Height="450px" Width="700px" AllowSorting="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttEvents Sorting="SortingHandler" Sorted="SortedHandler" TValue="TaskData" />
</SfGantt>

@code {
    private string SortMessage { get; set; }

    public void SortingHandler(SortingEventArgs args)
    {
        if (args.ColumnName == "TaskID")
        {
            args.Cancel = true;
            SortMessage = $"Sorting on '{args.ColumnName}' is disabled.";
        }
    }

    public void SortedHandler(SortedEventArgs args)
    {
        SortMessage = $"Sorted by '{args.ColumnName}' in '{args.Direction}' order.";
    }
}
```

## Sorting Custom Columns

Sort custom columns (non-standard task fields) by adding them to the column collection:

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Buttons

<SfButton CssClass="e-primary" @onclick="Sorting" style="margin-bottom: 16px;">
    Sort Custom Column
</SfButton>

<SfGantt @ref="GanttInstance" DataSource="@TaskCollection" Height="450px" Width="700px" AllowSorting="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttColumns>
        <GanttColumn Field="TaskID" Width="100" />
        <GanttColumn Field="TaskName" Width="250" />
        <GanttColumn Field="StartDate" />
        <GanttColumn Field="Duration" />
        <GanttColumn Field="Progress" />
        <GanttColumn Field="CustomColumn" HeaderText="Custom" />
    </GanttColumns>
</SfGantt>

@code {
    public SfGantt<TaskData> GanttInstance;

    public void Sorting()
    {
        GanttInstance.SortByColumnAsync("CustomColumn", Syncfusion.Blazor.Grids.SortDirection.Descending, false);
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
        public int CustomColumn { get; set; }
    }

    public static List<TaskData> GetTaskCollection()
    {
        return new List<TaskData>()
        {
            new TaskData() { TaskID = 1, TaskName = "Project initiation", StartDate = new DateTime(2022, 04, 05), CustomColumn = 2 },
            new TaskData() { TaskID = 2, TaskName = "Identify Site location", StartDate = new DateTime(2022, 04, 05), CustomColumn = 3, ParentID = 1 },
            new TaskData() { TaskID = 3, TaskName = "Perform soil test", StartDate = new DateTime(2022, 04, 05), CustomColumn = 4, ParentID = 1 }
        };
    }
}
```

## Hierarchical Sorting Behavior

**Important:** In hierarchical data, sorting respects the parent-child relationship:
- Parent tasks are sorted first
- Child tasks are sorted within their parent group
- The hierarchy structure is preserved

Example:
```
Before Sort:          After Sort (by TaskName):
- Project A           - Project A
  - Task Z              - Task A
  - Task A              - Task Z
- Project B           - Project B
  - Task Y              - Task X
  - Task X              - Task Y
```

## Touch Interaction

On touch-enabled devices:
- **Single Column Sort:** Tap column header to sort
- **Multi-Column Sort:** Tap column header → popup appears → tap popup → tap additional headers to add to multi-sort

![Multiple Sorting in Blazor Gantt Chart](images/blazor-gantt-chart-multiple-sorting.png)

## Best Practices

### Performance Tips
- **Large Datasets (100k+ records)**: Implement server-side sorting with `SfDataManager` and remote adaptors
- **Minimize Columns**: Reduce visible columns when sorting large datasets
- **Virtual Scrolling**: Combine with `EnableVirtualization` for better performance

### User Experience
- Set appropriate initial sort to show most relevant data first
- Use multi-column sort for complex data organization (e.g., sort by Priority, then by Date)
- Combine sorting with filtering for powerful data exploration
- Consider disabling sort on ID columns if they're auto-generated

### Development Tips
- Preserve sort state in browser storage to restore user preferences
- Use `Sorting` event to validate or customize sort behavior
- Handle custom data types with proper comparison logic
- Test sorting with null values to ensure graceful handling

## Common Scenarios

### Sort by Date (Newest First)
```razor
<GanttSortSettings>
    <GanttSortDescriptors>
        <GanttSortDescriptor Field="StartDate" Direction="SortDirection.Descending" />
    </GanttSortDescriptors>
</GanttSortSettings>
```

### Sort by Priority, Then by Date
```razor
<GanttSortSettings>
    <GanttSortDescriptors>
        <GanttSortDescriptor Field="Priority" Direction="SortDirection.Descending" />
        <GanttSortDescriptor Field="StartDate" Direction="SortDirection.Ascending" />
    </GanttSortDescriptors>
</GanttSortSettings>
```

### Sort Programmatically on Button Click
```csharp
public void SortByProgress()
{
    GanttInstance.SortByColumnAsync("Progress", SortDirection.Descending, false);
}
```

### Add to Existing Multi-Sort
```csharp
public void AddToSort()
{
    GanttInstance.SortByColumnAsync("Duration", SortDirection.Ascending, true); // isMultiSort = true
}
```

## Troubleshooting

**Issue**: Sorting not working for custom columns  
**Solution**: Ensure the column is added to `GanttColumns` collection and `Field` property matches data source property name

**Issue**: Multi-column sort not working  
**Solution**: Verify `AllowSorting="true"` is set on the Gantt component, not just individual columns

**Issue**: Sort order incorrect for string columns  
**Solution**: Check for leading/trailing spaces in data. Consider trimming values or implementing custom sort logic

**Issue**: Performance degradation when sorting  
**Solution**: For large datasets, implement server-side sorting using `SfDataManager` with `UrlAdaptor`

**Issue**: Hierarchy lost after sorting  
**Solution**: This is expected behavior. Sorting respects hierarchy by sorting within parent groups, not globally

## Advanced: Server-Side Sorting

For large datasets, implement server-side sorting:

```razor
<SfGantt TValue="TaskData" AllowSorting="true">
    <SfDataManager Url="api/tasks" Adaptor="Adaptors.WebApiAdaptor" />
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
</SfGantt>
```

Server controller:
```csharp
[HttpGet]
public IActionResult Get([FromQuery] DataManagerRequest dm)
{
    IEnumerable<TaskData> data = _taskRepository.GetAll();
    
    // Apply sorting
    if (dm.Sorted != null && dm.Sorted.Any())
    {
        data = PerformSorting(data, dm.Sorted);
    }
    
    return Json(new { result = data, count = data.Count() });
}
```
