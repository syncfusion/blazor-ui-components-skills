# Accessibility and Globalization in Blazor TimePicker

## Table of Contents
- [WCAG 2.2 Compliance](#wcag-22-compliance)
- [Keyboard Navigation](#keyboard-navigation)
- [WAI-ARIA Attributes](#wai-aria-attributes)
- [Screen Reader Support](#screen-reader-support)
- [RTL (Right-to-Left) Support](#rtl-right-to-left-support)
- [Localization and Culture](#localization-and-culture)
- [International Usage Examples](#international-usage-examples)
- [Accessibility Best Practices](#accessibility-best-practices)

## WCAG 2.2 Compliance

The Blazor TimePicker component meets WCAG 2.2 standards for web accessibility:

### Compliance Standards

| Standard | Status | Details |
|----------|--------|---------|
| WCAG 2.2 | AA | General accessibility guidelines met |
| Section 508 | ✓ Compliant | US government accessibility standard |
| ADA | ✓ Compliant | Americans with Disabilities Act |

### What This Means

- **Color Contrast**: Text meets minimum contrast ratios (4.5:1 for normal text)
- **Keyboard Navigation**: Fully operable with keyboard only
- **Screen Readers**: Compatible with NVDA, JAWS, VoiceOver
- **Mobile Accessibility**: Works with mobile screen readers (iOS VoiceOver, Android TalkBack)

### Verifying Compliance

```razor
@using Syncfusion.Blazor.Calendars

<!-- Test for accessibility -->
<SfTimePicker TValue="DateTime?" 
    Placeholder="Accessible time picker"
    AriaLabel="Select meeting time"
    Enabled="true">
</SfTimePicker>

<!-- Verify in browser with axe DevTools -->
```

## Keyboard Navigation

The TimePicker supports full keyboard navigation without mouse:

### Keyboard Shortcuts

| Key | Action | Notes |
|-----|--------|-------|
| <kbd>Alt</kbd> + <kbd>↓</kbd> | Open popup | Windows shortcut |
| <kbd>↑</kbd> | Previous time | In popup |
| <kbd>↓</kbd> | Next time | In popup |
| <kbd>←</kbd> / <kbd>→</kbd> | Navigate within time | Hour/minute/second |
| <kbd>Home</kbd> | First time | In popup |
| <kbd>End</kbd> | Last time | In popup |
| <kbd>Enter</kbd> | Select focused time | Closes popup |
| <kbd>Esc</kbd> | Close popup | Returns to input |
| <kbd>Alt</kbd> + <kbd>↑</kbd> | Close popup | Alternative close |

### Keyboard Navigation Example

```razor
@using Syncfusion.Blazor.Calendars

<p>Try navigating with keyboard:</p>
<p>1. Tab to input field</p>
<p>2. Alt + ↓ to open popup</p>
<p>3. Use arrow keys to navigate</p>
<p>4. Enter to select</p>

<SfTimePicker TValue="DateTime?" 
    Placeholder="Press Alt+↓ to open"
    @ref="TimePickerRef">
</SfTimePicker>

@code {
    private SfTimePicker<DateTime?> TimePickerRef;

    private async Task KeyboardDemo()
    {
        // Users can navigate via keyboard
    }
}
```

### Testing Keyboard Navigation

```razor
@using Syncfusion.Blazor.Calendars

<div class="keyboard-test">
    <h3>Keyboard Navigation Test</h3>
    
    <label for="time1">Time Selection:</label>
    <SfTimePicker TValue="DateTime?" 
        ID="time1"
        Placeholder="Use arrow keys to navigate"
        TabIndex="0">
    </SfTimePicker>

    <p>Instructions:</p>
    <ul>
        <li>Tab: Focus input</li>
        <li>Alt+Down: Open dropdown</li>
        <li>Up/Down: Navigate times</li>
        <li>Enter: Select</li>
    </ul>
</div>
```

## WAI-ARIA Attributes

The TimePicker includes WAI-ARIA attributes that help assistive technologies understand the component:

### Built-In ARIA Support

The component automatically provides:

| ARIA Attribute | Purpose |
|----------------|---------|
| `aria-haspopup="listbox"` | Announces popup |
| `aria-expanded="true/false"` | Indicates popup state |
| `aria-disabled="true/false"` | Disabled state |
| `aria-selected="true"` | Currently selected item |
| `aria-activedescendant` | Active focused item |
| `aria-autocomplete="list"` | Suggests completion |
| `role="combobox"` | Component type |

### Custom ARIA Labels

```razor
@using Syncfusion.Blazor.Calendars

<SfTimePicker TValue="DateTime?" 
    AriaLabel="Select appointment time"
    Placeholder="Choose time">
</SfTimePicker>

@* Or with additional ARIA attributes *@
<SfTimePicker TValue="DateTime?" 
    ID="appointmentTime"
    Placeholder="Meeting start time"
    AriaLabelledBy="timeLabel">
</SfTimePicker>

<label id="timeLabel">Meeting Start Time (24-hour format)</label>
```

### ARIA Labels for Complex Scenarios

```razor
@using Syncfusion.Blazor.Calendars

<form>
    <label for="startTime">Meeting Start Time</label>
    <SfTimePicker TValue="DateTime?" 
        ID="startTime"
        AriaLabel="Meeting start time, 24-hour format"
        AriaRequired="true">
    </SfTimePicker>

    <label for="endTime">Meeting End Time</label>
    <SfTimePicker TValue="DateTime?" 
        ID="endTime"
        AriaLabel="Meeting end time, must be after start time"
        AriaRequired="true">
    </SfTimePicker>
</form>
```

## Screen Reader Support

Screen readers automatically read TimePicker content:

### Screen Reader Behavior

```razor
@using Syncfusion.Blazor.Calendars

<!-- Screen readers will announce:
     "Select appointment time, combo box, collapsed" when focusing input
     "Time list box" when popup opens
     "2:30 PM, selected" when item is highlighted -->

<SfTimePicker TValue="DateTime?" 
    Placeholder="Select appointment time"
    AriaLabel="Select appointment time">
</SfTimePicker>
```

### Testing with NVDA (Free Screen Reader)

1. Download NVDA (NV Access)
2. Enable NVDA
3. Tab to TimePicker input
4. Press Alt+↓ to open popup
5. Use arrow keys to navigate
6. NVDA reads: "2:30 PM, selected" etc.

### Testing with JAWS

1. Launch JAWS
2. Navigate to TimePicker with Tab
3. Press Enter or Space to open
4. Arrow keys to navigate
5. JAWS announces each time as you navigate

## RTL (Right-to-Left) Support

Enable RTL for languages like Arabic, Hebrew, Urdu, Persian:

### Enable RTL

```razor
@using Syncfusion.Blazor.Calendars

<!-- Single RTL component -->
<SfTimePicker TValue="DateTime?" 
    EnableRtl="true"
    Placeholder="اختر وقت">
</SfTimePicker>
```

### Global RTL Configuration

```razor
@* In _Imports.razor or app layout *@
@using Syncfusion.Blazor

@* All components RTL *@
<SfBaseProvider RtlMode="true">
    <SfTimePicker TValue="DateTime?"></SfTimePicker>
</SfBaseProvider>
```

### RTL with Localization

```razor
@using Syncfusion.Blazor.Calendars
@inject HttpClient Http
@inject IJSRuntime JsRuntime

<SfTimePicker TValue="DateTime?" 
    EnableRtl="true">
</SfTimePicker>

@code {
    protected override async Task OnInitializedAsync()
    {
        // Load Arabic locale data
        var arabicLocale = await Http.GetJsonAsync<object>("locales/ar.json");
        await JsRuntime.Sf()
            .LoadLocaleData(arabicLocale)
            .SetCulture("ar");
    }
}
```

### RTL Examples by Language

```razor
@* Arabic (RTL) *@
<SfTimePicker TValue="DateTime?" 
    EnableRtl="true"
    Placeholder="اختر وقت">
</SfTimePicker>

@* Hebrew (RTL) *@
<SfTimePicker TValue="DateTime?" 
    EnableRtl="true"
    Placeholder="בחר זמן">
</SfTimePicker>

@* English (LTR) - default *@
<SfTimePicker TValue="DateTime?" 
    Placeholder="Select time">
</SfTimePicker>
```

## Localization and Culture

The TimePicker automatically uses the current culture's format and locale:

### Culture Configuration

```razor
@using Syncfusion.Blazor.Calendars
@inject HttpClient Http
@inject IJSRuntime JsRuntime

<SfTimePicker TValue="DateTime?"></SfTimePicker>

@code {
    protected override async Task OnInitializedAsync()
    {
        // Set specific culture
        var localeData = await Http.GetJsonAsync<object>("locales/de.json");
        await JsRuntime.Sf()
            .LoadLocaleData(localeData)
            .SetCulture("de-DE");
    }
}
```

### Common Cultures and Formats

| Culture | Locale | Time Format | Direction |
|---------|--------|-------------|-----------|
| US English | en-US | 2:30 PM | LTR |
| German | de-DE | 14:30 | LTR |
| French | fr-FR | 14:30 | LTR |
| Spanish | es-ES | 14:30 | LTR |
| Arabic | ar | 2:30 م (PM) | RTL |
| Hebrew | he | 14:30 | RTL |
| Japanese | ja-JP | 14:30 | LTR |
| Chinese | zh-CN | 下午 2:30 | LTR |

### Dynamic Culture Switching

```razor
@using Syncfusion.Blazor.Calendars
@inject IJSRuntime JsRuntime
@inject HttpClient Http

<div>
    <select @onchange="ChangeCulture">
        <option value="en-US">English (US)</option>
        <option value="de-DE">Deutsch</option>
        <option value="fr-FR">Français</option>
        <option value="ar">العربية</option>
    </select>
</div>

<SfTimePicker TValue="DateTime?" @ref="TimePickerRef"></SfTimePicker>

@code {
    private SfTimePicker<DateTime?> TimePickerRef;

    private async Task ChangeCulture(ChangeEventArgs e)
    {
        string culture = e.Value?.ToString() ?? "en-US";
        
        var localeData = await Http.GetJsonAsync<object>(
            $"locales/{culture.Split('-')[0]}.json"
        );

        await JsRuntime.Sf()
            .LoadLocaleData(localeData)
            .SetCulture(culture);
    }
}
```

## International Usage Examples

### Example 1: Multi-Language Business App

```razor
@using Syncfusion.Blazor.Calendars
@inject IJSRuntime JsRuntime
@inject HttpClient Http

<div class="lang-selector">
    <button @onclick="@(() => SetLanguage("en"))">English</button>
    <button @onclick="@(() => SetLanguage("de"))">German</button>
    <button @onclick="@(() => SetLanguage("ar"))">العربية</button>
</div>

<SfTimePicker TValue="DateTime?" 
    @ref="TimePickerRef"
    Placeholder="Select time"
    EnableRtl="@IsRTL">
</SfTimePicker>

@code {
    private SfTimePicker<DateTime?> TimePickerRef;
    private bool IsRTL { get; set; }

    private async Task SetLanguage(string lang)
    {
        var localeData = await Http.GetJsonAsync<object>($"locales/{lang}.json");
        await JsRuntime.Sf()
            .LoadLocaleData(localeData)
            .SetCulture(lang);

        IsRTL = lang == "ar";
        StateHasChanged();
    }
}
```

### Example 2: Meeting Scheduler (International)

```razor
@using Syncfusion.Blazor.Calendars

<div class="meeting-scheduler">
    <h3>Schedule Meeting</h3>
    
    <label>Your Timezone:</label>
    <select @bind="@UserTimezone">
        <option>America/New_York (EST)</option>
        <option>Europe/London (GMT)</option>
        <option>Asia/Dubai (GST)</option>
    </select>

    <label>Meeting Time (24-hour UTC):</label>
    <SfTimePicker TValue="DateTime?" 
        @bind-Value="@MeetingTime"
        Format="HH:mm"
        AriaLabel="Meeting time in 24-hour format">
    </SfTimePicker>

    <p>Local Time: @GetLocalTime()</p>
</div>

@code {
    public DateTime? MeetingTime { get; set; }
    public string UserTimezone { get; set; } = "America/New_York (EST)";

    private string GetLocalTime()
    {
        if (MeetingTime == null) return "Not selected";
        // Convert UTC to user's timezone
        return $"{MeetingTime:HH:mm} UTC";
    }
}
```

### Example 3: Appointment with Accessibility Focus

```razor
@using Syncfusion.Blazor.Calendars

<div role="form" aria-labelledby="appointmentTitle">
    <h2 id="appointmentTitle">Book Appointment</h2>

    <label for="doctorSelect">Select Doctor:</label>
    <select id="doctorSelect" @bind="@SelectedDoctor" 
        aria-describedby="doctorHelp">
        <option>Dr. Smith</option>
        <option>Dr. Johnson</option>
    </select>
    <span id="doctorHelp">Choose your preferred doctor</span>

    <label for="appointmentTime">Appointment Time (required):</label>
    <SfTimePicker TValue="DateTime?" 
        ID="appointmentTime"
        @bind-Value="@AppointmentTime"
        Min="@WorkStartTime"
        Max="@WorkEndTime"
        Step=30
        AriaLabel="Select appointment time within business hours (9 AM to 5 PM)"
        AriaRequired="true"
        Placeholder="HH:mm">
    </SfTimePicker>

    <button @onclick="BookAppointment" aria-label="Book appointment">
        Book Appointment
    </button>
</div>

@code {
    public string SelectedDoctor { get; set; }
    public DateTime? AppointmentTime { get; set; }
    public DateTime WorkStartTime { get; set; } = new(2026, 3, 19, 9, 0, 0);
    public DateTime WorkEndTime { get; set; } = new(2026, 3, 19, 17, 0, 0);

    private async Task BookAppointment()
    {
        if (AppointmentTime == null)
        {
            // Announce error to screen readers
            await JsRuntime.InvokeVoidAsync("announce", "Please select appointment time");
            return;
        }
        // Book appointment
    }
}
```

## Accessibility Best Practices

### Best Practice 1: Always Provide Labels

```razor
@* ❌ BAD - No label *@
<SfTimePicker TValue="DateTime?"></SfTimePicker>

@* ✅ GOOD - Proper label *@
<label for="meetingTime">Meeting Time</label>
<SfTimePicker TValue="DateTime?" ID="meetingTime"></SfTimePicker>
```

### Best Practice 2: Use AriaLabel for Context

```razor
@* Helps screen reader users understand purpose *@
<SfTimePicker TValue="DateTime?" 
    AriaLabel="Select appointment time, business hours 9am to 5pm"
    Min="@new DateTime(2026, 3, 19, 9, 0, 0)"
    Max="@new DateTime(2026, 3, 19, 17, 0, 0)">
</SfTimePicker>
```

### Best Practice 3: Indicate Required Fields

```razor
@* Visually and programmatically *@
<label for="requiredTime">
    Time <span aria-label="required">*</span>
</label>
<SfTimePicker TValue="DateTime?" 
    ID="requiredTime"
    AriaRequired="true">
</SfTimePicker>
```

### Best Practice 4: Provide Validation Feedback

```razor
@using Syncfusion.Blazor.Calendars

<label for="appointmentTime">Appointment Time</label>
<SfTimePicker TValue="DateTime?" 
    ID="appointmentTime"
    @bind-Value="@AppointmentTime"
    AriaDescribedBy="appointmentHelp">
</SfTimePicker>
<span id="appointmentHelp" class="hint">
    Select time between 9 AM and 5 PM weekdays
</span>

@code {
    public DateTime? AppointmentTime { get; set; }
}
```

### Best Practice 5: Test with Real Assistive Technology

- Use NVDA (free screen reader)
- Use browser DevTools accessibility features
- Test keyboard navigation
- Test with actual users who use assistive tech

## See Also

- [Styling and Appearance →](../styling-and-appearance.md)
- [Getting Started →](../getting-started.md)
- [Events and Handlers →](../events-and-handlers.md)
