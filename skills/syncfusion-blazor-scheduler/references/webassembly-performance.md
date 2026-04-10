# WebAssembly Performance in Blazor Scheduler Component

## Table of Contents

1. [Avoid Unnecessary Component Renders](#avoid-unnecessary-component-renders)
2. [Avoid Unnecessary Component Renders After Scheduler Events](#avoid-unnecessary-component-renders-after-scheduler-events)
3. [Performance Guidelines](#performance-guidelines)

## Overview

This section provides performance guidelines for using Syncfusion Scheduler component efficiently in Blazor WebAssembly application. The best practice or guidelines for general framework Blazor WebAssembly performance can be found [here](https://learn.microsoft.com/en-us/aspnet/core/blazor/performance?view=aspnetcore-7.0).

## Avoid Unnecessary Component Renders

During Blazor Diffing Algorithm, every views of the Scheduler component and its child component will be checked for re-rendering. For instance, having **EventCallBack** on the application or Scheduler will check every child component, once event callback is completed.

You can have fine-grained control over Scheduler component rendering. **PreventRender** method helps to avoid unnecessary re-rendering of the Scheduler component. This method internally overrides the **ShouldRender** method of the Scheduler to prevent rendering.

In the following example:

* **PreventRender** method is called in the **IncrementCount** method which is a click callback.
* Now, Scheduler component will not be a part of the rendering which happens as result of the click event and **currentCount** alone will get updated.

```cshtml
@using Syncfusion.Blazor.Schedule

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

<SfSchedule @ref="ScheduleRef" TValue=AppointmentData>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
        <ScheduleView Option="View.Agenda"></ScheduleView>
    </ScheduleViews>
</SfSchedule>
@code {
    SfSchedule<AppointmentData> ScheduleRef;
    private int currentCount = 0;
    private void IncrementCount()
    {
        ScheduleRef.PreventRender();
        currentCount++;
    }
    // AppointmentData class definition omitted for brevity - see common-patterns.md
}
```

## Avoid Unnecessary Component Renders After Scheduler Events

When a callback method is assigned to the Scheduler events, then the **StateHasChanged** will be called in parent component of the Scheduler automatically once the event is completed.

You can prevent this re-rendering of the Scheduler component by calling the **PreventRender** method.

In the following example:

* **OnCellClick** event is bound with a callback method, so once cell click event is completed the **StateHasChanged** will be invoked for the parent component.

```cshtml
@using Syncfusion.Blazor.Schedule

<p style="color:green; font-size:20px">@Status</p>
<SfSchedule @ref="ScheduleRef" TValue="AppointmentData" Width="100%" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" OnCellClick="OnCellClick" ActionCompleted="OnActionCompleted"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
        <ScheduleView Option="View.Agenda"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    SfSchedule<AppointmentData> ScheduleRef;
    string Status = string.Empty;
    DateTime CurrentDate = new DateTime(2020, 3, 10);
    public void OnCellClick(CellClickEventArgs args)
    {
        ScheduleRef.PreventRender();
    }
    public void OnActionCompleted(ActionEventArgs<AppointmentData> args)
    {
        ScheduleRef.PreventRender();
        if (args.ActionType == ActionType.EventCreate || args.ActionType == ActionType.EventChange)
        {
            Status = "Success";   //Status become success on create and update of an event.
        }
    }
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
    new AppointmentData { Id = 1, Subject = "Meeting", StartTime = new DateTime(2020, 3, 10, 9, 30, 0) , EndTime = new DateTime(2020, 3, 10, 12, 0, 0) }
    };
    // AppointmentData class definition omitted for brevity - see common-patterns.md
}
```

**Note:** 
* **PreventRender** method accepts boolean argument that accepts true or false to disable or enable rendering respectively.
* **PreventRender** method can be used only after Scheduler component completed initial rendering. Calling this method during initial rendering will not have any effect.
* **PreventRender** method internally overrides the **ShouldRender** method of the Scheduler to prevent rendering.
* It is recommended to use **PreventRender** method for user interactive events such as OnCellClick, OnEventClick etc. for better performance.
* For events without any argument such as **DataBound**, you can use **PreventRender** method of the Scheduler to disable rendering.
