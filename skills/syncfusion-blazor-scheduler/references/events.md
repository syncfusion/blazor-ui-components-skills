---
layout: post
title: Events in Blazor Scheduler Component | Syncfusion
description: Learn here all about available Scheduler Events and customizing Syncfusion Blazor Scheduler component and performing actions within it.
platform: Blazor
control: Scheduler
documentation: ug
---

# Events in Blazor Scheduler Component

In this section, the list of events of the Scheduler component have been provided which will be triggered for appropriate Scheduler actions.

The events should be provided to the Scheduler using **ScheduleEvents** tag. When using events of Scheduler, **TValue** must be provided in the **ScheduleEvents** tag.

## Table of Contents
- [Common Data Model](#common-data-model)
- [ActionCompleted](#actioncompleted)
- [Created](#created)
- [DataBinding](#databinding)
- [DataBound](#databound)
- [Destroyed](#destroyed)
- [Dragged](#dragged)
- [EventRendered](#eventrendered)
- [MoreEventsClicked](#moreeventstclicked)
- [Navigating](#navigating)
- [OnActionBegin](#onactionbegin)
- [OnActionFailure](#onactionfailure)
- [OnCellClick](#oncellclick)
- [OnCellDoubleClick](#oncelldoubleclick)
- [OnDragStart](#ondragstart)
- [OnEventClick](#oneventclick)
- [OnEventDoubleClick](#oneventdoubleclick)
- [OnPopupClose](#onpopupclose)
- [OnPopupOpen](#onpopupopen)
- [OnRenderCell](#onrendercell)
- [OnResizeStart](#onresizestart)
- [Resized](#resized)

## Common Data Model

Throughout the examples in this document, we use the `AppointmentData` class as the data model. Define this class once in your code:

```csharp
public class AppointmentData
{
    public int Id { get; set; }
    public string Subject { get; set; }
    public string Location { get; set; }
    public DateTime StartTime { get; set; }
    public DateTime EndTime { get; set; }
    public string Description { get; set; }
    public bool IsAllDay { get; set; }
    public string RecurrenceRule { get; set; }
    public string RecurrenceException { get; set; }
    public Nullable<int> RecurrenceID { get; set; }
}
```

## ActionCompleted

[`ActionCompleted`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_ActionCompleted) event triggers on successful completion of the Scheduler actions.

The action type that can be checked within the [`ActionCompleted`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_ActionCompleted) event are as follows.

| ActionType | Description |
|------|-------------|
| `EventCreate` | Triggers once event is created.|
| `EventChange` | Triggers once event is updated.|
| `EventRemove` | Triggers once event is deleted.|
| `DateNavigate` | Triggers once date navigation is performed.|
| `ViewNavigate` | Triggers once view navigation is performed.|
| `ResourceExpand` | Triggers once resource is expanded in timeline views.|
| `ResourceCollapse` | Triggers once resource is collapsed in timeline views.|

```cshtml
@using Syncfusion.Blazor.Schedule

<p style="color:green; font-size:20px">@Status</p>
<SfSchedule TValue="AppointmentData">
    <ScheduleEvents TValue="AppointmentData" ActionCompleted="OnActionCompleted"></ScheduleEvents>
</SfSchedule>

@code{
    string Status = "";
    public void OnActionCompleted(ActionEventArgs<AppointmentData> args)
    {
        if (args.ActionType == ActionType.EventCreate || args.ActionType == ActionType.EventChange)
        {
            Status = "Success";   //Status become success on create and update of an event.
        }
    }
}
```

## Created

[`Created`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_Created) event triggers after the Scheduler component is created.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData">
    <ScheduleEvents TValue="AppointmentData" Created="OnCreated"></ScheduleEvents>
</SfSchedule>

@code{
    public void OnCreated()
    {
        //Here you can customize your code
    }
}
```

## DataBinding

[`DataBinding`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_DataBinding) event triggers before the data binds to the Scheduler.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData">
    <ScheduleEvents TValue="AppointmentData" DataBinding="DataBindHandler"></ScheduleEvents>
</SfSchedule>

@code{
    public void DataBindHandler(DataBindingEventArgs<AppointmentData> args)
    {
        //Triggers before the data binds to the scheduler, while performing CRUD actions, View and Date navigations
    }
}
```

## DataBound

[`DataBound`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_DataBound) event triggers once the event data is bound to the Scheduler.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" @ref="@ScheduleRef">
    <ScheduleEvents TValue="AppointmentData" DataBound="OnDataBound"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
</SfSchedule>

@code{
    SfSchedule<AppointmentData> ScheduleRef;
    public async Task OnDataBound(DataBoundEventArgs<AppointmentData> args)
    {
        List<AppointmentData> eventCollection = await ScheduleRef.GetEvents();   //You can get the entire appointment collections in the EventCollection variable
    }
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { Id = 1, Subject = "Meeting", StartTime = new DateTime(2020, 3, 10, 9, 30, 0) , EndTime = new DateTime(2020, 3, 10, 12, 0, 0) }
    };
}
```

## Destroyed

[`Destroyed`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_Destroyed) event triggers when the Scheduler component is destroyed.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData">
    <ScheduleEvents TValue="AppointmentData" Destroyed="OnDestroyed"></ScheduleEvents>
</SfSchedule>

@code{
    public void OnDestroyed()
    {
        //Here you can customize your code
    }
}
```

## Dragged

[`Dragged`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_Dragged) event triggers when the dragging of appointment is stopped.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" @ref="@ScheduleObj">
    <ScheduleEvents TValue="AppointmentData" Dragged="OnDragged"></ScheduleEvents>
</SfSchedule>

@code{
    SfSchedule<AppointmentData> ScheduleObj;
    public async Task OnDragged(DragEventArgs<AppointmentData> args)
    {
        await ScheduleObj.OpenEditorAsync(args.Data, CurrentAction.Save);   //To open the editor window at drag stop
    }
}
```

## EventRendered

[`EventRendered`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_EventRendered) event triggers before each of the event getting rendered on the Scheduler user interface.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData">
    <ScheduleEvents TValue="AppointmentData" EventRendered="OnEventRendered"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
</SfSchedule>
<style>
    .e-schedule .e-vertical-view .e-all-day-appointment-wrapper .e-appointment.custom-class,
    .e-schedule .e-vertical-view .e-day-wrapper .e-appointment.custom-class,
    .e-schedule .e-month-view .e-appointment.custom-class {
        background: yellow;
        color: red;
    }
</style>

@code{
    public string[] CustomClass = { "custom-class" };
    public void OnEventRendered(EventRenderedArgs<AppointmentData> args)
    {
        args.CssClasses = new List<string>(CustomClass);
    }
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { Id = 1, Subject = "Meeting", StartTime = new DateTime(2020, 3, 10, 9, 30, 0) , EndTime = new DateTime(2020, 3, 10, 12, 0, 0) }
    };
}
```

## MoreEventsClicked

[`MoreEventsClicked`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_MoreEventsClicked) event triggers when the more events indicator are clicked.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData">
    <ScheduleEvents TValue="AppointmentData" MoreEventsClicked="OnMoreEventsClick"></ScheduleEvents>
</SfSchedule>

@code{
    public void OnMoreEventsClick(MoreEventsClickArgs args)
    {
        args.Cancel = true;   //To prevent showing the appointments in more indiactor
    }
}
```

## Navigating

[`Navigating`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_Navigating) event triggers before the date or view navigation takes place on Scheduler.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData">
    <ScheduleEvents TValue="AppointmentData" Navigating="OnNavigating"></ScheduleEvents>
</SfSchedule>

@code{
    public void OnNavigating(NavigatingEventArgs args)
    {
        if (args.ActionType == ActionType.DateNavigate) {
            args.Cancel = true;   //To prevent date navigation
        }
    }
}
```

## OnActionBegin

[`OnActionBegin`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_OnActionBegin) event triggers on beginning of every Scheduler action.

The request type that can be checked within the [`OnActionBegin`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_OnActionBegin) event are as follows.

| ActionType | Description |
|------|-------------|
| `EventCreate` | Triggers on Creating new event.|
| `EventChange` | Triggers on updating an event.|
| `EventRemove` | Triggers on Deleting an event.|
| `DateNavigate` | Triggers while performing date navigations.|
| `ViewNavigate` | Triggers while performing view navigations.|
| `ResourceExpand` | Triggers while expanding resource on timeline views.|
| `ViewNavigate` | Triggers while collapsing resource on timeline views.|

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData">
    <ScheduleEvents TValue="AppointmentData" OnActionBegin="OnActionBegin"></ScheduleEvents>
</SfSchedule>

@code{
    public void OnActionBegin(ActionEventArgs<AppointmentData> args)
    {
        if (args.ActionType == ActionType.EventRemove)   //To check for request type is event delete
        {
            args.Cancel = true;   //To prevent the appointment deletion
        }
    }
}
```

## OnActionFailure

[`OnActionFailure`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_OnActionFailure) event triggers when a Scheduler action gets failed or interrupted.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData">
    <ScheduleEvents TValue="AppointmentData" OnActionFailure="OnActionFailure"></ScheduleEvents>
</SfSchedule>

@code{
    public void OnActionFailure(ActionEventArgs<AppointmentData> args)
    {
        //args.Error catches the failure details.
    }
}
```

## OnCellClick

[`OnCellClick`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_OnCellClick) event triggers when the Scheduler cells are single clicked or on single tap on the same cells in mobile devices.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" @ref="@ScheduleRef">
    <ScheduleEvents TValue="AppointmentData" OnCellClick="OnCellClick"></ScheduleEvents>
</SfSchedule>

@code{
    SfSchedule<AppointmentData> ScheduleRef;
    public async Task OnCellClick(CellClickEventArgs args)
    {
        args.Cancel = true;
        await ScheduleRef.OpenEditorAsync(args, CurrentAction.Add);   //To open editor window on cell click
    }
}
```

## OnCellDoubleClick

[`OnCellDoubleClick`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_OnCellDoubleClick) event triggers when the Scheduler cells are double clicked.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData">
    <ScheduleEvents TValue="AppointmentData" OnCellDoubleClick="OnCellDoubleClick"></ScheduleEvents>
</SfSchedule>

@code{
    public void OnCellDoubleClick(CellClickEventArgs args)
    {
        args.Cancel = true;   //To prevent the opening of editor window on cells alone.
    }
}
```

## OnDragStart

[`OnDragStart`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_OnDragStart) event triggers when an appointment is started to drag.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData">
    <ScheduleEvents TValue="AppointmentData" OnDragStart="OnDragStart"></ScheduleEvents>
</SfSchedule>

@code{
    public void OnDragStart(DragEventArgs<AppointmentData> args)
    {
        args.Scroll.Enable = false;   //To prevent scroll action on dragging
    }
}
```

## OnEventClick

[`OnEventClick`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_OnEventClick) event triggers when the events are single clicked or on single tapping the events on the mobile devices.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" @ref="@ScheduleRef">
    <ScheduleEvents TValue="AppointmentData" OnEventClick="OnEventClick"></ScheduleEvents>
</SfSchedule>

@code{
    SfSchedule<AppointmentData> ScheduleRef;
    public async Task OnEventClick(EventClickArgs<AppointmentData> args)
    {
        args.Cancel = true;
        await ScheduleRef.OpenEditorAsync(args.Event, CurrentAction.Save);   //To open the editor on event click
    }
}
```

## OnEventDoubleClick

[`OnEventDoubleClick`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_OnEventDoubleClick) event triggers when the events are double clicked.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" @ref="@ScheduleRef">
    <ScheduleEvents TValue="AppointmentData" OnEventDoubleClick="OnEventDoubleClick"></ScheduleEvents>
</SfSchedule>

@code{
    SfSchedule<AppointmentData> ScheduleRef;
    public async Task OnEventDoubleClick(EventClickArgs<AppointmentData> args)
    {
        args.Cancel = true;
        await ScheduleRef.OpenQuickInfoPopupAsync(args.Event); //To open Quick popup on double click
    }
}
```

## OnPopupClose

[`OnPopupClose`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_OnPopupClose) event triggers before any of the Scheduler popups close on the page.

In case, if you need to prevent only specific popups on Scheduler, then you can check the condition based on the popup type. The types of the popup that can be checked within the [`OnPopupClose`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_OnPopupClose) event are as follows.

| PopupType | Description |
|------|-------------|
| `Editor` | For Detailed editor window.|
| `QuickInfo` | For Quick popup which opens on cell click.|
| `EditEventInfo` |For  Quick popup which opens on event click.|
| `ViewEventInfo` | For Quick popup which opens on responsive mode.|
| `EventContainer` | For more event indicator popup.|
| `RecurrenceAlert` | For edit recurrence event alert popup.|
| `DeleteAlert` | For delete confirmation popup.|
| `ValidationAlert` | For validation alert popup.|
| `RecurrenceValidationAlert` | For recurrence validation alert popup.|

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData">
    <ScheduleEvents TValue="AppointmentData" OnPopupClose="OnPopupClose"></ScheduleEvents>
</SfSchedule>

@code{
    public void OnPopupClose(PopupCloseEventArgs<AppointmentData> args)
    {
        if (args.Type == PopupType.Editor || args.Type == PopupType.QuickInfo)
        {
            args.Data.Subject = (args.Data.Subject == "Add title") ? "New event" : args.Data.Subject;   //The default subject is changed from Add Title to New event
        }
    }
}
```

## OnPopupOpen

[`OnPopupOpen`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_OnPopupOpen) event triggers before any of the Scheduler popups opens on the page.

In case, if you need to prevent only specific popups on Scheduler, then you can check the condition based on the popup type. The types of the popup that can be checked within the [`OnPopupOpen`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_OnPopupOpen) event are as follows.

| PopupType | Description |
|------|-------------|
| `Editor` | For Detailed editor window.|
| `QuickInfo` | For Quick popup which opens on cell click.|
| `EditEventInfo` |For  Quick popup which opens on event click.|
| `ViewEventInfo` | For Quick popup which opens on responsive mode.|
| `EventContainer` | For more event indicator popup.|
| `RecurrenceAlert` | For edit recurrence event alert popup.|
| `DeleteAlert` | For delete confirmation popup.|
| `ValidationAlert` | For validation alert popup.|
| `RecurrenceValidationAlert` | For recurrence validation alert popup.|

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData">
    <ScheduleEvents TValue="AppointmentData" OnPopupOpen="OnPopupOpen"></ScheduleEvents>
</SfSchedule>

@code{
    public void OnPopupOpen(PopupOpenEventArgs<AppointmentData> args)
    {
        if (args.Type == PopupType.ValidationAlert)
        {
            args.Cancel = true;   //To prevent start and end time validation alert
        }
    }
}
```

## OnRenderCell

[`OnRenderCell`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_OnRenderCell) event triggers before each element of the Schedule rendering on the page.

The ElementType that can be checked within the [`OnRenderCell`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_OnRenderCell) event are as follows.

| ElementType | Description |
|------|-------------|
| `AllDayCells` | Triggers on all day cell rendering.|
| `DateHeader` | Triggers on header cell rendering.|
| `EmptyCells` | Triggers on empty cell rendering on header bar.|
| `MajorSlot` | Triggers on major time slot cell rendering.|
| `MinorSlot` | Triggers on minor time slot cell rendering.|
| `MonthCells` | Triggers on month cell rendering.|
| `MonthDay` | Triggers on header cell in month view rendering.|
| `ResourceGroupCells` | Triggers on rendering of work cells for parent resource.|
| `ResourceHeader` | Triggers on resource header cell rendering.|
| `WorkCells` | Triggers on work cell rendering.|

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData">
    <ScheduleEvents TValue="AppointmentData" OnRenderCell="OnRenderCell"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
</SfSchedule>
<style>
    .e-schedule .e-vertical-view .e-work-hours.custom-class {
        background-color: ivory;
    }
</style>

@code{
    public string[] CustomClass = { "custom-class" };
    public void OnRenderCell(RenderCellEventArgs args)
    {
        if (args.ElementType == ElementType.WorkCells)
        {
            args.CssClasses = new List<string>(CustomClass); //The default work hours color is changed to ivory color
        }
    }
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { Id = 1, Subject = "Meeting", StartTime = new DateTime(2020, 3, 10, 9, 30, 0) , EndTime = new DateTime(2020, 3, 10, 12, 0, 0) }
    };
}
```

## OnResizeStart

[`OnResizeStart`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_OnResizeStart) event triggers when an appointment is started to resize.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData">
    <ScheduleEvents TValue="AppointmentData" OnResizeStart="OnResizeStart"></ScheduleEvents>
</SfSchedule>

@code{
    public void OnResizeStart(ResizeEventArgs<AppointmentData> args)
    {
        args.Scroll.Enable = false;   //To prevent scrolling will resize the event
        args.Interval = 10;   //To change the resizing time imterval from 30 minutes(default) to 10 minutes
    }
}
```

## Resized

[`Resized`](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Schedule.ScheduleEvents-1.html#Syncfusion_Blazor_Schedule_ScheduleEvents_1_Resized) event triggers when the resizing of appointment is stopped.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData">
    <ScheduleEvents TValue="AppointmentData" Resized="OnResized"></ScheduleEvents>
</SfSchedule>

@code{
    public void OnResized(ResizeEventArgs<AppointmentData> args)
    {
        args.Cancel = true;   //To prevent resize action
    }
}
```