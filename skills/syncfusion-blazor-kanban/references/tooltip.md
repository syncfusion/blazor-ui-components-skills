# Tooltip in Blazor Kanban Component

The Kanban component provides built-in tooltip support to display additional card details on hover.

## Table of Contents

- [Enabling Tooltip](#enabling-tooltip)
- [Custom Tooltip Content](#custom-tooltip-content)

## Enabling Tooltip

Enable the tooltip with `EnableTooltip="true"` on `SfKanban`. The tooltip shows the card's header and content fields by default:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks" EnableTooltip="true">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
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
        new TasksModel { Id = "Task 1", Status = "Open", Summary = "Analyze the new requirements gathered from the customer." },
        new TasksModel { Id = "Task 2", Status = "InProgress", Summary = "Improve application performance." },
        new TasksModel { Id = "Task 3", Status = "Open", Summary = "Arrange a web meeting with the customer." },
        new TasksModel { Id = "Task 4", Status = "InProgress", Summary = "Fix the issues reported in the IE browser." },
        new TasksModel { Id = "Task 5", Status = "Close", Summary = "Validate new requirements." },
    };
}
```

## Custom Tooltip Content

Add custom HTML elements to the card and apply the `e-tooltip-text` CSS class to include them in the tooltip:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks" EnableTooltip="true">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary">
        <Template>
            @{
                TasksModel data = (TasksModel)context;
                <div class="e-card-content">
                    <p>@data.Summary</p>
                    <!-- This element's content will appear in the tooltip -->
                    <div class="e-tooltip-text">
                        <p>Assignee: @data.Assignee</p>
                        <p>Priority: @data.Priority</p>
                        <p>Type: @data.Type</p>
                    </div>
                </div>
            }
        </Template>
    </KanbanCardSettings>
</SfKanban>

@code {
    public class TasksModel
    {
        public string Id { get; set; }
        public string Status { get; set; }
        public string Summary { get; set; }
        public string Assignee { get; set; }
        public string Priority { get; set; }
        public string Type { get; set; }
    }
}
```

> Elements with the `e-tooltip-text` CSS class are hidden on the card but are displayed in the tooltip popup when hovering over the card.

## Tooltip Properties Reference

| Property | Type | Description |
|----------|------|-------------|
| `EnableTooltip` | bool | Enable/disable the built-in tooltip (default: false) |

## CSS Class for Custom Tooltip Content

| Class | Description |
|-------|-------------|
| `e-tooltip-text` | Applied to elements that should be visible in the tooltip but hidden on the card |
