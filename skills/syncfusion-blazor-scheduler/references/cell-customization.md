# Cell Customization

## Table of Contents
- [Overview](#overview)
- [Setting Cell Dimensions](#setting-cell-dimensions)
- [Customizing Cells with CellTemplate](#customizing-cells-with-celltemplate)
- [Cell Header Template](#cell-header-template)
- [OnRenderCell Event](#onrendercell-event)
- [Weekend Cell Customization](#weekend-cell-customization)
- [Min and Max Date Customization](#min-and-max-date-customization)
- [Disabling Multiple Cell Selection](#disabling-multiple-cell-selection)

## Overview

Customize Scheduler cells using CellTemplate, CellHeaderTemplate, and the OnRenderCell event. These approaches allow you to modify cell appearance, add custom content, and apply conditional styling based on date, time, or other criteria.

## Setting Cell Dimensions

### Vertical Views Cell Dimensions

Customize cell width and height in Day, Week, and Work Week views using CssClass:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" CssClass="schedule-cell-dimension" Height="550px">
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    .schedule-cell-dimension.e-schedule .e-vertical-view .e-date-header-wrap table col,
    .schedule-cell-dimension.e-schedule .e-vertical-view .e-content-wrap table col {
        width: 200px;
    }

    .schedule-cell-dimension.e-schedule .e-vertical-view .e-time-cells-wrap table td,
    .schedule-cell-dimension.e-schedule .e-vertical-view .e-work-cells {
        height: 100px;
    }

    .schedule-cell-dimension.e-schedule .e-month-view .e-work-cells,
    .schedule-cell-dimension.e-schedule .e-month-view .e-date-header-wrap table col {
        width: 200px;
    }

    .schedule-cell-dimension.e-schedule .e-month-view .e-work-cells {
        height: 200px;
    }
</style>

@code{
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

### Timeline Views Cell Dimensions

Customize Timeline view cell dimensions:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" CssClass="schedule-cell-dimension" Height="550px">
    <ScheduleViews>
        <ScheduleView Option="View.TimelineWeek" MaxEventsPerRow="10"></ScheduleView>
        <ScheduleView Option="View.TimelineMonth" MaxEventsPerRow="10"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    .schedule-cell-dimension.e-schedule .e-timeline-view .e-date-header-wrap table col,
    .schedule-cell-dimension.e-schedule .e-timeline-view .e-content-wrap table col {
        width: 200px;
    }

    .schedule-cell-dimension.e-schedule .e-timeline-month-view .e-date-header-wrap table col,
    .schedule-cell-dimension.e-schedule .e-timeline-month-view .e-content-wrap table col {
        width: 200px;
    }
</style>

@code{
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Customizing Cells with CellTemplate

The CellTemplate allows you to customize cell background with specific images, text, or custom content on given date values:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Width="100%" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleTemplates>
        <CellTemplate>
            <div class="templatewrap">
                @{
                    @if ((int)(context as TemplateContext).Date.Month == 3 && (int)(context as TemplateContext).Date.Day == 24)
                    {
                        <div class="caption">Team Meeting</div>
                    }
                    @if ((int)(context as TemplateContext).Date.Month == 3 && (int)(context as TemplateContext).Date.Day == 25)
                    {
                        <div class="caption">Project Deadline</div>
                    }
                    @if ((int)(context as TemplateContext).Date.Month == 3 && (int)(context as TemplateContext).Date.Day == 26)
                    {
                        <div class="caption">Training Day</div>
                    }
                }
            </div>
        </CellTemplate>
    </ScheduleTemplates>
    <ScheduleViews>
        <ScheduleView Option="View.Month" MaxEventsPerRow="2"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    .e-schedule .e-month-view .e-work-cells {
        position: relative;
    }

    .e-schedule .templatewrap {
        text-align: center;
        position: absolute;
        width: 100%;
    }

    .e-schedule .caption {
        overflow: hidden;
        text-overflow: ellipsis;
        vertical-align: middle;
        font-weight: bold;
        color: #d9534f;
    }
</style>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Cell Header Template

Customize the month header of each date cell using CellHeaderTemplate:

```cshtml
@using Syncfusion.Blazor.Schedule
@using System.Globalization

<SfSchedule TValue="AppointmentData" Width="100%" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleTemplates>
        <CellHeaderTemplate>
            <div>@context.Date.ToString("dd ddd", CultureInfo.InvariantCulture)</div>
        </CellHeaderTemplate>
    </ScheduleTemplates>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0),
            RecurrenceRule = "FREQ=DAILY;INTERVAL=1;COUNT=5" 
        }
    };
    
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
        public string RecurrenceRule { get; set; }
        public Nullable<int> RecurrenceID { get; set; }
        public string RecurrenceException { get; set; }
    }
}
```

## OnRenderCell Event

Customize cells using the OnRenderCell event. This event fires for each cell during rendering and provides access to RenderCellEventArgs with ElementType property:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Width="100%" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" OnRenderCell="OnRenderCell"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    .e-schedule .e-vertical-view .e-work-hours.custom-class {
        background-color: ivory;
    }
</style>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] CustomClass = { "custom-class" };
    
    public void OnRenderCell(RenderCellEventArgs args)
    {
        // Customize cells based on ElementType
        if (args.ElementType == ElementType.WorkCells)
        {
            args.CssClasses = new List<string>(CustomClass);
        }
    }
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0),
            EndTime = new DateTime(2026, 3, 24, 12, 0, 0) 
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

## Weekend Cell Customization

Customize the background color of weekend cells using OnRenderCell event and CssClasses:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Width="100%" Height="550px" @bind-SelectedDate="@CurrentDate" CssClass="schedule-cell-customization">
    <ScheduleEvents TValue="AppointmentData" OnRenderCell="OnRenderCell"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<style>
    .schedule-cell-customization.e-schedule .e-vertical-view .e-work-cells.weekend-class {
        background-color: #ffdea2;
    }

    .schedule-cell-customization.e-schedule .e-month-view .e-work-cells:not(.e-work-days) {
        background-color: #f08080;
    }
</style>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string[] WeekendClass = { "weekend-class" };
    
    public void OnRenderCell(RenderCellEventArgs args)
    {
        // Check if the date is weekend
        if (args.ElementType == ElementType.WorkCells)
        {
            if (args.Date.DayOfWeek == DayOfWeek.Sunday || args.Date.DayOfWeek == DayOfWeek.Saturday)
            {
                args.CssClasses = new List<string>(WeekendClass);
            }
        }
    }
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0),
            EndTime = new DateTime(2026, 3, 24, 12, 0, 0) 
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

## Min and Max Date Customization

Restrict date range by setting MinDate and MaxDate properties. Dates outside this range will be disabled and navigation will be blocked:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" 
    Height="650px" 
    MinDate="new DateTime(2026, 3, 1)" 
    MaxDate="new DateTime(2026, 3, 31)" 
    @bind-SelectedDate="@CurrentDate">
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Disabling Multiple Cell Selection

By default, AllowMultiCellSelection and AllowMultiRowSelection properties are enabled. Disable them to prevent multiple cell/row selection:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" 
    Height="550px" 
    AllowMultiCellSelection="false"
    AllowMultiRowSelection="false"
    @bind-SelectedDate="@CurrentDate">
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
    }
}
```

## Common Patterns

**CellTemplate vs OnRenderCell:**
- Use **CellTemplate** for adding custom HTML content or images to cells
- Use **OnRenderCell** for applying CSS classes and styling based on conditions

**Applying Styles:**
- Use **CssClass** property on SfSchedule for global cell styling
- Use **OnRenderCell** with **CssClasses** for conditional styling
- Use `<style>` tags with selector chains like `.custom-class.e-schedule .e-work-cells`

**ElementType Values:**
- `ElementType.WorkCells`: Regular time slot cells in vertical/timeline views
- `ElementType.AllDayCells`: All-day event row cells
- `ElementType.MonthCells`: Month view date cells
