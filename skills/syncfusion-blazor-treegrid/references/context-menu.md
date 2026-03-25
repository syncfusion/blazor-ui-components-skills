# Context Menu in Syncfusion Blazor TreeGrid

> **Required namespace:** Include `@using Syncfusion.Blazor.Grids;` for `ContextMenuItemModel` and context menu event arguments.

> **⚠️ IMPORTANT - EditMode Namespace Clarification:**
> When using `TreeGridEditSettings.Mode` property, always use `Syncfusion.Blazor.TreeGrid.EditMode` (not `Syncfusion.Blazor.Grids.EditMode`).
> Both namespaces define an `EditMode` enum, but TreeGrid requires its own:
> ```razor
> <TreeGridEditSettings Mode="Syncfusion.Blazor.TreeGrid.EditMode.Row" />
> ```

The TreeGrid supports a right-click context menu with default grid actions and custom menu items.

## Table of Contents
- [Enable Context Menu](#enable-context-menu)
- [Default Context Menu Items](#default-context-menu-items)
- [Custom Context Menu Items](#custom-context-menu-items)
- [Context Menu with Paging](#context-menu-with-paging)
- [Conditionally Show/Hide Items](#conditionally-showhide-items)
- [Key Notes](#key-notes)

---

## Enable Context Menu

Pass item names to `ContextMenuItems`:

```razor
<SfTreeGrid DataSource="@TreeData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            ContextMenuItems="@ContextItems">
    <TreeGridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true" />
    ...
</SfTreeGrid>

@code {
    private List<object> ContextItems = new()
    {
        "AutoFit", "AutoFitAll",
        "SortAscending", "SortDescending",
        "Edit", "Delete", "Save", "Cancel",
        "PdfExport", "ExcelExport", "CsvExport",
        "FirstPage", "PrevPage", "LastPage", "NextPage"
    };
}
```

---

## Default Context Menu Items

| Item | Action |
|---|---|
| `AutoFit` | Auto-fit the clicked column |
| `AutoFitAll` | Auto-fit all columns |
| `SortAscending` | Sort column ascending |
| `SortDescending` | Sort column descending |
| `Edit` | Edit the clicked row |
| `Delete` | Delete the clicked row |
| `Save` | Save pending edit |
| `Cancel` | Cancel pending edit |
| `Copy` | Copy selected cells |
| `PdfExport` | Export to PDF |
| `ExcelExport` | Export to Excel |
| `CsvExport` | Export to CSV |
| `FirstPage` | Navigate to first page |
| `PrevPage` | Navigate to previous page |
| `LastPage` | Navigate to last page |
| `NextPage` | Navigate to next page |
| `AddRecord` | Add a new row |
| `Indent` | Indent selected row (make it a child) |
| `Outdent` | Outdent selected row (make it a sibling) |
| `CollapseAll` | Collapse all rows |
| `ExpandAll` | Expand all rows |

---

## Custom Context Menu Items

Add custom items using `ContextMenuItemModel`:

```razor
<SfTreeGrid ContextMenuItems="@ContextItems">
    <TreeGridEvents TValue="TaskItem" ContextMenuItemClicked="OnContextMenuClick" />
    ...
</SfTreeGrid>

@code {
    private List<object> ContextItems = new()
    {
        "Edit", "Delete",
        new Syncfusion.Blazor.Grids.ContextMenuItemModel
        {
            Text = "View Details",
            Id = "view_details",
            IconCss = "e-icons e-eye"
        },
        new Syncfusion.Blazor.Grids.ContextMenuItemModel
        {
            Text = "Duplicate Row",
            Id = "duplicate_row",
            IconCss = "e-icons e-copy"
        }
    };

    private void OnContextMenuClick(Syncfusion.Blazor.Grids.ContextMenuClickEventArgs<TaskItem> args)
    {
        switch (args.Item.Id)
        {
            case "view_details":
                Console.WriteLine($"Viewing: {args.RowInfo.RowData.TaskName}");
                // navigate or open a dialog
                break;
            case "duplicate_row":
                var copy = args.RowInfo.RowData;
                // add a copy to data source
                break;
        }
    }
}
```

---

## Context Menu with Paging

When enabling paging-related context menu items (`FirstPage`, `PrevPage`, `NextPage`, `LastPage`), ensure your data source has enough records to produce multiple pages:

```razor
@using Syncfusion.Blazor.TreeGrid;

<SfTreeGrid DataSource="@TreeData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            AllowPaging="true"
            AllowSorting="true"
            TreeColumnIndex="1"
            ContextMenuItems="@(new List<object>() { "SortAscending", "SortDescending", "FirstPage", "PrevPage", "NextPage", "LastPage", "Edit", "Delete", "AutoFit" })">
    <TreeGridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true" Mode="EditMode.Row"></TreeGridEditSettings>
    <TreeGridPageSettings PageSize="2"></TreeGridPageSettings>
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="Task ID" IsPrimaryKey="true" Width="80"></TreeGridColumn>
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="200"></TreeGridColumn>
        <TreeGridColumn Field="Duration" HeaderText="Duration" Width="120"></TreeGridColumn>
        <TreeGridColumn Field="Priority" HeaderText="Priority" Width="100"></TreeGridColumn>
        <TreeGridColumn Field="Progress" HeaderText="Progress" Width="100"></TreeGridColumn>
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public class BusinessObject
    {
        public int TaskId { get; set; }
        public string TaskName { get; set; }
        public int Duration { get; set; }
        public string Priority { get; set; }
        public int Progress { get; set; }
        public int? ParentId { get; set; }
    }

    public List<BusinessObject> TreeData = new List<BusinessObject>();

    protected override void OnInitialized()
    {
        // Generate 30 records to demonstrate paging with PageSize="2" (creates 15 pages)
        int taskId = 1;
        
        // Parent Task 1 with children
        TreeData.Add(new BusinessObject() { TaskId = taskId, TaskName = "Parent Task 1", Duration = 50000, Progress = 70, ParentId = null, Priority = "High" });
        int parentId = taskId++;
        for (int i = 0; i < 4; i++)
            TreeData.Add(new BusinessObject() { TaskId = taskId++, TaskName = $"Child Task {i + 1}", Duration = 40000 + (i * 5000), Progress = 65 + i * 3, ParentId = parentId, Priority = new[] { "High", "Normal", "Low", "Critical" }[i] });
        
        // Parent Task 2 with children
        TreeData.Add(new BusinessObject() { TaskId = taskId, TaskName = "Parent Task 2", Duration = 60000, Progress = 75, ParentId = null, Priority = "Normal" });
        parentId = taskId++;
        for (int i = 0; i < 4; i++)
            TreeData.Add(new BusinessObject() { TaskId = taskId++, TaskName = $"Child Task {i + 5}", Duration = 50000 + (i * 5000), Progress = 55 + i * 4, ParentId = parentId, Priority = new[] { "Critical", "High", "Normal", "Low" }[i] });
        
        // Parent Task 3 with children
        TreeData.Add(new BusinessObject() { TaskId = taskId, TaskName = "Parent Task 3", Duration = 70000, Progress = 80, ParentId = null, Priority = "Critical" });
        parentId = taskId++;
        for (int i = 0; i < 4; i++)
            TreeData.Add(new BusinessObject() { TaskId = taskId++, TaskName = $"Child Task {i + 9}", Duration = 45000 + (i * 3000), Progress = 70 + i * 2, ParentId = parentId, Priority = new[] { "Low", "Normal", "High", "Critical" }[i] });
        
        // Parent Task 4 with children
        TreeData.Add(new BusinessObject() { TaskId = taskId, TaskName = "Parent Task 4", Duration = 55000, Progress = 65, ParentId = null, Priority = "Low" });
        parentId = taskId++;
        for (int i = 0; i < 4; i++)
            TreeData.Add(new BusinessObject() { TaskId = taskId++, TaskName = $"Child Task {i + 13}", Duration = 48000 + (i * 4000), Progress = 60 + i * 3, ParentId = parentId, Priority = new[] { "Normal", "Critical", "Low", "High" }[i] });
        
        // Parent Task 5 with children
        TreeData.Add(new BusinessObject() { TaskId = taskId, TaskName = "Parent Task 5", Duration = 65000, Progress = 85, ParentId = null, Priority = "High" });
        parentId = taskId++;
        for (int i = 0; i < 4; i++)
            TreeData.Add(new BusinessObject() { TaskId = taskId++, TaskName = $"Child Task {i + 17}", Duration = 52000 + (i * 6000), Progress = 75 + i * 2, ParentId = parentId, Priority = new[] { "High", "Low", "Normal", "Critical" }[i] });
    }
}
```

**Key Points:**
- `PageSize="2"` displays 2 records per page with 30 total records (15 pages)
- Paging context menu items are now functional for navigating between multiple pages
- Right-click any row to access FirstPage, PrevPage, NextPage, and LastPage options

---

## Conditionally Show/Hide Items

Use `ContextMenuOpen` to hide items based on row data:

```razor
<TreeGridEvents TValue="TaskItem"
                ContextMenuOpen="OnContextMenuOpen"
                ContextMenuItemClicked="OnMenuClick" />

@code {
    private void OnContextMenuOpen(ContextMenuOpenEventArgs<TaskItem> args)
    {
        // Hide Delete for locked rows
        foreach (var item in args.Items)
        {
            if (item.Id == "TreeGrid_delete" && args.RowInfo.RowData?.IsLocked == true)
                item.Hidden = true;
        }
    }
}
```

### Expand/Collapse Best Practice

Use the TreeGrid key-based expand/collapse API for row actions:

```csharp
await TreeGridRef.ExpandByKeyAsync(rowData.TaskId);
await TreeGridRef.CollapseByKeyAsync(rowData.TaskId);
```

---

## Key Notes

- The context menu only appears when right-clicking on a **data row** or **header cell**, depending on the item type.
- Export items (`PdfExport`, `ExcelExport`) require `AllowPdfExport="true"` / `AllowExcelExport="true"` on the TreeGrid.
- `Indent` / `Outdent` items are specific to TreeGrid (not available in regular DataGrid).
