# Time Formats in Blazor TimePicker

## Table of Contents
- [Display Format Overview](#display-format-overview)
- [Standard Format Strings](#standard-format-strings)
- [Custom Format Strings](#custom-format-strings)
- [Input Formats](#input-formats)
- [Culture-Based Defaults](#culture-based-defaults)
- [Format Examples](#format-examples)
- [When to Use Each Format](#when-to-use-each-format)
- [Format Conversion Best Practices](#format-conversion-best-practices)

## Display Format Overview

The `Format` property controls how the time is displayed in the TimePicker input field and dropdown list. By default, the format is based on the current culture/locale.

### Setting Display Format

```razor
@using Syncfusion.Blazor.Calendars

<!-- Default culture format -->
<SfTimePicker TValue="DateTime?"></SfTimePicker>

<!-- Custom 24-hour format -->
<SfTimePicker TValue="DateTime?" Format="HH:mm"></SfTimePicker>

<!-- Custom 12-hour format -->
<SfTimePicker TValue="DateTime?" Format="hh:mm tt"></SfTimePicker>
```

## Standard Format Strings

Standard format strings use single letters that vary by culture:

| Format | Description | Example (US Culture) |
|--------|-------------|-------------------|
| `t` | Short time | 2:30 PM |
| `T` | Long time | 2:30:45 PM |
| `g` | General (date/time) | 3/19/2026 2:30 PM |

### Using Standard Formats

```razor
<!-- Short time format -->
<SfTimePicker TValue="DateTime?" Format="t" Placeholder="2:30 PM"></SfTimePicker>

<!-- Long time format -->
<SfTimePicker TValue="DateTime?" Format="T" Placeholder="2:30:45 PM"></SfTimePicker>
```

## Custom Format Strings

Custom format strings provide precise control over time display:

### Common Custom Format Patterns

| Pattern | Meaning | Example |
|---------|---------|---------|
| `h` | Hour (1-12) | 2 |
| `hh` | Hour (01-12, zero-padded) | 02 |
| `H` | Hour (0-23) | 14 |
| `HH` | Hour (00-23, zero-padded) | 14 |
| `m` | Minute (0-59) | 5 |
| `mm` | Minute (00-59, zero-padded) | 05 |
| `s` | Second (0-59) | 8 |
| `ss` | Second (00-59, zero-padded) | 08 |
| `t` | AM/PM designator (first char) | P |
| `tt` | AM/PM designator (full) | PM |

### Custom Format Examples

```razor
@using Syncfusion.Blazor.Calendars

<!-- 24-hour format without seconds -->
<SfTimePicker TValue="DateTime?" Format="HH:mm"></SfTimePicker>
<!-- Output: 14:30 -->

<!-- 24-hour format with seconds -->
<SfTimePicker TValue="DateTime?" Format="HH:mm:ss"></SfTimePicker>
<!-- Output: 14:30:45 -->

<!-- 12-hour format with AM/PM -->
<SfTimePicker TValue="DateTime?" Format="hh:mm tt"></SfTimePicker>
<!-- Output: 02:30 PM -->

<!-- 12-hour format with seconds -->
<SfTimePicker TValue="DateTime?" Format="hh:mm:ss tt"></SfTimePicker>
<!-- Output: 02:30:45 PM -->

<!-- Hour and minute only (12-hour) -->
<SfTimePicker TValue="DateTime?" Format="h:mm tt"></SfTimePicker>
<!-- Output: 2:30 PM -->
```

## Input Formats

The `InputFormats` property defines which formats the user can enter. Multiple formats can be accepted:

### Setting Input Formats

```razor
@using Syncfusion.Blazor.Calendars

<SfTimePicker TValue="DateTime?" 
    Format="HH:mm"
    InputFormats="@InputFmts">
</SfTimePicker>

@code {
    public List<string> InputFmts = new List<string>() 
    { 
        "HH:mm", 
        "h:mm tt",
        "HHmm"
    };
}
```

### Input Format Behavior

When user types a time, the component:
1. Checks against each format in `InputFormats` list
2. Converts to display `Format` after focus loss
3. Shows validation error if no format matches

### Multiple Input Format Example

```razor
@using Syncfusion.Blazor.Calendars

<!-- Accept both 24-hour and 12-hour input -->
<SfTimePicker TValue="DateTime?" 
    Format="HH:mm"
    InputFormats="@new List<string> { 'HH:mm', 'hh:mm tt', 'HHmm' }"
    Placeholder="Enter time (24h or 12h)">
</SfTimePicker>

@code {
    // User can type: "14:30", "2:30 PM", or "1430"
    // All convert to display format: "14:30"
}
```

### Common Input/Display Format Combinations

```razor
<!-- Accept flexible input, display consistent format -->
<SfTimePicker TValue="DateTime?" 
    Format="HH:mm"
    InputFormats="@new List<string> { 'HH:mm', 'H:mm', 'HHmm' }">
</SfTimePicker>

<!-- US-style input with standard display -->
<SfTimePicker TValue="DateTime?" 
    Format="hh:mm tt"
    InputFormats="@new List<string> { 'hh:mm tt', 'h:mm tt' }">
</SfTimePicker>
```

## Culture-Based Defaults

If no format is specified, the TimePicker uses the current culture's default:

```razor
@using Syncfusion.Blazor.Calendars
@inject HttpClient Http
@inject IJSRuntime JsRuntime

<!-- Uses culture default format (e.g., "2:30:45 PM" for en-US) -->
<SfTimePicker TValue="DateTime?"></SfTimePicker>

@code {
    protected override async Task OnInitializedAsync()
    {
        // Set specific culture if needed
        await JsRuntime.Sf().LoadLocaleData(
            await Http.GetJsonAsync<object>("./locales/en.json")
        ).SetCulture("en");
    }
}
```

### Culture Format Examples

| Culture | Default Format | Example |
|---------|----------------|---------|
| en-US | `t` (Short time) | 2:30 PM |
| de-DE | Hour:Minute:Second | 14:30:45 |
| fr-FR | Hour:Minute:Second | 14:30:45 |
| ar-SA | Hour:Minute:Second (RTL) | 30:14 (RTL) |

## Format Examples

### Example 1: Medical Appointment (24-hour, precise)

```razor
<SfTimePicker TValue="DateTime?" 
    Format="HH:mm:ss"
    Placeholder="Enter appointment time"
    Step=5>
</SfTimePicker>
<!-- Displays: 14:30:45 -->
```

### Example 2: Business Meeting (12-hour with AM/PM)

```razor
<SfTimePicker TValue="DateTime?" 
    Format="hh:mm tt"
    Placeholder="Select meeting time"
    Step=15>
</SfTimePicker>
<!-- Displays: 02:30 PM -->
```

### Example 3: Shift Scheduling (24-hour, hour only)

```razor
<SfTimePicker TValue="DateTime?" 
    Format="HH:00"
    Placeholder="Select shift hour"
    Step=60>
</SfTimePicker>
<!-- Displays: 14:00 -->
```

### Example 4: Flexible Input, Consistent Display

```razor
@using Syncfusion.Blazor.Calendars

<SfTimePicker TValue="DateTime?" 
    Format="HH:mm"
    InputFormats="@new List<string> { 'HH:mm', 'HHmm', 'H:mm' }"
    Placeholder="14:30 or 1430">
</SfTimePicker>

@code {
    // User types: "1430", "14:30", or "14:30"
    // All display as: "14:30"
}
```

## When to Use Each Format

### Use 24-Hour Format (HH:mm)

- **When**: Medical, military, aerospace, transportation, international apps
- **Reason**: Eliminates AM/PM confusion (no 12:30 AM vs 12:30 PM ambiguity)
- **Example**: Hospitals, train schedules, shift work

```razor
<SfTimePicker TValue="DateTime?" Format="HH:mm"></SfTimePicker>
```

### Use 12-Hour Format (hh:mm tt)

- **When**: Consumer apps, US/North America markets
- **Reason**: User familiarity, everyday apps (appointments, delivery times)
- **Example**: Food delivery, salon bookings, retail stores

```razor
<SfTimePicker TValue="DateTime?" Format="hh:mm tt"></SfTimePicker>
```

### Use Seconds (HH:mm:ss)

- **When**: Precise timing required
- **Reason**: Medical procedures, live event scheduling, system logs
- **Example**: Recording app events, streaming start times

```razor
<SfTimePicker TValue="DateTime?" Format="HH:mm:ss"></SfTimePicker>
```

### Use Culture-Based (t or T)

- **When**: Multi-locale apps, user preference
- **Reason**: Respects user's system culture settings
- **Example**: Global SaaS platforms

```razor
<SfTimePicker TValue="DateTime?" Format="t"></SfTimePicker>
```

## Format Conversion Best Practices

### Best Practice 1: Store in Consistent Format

Always store/transmit times in 24-hour ISO format internally:

```razor
@using Syncfusion.Blazor.Calendars

<SfTimePicker TValue="DateTime?" 
    @bind-Value="@SelectedTime"
    Format="hh:mm tt">
</SfTimePicker>

@code {
    private DateTime? SelectedTime { get; set; }

    private async Task SaveTime()
    {
        // Store as ISO 8601: "14:30:00" (24-hour format)
        string isoFormat = SelectedTime?.ToString("HH:mm:ss");
        await ApiClient.SaveTime(isoFormat);
    }
}
```

### Best Practice 2: Display Per Culture, Store Standardized

```razor
@using Syncfusion.Blazor.Calendars
@using System.Globalization

<SfTimePicker TValue="DateTime?" 
    @bind-Value="@SelectedTime"
    Format="@GetCultureFormat()">
</SfTimePicker>

@code {
    private DateTime? SelectedTime { get; set; }

    private string GetCultureFormat()
    {
        return CultureInfo.CurrentCulture.TwoLetterISOLanguageName switch
        {
            "en" => "hh:mm tt",  // US: 2:30 PM
            "de" => "HH:mm",     // German: 14:30
            "fr" => "HH:mm",     // French: 14:30
            _ => "HH:mm"
        };
    }
}
```

### Best Practice 3: Validate Format Before Saving

```razor
@using Syncfusion.Blazor.Calendars

<SfTimePicker TValue="DateTime?" 
    @bind-Value="@SelectedTime"
    Format="HH:mm">
</SfTimePicker>

@code {
    private DateTime? SelectedTime { get; set; }

    private bool IsValidTime(DateTime? time)
    {
        if (time == null) return false;
        
        // Validate business hours
        return time.Value.Hour >= 9 && time.Value.Hour < 17;
    }

    private async Task SaveIfValid()
    {
        if (IsValidTime(SelectedTime))
        {
            await Save();
        }
    }
}
```

## See Also

- [Data Binding →](../data-binding.md)
- [Getting Started →](../getting-started.md)
- [Mask Support →](../mask-support.md)
