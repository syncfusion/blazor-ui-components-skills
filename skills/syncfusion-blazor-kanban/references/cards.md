# Working with Cards in Blazor Kanban Component

Cards are the main elements of the Kanban board, representing task information with a header and content.

## Table of Contents

- [Header and Content](#header-and-content)
- [Tags](#tags)
- [Left Border Color (GrabberField)](#customizing-left-border-color)
- [Footer CSS Field](#rendering-custom-footer-elements)
- [Card Template](#customizing-card-layout-with-templates)
- [Card Selection](#selection)

## Header and Content

Map the `HeaderField` and `ContentField` properties in `KanbanCardSettings`:

```cshtml
<KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
```

Disable the header display with `ShowHeader="false"`:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings ShowHeader="false" HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
</SfKanban>
```

> `HeaderField` must map to a unique value in the data source to avoid duplicate card data.

## Tags

Display tag text with background color using the `TagsField` property. Multiple tags are comma-separated in the data source:

```cshtml
<KanbanCardSettings HeaderField="Id" ContentField="Summary" TagsField="CardTags"></KanbanCardSettings>
```

```csharp
// Model with tags
public class TasksModel
{
    public string Id { get; set; }
    public string Status { get; set; }
    public string Summary { get; set; }
    public List<string> CardTags { get; set; }
}

// Sample data
new TasksModel { Id = "Task 1", Status = "Open", Summary = "Analyze requirements.", CardTags = new List<string>() { "Analyze", "Customer" } }
```

## Customizing Left Border Color

Map a color field to the `GrabberField` to set a custom left border color per card:

```cshtml
<KanbanCardSettings HeaderField="Id" ContentField="Summary" GrabberField="Color"></KanbanCardSettings>
```

```csharp
new TasksModel { Id = "Task 1", Status = "Open", Summary = "Analyze requirements.", Color = "#8b447a" }
```

> Default card border left width is `3px`.

## Rendering Custom Footer Elements

Map CSS class names to the `FooterCssField` to render custom elements in the card footer:

```cshtml
<KanbanCardSettings HeaderField="Id" ContentField="Summary" FooterCssField="ClassName"></KanbanCardSettings>
```

```csharp
new TasksModel { Id = "Task 1", Status = "Open", Summary = "Analyze requirements.",
    ClassName = new List<string>() { "e-story", "e-low", "e-nancy" } }
```

Add corresponding CSS to display custom elements (icons, images) inside `.e-card-footer`.

## Customizing Card Layout with Templates

Use `Template` inside `KanbanCardSettings` to define a fully custom card layout:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
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
                    <table class="card-template-wrap">
                        <tbody>
                            <tr>
                                <td class="CardHeader">Id:</td>
                                <td>@data.Id</td>
                            </tr>
                            <tr>
                                <td class="CardHeader">Type:</td>
                                <td>@data.Type</td>
                            </tr>
                            <tr>
                                <td class="CardHeader">Priority:</td>
                                <td>@data.Priority</td>
                            </tr>
                            <tr>
                                <td class="CardHeader">Summary:</td>
                                <td>@data.Summary</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            }
        </Template>
    </KanbanCardSettings>
</SfKanban>

<style>
    .e-kanban .card-template-wrap .CardHeader { font-weight: 500; }
</style>

@code {
    public class TasksModel
    {
        public string Id { get; set; }
        public string Status { get; set; }
        public string Summary { get; set; }
        public string Type { get; set; }
        public string Priority { get; set; }
        public string Assignee { get; set; }
    }

    public List<TasksModel> Tasks = new List<TasksModel>()
    {
        new TasksModel { Id = "Task 1", Status = "Open", Summary = "Analyze the new requirements gathered from the customer.", Type = "Story", Priority = "Low", Assignee = "Nancy Davloio" },
        new TasksModel { Id = "Task 2", Status = "InProgress", Summary = "Improve application performance", Type = "Improvement", Priority = "Normal", Assignee = "Andrew Fuller" },
        new TasksModel { Id = "Task 3", Status = "Open", Summary = "Arrange a web meeting with the customer.", Type = "Others", Priority = "Critical", Assignee = "Janet Leverling" },
        new TasksModel { Id = "Task 4", Status = "InProgress", Summary = "Fix the issues reported in the IE browser.", Type = "Bug", Priority = "Release Breaker", Assignee = "Janet Leverling" },
        new TasksModel { Id = "Task 5", Status = "Review", Summary = "Fix the issues reported by the customer.", Type = "Bug", Priority = "Low", Assignee = "Steven walker" },
    };
}
```

## Selection

Control card selection behavior with `SelectionType`:

| Value | Description |
|-------|-------------|
| `None` | No cards can be selected |
| `Single` | Only one card at a time (default) |
| `Multiple` | Multiple cards can be selected |

```cshtml
<KanbanCardSettings HeaderField="Id" ContentField="Summary" SelectionType="SelectionType.Multiple"></KanbanCardSettings>
```

- **Multi-select randomly**: `Ctrl + click`
- **Multi-select range**: `Shift + click`

## KanbanCardSettings Properties

| Property | Type | Description |
|----------|------|-------------|
| `HeaderField` | string | Unique field for card header (must be unique in data source) |
| `ContentField` | string | Field displayed in card body |
| `ShowHeader` | bool | Show/hide card header (default: true) |
| `TagsField` | string | Field containing tag values (comma-separated or List) |
| `GrabberField` | string | Field providing left border color value |
| `FooterCssField` | string | Field containing CSS class names for card footer |
| `SelectionType` | `SelectionType` | Card selection mode: None, Single, Multiple |
