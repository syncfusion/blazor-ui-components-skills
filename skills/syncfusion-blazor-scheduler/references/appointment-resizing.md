# Resizing Appointments

## Table of Contents
- [Enable Resizing](#enable-resizing)
- [Disable Resizing](#disable-resizing)
- [Disable Scroll During Resize](#disable-scroll-during-resize)
- [Control Scroll Speed During Resize](#control-scroll-speed-during-resize)
- [Set Resize Time Interval](#set-resize-time-interval)
- [Notes](#notes)

## Enable Resizing

Use `AllowResizing="true"` to enable appointment resizing:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" AllowResizing="true" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
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

## Disable Resizing

Set `AllowResizing="false"`:

```cshtml
<SfSchedule TValue="AppointmentData" Height="550px" AllowResizing="false" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>
```

## Disable Scroll During Resize

Prevent automatic scrolling when resizing at edges:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" OnResizeStart="OnAppointmentResize"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    public void OnAppointmentResize(ResizeEventArgs<AppointmentData> args)
    {
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

## Control Scroll Speed During Resize

Adjust how fast scrolling occurs:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" OnResizeStart="OnAppointmentResize"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    public void OnAppointmentResize(ResizeEventArgs<AppointmentData> args)
    {
        // Scroll by 15 minutes when resizing at edge
        args.Scroll.ScrollBy = 15;
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

## Set Resize Time Interval

Change the increment interval during resizing:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" OnResizeStart="OnAppointmentResize"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    public void OnAppointmentResize(ResizeEventArgs<AppointmentData> args)
    {
        // Resize in 15-minute increments instead of default 30 minutes
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

## Notes

- Resizing not supported in Agenda and Month-Agenda views
- Default resize interval is 30 minutes
- Resize handlers appear on top and bottom of appointments
- Resizing adjusts EndTime (top) and StartTime (bottom)
