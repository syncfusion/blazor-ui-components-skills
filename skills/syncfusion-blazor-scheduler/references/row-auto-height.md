# Row Auto-Height

## Table of Contents
- [Enable Auto-Height for Cells](#enable-auto-height-for-cells)
- [Auto-Height in Month View](#auto-height-in-month-view)
- [Disable Auto-Height](#disable-auto-height)
- [Auto-Height with Custom Content](#auto-height-with-custom-content)
- [Auto-Height Performance](#auto-height-performance)
- [Row Height Calculation](#row-height-calculation)
- [Combine with Virtual Scrolling](#combine-with-virtual-scrolling)
- [Notes](#notes)

## Enable Auto-Height for Cells

Automatically expand cells to fit content:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" RowAutoHeight="true" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { Id = 1, Subject = "Meeting with very long description that needs multiple lines for display", StartTime = new DateTime(2026, 3, 24, 9, 0, 0), EndTime = new DateTime(2026, 3, 24, 10, 0, 0)},
        new AppointmentData { Id = 2, Subject = "Short", StartTime = new DateTime(2026, 3, 24, 11, 0, 0), EndTime = new DateTime(2026, 3, 24, 12, 0, 0)}
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

## Auto-Height in Month View

Expand month cells for many events:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" RowAutoHeight="true" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { Id = 1, Subject = "Event 1", StartTime = new DateTime(2026, 3, 24, 9, 0, 0), EndTime = new DateTime(2026, 3, 24, 10, 0, 0)},
        new AppointmentData { Id = 2, Subject = "Event 2", StartTime = new DateTime(2026, 3, 24, 10, 0, 0), EndTime = new DateTime(2026, 3, 24, 11, 0, 0)},
        new AppointmentData { Id = 3, Subject = "Event 3", StartTime = new DateTime(2026, 3, 24, 11, 0, 0), EndTime = new DateTime(2026, 3, 24, 12, 0, 0)},
        new AppointmentData { Id = 4, Subject = "Event 4", StartTime = new DateTime(2026, 3, 24, 14, 0, 0), EndTime = new DateTime(2026, 3, 24, 15, 0, 0)}
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

## Disable Auto-Height

Fixed cell heights:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" RowAutoHeight="false" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();
    
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Auto-Height with Custom Content

Handle long appointment text:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" RowAutoHeight="true" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource">
        <ScheduleField Subject="Subject" Description="Description"></ScheduleField>
    </ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Project Planning", 
            Description = "Detailed planning session for Q2 initiatives including resource allocation and timeline",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0)
        }
    };
    
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public string Description { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Auto-Height Performance

Manage performance with many events:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" RowAutoHeight="true" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();
    
    protected override void OnInitialized()
    {
        // Load events for current month only
        for (int day = 1; day <= DateTime.DaysInMonth(2026, 3); day++)
        {
            for (int event_num = 1; event_num <= 3; event_num++)
            {
                DataSource.Add(new AppointmentData
                {
                    Id = (day * 10) + event_num,
                    Subject = $"Event {event_num}",
                    StartTime = new DateTime(2026, 3, day, 9 + (event_num - 1), 0, 0),
                    EndTime = new DateTime(2026, 3, day, 10 + (event_num - 1), 0, 0)
                });
            }
        }
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

## Row Height Calculation

Row height automatically adjusts when RowAutoHeight="true":

```cshtml
<style>
    /* Default row height */
    .e-schedule .e-date-range {
        min-height: 80px;
    }
    
    /* Auto-height expands based on content */
    .e-schedule.e-row-auto-height .e-date-range {
        min-height: auto;
    }
    
    /* Event styling */
    .e-schedule .e-appointment {
        padding: 8px;
        overflow: hidden;
    }
</style>
```

## Combine with Virtual Scrolling

Optimize performance:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" RowAutoHeight="true" AllowVirtualScroll="true" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();
    
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

- RowAutoHeight="true" expands cells for content
- Useful for month view with many events
- Improves readability of busy schedules
- Automatic height calculation
- Performance impact with very large datasets
- Combine with virtual scrolling for optimization
- Works best with month and agenda views
- Default behavior: cells have minimum fixed height
