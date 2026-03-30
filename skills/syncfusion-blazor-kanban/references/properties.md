````markdown
# SfKanban Properties Reference

All configurable properties for the `SfKanban<TValue>` component and its sub-components.

**Source:** https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Kanban.SfKanban-1.html#properties  

## Table of Contents

- [SfKanban Core Properties](#sfkanban-core-properties)
- [KanbanCardSettings](#kanbancards settings)
- [KanbanColumn](#kanbancolumn)
- [KanbanDialogSettings](#kanbandialog settings)
- [KanbanSwimlaneSettings](#kanbanswimlanesettings)
- [KanbanSortSettings](#kanbansortsettings)
- [KanbanStackedHeader](#kanbanstackedheader)
- [ConstraintType Enum](#constrainttype-enum)

---

## SfKanban Core Properties

### AllowDragAndDrop
**Type:** `[Parameter] public bool AllowDragAndDrop { get; set; }`  
**Default:** `true`  
Gets or sets a value indicating whether drag and drop actions are enabled in the Kanban.

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks"
          AllowDragAndDrop="false">
</SfKanban>
```

---

### AllowKeyboard
**Type:** `[Parameter] public bool AllowKeyboard { get; set; }`  
**Default:** `true`  
Gets or sets a value indicating whether keyboard interaction is enabled in the Kanban board.

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks"
          AllowKeyboard="true">
</SfKanban>
```

---

### ConstraintType
**Type:** `[Parameter] public ConstraintType? ConstraintType { get; set; }`  
**Default:** `null`  
Defines the constraint type used to apply WIP validation based on column or swimlane. Possible values: `Column` and `Swimlane`.

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks"
          ConstraintType="ConstraintType.Column">
</SfKanban>
```

---

### CssClass
**Type:** `[Parameter] public string? CssClass { get; set; }`  
**Default:** `null`  
Used to customize the Kanban by applying custom CSS class names for specific styles and themes.

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks"
          CssClass="custom-kanban">
</SfKanban>
```

---

### DataSource
**Type:** `[Parameter] [JsonIgnore] public IEnumerable<TValue>? DataSource { get; set; }`  
**Default:** `null`  
Binds the list items either through local or remote service and assigns them to the component.

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="@Tasks">
</SfKanban>
```

---

### DataSourceChanged
**Type:** `[Parameter] public EventCallback<IEnumerable<TValue>>? DataSourceChanged { get; set; }`  
**Default:** `null`  
Invoked when the data source changes. Use for two-way data binding.

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="@Tasks"
          DataSourceChanged="@OnDataSourceChanged">
</SfKanban>

@code {
    private void OnDataSourceChanged(IEnumerable<TasksModel> data)
    {
        Tasks = data.ToList();
    }
}
```

---

### EnableRtl
**Type:** `[Parameter] public bool EnableRtl { get; set; }`  
**Default:** `false`  
Enables or disables rendering of the component in the right-to-left direction.

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks"
          EnableRtl="true">
</SfKanban>
```

---

### EnableTooltip
**Type:** `[Parameter] public bool EnableTooltip { get; set; }`  
**Default:** `false`  
Gets or sets a value indicating whether tooltips are enabled in the Kanban board. Shows card details on hover.

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks"
          EnableTooltip="true">
</SfKanban>
```

---

### ExternalDropId
**Type:** `[Parameter] public List<string>? ExternalDropId { get; set; }`  
**Default:** `null`  
Defines the IDs of external drop target components (e.g., another Kanban or Scheduler) on which cards can be dropped.

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks"
          ExternalDropId="@(new List<string>() { "ScheduleBoard" })">
</SfKanban>
```

---

### Height
**Type:** `[Parameter] public string? Height { get; set; }`  
**Default:** `"auto"`  
Specifies the height of the Kanban component. Accepts string values like `"500px"` or `"100%"`.

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks"
          Height="600px">
</SfKanban>
```

---

### ID
**Type:** `[Parameter] public string? ID { get; set; }`  
**Default:** `null`  
Gets or sets the ID of the Kanban component. Required when using `ExternalDropId` across components.

```razor
<SfKanban ID="KanbanBoard1" TValue="TasksModel" KeyField="Status" DataSource="Tasks">
</SfKanban>
```

---

### KeyField
**Type:** `[Parameter] public string? KeyField { get; set; }`  
**Default:** (none)  
**Required.** Defines the key field of the Kanban board. This field determines which column a card belongs to, matching its value against the `KeyField` values defined in each `KanbanColumn`.

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
</SfKanban>
```

---

### Locale
**Type:** `[Parameter] public string? Locale { get; set; }`  
**Default:** `"en-US"`  
Gets or sets the locale of the Kanban component for localization/internationalization.

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks"
          Locale="fr-FR">
</SfKanban>
```

---

### Query
**Type:** `public Query? Query { get; set; }`  
**Default:** `null`  
Defines the query used for filtering/sorting data when binding from a remote data source.

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="@RemoteData"
          Query="@KanbanQuery">
    ...
</SfKanban>

@code {
    private Query KanbanQuery = new Query().Where("Status", "notequal", "Archived");
}
```

---

## KanbanCardSettings

Configured via the `<KanbanCardSettings>` child component or the `CardSettings` property. Defines card header, content, template, and tooltip behavior.

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ContentField` | `string?` | `null` | Maps the data field displayed in the card body. |
| `EnableTooltip` | `bool?` | `null` | Enables or disables tooltip for individual cards. Overrides the board-level `EnableTooltip`. |
| `HeaderField` | `string?` | `null` | Maps the data field displayed in the card header (unique identifier). |
| `ShowHeader` | `bool?` | `true` | Shows or hides the card header. |
| `Template` | `string?` | `null` | Gets or sets a custom Razor template for the card content. |

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanCardSettings HeaderField="Title" ContentField="Summary" ShowHeader="true">
    </KanbanCardSettings>
</SfKanban>
```

**With custom card template:**

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanCardSettings HeaderField="Title">
        <Template>
            @{
                var task = (context as TasksModel);
                <div class="card-template">
                    <div class="card-header">@task.Title</div>
                    <div class="card-body">@task.Summary</div>
                    <div class="card-footer">Assignee: @task.Assignee</div>
                </div>
            }
        </Template>
    </KanbanCardSettings>
</SfKanban>
```

---

## KanbanColumn

Configured via `<KanbanColumn>` inside `<KanbanColumns>`. Defines each column of the board.

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `AllowAdding` | `bool` | `false` | Enables or disables the "Add card" button at the top of the column. |
| `AllowDrag` | `bool` | `true` | Enables or disables dragging of cards out of this column. |
| `AllowDrop` | `bool` | `true` | Enables or disables dropping of cards into this column. |
| `AllowToggle` | `bool` | `true` | Enables or disables expand/collapse toggle for this column. |
| `HeaderText` | `string?` | `null` | The display text for the column header. |
| `KeyField` | `List<string>` | (required) | The status values that map cards to this column. Supports multiple values. |
| `MaxCount` | `int?` | `null` | Maximum card count allowed (WIP limit). Shows a warning when exceeded. |
| `MinCount` | `int?` | `null` | Minimum card count required (WIP limit). Shows a warning when below threshold. |
| `ShowItemCount` | `bool` | `true` | Shows or hides the item count badge in the column header. |
| `Template` | `string?` | `null` | Custom Razor template for the column header. |
| `TransitionColumns` | `List<string>?` | `null` | Restricts which columns a card can be dropped into from this column. |

```razor
<KanbanColumns>
    <KanbanColumn HeaderText="Backlog"
                  KeyField="@(new List<string>() { "Open" })"
                  AllowAdding="true"
                  MinCount="2">
    </KanbanColumn>
    <KanbanColumn HeaderText="In Progress"
                  KeyField="@(new List<string>() { "InProgress" })"
                  MaxCount="3"
                  AllowToggle="true">
    </KanbanColumn>
    <KanbanColumn HeaderText="Done"
                  KeyField="@(new List<string>() { "Close" })"
                  AllowDrag="false">
    </KanbanColumn>
</KanbanColumns>
```

---

## KanbanDialogSettings

Configured via the `<KanbanDialogSettings>` child component. Controls the card editing dialog behavior.

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `AllowDragging` | `bool` | `false` | Enables or disables dragging the dialog window. |
| `AnimationSettings` | `DialogAnimationSettings?` | `null` | Animation settings (effect, duration, delay) for dialog open/close. |
| `CssClass` | `string?` | `null` | Custom CSS class for the dialog element. |
| `EnableResize` | `bool` | `false` | Enables or disables resizing the dialog window. |
| `Fields` | `string?` | `null` | Specifies the fields to show in the auto-generated dialog form. |
| `ShowCloseIcon` | `bool` | `true` | Shows or hides the close (X) icon in the dialog header. |
| `Template` | `string?` | `null` | Custom Razor template for the entire dialog content. |

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanDialogSettings AllowDragging="true" EnableResize="true">
    </KanbanDialogSettings>
</SfKanban>
```

**With custom dialog template:**

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanDialogSettings>
        <Template>
            @{
                var task = (context as TasksModel);
                <div>
                    <label>Title: <input @bind="task.Title" /></label>
                    <label>Status: <input @bind="task.Status" /></label>
                </div>
            }
        </Template>
    </KanbanDialogSettings>
</SfKanban>
```

---

## KanbanSwimlaneSettings

Configured via the `<KanbanSwimlaneSettings>` child component. Groups cards into horizontal swimlane rows.

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `AllowDragAndDrop` | `bool` | `true` | Enables or disables drag-and-drop between different swimlane rows. |
| `KeyField` | `string?` | `null` | The data field used to group cards into swimlane rows. |
| `ShowEmptySwimlane` | `bool` | `false` | Shows or hides swimlane rows that have no cards. |
| `Template` | `string?` | `null` | Custom Razor template for swimlane row headers. |

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanSwimlaneSettings KeyField="Assignee"
                            ShowEmptySwimlane="true"
                            AllowDragAndDrop="true">
    </KanbanSwimlaneSettings>
</SfKanban>
```

---

## KanbanSortSettings

Configured via the `<KanbanSortSettings>` child component. Controls the ordering of cards within columns.

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Direction` | `SortDirection` | `Ascending` | Sort direction: `Ascending` or `Descending`. |
| `Field` | `string?` | `null` | The data field used for sorting cards. Required when `SortBy` is `Index` or `Custom`. |
| `SortBy` | `SortOrderBy` | `DataSourceOrder` | Sorting mode: `DataSourceOrder`, `Index`, or `Custom`. |

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanSortSettings SortBy="SortOrderBy.Index"
                        Field="RankId"
                        Direction="SortDirection.Ascending">
    </KanbanSortSettings>
</SfKanban>
```

---

## KanbanStackedHeader

Configured via `<KanbanStackedHeader>` inside `<KanbanStackedHeaders>`. Groups multiple columns under a shared header label.

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `HeaderText` | `string?` | `null` | Display text shown in the stacked header cell. |
| `KeyField` | `string?` | `null` | Comma-separated column `KeyField` values this stacked header spans. |

```razor
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="Open" KeyField="@(new List<string>() { "Open" })"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() { "InProgress" })"></KanbanColumn>
        <KanbanColumn HeaderText="Testing" KeyField="@(new List<string>() { "Testing" })"></KanbanColumn>
        <KanbanColumn HeaderText="Close" KeyField="@(new List<string>() { "Close" })"></KanbanColumn>
    </KanbanColumns>
    <KanbanStackedHeaders>
        <KanbanStackedHeader HeaderText="To Do" KeyField="Open"></KanbanStackedHeader>
        <KanbanStackedHeader HeaderText="Development" KeyField="InProgress,Testing"></KanbanStackedHeader>
        <KanbanStackedHeader HeaderText="Done" KeyField="Close"></KanbanStackedHeader>
    </KanbanStackedHeaders>
</SfKanban>
```

---

## ConstraintType Enum

Defines the scope at which WIP (Work-In-Progress) validation limits are enforced.

| Value | Description |
|-------|-------------|
| `Column` | WIP constraint (`MinCount`/`MaxCount`) is validated per column across all swimlane rows combined. |
| `Swimlane` | WIP constraint is validated per column per swimlane row independently. |

```razor
<!-- Column-level WIP validation (default) -->
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks"
          ConstraintType="ConstraintType.Column">
    <KanbanColumns>
        <KanbanColumn HeaderText="In Progress"
                      KeyField="@(new List<string>() { "InProgress" })"
                      MaxCount="5">
        </KanbanColumn>
    </KanbanColumns>
</SfKanban>

<!-- Swimlane-level WIP validation -->
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks"
          ConstraintType="ConstraintType.Swimlane">
    <KanbanSwimlaneSettings KeyField="Assignee"></KanbanSwimlaneSettings>
    <KanbanColumns>
        <KanbanColumn HeaderText="In Progress"
                      KeyField="@(new List<string>() { "InProgress" })"
                      MaxCount="3">
        </KanbanColumn>
    </KanbanColumns>
</SfKanban>
```

---

## Quick Reference Table

| Property | Component | Type | Default | Description |
|----------|-----------|------|---------|-------------|
| `AllowDragAndDrop` | `SfKanban` | `bool` | `true` | Enable/disable drag-and-drop |
| `AllowKeyboard` | `SfKanban` | `bool` | `true` | Enable/disable keyboard navigation |
| `ConstraintType` | `SfKanban` | `ConstraintType?` | `null` | WIP validation scope (Column/Swimlane) |
| `CssClass` | `SfKanban` | `string?` | `null` | Custom CSS class for the board |
| `DataSource` | `SfKanban` | `IEnumerable<TValue>?` | `null` | Local or remote data |
| `EnableRtl` | `SfKanban` | `bool` | `false` | Right-to-left rendering |
| `EnableTooltip` | `SfKanban` | `bool` | `false` | Show card tooltips on hover |
| `ExternalDropId` | `SfKanban` | `List<string>?` | `null` | IDs of external drop targets |
| `Height` | `SfKanban` | `string?` | `"auto"` | Component height |
| `ID` | `SfKanban` | `string?` | `null` | Component identifier |
| `KeyField` | `SfKanban` | `string?` | (required) | Field that maps cards to columns |
| `Locale` | `SfKanban` | `string?` | `"en-US"` | Localization culture |
| `Query` | `SfKanban` | `Query?` | `null` | Remote data filter/sort query |
| `HeaderField` | `KanbanCardSettings` | `string?` | `null` | Card header data field |
| `ContentField` | `KanbanCardSettings` | `string?` | `null` | Card body data field |
| `ShowHeader` | `KanbanCardSettings` | `bool?` | `true` | Show/hide card headers |
| `KeyField` | `KanbanColumn` | `List<string>` | (required) | Status values for this column |
| `HeaderText` | `KanbanColumn` | `string?` | `null` | Column header display text |
| `MaxCount` | `KanbanColumn` | `int?` | `null` | WIP maximum limit |
| `MinCount` | `KanbanColumn` | `int?` | `null` | WIP minimum limit |
| `AllowToggle` | `KanbanColumn` | `bool` | `true` | Expand/collapse column |
| `KeyField` | `KanbanSwimlaneSettings` | `string?` | `null` | Swimlane grouping field |
| `ShowEmptySwimlane` | `KanbanSwimlaneSettings` | `bool` | `false` | Show empty swimlane rows |
| `SortBy` | `KanbanSortSettings` | `SortOrderBy` | `DataSourceOrder` | Card sort mode |
| `Field` | `KanbanSortSettings` | `string?` | `null` | Field used for index/custom sort |
````
