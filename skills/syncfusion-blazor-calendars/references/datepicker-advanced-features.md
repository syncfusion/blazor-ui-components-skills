# Advanced Features in Blazor DatePicker

## Table of Contents
- [Islamic Calendar](#islamic-calendar)
- [DateOnly Support](#dateonly-support)
- [Globalization and Localization](#globalization-and-localization)
- [Week Numbers](#week-numbers)
- [View Modes](#view-modes)
- [Mask Support](#mask-support)

## Islamic Calendar

Support for Islamic (Hijri) calendar alongside Gregorian calendar.

### Basic Islamic Calendar

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              CalendarMode="CalendarType.Islamic">
</SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
}
```

**Result:** Calendar displays Islamic dates, with Gregorian equivalent shown.

### Toggle Between Calendars

```razor
<div>
    <label>Calendar Type:</label>
    <input type="radio" name="calendar" value="gregorian" 
           @onchange="@((ChangeEventArgs e) => ToggleCalendar(false))" />
    <span>Gregorian</span>
    
    <input type="radio" name="calendar" value="islamic" 
           @onchange="@((ChangeEventArgs e) => ToggleCalendar(true))" />
    <span>Islamic</span>
</div>

<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              CalendarMode="@calendarMode">
</SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
    CalendarType calendarMode = CalendarType.Gregorian;

    void ToggleCalendar(bool isIslamic)
    {
        calendarMode = isIslamic ? CalendarType.Islamic : CalendarType.Gregorian;
    }
}
```

## DateOnly Support

Support for .NET 6+ `DateOnly` type (date without time component).

### Basic DateOnly

```razor
<SfDatePicker TValue="DateOnly" Value="@selectedDate"></SfDatePicker>

@code {
    DateOnly selectedDate = new DateOnly(2026, 3, 18);
}
```

**Advantage:** `DateOnly` explicitly represents date-only data (no time confusion).

### DateOnly with Binding

```razor
<SfDatePicker TValue="DateOnly" @bind-Value="@selectedDate"></SfDatePicker>

<p>Selected: @selectedDate.ToString("dddd, dd MMMM yyyy")</p>

@code {
    DateOnly selectedDate = DateOnly.FromDateTime(DateTime.Now);
}
```

### Convert Between DateTime and DateOnly

```razor
<!-- Input with DateTime -->
<SfDatePicker TValue="DateTime?" @bind-Value="@dateTime"></SfDatePicker>

<!-- Display as DateOnly -->
@if (dateTime.HasValue)
{
    <p>As DateOnly: @DateOnly.FromDateTime(dateTime.Value)</p>
}

@code {
    DateTime? dateTime;
}
```

### DateOnly in Forms

```razor
<EditForm Model="@employee">
    <SfDatePicker TValue="DateOnly" @bind-Value="@employee.HireDate"></SfDatePicker>
    <button type="submit">Submit</button>
</EditForm>

@code {
    Employee employee = new();

    class Employee
    {
        public DateOnly HireDate { get; set; }
    }
}
```

## Globalization and Localization

Support multiple languages and regional formats.

**Supported Locales:**
- `en` - English
- `de` - German
- `fr` - French
- `es` - Spanish
- `it` - Italian
- `ja` - Japanese
- `zh` - Chinese
- `ar` - Arabic
- And many more...

### RTL Support

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              EnableRtl="true">
</SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
}
```

**RTL (Right-to-Left)** is automatically enabled for Arabic, Hebrew, Persian locales.

Register custom locale in your resource files or localization setup.

## Week Numbers

Display week numbers in the calendar grid.

### Enable Week Numbers

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              WeekNumber="true">
</SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
}
```

**Result:** Calendar shows week number (1-52) on the left.

### Week Number with Date Range

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              WeekNumber="true"
              Min="@new DateTime(2026, 1, 1)"
              Max="@new DateTime(2026, 12, 31)">
</SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
}
```

Useful for project planning, fiscal calendars, or medical studies.

## View Modes

Switch between Month, Year, and Decade views.

### Month View (Default)

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              Start="CalendarView.Month">
</SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
}
```

### Year View

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              Start="CalendarView.Year">
</SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
}
```

Opens calendar showing 12 months (January-December).

### Decade View

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              Start="CalendarView.Decade">
</SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
}
```

Opens calendar showing 10 years at a time.

### Multi-View Navigation

```razor
<div>
    <label>Jump to:</label>
    <select @onchange="@((ChangeEventArgs e) => ChangeView(e.Value.ToString()))">
        <option value="Month">Month View</option>
        <option value="Year">Year View</option>
        <option value="Decade">Decade View</option>
    </select>
</div>

<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              Start="@viewDepth">
</SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
    CalendarView viewDepth = CalendarView.Month;

    void ChangeView(string view)
    {
        viewDepth = view switch
        {
            "Month" => CalendarView.Month,
            "Year" => CalendarView.Year,
            "Decade" => CalendarView.Decade,
            _ => CalendarView.Month
        };
    }
}
```

## Mask Support

Input masking for guided date entry.

### Basic Mask

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              Format="dd/MM/yyyy"
              Placeholder="DD/MM/YYYY">
</SfDatePicker>

@code {
    DateTime? selectedDate;
}
```

User must follow `DD/MM/YYYY` format with visual guidance.

### Numeric Mask

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              Format="MM/dd/yyyy"
              Placeholder="MM/DD/YYYY">
</SfDatePicker>

@code {
    DateTime? selectedDate;
}
```

Guides users through US-style date entry.

### ISO Format Mask

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              Format="yyyy-MM-dd"
              Placeholder="YYYY-MM-DD">
</SfDatePicker>

@code {
    DateTime? selectedDate;
}
```

Enforces international standard ISO 8601 format.

## Advanced Patterns

### Date Picker with Search Filter

```razor
<div>
    <input type="text" placeholder="Search dates..." @onchange="@OnSearch" />
</div>

<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              DisabledDates="@disabledDates"
              SpecialDates="@filteredSpecialDates">
</SfDatePicker>

@code {
    DateTime? selectedDate;
    int[] disabledDates;
    List<SpecialDate> filteredSpecialDates = new();

    void OnSearch(ChangeEventArgs args)
    {
        string searchText = args.Value.ToString();
        // Filter special dates based on search
    }
}
```

### Multi-Date Picker Coordination

```razor
<div>
    <label>From Date:</label>
    <SfDatePicker TValue="DateTime?" @bind-Value="@startDate" Max="@endDate"></SfDatePicker>
</div>

<div>
    <label>To Date:</label>
    <SfDatePicker TValue="DateTime?" @bind-Value="@endDate" Min="@startDate"></SfDatePicker>
</div>

<p>Duration: @CalculateDuration() days</p>

@code {
    DateTime? startDate = new DateTime(2026, 3, 1);
    DateTime? endDate = new DateTime(2026, 3, 31);

    int CalculateDuration()
    {
        if (startDate.HasValue && endDate.HasValue)
        {
            return (endDate.Value - startDate.Value).Days;
        }
        return 0;
    }
}
```

### Fiscal Year DatePicker

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              WeekNumber="true"
              Format="dd/MM/yyyy"
              Min="@new DateTime(2026, 4, 1)"
              Max="@new DateTime(2027, 3, 31)">
</SfDatePicker>

<p>Fiscal Year: @GetFiscalYear(selectedDate)</p>

@code {
    DateTime? selectedDate;

    string GetFiscalYear(DateTime? date)
    {
        if (!date.HasValue) return "";
        return date.Value.Month >= 4 
            ? $"FY{date.Value.Year}-{date.Value.Year + 1}" 
            : $"FY{date.Value.Year - 1}-{date.Value.Year}";
    }
}
```

## Troubleshooting

### Issue: Islamic calendar dates not displaying
**Solution:** Set `CalendarMode="CalendarType.Islamic"` and ensure Syncfusion localization resources are loaded

### Issue: DateOnly type not recognized
**Solution:** Ensure .NET 6+ and Syncfusion.Blazor 19.4.56+ version

### Issue: Locale not changing language
**Solution:** Verify locale code is supported (check Syncfusion docs for full list)

### Issue: Week numbers not showing
**Solution:** Set `WeekNumber="true"` and confirm theme CSS is loaded

## Next Steps

- **Events:** Handle [advanced event patterns](events.md)
- **Styling:** Apply [custom themes and localized styles](styling-and-appearance.md)
- **Accessibility:** Ensure [keyboard and screen reader support](accessibility.md)
