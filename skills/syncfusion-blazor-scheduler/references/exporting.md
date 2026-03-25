# Exporting and Importing Events

## Table of Contents
- [Overview](#overview)
- [Excel Export](#excel-export)
- [Excel Export with Custom Fields](#excel-export-with-custom-fields)
- [Exporting Recurring Event Occurrences](#exporting-recurring-event-occurrences)
- [Exporting Custom Event Data](#exporting-custom-event-data)
- [Custom Column Headers](#custom-column-headers)
- [Export with Custom Filename](#export-with-custom-filename)
- [Excel File Formats](#excel-file-formats)
- [Export with Date Format](#export-with-date-format)
- [Customize Excel Sheet Before Export](#customize-excel-sheet-before-export)
- [ICS Calendar Export](#ics-calendar-export)
- [ICS Export with Custom Filename](#ics-export-with-custom-filename)
- [Importing Events from ICS Files](#importing-events-from-ics-files)
- [Printing Scheduler](#printing-scheduler)
- [Printing with Options](#printing-with-options)

## Overview

The Scheduler provides comprehensive export and import functionality:
- **Excel Export:** Export appointments to .xlsx or .csv formats with ExportToExcelAsync
- **ICS Export:** Export to iCalendar format (.ics) for Outlook, Google Calendar compatibility
- **ICS Import:** Import events from external ICS files using ImportICalendarAsync
- **Printing:** Print Scheduler content using PrintAsync with optional formatting
- **Customization:** Customize exported data with fields, headers, formats, and date formatting
- **Recurring Events:** Export recurring occurrences as individual records

## Excel Export

Export all Scheduler appointments to an Excel file using `ExportToExcelAsync` method:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Buttons

<SfButton Content="Export to Excel" OnClick="OnExportToExcel"></SfButton>

<SfSchedule @ref="ScheduleRef" TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    SfSchedule<AppointmentData> ScheduleRef;
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Conference", 
            Location = "New York",
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        },
        new AppointmentData 
        { 
            Id = 2, 
            Subject = "Vacation", 
            Location = "Tokyo",
            StartTime = new DateTime(2026, 3, 25, 10, 0, 0), 
            EndTime = new DateTime(2026, 3, 25, 12, 0, 0) 
        }
    };

    public async Task OnExportToExcel()
    {
        // Export all events with default fields to Excel
        await ScheduleRef.ExportToExcelAsync();
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

**Default Behavior:**
- Exports all fields mapped in ScheduleEventSettings
- Downloads as `Schedule.xlsx`
- Default format is XLSX
- Includes all appointments in DataSource

## Excel Export with Custom Fields

Export only specific fields by defining the `Fields` array:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Buttons

<SfButton Content="Export Selected Fields" OnClick="OnExportCustomFields"></SfButton>

<SfSchedule @ref="ScheduleRef" TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    SfSchedule<AppointmentData> ScheduleRef;
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            Location = "Room A",
            Description = "Team sync",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 10, 0, 0) 
        }
    };

    public async Task OnExportCustomFields()
    {
        // Export only Id, Subject, StartTime, and EndTime fields
        ExportOptions options = new ExportOptions 
        { 
            Fields = new string[] { "Id", "Subject", "StartTime", "EndTime" }
        };
        
        await ScheduleRef.ExportToExcelAsync(options);
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public string Location { get; set; }
        public string Description { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**Fields Property:** Array of field names to include in export (Id, Subject, Location, StartTime, EndTime, etc.)

## Exporting Recurring Event Occurrences

Export recurring events as individual occurrences by setting `IncludeOccurrences` to true:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Buttons

<SfButton Content="Export with Occurrences" OnClick="OnExportOccurrences"></SfButton>

<SfSchedule @ref="ScheduleRef" TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    SfSchedule<AppointmentData> ScheduleRef;
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Daily Standup", 
            RecurrenceRule = "FREQ=DAILY;COUNT=10",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 9, 30, 0) 
        }
    };

    public async Task OnExportOccurrences()
    {
        // Export recurring events as individual occurrences
        ExportOptions options = new ExportOptions 
        { 
            IncludeOccurrences = true  // Default is false
        };
        
        await ScheduleRef.ExportToExcelAsync(options);
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
        public string RecurrenceRule { get; set; }
        public string RecurrenceException { get; set; }
        public Nullable<int> RecurrenceID { get; set; }
    }
}
```

**IncludeOccurrences Behavior:**
- `false` (default) - Exports recurring event once with RecurrenceRule
- `true` - Exports each occurrence as separate row in Excel
- Useful for flat data representation of recurring events

## Exporting Custom Event Data

Export specific subset of data by passing `CustomData` collection:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Buttons

<SfButton Content="Export Selected Events" OnClick="OnExportCustomData"></SfButton>

<SfSchedule @ref="ScheduleRef" TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    SfSchedule<AppointmentData> ScheduleRef;
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { Id = 1, Subject = "Meeting 1", Priority = "High", StartTime = new DateTime(2026, 3, 24, 9, 0, 0), EndTime = new DateTime(2026, 3, 24, 10, 0, 0) },
        new AppointmentData { Id = 2, Subject = "Meeting 2", Priority = "Low", StartTime = new DateTime(2026, 3, 24, 11, 0, 0), EndTime = new DateTime(2026, 3, 24, 12, 0, 0) },
        new AppointmentData { Id = 3, Subject = "Meeting 3", Priority = "High", StartTime = new DateTime(2026, 3, 24, 14, 0, 0), EndTime = new DateTime(2026, 3, 24, 15, 0, 0) }
    };

    public async Task OnExportCustomData()
    {
        // Export only high-priority meetings
        var highPriorityEvents = DataSource.Where(e => e.Priority == "High").ToList();
        
        ExportOptions options = new ExportOptions 
        { 
            CustomData = highPriorityEvents
        };
        
        await ScheduleRef.ExportToExcelAsync(options);
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public string Priority { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**CustomData Property:** Pass custom collection instead of default DataSource

## Custom Column Headers

Customize Excel column header text using `ExportFieldInfo`:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Buttons

<SfButton Content="Export with Custom Headers" OnClick="OnExportCustomHeaders"></SfButton>

<SfSchedule @ref="ScheduleRef" TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    SfSchedule<AppointmentData> ScheduleRef;
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Conference", 
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        }
    };

    public async Task OnExportCustomHeaders()
    {
        // Define custom header text for fields
        List<ExportFieldInfo> exportFields = new List<ExportFieldInfo>
        {
            new ExportFieldInfo { Name = "Id", DisplayName = "Event ID" },
            new ExportFieldInfo { Name = "Subject", DisplayName = "Event Title" },
            new ExportFieldInfo { Name = "StartTime", DisplayName = "Start Date/Time" },
            new ExportFieldInfo { Name = "EndTime", DisplayName = "End Date/Time" }
        };
        
        ExportOptions options = new ExportOptions 
        { 
            Fields = new string[] { "Id", "Subject", "StartTime", "EndTime" },
            FieldsInfo = exportFields
        };
        
        await ScheduleRef.ExportToExcelAsync(options);
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**ExportFieldInfo Properties:**
- `Name` - Original field name
- `DisplayName` - Header text in exported file

## Export with Custom Filename

Specify custom filename for exported Excel file:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Buttons

<SfButton Content="Export with Custom Name" OnClick="OnExportCustomName"></SfButton>

<SfSchedule @ref="ScheduleRef" TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    SfSchedule<AppointmentData> ScheduleRef;
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Team Meeting", 
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        }
    };

    public async Task OnExportCustomName()
    {
        // Export with custom filename
        ExportOptions options = new ExportOptions 
        { 
            FileName = "TeamSchedule_March2026"  // Downloads as TeamSchedule_March2026.xlsx
        };
        
        await ScheduleRef.ExportToExcelAsync(options);
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**Default Filename:** `Schedule.xlsx` (when FileName not specified)

## Excel File Formats

Export in XLSX or CSV format using `ExportType` property:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Buttons

<SfButton Content="Export as XLSX" OnClick="OnExportXlsx"></SfButton>
<SfButton Content="Export as CSV" OnClick="OnExportCsv"></SfButton>

<SfSchedule @ref="ScheduleRef" TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    SfSchedule<AppointmentData> ScheduleRef;
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Conference", 
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        }
    };

    public async Task OnExportXlsx()
    {
        ExportOptions options = new ExportOptions 
        { 
            ExportType = ExcelFormat.Xlsx  // Default format
        };
        
        await ScheduleRef.ExportToExcelAsync(options);
    }

    public async Task OnExportCsv()
    {
        ExportOptions options = new ExportOptions 
        { 
            ExportType = ExcelFormat.Csv
        };
        
        await ScheduleRef.ExportToExcelAsync(options);
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**ExportType Options:**
- `ExcelFormat.Xlsx` - Excel file format (.xlsx) - DEFAULT
- `ExcelFormat.Csv` - Comma-separated values (.csv)

## Export with Date Format

Specify custom date format for exported fields:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Buttons

<SfButton Content="Export with Date Format" OnClick="OnExportDateFormat"></SfButton>

<SfSchedule @ref="ScheduleRef" TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    SfSchedule<AppointmentData> ScheduleRef;
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 14, 30, 0), 
            EndTime = new DateTime(2026, 3, 24, 15, 30, 0) 
        }
    };

    public async Task OnExportDateFormat()
    {
        // Export with 24-hour time format: "03/24/2026 14:30:00"
        ExportOptions options = new ExportOptions 
        { 
            DateFormat = "MM/dd/yyyy H:mm:ss"  // 24-hour format
        };
        
        await ScheduleRef.ExportToExcelAsync(options);
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**DateFormat Examples:**
- `"MM/dd/yyyy H:mm:ss"` - 24-hour format
- `"MM/dd/yyyy h:mm:ss tt"` - 12-hour AM/PM format
- `"yyyy-MM-dd HH:mm"` - ISO format
- Uses MSDN date format strings

## Customize Excel Sheet Before Export

Customize Excel sheet formatting using `OnExcelExporting` event and `ExcelExport` API:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Buttons
@using Syncfusion.XlsIO

<SfButton Content="Export with Customization" OnClick="OnExportCustom"></SfButton>

<SfSchedule @ref="ScheduleRef" TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" OnExcelExporting="OnExcelExporting"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    SfSchedule<AppointmentData> ScheduleRef;
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Conference", 
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        }
    };

    public async Task OnExportCustom()
    {
        ExportOptions options = new ExportOptions 
        { 
            FileName = "CustomSchedule"
        };
        
        await ScheduleRef.ExportToExcelAsync(options);
    }

    private void OnExcelExporting(ExcelExportEventArgs args)
    {
        // Access the Excel worksheet
        var worksheet = args.Worksheet;
        
        if (worksheet != null)
        {
            // Customize header row formatting
            var headerRow = worksheet.Range[1, 1, 1, worksheet.UsedRange.LastColumn];
            headerRow.CellStyle.Font.Bold = true;
            headerRow.CellStyle.Font.Size = 12;
            headerRow.CellStyle.Interior.Color = System.Drawing.Color.LightGray;
            
            // Auto-fit columns
            worksheet.AutofitColumns();
            
            // Add custom header
            worksheet.InsertRow(1, 1);
            worksheet.Range[1, 1].Text = "Schedule Export Report";
            worksheet.Range[1, 1].CellStyle.Font.Bold = true;
            worksheet.Range[1, 1].CellStyle.Font.Size = 14;
        }
        
        // Can also set args.Cancel = true to prevent export
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**OnExcelExporting Event:**
- Access and modify worksheet before download
- Apply formatting (bold, colors, fonts)
- Add custom rows/headers
- Cancel export with `args.Cancel = true`

## ICS Calendar Export

Export Scheduler appointments to iCalendar (.ics) format for Outlook, Google Calendar:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Buttons

<SfButton Content="Export to Calendar" OnClick="OnExportICS"></SfButton>

<SfSchedule @ref="ScheduleRef" TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    SfSchedule<AppointmentData> ScheduleRef;
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Team Meeting", 
            Location = "Conference Room",
            Description = "Weekly sync",
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        }
    };

    public async Task OnExportICS()
    {
        // Export all events to iCalendar format (.ics file)
        await ScheduleRef.ExportToICalendarAsync();
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public string Location { get; set; }
        public string Description { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
        public string RecurrenceRule { get; set; }
        public string RecurrenceException { get; set; }
        public Nullable<int> RecurrenceID { get; set; }
    }
}
```

**ICS Format Benefits:**
- Compatible with Outlook, Google Calendar, Apple Calendar
- Supports recurring events
- Includes location, description, and other fields
- Standard iCalendar (RFC 5545) format
- Downloads as `Calendar.ics` by default

## ICS Export with Custom Filename

Specify custom filename for ICS export:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Buttons

<SfButton Content="Export Calendar with Custom Name" OnClick="OnExportICSCustom"></SfButton>

<SfSchedule @ref="ScheduleRef" TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    SfSchedule<AppointmentData> ScheduleRef;
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Training", 
            StartTime = new DateTime(2026, 3, 24, 14, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 15, 30, 0) 
        }
    };

    public async Task OnExportICSCustom()
    {
        // Export with custom filename
        await ScheduleRef.ExportToICalendarAsync("TeamEvents");  // Downloads as TeamEvents.ics
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**Default Filename:** `Calendar.ics` (when custom name not provided)

## Importing Events from ICS Files

Import events from external ICS files using `ImportICalendarAsync` with file uploader:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Inputs
@using System.IO

<SfUploader AllowedExtensions=".ics" Multiple="false">
    <UploaderButtons Browse="Choose ICS File"></UploaderButtons>
    <UploaderEvents ValueChange="OnFileSelect"></UploaderEvents>
</SfUploader>

<SfSchedule @ref="ScheduleRef" TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    SfSchedule<AppointmentData> ScheduleRef;
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>();

    public async Task OnFileSelect(UploadChangeEventArgs args)
    {
        foreach (var file in args.Files)
        {
            var stream = file.Stream;
            
            // Read file content as string
            using (var reader = new StreamReader(stream))
            {
                var fileContent = await reader.ReadToEndAsync();
                
                // Import ICS content into Scheduler
                await ScheduleRef.ImportICalendarAsync(fileContent);
            }
        }
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

**Import Behavior:**
- ImportICalendarAsync accepts ICS file content as string
- Parses VEVENT entries from ICS
- Adds imported events to Scheduler DataSource
- Supports recurring events with RRULE
- Merges with existing appointments (doesn't replace)

## Printing Scheduler

Print Scheduler content with current view using `PrintAsync` method:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Buttons

<SfButton Content="Print Scheduler" OnClick="OnPrint"></SfButton>

<SfSchedule @ref="ScheduleRef" TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    SfSchedule<AppointmentData> ScheduleRef;
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Conference", 
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        }
    };

    public async Task OnPrint()
    {
        // Print current view without options
        await ScheduleRef.PrintAsync();
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**Print Behavior:**
- Prints current active view
- Opens browser print dialog
- User controls paper size, orientation, margins
- Includes all visible appointments in current view

## Printing with Options

Print with custom dimensions using `PrintOptions`:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Buttons

<SfButton Content="Print with Custom Size" OnClick="OnPrintCustom"></SfButton>

<SfSchedule @ref="ScheduleRef" TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    SfSchedule<AppointmentData> ScheduleRef;
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Team Meeting", 
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 30, 0) 
        }
    };

    public async Task OnPrintCustom()
    {
        // Print with custom width and height
        PrintOptions options = new PrintOptions 
        { 
            Width = "1000px",
            Height = "800px"
        };
        
        await ScheduleRef.PrintAsync(options);
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**PrintOptions Properties:**
- `Width` - Print area width (e.g., "1000px", "100%")
- `Height` - Print area height (e.g., "800px")
- Allows scaling of printed content
- User still controls final print settings

## Export and Import Summary

| Operation | Method | Output | Custom Options |
|-----------|--------|--------|-----------------|
| Excel Export | ExportToExcelAsync | .xlsx / .csv | Fields, Custom Data, Headers, FileName, DateFormat |
| ICS Export | ExportToICalendarAsync | .ics | Custom FileName |
| ICS Import | ImportICalendarAsync | Merged into DataSource | File content string |
| Print | PrintAsync | Browser print dialog | Width, Height |
| Customize Excel | OnExcelExporting event | Modified worksheet | Formatting, styles, content |

## Common Patterns

### Export Only High-Priority Events
Use CustomData option to filter and export specific subset of appointments

### Integrate with Outlook
Export to ICS format and open in Outlook, Google Calendar, or Apple Calendar

### Backup Events
Regular Excel export provides backup of all scheduling data

### Report Generation
Combine ExportToExcelAsync with custom headers and date formatting for professional reports

### Import External Calendars
Support importing calendars shared by team members or external partners via ICS
