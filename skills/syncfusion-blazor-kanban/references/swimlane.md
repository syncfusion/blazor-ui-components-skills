# Swimlane in Blazor Kanban Component

Swimlanes allow you to categorize cards into horizontal rows, grouping tasks by a specific field such as assignee.

## Table of Contents

- [Render Swimlane Rows](#render-swimlane-rows)
- [Custom Swimlane Text](#custom-swimlane-text)
- [Swimlane Template](#swimlane-template)
- [Sorting Swimlane Rows](#sorting)
- [Custom Sorting Order](#custom-sorting-order)
- [Display Card Count](#display-card-count)
- [Enable Frozen Rows](#enable-frozen-rows)

## Render Swimlane Rows

Use `KanbanSwimlaneSettings` with `KeyField` to group cards into swimlane rows:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
    <KanbanSwimlaneSettings KeyField="Assignee"></KanbanSwimlaneSettings>
</SfKanban>
```

## Custom Swimlane Text

Use `TextField` to display a human-readable label different from `KeyField`:

```cshtml
<KanbanSwimlaneSettings KeyField="AssigneeId" TextField="AssigneeName"></KanbanSwimlaneSettings>
```

## Swimlane Template

Customize the swimlane header row using the `Template` child:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="Open" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
    <KanbanSwimlaneSettings KeyField="Assignee" TextField="Assignee">
        <Template>
            @{
                SwimlaneSettingsModel swimlane = (SwimlaneSettingsModel)context;
                <div class="swimlane-template e-swimlane-template-table">
                    <span class="swimlane-name">@swimlane.KeyField</span>
                </div>
            }
        </Template>
    </KanbanSwimlaneSettings>
</SfKanban>
```

## Sorting

Control swimlane row sort order using `SortDirection`:

```cshtml
<KanbanSwimlaneSettings KeyField="Assignee" SortDirection="SortDirection.Descending"></KanbanSwimlaneSettings>
```

| Value | Description |
|-------|-------------|
| `Ascending` | Sort swimlane rows A–Z (default) |
| `Descending` | Sort swimlane rows Z–A |

You can also sort dynamically using the `SwimlaneSorting` event:

```cshtml
<KanbanEvents TValue="TasksModel" SwimlaneSorting="SortHandler"></KanbanEvents>

@code {
    private void SortHandler(SwimlaneSortEventArgs args)
    {
        args.Cancel = true; // Prevent default sorting
        // Apply custom sorting logic here
    }
}
```

## Custom Sorting Order

Provide a specific swimlane row order using `SwimlaneData`:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
    <KanbanSwimlaneSettings KeyField="Assignee" TextField="Assignee"
                            SwimlaneData="SwimlaneItems">
    </KanbanSwimlaneSettings>
</SfKanban>

@code {
    public List<SwimlaneData> SwimlaneItems = new List<SwimlaneData>()
    {
        new SwimlaneData() { KeyField = "Andrew Fuller", TextField = "Andrew Fuller" },
        new SwimlaneData() { KeyField = "Janet Leverling", TextField = "Janet Leverling" },
        new SwimlaneData() { KeyField = "Nancy Davloio", TextField = "Nancy Davloio" },
    };
}
```

## Display Card Count

Show the number of cards per swimlane using `ShowItemCount`:

```cshtml
<KanbanSwimlaneSettings KeyField="Assignee" ShowItemCount="true"></KanbanSwimlaneSettings>
```

## Enable Frozen Rows

Keep swimlane header rows visible while scrolling with `EnableFrozenRows`:

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

> Set a fixed `Height` on the Kanban to enable scrolling for frozen rows to work.

## KanbanSwimlaneSettings Properties

| Property | Type | Description |
|----------|------|-------------|
| `KeyField` | string | Data source field for grouping cards into swimlane rows |
| `TextField` | string | Field used for swimlane row header display text |
| `AllowDragAndDrop` | bool | Enable dragging cards across swimlane rows (default: false) |
| `ShowItemCount` | bool | Show card count on each swimlane row header |
| `ShowEmptyRow` | bool | Show swimlane rows even when they have no cards |
| `SortDirection` | `SortDirection` | Default sort direction for swimlane rows |
| `SwimlaneData` | `List<SwimlaneData>` | Custom swimlane row order |
| `EnableFrozenRows` | bool | Freeze swimlane header rows when scrolling |
