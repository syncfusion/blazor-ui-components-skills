---
name: syncfusion-blazor-kanban
description: Implement Syncfusion Blazor Kanban board component (SfKanban) for card-based workflow management. Use this when building Kanban boards, task management UIs with swimlanes, or drag-and-drop column workflows in Blazor applications. This skill covers data binding, column and card configuration, swimlane organization, drag-and-drop interactions, dialog editing, sorting, WIP validation, accessibility, localization, and responsive mode.
metadata:
  component: Kanban
  platform: blazor
  package: Syncfusion.Blazor.Kanban
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: data-visualization
---

# Implementing Syncfusion Blazor Kanban Component

## When to Use This Skill

Use this skill when:
- Building Kanban boards or workflow management UIs in Blazor
- Implementing task/card management with column-based workflows
- Adding swimlane grouping to organize cards by assignee or category
- Enabling drag-and-drop card movement between columns
- Configuring WIP (Work-In-Progress) validation limits
- Customizing card appearance with templates
- Binding Kanban to local or remote data sources
- Integrating Kanban with external components (Schedule, TreeView)

## Quick Start

### 1. Install NuGet Packages

```csharp
dotnet add package Syncfusion.Blazor.Kanban
dotnet add package Syncfusion.Blazor.Themes
```

### 2. Register Service (Program.cs)

```csharp
using Syncfusion.Blazor;
builder.Services.AddSyncfusionBlazor();
```

### 3. Add Imports (_Imports.razor)

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Kanban
```

### 4. Add Stylesheet and Script (index.html or App.razor)

```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
```

### 5. Basic Kanban with Cards

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="To Do" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Testing" KeyField="@(new List<string>() {"Testing"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Title" ContentField="Summary"></KanbanCardSettings>
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
        new TasksModel { Id = "Task 3", Title = "BLAZ-29003", Status = "Open", Summary = "Arrange a web meeting with the customer.", Assignee = "Janet Leverling" },
        new TasksModel { Id = "Task 4", Title = "BLAZ-29004", Status = "Testing", Summary = "Fix the issues reported by the customer.", Assignee = "Janet Leverling" },
        new TasksModel { Id = "Task 5", Title = "BLAZ-29005", Status = "Close", Summary = "Fix the issues reported in Safari browser.", Assignee = "Steven walker" },
    };
}
```

## Navigation Guide

| Topic | Reference File |
|-------|---------------|
| Getting started (WebAssembly/Server/Web App) | [getting-started.md](references/getting-started.md) |
| Data binding (local, remote, ExpandoObject, Observable) | [data-binding.md](references/data-binding.md) |
| Column configuration | [columns.md](references/columns.md) |
| Card customization | [cards.md](references/cards.md) |
| Drag and drop (internal & external) | [drag-and-drop.md](references/drag-and-drop.md) |
| Swimlane grouping | [swimlane.md](references/swimlane.md) |
| Card editing dialog | [dialog.md](references/dialog.md) |
| Events reference | [events.md](references/events.md) |
| Card sorting | [sort.md](references/sort.md) |
| Workflow restrictions | [workflow.md](references/workflow.md) |
| WIP validation (MinCount/MaxCount) | [validation.md](references/validation.md) |
| Styling and CSS classes | [style.md](references/style.md) |
| Accessibility (WCAG, keyboard navigation) | [accessibility.md](references/accessibility.md) |
| Localization and RTL | [localization.md](references/localization.md) |
| Tooltips | [tooltip.md](references/tooltip.md) |
| Responsive mode | [responsive-mode.md](references/responsive-mode.md) |
| Height and width dimensions | [dimensions.md](references/dimensions.md) |

## Common Patterns

### Kanban with Swimlane

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="To Do" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Title" ContentField="Summary"></KanbanCardSettings>
    <KanbanSwimlaneSettings KeyField="Assignee"></KanbanSwimlaneSettings>
</SfKanban>
```

### Kanban with WIP Validation

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})" MinCount="2"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})" MaxCount="3"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Title" ContentField="Summary"></KanbanCardSettings>
</SfKanban>
```

### Kanban with Index-Based Sorting

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="To Do" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Title" ContentField="Summary"></KanbanCardSettings>
    <KanbanSortSettings SortBy="SortOrderBy.Index" Field="RankId"></KanbanSortSettings>
</SfKanban>
```

## Key Properties Reference

| Property | Component | Description |
|----------|-----------|-------------|
| `KeyField` | `SfKanban` | Maps the data field that determines which column a card belongs to |
| `DataSource` | `SfKanban` | Binds local or remote data to the Kanban |
| `AllowDragAndDrop` | `SfKanban` | Enables/disables drag-and-drop (default: true) |
| `EnableTooltip` | `SfKanban` | Shows card details on hover |
| `EnableRtl` | `SfKanban` | Enables right-to-left layout |
| `HeaderField` | `KanbanCardSettings` | Maps the unique ID field for card headers |
| `ContentField` | `KanbanCardSettings` | Maps the field shown in card body |
| `KeyField` | `KanbanColumn` | One or more status values that map cards to this column |
| `AllowToggle` | `KanbanColumn` | Allows column expand/collapse |
| `MinCount` | `KanbanColumn` | Minimum card count for WIP validation |
| `MaxCount` | `KanbanColumn` | Maximum card count for WIP validation |
| `TransitionColumns` | `KanbanColumn` | Restricts cards to only drop into specified columns |
| `AllowDrop` | `KanbanColumn` | Prevents cards from being dropped into a column |
| `AllowDrag` | `KanbanColumn` | Prevents cards from being dragged from a column |
| `KeyField` | `KanbanSwimlaneSettings` | Groups cards into swimlane rows by this field |
| `SortBy` | `KanbanSortSettings` | Sorting mode: DataSourceOrder, Index, or Custom |
