# Timezone Support

## Table of Contents
- [Set Scheduler Timezone](#set-scheduler-timezone)
- [Set Appointment Timezone](#set-appointment-timezone)
- [Display Appointments with Different Timezones](#display-appointments-with-different-timezones)
- [Set End Timezone](#set-end-timezone)
- [UTC Timezone Handling](#utc-timezone-handling)
- [Common Timezones](#common-timezones)
- [Notes](#notes)

## Set Scheduler Timezone

Configure scheduler timezone using `Timezone` property:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" Timezone="America/New_York" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { Id = 1, Subject = "Meeting", StartTime = new DateTime(2026, 3, 24, 9, 0, 0), EndTime = new DateTime(2026, 3, 24, 10, 0, 0)}
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

## Set Appointment Timezone

Configure individual appointment timezones:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "NY Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0),
            StartTimezone = "America/New_York"
        },
        new AppointmentData 
        { 
            Id = 2, 
            Subject = "London Meeting", 
            StartTime = new DateTime(2026, 3, 24, 14, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 15, 0, 0),
            StartTimezone = "Europe/London"
        }
    };
    
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public string StartTimezone { get; set; }
    }
}
```

## Display Appointments with Different Timezones

Show appointments converted to scheduler timezone:

```cshtml
@using Syncfusion.Blazor.Schedule

<div>Scheduler Timezone: @SelectedTimezone</div>

<SfSchedule TValue="AppointmentData" Height="550px" Timezone="@SelectedTimezone" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    string SelectedTimezone = "America/New_York";
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0),
            StartTimezone = "Asia/Tokyo"
        }
    };
    
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public string StartTimezone { get; set; }
    }
}
```

## Set End Timezone

Configure different end time timezone:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Cross-timezone Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0),
            StartTimezone = "America/New_York",
            EndTimezone = "Europe/London"
        }
    };
    
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public string StartTimezone { get; set; }
        public string EndTimezone { get; set; }
    }
}
```

## UTC Timezone Handling

Work with UTC times:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" Timezone="UTC" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "UTC Meeting", 
            StartTime = DateTime.UtcNow.AddHours(1), 
            EndTime = DateTime.UtcNow.AddHours(2)
        }
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

## Common Timezones

| Timezone | Region |
|----------|--------|
| America/New_York | Eastern Time |
| America/Chicago | Central Time |
| America/Denver | Mountain Time |
| America/Los_Angeles | Pacific Time |
| Europe/London | UK |
| Europe/Paris | Central Europe |
| Asia/Tokyo | Japan |
| Asia/Shanghai | China |
| Australia/Sydney | Australia |
| UTC | Coordinated Universal Time |

## Notes

- Timezone property affects how appointments are displayed
- StartTimezone and EndTimezone are built-in fields
- Use IANA timezone identifiers (e.g., "America/New_York")
- All times in AppointmentData should be DateTime (local or UTC)
- Scheduler converts display times based on configured timezone
- Daylight saving time automatically handled by IANA database
