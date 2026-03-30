# Dimensions in Blazor Kanban Component

Control the width and height of the Kanban component using the `Width` and `Height` properties.

## Table of Contents

- [Auto Dimensions](#auto-dimensions)
- [Pixel Dimensions](#pixel-dimensions)
- [Percentage Dimensions](#percentage-dimensions)

## Auto Dimensions

By default, both `Width` and `Height` are set to `"auto"`, which makes the Kanban fit its content and parent container:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks"
          Width="auto" Height="auto">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
</SfKanban>
```

- `Width="auto"`: Expands to fit the parent container width
- `Height="auto"`: Expands to fit the total card content (no vertical scroll)

## Pixel Dimensions

Set fixed pixel dimensions for a fixed-size board:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks"
          Width="650px" Height="550px">
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

> When `Height` is set to a fixed pixel value, the content area scrolls vertically when cards exceed the visible area.

## Percentage Dimensions

Set dimensions as a percentage of the parent container:

```cshtml
<div style="width: 100%; height: 600px;">
    <SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks"
              Width="100%" Height="100%">
        <KanbanColumns>
            <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
            <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
            <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
        </KanbanColumns>
        <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
    </SfKanban>
</div>
```

> When using percentage `Height`, ensure the parent container has a defined height. Without a parent height, `100%` may resolve to `0`.

## Dimensions Properties Reference

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Width` | string | `"auto"` | Width of the Kanban board (px, %, or `auto`) |
| `Height` | string | `"auto"` | Height of the Kanban board (px, %, or `auto`) |

## Dimension Value Examples

| Value | Effect |
|-------|--------|
| `"auto"` | Fits content/parent container |
| `"650px"` | Fixed 650-pixel width or height |
| `"100%"` | Fills 100% of parent container dimension |
| `"80vh"` | Sets height to 80% of the viewport height |

## Frozen Swimlane Rows with Fixed Height

When using `EnableFrozenRows` on swimlanes, a fixed height is required for the scroll behavior to work:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks" Height="500px">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
    <KanbanSwimlaneSettings KeyField="Assignee" EnableFrozenRows="true"></KanbanSwimlaneSettings>
</SfKanban>
```
