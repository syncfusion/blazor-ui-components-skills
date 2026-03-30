# Mask Support in Blazor TimePicker

## Table of Contents
- [Masking Overview](#masking-overview)
- [EnableMask Property](#enablemask-property)
- [TimePickerMaskPlaceholder](#timepickermaskplaceholder)
- [Placeholder Characters](#placeholder-characters)
- [Format-Specific Masking](#format-specific-masking)
- [Input Validation with Masks](#input-validation-with-masks)
- [Masking Examples](#masking-examples)
- [Troubleshooting Masks](#troubleshooting-masks)

## Masking Overview

Input masking helps users enter time in the correct format by providing visual guidance and enforcing structure. When enabled, the TimePicker input displays placeholder characters that guide users on where to enter hour, minute, and second values.

### What Masking Does

- **Visual Guide**: Shows format pattern (hh:mm:ss, HH:mm, etc.)
- **Input Validation**: Accepts only valid numeric input
- **Format Enforcement**: Automatically positions cursor to next segment
- **Placeholder Customization**: Uses culture-specific or custom characters

### Masking Example

```
Without Mask:  [________]  (unclear format)
With Mask:     [hh:mm:ss]  (clear 24-hour format)
```

## EnableMask Property

The `EnableMask` property enables or disables input masking in the TimePicker.

### Basic Mask Enable

```razor
@using Syncfusion.Blazor.Calendars

<!-- Enable masking with default format -->
<SfTimePicker TValue="DateTime?" 
    EnableMask="true"
    Format="HH:mm"
    Placeholder="24-hour time">
</SfTimePicker>
```

### When to Enable Masking

| Use Case | Enable | Reason |
|----------|--------|--------|
| Consumer apps (US) | Yes | Helps non-technical users |
| Medical/Professional | Yes | Enforces accurate data entry |
| Free-form input | No | Users prefer flexibility |
| Popup selection only | No | Users select, don't type |

### Mask Enable with Different Formats

```razor
@using Syncfusion.Blazor.Calendars

<!-- 24-hour with mask -->
<SfTimePicker TValue="DateTime?" 
    EnableMask="true"
    Format="HH:mm"
    Placeholder="24-hour">
</SfTimePicker>

<!-- 12-hour with AM/PM mask -->
<SfTimePicker TValue="DateTime?" 
    EnableMask="true"
    Format="hh:mm tt"
    Placeholder="12-hour with AM/PM">
</SfTimePicker>

<!-- With seconds mask -->
<SfTimePicker TValue="DateTime?" 
    EnableMask="true"
    Format="HH:mm:ss"
    Placeholder="With seconds">
</SfTimePicker>
```

### Mask Behavior

When `EnableMask="true"`:

1. Input field shows format pattern
2. User types numeric values
3. Cursor auto-advances to next segment
4. Non-numeric input is rejected
5. Invalid values are prevented

## TimePickerMaskPlaceholder

The `TimePickerMaskPlaceholder` directive customizes the character used for each time segment (hour, minute, second).

### MaskPlaceholder Structure

```razor
<SfTimePicker TValue="DateTime?">
    <TimePickerMaskPlaceholder Hour="h" Minute="m" Second="s"></TimePickerMaskPlaceholder>
</SfTimePicker>
```

### Default Placeholders

By default, the component uses culture-specific placeholders:

| Property | Default (en-US) |
|----------|-----------------|
| `Hour` | h |
| `Minute` | m |
| `Second` | s |

## Placeholder Characters

Customize which characters represent each segment of time:

### Basic Placeholder Customization

```razor
@using Syncfusion.Blazor.Calendars

<SfTimePicker TValue="DateTime?" 
    EnableMask="true"
    Format="HH:mm">
    <TimePickerMaskPlaceholder Hour="H" Minute="M"></TimePickerMaskPlaceholder>
</SfTimePicker>

<!-- Displays: [HH:MM] instead of [hh:mm] -->
```

### Placeholder Options

```razor
@using Syncfusion.Blazor.Calendars

<!-- Standard placeholders -->
<SfTimePicker TValue="DateTime?" EnableMask="true" Format="HH:mm:ss">
    <TimePickerMaskPlaceholder Hour="h" Minute="m" Second="s"></TimePickerMaskPlaceholder>
</SfTimePicker>

<!-- Numeric indicators -->
<SfTimePicker TValue="DateTime?" EnableMask="true" Format="HH:mm">
    <TimePickerMaskPlaceholder Hour="0" Minute="0"></TimePickerMaskPlaceholder>
</SfTimePicker>

<!-- Uppercase placeholders -->
<SfTimePicker TValue="DateTime?" EnableMask="true" Format="HH:mm:ss">
    <TimePickerMaskPlaceholder Hour="HH" Minute="MM" Second="SS"></TimePickerMaskPlaceholder>
</SfTimePicker>

<!-- Underscores (common masking style) -->
<SfTimePicker TValue="DateTime?" EnableMask="true" Format="HH:mm">
    <TimePickerMaskPlaceholder Hour="_" Minute="_"></TimePickerMaskPlaceholder>
</SfTimePicker>
```

### Custom Placeholder Display

```razor
@using Syncfusion.Blazor.Calendars

<style>
    .custom-mask-placeholder {
        font-family: monospace;
        font-weight: bold;
    }
</style>

<!-- User sees: 14:30:45 vs hh:mm:ss pattern -->
<SfTimePicker TValue="DateTime?" 
    EnableMask="true"
    Format="HH:mm:ss"
    CssClass="custom-mask-placeholder">
    <TimePickerMaskPlaceholder Hour="hh" Minute="mm" Second="ss"></TimePickerMaskPlaceholder>
</SfTimePicker>
```

## Format-Specific Masking

Different time formats require different mask configurations:

### 24-Hour Format Masking

```razor
@using Syncfusion.Blazor.Calendars

<!-- Standard 24-hour format: HH:mm -->
<SfTimePicker TValue="DateTime?" 
    EnableMask="true"
    Format="HH:mm"
    Placeholder="24-hour (14:30)">
    <TimePickerMaskPlaceholder Hour="HH" Minute="mm"></TimePickerMaskPlaceholder>
</SfTimePicker>

<!-- With seconds: HH:mm:ss -->
<SfTimePicker TValue="DateTime?" 
    EnableMask="true"
    Format="HH:mm:ss"
    Placeholder="24-hour with seconds">
    <TimePickerMaskPlaceholder Hour="HH" Minute="mm" Second="ss"></TimePickerMaskPlaceholder>
</SfTimePicker>
```

### 12-Hour Format Masking

```razor
@using Syncfusion.Blazor.Calendars

<!-- 12-hour with AM/PM: hh:mm tt -->
<SfTimePicker TValue="DateTime?" 
    EnableMask="true"
    Format="hh:mm tt"
    Placeholder="12-hour (02:30 PM)">
    <TimePickerMaskPlaceholder Hour="hh" Minute="mm"></TimePickerMaskPlaceholder>
</SfTimePicker>

<!-- With seconds: hh:mm:ss tt -->
<SfTimePicker TValue="DateTime?" 
    EnableMask="true"
    Format="hh:mm:ss tt"
    Placeholder="12-hour with seconds">
    <TimePickerMaskPlaceholder Hour="hh" Minute="mm" Second="ss"></TimePickerMaskPlaceholder>
</SfTimePicker>
```

### European Format Masking

```razor
@using Syncfusion.Blazor.Calendars

<!-- German/European: 24-hour dot separator -->
<SfTimePicker TValue="DateTime?" 
    EnableMask="true"
    Format="HH.mm"
    Placeholder="European format (14.30)">
    <TimePickerMaskPlaceholder Hour="HH" Minute="mm"></TimePickerMaskPlaceholder>
</SfTimePicker>
```

## Input Validation with Masks

Masking automatically validates input as the user types:

### Automatic Validation

```razor
@using Syncfusion.Blazor.Calendars

<SfTimePicker TValue="DateTime?" 
    EnableMask="true"
    Format="HH:mm"
    Placeholder="Valid: 00-23 for hours, 00-59 for minutes">
</SfTimePicker>

<!-- User attempts to type "25:65" -->
<!-- Mask prevents entry: Only valid hour/minute values accepted -->
```

### Validation Behavior

| Input | Accepted | Rejected | Reason |
|-------|----------|----------|--------|
| 14 | ✓ | - | Valid hour (0-23) |
| 25 | - | ✓ | Invalid hour (>23) |
| 30 | ✓ | - | Valid minute (0-59) |
| 99 | - | ✓ | Invalid minute (>59) |
| abc | - | ✓ | Non-numeric |

### Validation with Min/Max

```razor
@using Syncfusion.Blazor.Calendars

<!-- Allow only business hours: 09:00 to 17:00 -->
<SfTimePicker TValue="DateTime?" 
    EnableMask="true"
    Format="HH:mm"
    Min="@new DateTime(2026, 3, 19, 9, 0, 0)"
    Max="@new DateTime(2026, 3, 19, 17, 0, 0)"
    Placeholder="Business hours only">
    <TimePickerMaskPlaceholder Hour="HH" Minute="mm"></TimePickerMaskPlaceholder>
</SfTimePicker>

<!-- Mask prevents hours < 9 or > 17 -->
```

## Masking Examples

### Example 1: Medical Appointment (Strict Format)

```razor
@using Syncfusion.Blazor.Calendars

<div class="appointment-form">
    <h3>Schedule Medical Appointment</h3>
    
    <label for="appointmentTime">Appointment Time (24-hour):</label>
    <SfTimePicker TValue="DateTime?" 
        ID="appointmentTime"
        EnableMask="true"
        Format="HH:mm:ss"
        Step=5
        Placeholder="Precise time (HH:mm:ss)"
        @bind-Value="@AppointmentTime">
        <TimePickerMaskPlaceholder Hour="HH" Minute="mm" Second="ss"></TimePickerMaskPlaceholder>
    </SfTimePicker>

    <p>Entered Time: @AppointmentTime?.ToString("HH:mm:ss")</p>
</div>

@code {
    public DateTime? AppointmentTime { get; set; }
}
```

### Example 2: Business Hours Selection (12-Hour)

```razor
@using Syncfusion.Blazor.Calendars

<div class="meeting-scheduler">
    <h3>Schedule Meeting</h3>
    
    <label for="meetingTime">Meeting Time (US Format):</label>
    <SfTimePicker TValue="DateTime?" 
        ID="meetingTime"
        EnableMask="true"
        Format="hh:mm tt"
        Min="@new DateTime(2026, 3, 19, 9, 0, 0)"
        Max="@new DateTime(2026, 3, 19, 17, 0, 0)"
        Step=30
        @bind-Value="@MeetingTime">
        <TimePickerMaskPlaceholder Hour="hh" Minute="mm"></TimePickerMaskPlaceholder>
    </SfTimePicker>

    <p>Entered Time: @MeetingTime?.ToString("hh:mm tt")</p>
    <p>Valid Range: 9:00 AM to 5:00 PM</p>
</div>

@code {
    public DateTime? MeetingTime { get; set; }
}
```

### Example 3: Multi-Language Masking

```razor
@using Syncfusion.Blazor.Calendars
@inject IJSRuntime JsRuntime
@inject HttpClient Http

<div>
    <select @onchange="ChangeCulture">
        <option value="en">English (hh:mm tt)</option>
        <option value="de">Deutsch (HH:mm)</option>
        <option value="fr">Français (HH:mm)</option>
    </select>
</div>

<SfTimePicker TValue="DateTime?" 
    @ref="TimePickerRef"
    EnableMask="true"
    Format="@CurrentFormat"
    @bind-Value="@SelectedTime">
    <TimePickerMaskPlaceholder Hour="@HourPlaceholder" Minute="@MinutePlaceholder"></TimePickerMaskPlaceholder>
</SfTimePicker>

@code {
    private SfTimePicker<DateTime?> TimePickerRef;
    public DateTime? SelectedTime { get; set; }
    public string CurrentFormat { get; set; } = "hh:mm tt";
    public string HourPlaceholder { get; set; } = "hh";
    public string MinutePlaceholder { get; set; } = "mm";

    private async Task ChangeCulture(ChangeEventArgs e)
    {
        string lang = e.Value?.ToString() ?? "en";

        CurrentFormat = lang switch
        {
            "en" => "hh:mm tt",
            "de" => "HH:mm",
            "fr" => "HH:mm",
            _ => "HH:mm"
        };

        HourPlaceholder = lang switch
        {
            "en" => "hh",
            _ => "HH"
        };

        MinutePlaceholder = "mm";

        var localeData = await Http.GetJsonAsync<object>($"locales/{lang}.json");
        await JsRuntime.Sf()
            .LoadLocaleData(localeData)
            .SetCulture(lang);
    }
}
```

### Example 4: Progressive Enhancement

```razor
@using Syncfusion.Blazor.Calendars

<div class="time-input-container">
    <label for="timeWithoutMask">Without Mask (Flexible):</label>
    <SfTimePicker TValue="DateTime?" 
        ID="timeWithoutMask"
        EnableMask="false"
        Placeholder="Type freely">
    </SfTimePicker>

    <label for="timeWithMask">With Mask (Guided):</label>
    <SfTimePicker TValue="DateTime?" 
        ID="timeWithMask"
        EnableMask="true"
        Format="HH:mm"
        Placeholder="Guided entry">
        <TimePickerMaskPlaceholder Hour="HH" Minute="mm"></TimePickerMaskPlaceholder>
    </SfTimePicker>
</div>
```

## Troubleshooting Masks

### Issue 1: Mask Not Appearing

**Problem**: EnableMask is true but no placeholder shows.

**Solution**: Ensure Format is set correctly:

```razor
<!-- ❌ WRONG - No format specified -->
<SfTimePicker TValue="DateTime?" EnableMask="true"></SfTimePicker>

<!-- ✅ CORRECT - Format must be specified -->
<SfTimePicker TValue="DateTime?" 
    EnableMask="true"
    Format="HH:mm">
</SfTimePicker>
```

### Issue 2: Placeholder Characters Not Showing

**Problem**: TimePickerMaskPlaceholder set but characters don't appear.

**Solution**: Ensure TimePickerMaskPlaceholder is inside component:

```razor
<!-- ❌ WRONG - MaskPlaceholder outside component -->
<SfTimePicker TValue="DateTime?" EnableMask="true" Format="HH:mm"></SfTimePicker>
<TimePickerMaskPlaceholder Hour="HH" Minute="mm"></TimePickerMaskPlaceholder>

<!-- ✅ CORRECT - MaskPlaceholder inside component -->
<SfTimePicker TValue="DateTime?" EnableMask="true" Format="HH:mm">
    <TimePickerMaskPlaceholder Hour="HH" Minute="mm"></TimePickerMaskPlaceholder>
</SfTimePicker>
```

### Issue 3: Validation Too Strict

**Problem**: Users can't enter any values with mask.

**Solution**: Verify format string and placeholder match:

```razor
<!-- ❌ Format and placeholder mismatch -->
<SfTimePicker TValue="DateTime?" 
    EnableMask="true"
    Format="hh:mm tt">
    <TimePickerMaskPlaceholder Hour="HH" Minute="mm"></TimePickerMaskPlaceholder>
</SfTimePicker>

<!-- ✅ Format and placeholder match -->
<SfTimePicker TValue="DateTime?" 
    EnableMask="true"
    Format="hh:mm tt">
    <TimePickerMaskPlaceholder Hour="hh" Minute="mm"></TimePickerMaskPlaceholder>
</SfTimePicker>
```

### Issue 4: Min/Max Not Enforced with Mask

**Problem**: Mask allows entry outside Min/Max range.

**Solution**: Mask guides format, but validation occurs on blur:

```razor
<!-- Mask prevents invalid hour/minute values -->
<!-- Min/Max checked when user leaves field -->
<SfTimePicker TValue="DateTime?" 
    EnableMask="true"
    Format="HH:mm"
    Min="@new DateTime(2026, 3, 19, 9, 0, 0)"
    Max="@new DateTime(2026, 3, 19, 17, 0, 0)">
    <TimePickerMaskPlaceholder Hour="HH" Minute="mm"></TimePickerMaskPlaceholder>
</SfTimePicker>

<!-- Mask prevents hours >23, but Min/Max additional constraint -->
```

## See Also

- [Time Formats →](../time-formats.md)
- [Getting Started →](../getting-started.md)
- [Styling and Appearance →](../styling-and-appearance.md)
