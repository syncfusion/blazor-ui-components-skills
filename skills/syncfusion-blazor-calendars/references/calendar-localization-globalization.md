# Localization & Globalization

## Table of Contents
- [Locale Support](#locale-support)
- [Right-to-Left (RTL) Layout](#right-to-left-rtl-layout)
- [Islamic Calendar](#islamic-calendar)
- [Week Customization](#week-customization)
- [Date Formatting](#date-formatting)
- [Common Patterns](#common-patterns)

---

## Locale Support

### What is Localization?

Localization adapts the Calendar to different languages and cultures:
- Month/day names in different languages
- Date formats (DD/MM/YYYY vs MM/DD/YYYY)
- Week numbering and first day of week
- Number formatting

### Supported Locales

Syncfusion Calendar supports 150+ locales including:
- English (en)
- Spanish (es)
- French (fr)
- German (de)
- Chinese (zh)
- Japanese (ja)
- Arabic (ar)
- And many more...

### Common Locale Codes

| Language | Code | Region | Example |
|----------|------|--------|---------|
| English | en | United States | en-US |
| Spanish | es | Spain | es-ES |
| French | fr | France | fr-FR |
| German | de | Germany | de-DE |
| Italian | it | Italy | it-IT |
| Chinese | zh | China | zh-CN |
| Japanese | ja | Japan | ja-JP |
| Korean | ko | Korea | ko-KR |
| Arabic | ar | Saudi Arabia | ar-SA |
| Russian | ru | Russia | ru-RU |
| Portuguese | pt | Brazil | pt-BR |


## Right-to-Left (RTL) Layout

### What is RTL?

Right-to-Left layout is required for languages like Arabic, Hebrew, Persian, and Urdu where text flows from right to left.

### Enabling RTL

```razor
<SfCalendar TValue="DateTime?" EnableRtl="true"></SfCalendar>
```

### Property

- **EnableRtl** (bool) - Enables/disables RTL layout

### Important Notes

- RTL is **not** automatically inferred from Locale
- Must explicitly set `EnableRtl="true"` for RTL languages
- Even if you set arabic culture, RTL won't activate without `EnableRtl="true"`

---

## Islamic Calendar

### What is Islamic Calendar?

Islamic Calendar (Hijri) is the lunar calendar used in Muslim cultures. Dates are approximately 11 days behind the Gregorian calendar.

### Using Islamic Calendar

The Calendar can display Islamic dates alongside Gregorian dates.

```razor
@using Syncfusion.Blazor.Calendars

<h3>Islamic Calendar View</h3>

<SfCalendar TValue="DateTime?" EnableRtl="true"></SfCalendar>
```

### Example: Dual Calendar Display

```razor
@using Syncfusion.Blazor.Calendars

<h3>Gregorian & Islamic Calendar</h3>

<div class="calendar-row">
    <div class="calendar-col">
        <h5>Gregorian Calendar</h5>
        <SfCalendar TValue="DateTime?" 
            @bind-Value="@SelectedDate"
            EnableRtl="false">
        </SfCalendar>
    </div>
    
    <div class="calendar-col">
        <h5>Islamic Calendar (Hijri)</h5>
        <SfCalendar TValue="DateTime?" 
            @bind-Value="@SelectedDate"
            EnableRtl="true">
        </SfCalendar>
    </div>
</div>

<p>Selected: @SelectedDate?.ToString("dd/MM/yyyy")</p>

@code {
    public DateTime? SelectedDate { get; set; } = DateTime.Now;
}

<style>
    .calendar-row {
        display: flex;
        gap: 20px;
    }
    
    .calendar-col {
        flex: 1;
    }
</style>
```

### Islamic Date Conversion Notes

- Hijri year ~354 days (lunar), Gregorian ~365 days (solar)
- Hijri year approximately 11 days shorter than Gregorian
- Year 1 AH = July 16, 622 CE (Islamic era start)
- Most Islamic calendars are also RTL

---

## Week Customization

### First Day of Week

By default, Sunday is the first day of the week (0). Different cultures use different first days:

| Day | Code |
|-----|------|
| Sunday | 0 |
| Monday | 1 |
| Tuesday | 2 |
| Wednesday | 3 |
| Thursday | 4 |
| Friday | 5 |
| Saturday | 6 |

### Setting First Day

```razor
<SfCalendar TValue="DateTime?" FirstDayOfWeek="1"></SfCalendar>
```

### Example 1: Monday-First Calendar

```razor
@using Syncfusion.Blazor.Calendars

<h3>Week Starts on Monday</h3>

<SfCalendar TValue="DateTime?" FirstDayOfWeek="1"></SfCalendar>
```

**Output:** Calendar shows Mon-Sun instead of Sun-Sat.

### Example 3: Show Week Numbers

```razor
@using Syncfusion.Blazor.Calendars

<h3>Calendar with ISO Week Numbers</h3>

<SfCalendar TValue="DateTime?" ShowWeekNumbers="true"></SfCalendar>
```

**Output:** Shows week numbers in the left column (ISO 8601 standard).

---

## Date Formatting

### Locale-Based Date Formats

Different locales use different date formats:

| Locale | Format | Example |
|--------|--------|---------|
| en-US | MM/DD/YYYY | 03/19/2024 |
| en-GB | DD/MM/YYYY | 19/03/2024 |
| de-DE | DD.MM.YYYY | 19.03.2024 |
| fr-FR | DD/MM/YYYY | 19/03/2024 |
| ja-JP | YYYY/MM/DD | 2024/03/19 |

### Displaying Formatted Dates

```razor
@using Syncfusion.Blazor.Calendars

<h3>Locale-Aware Date Display</h3>

<p>@SelectedDate?.ToString("d", System.Globalization.CultureInfo.GetCultureInfo(CurrentLocale))</p>

<SfCalendar TValue="DateTime?" 
    @bind-Value="@SelectedDate">
</SfCalendar>

@code {
    public DateTime? SelectedDate { get; set; } = DateTime.Now;
}
```

### Custom Date Formatting

```razor
@using Syncfusion.Blazor.Calendars

<h3>Custom Date Format</h3>

<SfCalendar TValue="DateTime?" @bind-Value="@SelectedDate"></SfCalendar>

<p>ISO Format: @SelectedDate?.ToString("O")</p>
<p>Long Format: @SelectedDate?.ToString("D")</p>
<p>Short Format: @SelectedDate?.ToString("d")</p>

@code {
    public DateTime? SelectedDate { get; set; } = DateTime.Now;
}
```

---

## Common Patterns

### Pattern 1: Auto-Detect System Locale

```razor
@code {
    private string SystemLocale { get; set; } = 
        System.Globalization.CultureInfo.CurrentCulture.Name;
    
    private bool SystemIsRtl { get; set; } = 
        System.Globalization.CultureInfo.CurrentCulture.TextInfo.IsRightToLeft;
}
```

### Pattern 2: Time Zone-Aware Calendar

```razor
@code {
    private DateTime GetLocalDateTime(DateTime utc)
    {
        var tz = TimeZoneInfo.FindSystemTimeZoneById("Asia/Tokyo");
        return TimeZoneInfo.ConvertTime(utc, tz);
    }
    
    private DateTime ConvertToUtc(DateTime local)
    {
        var tz = TimeZoneInfo.FindSystemTimeZoneById("Asia/Tokyo");
        return TimeZoneInfo.ConvertTimeToUtc(local, tz);
    }
}
```

### Pattern 4: Multi-Calendar View

```razor
@using Syncfusion.Blazor.Calendars

<h3>Calendar Comparison</h3>

<div class="calendars-grid">
    <div>
        <h5>US Format</h5>
        <SfCalendar TValue="DateTime?" FirstDayOfWeek="0"></SfCalendar>
    </div>
    <div>
        <h5>European Format</h5>
        <SfCalendar TValue="DateTime?" FirstDayOfWeek="1"></SfCalendar>
    </div>
    <div>
        <h5>Islamic Calendar</h5>
        <SfCalendar TValue="DateTime?" EnableRtl="true"></SfCalendar>
    </div>
</div>

<style>
    .calendars-grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
        gap: 20px;
    }
</style>
```

---

## Locale Data Loading

### Setting Up Locale Files

Syncfusion provides locale JSON files. For custom locales:

1. Get locale file from Syncfusion resources
2. Place in `wwwroot/locale/` folder
3. Load via HTTP client
4. Register with component

---

## Best Practices

1. **Always set locale explicitly** - Don't rely on browser defaults
2. **Load locale data early** - Prevent flickering
3. **Test with actual locales** - Different date formats, week starts
4. **Use locale-appropriate defaults** - First day of week, date format
5. **Consider RTL** - Test with Arabic, Hebrew, Persian
6. **Handle timezone differences** - Store as UTC, display in user timezone
7. **Test accessibility** - Ensure locale changes don't break keyboard nav
