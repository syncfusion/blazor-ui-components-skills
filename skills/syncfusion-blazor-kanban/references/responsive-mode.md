# Responsive Mode in Blazor Kanban Component

The Kanban component automatically adapts to smaller screen sizes with a responsive layout designed for touch and mobile devices.

## Table of Contents

- [Default Responsive Layout](#default-responsive-layout)
- [Swimlane Responsive Layout](#swimlane-responsive-layout)
- [Scrolling](#scrolling)
- [Selection in Responsive Mode](#selection-in-responsive-mode)

## Default Responsive Layout

On small screens (width ≤ 600px), the Kanban component renders in a responsive view by default:

- **80% width**: The active/selected column occupies 80% of the screen
- **20% width**: Adjacent columns appear at 20% as a peek

Users can swipe left or right to navigate between columns. The active column expands to full view.

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Testing" KeyField="@(new List<string>() {"Testing"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
</SfKanban>
```

> No additional configuration is needed. The responsive layout activates automatically based on screen width.

### Touch Interactions in Responsive Mode

| Interaction | Action |
|-------------|--------|
| **Tap and hold** a card | Activates drag mode for the card |
| **Swipe left/right** | Navigate between columns |
| **Tap** a card | Select the card (single selection mode) |

## Swimlane Responsive Layout

When swimlanes are enabled, the responsive mode shows a popup/dropdown to let users switch swimlane rows:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
    <KanbanSwimlaneSettings KeyField="Assignee" TextField="Assignee"></KanbanSwimlaneSettings>
</SfKanban>
```

In swimlane responsive mode:
- A swimlane selector dropdown appears at the top
- The selected swimlane row's cards are shown in the active column view
- Users can swipe to navigate between columns while staying in the selected swimlane

## Scrolling

In responsive mode, horizontal scrolling within columns is supported automatically. Content beyond the viewport is reachable by scrolling.

To enable vertical scrolling within columns, set a fixed height:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks" Height="500px">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
</SfKanban>
```

## Selection in Responsive Mode

In responsive mode, only **single card selection** is supported. Multi-selection via `Ctrl+click` or `Shift+click` is not available on touch devices:

```cshtml
<!-- Single selection is the effective mode on touch/mobile -->
<KanbanCardSettings HeaderField="Id" ContentField="Summary"
    SelectionType="SelectionType.Single">
</KanbanCardSettings>
```

> Even if `SelectionType.Multiple` is configured, on touch devices only single selection is active.

## Responsive Behavior Summary

| Feature | Desktop | Mobile/Touch |
|---------|---------|--------------|
| Column layout | All columns visible | 80%/20% active column view |
| Card drag | Mouse drag | Tap and hold, then drag |
| Column navigation | Scroll bar | Swipe gesture |
| Swimlane navigation | Rows visible | Dropdown selector |
| Card selection | Single or Multiple | Single only |
| Keyboard navigation | Full support | Not applicable |
