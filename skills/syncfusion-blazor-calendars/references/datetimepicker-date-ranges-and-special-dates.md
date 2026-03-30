# Date Ranges & Special Dates

Learn how to set date constraints, highlight special dates, and manage date ranges in the DateTimePicker component.

## Setting Date Range Constraints

### Minimum and Maximum Dates

Restrict users to select dates within a specific range:

```blazor
@page "/datetimepicker-range"
@using Syncfusion.Blazor.Calendars

<SfDateTimePicker TValue="DateTime?" 
                  Min="@minDate" 
                  Max="@maxDate"
                  Value="@selectedDate">
</SfDateTimePicker>

@code {
    private DateTime minDate = new DateTime(2024, 1, 1);
    private DateTime maxDate = new DateTime(2024, 12, 31);
    private DateTime? selectedDate;
}
```

### Dynamic Range Constraints

Update date constraints based on user interactions:

```blazor
@page "/datetimepicker-dynamic-range"
@using Syncfusion.Blazor.Calendars

<div>
    <div>
        <label>Range Type:</label>
        <select @onchange="OnRangeChanged">
            <option value="quarter">This Quarter</option>
            <option value="year">This Year</option>
            <option value="custom">Last 30 Days</option>
        </select>
    </div>
    
    <div style="margin-top: 10px;">
        <SfDateTimePicker TValue="DateTime?" 
                          Min="@minDate" 
                          Max="@maxDate"
                          Placeholder="Select within range">
        </SfDateTimePicker>
    </div>
    
    <p>Allowed range: @minDate.ToString("d") to @maxDate.ToString("d")</p>
</div>

@code {
    private DateTime minDate;
    private DateTime maxDate;
    
    protected override void OnInitialized()
    {
        SetQuarterRange();
    }
    
    private void OnRangeChanged(ChangeEventArgs e)
    {
        var rangeType = e.Value.ToString();
        
        switch (rangeType)
        {
            case "quarter":
                SetQuarterRange();
                break;
            case "year":
                SetYearRange();
                break;
            case "custom":
                SetLastThirtyDaysRange();
                break;
        }
    }
    
    private void SetQuarterRange()
    {
        var now = DateTime.Now;
        var quarter = (now.Month - 1) / 3;
        minDate = new DateTime(now.Year, quarter * 3 + 1, 1);
        maxDate = minDate.AddMonths(3).AddDays(-1);
    }
    
    private void SetYearRange()
    {
        minDate = new DateTime(DateTime.Now.Year, 1, 1);
        maxDate = new DateTime(DateTime.Now.Year, 12, 31);
    }
    
    private void SetLastThirtyDaysRange()
    {
        maxDate = DateTime.Now;
        minDate = maxDate.AddDays(-30);
    }
}
```

## Special Dates

### Highlighting Special Dates

Use `SpecialDates` to highlight specific dates in the calendar:

```blazor
@page "/datetimepicker-special"
@using Syncfusion.Blazor.Calendars

<div>
    <label>Select a date (holidays highlighted in red):</label>
    <SfDateTimePicker TValue="DateTime?" 
                      SpecialDates="@specialDates">
    </SfDateTimePicker>
</div>

@code {
    private List<DateTime> specialDates = new()
    {
        new DateTime(2024, 1, 1),   // New Year
        new DateTime(2024, 7, 4),   // Independence Day
        new DateTime(2024, 12, 25)  // Christmas
    };
}
```

### Dynamic Special Dates

```blazor
@page "/datetimepicker-holidays"
@using Syncfusion.Blazor.Calendars

<div>
    <label>Select Year:</label>
    <input @onchange="OnYearChanged" type="number" value="2024" />
    
    <div style="margin-top: 10px;">
        <SfDateTimePicker TValue="DateTime?" 
                          SpecialDates="@GetHolidaysForYear()">
        </SfDateTimePicker>
    </div>
</div>

@code {
    private int selectedYear = 2024;
    
    private void OnYearChanged(ChangeEventArgs e)
    {
        if (int.TryParse(e.Value?.ToString(), out int year))
        {
            selectedYear = year;
        }
    }
    
    private List<DateTime> GetHolidaysForYear()
    {
        return new()
        {
            new DateTime(selectedYear, 1, 1),    // New Year
            new DateTime(selectedYear, 1, 15),   // MLK Day
            new DateTime(selectedYear, 7, 4),    // Independence Day
            new DateTime(selectedYear, 9, 1),    // Labor Day
            new DateTime(selectedYear, 12, 25)   // Christmas
        };
    }
}
```

## Disabling Specific Dates

### Using DisableDates Method

Prevent selection of specific dates or date ranges:

```blazor
@page "/datetimepicker-disable"
@using Syncfusion.Blazor.Calendars

<div>
    <label>Book a slot (weekends disabled):</label>
    <SfDateTimePicker TValue="DateTime?" 
                      DisabledDates="@GetDisabledDates()">
    </SfDateTimePicker>
</div>

@code {
    private List<DateTime> GetDisabledDates()
    {
        var disabledDates = new List<DateTime>();
        
        // Disable all Saturdays and Sundays for this month
        var today = DateTime.Now;
        for (int i = 0; i < 31; i++)
        {
            var date = today.AddDays(i);
            if (date.DayOfWeek == DayOfWeek.Saturday || 
                date.DayOfWeek == DayOfWeek.Sunday)
            {
                disabledDates.Add(date);
            }
        }
        
        return disabledDates;
    }
}
```

### Disable Past Dates

```blazor
@page "/datetimepicker-future"
@using Syncfusion.Blazor.Calendars

<SfDateTimePicker TValue="DateTime?" 
                  Min="@minFutureDate"
                  Placeholder="Select future date only">
</SfDateTimePicker>

@code {
    private DateTime minFutureDate = DateTime.Now;
}
```

### Disable Specific Days of Week

```blazor
@page "/datetimepicker-weekdays"
@using Syncfusion.Blazor.Calendars

<div>
    <label>Select business day (no weekends):</label>
    <SfDateTimePicker TValue="DateTime?" 
                      DisabledDates="@GetWeekendDates()">
    </SfDateTimePicker>
</div>

@code {
    private List<DateTime> GetWeekendDates()
    {
        var weekends = new List<DateTime>();
        var startDate = DateTime.Now;
        
        // Disable weekends for next 365 days
        for (int i = 0; i < 365; i++)
        {
            var date = startDate.AddDays(i);
            if (date.DayOfWeek == DayOfWeek.Saturday || 
                date.DayOfWeek == DayOfWeek.Sunday)
            {
                weekends.Add(date);
            }
        }
        
        return weekends;
    }
}
```

## Week Number Configuration

### Display Week Numbers

Show ISO 8601 week numbers in the calendar:

```blazor
@page "/datetimepicker-weeks"
@using Syncfusion.Blazor.Calendars

<SfDateTimePicker TValue="DateTime?" 
                  WeekNumber="true">
</SfDateTimePicker>
```

### Using Week Numbers for Selection

```blazor
@page "/datetimepicker-week-select"
@using Syncfusion.Blazor.Calendars

<div>
    <SfDateTimePicker TValue="DateTime?" 
                      WeekNumber="true">
        <DateTimePickerEvents TValue="DateTime?" ValueChange="@OnDateSelected"></DateTimePickerEvents>
    </SfDateTimePicker>
    
    @if (selectedDate.HasValue)
    {
        <p>Selected date: @selectedDate.Value.ToString("dddd, MMMM dd, yyyy")</p>
        <p>Week number: @GetWeekNumber(selectedDate.Value)</p>
    }
</div>

@code {
    private DateTime? selectedDate;
    
    private void OnDateSelected(ChangedEventArgs<DateTime?> args)
    {
        selectedDate = args.Value;
    }
    
    private int GetWeekNumber(DateTime date)
    {
        var culture = System.Globalization.CultureInfo.CurrentCulture;
        var calendar = culture.Calendar;
        return calendar.GetWeekOfYear(date, 
            System.Globalization.CalendarWeekRule.FirstFourDayWeek, 
            DayOfWeek.Monday);
    }
}
```

## Complete Example: Advanced Date Management

Combining multiple features for a comprehensive date selection experience:

```blazor
@page "/datetimepicker-advanced"
@using Syncfusion.Blazor.Calendars

<div style="padding: 20px;">
    <h3>Advanced Date Selection</h3>
    
    <div>
        <label>Select appointment date (business hours only):</label>
        <SfDateTimePicker TValue="DateTime?" 
                          @bind-Value="appointmentDate"
                          Min="@minAppointmentDate"
                          Max="@maxAppointmentDate"
                          DisabledDates="@GetUnavailableDates()"
                          SpecialDates="@GetUrgentDates()"
                          Format="dddd, MMMM dd, yyyy HH:mm"
                          Placeholder="Choose available slot">
        </SfDateTimePicker>
    </div>
    
    @if (appointmentDate.HasValue)
    {
        <div style="margin-top: 20px; padding: 10px; background-color: #f0f0f0; border-radius: 4px;">
            <h4>Appointment Details</h4>
            <p><strong>Date:</strong> @appointmentDate.Value.ToString("D")</p>
            <p><strong>Time:</strong> @appointmentDate.Value.ToString("HH:mm")</p>
            <p><strong>Day:</strong> @appointmentDate.Value.ToString("dddd")</p>
            
            @if (IsUrgent(appointmentDate.Value))
            {
                <p style="color: red;">⚠️ Urgent slot selected</p>
            }
        </div>
    }
</div>

@code {
    private DateTime? appointmentDate;
    private DateTime minAppointmentDate = DateTime.Now;
    private DateTime maxAppointmentDate = DateTime.Now.AddDays(30);
    
    private List<DateTime> GetUnavailableDates()
    {
        var unavailable = new List<DateTime>();
        var startDate = DateTime.Now;
        
        // Disable weekends and past dates
        for (int i = 0; i < 60; i++)
        {
            var date = startDate.AddDays(i);
            if (date.DayOfWeek == DayOfWeek.Saturday || 
                date.DayOfWeek == DayOfWeek.Sunday)
            {
                unavailable.Add(date);
            }
        }
        
        return unavailable;
    }
    
    private List<DateTime> GetUrgentDates()
    {
        // Dates that need urgent attention
        return new()
        {
            DateTime.Now.AddDays(1),
            DateTime.Now.AddDays(2),
            DateTime.Now.AddDays(3)
        };
    }
    
    private bool IsUrgent(DateTime date)
    {
        return GetUrgentDates().Any(d => d.Date == date.Date);
    }
}
```

## Edge Cases & Considerations

### When Min > Max
```blazor
<!-- ❌ Invalid: Min is greater than Max -->
<SfDateTimePicker Min="@new DateTime(2024, 12, 31)" 
                  Max="@new DateTime(2024, 1, 1)">
</SfDateTimePicker>
```

### When Special Dates are Outside Range
```blazor
<!-- Special dates outside Min/Max are ignored -->
<SfDateTimePicker Min="@new DateTime(2024, 6, 1)"
                  Max="@new DateTime(2024, 6, 30)"
                  SpecialDates="@new List<DateTime> { new DateTime(2024, 7, 4) }">
</SfDateTimePicker>
```

---

**Related Topics:**
- [Getting Started](getting-started.md) - Basic setup
- [Masking & Input Validation](masking-and-input-validation.md) - Validate date ranges
- [Date & Time Formatting](date-time-formatting.md) - Format range display
