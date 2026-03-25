# Rows and Height Configuration

## Table of Contents
- [Overview](#overview)
- [RowHeight](#rowheight) - [AutoFitRows](#autofitrows) - [TaskbarHeight](#taskbarheight) - [Row Styling](#row-styling) - [Row Templates](#row-templates) - [AllowRowDragDrop](#allowrowdragdrop) - [Accessibility](#accessibility) - [Tips](#tips) - [Performance Considerations](#performance-considerations)

---

## Overview
Control row height, auto-fit, and row-level styling to optimize the visual layout and readability of the Gantt grid.

## RowHeight

The `RowHeight` property sets the height of each row in pixels:

```razor
<SfGantt RowHeight="25">
    ...
</SfGantt>
```

Default is typically 36px. Adjust based on content density and accessibility needs.

## AutoFitRows

Enable automatic row height adjustment to fit content:

```razor
<SfGantt AutoFitRows="true">
    ...
</SfGantt>
```

When enabled, rows expand to show all content without clipping. Useful for multi-line text or templates with dynamic content.

## TaskbarHeight

Control the height of taskbar elements on the timeline:

```razor
<SfGantt TaskbarHeight="20">
    ...
</SfGantt>
```

Smaller values create compact timelines; larger values make taskbars more visible.

## Row Styling

Apply custom CSS classes or inline styles to rows via `RowDataBound` event or row templates:

```csharp
public void OnRowDataBound(RowDataBoundEventArgs<TaskData> args)
{
    if (args.Data.Progress == 100)
        args.RowElement?.AddClass("row-complete");
    
    if (args.Data.Priority == "High")
        args.RowElement?.AddClass("row-priority-high");
}
```

CSS:

```css
.row-complete {
    background-color: #e8f5e9;
}

.row-priority-high {
    background-color: #ffebee;
}
```

## Row Templates

Use row templates for custom row layouts (advanced):

```razor
<GanttTemplates>
    <RowTemplate Context="task">
        <tr>
            <td>@task.TaskID</td>
            <td class="custom-row">@task.TaskName - @task.Assignee</td>
        </tr>
    </RowTemplate>
</GanttTemplates>
```

## AllowRowDragDrop

Enable row reordering via drag-and-drop:

```razor
<SfGantt AllowRowDragDrop="true">
    ...
</SfGantt>
```

Users can drag rows to reorder tasks and change parent-child hierarchy.

## Accessibility

- Ensure adequate `RowHeight` for readability (especially for touch interfaces).
- Use semantic row markup and ARIA attributes for screen readers.
- Provide keyboard navigation (arrow keys to move between rows).

## Tips
- Use `AutoFitRows` sparingly; it can slow rendering for large datasets.
- Combine with `EnableVirtualization` to optimize performance.
- Color-code rows by priority, status, or completion for quick scanning.

## Performance Considerations
- Fixed `RowHeight` performs better than `AutoFitRows`.
- Large row templates can impact rendering speed. Test with your dataset size.
