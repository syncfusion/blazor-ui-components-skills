# Getting Started with Syncfusion Blazor Scheduler

## Table of Contents
- [Install NuGet Packages](#install-nuget-packages)
- [Add Import Namespaces](#add-import-namespaces)
- [Register Syncfusion Service](#register-syncfusion-service)
- [Add Stylesheet and Script Resources](#add-stylesheet-and-script-resources)
- [Add Blazor Scheduler Component](#add-blazor-scheduler-component)
- [Populating Appointments](#populating-appointments)
- [Setting Date](#setting-date)
- [Setting View](#setting-view)
- [Individual View Customization](#individual-view-customization)

## Common Data Model

Throughout this documentation, the following `AppointmentData` class is used in examples. You only need to define it once in your component:

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

## Install NuGet Packages

**Using NuGet Package Manager:**
```
Install-Package Syncfusion.Blazor.Schedule -Version {{ site.releaseversion }}
Install-Package Syncfusion.Blazor.Themes -Version {{ site.releaseversion }}
```

**Using .NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.Schedule
dotnet add package Syncfusion.Blazor.Themes
dotnet restore
```

## Add Import Namespaces

Open the **~/_Imports.razor** file and import the namespaces:

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Schedule
```

## Register Syncfusion Service

Register the Syncfusion Blazor Service in the **~/Program.cs** file:

```csharp
using Syncfusion.Blazor;

builder.Services.AddSyncfusionBlazor();

```

## Add Stylesheet and Script Resources

Include the stylesheet and script references within the `<head>` section of the **~/index.html** file:

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

## Add Blazor Scheduler Component

Add the Syncfusion Blazor Scheduler component in the **~/Pages/Index.razor** file:

```razor
<SfSchedule TValue="AppointmentData">
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
        <ScheduleView Option="View.Agenda"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    // Define AppointmentData class here (see Common Data Model section above)
}
```

## Populating Appointments

To populate the Scheduler with appointments, bind the event data using the `DataSource` property:

```razor
<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
        <ScheduleView Option="View.Agenda"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2025, 2, 14);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { 
            Id = 1, 
            Subject = "Paris", 
            StartTime = new DateTime(2025, 2, 13, 10, 0, 0), 
            EndTime = new DateTime(2025, 2, 13, 12, 0, 0) 
        },
        new AppointmentData { 
            Id = 2, 
            Subject = "Germany", 
            StartTime = new DateTime(2025, 2, 15, 10, 0, 0), 
            EndTime = new DateTime(2025, 2, 15, 12, 0, 0) 
        }
    };
    // AppointmentData class defined here
}
```

## Setting Date

The Scheduler usually displays the system date. To change the current date, use the `SelectedDate` property:

```razor
<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <!-- ScheduleViews here -->
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2020, 1, 10);
}
```

## Setting View

The Scheduler displays `Week` view by default. To change the current view, use the `CurrentView` property:

**Available Views:**
- Day, Week, WorkWeek, Month, Agenda, MonthAgenda
- TimelineDay, TimelineWeek, TimelineWorkWeek, TimelineMonth, TimelineYear, Year

```razor
<SfSchedule TValue="AppointmentData" Height="650px" @bind-CurrentView="@CurrentView">
    <!-- ScheduleViews here -->
</SfSchedule>

@code {
    View CurrentView = View.Month;
}
```

## Individual View Customization

Each Scheduler view can be customized with its own options:

```razor
<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleViews>
        <ScheduleView Option="View.Week" StartHour="07:00" EndHour="15:00"></ScheduleView>
        <ScheduleView Option="View.WorkWeek" StartHour="10:00" EndHour="18:00"></ScheduleView>
        <ScheduleView Option="View.Month" MaxEventsPerRow="2" ShowWeekend="false"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2020, 2, 13);
}
