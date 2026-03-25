# Filtering and Search

## Table of Contents
- [Overview](#overview)
- [When to Read](#when-to-read) - [Key Properties and Features](#key-properties-and-features) - [Basic Filtering Setup](#basic-filtering-setup) - [Filter Types](#filter-types) - [Filter Hierarchy Modes](#filter-hierarchy-modes) - [Initial Filter](#initial-filter) - [Filter Operators](#filter-operators) - [Diacritic Filtering](#diacritic-filtering) - [Programmatic Filtering](#programmatic-filtering) - [Custom Filter Per Column](#custom-filter-per-column) - [Search Functionality](#search-functionality) - [Filter and Search Events](#filter-and-search-events) - [Filter vs Search Comparison](#filter-vs-search-comparison) - [Best Practices](#best-practices) - [Common Scenarios](#common-scenarios) - [Troubleshooting](#troubleshooting)

---

## Overview

The Syncfusion Blazor Gantt Chart component provides powerful filtering capabilities to display specific records based on defined criteria. It supports filter menu, Excel-like filtering, and toolbar search to help users quickly narrow down visible data in large datasets.

## When to Read
- You need to enable column-based filtering in your Gantt Chart
- You want to implement search functionality across task fields
- You need to understand filter hierarchy modes for parent-child relationships
- You require programmatic filtering or custom filter operators

## Key Properties and Features

### Filtering Properties
- `AllowFiltering` - Enables filtering functionality
- `GanttFilterSettings.Type` - Filter type: `Menu` or `Excel`
- `GanttFilterSettings.HierarchyMode` - Controls hierarchical filtering behavior
- `GanttFilterSettings.Columns` - Initial filter conditions using `PredicateModel`
- `GanttFilterSettings.Operators` - Custom filter operators per column
- `GanttFilterSettings.IgnoreAccent` - Enable/disable diacritic-sensitive filtering

### Search Properties
- `GanttSearchSettings.Fields` - Specific fields to search within
- `GanttSearchSettings.Operator` - Search operator (StartsWith, EndsWith, Contains, Equal)
- `GanttSearchSettings.Key` - Initial search text
- `GanttSearchSettings.IgnoreCase` - Case-sensitive/insensitive search
- `GanttSearchSettings.IgnoreAccent` - Diacritic-sensitive search

### Methods
- `FilterByColumnAsync(field, operator, value, predicate, matchCase, ignoreAccent)` - Apply filter to specific column
- `ClearFilteringAsync()` - Remove all applied filters
- `SearchAsync(searchKey)` - Perform search programmatically
- `GetFilteredRecordsAsync()` - Retrieve currently filtered records

### Events
- `Filtering` - Triggered before filter is applied (cancellable)
- `Filtered` - Triggered after filter is applied
- `FilterDialogOpening` - Triggered before filter dialog opens
- `FilterDialogOpened` - Triggered after filter dialog opens
- `Searching` - Triggered before search operation
- `Searched` - Triggered after search completes

## Basic Filtering Setup

Enable filtering by setting `AllowFiltering="true"`:

```razor
@using Syncfusion.Blazor.Gantt

<SfGantt DataSource="@TaskCollection" Width="900px" AllowFiltering="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" 
                     EndDate="EndDate" Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
</SfGantt>

@code {
    private List<TaskData> TaskCollection { get; set; }
    
    protected override void OnInitialized()
    {
        TaskCollection = GetTaskCollection();
    }

    private static List<TaskData> GetTaskCollection()
    {
        return new List<TaskData>()
        {
            new TaskData() { TaskID = 1, TaskName = "Project initiation", StartDate = new DateTime(2022, 01, 04), EndDate = new DateTime(2022, 01, 7) },
            new TaskData() { TaskID = 2, TaskName = "Identify Site location", StartDate = new DateTime(2022, 01, 04), Duration = "0", Progress = 30, ParentID = 1 },
            new TaskData() { TaskID = 3, TaskName = "Perform soil test", StartDate = new DateTime(2022, 01, 04), Duration = "4", Progress = 40, ParentID = 1 },
            new TaskData() { TaskID = 4, TaskName = "Soil test approval", StartDate = new DateTime(2022, 01, 04), Duration = "0", Progress = 30, ParentID = 1 },
            new TaskData() { TaskID = 5, TaskName = "Project estimation", StartDate = new DateTime(2022, 01, 04), EndDate = new DateTime(2022, 01, 10) },
            new TaskData() { TaskID = 6, TaskName = "Develop floor plan for estimation", StartDate = new DateTime(2022, 01, 06), Duration = "3", Progress = 30, ParentID = 5 }
        };
    }

    public class TaskData
    {
        public int TaskID { get; set; }
        public string TaskName { get; set; }
        public DateTime StartDate { get; set; }
        public DateTime EndDate { get; set; }
        public string Duration { get; set; }
        public int Progress { get; set; }
        public int? ParentID { get; set; }
    }
}
```

## Filter Types

The Gantt Chart supports two filter UI types:

| Type | Description | Use Case |
|------|-------------|----------|
| `Menu` | Column header dropdown with filter options | Simple, single-column filtering |
| `Excel` | Excel-like filter panel with advanced criteria | Complex multi-column filters with AND/OR logic |

```razor
<SfGantt AllowFiltering="true">
    <GanttFilterSettings Type="FilterType.Excel" />
</SfGantt>
```

## Filter Hierarchy Modes

Control how parent-child relationships are handled during filtering using `HierarchyMode`:

| Mode | Behavior |
|------|----------|
| `Parent` | Show parent tasks if any child matches the filter (default) |
| `Child` | Show child tasks if parent matches the filter |
| `Both` | Show both parent and child tasks that match |
| `None` | Show only the filtered rows without hierarchy context |

### Hierarchy Mode Example

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.DropDowns

<div style="padding-bottom:20px">
    <label style="font-weight:bold">Hierarchy Mode:</label>
    <SfDropDownList TValue="string" TItem="DropDownItem" Width="150px" 
                    DataSource="@filterModesData" @bind-Value="@SelectedMode">
        <DropDownListFieldSettings Text="mode" Value="id" />
        <DropDownListEvents TValue="string" TItem="DropDownItem" ValueChange="OnModeChange" />
    </SfDropDownList>
</div>

<SfGantt @ref="GanttObject" DataSource="@TaskCollection" Height="450px" Width="900px" AllowFiltering="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" 
                     Duration="Duration" Progress="Progress" ParentID="ParentID" />
    <GanttFilterSettings HierarchyMode="@HierarchyMode" />
</SfGantt>

@code {
    private SfGantt<TaskData> GanttObject;
    public string SelectedMode { get; set; } = "None";
    public FilterHierarchyMode HierarchyMode { get; set; } = FilterHierarchyMode.None;

    public List<DropDownItem> filterModesData = new List<DropDownItem>
    {
        new DropDownItem { id = "None", mode = "None" },
        new DropDownItem { id = "Parent", mode = "Parent" },
        new DropDownItem { id = "Child", mode = "Child" },
        new DropDownItem { id = "Both", mode = "Both" }
    };

    private void OnModeChange(ChangeEventArgs<string, DropDownItem> args)
    {
        HierarchyMode = Enum.Parse<FilterHierarchyMode>(args.Value);
        GanttObject.ClearFilteringAsync();
    }

    public class DropDownItem
    {
        public string id { get; set; }
        public string mode { get; set; }
    }
}
```

## Initial Filter (Predefined Filter)

Apply filters when the Gantt Chart initially loads using `GanttFilterSettings.Columns`:

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Grids

<SfGantt DataSource="@TaskCollection" Height="450px" Width="900px" AllowFiltering="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" 
                     Duration="Duration" Progress="Progress" ParentID="ParentID" />
    <GanttFilterSettings Columns="@FilterColumns" />
</SfGantt>

@code {
    public List<PredicateModel> FilterColumns { get; set; } = new List<PredicateModel>
    {
        new PredicateModel() { Field = "TaskName", MatchCase = false, Operator = Operator.StartsWith, Predicate = "and", Value = "Identify" },
        new PredicateModel() { Field = "TaskID", MatchCase = false, Operator = Operator.Equal, Predicate = "and", Value = 2 }
    };
}
```

## Filter Operators

Filter operators define the comparison logic:

| Operator | Description | Supported Types |
|----------|-------------|-----------------|
| `startswith` | Matches values beginning with specified text | String |
| `endswith` | Matches values ending with specified text | String |
| `contains` | Matches values containing specified text | String |
| `equal` | Matches exact value | String, Number, Boolean, Date |
| `notequal` | Does not match value | String, Number, Boolean, Date |
| `greaterthan` | Greater than value | Number, Date |
| `greaterthanorequal` | Greater than or equal to value | Number, Date |
| `lessthan` | Less than value | Number, Date |
| `lessthanorequal` | Less than or equal to value | Number, Date |

> Default operator is `equal`

## Diacritic Filtering

Enable diacritic-sensitive filtering by setting `IgnoreAccent="true"`:

```razor
<SfGantt DataSource="@TaskCollection" AllowFiltering="true">
    <GanttTaskFields Id="TaskId" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttFilterSettings IgnoreAccent="true" />
</SfGantt>
```

When enabled, filtering for "Project" will match "Project", "Project", etc.

## Programmatic Filtering

### Filter by Column

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Buttons

<SfButton @onclick="Filter">Filter Column</SfButton>

<SfGantt @ref="GanttInstance" DataSource="@TaskCollection" AllowFiltering="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
</SfGantt>

@code {
    public SfGantt<TaskData> GanttInstance;

    public async void Filter()
    {
        await GanttInstance.FilterByColumnAsync("TaskName", "startswith", "Iden", "or", true, false);
    }
}
```

### Clear All Filters

```razor
<SfButton @onclick="ClearFilter">Clear Filters</SfButton>

@code {
    public async Task ClearFilter()
    {
        await GanttInstance.ClearFilteringAsync();
    }
}
```

### Get Filtered Records

```razor
<SfButton @onclick="GetRecords">Get Filtered Records</SfButton>

@code {
    public async Task GetRecords()
    {
        var filteredRecords = await GanttInstance.GetFilteredRecordsAsync();
        FilteredTasks = filteredRecords.Cast<TaskData>().ToList();
    }
}
```

## Custom Filter Per Column

Set different filter types for individual columns:

```razor
<SfGantt AllowFiltering="true">
    <GanttColumns>
        <GanttColumn Field="TaskID" HeaderText="Task ID" 
                     FilterSettings="@(new FilterSettings { Type = Syncfusion.Blazor.Grids.FilterType.Menu })" />
        <GanttColumn Field="TaskName" HeaderText="Task Name" 
                     FilterSettings="@(new FilterSettings { Type = Syncfusion.Blazor.Grids.FilterType.Excel })" />
    </GanttColumns>
</SfGantt>
```

## Search Functionality

### Enable Search in Toolbar

```razor
<SfGantt DataSource="@TaskCollection" Toolbar="@(new List<string>() { "Search" })">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
</SfGantt>
```

### Initial Search Settings

```razor
<SfGantt DataSource="@TaskCollection" Toolbar="@(new List<string>() { "Search" })">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttSearchSettings Fields="@(new string[] { "TaskName" })" 
                         Operator="Operator.Contains" 
                         Key="Project" 
                         IgnoreCase="true" 
                         IgnoreAccent="true" />
</SfGantt>
```

### Search Operators

| Operator | Description |
|----------|-------------|
| `startsWith` | Matches values beginning with text |
| `endsWith` | Matches values ending with text |
| `contains` | Matches values containing text (default) |
| `equal` | Matches exact text |
| `notEqual` | Does not match text |

### Programmatic Search

```razor
@using Syncfusion.Blazor.Inputs
@using Syncfusion.Blazor.Buttons

<div style="display: flex; gap: 10px;">
    <SfTextBox @bind-Value="SearchText" Placeholder="Search text" Width="200px" />
    <SfButton @onclick="Search">Search</SfButton>
</div>

<SfGantt @ref="GanttInstance" DataSource="@TaskCollection" AllowFiltering="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
</SfGantt>

@code {
    private SfGantt<TaskData> GanttInstance;
    private string SearchText { get; set; } = string.Empty;

    private void Search()
    {
        GanttInstance?.SearchAsync(SearchText);
    }
}
```

### Clear Search

```razor
<SfButton @onclick="ClearSearch">Clear Search</SfButton>

@code {
    public void ClearSearch()
    {
        GanttInstance?.SearchAsync("");
    }
}
```

### Search Specific Columns

```razor
<SfGantt DataSource="@TaskCollection" Toolbar="@(new List<string>() { "Search" })">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate" />
    <GanttSearchSettings Fields="@(new string[] { "TaskName", "Duration" })" />
</SfGantt>
```

## Filter and Search Events

### Filtering Events

```csharp
public void FilteringHandler(Syncfusion.Blazor.Grids.FilteringEventArgs args)
{
    // Before filter is applied
    if (args.ColumnName == "TaskName")
    {
        args.Cancel = true; // Cancel filtering for TaskName column
    }
}

public void FilteredHandler(FilteredEventArgs args)
{
    // After filter is applied
    Console.WriteLine($"Filtering completed for: {args.ColumnName}");
}

public void FilterDialogOpeningHandler(FilterDialogOpeningEventArgs args)
{
    // Before filter dialog opens
    Console.WriteLine($"Filter dialog opening for: {args.ColumnName}");
}

public void FilterDialogOpenedHandler(FilterDialogOpenedEventArgs args)
{
    // After filter dialog opens
}
```

### Search Events

```csharp
public void SearchingHandler(SearchingEventArgs args)
{
    // Before search operation
    Console.WriteLine($"Searching for: {args.SearchKey}");
}

public void SearchedHandler(SearchedEventArgs args)
{
    // After search completes
}
```

## Filter vs Search Comparison

| Feature | Filtering | Searching |
|---------|-----------|-----------|
| **Scope** | Single or multiple columns with AND/OR logic | All columns (or specified fields) |
| **UI** | Column headers or filter dialog | Toolbar search box |
| **Operators** | Multiple (equals, contains, greater than, etc.) | Text matching only |
| **Precision** | Column-specific with data type awareness | General text search |
| **Hierarchy** | Respects HierarchyMode settings | Searches all visible records |

## Best Practices

### Performance Tips
- **Large Datasets (100k+ records)**: Implement server-side filtering with `SfDataManager` and remote adaptors
- **Debouncing**: For search boxes, add debounce logic to avoid excessive operations
- **Virtual Scrolling**: Combine with `EnableVirtualization` for better performance with filters

### User Experience
- Use `FilterType.Menu` for simple, user-friendly filtering
- Use `FilterType.Excel` for power users needing complex criteria
- Combine search and filter: search narrows results, then apply column filters
- Set appropriate `HierarchyMode` based on your data structure

### Development Tips
- Always set `AllowFiltering="true"` when using filtering features
- Validate filter predicates before applying programmatically
- Handle filter events to implement custom validation logic
- Use `GetFilteredRecordsAsync()` for export or further processing

## Common Scenarios

### Filter by Date Range
```csharp
var filterColumns = new List<PredicateModel>
{
    new PredicateModel() { Field = "StartDate", Operator = Operator.GreaterThanOrEqual, Value = new DateTime(2022, 01, 01) },
    new PredicateModel() { Field = "StartDate", Operator = Operator.LessThanOrEqual, Value = new DateTime(2022, 12, 31), Predicate = "and" }
};
```

### Filter with Multiple Conditions
```csharp
var filterColumns = new List<PredicateModel>
{
    new PredicateModel() { Field = "Progress", Operator = Operator.GreaterThan, Value = 50 },
    new PredicateModel() { Field = "Duration", Operator = Operator.LessThan, Value = 10, Predicate = "and" }
};
```

### Search with Case Sensitivity
```razor
<GanttSearchSettings Key="project" IgnoreCase="false" />
```

## Troubleshooting

**Issue**: Filter not working for custom columns  
**Solution**: Ensure the column `Field` property matches the data source property name exactly

**Issue**: Hierarchy not showing correctly after filter  
**Solution**: Verify `HierarchyMode` is set appropriately (try `Parent` or `Both`)

**Issue**: Search not finding records  
**Solution**: Check if `Fields` property is set correctly in `GanttSearchSettings`

**Issue**: Performance degradation with filters  
**Solution**: Implement server-side filtering using `SfDataManager` with `UrlAdaptor`
