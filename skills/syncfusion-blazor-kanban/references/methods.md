````markdown
# SfKanban Public Methods Reference

All public methods available on the `SfKanban<TValue>` component instance accessed via `@ref`.

**Source:** https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Kanban.SfKanban-1.html#methods  
**Namespace:** `Syncfusion.Blazor.Kanban`

## Table of Contents

- [Getting a Component Reference](#getting-a-component-reference)
- [Card Management](#card-management)
  - [AddCardAsync](#addcardasync)
  - [UpdateCardAsync](#updatecardasync)
  - [DeleteCardAsync](#deletecardasync)
- [Column Management](#column-management)
  - [DeleteColumnAsync](#deletecolumnasync)
- [Data Retrieval](#data-retrieval)
  - [GetColumnDataByKeys](#getcolumndatabykeys)
  - [GetSwimlaneData](#getswimlanedata)
  - [GetTargetDetailsAsync](#gettargetdetailsasync)
- [Dialog Control](#dialog-control)
  - [OpenDialogAsync](#opendialogasync)
  - [CloseDialogAsync](#closedialogasync)
- [Board Control](#board-control)
  - [RefreshAsync](#refreshasync)
  - [ShowSpinnerAsync](#showspinnerasync)
  - [HideSpinnerAsync](#hidespinnerasync)
  - [UpdateViewDataAsync](#updateviewdataasync)
- [Methods Summary Table](#methods-summary-table)

---

## Getting a Component Reference

Use `@ref` to capture the `SfKanban` instance and call methods on it:

```razor
<SfKanban @ref="kanbanRef" TValue="TasksModel" KeyField="Status" DataSource="@Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="To Do" KeyField="@(new List<string>() { "Open" })"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() { "InProgress" })"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() { "Close" })"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Title" ContentField="Summary"></KanbanCardSettings>
</SfKanban>

@code {
    private SfKanban<TasksModel> kanbanRef;
    // Use kanbanRef to call methods such as AddCardAsync, RefreshAsync, etc.
}
```

---

## Card Management

### AddCardAsync

Adds one or more new cards to the Kanban data source.

#### Overloads

| Signature | Description |
|-----------|-------------|
| `AddCardAsync(TValue cardData, int index = 0)` | Adds a single card at the specified index. |
| `AddCardAsync(List<TValue> cardData, int index = 0)` | Adds multiple cards at the specified index. |

**Returns:** `Task`

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `cardData` | `TValue` or `List<TValue>` | Yes | The card data object(s) to add. |
| `index` | `int` | No (default: `0`) | Zero-based index position within the column to insert the card. |

```razor
<button @onclick="AddSingleCard">Add Card</button>
<button @onclick="AddMultipleCards">Add Multiple Cards</button>

@code {
    private async Task AddSingleCard()
    {
        var newCard = new TasksModel
        {
            Id = "Task 100",
            Title = "BLAZ-30001",
            Status = "Open",
            Summary = "New task added programmatically.",
            Assignee = "Nancy Davloio"
        };
        await kanbanRef.AddCardAsync(newCard, 0);
    }

    private async Task AddMultipleCards()
    {
        var newCards = new List<TasksModel>
        {
            new TasksModel { Id = "Task 101", Title = "BLAZ-30002", Status = "Open", Summary = "Task A" },
            new TasksModel { Id = "Task 102", Title = "BLAZ-30003", Status = "InProgress", Summary = "Task B" }
        };
        await kanbanRef.AddCardAsync(newCards);
    }
}
```

---

### UpdateCardAsync

Updates one or more existing cards in the Kanban data source.

#### Overloads

| Signature | Description |
|-----------|-------------|
| `UpdateCardAsync(TValue cardData, int index)` | Updates a single card at the specified index. |
| `UpdateCardAsync(List<TValue> cardData, int index)` | Updates multiple cards at the specified index. |

**Returns:** `Task`  
**Throws:** `Exception`

**Parameters:**

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `cardData` | `TValue` or `List<TValue>` | Yes | The updated card data object(s). |
| `index` | `int` | Yes | The index position at which to update. |

```razor
<button @onclick="UpdateCard">Update Card</button>

@code {
    private async Task UpdateCard()
    {
        var updatedCard = new TasksModel
        {
            Id = "Task 1",
            Title = "BLAZ-29001",
            Status = "InProgress",  // moved to a different column
            Summary = "Updated summary text.",
            Assignee = "Andrew Fuller"
        };
        await kanbanRef.UpdateCardAsync(updatedCard, 0);
    }
}
```

---

### DeleteCardAsync

Deletes one or more cards from the Kanban data source.

#### Overloads

| Signature | Description |
|-----------|-------------|
| `DeleteCardAsync(TValue cardData)` | Deletes a card using the full card data object. |
| `DeleteCardAsync(List<TValue> cardData)` | Deletes multiple cards using a list of card data objects. |
| `DeleteCardAsync(int id)` | Deletes a card by its integer primary key. |
| `DeleteCardAsync(string id)` | Deletes a card by its string primary key. |

**Returns:** `Task`

```razor
<button @onclick="DeleteByObject">Delete Card (Object)</button>
<button @onclick="DeleteById">Delete Card (ID)</button>

@code {
    private async Task DeleteByObject()
    {
        // Delete using the full data object
        await kanbanRef.DeleteCardAsync(Tasks[0]);
    }

    private async Task DeleteById()
    {
        // Delete using string ID
        await kanbanRef.DeleteCardAsync("Task 1");
    }
}
```

---

## Column Management

### DeleteColumnAsync

Deletes a column from the Kanban board at the specified index.

**Signature:** `DeleteColumnAsync(int index)`  
**Returns:** `Task`

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `index` | `int` | Yes | Zero-based index of the column to delete. |

```razor
<button @onclick="RemoveLastColumn">Remove Last Column</button>

@code {
    private async Task RemoveLastColumn()
    {
        // Delete the last column (index 3 for a 4-column board)
        await kanbanRef.DeleteColumnAsync(3);
    }
}
```

---

## Data Retrieval

### GetColumnDataByKeys

Returns all card data records belonging to the columns matching the provided key field values.

**Signature:** `GetColumnDataByKeys(List<string> keys)`  
**Returns:** `List<TValue>`

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `keys` | `List<string>` | Yes | The column key field values to retrieve data for. |

```razor
<button @onclick="GetOpenCards">Get Open Column Data</button>

@code {
    private void GetOpenCards()
    {
        var openCards = kanbanRef.GetColumnDataByKeys(new List<string> { "Open" });
        Console.WriteLine($"Open cards count: {openCards.Count}");

        // Get data from multiple column keys
        var activeCards = kanbanRef.GetColumnDataByKeys(new List<string> { "Open", "InProgress" });
    }
}
```

---

### GetSwimlaneData

Returns all card data records belonging to the specified swimlane row.

**Signature:** `GetSwimlaneData(string keyField)`  
**Returns:** `List<TValue>`

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `keyField` | `string` | Yes | The swimlane key field value (e.g., an assignee name) to retrieve data for. |

```razor
<button @onclick="GetAssigneeData">Get Assignee's Cards</button>

@code {
    private void GetAssigneeData()
    {
        var assigneeCards = kanbanRef.GetSwimlaneData("Nancy Davloio");
        Console.WriteLine($"Cards assigned to Nancy: {assigneeCards.Count}");
    }
}
```

---

### GetTargetDetailsAsync

Returns card and column details based on the mouse cursor's left/top screen coordinates. Useful for custom drag-and-drop integrations.

**Signature:** `GetTargetDetailsAsync(double left, double top)`  
**Returns:** `Task<KanbanTargetDetails<TValue>>`

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `left` | `double` | Yes | The client X (horizontal) coordinate. |
| `top` | `double` | Yes | The client Y (vertical) coordinate. |

**Sub-type: `KanbanTargetDetails<TValue>` Properties**

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ColumnKeyField` | `string?` | `""` | Gets or sets the column key field based on the mouse cursor position during drag-and-drop. |
| `CurrentCardId` | `string?` | `null` | Gets or sets the current card ID based on the cursor position. |
| `Index` | `int` | `0` | Gets or sets the current card position (index) in the column. |
| `PreviousCardData` | `List<TValue>?` | `null` | Gets or sets the data of the previous card; used to retrieve state during reordering. |
| `PreviousCardId` | `string?` | `null` | Gets or sets the previous card ID based on cursor position; crucial when `SortOrderBy` is `Index`. |
| `SwimlaneKeyField` | `string?` | `null` | Gets or sets the swimlane key field based on current mouse position. |

```razor
@code {
    private async Task OnCustomDrop(double clientX, double clientY)
    {
        var target = await kanbanRef.GetTargetDetailsAsync(clientX, clientY);
        Console.WriteLine($"Column: {target.ColumnKeyField}");
        Console.WriteLine($"Card at position: {target.CurrentCardId} (index {target.Index})");
        Console.WriteLine($"Previous card: {target.PreviousCardId}");
        Console.WriteLine($"Swimlane: {target.SwimlaneKeyField}");
    }
}
```

---

## Dialog Control

### OpenDialogAsync

Programmatically opens the card editing dialog for a given action and card data.

**Signature:** `OpenDialogAsync(CurrentAction action, TValue data)`  
**Returns:** `Task`

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `action` | `CurrentAction` | Yes | The dialog action: `Add`, `Edit`, or `Delete`. |
| `data` | `TValue` | Yes | The card data to display/edit in the dialog. |

```razor
<button @onclick="OpenAddDialog">Add New Card via Dialog</button>
<button @onclick="() => OpenEditDialog(Tasks[0])">Edit First Card</button>

@code {
    private async Task OpenAddDialog()
    {
        await kanbanRef.OpenDialogAsync(CurrentAction.Add, new TasksModel());
    }

    private async Task OpenEditDialog(TasksModel card)
    {
        await kanbanRef.OpenDialogAsync(CurrentAction.Edit, card);
    }
}
```

---

### CloseDialogAsync

Programmatically closes the card editing dialog.

**Signature:** `CloseDialogAsync()`  
**Returns:** `Task`

```razor
<button @onclick="CloseDialog">Close Dialog</button>

@code {
    private async Task CloseDialog()
    {
        await kanbanRef.CloseDialogAsync();
    }
}
```

---

## Board Control

### RefreshAsync

Refreshes the entire Kanban board, re-rendering the header and content to reflect the latest data state.

**Signature:** `RefreshAsync()`  
**Returns:** `Task`  
**Throws:** `Exception`

```razor
<button @onclick="RefreshBoard">Refresh Board</button>

@code {
    private async Task RefreshBoard()
    {
        await kanbanRef.RefreshAsync();
    }
}
```

---

### ShowSpinnerAsync

Manually displays the loading spinner over the Kanban board to indicate a processing operation.

**Signature:** `ShowSpinnerAsync()`  
**Returns:** `Task`  
**Throws:** `Exception`

```razor
@code {
    private async Task LoadData()
    {
        await kanbanRef.ShowSpinnerAsync();
        // Perform long-running operation
        await FetchRemoteData();
        await kanbanRef.HideSpinnerAsync();
    }
}
```

---

### HideSpinnerAsync

Manually hides the loading spinner.

**Signature:** `HideSpinnerAsync()`  
**Returns:** `Task`

```razor
@code {
    private async Task HideSpinner()
    {
        await kanbanRef.HideSpinnerAsync();
    }
}
```

---

### UpdateViewDataAsync

Asynchronously replaces the Kanban board's current data with a new data set.

**Signature:** `UpdateViewDataAsync(IEnumerable<TValue> data)`  
**Returns:** `Task`  
**Throws:** `Exception`

| Name | Type | Required | Description |
|------|------|----------|-------------|
| `data` | `IEnumerable<TValue>` | Yes | The new data set to render on the board. |

```razor
<button @onclick="SwitchDataSet">Switch Data Set</button>

@code {
    private async Task SwitchDataSet()
    {
        var newData = await FetchUpdatedTasksFromServer();
        await kanbanRef.UpdateViewDataAsync(newData);
    }
}
```

---

## Methods Summary Table

| Method | Returns | Description |
|--------|---------|-------------|
| `AddCardAsync(TValue, int)` | `Task` | Add a single card at an index |
| `AddCardAsync(List<TValue>, int)` | `Task` | Add multiple cards at an index |
| `UpdateCardAsync(TValue, int)` | `Task` | Update a single card |
| `UpdateCardAsync(List<TValue>, int)` | `Task` | Update multiple cards |
| `DeleteCardAsync(TValue)` | `Task` | Delete card by data object |
| `DeleteCardAsync(List<TValue>)` | `Task` | Delete multiple cards by data objects |
| `DeleteCardAsync(int)` | `Task` | Delete card by integer ID |
| `DeleteCardAsync(string)` | `Task` | Delete card by string ID |
| `DeleteColumnAsync(int)` | `Task` | Delete a column by index |
| `GetColumnDataByKeys(List<string>)` | `List<TValue>` | Retrieve cards in specified columns |
| `GetSwimlaneData(string)` | `List<TValue>` | Retrieve cards in a swimlane row |
| `GetTargetDetailsAsync(double, double)` | `Task<KanbanTargetDetails<TValue>>` | Get card/column at screen coordinates |
| `OpenDialogAsync(CurrentAction, TValue)` | `Task` | Open card dialog programmatically |
| `CloseDialogAsync()` | `Task` | Close card dialog programmatically |
| `RefreshAsync()` | `Task` | Re-render the full board |
| `ShowSpinnerAsync()` | `Task` | Show the loading spinner |
| `HideSpinnerAsync()` | `Task` | Hide the loading spinner |
| `UpdateViewDataAsync(IEnumerable<TValue>)` | `Task` | Replace board data with a new data set |
````
