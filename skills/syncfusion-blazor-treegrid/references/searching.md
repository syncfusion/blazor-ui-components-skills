# Searching in Syncfusion Blazor TreeGrid

> **Required namespace:** Include `@using Syncfusion.Blazor.Grids;` for TreeGridSearchSettings and search events.

For programmatic search with `SfButton`, also include:

`@using Syncfusion.Blazor.Buttons`

The TreeGrid toolbar provides a built-in Search box that filters visible records across all (or specified) columns in real time.

## Table of Contents
- [Enable Toolbar Search](#enable-toolbar-search)
- [Search Settings](#search-settings)
- [Search Operators](#search-operators)
- [Search Hierarchy Modes](#search-hierarchy-modes)
- [Initial Search Text](#initial-search-text)
- [Programmatic Search](#programmatic-search)
- [Search Specific Columns](#search-specific-columns)
- [Searching with Case Sensitivity](#searching-with-case-sensitivity)
- [Searching with Ignore Accent](#searching-with-ignore-accent)
- [Search Events](#search-events)
- [Key Notes](#key-notes)

---

## Enable Toolbar Search

Add `"Search"` to the Toolbar list:

```razor
<SfTreeGrid DataSource="@TreeData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            TreeColumnIndex="1"
            Toolbar="@(new List<string> { "Search" })">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="ID"        Width="80" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="200" />
        <TreeGridColumn Field="Priority" HeaderText="Priority"  Width="120" />
    </TreeGridColumns>
</SfTreeGrid>
```

---

## Search Settings

Configure search behavior with `TreeGridSearchSettings`:

```razor
<SfTreeGrid Toolbar="@(new List<string> { "Search" })" ...>
    <TreeGridSearchSettings Fields="@SearchFields"
                            Operator="Syncfusion.Blazor.Grids.Operator.Contains"
                            IgnoreCase="true"
                            IgnoreAccent="true"
                            HierarchyMode="FilterHierarchyMode.Parent" />
    ...
</SfTreeGrid>

@code {
    // Limit search to specific columns only
    private string[] SearchFields = new[] { "TaskName", "Priority" };
}
```

| Property | Type | Description |
|---|---|---|
| `Fields` | string[] | Columns to search (default: all searchable columns) |
| `Operator` | Operator | `Contains` (default), `StartsWith`, `EndsWith`, `Equal` |
| `IgnoreCase` | bool | Case-insensitive search (default: true) |
| `IgnoreAccent` | bool | Ignore accent characters (é = e) |
| `Key` | string | Initial search text |
| `HierarchyMode` | FilterHierarchyMode | Hierarchy filtering mode: `Parent` (default — shows parent rows), `Child` (shows child rows), `Both` (shows parent and child), `None` (exact matches only) |

---

## Search Operators

The search operator can be defined in the `Operator` property of `TreeGridSearchSettings` to configure specific searching behavior.

The following operators are supported:

| Operator   | Description                                      |
|------------|--------------------------------------------------|
| startsWith | Checks if a value begins with the specified text |
| endsWith   | Checks if a value ends with the specified text   |
| contains   | Checks if a value contains the specified text    |
| equal      | Checks if a value is equal to the specified text |
| notEqual   | Checks if a value is not equal to the text       |

> The default operator is **contains**.

```razor
<SfTreeGrid Toolbar="@(new List<string> { "Search" })" ...>
    <TreeGridSearchSettings Operator="Syncfusion.Blazor.Grids.Operator.StartsWith" />
    ...
</SfTreeGrid>
```

---

## Search Hierarchy Modes

TreeGrid supports four hierarchy modes via the `HierarchyMode` property of `TreeGridSearchSettings`. This controls which related records are shown alongside the search matches:

| Mode | Description |
|---|---|
| `Parent` | **(Default)** Search results are shown with their ancestor records. If a match has no parent, only the match is shown. |
| `Child` | Search results are shown with their descendant records. If a match has no children, only the match is shown. |
| `Both` | Search results are shown with both parent and child records. If a match has neither, only the match is shown. |
| `None` | Only the exact matching records are displayed — no parent or child rows are included. |

```razor
<SfTreeGrid Toolbar="@(new List<string> { "Search" })" ...>
    <TreeGridSearchSettings Fields="@SearchFields"
                            HierarchyMode="FilterHierarchyMode.Child" />
    ...
</SfTreeGrid>
```

> `FilterHierarchyMode` is in the `Syncfusion.Blazor.TreeGrid` namespace.

---

## Initial Search Text

Pre-populate the search box on load:

```razor
<TreeGridSearchSettings Key="Phase 1" />
```

---

## Programmatic Search

Trigger or clear search from code:

```razor
<SfTreeGrid @ref="TreeRef" Toolbar="@(new List<string> { "Search" })" ...>
    ...
</SfTreeGrid>
<input @bind="SearchText" placeholder="Type to search..." />
<button @onclick="DoSearch">Search</button>
<button @onclick="ClearSearch">Clear</button>

@code {
    private SfTreeGrid<TaskItem> TreeRef;
    private string SearchText = "";

    private async Task DoSearch()
    {
        await TreeRef.SearchAsync(SearchText);
    }

    private async Task ClearSearch()
    {
        await TreeRef.SearchAsync("");
    }
}
```

---

## Search Specific Columns

By default, TreeGrid searches all visible columns. To restrict the search to specific columns, define the desired field names in the `Fields` property of `TreeGridSearchSettings`:

```razor
<SfTreeGrid IdMapping="TaskId" 
            DataSource="@TreeGridData" 
            ParentIdMapping="ParentId" 
            TreeColumnIndex="1"
            Toolbar="@(new List<string> { "Search" })">
    <TreeGridSearchSettings Fields="@SpecificCols" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="Task ID" Width="80" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration" Width="100" />
        <TreeGridColumn Field="Progress" HeaderText="Progress" Width="100" />
        <TreeGridColumn Field="Priority" HeaderText="Priority" Width="80" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    private string[] SpecificCols = new[] { "TaskId", "Duration" };
    public List<BusinessObject> TreeGridData { get; set; }
    
    protected override void OnInitialized()
    {
        this.TreeGridData = BusinessObject.GetSelfDataSource().ToList();
    }
}
```

In this example, the search will only look for matches in the `TaskId` and `Duration` columns, ignoring other columns like `TaskName`, `Progress`, and `Priority`.

---

## Searching with Case Sensitivity

By default, TreeGrid search is **case-insensitive** (e.g., "Task" and "task" are treated the same). To enable case-sensitive searching, set `IgnoreCase="false"` in `TreeGridSearchSettings`:

```razor
<SfTreeGrid DataSource="@TreeData" 
            IdMapping="TaskId" 
            ParentIdMapping="ParentId" 
            TreeColumnIndex="1"
            Toolbar="@(new List<string> { "Search" })">
    <TreeGridSearchSettings IgnoreCase="false" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="Task ID" Width="80" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="200" />
        <TreeGridColumn Field="Priority" HeaderText="Priority" Width="120" />
    </TreeGridColumns>
</SfTreeGrid>
```

When `IgnoreCase="false"`, searching for "task" will not match "Task" or "TASK".

---

## Searching with Ignore Accent

By default, TreeGrid search is **accent-sensitive** (e.g., "José" and "Jose" are treated differently). To enable accent-insensitive searching (diacritic characters are ignored), set `IgnoreAccent="true"` in `TreeGridSearchSettings`:

```razor
<SfTreeGrid DataSource="@TreeData" 
            IdMapping="TaskId" 
            ParentIdMapping="ParentId" 
            TreeColumnIndex="1"
            Toolbar="@(new List<string> { "Search" })">
    <TreeGridSearchSettings IgnoreAccent="true" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="Task ID" Width="80" />
        <TreeGridColumn Field="ResourceName" HeaderText="Resource Name" Width="200" />
        <TreeGridColumn Field="City" HeaderText="City" Width="150" />
    </TreeGridColumns>
</SfTreeGrid>
```

When `IgnoreAccent="true"`, searching for "Jose" will match "José", "Zoë" will match "Zoe", etc.

> **Notes:**
> - This feature works only for characters outside the ASCII range.
> - Accent-insensitive search applies to both searching and filtering operations when using an `IEnumerable` data source.

---

## Search Events

Respond to search actions using the `Searching` and `Searched` events:

| Event | Definition | Cancel? | Args Type |
|---|---|---|---|
| `Searching` | Gets or sets the event callback that is raised before the search action is performed in the tree grid. | ✅ | `SearchingEventArgs` |
| `Searched` | Gets or sets the event callback that is raised after the search action is performed in the tree grid. | ❌ | `SearchedEventArgs` |

**`SearchingEventArgs` properties (Searching):**

| Property | Type | Notes |
|---|---|---|
| `SearchText` | `string` | The search text entered in the toolbar |
| `Cancel` | `bool` | Set `true` to prevent the search |

**`SearchedEventArgs` properties (Searched):**

| Property | Type | Notes |
|---|---|---|
| `SearchText` | `string` | The search text that was applied |

**Prevent search if no text provided:**

```csharp
private void OnSearching(SearchingEventArgs args)
{
    if (string.IsNullOrWhiteSpace(args.SearchText))
    {
        args.Cancel = true; // Prevent empty searches
    }
}
```

**Setup in TreeGrid:**

```razor
<SfTreeGrid Toolbar="@(new List<string> { "Search" })" ...>
    <TreeGridEvents Searching="OnSearching" 
                    Searched="OnSearched"
                    TValue="BusinessObject" />
    ...
</SfTreeGrid>

@code {
    private void OnSearching(SearchingEventArgs args)
    {
        // Handle before search
    }

    private void OnSearched(SearchedEventArgs args)
    {
        // Handle after search
    }
}
```

---

## Key Notes

- Search preserves tree hierarchy — matching child rows show their parent rows too.
- For column-level search, use **Filter Bar** (see [filtering.md](filtering.md)) — the toolbar search is a global cross-column search.
- For remote data, the adaptor translates search to server-side query parameters.

---
