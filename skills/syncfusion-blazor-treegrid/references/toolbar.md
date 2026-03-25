# Toolbar in Syncfusion Blazor TreeGrid

> **Required namespace:** Include `@using Syncfusion.Blazor.Grids;` for toolbar-related types and ClickEventArgs.

The TreeGrid toolbar sits above the grid and provides quick-access actions like adding rows, searching, exporting, and more.

---

## Table of Contents

- [Built-in Toolbar Items](#built-in-toolbar-items)
- [Custom Items with Same Text as Built-in Items](#custom-items-with-same-text-as-built-in-items)
- [Enable/Disable Toolbar Items Dynamically](#enabledisable-toolbar-items-dynamically)

---

## Built-in Toolbar Items

Pass a list of item names to the `Toolbar` property:

```razor
<SfTreeGrid DataSource="@TreeData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            Toolbar="@ToolbarItems">
    <TreeGridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true" />
    ...
</SfTreeGrid>

@code {
    private List<string> ToolbarItems = new()
    {
        "Add", "Edit", "Delete", "Update", "Cancel",  // editing
        "Search",                                       // search box
        "ExcelExport", "PdfExport", "Print", // export
        "ExpandAll", "CollapseAll"                     // expand/collapse
    };
}
```

| Item | Action |
|---|---|
| `Add` | Add a new row |
| `Edit` | Edit selected row |
| `Delete` | Delete selected row |
| `Update` | Save current edit |
| `Cancel` | Cancel current edit |
| `Search` | Show search text box |
| `ExcelExport` | Export to Excel |
| `PdfExport` | Export to PDF |
| `Print` | Print the grid |
| `ExpandAll` | Expand all rows |
| `CollapseAll` | Collapse all rows |

---

## Custom Items with Same Text as Built-in Items

> **Pitfall:** If a custom `ItemModel` uses `Text` equal to a built-in item name (e.g., `"Add"`, `"Delete"`) **without** setting a unique `Id`, the TreeGrid treats it as the built-in item — which may be disabled until `TreeGridEditSettings` is present. Always set a distinct `Id` on custom items.

```razor
@code {
    // ✅ Correct — unique Id prevents misidentification
    List<Syncfusion.Blazor.Navigations.ItemModel> ToolbarItems = new()
    {
        new() { Text = "Add",    Id = "custom_add",    TooltipText = "Add Record",    PrefixIcon = "add" },
        new() { Text = "Delete", Id = "custom_delete", TooltipText = "Delete Record", PrefixIcon = "delete" },
    };
}
```

---

## Enable/Disable Toolbar Items Dynamically

Use the `EnableToolbarItemsAsync` method. Built-in item IDs follow the pattern `{TreeGridID}_gridcontrol_{ItemName}`:

```razor
<SfTreeGrid ID="TreeGrid" @ref="TreeRef" Toolbar="@(new string[] { "ExpandAll", "CollapseAll" })" ...>
    <TreeGridEvents TValue="TaskItem" OnToolbarClick="ToolBarClick" />
    ...
</SfTreeGrid>

@code {
    private SfTreeGrid<TaskItem> TreeRef;

    // Enable items (e.g. triggered by a button)
    public void Enable()
    {
        TreeRef.EnableToolbarItemsAsync(
            new List<string> { "TreeGrid_gridcontrol_ExpandAll", "TreeGrid_gridcontrol_CollapseAll" }, true);
    }

    // Disable items
    public void Disable()
    {
        TreeRef.EnableToolbarItemsAsync(
            new List<string> { "TreeGrid_gridcontrol_ExpandAll", "TreeGrid_gridcontrol_CollapseAll" }, false);
    }

    public void ToolBarClick(Syncfusion.Blazor.Navigations.ClickEventArgs args)
    {
        if (args.Item.Text == "ExpandAll")   TreeRef.ExpandAll();
        if (args.Item.Text == "CollapseAll") TreeRef.CollapseAllAsync();
    }
}
```

> Built-in item IDs use the format `{TreeGridID}_gridcontrol_{ItemName}` (e.g., `TreeGrid_gridcontrol_ExpandAll`). The `ID` attribute on `<SfTreeGrid>` sets the prefix.

---
