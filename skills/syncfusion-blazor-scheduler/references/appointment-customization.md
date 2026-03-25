# Appointment Customization

## Table of Contents
- [Customizing Appointments with Template](#customizing-appointments-with-template)
- [Using EventRendered Event](#using-eventrendered-event)
- [Customizing with Attributes](#customizing-with-attributes)
- [Using CssClass Field](#using-cssclass-field)
- [Using CssClass Property on Scheduler](#using-cssclass-property-on-scheduler)
- [Display Tooltips for Appointments](#display-tooltips-for-appointments)
- [Conditional Customization](#conditional-customization)
- [Important Notes](#important-notes)

## Customizing Appointments with Template

Apply custom HTML templates to change appointment appearance:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource">
        <Template>
            <div class="appointment-template">
                <div class="subject">@((context as AppointmentData).Subject)</div>
                <div class="time">@((context as AppointmentData).StartTime.ToString("hh:mm tt"))</div>
                <div class="location">📍 @((context as AppointmentData).Location)</div>
            </div>
        </Template>
    </ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    .appointment-template {
        padding: 5px;
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        border-radius: 4px;
        height: 100%;
    }
    
    .appointment-template .subject {
        font-weight: bold;
        margin-bottom: 2px;
    }
    
    .appointment-template .time {
        font-size: 12px;
        opacity: 0.9;
    }
    
    .appointment-template .location {
        font-size: 11px;
    }
</style>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1,
            Subject = "Team Meeting",
            Location = "Room 101",
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0)
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public string Location { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Using EventRendered Event

Customize appointments based on conditions before rendering:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" EventRendered="OnEventRendered"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    .e-schedule .e-appointment.high-priority {
        background-color: #ff4757 !important;
        border-left: 4px solid #c92a2a !important;
    }
    
    .e-schedule .e-appointment.medium-priority {
        background-color: #ffa502 !important;
        border-left: 4px solid #ff6348 !important;
    }
    
    .e-schedule .e-appointment.completed {
        background-color: #2ed573 !important;
        text-decoration: line-through;
        opacity: 0.7;
    }
</style>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    public void OnEventRendered(EventRenderedArgs<AppointmentData> args)
    {
        if (args.Data.Priority == "High")
        {
            args.CssClasses = new List<string> { "high-priority" };
        }
        else if (args.Data.Priority == "Medium")
        {
            args.CssClasses = new List<string> { "medium-priority" };
        }
        
        if (args.Data.Status == "Completed")
        {
            args.CssClasses.Add("completed");
        }
    }
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1,
            Subject = "Critical Issue",
            Priority = "High",
            Status = "Active",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0)
        },
        new AppointmentData 
        { 
            Id = 2,
            Subject = "Review Task",
            Priority = "Medium",
            Status = "Completed",
            StartTime = new DateTime(2026, 3, 24, 11, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 12, 0, 0)
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public string Priority { get; set; }
        public string Status { get; set; }
    }
}
```

## Customizing with Attributes

Add or modify HTML attributes dynamically:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" EventRendered="OnEventRendered"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    public void OnEventRendered(EventRenderedArgs<AppointmentData> args)
    {
        var attributes = new Dictionary<string, object>
        {
            { "style", $"background: {GetColorByStatus(args.Data.Status)}; border-radius: 4px;" },
            { "title", args.Data.Subject + " - " + args.Data.Status },
            { "data-status", args.Data.Status }
        };
        args.Attributes = attributes;
    }
    
    private string GetColorByStatus(string status)
    {
        return status switch
        {
            "Confirmed" => "#4CAF50",
            "Pending" => "#FF9800",
            "Cancelled" => "#F44336",
            _ => "#2196F3"
        };
    }
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1,
            Subject = "Confirmed",
            Status = "Confirmed",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0)
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public string Status { get; set; }
    }
}
```

## Using CssClass Field

Apply CSS classes directly from data model:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    /* Event type styles */
    .e-schedule .e-appointment.meeting {
        background-color: #3498db;
        color: white;
    }
    
    .e-schedule .e-appointment.conference {
        background-color: #9b59b6;
        color: white;
    }
    
    .e-schedule .e-appointment.training {
        background-color: #1abc9c;
        color: white;
    }
    
    .e-schedule .e-appointment.lunch {
        background-color: #e74c3c;
        color: white;
        border-bottom: 3px dotted #c0392b;
    }
</style>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1,
            Subject = "Team Meeting",
            CssClass = "meeting",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0)
        },
        new AppointmentData 
        { 
            Id = 2,
            Subject = "Annual Conference",
            CssClass = "conference",
            StartTime = new DateTime(2026, 3, 24, 10, 30, 0),
            EndTime = new DateTime(2026, 3, 24, 12, 0, 0)
        },
        new AppointmentData 
        { 
            Id = 3,
            Subject = "Lunch Break",
            CssClass = "lunch",
            StartTime = new DateTime(2026, 3, 24, 12, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 13, 0, 0)
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public string CssClass { get; set; }
    }
}
```

## Using CssClass Property on Scheduler

Apply global styles to all appointments:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" CssClass="custom-scheduler" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    /* Apply custom styles to all appointments in this scheduler */
    .custom-scheduler.e-schedule .e-appointment {
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        font-weight: 500;
        letter-spacing: 0.5px;
    }
    
    .custom-scheduler.e-schedule .e-appointment:hover {
        transform: translateY(-2px);
        box-shadow: 0 4px 8px rgba(0,0,0,0.15);
    }
</style>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1,
            Subject = "Meeting",
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0)
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

## Display Tooltips for Appointments

Show additional information on hover:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource" EnableTooltip="true">
        <TooltipTemplate>
            <div class="tooltip-content">
                <strong>@((context as AppointmentData).Subject)</strong>
                <div>📍 @((context as AppointmentData).Location)</div>
                <div>⏰ @((context as AppointmentData).StartTime.ToString("hh:mm tt")) - @((context as AppointmentData).EndTime.ToString("hh:mm tt"))</div>
                <div>📝 @((context as AppointmentData).Description)</div>
            </div>
        </TooltipTemplate>
    </ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    .tooltip-content {
        padding: 10px;
        background: #333;
        color: white;
        border-radius: 4px;
        font-size: 12px;
    }
</style>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1,
            Subject = "Project Kickoff",
            Location = "Room 201",
            Description = "Discuss project scope and timeline",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 10, 30, 0)
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public string Location { get; set; }
        public string Description { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Conditional Customization

Customize based on appointment data:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" EventRendered="OnEventRendered"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource">
        <Template>
            <div class="@GetTemplateClass((context as AppointmentData))">
                <span class="urgency">@GetUrgencyIcon((context as AppointmentData))</span>
                <strong>@((context as AppointmentData).Subject)</strong>
            </div>
        </Template>
    </ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    private string GetTemplateClass(AppointmentData data)
    {
        return data.Priority switch
        {
            "High" => "appointment high-urgent",
            "Medium" => "appointment medium-urgent",
            _ => "appointment low-urgent"
        };
    }
    
    private string GetUrgencyIcon(AppointmentData data)
    {
        return data.Priority switch
        {
            "High" => "🔴",
            "Medium" => "🟡",
            _ => "🟢"
        };
    }
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1,
            Subject = "Critical",
            Priority = "High",
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0)
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public string Priority { get; set; }
    }
}
```

## Important Notes

- Templates cannot override `height`, `width`, `top`, `left`, `right`, and `display` CSS properties
- Use `EventRendered` for dynamic styles based on data
- Use `Template` for custom HTML structure
- Combine both for maximum customization
- Always consider accessibility when customizing colors
