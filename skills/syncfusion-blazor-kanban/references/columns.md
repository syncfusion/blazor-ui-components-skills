# Columns in Blazor Kanban Component

Kanban columns represent each stage of the workflow process. Column definitions serve as the schema for the Kanban board's `DataSource`.

## Single-Key Mapping

Map a single data value to a column using the `KeyField` property:

```cshtml
@using Syncfusion.Blazor.Kanban

<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Testing" KeyField="@(new List<string>() {"Testing"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
</SfKanban>

@code {
    public class TasksModel
    {
        public string Id { get; set; }
        public string Title { get; set; }
        public string Status { get; set; }
        public string Summary { get; set; }
        public string Assignee { get; set; }
    }

    public List<TasksModel> Tasks = new List<TasksModel>()
    {
        new TasksModel { Id = "Task 1", Title = "BLAZ-29001", Status = "Open", Summary = "Analyze the new requirements gathered from the customer.", Assignee = "Nancy Davloio" },
        new TasksModel { Id = "Task 2", Title = "BLAZ-29002", Status = "InProgress", Summary = "Improve application performance", Assignee = "Andrew Fuller" },
        new TasksModel { Id = "Task 3", Title = "BLAZ-29003", Status = "Open", Summary = "Arrange a web meeting with the customer to get new requirements.", Assignee = "Janet Leverling" },
        new TasksModel { Id = "Task 4", Title = "BLAZ-29004", Status = "InProgress", Summary = "Fix the issues reported in the IE browser.", Assignee = "Janet Leverling" },
        new TasksModel { Id = "Task 5", Title = "BLAZ-29005", Status = "Review", Summary = "Fix the issues reported by the customer.", Assignee = "Steven walker" },
    };
}
```

> The `KeyField` property is required to render columns on the Kanban board.

## Multi-Key Mapping

Render a single column with multiple key values:

```cshtml
<KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open", "Validate"})"></KanbanColumn>
```

## Toggle Columns

Enable expand/collapse on columns with `AllowToggle`:

```cshtml
<SfKanban KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})" AllowToggle="true"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})" AllowToggle="true"></KanbanColumn>
        <KanbanColumn HeaderText="Testing" KeyField="@(new List<string>() {"Testing"})" AllowToggle="true"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})" AllowToggle="true"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
</SfKanban>
```

> By default, collapsed column width is `50px`.

## Initially Collapsed Column

Use `IsExpanded="false"` to render a column collapsed on load. Requires `AllowToggle="true"`:

```cshtml
<KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})" AllowToggle="true" IsExpanded="false"></KanbanColumn>
<KanbanColumn HeaderText="Testing" KeyField="@(new List<string>() {"Testing"})" AllowToggle="true" IsExpanded="false"></KanbanColumn>
```

## Header Template

Customize column headers using a `Template`:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})">
            <Template>
                @{
                    KanbanColumn column = (KanbanColumn)context;
                    <div class="header-template-wrap">
                        <div class="header-icon e-icons @column.KeyField[0]"></div>
                        <div class="header-text">@column.HeaderText</div>
                    </div>
                }
            </Template>
        </KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
</SfKanban>
```

## Stacked Headers

Group related columns under a common category:

```cshtml
<SfKanban KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="To Do" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Testing" KeyField="@(new List<string>() {"Testing"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanStackedHeaders>
        <KanbanStackedHeader Text="To Do" KeyFields="@(new List<string>() {"Open"})"></KanbanStackedHeader>
        <KanbanStackedHeader Text="Development Phase" KeyFields="@(new List<string>() {"InProgress", "Testing"})"></KanbanStackedHeader>
        <KanbanStackedHeader Text="Done" KeyFields="@(new List<string>() {"Close"})"></KanbanStackedHeader>
    </KanbanStackedHeaders>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
</SfKanban>
```

## Column Properties Reference

| Property | Type | Description |
|----------|------|-------------|
| `HeaderText` | string | Display text for the column header |
| `KeyField` | `List<string>` | One or more status values mapped to this column |
| `AllowToggle` | bool | Enables expand/collapse toggle icon |
| `IsExpanded` | bool | Controls initial expanded/collapsed state (requires `AllowToggle`) |
| `AllowAdding` | bool | Shows an add card button in the column |
| `ShowItemCount` | bool | Shows total card count in the column header |
| `MinCount` | int | Minimum WIP limit (see validation.md) |
| `MaxCount` | int | Maximum WIP limit (see validation.md) |
| `TransitionColumns` | `List<string>` | Restricts cards to only move to specified columns |
| `AllowDrop` | bool | Prevents cards from being dropped into this column |
| `AllowDrag` | bool | Prevents cards from being dragged from this column |
