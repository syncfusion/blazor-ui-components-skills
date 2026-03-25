# Sorting in Syncfusion Blazor TreeGrid

> **Required namespace:** Include `@using Syncfusion.Blazor.TreeGrid;` for TreeGrid components and `@using Syncfusion.Blazor.Grids;` for `SortDirection` and `TextAlign` enums and for event handlers using sorting events.

Sorting enables reordering rows in **Ascending** or **Descending** order. To sort a column, click the column header. To sort multiple columns, press and hold the **Ctrl** key and click the column header. Sorting of any one of the multi-sorted columns can be cleared by pressing and holding the **Shift** key and clicking the specific column header.

## Table of Contents
- [Enable Sorting](#enable-sorting)
- [Initial Sort Order](#initial-sort-order)
- [Multi-Column Sorting](#multi-column-sorting)
- [Disable Sort on Specific Columns](#disable-sort-on-specific-columns)
- [Programmatic Sorting](#programmatic-sorting)
- [Custom Sort Comparer](#custom-sort-comparer)
- [Sort Events](#sort-events)
- [Touch Interaction](#touch-interaction)
- [Key Notes](#key-notes)

---

## Enable Sorting

Set `AllowSorting="true"` on the `SfTreeGrid`. Sorting options can be configured through `TreeGridSortSettings`.

```cshtml
@using Syncfusion.Blazor.TreeGrid;

<SfTreeGrid DataSource="@TreeGridData"
            AllowSorting="true"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            TreeColumnIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80"  TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Priority" HeaderText="Priority"  Width="80" />
    </TreeGridColumns>
</SfTreeGrid>
```

- TreeGrid columns are sorted in **Ascending** order by default. Clicking an already sorted column toggles its sort direction.
- Apply and clear sorting programmatically using `SortByColumnAsync` and `ClearSortingAsync`.
- To disable sorting for a particular column, set `AllowSorting="false"` on the `TreeGridColumn`.

---

## Initial Sort Order

Pre-sort the TreeGrid on load by setting `Field` and `Direction` in the `Columns` of `TreeGridSortSettings`:

```cshtml
<SfTreeGrid DataSource="@TreeGridData" AllowSorting="true" IdMapping="TaskId" ParentIdMapping="ParentId" TreeColumnIndex="1">
    <TreeGridSortSettings>
        <TreeGridSortColumns>
            <TreeGridSortColumn Field="TaskName" Direction="Syncfusion.Blazor.Grids.SortDirection.Descending" />
        </TreeGridSortColumns>
    </TreeGridSortSettings>
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80"  TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Priority" HeaderText="Priority"  Width="80" />
    </TreeGridColumns>
</SfTreeGrid>
```

---

## Multi-Column Sorting

Hold **Ctrl** and click additional column headers to sort by multiple columns simultaneously. Hold **Shift** and click a sorted column header to clear sorting on that specific column.

Configure initial multi-column sort:

```cshtml
<TreeGridSortSettings>
    <TreeGridSortColumns>
        <TreeGridSortColumn Field="TaskName" Direction="Syncfusion.Blazor.Grids.SortDirection.Descending" />
    </TreeGridSortColumns>
</TreeGridSortSettings>
```

---

## Disable Sort on Specific Columns

```cshtml
<TreeGridColumn Field="Progress" AllowSorting="false" />
```

---

## Programmatic Sorting

Sort columns from code using `SortByColumnAsync` and clear all sorts using `ClearSortingAsync`:

```cshtml
@using Syncfusion.Blazor.TreeGrid;

<button @onclick="SortByName">Sort by Name</button>
<button @onclick="ClearSort">Clear Sort</button>

<SfTreeGrid @ref="TreeRef" AllowSorting="true" DataSource="@TreeGridData" IdMapping="TaskId" ParentIdMapping="ParentId" TreeColumnIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    private SfTreeGrid<BusinessObject> TreeRef;

    private async Task SortByName()
    {
        await TreeRef.SortByColumnAsync("TaskName", Syncfusion.Blazor.Grids.SortDirection.Ascending);
    }

    private async Task ClearSort()
    {
        await TreeRef.ClearSortingAsync();
    }
}
```

---

## Custom Sort Comparer

Override sort logic for a specific column using the `SortComparer` property of `TreeGridColumn`. The `SortComparer` type uses the `IComparer<object>` interface — the parameters `a` and `b` represent the **full row data objects** (cast to the model class), not raw cell values. The function returns **-1**, **0**, or **1** depending on the comparison result.

```cshtml
@using Syncfusion.Blazor.TreeGrid
@using Syncfusion.Blazor.Grids

<SfTreeGrid DataSource="@TreeGridData" AllowSorting="true" IdMapping="TaskId" ParentIdMapping="ParentId" TreeColumnIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Priority"
                        HeaderText="Priority"
                        Width="120"
                        SortComparer="@(new PriorityComparer())" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    private List<BusinessObject> TreeGridData { get; set; }

    protected override void OnInitialized()
    {
        // Initialize TreeGridData here
    }

    // Sort Priority column by custom order: Low → Normal → High → Critical
    public class PriorityComparer : IComparer<object>
    {
        private static readonly Dictionary<string, int> Order = new()
        {
            { "Low",      1 },
            { "Normal",   2 },
            { "High",     3 },
            { "Critical", 4 }
        };

        public int Compare(object x, object y)
        {
            var a = x as BusinessObject;
            var b = y as BusinessObject;
            int pa = Order.GetValueOrDefault(a?.Priority ?? "", 0);
            int pb = Order.GetValueOrDefault(b?.Priority ?? "", 0);
            return pa.CompareTo(pb);
        }
    }
}
```

> **Notes:**
> - The `SortComparer` property works **only for local data**.
> - When using a column template to display data, the `Field` property of `TreeGridColumn` must be set for `SortComparer` to work.

---

## Sort Events

**Dedicated sort events (recommended):**

> **`SortingEventArgs` properties** (inherits `SortedEventArgs`):
> - `ColumnName` (`string?`) — field name of the column being sorted
> - `Direction` (`SortDirection`) — `Ascending`, `Descending`, or `None`
> - `Action` (`NotifyCollectionChangedAction`) — `Add`, `Remove`, `Replace`, or `Reset`
> - `SortedColumns` (`List<SortColumn>?`) — all sorted columns (populated via `SortColumnsAsync`)
> - `Cancel` (`bool`) — set `true` to cancel the sort *(SortingEventArgs only)*
> - `IsCtrlKeyPressed` (`bool`) — whether Ctrl was held for multi-sort *(SortingEventArgs only)*

```razor
<SfTreeGrid AllowSorting="true" ...>
    <TreeGridEvents TValue="TaskItem"
                    Sorting="OnSortingHandler"
                    Sorted="OnSortedHandler" />
    ...
</SfTreeGrid>

@code {
    /// <summary>
    /// Fires before sorting is applied (Cancellable).
    /// Use args.ColumnName (not args.Column) to get the sorted field name.
    /// </summary>
    private void OnSortingHandler(SortingEventArgs args)
    {
        Console.WriteLine($"Sorting by: {args.ColumnName}, Direction: {args.Direction}");

        // Prevent sorting on a specific column
        if (args.ColumnName == "TaskId")
        {
            args.Cancel = true;
        }
    }

    /// <summary>
    /// Fires after sorting is applied.
    /// </summary>
    private void OnSortedHandler(SortedEventArgs args)
    {
        Console.WriteLine($"Sort complete. Column: {args.ColumnName}, Direction: {args.Direction}, Action: {args.Action}");
    }
}
```

> **Note:** `args.RequestType` is the current action name. For sorting actions, its value is `sorting`.

---

## Touch Interaction

When the TreeGrid header is tapped on touchscreen devices, the selected column header is sorted. A popup is displayed for multi-column sorting. To sort multiple columns, tap the popup and then tap the desired TreeGrid headers.

---

## Key Notes

- Child rows remain grouped under their parent after sorting — row hierarchy is preserved.
- For **remote data**, the server must handle the sort query parameters (e.g., `$orderby`) based on the adaptor used.
- `SortComparer` works **only with local data**.

---
