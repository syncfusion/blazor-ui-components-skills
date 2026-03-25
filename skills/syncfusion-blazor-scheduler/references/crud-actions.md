# CRUD Actions

## Table of Contents
- [Overview](#overview)
- [Add Appointments](#add-appointments)
- [Edit Appointments](#edit-appointments)
- [Delete Appointments](#delete-appointments)
- [Drag and Drop](#drag-and-drop)
- [Resize](#resize)
- [Recurring Events Handling](#recurring-events-handling)

## Overview

CRUD (Create, Read, Update, Delete) operations allow you to manipulate appointments programmatically or through the UI. Appointments can be added via the editor window, quick popup, inline editing, or the `AddEventAsync()` method. Similarly, they can be edited, deleted, dragged, and resized using various approaches.

## Add Appointments

### Creation Using Editor Window

Double-click on scheduler cells to open the default editor window with fields for Subject, Location, Start/End time, All-day, Timezone, Description, and recurrence options. Fill in the fields and click Save to create the appointment.

For quick creation, single-click a cell to open a quick popup with just the Subject field, or select multiple cells and press Enter.

### Creation Using AddEventAsync Method

Programmatically create appointments using the `AddEventAsync()` method:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Buttons

<SfButton Content="ADD" OnClick="OnClick"></SfButton>

<SfSchedule @ref="ScheduleRef" TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
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
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    SfSchedule<AppointmentData> ScheduleRef;
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
    
    public async Task OnClick()
    {
        AppointmentData eventData = new AppointmentData
        {
            Id = 10,
            Subject = "Added Event",
            StartTime = new DateTime(2026, 3, 25, 9, 30, 0),
            EndTime = new DateTime(2026, 3, 25, 11, 30, 0),
        };
        await ScheduleRef.AddEventAsync(eventData);
    }
    
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
}
```

### Inline Creation

Enable the `AllowInline` property to create appointments quickly. Single-click a cell or press Enter on selected cells to display an inline textbox. Enter the Subject and press Enter or click outside to create the appointment:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" AllowInline="true" @bind-SelectedDate="@CurrentDate">
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
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0),
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
        public string Description { get; set; }
        public bool IsAllDay { get; set; }
        public string RecurrenceRule { get; set; }
        public string RecurrenceException { get; set; }
        public Nullable<int> RecurrenceID { get; set; }
    }
}
```

### Restricting Add Action Based on Criteria

Use `OnActionBegin` event to validate and prevent appointment creation based on business logic:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Width="100%" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" OnActionBegin="OnActionBegin"></ScheduleEvents>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month" MaxEventsPerRow="2"></ScheduleView>
    </ScheduleViews>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    public void OnActionBegin(ActionEventArgs<AppointmentData> args)
    {
        // Prevent creation on weekends
        if (args.ActionType == ActionType.EventCreate)
        {
            int[] weekEnds = new int[2] { 0, 6 };
            AppointmentData data = args.AddedRecords[0];
            DateTime date = data.StartTime;
            int weekDay = (int)date.DayOfWeek;
            
            if (weekDay == weekEnds[0] || weekDay == weekEnds[1])
            {
                args.Cancel = true;
                Console.WriteLine("Appointments cannot be created on weekends");
            }
        }
    }
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 11, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 12, 30, 0) 
        },
        new AppointmentData 
        { 
            Id = 2, 
            Subject = "Conference", 
            StartTime = new DateTime(2026, 3, 27, 18, 0, 0),
            EndTime = new DateTime(2026, 3, 27, 19, 30, 0) 
        }
    };

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
}
```

### Validation with ScheduleField

Add field validation to the editor window using `ScheduleField`:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource">
        <ScheduleField>
            <FieldSubject Name="Subject" Validation="@ValidationRules"></FieldSubject>
            <FieldLocation Name="Location" Validation="@LocationValidationRules"></FieldLocation>
            <FieldDescription Name="Description" Validation="@DescriptionValidationRules"></FieldDescription>
            <FieldStartTime Name="StartTime" Validation="@ValidationRules"></FieldStartTime>
            <FieldEndTime Name="EndTime" Validation="@ValidationRules"></FieldEndTime>
        </ScheduleField>
    </ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
        <ScheduleView Option="View.Agenda"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    static Dictionary<string, object> ValidationMessages = new Dictionary<string, object>() 
    { 
        { "regex", "Special character(s) not allowed in this field" } 
    };
    
    ValidationRules ValidationRules = new ValidationRules { Required = true };
    ValidationRules LocationValidationRules = new ValidationRules 
    { 
        Required = true, 
        RegexPattern = "^[a-zA-Z0-9- ]*$", 
        Messages = ValidationMessages 
    };
    ValidationRules DescriptionValidationRules = new ValidationRules 
    { 
        Required = true, 
        MinLength = 5, 
        MaxLength = 500 
    };

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0),
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
        public string Description { get; set; }
        public bool IsAllDay { get; set; }
        public string RecurrenceRule { get; set; }
        public string RecurrenceException { get; set; }
        public Nullable<int> RecurrenceID { get; set; }
    }
}
```

## Edit Appointments

### Update Using Editor Window

Double-click an appointment to open the editor pre-filled with existing details. Modify the fields and click Save to update. You can also single-click to open the quick info popup with Edit and Delete options.

### Update Using SaveEventAsync Method

Programmatically update appointments using `SaveEventAsync()`. The `Id` field is mandatory:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Buttons

<SfButton Content="EDIT" OnClick="OnClick"></SfButton>

<SfSchedule @ref="ScheduleRef" TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
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
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    SfSchedule<AppointmentData> ScheduleRef;
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Testing", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0)
        },
        new AppointmentData 
        { 
            Id = 2, 
            Subject = "Conference", 
            StartTime = new DateTime(2026, 3, 29, 9, 30, 0),
            EndTime = new DateTime(2026, 3, 29, 11, 0, 0)
        }
    };
    
    public async Task OnClick()
    {
        AppointmentData eventData = new AppointmentData
        {
            Id = 1,
            Subject = "Edited",
            StartTime = new DateTime(2026, 3, 24, 10, 30, 0),
            EndTime = new DateTime(2026, 3, 24, 12, 0, 0),
        };
        await ScheduleRef.SaveEventAsync(eventData);
    }
    
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
}
```

### Inline Editing

Enable the `AllowInline` property to edit appointment Subject directly. Single-click an appointment to edit, then press Enter or click outside to save:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" AllowInline="true" @bind-SelectedDate="@CurrentDate">
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
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0),
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
        public string Description { get; set; }
        public bool IsAllDay { get; set; }
        public string RecurrenceRule { get; set; }
        public string RecurrenceException { get; set; }
        public Nullable<int> RecurrenceID { get; set; }
    }
}
```

### Restricting Edit Action Based on Criteria

Use `OnActionBegin` event to prevent editing during non-working hours:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Width="100%" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleWorkHours Highlight="true" Start="@StartHour" End="@EndHour"></ScheduleWorkHours>
    <ScheduleEvents TValue="AppointmentData" OnActionBegin="OnActionBegin"></ScheduleEvents>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month" MaxEventsPerRow="2"></ScheduleView>
    </ScheduleViews>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    public string StartHour { get; set; } = "09:00";
    public string EndHour { get; set; } = "18:00";
    
    public void OnActionBegin(ActionEventArgs<AppointmentData> args)
    {
        // Prevent editing outside work hours
        if (args.ActionType == ActionType.EventChange)
        {
            bool workHours = false, flag = false;
            int[] weekEnds = new int[2] { 0, 6 };
            AppointmentData data = args.ChangedRecords[0];
            DateTime date = data.StartTime;
            int weekDay = (int)date.DayOfWeek;
            
            if (weekDay == weekEnds[0] || weekDay == weekEnds[1])
            {
                flag = true;
            }
            
            int hour = data.StartTime.Hour;
            int workHoursStart = Convert.ToInt32(StartHour.Substring(0, 2));
            int workHoursEnd = Convert.ToInt32(EndHour.Substring(0, 2));
            
            if (workHoursStart <= hour && workHoursEnd > hour)
            {
                workHours = true;
            }
            
            if (flag || !workHours)
            {
                args.Cancel = true;
                Console.WriteLine("Cannot edit outside working hours");
            }
        }
    }
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 30, 0) 
        },
        new AppointmentData 
        { 
            Id = 2, 
            Subject = "Conference", 
            StartTime = new DateTime(2026, 3, 27, 18, 0, 0),
            EndTime = new DateTime(2026, 3, 27, 19, 30, 0) 
        }
    };

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
}
```

## Delete Appointments

### Deletion Methods

Delete appointments by:
1. Selecting and clicking delete icon from quick popup
2. Selecting and pressing Delete key
3. Selecting multiple appointments and pressing Delete key
4. Double-clicking to open editor and clicking Delete button

A confirmation popup will appear before deletion (except when using editor window).

### Deletion Using DeleteEventAsync Method

**Normal Events** - Delete by Id or event object:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Buttons

<SfButton Content="DELETE" OnClick="OnClick"></SfButton>

<SfSchedule @ref="ScheduleRef" TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
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
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    SfSchedule<AppointmentData> ScheduleRef;
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Testing", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0)
        },
        new AppointmentData 
        { 
            Id = 2, 
            Subject = "Conference", 
            StartTime = new DateTime(2026, 3, 28, 9, 30, 0),
            EndTime = new DateTime(2026, 3, 28, 11, 0, 0)
        }
    };
    
    public async Task OnClick()
    {
        await ScheduleRef.DeleteEventAsync(2);
    }
    
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
}
```

**Recurring Events** - Delete single occurrence or entire series:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Buttons

<SfButton Content="DELETE SERIES" OnClick="OnClick"></SfButton>

<SfSchedule @ref="ScheduleRef" TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
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
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    SfSchedule<AppointmentData> ScheduleRef;
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0),
            RecurrenceRule = "FREQ=DAILY;INTERVAL=1;COUNT=3"
        },
        new AppointmentData 
        { 
            Id = 2, 
            Subject = "Testing", 
            StartTime = new DateTime(2026, 3, 25, 12, 0, 0),
            EndTime = new DateTime(2026, 3, 25, 13, 30, 0),
            RecurrenceRule = "FREQ=DAILY;INTERVAL=1;COUNT=3"
        }
    };
    
    public async Task OnClick()
    {
        AppointmentData eventData = new AppointmentData
        {
            Id = 2,
            Subject = "Testing",
            StartTime = new DateTime(2026, 3, 25, 12, 0, 0),
            EndTime = new DateTime(2026, 3, 25, 13, 30, 0),
            RecurrenceID = 2,
            RecurrenceRule = "FREQ=DAILY;INTERVAL=1;COUNT=3"
        };
        // Delete entire series
        await ScheduleRef.DeleteEventAsync(eventData, CurrentAction.DeleteSeries);
    }
    
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
}
```

## Drag and Drop

When you drag and drop a normal event, the event is edited. For recurring events, by default only the single occurrence is edited (batch action occurs - both Add and Edit actions happen). A dialog prompts you to choose between editing single occurrence or entire series.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Width="100%" Height="550px" @bind-SelectedDate="@CurrentDate">
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
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0),
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
        public string Description { get; set; }
        public bool IsAllDay { get; set; }
        public string RecurrenceRule { get; set; }
        public string RecurrenceException { get; set; }
        public Nullable<int> RecurrenceID { get; set; }
    }
}
```

## Resize

When you resize a normal event, the event is edited. For recurring events, by default only the single occurrence is edited. A dialog prompts you to choose between resizing single occurrence or entire series.

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Width="100%" Height="550px" @bind-SelectedDate="@CurrentDate">
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
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0),
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
        public string Description { get; set; }
        public bool IsAllDay { get; set; }
        public string RecurrenceRule { get; set; }
        public string RecurrenceException { get; set; }
        public Nullable<int> RecurrenceID { get; set; }
    }
}
```

## Recurring Events Handling

### Editing Single Occurrence

When editing a recurring event, a dialog appears asking whether to edit the single event or entire series. If **EDIT EVENT** is selected:

1. A new event object is created from parent data
2. `RecurrenceID` field holds the parent event Id
3. A new unique `Id` is generated
4. Parent event is updated with `RecurrenceException` containing the edited occurrence date

This results in a batch action with both Add and Edit actions.

### Editing Entire Series

When **EDIT SERIES** is selected:

1. All child occurrences (edited occurrences) are removed from the data source
2. Only the parent recurring event is updated
3. This results in a batch action with both Delete and Edit actions

### Deleting Single Occurrence

When deleting a recurring event, a dialog appears asking whether to delete single event or entire series. If **DELETE EVENT** is selected:

1. The selected occurrence is removed from UI
2. Parent event is updated with `RecurrenceException` containing deleted occurrence date
3. Only an update action occurs on parent event

### Deleting Entire Series

When **DELETE SERIES** is selected:

1. All child occurrences are also removed from data source
2. The entire parent recurring event is deleted
3. A remove action deletes one or more event objects

## Common Patterns

**CRUD Methods Summary:**
- `AddEventAsync(AppointmentData)` - Add new appointment
- `SaveEventAsync(AppointmentData, CurrentAction)` - Update existing appointment
- `DeleteEventAsync(AppointmentData, CurrentAction)` - Delete appointment
- `GetEventsAsync()` - Retrieve all events
- `GetCurrentViewEvents()` - Get events in current view

**Event Validation:**
- Use `ScheduleField` to add field-level validation
- Use `OnActionBegin` event to prevent actions based on business logic
- `ValidationRules` supports Required, RegexPattern, MinLength, MaxLength

**Recurring Event Operations:**
- For single occurrence edits/deletes: Use `CurrentAction.EditOccurrence` or `CurrentAction.DeleteOccurrence`
- For entire series: Use `CurrentAction.EditSeries` or `CurrentAction.DeleteSeries`
- RecurrenceException stores excluded/modified occurrence dates
