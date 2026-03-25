# Scrolling in Syncfusion Blazor TreeGrid

## Table of Contents
- [Set Width and Height](#set-width-and-height)
- [Responsive with Parent Container](#responsive-with-parent-container)
- [Frozen Rows and Columns](#frozen-rows-and-columns)
- [Sticky Header](#sticky-header)
- [Programmatic Scroll — ScrollIntoViewAsync](#programmatic-scroll--scrollintoviewasync)

---

## Set Width and Height

Fix the grid size to enable scrollbars:

```razor
<SfTreeGrid DataSource="@TreeData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            Width="800"
            Height="400"
            TreeColumnIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="ID"        Width="80" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="200" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" />
    </TreeGridColumns>
</SfTreeGrid>
```

- **Vertical scrollbar** appears when total row height exceeds `Height`
- **Horizontal scrollbar** appears when total column width exceeds `Width`
- Default for both is `"auto"` (no fixed size)

---

## Responsive with Parent Container

Fill the parent container dynamically:

```razor
<div style="height: 500px; width: 100%;">
    <SfTreeGrid Width="100%" Height="100%" ...>
        ...
    </SfTreeGrid>
</div>
```

> The parent container **must have explicit height** when `Height="100%"` is used — otherwise the grid collapses.

---

## Frozen Rows and Columns

Keep rows or columns visible during scrolling:

### Frozen Columns (left-side fixed)

```razor
<SfTreeGrid FrozenColumns="2" Width="800" ...>
    <!-- First 2 columns stay fixed; remaining scroll horizontally -->
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   Width="80" />   <!-- frozen -->
        <TreeGridColumn Field="TaskName" Width="200" />  <!-- frozen -->
        <TreeGridColumn Field="Duration" Width="120" />  <!-- scrollable -->
        <TreeGridColumn Field="Progress" Width="120" />  <!-- scrollable -->
        <TreeGridColumn Field="StartDate" Width="140" /> <!-- scrollable -->
    </TreeGridColumns>
</SfTreeGrid>
```

### Frozen Rows (top-side fixed)

```razor
<SfTreeGrid FrozenRows="3" Height="400" ...>
    <!-- First 3 rows stay fixed at top; rest scroll vertically -->
    ...
</SfTreeGrid>
```

### Individual column freeze

```razor
<!-- Freeze specific column to left -->
<TreeGridColumn Field="TaskName" IsFrozen="true" Freeze="FreezeDirection.Left" Width="200" />

<!-- Freeze to right -->
<TreeGridColumn Field="Actions" IsFrozen="true" Freeze="FreezeDirection.Right" Width="120" />
```

### Drag freeze-line to add/remove frozen columns

Set `AllowFreezeLineMoving="true"` to let users drag the frozen column separator at runtime:

```razor
<SfTreeGrid AllowFreezeLineMoving="true" Height="400" ...>
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   IsFrozen="true" Freeze="FreezeDirection.Left" Width="100" />
        <TreeGridColumn Field="TaskName" Width="200" />
        <TreeGridColumn Field="Approved" IsFrozen="true" Freeze="FreezeDirection.Right" Width="100" />
    </TreeGridColumns>
</SfTreeGrid>
```

> If no frozen columns are specified, the separator appears at both ends and users can drag to freeze from either side.

### Frozen rows/columns — Limitations

The following features are **not supported** when frozen rows or columns are enabled:
- Row Template
- Detail Template
- Cell Editing

---

## Sticky Header

Keep the column headers visible as the user scrolls down:

```razor
<SfTreeGrid EnableStickyHeader="true" Height="400" ...>
    ...
</SfTreeGrid>
```

---

## Programmatic Scroll — ScrollIntoViewAsync

Use `ScrollIntoViewAsync(columnIndex, rowIndex, rowHeight)` to programmatically scroll the grid to a specific row (vertical) or column (horizontal).

**Signature:**
```csharp
await TreeRef.ScrollIntoViewAsync(int columnIndex, int rowIndex, int rowHeight);
```

| Parameter | Description | Use `-1` to skip |
|---|---|---|
| `columnIndex` | 0-based column index to scroll horizontally | `-1` = no horizontal scroll |
| `rowIndex` | 0-based row index to scroll vertically | `-1` = no vertical scroll |
| `rowHeight` | Row height in px (used for virtual scroll) | `-1` = use default |

**Example — scroll to a specific row and/or column:**

```razor
@using Syncfusion.Blazor.TreeGrid
@using Syncfusion.Blazor.Buttons

<div>
    ColumnIndex: <input @bind-value="@ColumnIndex" />
    <SfButton @onclick="Scroll">Scroll Horizontally</SfButton>
</div>
<div>
    RowIndex: <input @bind-value="@RowIndex" />
    <SfButton @onclick="Scroll">Scroll Vertically</SfButton>
</div>

<SfTreeGrid @ref="TreeGrid"
            DataSource="@TreeData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            Height="400"
            Width="600"
            TreeColumnIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="ID"        Width="150" />
        <TreeGridColumn Field="TaskName" HeaderText="Name"      Width="150" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="150" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="150" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    private SfTreeGrid<TaskItem> TreeGrid;
    private List<TaskItem> TreeData = new();
    public int ColumnIndex { get; set; } = -1;
    public int RowIndex { get; set; } = -1;

    public async Task Scroll()
    {
        // Pass -1 for the axis you do NOT want to scroll
        await TreeGrid.ScrollIntoViewAsync(ColumnIndex, RowIndex, -1);
    }
}
```

> - Pass `-1` for `columnIndex` to scroll only vertically, or `-1` for `rowIndex` to scroll only horizontally.
> - The `rowHeight` parameter is relevant when row virtualization is enabled; pass `-1` to use the default row height.

---

## Key Notes

- **Width="100%"** without explicit height: horizontal scroll works, but no vertical scroll
- **`Height="100%"`** requires the parent container to have an explicit height, otherwise the grid collapses
- `EnableStickyHeader` requires the grid to be inside a scrollable parent container; setting `Height` on the grid itself is not required but the parent must scroll
- Frozen rows/columns do **not** support Row Template, Detail Template, or Cell Editing
- `AllowFreezeLineMoving` lets users drag the freeze separator at runtime without any code changes
- `ScrollIntoViewAsync`: pass `-1` for any parameter you want to skip (no scroll on that axis)

---
