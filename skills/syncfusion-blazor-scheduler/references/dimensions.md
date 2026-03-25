# Sizing and Dimensions

## Table of Contents
- [Set Height](#set-height)
- [Set Width](#set-width)
- [Responsive Sizing](#responsive-sizing)
- [Cell Height](#cell-height)
- [Min/Max Date Range](#minmax-date-range)
- [Fixed Size](#fixed-size)
- [Timescale Configuration](#timescale-configuration)
- [Container Sizing](#container-sizing)
- [Sizing Units](#sizing-units)
- [Default Dimensions](#default-dimensions)
- [Notes](#notes)

## Set Height

Configure scheduler height:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="600px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
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

## Set Width

Configure scheduler width:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Width="100%" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
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

## Responsive Sizing

Use percentage or viewport units:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Width="100%" Height="100vh" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
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

## Cell Height

Set timescale cell height:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleTimeScale Interval="60" SlotCount="2"></ScheduleTimeScale>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
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

## Min/Max Date Range

Restrict visible date range:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" 
    MinDate="@MinDate" 
    MaxDate="@MaxDate" 
    @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime MinDate = new DateTime(2026, 1, 1);
    DateTime MaxDate = new DateTime(2026, 12, 31);
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

## Fixed Size

Set exact dimensions:

```cshtml
@using Syncfusion.Blazor.Schedule

<div style="width: 800px; height: 600px;">
    <SfSchedule TValue="AppointmentData" Height="100%" Width="100%" @bind-SelectedDate="@CurrentDate">
        <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
        <ScheduleViews>
            <ScheduleView Option="View.Week"></ScheduleView>
        </ScheduleViews>
    </SfSchedule>
</div>

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

## Timescale Configuration

Adjust time slot dimensions:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleTimeScale Interval="30" SlotCount="2"></ScheduleTimeScale>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
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

## Container Sizing

Size scheduler within container:

```cshtml
@using Syncfusion.Blazor.Schedule

<div class="scheduler-container">
    <SfSchedule TValue="AppointmentData" Height="550px" Width="100%" @bind-SelectedDate="@CurrentDate">
        <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
        <ScheduleViews>
            <ScheduleView Option="View.Week"></ScheduleView>
        </ScheduleViews>
    </SfSchedule>
</div>

<style>
    .scheduler-container {
        padding: 20px;
        max-width: 1200px;
        margin: 0 auto;
    }
</style>

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

## Sizing Units

| Unit | Example | Description |
|------|---------|-------------|
| px | 550px | Fixed pixels |
| % | 100% | Percentage of parent |
| em | 30em | Relative to font size |
| rem | 30rem | Relative to root font size |
| vh | 100vh | Percentage of viewport height |
| vw | 100vw | Percentage of viewport width |

## Default Dimensions

- Default Height: 550px
- Default Width: 100%
- TimeScale Interval: 60 minutes
- SlotCount: 2
- Cell Height: Calculated based on Interval

## Notes

- Height and Width can use any CSS unit
- MinDate/MaxDate restrict navigation
- Percentage sizing requires parent container with defined size
- TimeScale Interval is in minutes (15, 30, 60)
- SlotCount controls subdivisions per hour
