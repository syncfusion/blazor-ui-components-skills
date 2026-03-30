# Events in Blazor Kanban Component

Kanban provides events to intercept and customize behavior at key interaction points.

**Source:** https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Kanban.KanbanEvents-1.html

## Table of Contents

- [Registering Events](#registering-events)
- [OnLoad](#onload)
- [Created](#created)
- [ActionBegin and ActionComplete](#actionbegin-and-actioncomplete)
- [ActionFailure](#actionfailure)
- [CardClick and CardDoubleClick](#cardclick-and-carddoubleclick)
- [CardRendered](#cardrendered)
- [DataBinding](#databinding)
- [DialogOpen and DialogClose](#dialogopen-and-dialogclose)
- [DragStart and DragStop](#dragstart-and-dragstop)
- [QueryCellInfo](#querycellinfo)
- [SwimlaneSorting](#swimlanesorting)
- [All Events Reference](#all-events-reference)

## Registering Events

Use the `KanbanEvents` child component to subscribe to events:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
    <KanbanEvents TValue="TasksModel"
        OnLoad="OnLoadHandler"
        ActionBegin="ActionBeginHandler"
        ActionComplete="ActionCompleteHandler"
        CardClick="CardClickHandler"
        DragStart="DragStartHandler"
        DragStop="DragStopHandler">
    </KanbanEvents>
</SfKanban>
```

## OnLoad

Fires once when the Kanban component loads (before initial render). Use this event to make initial configurations or preparations before the Kanban component is fully rendered.

**EventCallback:** `EventCallback<object>`  
**Event Arguments:** `object` (generic placeholder — no specific properties defined)

```cshtml
@using Syncfusion.Blazor.Kanban

<SfKanban TValue="TasksModel" KeyField="Status" DataSource="@Tasks">
    <KanbanEvents TValue="TasksModel" OnLoad="@OnLoadHandler"></KanbanEvents>
</SfKanban>

@code {
    public void OnLoadHandler(Object args)
    {
        // Component is initializing — make pre-render configurations here
    }
}
```

## Created

Triggers after the Kanban component is fully created. Use this event to perform setup tasks that require the component to be fully instantiated.

**EventCallback:** `EventCallback<object>`  
**Event Arguments:** `object` (generic placeholder — no specific properties defined)

```cshtml
@using Syncfusion.Blazor.Kanban

<SfKanban TValue="TasksModel" KeyField="Status" DataSource="@Tasks">
    <KanbanEvents TValue="TasksModel" Created="@CreatedHandler"></KanbanEvents>
</SfKanban>

@code {
    public void CreatedHandler(Object args)
    {
        // Component is fully created — perform post-creation setup here
    }
}
```

## ActionBegin and ActionComplete

`ActionBegin` fires before any CRUD action (drag, dialog save/delete). `ActionComplete` fires after:

```csharp
private void ActionBeginHandler(ActionEventArgs<TasksModel> args)
{
    // args.ActionType: CRUD, DragAndDrop, ColumnToggle, etc.
    // args.AddedRecords, args.ChangedRecords, args.DeletedRecords
    if (args.RequestType == "CardChanged")
    {
        // Card was dragged to another column
    }
}

private void ActionCompleteHandler(ActionEventArgs<TasksModel> args)
{
    // CRUD has completed
    Console.WriteLine($"Action complete: {args.RequestType}");
}
```

## ActionFailure

Fires when a remote data operation fails:

```csharp
private void ActionFailureHandler(ActionEventArgs<TasksModel> args)
{
    // args.Error: exception details
    Console.WriteLine($"Action failed: {args.Error.Message}");
}
```

## CardClick and CardDoubleClick

```csharp
private void CardClickHandler(CardClickEventArgs<TasksModel> args)
{
    // args.Data: the clicked card's data
    Console.WriteLine($"Card clicked: {args.Data.Id}");
}

private void CardDoubleClickHandler(CardClickEventArgs<TasksModel> args)
{
    // Default behavior: opens dialog; set args.Cancel = true to prevent
    args.Cancel = false;
}
```

## CardRendered

Fires once for each card during rendering. Use it to customize card DOM:

```csharp
private void CardRenderedHandler(CardRenderedEventArgs<TasksModel> args)
{
    // args.Data: card data
    // args.Element: the card DOM element reference
}
```

## DataBinding

Fires before the Kanban data is bound. Use this event to modify or validate data before it is presented on the board.

**EventCallback:** `EventCallback<DataBindingEventArgs<TValue>>`

**Event Arguments (`DataBindingEventArgs<TValue>`):**
- `int Count` — Gets or sets the count of cards.
- `List<TValue>? Result` — Gets or sets the result data intended for binding.

```cshtml
@using Syncfusion.Blazor.Kanban

<SfKanban TValue="TasksModel" KeyField="Status" DataSource="@Tasks">
    <KanbanEvents TValue="TasksModel" DataBinding="@DataBindingHandler"></KanbanEvents>
</SfKanban>

@code {
    public void DataBindingHandler(DataBindingEventArgs<TasksModel> args)
    {
        // args.Result: modify or filter data before rendering
        // args.Count: total number of incoming records
    }
}
```

## DialogOpen and DialogClose

**`DialogOpen`** fires before the dialog opens (cancellable). **`DialogClose`** fires before the dialog closes.

**`DialogOpen` Event Arguments (`DialogOpenEventArgs<TValue>`):**
- `bool Cancel` — Set to `true` to prevent the dialog from opening.
- `TValue? Data` — The card data associated with this dialog action.
- `CurrentAction RequestType` — The action type: `Add` or `Edit`.

**`DialogClose` Event Arguments (`DialogCloseEventArgs<TValue>`):**
- `bool Cancel` — Set to `true` to prevent the dialog from closing.
- `TValue? Data` — The card data associated with this dialog action.
- `string? Interaction` — The interaction type that triggered the dialog close.
- `CurrentAction RequestType` — The action requested by the dialog.

```cshtml
@using Syncfusion.Blazor.Kanban

<SfKanban TValue="TasksModel" KeyField="Status" DataSource="@Tasks">
    <KanbanEvents TValue="TasksModel"
        DialogOpen="@DialogOpenHandler"
        DialogClose="@DialogCloseHandler">
    </KanbanEvents>
</SfKanban>

@code {
    public void DialogOpenHandler(DialogOpenEventArgs<TasksModel> args)
    {
        // Prevent editing cards in 'Close' status
        if (args.Data?.Status == "Close")
        {
            args.Cancel = true;
        }
    }

    public void DialogCloseHandler(DialogCloseEventArgs<TasksModel> args)
    {
        Console.WriteLine($"Dialog closed. Interaction: {args.Interaction}");
    }
}
```

## DragStart and DragStop

**`DragStart`** fires when a card drag begins. **`DragStop`** fires when the drag ends (card is dropped).

**Event Arguments (`DragEventArgs<TValue>`):**
- `bool Cancel` — Set to `true` to cancel the drag or drop action.
- `List<TValue>? Data` — The card data objects being dragged.
- `int DragIndex` — The index of the dragged/dropped element.
- `bool IsExternal` — Whether the drop is to an external component.
- `double Left` — Client X coordinate of the cursor.
- `List<TValue>? PreviousCardData` — Data of the card that was previously at the drop position.
- `double Top` — Client Y coordinate of the cursor.

```cshtml
@using Syncfusion.Blazor.Kanban

<SfKanban TValue="TasksModel" KeyField="Status" DataSource="@Tasks">
    <KanbanEvents TValue="TasksModel"
        DragStart="@DragStartHandler"
        DragStop="@DragStopHandler">
    </KanbanEvents>
</SfKanban>

@code {
    public void DragStartHandler(DragEventArgs<TasksModel> args)
    {
        // args.Data: list of cards being dragged
        Console.WriteLine($"Drag started for {args.Data?.Count} card(s)");
    }

    public void DragStopHandler(DragEventArgs<TasksModel> args)
    {
        // args.Data: dropped card data; args.IsExternal: dropped outside Kanban
        Console.WriteLine($"Card dropped. External: {args.IsExternal}");
    }
}
```

## QueryCellInfo

Fires before each column cell is rendered. Use to customize column cell appearance or inject additional data.

**EventCallback:** `EventCallback<QueryCellInfoEventArgs<TValue>>`

**Event Arguments (`QueryCellInfoEventArgs<TValue>`):**
- `bool Cancel` — Set to `true` to cancel the rendering action.
- `List<SwimlaneSettingsModel>? Data` — Data associated with the cell being rendered.
- `string? RequestType` — The request type of the current action.

```cshtml
@using Syncfusion.Blazor.Kanban

<SfKanban TValue="TasksModel" KeyField="Status" DataSource="@Tasks">
    <KanbanEvents TValue="TasksModel" QueryCellInfo="@QueryCellInfoHandler"></KanbanEvents>
</SfKanban>

@code {
    public void QueryCellInfoHandler(QueryCellInfoEventArgs<TasksModel> args)
    {
        // Customize cell rendering based on column key, swimlane, etc.
    }
}
```

## SwimlaneSorting

Fires before swimlane rows are rendered/sorted. Use to manage or customize swimlane row ordering.

**EventCallback:** `EventCallback<SwimlaneSortEventArgs>`

**Event Arguments (`SwimlaneSortEventArgs`):**
- `List<SwimlaneSettingsModel>? SwimlaneRows` — Gets or sets the sorting order of swimlane rows.

```cshtml
@using Syncfusion.Blazor.Kanban

<SfKanban TValue="TasksModel" KeyField="Status" DataSource="@Tasks">
    <KanbanSwimlaneSettings KeyField="Assignee"></KanbanSwimlaneSettings>
    <KanbanEvents TValue="TasksModel" SwimlaneSorting="@SwimlaneSortingHandler"></KanbanEvents>
</SfKanban>

@code {
    public void SwimlaneSortingHandler(SwimlaneSortEventArgs args)
    {
        // args.SwimlaneRows: modify the order of swimlane rows here
    }
}
```

## All Events Reference

| Event | EventCallback Type | Cancellable | Description |
|-------|-------------------|-------------|-------------|
| `OnLoad` | `EventCallback<object>` | No | Fires when the component loads, before initial render |
| `Created` | `EventCallback<object>` | No | Fires after the component is fully created |
| `ActionBegin` | `EventCallback<ActionEventArgs<TValue>>` | Yes | Fires before any CRUD action begins |
| `ActionComplete` | `EventCallback<ActionEventArgs<TValue>>` | No | Fires after a CRUD action succeeds |
| `ActionFailure` | `EventCallback<ActionEventArgs<TValue>>` | No | Fires when a CRUD action fails |
| `CardClick` | `EventCallback<CardClickEventArgs<TValue>>` | Yes | Fires on single click of a card |
| `CardDoubleClick` | `EventCallback<CardClickEventArgs<TValue>>` | Yes | Fires on double-click of a card (opens dialog by default) |
| `CardRendered` | `EventCallback<CardRenderedEventArgs<TValue>>` | Yes | Fires before each card is rendered |
| `DataBinding` | `EventCallback<DataBindingEventArgs<TValue>>` | No | Fires before data is bound to the component |
| `DialogClose` | `EventCallback<DialogCloseEventArgs<TValue>>` | Yes | Fires before the editing dialog closes |
| `DialogOpen` | `EventCallback<DialogOpenEventArgs<TValue>>` | Yes | Fires before the editing dialog opens |
| `DragStart` | `EventCallback<DragEventArgs<TValue>>` | Yes | Fires when a card drag operation begins |
| `DragStop` | `EventCallback<DragEventArgs<TValue>>` | Yes | Fires when a card drag operation ends |
| `QueryCellInfo` | `EventCallback<QueryCellInfoEventArgs<TValue>>` | Yes | Fires before each column cell is rendered |
| `SwimlaneSorting` | `EventCallback<SwimlaneSortEventArgs>` | No | Fires before swimlane rows are sorted/rendered |
