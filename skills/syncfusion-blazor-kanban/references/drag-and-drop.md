# Drag and Drop in Blazor Kanban Component

The Kanban component supports dragging cards between columns, swimlanes, and across external components.

## Table of Contents

- [Default Column Drag and Drop](#drag-and-drop)
- [Drag and Drop across Swimlanes](#drag-and-drop-across-swimlanes)
- [Kanban-to-Kanban Transfer](#kanban-to-kanban)
- [Kanban-to-Schedule Transfer](#kanban-to-schedule)

## Drag and Drop

Card drag and drop between columns is enabled by default. Set `AllowDragAndDrop="false"` to disable:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks" AllowDragAndDrop="false">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
</SfKanban>
```

You can also control drag/drop per column with `AllowDrag` and `AllowDrop` on `KanbanColumn`.

## Drag and Drop across Swimlanes

Enable swimlane-to-swimlane card movement with `AllowDragAndDrop` on `KanbanSwimlaneSettings`:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
    <KanbanSwimlaneSettings KeyField="Assignee" TextField="Assignee" AllowDragAndDrop="true"></KanbanSwimlaneSettings>
</SfKanban>
```

## Kanban-to-Kanban

Transfer cards between two separate Kanban boards using `ExternalDropId` and the `DragStop` event.

```cshtml
<SfKanban @ref="KanbanRef1" TValue="TasksModel" KeyField="Status" DataSource="Tasks1"
          ExternalDropId="@(new List<string>() { "kanban2" })">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
    <KanbanEvents TValue="TasksModel" DragStop="DragStop1"></KanbanEvents>
</SfKanban>

<SfKanban @ref="KanbanRef2" ID="kanban2" TValue="TasksModel" KeyField="Status" DataSource="Tasks2"
          ExternalDropId="@(new List<string>() { "kanban1" })">
    <KanbanColumns>
        <KanbanColumn HeaderText="In Review" KeyField="@(new List<string>() {"Review"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
    <KanbanEvents TValue="TasksModel" DragStop="DragStop2"></KanbanEvents>
</SfKanban>

@code {
    SfKanban<TasksModel> KanbanRef1;
    SfKanban<TasksModel> KanbanRef2;

    public List<TasksModel> Tasks1 = new List<TasksModel>() { /* ... */ };
    public List<TasksModel> Tasks2 = new List<TasksModel>() { /* ... */ };

    private async Task DragStop1(DragEventArgs<TasksModel> args)
    {
        // IsExternal is true when the card was dropped onto the other Kanban board
        if (args.IsExternal)
        {
            await KanbanRef1.DeleteCardAsync(args.Data);
            await KanbanRef2.AddCardAsync(args.Data, args.DragIndex);
            args.Cancel = true;
        }
    }

    private async Task DragStop2(DragEventArgs<TasksModel> args)
    {
        if (args.IsExternal)
        {
            await KanbanRef2.DeleteCardAsync(args.Data);
            await KanbanRef1.AddCardAsync(args.Data, args.DragIndex);
            args.Cancel = true;
        }
    }

    public class TasksModel
    {
        public string Id { get; set; }
        public string Status { get; set; }
        public string Summary { get; set; }
    }
}
```

> Use `ExternalDropId` (type `List<string>`) with the target Kanban's `ID` attribute value. In `DragStop`, use `args.IsExternal` to detect a cross-board drop. Call `DeleteCardAsync` on the source and `AddCardAsync` on the target using `args.DragIndex`, then set `args.Cancel = true` to prevent default behavior.

## Kanban-to-Schedule

Drag cards from Kanban to a Schedule component and vice versa.

```cshtml
<SfKanban @ref="KanbanObj" TValue="CardData" KeyField="DepartmentName" DataSource="CardDataList"
          ExternalDropId="@(new List<string>() { "Schedule" })">
    <KanbanColumns>
        <KanbanColumn HeaderText="SALES" KeyField="@(new List<string>() {"Sales"})"></KanbanColumn>
        <KanbanColumn HeaderText="SUPPORT" KeyField="@(new List<string>() {"Support"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
    <KanbanEvents TValue="CardData" DragStop="KanbanDragStop"></KanbanEvents>
</SfKanban>

<SfSchedule @ref="ScheduleObj" ID="Schedule" TValue="ScheduleData"
            DataSource="@ScheduleDataList" AllowDragAndDrop="true">
    <ScheduleDragAndDropSettings ExternalDropId="@(new List<string>() { "Kanban" })"></ScheduleDragAndDropSettings>
    <ScheduleEvents TValue="ScheduleData" OnActionBegin="ActionBegin"></ScheduleEvents>
    <!-- ... ScheduleViews, ScheduleEventSettings -->
</SfSchedule>

@code {
    SfKanban<CardData> KanbanObj;
    SfSchedule<ScheduleData> ScheduleObj;
    public List<CardData> CardDataList = new List<CardData>() { /* ... */ };
    public List<ScheduleData> ScheduleDataList = new List<ScheduleData>() { /* ... */ };

    private async void KanbanDragStop(DragEventArgs<CardData> args)
    {
        ScheduleData scheduleData = new ScheduleData()
        {
            Id = Convert.ToInt32(args.Data[0].Id),
            Subject = args.Data[0].Summary,
            StartTime = args.DropTime,
            EndTime = args.DropTime.AddHours(1),
        };
        await KanbanObj.DeleteCardAsync(args.Data);
        await ScheduleObj.AddEventAsync(scheduleData);
        args.Cancel = true;
    }

    private async void ActionBegin(ActionEventArgs<ScheduleData> args)
    {
        if (args.ActionType == ActionType.EventRemove && args.CancelType == CancelType.External)
        {
            CardData cardData = new CardData()
            {
                Id = args.DeletedEvents[0].Id.ToString(),
                Summary = args.DeletedEvents[0].Subject,
                DepartmentName = "Sales"
            };
            await KanbanObj.AddCardAsync(cardData);
        }
    }
}
```

## DragEventArgs Properties

| Property | Type | Description |
|----------|------|-------------|
| `Data` | `List<TValue>` | Cards being dragged |
| `DragIndex` | `int` | Index of the dragged card in the source column. Use as the insertion index for `AddCardAsync`. |
| `IsExternal` | `bool` | `true` when the card was dropped onto a different (external) Kanban component. Use this to gate cross-board logic. |
| `PreviousCardData` | `TValue` | Card data state before the drag began |
| `Left` | `double` | Client X coordinate of the drop target |
| `Top` | `double` | Client Y coordinate of the drop target |
| `Cancel` | `bool` | Set to `true` to prevent the default drop behaviour (required when manually calling `DeleteCardAsync`/`AddCardAsync`) |

### Key usage pattern for Kanban-to-Kanban

```csharp
private async Task OnDragStop(DragEventArgs<TasksModel> args)
{
    // Gate on IsExternal — fires for both internal reorders and external drops
    if (!args.IsExternal) return;

    // Mutate status to match the target board before adding
    foreach (var card in args.Data)
        card.Status = "To Do";

    await SourceKanbanRef.DeleteCardAsync(args.Data);
    await TargetKanbanRef.AddCardAsync(args.Data, args.DragIndex);
    args.Cancel = true;
}
```
