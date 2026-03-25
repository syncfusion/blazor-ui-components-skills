# Styling and Theming

## Table of Contents
- [CSS Class Property](#css-class-property)
- [Built-in Themes](#built-in-themes)
- [Appointment Styling](#appointment-styling)
- [Dynamic Styling with EventRendered](#dynamic-styling-with-eventrendered)
- [Cell Styling](#cell-styling)
- [Header Styling](#header-styling)
- [Resource Coloring](#resource-coloring)
- [Theme Options](#theme-options)
- [Common CSS Classes](#common-css-classes)
- [Notes](#notes)

## CSS Class Property

Apply CSS classes to scheduler:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" CssClass="custom-scheduler" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    .custom-scheduler {
        background-color: #f5f5f5;
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

## Built-in Themes

Apply predefined themes:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" Theme="Material" @bind-SelectedDate="@CurrentDate">
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

## Appointment Styling

Style individual appointments with CssClass:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    .urgent { background-color: #ff4757 !important; color: white; }
    .normal { background-color: #2ed573 !important; color: white; }
    .low { background-color: #ffa502 !important; color: white; }
</style>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { Id = 1, Subject = "Urgent Meeting", Priority = "High", CssClass = "urgent", StartTime = new DateTime(2026, 3, 24, 9, 0, 0), EndTime = new DateTime(2026, 3, 24, 10, 0, 0)},
        new AppointmentData { Id = 2, Subject = "Regular Meeting", Priority = "Normal", CssClass = "normal", StartTime = new DateTime(2026, 3, 24, 11, 0, 0), EndTime = new DateTime(2026, 3, 24, 12, 0, 0)},
        new AppointmentData { Id = 3, Subject = "Low Priority", Priority = "Low", CssClass = "low", StartTime = new DateTime(2026, 3, 24, 14, 0, 0), EndTime = new DateTime(2026, 3, 24, 15, 0, 0)}
    };
    
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public string Priority { get; set; }
        public string CssClass { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Dynamic Styling with EventRendered

Apply styles conditionally:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" EventRendered="OnEventRender"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    .holiday { background-color: #ffd700 !important; }
    .weekend { background-color: #87ceeb !important; }
</style>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { Id = 1, Subject = "Holiday", IsHoliday = true, StartTime = new DateTime(2026, 3, 24, 9, 0, 0), EndTime = new DateTime(2026, 3, 24, 10, 0, 0)}
    };
    
    private async Task OnEventRender(EventRenderedArgs<AppointmentData> args)
    {
        if (args.Data.IsHoliday)
        {
            args.Attributes = new Dictionary<string, object> { { "class", "holiday" } };
        }
    }
    
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public bool IsHoliday { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Cell Styling

Style time slots:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" CellRendering="OnCellRender"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    .weekend-cell { background-color: #f0f0f0; }
</style>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();
    
    private async Task OnCellRender(CellRenderingEventArgs args)
    {
        if (args.Date.DayOfWeek == DayOfWeek.Saturday || args.Date.DayOfWeek == DayOfWeek.Sunday)
        {
            args.Attributes = new Dictionary<string, object> { { "class", "weekend-cell" } };
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

## Header Styling

Style scheduler header:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" CssClass="custom-header" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    .custom-header .e-toolbar {
        background-color: #1a73e8;
        color: white;
    }
    .custom-header .e-toolbar button {
        color: white;
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

## Resource Coloring

Color appointments by resource:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleGroup Resources="@Resources"></ScheduleGroup>
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int" DataSource="@ResourceData" Id="Id" Text="Name" ColorField="Color"></ScheduleResource>
    </ScheduleResources>
    <ScheduleViews>
        <ScheduleView Option="View.Week"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public List<string> Resources = new List<string> { "Rooms" };
    
    List<ResourceData> ResourceData = new List<ResourceData>
    {
        new ResourceData { Id = 1, Name = "Room A", Color = "#FF6B6B" },
        new ResourceData { Id = 2, Name = "Room B", Color = "#4ECDC4" }
    };
    
    List<AppointmentData> DataSource = new List<AppointmentData>();
    
    public class ResourceData
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Color { get; set; }
    }
    
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public int ResourceId { get; set; }
    }
}
```

## Theme Options

| Theme | Description |
|-------|-------------|
| Material | Google Material Design |
| Bootstrap | Bootstrap styling |
| Bootstrap4 | Bootstrap 4 styling |
| Office-365 | Office 365 design |
| HighContrast | High contrast accessibility |

## Common CSS Classes

| Class | Target |
|-------|--------|
| .e-schedule | Scheduler container |
| .e-appointment | Appointment element |
| .e-cell | Time cell |
| .e-toolbar | Header toolbar |
| .e-header-cells | Header cells |

## Notes

- CssClass property applies to scheduler container
- Use !important for custom appointment styles
- EventRendered fires for each appointment
- CellRendering fires for each time cell
- Theme property overrides CSS classes
- Built-in themes require CSS in index.html
