# Filtering in Syncfusion Blazor TreeGrid

> **Namespace note:** `FilterType` belongs to `Syncfusion.Blazor.TreeGrid`, **not** `Syncfusion.Blazor.Grids`. When both namespaces are imported, always qualify it as `Syncfusion.Blazor.TreeGrid.FilterType.Menu` (or `.Excel`, `.FilterBar`) to avoid CS1503 type-mismatch errors.



## Table of Contents
- [Enable Filtering](#enable-filtering)
- [Filter Hierarchy Modes](#filter-hierarchy-modes)
- [Filter Bar (Default)](#filter-bar-default)
- [Filter Menu](#filter-menu)
- [Excel-Like Filter](#excel-like-filter)
- [Custom Filter Operators](#custom-filter-operators)
- [Programmatic Filtering](#programmatic-filtering)
- [Filter on Specific Columns Only](#filter-on-specific-columns-only)
- [Filter Events](#filter-events)
- [Initial Filter](#initial-filter)
- [Filter Operators](#filter-operators)
- [Wildcard and LIKE Operator Filter](#wildcard-and-like-operator-filter)
- [Filter Enum Column](#filter-enum-column)
- [Combining Filter Types](#combining-filter-types)

---

## Enable Filtering

```razor
<SfTreeGrid DataSource="@TreeData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            AllowFiltering="true"
            TreeColumnIndex="1">
    ...
</SfTreeGrid>
```

The default mode is **FilterBar** — an input row appears below the column headers.

---

## Filter Hierarchy Modes

TreeGrid supports four hierarchy modes via the `HierarchyMode` property of `TreeGridFilterSettings`. This controls which related records are shown alongside the filtered matches:

| Mode | Description |
|---|---|
| `Parent` | **(Default)** Filtered records are shown with their ancestor records. If a match has no parent, only the match is shown. |
| `Child` | Filtered records are shown with their descendant records. If a match has no children, only the match is shown. |
| `Both` | Filtered records are shown with both parent and child records. If a match has neither, only the match is shown. |
| `None` | Only the exact matching records are displayed — no parent or child rows are included. |

```razor
<SfTreeGrid AllowFiltering="true" ...>
    <TreeGridFilterSettings HierarchyMode="FilterHierarchyMode.Child" />
    ...
</SfTreeGrid>
```

**With a specific filter type:**
```razor
<SfTreeGrid AllowFiltering="true" ...>
    <TreeGridFilterSettings HierarchyMode="FilterHierarchyMode.None"
                            Type="Syncfusion.Blazor.TreeGrid.FilterType.Excel" />
    ...
</SfTreeGrid>
```

> `FilterHierarchyMode` is in the `Syncfusion.Blazor.TreeGrid` namespace.

---

## Filter Bar (Default)

Each column gets an inline text box where users type to filter. Setting `AllowFiltering` to `true` renders the filter bar row next to the header, allowing records to be filtered with different expressions depending on the column type.

**Filter bar expressions:**

Enter the following filter expressions (operators) manually in the filter bar:

| Expression | Example | Description | Column Type |
|---|---|---|---|
| = | =value | equal | Number |
| != | !=value | notequal | Number |
| > | >value | greaterthan | Number |
| < | <value | lessthan | Number |
| >= | >=value | greaterthanorequal | Number |
| <= | <=value | lessthanorequal | Number |
| * | *value | startswith | String |
| % | %value | endswith | String |
| N/A | N/A | **Equal** operator will always be used for date filter. | Date |
| N/A | N/A | **Equal** operator will always be used for Boolean filter. | Boolean |

**Filter bar modes:**

```razor
<SfTreeGrid AllowFiltering="true" ...>
    <TreeGridFilterSettings Mode="FilterBarMode.OnEnter" />
    ...
</SfTreeGrid>
```

| FilterBarMode | Behavior |
|---|---|
| `Immediate` | Filters as user types (default) |
| `OnEnter` | Filters only when Enter is pressed |

You can also set a delay for the `Immediate` mode using the `ImmediateModeDelay` property of `TreeGridFilterSettings`, which defines the time (in milliseconds) the TreeGrid waits before performing the filter operation:

```razor
<SfTreeGrid AllowFiltering="true" ...>
    <TreeGridFilterSettings Mode="FilterBarMode.Immediate" ImmediateModeDelay="300" />
    ...
</SfTreeGrid>
```

**Change default filter operator:**

You can change the default filter operator for a column by setting the `FilterSettings` property inline on the column. In the following sample, the default operator for `TaskName` is changed from `startswith` to `contains`:

```razor
<TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="100"
    FilterSettings="@(new FilterSettings{ Operator = Operator.Contains })">
</TreeGridColumn>
```

**⚠️ CRITICAL - FilterTemplate Context Access:**
- Use `<FilterTemplate>` as a **child element**, NOT a property
- Type-cast context as `PredicateModel`: `(context as PredicateModel)`
- The context represents the current filter row being rendered

✅ CORRECT - Component-Based SfDropDownList (RECOMMENDED):
```razor
<TreeGridColumn Field="Priority" HeaderText="Priority (Filter)">
    <FilterTemplate>
        <SfDropDownList TValue="string" TItem="DropdownDataModel"
                        DataSource="@PriorityDropdownData"
                        @bind-Value="@PriorityFilterValue">
            <DropDownListEvents TValue="string" TItem="DropdownDataModel" 
                               ValueChange="OnPriorityFilterChange" />
            <DropDownListFieldSettings Text="Text" Value="Value" />
        </SfDropDownList>
    </FilterTemplate>
</TreeGridColumn>

@code {
    private SfTreeGrid<TaskItem> TreeGridRef { get; set; }

    private List<DropdownDataModel> PriorityDropdownData = new()
    {
        new DropdownDataModel { Text = "High", Value = "High" },
        new DropdownDataModel { Text = "Medium", Value = "Medium" },
        new DropdownDataModel { Text = "Low", Value = "Low" },
        new DropdownDataModel { Text = "All", Value = "All" }
    };

    private string PriorityFilterValue = "All";

    public class DropdownDataModel
    {
        public string Text { get; set; }
        public string Value { get; set; }
    }

    // Event handler with proper async/await and ChangeEventArgs type parameters
    private async Task OnPriorityFilterChange(Syncfusion.Blazor.DropDowns.ChangeEventArgs<string, DropdownDataModel> args)
    {
        if (TreeGridRef != null)
        {
            if (args.Value == "All")
            {
                // "All" option clears the filter
                await TreeGridRef.ClearFilteringAsync();
            }
            else
            {
                // Apply column filter programmatically
                await TreeGridRef.FilterByColumnAsync("Priority", "equal", args.Value);
            }
        }
    }
}
```

❌ WRONG (Do NOT use FilterBarTemplate as a property):
```razor
<!-- ❌ WRONG -->
<!-- <TreeGridColumn Field="Status" FilterBarTemplate="@FilterTemplate" /> -->
```

**Key FilterTemplate Rules:**
1. Always use `@bind-Value` for two-way binding in SfDropDownList (not just `Value`)
2. Always include `DropDownListFieldSettings` to map Text and Value properties
3. Always include `DropDownListEvents` with `ValueChange` handler
4. Event handler signature MUST be `ChangeEventArgs<TValue, TItem>` where TValue is the bind type
5. Make event handlers `async Task` for programmatic filtering
6. Use `ClearFilteringAsync()` for "All" option to clear the filter
7. Use `FilterByColumnAsync(columnName, operator, value)` to apply filter programmatically

---

## Filter Menu

Each column header gets a filter icon that opens a popup with condition controls. The filter menu UI is rendered based on the column type, which allows filtering data with different operators:

```razor
<SfTreeGrid AllowFiltering="true" ...>
    <TreeGridFilterSettings HierarchyMode="@FilterHierarchyMode.Parent"
                            Type="Syncfusion.Blazor.TreeGrid.FilterType.Menu" />
    ...
</SfTreeGrid>
```

The filter menu lets users pick an operator (equals, contains, starts with, etc.) and value.

> * `AllowFiltering` must be set as `true` to enable the filter menu.
> * Setting the `AllowFiltering` property of `TreeGridColumn` to `false` will prevent filter menu rendering for that column.

**Set filter menu on specific columns only:**
```razor
@using Syncfusion.Blazor.TreeGrid
@using Syncfusion.Blazor.Grids

<SfTreeGrid DataSource="@TreeGridData" IdMapping="TaskId"
            ParentIdMapping="ParentId" TreeColumnIndex="1" AllowFiltering="true">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="Task ID" Width="80"
            TextAlign="TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration" Width="100"
            TextAlign="TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress" Width="100"
            TextAlign="TextAlign.Right" />
        <TreeGridColumn Field="Priority" HeaderText="Priority" Width="80"
            FilterSettings="@(new FilterSettings { Type = FilterType.Menu })" />
    </TreeGridColumns>
</SfTreeGrid>
```

> Use the `FilterSettings` **attribute** on `TreeGridColumn` (with `Syncfusion.Blazor.Grids.FilterSettings`) to set a per-column filter type. Do NOT use a child `<TreeGridFilterSettings>` element inside a column — that component is for grid-level settings only.

**Custom component in filter menu:**

The `FilterTemplate` property of a column is used to add custom filter components in the filter menu. You can type-cast the implicit `context` as `PredicateModel<T>` to get filter values inside the template. In the following sample, a dropdown is used as a custom component in the Duration column:

```razor
<SfTreeGrid @ref="TreeGrid" DataSource="@TreeGridData" IdMapping="TaskId"
            ParentIdMapping="ParentId" TreeColumnIndex="1" AllowFiltering="true">
    <TreeGridFilterSettings Type="Syncfusion.Blazor.TreeGrid.FilterType.Menu" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="Task ID" Width="80"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="100" />
        <TreeGridColumn Field="Duration" HeaderText="Duration" Width="100"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right">
            <FilterTemplate>
                <SfDropDownList TValue="string" DataSource="@DropDownData" TItem="string"
                    Value="@((string)(context as PredicateModel).Value)">
                    <DropDownListEvents ValueChange="change" TValue="string" TItem="string" />
                </SfDropDownList>
            </FilterTemplate>
        </TreeGridColumn>
    </TreeGridColumns>
</SfTreeGrid>

@code {
    SfTreeGrid<BusinessObject> TreeGrid;

    public List<string> DropDownData { get; set; } = new List<string>
    {
        "10", "50", "5", "6", "4", "All"
    };

    public void change(Syncfusion.Blazor.DropDowns.ChangeEventArgs<string, string> args)
    {
        if (args.Value == "All")
        {
            List<string> columns = new List<string> { "Duration" };
            TreeGrid.ClearFiltering(columns);
        }
        else
        {
            TreeGrid.FilterByColumn("Duration", "equal", args.Value);
        }
    }
}
```

**Override default filter operators for menu filtering:**

The default filter operators for a column can be overridden using the `FilterDialogOpening` event. In the following sample, the filter operators for the **Task Name** column are overridden:

```razor
<SfTreeGrid AllowFiltering="true" ...>
    <TreeGridEvents FilterDialogOpening="FilterDialogOpeningHandler" TValue="BusinessObject" />
    <TreeGridFilterSettings Type="Syncfusion.Blazor.TreeGrid.FilterType.Menu" />
    ...
</SfTreeGrid>

@code {
    public void FilterDialogOpeningHandler(FilterDialogOpeningEventArgs args)
    {
        if (args.ColumnName == "TaskName")
        {
            args.FilterOperators = new List<object>
            {
                new Operators() { Text = "Equal", Value = "equal" },
                new Operators() { Text = "Contains", Value = "contains" }
            };
        }
    }

    public class Operators
    {
        public string Value { get; set; }
        public string Text { get; set; }
    }
}
```

**Enable different filter type for a column:**

Both **Menu** and **Excel** filter types can be used in the same TreeGrid. Set the `Type` in the `FilterSettings` property of `TreeGridColumn`. In the following sample, the global filter type is Excel but the `TaskId` and `Priority` columns use Menu:

```razor
<SfTreeGrid AllowFiltering="true" ...>
    <TreeGridFilterSettings HierarchyMode="FilterHierarchyMode.None"
                            Type="Syncfusion.Blazor.TreeGrid.FilterType.Excel" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"
            FilterSettings="@(new Syncfusion.Blazor.Grids.FilterSettings{ Type = Syncfusion.Blazor.Grids.FilterType.Menu })"
            HeaderText="Task ID" Width="80"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="100" />
        <TreeGridColumn Field="Priority"
            FilterSettings="@(new Syncfusion.Blazor.Grids.FilterSettings{ Type = Syncfusion.Blazor.Grids.FilterType.Menu })"
            HeaderText="Priority" Width="100" />
    </TreeGridColumns>
</SfTreeGrid>
```

> **Limitations:** The different filter types such as Excel and Menu can be defined in different columns of the same TreeGrid. However, you cannot use these filter types along with the FilterBar type (the default). This is because FilterBar type requires UI-level changes incompatible with other filter types. For all non-FilterBar types, icons will be rendered in the column header.

---

## Excel-Like Filter

A checkbox list + search box filter UI, similar to Excel. The Excel menu contains options such as Sorting, Clear filter, and a sub-menu for advanced filtering:

```razor
<SfTreeGrid AllowFiltering="true" ...>
    <TreeGridFilterSettings HierarchyMode="FilterHierarchyMode.Parent"
                            Type="Syncfusion.Blazor.TreeGrid.FilterType.Excel" />
    ...
</SfTreeGrid>
```

Features of Excel filter:
- Checkbox list of distinct values
- Search box to narrow the list
- Custom filter conditions (AND / OR with two predicates)
- "Select All" / "Clear Filter" options

**Filter item template:**

The `FilterItemTemplate` property helps to customize each Excel filter list element for display purposes. Use the implicit named parameter `context` of type `FilterItemTemplateContext` to access list values inside the template:

```razor
<TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="100">
    <FilterItemTemplate>
        @{
            var filterContext = (context as FilterItemTemplateContext);
            var itemTemplateValue = "Textof(" + filterContext.Value + ")";
        }
        @itemTemplateValue
    </FilterItemTemplate>
</TreeGridColumn>
```

**Customize the height and width of filter popup:**

You can customize the height and width of each column's filter dialog using CSS style in the `FilterDialogOpening` event. Before the filter dialog opens for each column, the `FilterDialogOpening` event is triggered and you can apply dynamic CSS at that point:

```razor
<SfTreeGrid ID="TreeGrid" DataSource="@TreeGridData" IdMapping="TaskId"
            ParentIdMapping="ParentId" TreeColumnIndex="1" AllowFiltering="true">
    <TreeGridEvents FilterDialogOpening="FilterDialogOpeningHandler" TValue="BusinessObject" />
    <TreeGridFilterSettings HierarchyMode="FilterHierarchyMode.Parent"
                            Type="Syncfusion.Blazor.TreeGrid.FilterType.Excel" />
    ...
</SfTreeGrid>

@if (IsLarge)
{
    <style>
        #TreeGrid .e-excelfilter.e-popup.e-popup-open {
            height: 400px;
            width: 350px !important;
        }
    </style>
}
@if (IsSmall)
{
    <style>
        #TreeGrid .e-excelfilter.e-popup.e-popup-open {
            height: 450px;
            width: 280px !important;
        }
    </style>
}

@code {
    public bool IsLarge;
    public bool IsSmall;

    public void FilterDialogOpeningHandler(FilterDialogOpeningEventArgs args)
    {
        if (args.ColumnName == "TaskName")
        {
            IsLarge = true;
            IsSmall = false;
        }
        else if (args.ColumnName == "TaskId")
        {
            IsSmall = true;
            IsLarge = false;
        }
        else
        {
            IsLarge = false;
            IsSmall = false;
        }
    }
}
```

**Add current selection option to filter checkbox:**

By default, the Excel filter clears previously filtered values when filtering is applied again on the same column. To retain previous filter values when searching, the **Add current selection to filter** checkbox is displayed when data is searched in the search bar of the Excel filter dialog:

```razor
<SfTreeGrid ID="Grid" DataSource="@TreeGridData" AllowPaging="true" AllowFiltering="true"
            IdMapping="TaskId" ParentIdMapping="ParentId" TreeColumnIndex="1">
    <TreeGridPageSettings PageSize="2" />
    <TreeGridFilterSettings Type="Syncfusion.Blazor.TreeGrid.FilterType.Excel" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="Task ID" Width="80" IsPrimaryKey="true"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration" Width="100"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress" Width="100"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Priority" HeaderText="Priority" Width="100"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
    </TreeGridColumns>
</SfTreeGrid>
```

---

## Custom Filter Operators

Change the default filter operator per column using the `FilterSettings` attribute of `TreeGridColumn` with `Syncfusion.Blazor.Grids.FilterSettings`:

```razor
@using Syncfusion.Blazor.Grids;

<TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="100"
    FilterSettings="@(new FilterSettings{ Operator = Operator.Contains })">
</TreeGridColumn>
```

> Use the `FilterSettings` **attribute** on `TreeGridColumn` (with `Syncfusion.Blazor.Grids.FilterSettings`) to set a per-column operator. Do NOT use a child `<TreeGridFilterSettings>` element inside a column — that element is for grid-level filter settings only.

Available operators: `Equal`, `NotEqual`, `StartsWith`, `EndsWith`, `Contains`, `LessThan`, `GreaterThan`, `LessThanOrEqual`, `GreaterThanOrEqual`, `Between`, `IsNull`, `IsNotNull`

**Custom Filter UI with Built-In Operators & Dropdown:**
```razor
<TreeGridColumn Field="Status" HeaderText="Status" Width="120">
    <FilterTemplate>
        <SfDropDownList TValue="string" TItem="DropdownDataModel"
                        DataSource="@StatusDropdownData"
                        @bind-Value="@StatusFilterValue">
            <DropDownListEvents TValue="string" TItem="DropdownDataModel" 
                               ValueChange="OnStatusFilterChange" />
            <DropDownListFieldSettings Text="Text" Value="Value" />
        </SfDropDownList>
    </FilterTemplate>
</TreeGridColumn>

@code {
    private SfTreeGrid<TaskItem> TreeGridRef { get; set; }

    private List<DropdownDataModel> StatusDropdownData = new()
    {
        new DropdownDataModel { Text = "Open", Value = "Open" },
        new DropdownDataModel { Text = "In Progress", Value = "In Progress" },
        new DropdownDataModel { Text = "Closed", Value = "Closed" },
        new DropdownDataModel { Text = "All", Value = "All" }
    };

    private string StatusFilterValue = "All";

    public class DropdownDataModel
    {
        public string Text { get; set; }
        public string Value { get; set; }
    }

    private async Task OnStatusFilterChange(Syncfusion.Blazor.DropDowns.ChangeEventArgs<string, DropdownDataModel> args)
    {
        if (TreeGridRef != null)
        {
            if (args.Value == "All")
            {
                await TreeGridRef.ClearFilteringAsync();
            }
            else
            {
                // Use FilterByColumnAsync with "equal" operator
                await TreeGridRef.FilterByColumnAsync("Status", "equal", args.Value);
            }
        }
    }
}
```

---

## Programmatic Filtering

Filter columns from code:

```razor
<SfTreeGrid @ref="TreeRef" AllowFiltering="true" ...>
    ...
</SfTreeGrid>
<button @onclick="FilterByStatus">Show In Progress</button>
<button @onclick="ClearFilters">Clear Filters</button>

@code {
    private SfTreeGrid<TaskItem> TreeRef;

    private async Task FilterByStatus()
    {
        await TreeRef.FilterByColumnAsync("Status", "equal", "In Progress");
    }

    private async Task ClearFilters()
    {
        await TreeRef.ClearFilteringAsync();
        // Clear a specific column: await TreeRef.ClearFilteringAsync(new[] { "Status" });
    }
}
```

---

## Filter on Specific Columns Only

Disable filtering globally, then enable per column — or vice versa:

```razor
<!-- Disable filter for specific columns -->
<SfTreeGrid AllowFiltering="true" ...>
    <TreeGridColumns>
        <TreeGridColumn Field="Id"       AllowFiltering="false" Width="80" />
        <TreeGridColumn Field="TaskName" AllowFiltering="true"  Width="200" />
    </TreeGridColumns>
</SfTreeGrid>
```

---

## Filter Events

```razor
<SfTreeGrid AllowFiltering="true" ...>
    <TreeGridEvents TValue="TaskItem"
                    Filtering="OnFilteringHandler"
                    Filtered="OnFilteredHandler"
                    FilterDialogOpening="OnFilterDialogOpeningHandler"
                    FilterDialogOpened="OnFilterDialogOpenedHandler" />
    ...
</SfTreeGrid>

@code {
    /// <summary>
    /// Fires before filtering is applied (Cancellable)
    /// Property: args.ColumnName — name of the column being filtered
    /// </summary>
    private void OnFilteringHandler(FilteringEventArgs args)
    {
        // TODO: Implement custom filtering logic here
        // Example patterns:
        /*
        // Log filter action
        Console.WriteLine($"Filtering: {args.ColumnName}");
        
        // Prevent filtering on certain columns
        // if (args.ColumnName == "SensitiveData")
        // {
        //     args.Cancel = true;  // Prevent filter
        // }
        
        // Validate filter criteria
        // if (!IsValidFilterValue(args.ColumnName))
        // {
        //     args.Cancel = true;
        // }
        */
    }

    /// <summary>
    /// Fires after filtering is applied (Non-cancellable)
    /// Property: args.CurrentViewRecords — count of visible records after filter
    /// </summary>
    private void OnFilteredHandler(FilteredEventArgs args)
    {
        // TODO: Implement custom post-filter logic here
    }

    /// <summary>
    /// Fires before filter dialog opens (Cancellable)
    /// Property: args.ColumnName — name of the column whose filter dialog is opening
    /// </summary>
    private void OnFilterDialogOpeningHandler(FilterDialogOpeningEventArgs args)
    {
        // TODO: Implement custom logic before filter dialog opens
        // Example patterns:
        /*
        // Customize dialog content
        Console.WriteLine($"Filter dialog opening for column: {args.ColumnName}");
        
        // Disable filter for certain columns
        // if (args.ColumnName == "ProtectedField")
        // {
        //     args.Cancel = true;  // Prevent filter dialog
        // }
        
        // Load custom filter options
        // var filterOptions = await LoadFilterOptionsAsync(args.ColumnName);
        */
    }

    /// <summary>
    /// Fires after filter dialog is opened (Non-cancellable)
    /// Property: args.ColumnName — name of the column whose filter dialog is open
    /// </summary>
    private void OnFilterDialogOpenedHandler(FilterDialogOpenedEventArgs args)
    {
        // TODO: Implement custom logic after filter dialog opens
        // Example patterns:
        /*
        // Customize opened dialog
        Console.WriteLine($"Filter dialog opened for column: {args.ColumnName}");
        
        // Pre-populate filter values
        // SetDefaultFilterValue(args.ColumnName);
        
        // Apply custom styling or validation
        // ApplyCustomDialogStyling();
        */
    }
}
```


---

## Initial Filter

To apply a filter at initial rendering, set the filter `PredicateModel` in the `Columns` property of `TreeGridFilterSettings`:

```razor
<SfTreeGrid IdMapping="TaskId" DataSource="@TreeGridData" AllowFiltering="true"
            ParentIdMapping="ParentId" TreeColumnIndex="1">
    <TreeGridFilterSettings>
        <TreeGridFilterColumns>
            <TreeGridFilterColumn Field="TaskName" MatchCase="false"
                Operator="Syncfusion.Blazor.Operator.StartsWith"
                Predicate="and" Value="@Name" />
            <TreeGridFilterColumn Field="Duration" MatchCase="false"
                Operator="Syncfusion.Blazor.Operator.Equal"
                Predicate="and" Value="@Duration" />
        </TreeGridFilterColumns>
    </TreeGridFilterSettings>
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="Task ID" Width="80"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration" Width="100"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress" Width="100"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Priority" HeaderText="Priority" Width="80" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public string Name { get; set; } = "Child";
    public int Duration { get; set; } = 5;
}
```

---

## Filter Operators

The filter operator for a column can be defined in the `Operator` property of the `Columns` collection in `FilterSettings`. The available operators and their supported data types are:

| Operator | Description | Supported Types |
|---|---|---|
| startswith | Checks whether the value begins with the specified value. | String |
| doesnotstartswith | Checks whether the value does not begin with the specified value. | String |
| endswith | Checks whether the value ends with the specified value. | String |
| doesnotendswith | Checks whether the value does not end with the specified value. | String |
| contains | Checks whether the value contains the specified value. | String |
| doesnotcontains | Checks whether the value does not contain the specified value. | String |
| empty | Checks whether the value is empty. | String |
| notempty | Checks whether the value is not empty. | String |
| like | Processes single search patterns using the `%` symbol, retrieving values matching the specified patterns. | String |
| wildcard | Processes one or more search patterns using the `*` symbol, retrieving values matching the specified patterns. | String |
| equal | Checks whether the value is equal to the specified value. | String \| Number \| Boolean \| Date |
| notequal | Checks for values not equal to the specified value. | String \| Number \| Boolean \| Date |
| greaterthan | Checks whether the value is greater than the specified value. | Number \| Date |
| greaterthanorequal | Checks whether a value is greater than or equal to the specified value. | Number \| Date |
| lessthan | Checks whether the value is less than the specified value. | Number \| Date |
| lessthanorequal | Checks whether the value is less than or equal to the specified value. | Number \| Date |
| null | Checks whether the value is null. | Number \| Date |
| notnull | Checks whether the value is not null. | Number \| Date |

> By default, the `Operator` value is **equal**.

---

## Wildcard and LIKE Operator Filter

**Wildcard** and **LIKE** filter operators filter values based on a given string pattern and apply to string-type columns. They work slightly differently:

### Wildcard filtering

The **Wildcard** filter processes one or more search patterns using the `*` symbol, retrieving values that match the specified patterns. It is supported for all search options in the TreeGrid.

| Pattern | Description |
|---|---|
| a*b | Everything that starts with "a" and ends with "b". |
| a* | Everything that starts with "a". |
| *b | Everything that ends with "b". |
| *a* | Everything that has an "a" in it. |
| *a*b* | Everything that has an "a" in it, followed by anything, followed by a "b", followed by anything. |

> When using the **Wildcard** operator, records can be filtered using the filter `HierarchyMode` of `FilterSettings`. Filtered records are displayed according to the mode specified.

### LIKE filtering

The **LIKE** filter processes single search patterns using the `%` symbol. The following TreeGrid features support LIKE filtering on string-type columns:
- Filter Menu
- Custom Filter of Excel filter type

| Pattern | Description |
|---|---|
| %ab% | Returns all values that contain "ab". |
| ab% | Returns all values that end with "ab". |
| %ab | Returns all values that start with "ab". |

> When using the **LIKE** operator, records can be filtered using the filter `HierarchyMode` of `FilterSettings`. Filtered records are displayed according to the mode specified.

---

## Filter Enum Column

You can filter enum-type data in a TreeGrid column using the `FilterTemplate` feature. In the following sample, enumerated list data is bound to the `Priority` column, and an `SfDropDownList` is rendered in the `FilterTemplate`. In the `ValueChange` event of the dropdown, the column is filtered dynamically using `FilterByColumnAsync`:

```razor
<SfTreeGrid @ref="TreeGrid" DataSource="@TreeData" AllowFiltering="true"
            IdMapping="TaskID" ParentIdMapping="ParentID" TreeColumnIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskID" HeaderText="Task ID" Width="80"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="145" />
        <TreeGridColumn Field="Duration" HeaderText="Duration" Width="100"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress" Width="200" />
        <TreeGridColumn Field="Priority" HeaderText="Priority" Width="200">
            <FilterTemplate>
                <SfDropDownList Placeholder="Priority" ID="Priority"
                    Value="@((string)(context as PredicateModel).Value)"
                    DataSource="@FilterDropData" TValue="string" TItem="Data">
                    <DropDownListEvents TItem="Data" ValueChange="Change" TValue="string" />
                    <DropDownListFieldSettings Value="Priority" Text="Priority" />
                </SfDropDownList>
            </FilterTemplate>
        </TreeGridColumn>
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public SfTreeGrid<SelfReferenceData> TreeGrid;

    public class Data
    {
        public string Priority { get; set; }
    }

    List<Data> FilterDropData = new List<Data>
    {
        new Data() { Priority = "All" },
        new Data() { Priority = "High" },
        new Data() { Priority = "Low" },
        new Data() { Priority = "Critical" }
    };

    public async Task Change(Syncfusion.Blazor.DropDowns.ChangeEventArgs<string, Data> args)
    {
        if (args.Value == "All")
        {
            await this.TreeGrid.ClearFilteringAsync();
        }
        else
        {
            await this.TreeGrid.FilterByColumnAsync("Priority", "contains", args.Value);
        }
    }
}
```

---

## Combining Filter Types

Both **Menu** and **Excel** filter types can be used in the same TreeGrid. Set the global type on `TreeGridFilterSettings` and override per column using the `FilterSettings` property of `TreeGridColumn`.

> You cannot mix **FilterBar** (the default) with Menu or Excel types in the same TreeGrid. FilterBar requires UI-level changes that are incompatible with the other filter types.

---

## Key Notes

- Filtering applies to **all records** and the visible result depends on the active `HierarchyMode` (see [Filter Hierarchy Modes](#filter-hierarchy-modes)).
- The **default** `HierarchyMode` is `Parent` — matched records are shown together with their ancestor rows.
- Use `FilterHierarchyMode.None` when you want **only** the exact matching rows (no parents or children) to appear.
- For **remote data**, filter predicates are translated to query parameters by the adaptor.
- Combining FilterBar for most columns with Menu or Excel for specific columns is a common pattern.

---
