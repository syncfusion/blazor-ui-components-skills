# Workflow in Blazor Kanban Component

Workflow allows you to restrict the columns to which cards can be dragged, enforcing allowed transitions between column states.

## Table of Contents

- [Defining Transition Columns](#defining-transition-columns)
- [Restricting Drag from Columns](#restricting-drag-from-a-column)
- [Restricting Drop to Columns](#restricting-drop-to-a-column)

## Defining Transition Columns

Use `TransitionColumns` on `KanbanColumn` to specify which other columns a card can move to from this column:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"
            TransitionColumns="@(new List<string>() {"InProgress"})">
        </KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"
            TransitionColumns="@(new List<string>() {"Testing", "Close"})">
        </KanbanColumn>
        <KanbanColumn HeaderText="Testing" KeyField="@(new List<string>() {"Testing"})"
            TransitionColumns="@(new List<string>() {"InProgress", "Close"})">
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

    public List<TasksModel> Tasks = new List<TasksModel>()
    {
        new TasksModel { Id = "Task 1", Status = "Open", Summary = "Analyze requirements." },
        new TasksModel { Id = "Task 2", Status = "InProgress", Summary = "Improve performance." },
        new TasksModel { Id = "Task 3", Status = "Testing", Summary = "Test new features." },
        new TasksModel { Id = "Task 4", Status = "Close", Summary = "Deploy to production." },
    };
}
```

> Cards can only be dropped into the columns listed in `TransitionColumns`. If `TransitionColumns` is not set on a column, cards from it can be dragged to any column.

## Restricting Drag from a Column

Set `AllowDrag="false"` on a `KanbanColumn` to prevent cards in that column from being dragged:

```cshtml
<KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})" AllowDrag="false">
</KanbanColumn>
```

This prevents users from moving cards out of the Done column.

## Restricting Drop to a Column

Set `AllowDrop="false"` on a `KanbanColumn` to prevent cards from being dropped into that column:

```cshtml
<KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})" AllowDrop="false">
</KanbanColumn>
```

This prevents users from moving cards back into the Backlog column.

## Combined Workflow Example

Enforce a strict linear workflow:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <!-- Backlog: can only go forward to In Progress; nothing can drop back here -->
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"
            TransitionColumns="@(new List<string>() {"InProgress"})"
            AllowDrop="false">
        </KanbanColumn>
        <!-- In Progress: can go to Testing or Done -->
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"
            TransitionColumns="@(new List<string>() {"Testing", "Close"})">
        </KanbanColumn>
        <!-- Testing: can go back to In Progress or forward to Done -->
        <KanbanColumn HeaderText="Testing" KeyField="@(new List<string>() {"Testing"})"
            TransitionColumns="@(new List<string>() {"InProgress", "Close"})">
        </KanbanColumn>
        <!-- Done: cards cannot be dragged out -->
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})" AllowDrag="false">
        </KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
</SfKanban>
```

## Workflow Properties Reference

| Property | Component | Type | Description |
|----------|-----------|------|-------------|
| `TransitionColumns` | `KanbanColumn` | `List<string>` | List of column key values this column's cards can be dragged to |
| `AllowDrag` | `KanbanColumn` | bool | Allow dragging cards out of this column (default: true) |
| `AllowDrop` | `KanbanColumn` | bool | Allow dropping cards into this column (default: true) |
