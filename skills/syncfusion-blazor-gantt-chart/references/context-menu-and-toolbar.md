# Context Menu and Toolbar

## Table of Contents
- [When to Use](#when-to-use)
- [Toolbar — Built-in Items](#toolbar--built-in-items)
- [Toolbar — Custom Items](#toolbar--custom-items)
- [Toolbar — Mixed Built-in and Custom Items](#toolbar--mixed-built-in-and-custom-items)
- [Enable or Disable Toolbar Items Dynamically](#enable-or-disable-toolbar-items-dynamically)
- [Context Menu — Enable with Default Items](#context-menu--enable-with-default-items)
- [Context Menu — Custom Items Only](#context-menu--custom-items-only)
- [Context Menu — Mixed Built-in and Custom Items](#context-menu--mixed-built-in-and-custom-items)
- [Context Menu — Sub-menu Items](#context-menu--sub-menu-items)
- [Context Menu — Disable for Specific Columns](#context-menu--disable-for-specific-columns)
- [Context Menu — Disable Items Dynamically](#context-menu--disable-items-dynamically)
- [Key Properties and APIs](#key-properties-and-apis)
- [Common Scenarios and Decisions](#common-scenarios-and-decisions)
- [Troubleshooting](#troubleshooting)

---

## When to Use

Guide the user to this reference when they need to:
- Add action buttons (Add, Edit, Delete, Export, Search, Zoom) to the Gantt toolbar
- Create custom toolbar buttons with click handlers
- Align custom toolbar items to the left or right
- Enable/disable toolbar items based on application state
- Enable right-click context menu on Gantt rows or column headers
- Add custom actions to the context menu
- Create sub-menu (nested) context menu items
- Disable the context menu for specific columns or conditionally disable menu items

---

## Toolbar — Built-in Items

When the user wants standard action buttons in the toolbar, pass built-in item names as strings to the `Toolbar` property.

**Full list of built-in toolbar items:**

| Item | Action |
|---|---|
| `Add` | Adds a new task record |
| `Cancel` | Cancels the current edit state |
| `CollapseAll` | Collapses all rows |
| `Delete` | Deletes the selected record |
| `Edit` | Opens edit mode for the selected record |
| `ExpandAll` | Expands all rows |
| `Indent` | Indents the selected task one level |
| `Outdent` | Outdents the selected task one level |
| `NextTimeSpan` | Navigates timeline forward |
| `PrevTimeSpan` | Navigates timeline backward |
| `Search` | Activates search input |
| `Update` | Saves the edited record |
| `ZoomIn` | Zooms in on the timeline |
| `ZoomOut` | Zooms out on the timeline |
| `ZoomToFit` | Fits all tasks to available chart width |
| `ExcelExport` | Exports to Excel |
| `PdfExport` | Exports to PDF |
| `CriticalPath` | Toggles critical path highlighting |

```razor
@using Syncfusion.Blazor.Gantt

<SfGantt DataSource="@TaskCollection"
         Toolbar="@(new List<string>() { "Add", "Edit", "Delete", "Update", "Cancel", "Search", "ExpandAll", "CollapseAll", "ZoomIn", "ZoomOut", "ZoomToFit" })"
         Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttEditSettings AllowEditing="true" AllowAdding="true" AllowDeleting="true">
    </GanttEditSettings>
</SfGantt>
```

---

## Toolbar — Custom Items

When the user wants custom toolbar buttons (not in the built-in list), use `ToolbarItem` objects in the `Toolbar` property and handle clicks via `OnToolbarClick`.

Custom toolbar items default to left-aligned. Use the `Align` property to position them right.

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Navigations

<SfGantt @ref="Gantt" DataSource="@TaskCollection" Toolbar="@Toolbaritems" Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttEvents OnToolbarClick="ToolbarClickHandler" TValue="TaskData"></GanttEvents>
</SfGantt>

@code {
    public SfGantt<TaskData> Gantt;

    public List<ToolbarItem> Toolbaritems = new List<ToolbarItem>()
    {
        new ToolbarItem() { Text = "Expand All", TooltipText = "Expand All", Id = "ExpandAll" },
        new ToolbarItem() { Text = "Collapse All", TooltipText = "Collapse All", Id = "CollapseAll",
                            Align = ItemAlign.Right }  // Right-aligned
    };

    public async void ToolbarClickHandler(ClickEventArgs args)
    {
        if (args.Item.Id == "ExpandAll")
            await Gantt.ExpandAllAsync();

        if (args.Item.Id == "CollapseAll")
            await Gantt.CollapseAllAsync();
    }

    private List<TaskData> TaskCollection { get; set; }
    protected override void OnInitialized()
    {
        TaskCollection = new List<TaskData>
        {
            new TaskData { TaskID = 1, TaskName = "Project initiation", StartDate = new DateTime(2022, 01, 04), EndDate = new DateTime(2022, 01, 07) },
            new TaskData { TaskID = 2, TaskName = "Identify site location", StartDate = new DateTime(2022, 01, 04), Duration = "0", Progress = 30, ParentID = 1 },
            new TaskData { TaskID = 3, TaskName = "Perform soil test", StartDate = new DateTime(2022, 01, 04), Duration = "4", Progress = 40, ParentID = 1 },
            new TaskData { TaskID = 4, TaskName = "Project estimation", StartDate = new DateTime(2022, 01, 06), EndDate = new DateTime(2022, 01, 10) },
            new TaskData { TaskID = 5, TaskName = "Develop floor plan", StartDate = new DateTime(2022, 01, 06), Duration = "3", Progress = 30, ParentID = 4 }
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

> If a toolbar item `Id` does not match any built-in item name, it is automatically treated as a custom item.

---

## Toolbar — Mixed Built-in and Custom Items

When the user wants both built-in and custom items together, use `List<Object>` and mix string names with `ToolbarItem` objects.

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Navigations

<SfGantt DataSource="@TaskCollection" Toolbar="@Toolbaritems" Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttEvents OnToolbarClick="ToolbarClickHandler" TValue="TaskData"></GanttEvents>
</SfGantt>

@code {
    public string Message;

    // Mix strings (built-in) with ToolbarItem objects (custom)
    public List<Object> Toolbaritems = new List<Object>()
    {
        "ExpandAll",
        "CollapseAll",
        new ToolbarItem() { Text = "Approve All", TooltipText = "Approve All Tasks", Id = "ApproveAll" }
    };

    public void ToolbarClickHandler(ClickEventArgs args)
    {
        if (args.Item.Id == "ApproveAll")
            Message = "Approve All clicked";
    }
}
```

---

## Enable or Disable Toolbar Items Dynamically

When the user needs to enable or disable toolbar buttons based on state, call `gantt.EnableItems(indexes, isEnabled)` with a list of 0-based item indexes.

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfSwitch @bind-Checked="isEnabled" ValueChange="OnSwitchChange" TChecked="bool?"></SfSwitch>

<SfGantt @ref="Gantt" DataSource="@TaskCollection" Toolbar="@Toolbaritems"
         AllowFiltering="true" Height="450px" Width="900px">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttEvents OnToolbarClick="ToolbarClickHandler" TValue="TaskData"></GanttEvents>
</SfGantt>

@code {
    public SfGantt<TaskData> Gantt;
    private bool? isEnabled = true;

    public List<ToolbarItem> Toolbaritems = new List<ToolbarItem>()
    {
        new ToolbarItem() { Text = "Quick Filter", TooltipText = "Quick Filter", Id = "quickfilter" },
        new ToolbarItem() { Text = "Clear Filter", TooltipText = "Clear Filter", Id = "clearfilter" }
    };

    public async void ToolbarClickHandler(ClickEventArgs args)
    {
        if (args.Item.Id == "quickfilter")
            await Gantt.FilterByColumnAsync("TaskName", "startswith", "Perform");

        if (args.Item.Id == "clearfilter")
            await Gantt.ClearFilteringAsync();
    }

    private void OnSwitchChange(ChangeEventArgs<bool?> args)
    {
        isEnabled = args.Checked;
        // Enable/disable items at index 0 (quickfilter) and index 1 (clearfilter)
        Gantt.EnableItems(new List<int>() { 0, 1 }, isEnabled ?? true);
    }
}
```

**Pattern:** Pass 0-based item indexes to `EnableItems`. `true` = enabled, `false` = disabled.

---

## Context Menu — Enable with Default Items

When the user wants right-click actions on Gantt rows, set `EnableContextMenu="true"`. Built-in items appear automatically based on the clicked element.

**Built-in context menu items:**

| Item | Description |
|---|---|
| `AutoFit` | Auto-fits the current column |
| `AutoFitAll` | Auto-fits all columns |
| `SortAscending` | Sorts current column ascending |
| `SortDescending` | Sorts current column descending |
| `TaskInformation` | Opens edit dialog for the task |
| `Add` | Adds a new row |
| `Indent` | Indents the selected task one level |
| `Outdent` | Outdents the selected task one level |
| `DeleteTask` | Deletes the current task |
| `Save` | Saves the edited task |
| `Cancel` | Cancels the current edit |
| `DeleteDependency` | Removes the dependency link |
| `Convert` | Converts task to/from milestone |

```razor
@using Syncfusion.Blazor.Gantt

<SfGantt DataSource="@TaskCollection" Height="450px" Width="900px"
         EnableContextMenu="true" AllowSorting="true" AllowResizing="true">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" Dependency="Predecessor" ParentID="ParentID">
    </GanttTaskFields>
    <GanttEditSettings AllowAdding="true" AllowDeleting="true" AllowEditing="true">
    </GanttEditSettings>
</SfGantt>
```

---

## Context Menu — Custom Items Only

When the user wants fully custom right-click menu items, assign a `List<ContextMenuItemModel>` to `ContextMenuItems` and handle clicks with `ContextMenuItemClicked`.

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Grids

<SfGantt @ref="GanttInstance" DataSource="@TaskCollection" Height="450px" Width="700px"
         ContextMenuItems="@contextMenuItems">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttEvents ContextMenuItemClicked="ContextMenuItemClickedHandler" TValue="TaskData">
    </GanttEvents>
</SfGantt>

@code {
    private SfGantt<TaskData> GanttInstance;

    private List<ContextMenuItemModel> contextMenuItems = new List<ContextMenuItemModel>()
    {
        new ContextMenuItemModel() { Text = "Refresh", Target = ".e-content", Id = "Refresh" }
    };

    private async void ContextMenuItemClickedHandler(ContextMenuClickEventArgs<TaskData> args)
    {
        if (args.Item.Id == "Refresh")
            await GanttInstance.RefreshAsync();
    }
}
```

**`Target`** controls where the menu appears:
- `".e-content"` — on row cells
- `".e-headercell"` — on column headers

---

## Context Menu — Mixed Built-in and Custom Items

When the user wants both built-in actions and custom items in the same menu, use `List<Object>` mixing built-in string names with `ContextMenuItemModel` objects.

```razor
<SfGantt @ref="GanttInstance" DataSource="@TaskCollection" Height="450px" Width="700px"
         ContextMenuItems="@(new List<Object>() {
             "Add",
             new ContextMenuItemModel { Text = "Copy with headers", Target = ".e-content", Id = "copywithheader" }
         })">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttEditSettings AllowAdding="true"></GanttEditSettings>
    <GanttEvents ContextMenuItemClicked="ContextMenuItemClickedHandler" TValue="TaskData">
    </GanttEvents>
</SfGantt>

@code {
    private SfGantt<TaskData> GanttInstance;

    private async void ContextMenuItemClickedHandler(ContextMenuClickEventArgs<TaskData> args)
    {
        if (args.Item.Id == "copywithheader")
            await GanttInstance.CopyAsync(true);
    }
}
```

---

## Context Menu — Sub-menu Items

When the user wants nested (sub-menu) context menu items, add `Items` (`List<MenuItem>`) to a `ContextMenuItemModel`. Handle all clicks in `ContextMenuItemClicked`.

```razor
@using Syncfusion.Blazor.Gantt
@using Syncfusion.Blazor.Grids
@using Syncfusion.Blazor.Navigations

<SfGantt @ref="Gantt" DataSource="@TaskCollection" Height="450px" Width="700px"
         ContextMenuItems="@contextMenuItems">
    <GanttTaskFields Id="TaskID" Name="TaskName" StartDate="StartDate" EndDate="EndDate"
                     Duration="Duration" Progress="Progress" ParentID="ParentID">
    </GanttTaskFields>
    <GanttEditSettings AllowEditing="true"></GanttEditSettings>
    <GanttEvents ContextMenuItemClicked="ContextMenuItemClickedHandler" TValue="TaskData">
    </GanttEvents>
</SfGantt>

@code {
    private SfGantt<TaskData> Gantt;

    private List<ContextMenuItemModel> contextMenuItems = new List<ContextMenuItemModel>()
    {
        new ContextMenuItemModel
        {
            Text = "Gantt Action", Target = ".e-content", Id = "GanttAction",
            Items = new List<MenuItem>()
            {
                new MenuItem { Text = "Copy with headers", Id = "copywithheader" },
                new MenuItem { Text = "Edit", Id = "Edit" }
            }
        }
    };

    public async void ContextMenuItemClickedHandler(ContextMenuClickEventArgs<TaskData> args)
    {
        if (args.Item.Id == "copywithheader")
            await Gantt.CopyAsync(true);

        if (args.Item.Id == "Edit")
            await Gantt.OpenEditDialogAsync();
    }
}
```

---

## Context Menu — Disable for Specific Columns

When the user wants to prevent the context menu from appearing on a specific column, use `ContextMenuOpen` and set `Args.Cancel = true`.

```razor
<GanttEvents ContextMenuOpen="OnContextMenuOpen"
             ContextMenuItemClicked="ContextMenuItemClickedHandler"
             TValue="TaskData">
</GanttEvents>

@code {
    public void OnContextMenuOpen(ContextMenuOpenEventArgs<TaskData> Args)
    {
        // Completely suppress the menu when right-clicking on the Duration column
        if (Args.Column != null && Args.Column.Field == "Duration")
        {
            Args.Cancel = true;
        }
    }
}
```

**Pattern:** `Args.Cancel = true` → menu does not open. `Args.Column.Field` identifies the clicked column.

---

## Context Menu — Disable Items Dynamically

When the user wants the menu to appear but certain items should be grayed out based on conditions, use `ContextMenuOpen` and set `Args.ContextMenu.Items[index].Disabled`.

```razor
@using Syncfusion.Blazor.Navigations

<GanttEvents ContextMenuOpen="OnContextMenuOpen"
             ContextMenuItemClicked="ContextMenuItemClickedHandler"
             TValue="TaskData">
</GanttEvents>

@code {
    public void OnContextMenuOpen(ContextMenuOpenEventArgs<TaskData> Args)
    {
        if (Args.Column != null && Args.Column.Field == "Duration")
        {
            // Disable item at index 0 when clicking on Duration column
            Args.ContextMenu.Items[0].Disabled = true;
        }
        else
        {
            Args.ContextMenu.Items[0].Disabled = false;
        }
    }
}
```

**Pattern:** `Disabled = true` → item appears grayed out and unclickable. Use `Cancel = true` to hide the entire menu instead.

---

## Key Properties and APIs

| Property / Event / Method | Where Used | Purpose |
|---|---|---|
| `Toolbar` | `SfGantt` | `List<string>` (built-in), `List<ToolbarItem>` (custom), or `List<Object>` (mixed) |
| `OnToolbarClick` | `GanttEvents` | Fires on toolbar click; use `args.Item.Id` to identify |
| `EnableItems(indexes, enabled)` | `SfGantt` method | Enable/disable toolbar items by 0-based index at runtime |
| `ToolbarItem.Align` | `ToolbarItem` | `ItemAlign.Left` (default) or `ItemAlign.Right` |
| `EnableContextMenu` | `SfGantt` | `true` to show right-click context menu with built-in items |
| `ContextMenuItems` | `SfGantt` | `List<ContextMenuItemModel>` or `List<Object>` for custom/mixed menus |
| `ContextMenuItemClicked` | `GanttEvents` | Fires on context menu item click; use `args.Item.Id` |
| `ContextMenuOpen` | `GanttEvents` | Fires before menu opens; cancel or disable items here |
| `ContextMenuOpenEventArgs.Cancel` | Event arg | Set `true` to prevent the menu from opening |
| `ContextMenuOpenEventArgs.Column` | Event arg | Column that was right-clicked (`null` for row-area clicks) |
| `ContextMenuOpenEventArgs.ContextMenu.Items` | Event arg | Menu items list; set `.Disabled` per item |
| `ContextMenuItemModel.Items` | Model | `List<MenuItem>` for nested sub-menu entries |

---

## Common Scenarios and Decisions

**User wants standard Add/Edit/Delete/Search buttons in the toolbar**
→ Pass built-in string names: `"Add"`, `"Edit"`, `"Delete"`, `"Search"` to `Toolbar`.

**User wants a custom button in the toolbar**
→ Use `List<ToolbarItem>` with an `Id`; handle click in `OnToolbarClick` by checking `args.Item.Id`.

**User wants built-in and custom toolbar buttons together**
→ Use `List<Object>` mixing string names and `ToolbarItem` objects.

**User wants to disable a toolbar button until a row is selected**
→ Call `Gantt.EnableItems(new List<int>() { index }, false)` on load, then `true` in the `RowSelected` event.

**User wants right-click menu with default Gantt actions**
→ Set `EnableContextMenu="true"` on `SfGantt`. No additional configuration needed.

**User wants only custom items in the context menu**
→ Assign `List<ContextMenuItemModel>` to `ContextMenuItems`. Use `ContextMenuItemClicked` for handling.

**User wants both built-in and custom items in the same context menu**
→ Use `List<Object>` mixing string names and `ContextMenuItemModel` objects.

**User wants nested sub-menu in the context menu**
→ Add `Items = new List<MenuItem>() { ... }` to a `ContextMenuItemModel`.

**User wants to hide context menu on a specific column**
→ Handle `ContextMenuOpen`, check `Args.Column.Field`, set `Args.Cancel = true`.

**User wants to gray out (not hide) a menu item conditionally**
→ Handle `ContextMenuOpen`, set `Args.ContextMenu.Items[index].Disabled = true`.

---

## Troubleshooting

**Custom toolbar item click is not firing**
- Confirm the `Id` in `ToolbarItem` exactly matches what you check in `args.Item.Id` in `OnToolbarClick`
- Ensure `GanttEvents OnToolbarClick` is wired up on the same `SfGantt` component

**Built-in toolbar item (Edit/Delete) not working**
- Confirm `GanttEditSettings AllowEditing="true"` / `AllowDeleting="true"` is set — these permissions are required

**Context menu not appearing on right-click**
- For built-in menu: confirm `EnableContextMenu="true"` is on `SfGantt`
- For custom menu: confirm `ContextMenuItems` is assigned and not empty

**Context menu custom item click handler not called**
- Confirm `GanttEvents ContextMenuItemClicked` is registered on the component
- Check that `args.Item.Id` matches the `Id` in `ContextMenuItemModel` exactly

**Sub-menu items not showing**
- Confirm `Items` is a `List<MenuItem>` (from `Syncfusion.Blazor.Navigations`), not `List<ContextMenuItemModel>`
- Add `@using Syncfusion.Blazor.Navigations` to the page
