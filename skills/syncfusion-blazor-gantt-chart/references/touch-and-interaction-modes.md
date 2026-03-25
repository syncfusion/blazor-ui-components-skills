# Touch and Interaction Modes

## Table of Contents
- [Overview](#overview)
- [Primary Interaction Areas](#primary-interaction-areas)
- [Touch Support](#touch-support)
- [Pointer-Based Operations](#pointer-based-operations)
- [Splitter Interaction](#splitter-interaction)
- [Selection Behavior](#selection-behavior)
- [Mobile UX Recommendations](#mobile-ux-recommendations)
- [Accessibility Considerations](#accessibility-considerations)
- [Common Issues](#common-issues)
- [Summary](#summary)

---

## Overview
The Blazor Gantt Chart supports mouse, touch, and mixed-input interaction models for desktop, tablet, and mobile scenarios. Use this reference when tuning user interaction behavior across devices.

## Primary Interaction Areas
- Grid area for row selection, editing, sorting, and resizing
- Chart area for taskbar dragging, resizing, and dependency visualization
- Splitter for resizing grid and chart panes
- Toolbar and context menu for command-driven interactions

## Touch Support
Touch behavior is available automatically on supported devices, but design choices affect usability.

### Recommended touch-friendly settings
- Increase `RowHeight` for easier tap targets
- Increase `TaskbarHeight` for drag handles
- Use fewer visible columns on small screens
- Prefer toolbar actions over dense context menus on mobile

```razor
<SfGantt TValue="TaskData" DataSource="@Tasks" RowHeight="48" Height="500px">
    <GanttEditSettings AllowTaskbarEditing="true" AllowEditing="true"></GanttEditSettings>
</SfGantt>
```

## Pointer-Based Operations
### Grid interactions
- Tap/click row to select
- Double-tap/double-click to edit where editing is enabled
- Drag column edge to resize
- Drag header to reorder when enabled

### Chart interactions
- Drag taskbar to move scheduled dates
- Drag resize handles to change start or end dates
- Drag progress handle to update completion percentage
- Scroll or swipe horizontally to navigate the timeline

## Splitter Interaction
Use `GanttSplitterSettings` to define the initial divider position between the tree grid and chart panel.

```razor
<GanttSplitterSettings Position="35%"></GanttSplitterSettings>
```

Practical tips:
- Use a wider grid pane when task names or many columns are important
- Use a wider chart pane when timeline edits are the main workflow

## Selection Behavior
Touch devices work best with simpler selection patterns.
- Prefer row selection over cell selection on mobile
- Use `SelectionType.Single` unless multi-select is a clear requirement
- Avoid dense editing workflows that require precise cell taps

```razor
<GanttSelectionSettings Mode="SelectionMode.Row" Type="SelectionType.Single"></GanttSelectionSettings>
```

## Mobile UX Recommendations
- Keep toolbar commands concise
- Avoid too many simultaneous visible markers and labels
- Reduce horizontal density in the timeline
- Use dialog editing for complex forms instead of inline editing

## Accessibility Considerations
Touch support should still preserve keyboard and screen reader accessibility on hybrid devices.
- Ensure toolbar commands have clear labels
- Do not rely on gesture-only interactions
- Keep focus movement predictable after dialogs and edits

## Common Issues
### Taskbar drag feels difficult
- Increase `TaskbarHeight`
- Reduce clutter from labels or templates
- Test with fewer overlapping visual elements

### Small-screen layout feels crowded
- Hide secondary columns
- Move complex commands into toolbar or dialog workflows
- Use zoomed-out timeline defaults only when necessary

## Summary
Touch interaction works best when taskbars, rows, and commands are sized generously and editing flows favor dialogs over highly precise inline operations.