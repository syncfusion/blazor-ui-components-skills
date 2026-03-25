# Virtualization in Syncfusion Blazor TreeGrid

## Table of Contents
- [Row Virtualization](#row-virtualization)
- [Column Virtualization](#column-virtualization)
- [Virtual Mask Rows (Cell Placeholder)](#virtual-mask-rows-cell-placeholder)
- [Virtualization Configuration Properties](#virtualization-configuration-properties)
- [Virtualization vs Paging](#virtualization-vs-paging)
- [Limitations](#limitations)

---

## Row Virtualization

Render only the rows visible in the viewport — DOM size stays constant regardless of data size:

```razor
<SfTreeGrid DataSource="@TreeData"
            TValue="TaskItem"
            ChildMapping="Children"
            TreeColumnIndex="1"
            EnableVirtualization="true"
            RowHeight="35"
            Height="400">
    <TreeGridPageSettings PageSize="40" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="ID"        Width="80" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="200" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" />
    </TreeGridColumns>
</SfTreeGrid>
```

> **`Height` is required** with `EnableVirtualization` — the grid needs a fixed height to determine the viewport.
> **`RowHeight` is required** for accurate row virtualization calculations.

**Works with self-referential data too:**
```razor
<SfTreeGrid IdMapping="TaskId"
            ParentIdMapping="ParentId"
            EnableVirtualization="true"
            RowHeight="35"
            Height="500">
    <TreeGridPageSettings PageSize="40" />
</SfTreeGrid>
```

**Expand/collapse state is preserved** during virtual scrolling — child rows remain collapsed/expanded as expected.

**Performance tips:**
- Provide data as an array (`TaskItem[]`) rather than `List<TaskItem>` for best performance with large datasets.
- Set `RowHeight` to a fixed value for all rows to ensure consistent virtualization.
- Use `OverscanCount` to control buffer rows and balance performance vs. visual appeal.

---

## Column Virtualization

Render only the columns visible in the horizontal viewport — useful when you have 50+ columns:

```razor
<SfTreeGrid EnableColumnVirtualization="true"
            Width="800"
            Height="400"
            ...>
    <TreeGridColumns>
        <!-- Many columns — only visible ones render at a time -->
        <TreeGridColumn Field="Col1"  HeaderText="Col 1"  Width="100" />
        <TreeGridColumn Field="Col2"  HeaderText="Col 2"  Width="100" />
        <!-- ... up to 50+ columns -->
    </TreeGridColumns>
</SfTreeGrid>
```

**Combine row and column virtualization:**
```razor
<SfTreeGrid EnableVirtualization="true"
            EnableColumnVirtualization="true"
            Height="400"
            Width="900"
            ...>
```

---

## Virtual Mask Rows (Cell Placeholder)

Displays placeholder content in cells while data is loading during scrolling. This feature improves perceived performance by showing placeholders while new rows are being fetched. The same set of DOM elements is reused to enhance performance.

Enable cell placeholder during virtualization by setting `EnableVirtualMaskRow="true"` along with `EnableVirtualization` or `EnableColumnVirtualization`:

```razor
<SfTreeGrid Height="300"
            Width="450"
            RowHeight="38"
            EnableVirtualMaskRow="true"
            DataSource="@TreeGridData"
            IdMapping="TaskID"
            TreeColumnIndex="1"
            ParentIdMapping="ParentID"
            EnableVirtualization="true"
            EnableColumnVirtualization="true">
    <TreeGridPageSettings PageSize="40" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="ID"        Width="80" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="200" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" />
    </TreeGridColumns>
</SfTreeGrid>
```

**With hierarchical data:**
```razor
<SfTreeGrid RowHeight="35"
            OverscanCount="5"
            TValue="VirtualData"
            DataSource="@TreeGridData"
            ChildMapping="Children"
            EnableVirtualization="true"
            Height="400"
            TreeColumnIndex="1"
            EnableVirtualMaskRow="true">
    <TreeGridPageSettings PageSize="30" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="ID"        Width="80" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="200" />
    </TreeGridColumns>
</SfTreeGrid>
```

> **Best Practice:** For optimal performance with virtual mask rows, define both the `PageSize` property (in `TreeGridPageSettings`) and the `RowHeight` property.

---

## Virtualization Configuration Properties

Fine-tune virtualization behavior with these core properties:

| Property | Description | Default | Example |
|---|---|---|---|
| `EnableVirtualization` | Enables row virtualization | `false` | `true` |
| `EnableColumnVirtualization` | Enables column virtualization | `false` | `true` |
| `EnableVirtualMaskRow` | Shows placeholder cells during loading | `false` | `true` |
| `RowHeight` | Fixed height per row (required for accurate virtualization) | Auto | `35` or `38` |
| `OverscanCount` | Number of buffer rows rendered before/after the viewport | `3` | `5` |
| `Height` | Fixed height of the grid container | — | `400` or `"400px"` |
| `Width` | Fixed width of the grid container | — | `800` or `"100%"` |

### OverscanCount Behavior

The `OverscanCount` property controls how many extra rows are rendered as a buffer:
- **First/Last Page:** renders `OverscanCount` buffer rows (e.g., if `OverscanCount="5"`, 5 rows are rendered as buffer)
- **Middle Pages:** renders `OverscanCount` rows before AND after the visible area (total 10 rows with `OverscanCount="5"`)

This minimizes re-rendering during scrolling and provides a smoother user experience.

### PageSize and RowHeight

```razor
<SfTreeGrid EnableVirtualization="true"
            RowHeight="35"
            Height="400">
    <!-- By default, renders ~2x visible rows (400/35 ≈ 11 rows visible + overscans) -->
    <TreeGridPageSettings PageSize="40" />
    <TreeGridColumns>
        <!-- columns -->
    </TreeGridColumns>
</SfTreeGrid>
```

- **RowHeight:** Set a fixed height for each row. Without this, virtualization calculations become inaccurate.
- **PageSize:** Controls how many records are considered as a "page" during virtual scrolling. Typically 1.5–2x your visible row count.


## Virtualization vs Paging

| Feature | Paging | Row Virtualization |
|---|---|---|
| UX | Page controls | Seamless scroll |
| Data loaded | Per page | All data upfront |
| Best for | < 5K records | 5K–100K records |
| Export support | ✅ Full | ⚠️ Current view |
| Accessibility | ✅ Best | ⚠️ Limited | 
| Jump to row | ✅ Via page | ❌ |
| Editing support | ✅ Full | ✅ |
| Remote data | ✅ | ✅ | ✅|

---

## Limitations

| Limitation | Detail |
|---|---|
| Height required | `EnableVirtualization` needs a fixed `Height`; virtualization cannot work with auto-height |
| RowHeight required | Must set a fixed `RowHeight` for accurate virtualization calculations |
| Row template | `RowTemplate` is not supported with row virtualization |
| Detail template | `DetailTemplate` is not supported with row virtualization |
| Column spanning | Cell/row spanning is not compatible with column virtualization |
| Export | Exporting exports only currently rendered rows in virtualization mode — use paging for full data export |
| Aggregates | Aggregates may not reflect all data when virtual scroll is active |
| Dynamic Row Height | All rows must have the same fixed height; variable row heights break virtualization |

---
