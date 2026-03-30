# Date & Time Formatting

Master date and time formatting in the DateTimePicker component with custom format strings and locale-specific configurations.

## Table of Contents
- [Format String Patterns](#format-string-patterns)
- [Standard Formats](#standard-formats)
- [Custom Format Examples](#custom-format-examples)
- [Parse and Display Formats](#parse-and-display-formats)
- [Locale-Specific Formatting](#locale-specific-formatting)

## Format String Patterns

DateTimePicker uses standard .NET date/time format specifiers. Here are the most common patterns:

### Date Patterns
| Pattern | Meaning | Example |
|---------|---------|---------|
| `d` | Day of month (1-31) | `5` or `15` |
| `dd` | Day of month, zero-padded (01-31) | `05` or `15` |
| `ddd` | Abbreviated day name | `Mon`, `Tue` |
| `dddd` | Full day name | `Monday`, `Tuesday` |
| `M` | Month (1-12) | `3` or `12` |
| `MM` | Month, zero-padded (01-12) | `03` or `12` |
| `MMM` | Abbreviated month name | `Jan`, `Mar` |
| `MMMM` | Full month name | `January`, `March` |
| `yy` | Year, 2-digit | `24` |
| `yyyy` | Year, 4-digit | `2024` |

### Time Patterns
| Pattern | Meaning | Example |
|---------|---------|---------|
| `h` | Hour (1-12) | `1`, `10` |
| `hh` | Hour (01-12), zero-padded | `01`, `10` |
| `H` | Hour (0-23, 24-hour format) | `0`, `13` |
| `HH` | Hour (00-23), zero-padded | `00`, `13` |
| `m` | Minute (0-59) | `5`, `45` |
| `mm` | Minute (00-59), zero-padded | `05`, `45` |
| `s` | Second (0-59) | `5`, `45` |
| `ss` | Second (00-59), zero-padded | `05`, `45` |
| `t` | AM/PM designator (first char) | `A` or `P` |
| `tt` | AM/PM designator (full) | `AM` or `PM` |

## Standard Formats

### Predefined Format Strings

Common format combinations for different regions:

**12-Hour Format with AM/PM**
```blazor
<SfDateTimePicker TValue="DateTime?" Format="MM/dd/yyyy h:mm tt"></SfDateTimePicker>
<!-- Output: 03/19/2024 2:30 PM -->
```

**24-Hour Format**
```blazor
<SfDateTimePicker TValue="DateTime?" Format="dd/MM/yyyy HH:mm"></SfDateTimePicker>
<!-- Output: 19/03/2024 14:30 -->
```

**ISO 8601 Format**
```blazor
<SfDateTimePicker TValue="DateTime?" Format="yyyy-MM-ddTHH:mm:ss"></SfDateTimePicker>
<!-- Output: 2024-03-19T14:30:00 -->
```

**Full Date and Time**
```blazor
<SfDateTimePicker TValue="DateTime?" Format="dddd, MMMM dd, yyyy h:mm tt"></SfDateTimePicker>
<!-- Output: Tuesday, March 19, 2024 2:30 PM -->
```

**Time Only**
```blazor
<SfDateTimePicker TValue="DateTime?" Format="HH:mm:ss"></SfDateTimePicker>
<!-- Output: 14:30:45 -->
```

## Custom Format Examples

### Example 1: US Format with Seconds
```blazor
<SfDateTimePicker TValue="DateTime?" Format="MM/dd/yyyy HH:mm:ss"></SfDateTimePicker>
<!-- 03/19/2024 14:30:45 -->
```

### Example 2: European Format
```blazor
<SfDateTimePicker TValue="DateTime?" Format="dd.MM.yyyy HH:mm"></SfDateTimePicker>
<!-- 19.03.2024 14:30 -->
```

### Example 3: Verbose Format with Day Name
```blazor
<SfDateTimePicker TValue="DateTime?" Format="dddd, dd MMMM yyyy, HH:mm"></SfDateTimePicker>
<!-- Tuesday, 19 March 2024, 14:30 -->
```

### Example 4: Short Date with 12-Hour Time
```blazor
<SfDateTimePicker TValue="DateTime?" Format="MM/dd/yy hh:mm tt"></SfDateTimePicker>
<!-- 03/19/24 02:30 PM -->
```

### Example 5: Database Format
```blazor
<SfDateTimePicker TValue="DateTime?" Format="yyyy-MM-dd HH:mm:ss"></SfDateTimePicker>
<!-- 2024-03-19 14:30:00 -->
```

## Parse and Display Formats

The DateTimePicker supports different formats for parsing input and displaying output.

### Parse Format (Input)
Controls how the component interprets user input:

```blazor
<SfDateTimePicker TValue="DateTime?" 
                  Format="MM/dd/yyyy HH:mm"
                  ParseFormats="@parseFormats">
</SfDateTimePicker>

@code {
    private string[] parseFormats = new[]
    {
        "MM/dd/yyyy HH:mm",
        "MM/dd/yyyy hh:mm tt",
        "dd/MM/yyyy HH:mm",
        "yyyy-MM-dd HH:mm"
    };
}
```

This allows users to input dates in multiple formats, which are all parsed correctly.

### Display Format vs Parse Format
```blazor
<!-- Input accepts multiple formats, but displays in standard format -->
<SfDateTimePicker TValue="DateTime?" 
                  Format="MM/dd/yyyy HH:mm"
                  ParseFormats="@parseFormats">
</SfDateTimePicker>

@code {
    // Users can type: "03/19/2024 14:30" or "19/03/2024 14:30"
    // But it displays as: "03/19/2024 14:30"
    private string[] parseFormats = new[]
    {
        "MM/dd/yyyy HH:mm",
        "dd/MM/yyyy HH:mm"
    };
}
```

## Locale-Specific Formatting

Different cultures have different date/time conventions. The DateTimePicker respects system locale by default.

### Using Culture Info

```blazor
@page "/datetimepicker-culture"
@using Syncfusion.Blazor.Calendars
@using System.Globalization

<SfDateTimePicker TValue="DateTime?" 
                  Format="@format"
                  Value="@selectedDate">
</SfDateTimePicker>

@code {
    private DateTime? selectedDate = new DateTime(2024, 3, 19, 14, 30, 0);
    private string format = CultureInfo.CurrentCulture.DateTimeFormat.ShortDatePattern 
                           + " " + 
                           CultureInfo.CurrentCulture.DateTimeFormat.ShortTimePattern;
}
```

### Common Culture Examples

**US English (en-US)**
```blazor
<!-- Format: 3/19/2024 2:30 PM -->
<SfDateTimePicker TValue="DateTime?" Format="M/d/yyyy h:mm tt"></SfDateTimePicker>
```

**German (de-DE)**
```blazor
<!-- Format: 19.03.2024 14:30 -->
<SfDateTimePicker TValue="DateTime?" Format="dd.MM.yyyy HH:mm"></SfDateTimePicker>
```

**French (fr-FR)**
```blazor
<!-- Format: 19/03/2024 14:30 -->
<SfDateTimePicker TValue="DateTime?" Format="dd/MM/yyyy HH:mm"></SfDateTimePicker>
```

**Japanese (ja-JP)**
```blazor
<!-- Format: 2024/03/19 14:30 -->
<SfDateTimePicker TValue="DateTime?" Format="yyyy/MM/dd HH:mm"></SfDateTimePicker>
```

## Formatting in Data Binding

When displaying formatted dates in your code:

```blazor
@page "/format-display"
@using Syncfusion.Blazor.Calendars

<div>
    <SfDateTimePicker TValue="DateTime?" @bind-Value="selectedDate"></SfDateTimePicker>
    
    @if (selectedDate.HasValue)
    {
        <p>Standard: @selectedDate.Value.ToString("g")</p>
        <p>ISO 8601: @selectedDate.Value.ToString("O")</p>
        <p>Custom: @selectedDate.Value.ToString("dddd, MMMM dd, yyyy h:mm tt")</p>
    }
</div>

@code {
    private DateTime? selectedDate;
}
```

## Format Validation and Edge Cases

### Case Sensitivity
Format strings are **case-sensitive**. Use correct case:
- `yyyy` (4-digit year, not `YYYY`)
- `MM` (month 01-12, not `mm`)
- `mm` (minutes, not `MM`)
- `HH` (24-hour format, not `hh`)

### Incorrect Format Examples
```blazor
<!-- ❌ Wrong - will not parse correctly -->
<SfDateTimePicker TValue="DateTime?" Format="YYYY-MM-DD"></SfDateTimePicker>

<!-- ✅ Correct -->
<SfDateTimePicker TValue="DateTime?" Format="yyyy-MM-dd"></SfDateTimePicker>
```

### Ambiguous Formats
Be careful with ambiguous formats that could cause confusion:

```blazor
<!-- Could be AM/PM hour or minute? Specify clearly -->
<!-- ❌ Avoid -->
<SfDateTimePicker TValue="DateTime?" Format="hh:MM:ss tt"></SfDateTimePicker>

<!-- ✅ Use -->
<SfDateTimePicker TValue="DateTime?" Format="hh:mm:ss tt"></SfDateTimePicker>
```

---

**Related Topics:**
- [Data Binding & Events](data-binding-and-events.md) - Handle formatted date values
- [Masking & Input Validation](masking-and-input-validation.md) - Control input format with masks
- [Globalization & Accessibility](globalization-and-accessibility.md) - Locale-specific formatting
