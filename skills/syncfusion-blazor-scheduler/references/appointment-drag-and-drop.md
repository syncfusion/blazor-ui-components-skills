# Drag and Drop Appointments

## Table of Contents
- [Enable Drag and Drop](#enable-drag-and-drop)
- [Disable Drag and Drop](#disable-drag-and-drop)
- [Drag Multiple Appointments](#drag-multiple-appointments)
- [Prevent Drag on Specific Targets](#prevent-drag-on-specific-targets)
- [Control Scroll During Drag](#control-scroll-during-drag)
- [Adjust Scroll Speed](#adjust-scroll-speed)
- [Set Drag Time Interval](#set-drag-time-interval)
- [Auto-Navigate While Dragging](#auto-navigate-while-dragging)
- [Handle Drag Completion](#handle-drag-completion)
- [Drag Events Between Schedulers](#drag-events-between-schedulers)
- [Notes](#notes)

## Enable Drag and Drop

The `AllowDragAndDrop` property enables dragging appointments to new times:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" AllowDragAndDrop="true" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { Id = 1, Subject = "Meeting", StartTime = new DateTime(2026, 3, 24, 9, 30, 0), EndTime = new DateTime(2026, 3, 24, 11, 0, 0)}
    };
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Disable Drag and Drop

Set `AllowDragAndDrop="false"` to prevent dragging:

```cshtml
<SfSchedule TValue="AppointmentData" Height="550px" AllowDragAndDrop="false" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>
```

## Drag Multiple Appointments

Enable `AllowMultiDrag` to drag multiple selected events:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" AllowMultiDrag="true" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { Id = 1, Subject = "Meeting 1", StartTime = new DateTime(2026, 3, 24, 9, 30, 0), EndTime = new DateTime(2026, 3, 24, 11, 0, 0)},
        new AppointmentData { Id = 2, Subject = "Meeting 2", StartTime = new DateTime(2026, 3, 24, 12, 0, 0), EndTime = new DateTime(2026, 3, 24, 13, 0, 0)}
    };
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

Users hold Ctrl and click multiple events, then drag them together.

## Prevent Drag on Specific Targets

Use `OnDragStart` event to exclude certain areas:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" OnDragStart="OnAppointmentDrag"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    public void OnAppointmentDrag(DragEventArgs<AppointmentData> args)
    {
        // Prevent dragging from header and all-day cells
        args.ExcludeSelectors = "e-header-cells,e-all-day-cells";
    }
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { Id = 1, Subject = "Meeting", StartTime = new DateTime(2026, 3, 24, 9, 30, 0), EndTime = new DateTime(2026, 3, 24, 11, 0, 0)}
    };
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Control Scroll During Drag

Disable automatic scrolling at edges:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" OnDragStart="OnAppointmentDrag"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    public void OnAppointmentDrag(DragEventArgs<AppointmentData> args)
    {
        // Disable scrolling
        args.Scroll.Enable = false;
    }
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { Id = 1, Subject = "Meeting", StartTime = new DateTime(2026, 3, 24, 9, 30, 0), EndTime = new DateTime(2026, 3, 24, 11, 0, 0)}
    };
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Adjust Scroll Speed

Control how fast the scheduler scrolls during drag:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" OnDragStart="OnAppointmentDrag"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    public void OnAppointmentDrag(DragEventArgs<AppointmentData> args)
    {
        // Scroll by 5 minutes every 200ms
        args.Scroll.ScrollBy = 5;
        args.Scroll.TimeDelay = 200;
    }
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { Id = 1, Subject = "Meeting", StartTime = new DateTime(2026, 3, 24, 9, 30, 0), EndTime = new DateTime(2026, 3, 24, 11, 0, 0)}
    };
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Set Drag Time Interval

Change the increment interval during dragging:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" OnDragStart="OnAppointmentDrag"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    public void OnAppointmentDrag(DragEventArgs<AppointmentData> args)
    {
        // Drag in 15-minute intervals instead of default 30 minutes
        args.Interval = 15;
    }
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { Id = 1, Subject = "Meeting", StartTime = new DateTime(2026, 3, 24, 9, 30, 0), EndTime = new DateTime(2026, 3, 24, 11, 0, 0)}
    };
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Auto-Navigate While Dragging

Enable date range navigation while dragging at edges:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" OnDragStart="OnAppointmentDrag"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    public void OnAppointmentDrag(DragEventArgs<AppointmentData> args)
    {
        // Enable navigation - hold at edge for 2 seconds to auto-navigate
        args.Navigation.Enable = true;
        args.Navigation.TimeDelay = 2000;
    }
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { Id = 1, Subject = "Meeting", StartTime = new DateTime(2026, 3, 24, 9, 30, 0), EndTime = new DateTime(2026, 3, 24, 11, 0, 0)}
    };
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Handle Drag Completion

Execute actions when drag completes:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule @ref="ScheduleRef" TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" Dragged="OnDragComplete"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    SfSchedule<AppointmentData> ScheduleRef;
    
    public async Task OnDragComplete(DragEventArgs<AppointmentData> args)
    {
        // Show editor after drag completes
        args.Cancel = true;
        await ScheduleRef.OpenEditorAsync(args.Data, CurrentAction.Save);
    }
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { Id = 1, Subject = "Meeting", StartTime = new DateTime(2026, 3, 24, 9, 30, 0), EndTime = new DateTime(2026, 3, 24, 11, 0, 0)}
    };
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Drag Events Between Schedulers

Move events between two schedulers:

```cshtml
@using Syncfusion.Blazor.Schedule

<div class="row">
    <div class="col-lg-6">
        <h3>Scheduler 1</h3>
        <SfSchedule @ref="Schedule1Ref" Height="550px" TValue="AppointmentData" EventDragArea=".ScheduleContainer" @bind-SelectedDate="@CurrentDate">
            <ScheduleEvents TValue="AppointmentData" Dragged="OnDragged"></ScheduleEvents>
            <ScheduleEventSettings DataSource="@Schedule1Data"></ScheduleEventSettings>
            <ScheduleViews>
                <ScheduleView Option="View.Week"></ScheduleView>
            </ScheduleViews>
        </SfSchedule>
    </div>
    <div class="col-lg-6">
        <h3>Scheduler 2</h3>
        <SfSchedule @ref="Schedule2Ref" Height="550px" TValue="AppointmentData" CssClass="ScheduleContainer" SelectedDate="@CurrentDate">
            <ScheduleEventSettings DataSource="@Schedule2Data"></ScheduleEventSettings>
            <ScheduleViews>
                <ScheduleView Option="View.Week"></ScheduleView>
            </ScheduleViews>
        </SfSchedule>
    </div>
</div>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    SfSchedule<AppointmentData> Schedule1Ref;
    SfSchedule<AppointmentData> Schedule2Ref;
    
    List<AppointmentData> Schedule1Data = new List<AppointmentData>
    {
        new AppointmentData { Id = 1, Subject = "Meeting", StartTime = new DateTime(2026, 3, 24, 9, 30, 0), EndTime = new DateTime(2026, 3, 24, 11, 0, 0)}
    };
    
    List<AppointmentData> Schedule2Data = new List<AppointmentData>();
    
    public async Task OnDragged(DragEventArgs<AppointmentData> args)
    {
        await Schedule1Ref.DeleteEventAsync(args.Data.Id);
        Random random = new Random();
        args.Data.Id = random.Next(1000);
        await Schedule2Ref.AddEventAsync(args.Data);
    }
    
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Notes

- Drag-drop not supported in Agenda and Month-Agenda views
- Multi-drag not supported on mobile devices
- Default drag interval is 30 minutes
- Use events to customize drag behavior
