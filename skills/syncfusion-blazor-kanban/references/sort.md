# Sorting in Blazor Kanban Component

The Kanban component allows cards within each column to be sorted using `KanbanSortSettings`.

## Table of Contents

- [Default Sort Order (DataSourceOrder)](#default-data-source-order)
- [Sort by Index](#sort-by-index-field)
- [Custom Sort Order](#custom-sort-order)
- [Sort Direction](#sort-direction)

## Default Data Source Order

By default, cards render in the same order as the data source. This is `SortBy.DataSourceOrder`:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
    <KanbanSortSettings SortBy="SortOrderBy.DataSourceOrder" Direction="SortDirection.Ascending"></KanbanSortSettings>
</SfKanban>
```

## Sort by Index Field

Sort cards by a numeric index field. Map the index field with `Field`:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
    <KanbanSortSettings SortBy="SortOrderBy.Index" Field="RankId" Direction="SortDirection.Ascending"></KanbanSortSettings>
</SfKanban>

@code {
    public class TasksModel
    {
        public string Id { get; set; }
        public string Status { get; set; }
        public string Summary { get; set; }
        public int RankId { get; set; }  // Index field for sorting
    }

    public List<TasksModel> Tasks = new List<TasksModel>()
    {
        new TasksModel { Id = "Task 1", Status = "Open", Summary = "Analyze requirements.", RankId = 1 },
        new TasksModel { Id = "Task 2", Status = "Open", Summary = "Update website.", RankId = 2 },
        new TasksModel { Id = "Task 3", Status = "InProgress", Summary = "Fix browser issues.", RankId = 1 },
    };
}
```

> When `SortBy="SortOrderBy.Index"`, drag-and-drop updates the `RankId` values automatically to maintain the new order.

## Custom Sort Order

Use `SortOrderBy.Custom` along with a custom comparer to implement your own sort logic:

```cshtml
<KanbanSortSettings SortBy="SortOrderBy.Custom" Field="Priority" Direction="SortDirection.Ascending"></KanbanSortSettings>
```

When `SortBy` is `Custom`, the `Field` property specifies which field's values are compared. You can handle the `SwimlaneSorting` event or use custom data ordering in the data source.

## Sort Direction

| Value | Description |
|-------|-------------|
| `Ascending` | Cards sorted in ascending order (default) |
| `Descending` | Cards sorted in descending order |

```cshtml
<KanbanSortSettings SortBy="SortOrderBy.Index" Field="RankId" Direction="SortDirection.Descending"></KanbanSortSettings>
```

## KanbanSortSettings Properties

| Property | Type | Description |
|----------|------|-------------|
| `SortBy` | `SortOrderBy` | Sort method: `DataSourceOrder`, `Index`, `Custom` |
| `Field` | string | Data source field used for sorting (required for `Index` and `Custom`) |
| `Direction` | `SortDirection` | Sort order: `Ascending` or `Descending` |

## SortOrderBy Enum Values

| Value | Description |
|-------|-------------|
| `DataSourceOrder` | Cards appear in the same order as the data source |
| `Index` | Cards are sorted by a numeric rank/index field |
| `Custom` | Cards are sorted using a user-defined field and custom logic |
