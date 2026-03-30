# Validation in Blazor Kanban Component

The Kanban component provides constraint-based validation to limit the number of cards allowed in each column or swimlane row.

## Table of Contents

- [Minimum Card Count](#minimum-card-count)
- [Maximum Card Count](#maximum-card-count)
- [Constraint Type](#constraint-type)
- [Column Constraint Example](#column-constraint)
- [Swimlane Constraint Example](#swimlane-constraint)

## Minimum Card Count

Set `MinCount` on a `KanbanColumn` to define the minimum number of cards that should be in the column. A visual warning appears when the card count falls below this value:

```cshtml
<KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})" MinCount="2">
</KanbanColumn>
```

## Maximum Card Count

Set `MaxCount` on a `KanbanColumn` to define the maximum cards allowed. Cards cannot be dragged into a column that has reached its limit:

```cshtml
<KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})" MaxCount="5">
</KanbanColumn>
```

## Constraint Type

Use `ConstraintType` on `SfKanban` to control whether the min/max limit applies per column or per swimlane row within a column:

| Value | Description |
|-------|-------------|
| `ConstraintType.Column` | Count is checked across the entire column (default) |
| `ConstraintType.Swimlane` | Count is checked within each swimlane row separately |

## Column Constraint

Full example applying both min and max constraints at the column level. Set `ConstraintType="ConstraintType.Column"` on `SfKanban` (this is also the default):

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks"
    ConstraintType="ConstraintType.Column">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})" MinCount="6">
        </KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"
            MinCount="2" MaxCount="5">
        </KanbanColumn>
        <KanbanColumn HeaderText="Testing" KeyField="@(new List<string>() {"Testing"})"
            MinCount="2" MaxCount="5">
        </KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})">
        </KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
</SfKanban>

@code {
    public class TasksModel
    {
        public string Id { get; set; }
        public string Status { get; set; }
        public string Summary { get; set; }
    }
}
```

## Swimlane Constraint

Apply per-swimlane min/max validation by setting `ConstraintType="ConstraintType.Swimlane"` on `SfKanban`:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks"
    ConstraintType="ConstraintType.Swimlane">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})">
        </KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"
            MinCount="2" MaxCount="5">
        </KanbanColumn>
        <KanbanColumn HeaderText="Testing" KeyField="@(new List<string>() {"Testing"})"
            MinCount="2" MaxCount="5">
        </KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})">
        </KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
    <KanbanSwimlaneSettings KeyField="Assignee" TextField="Assignee"></KanbanSwimlaneSettings>
</SfKanban>
```

> With `ConstraintType.Swimlane`, each swimlane row is independently checked against the min/max values. For example, if `MaxCount=5`, each assignee's row can have at most 5 cards in "In Progress".

## Validation Properties Reference

| Property | Component | Type | Description |
|----------|-----------|------|-------------|
| `MinCount` | `KanbanColumn` | `int` | Minimum card count; visual warning when below this |
| `MaxCount` | `KanbanColumn` | `int` | Maximum card count; drag-in blocked when at this limit |
| `ConstraintType` | `SfKanban` | `ConstraintType` | Apply constraint at `Column` (default) or `Swimlane` level |
