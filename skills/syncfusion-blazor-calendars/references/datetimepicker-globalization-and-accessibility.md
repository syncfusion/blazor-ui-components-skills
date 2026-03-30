# Globalization & Accessibility

Learn how to implement globalization, internationalization, and accessibility features in the DateTimePicker component.

## Table of Contents
- [Globalization Support](#globalization-support)
- [Locale-Specific Configuration](#locale-specific-configuration)
- [RTL (Right-to-Left) Support](#rtl-right-to-left-support)
- [Islamic Calendar](#islamic-calendar)
- [Accessibility Features](#accessibility-features)
- [ARIA Support](#aria-support)
- [Keyboard Navigation](#keyboard-navigation)

## Locale-Specific Configuration

### Date Format by Culture

Different cultures have different date conventions:

```blazor
@page "/datetimepicker-culture-formats"
@using Syncfusion.Blazor.Calendars
@using System.Globalization

<div>
    <div>
        <h4>US Format (en-US):</h4>
        <SfDateTimePicker TValue="DateTime?" 
                          Format="M/d/yyyy h:mm tt"
                          Value="@testDate">
        </SfDateTimePicker>
        <!-- Displays: 3/19/2024 2:30 PM -->
    </div>
    
    <div style="margin-top: 20px;">
        <h4>German Format (de-DE):</h4>
        <SfDateTimePicker TValue="DateTime?" 
                          Format="dd.MM.yyyy HH:mm"
                          Value="@testDate">
        </SfDateTimePicker>
        <!-- Displays: 19.03.2024 14:30 -->
    </div>
    
    <div style="margin-top: 20px;">
        <h4>Japanese Format (ja-JP):</h4>
        <SfDateTimePicker TValue="DateTime?" 
                          Format="yyyy/MM/dd HH:mm"
                          Value="@testDate">
        </SfDateTimePicker>
        <!-- Displays: 2024/03/19 14:30 -->
    </div>
</div>

@code {
    private DateTime testDate = new DateTime(2024, 3, 19, 14, 30, 0);
}
```

## RTL (Right-to-Left) Support

### Enable RTL Mode

For Arabic, Hebrew, Persian, and other RTL languages:

```blazor
@page "/datetimepicker-rtl"
@using Syncfusion.Blazor.Calendars

<div dir="rtl">
    <label>اختر التاريخ والوقت:</label>
    <SfDateTimePicker TValue="DateTime?" 
                      EnableRtl="true">
    </SfDateTimePicker>
</div>

<style>
    [dir="rtl"] {
        text-align: right;
    }
</style>
```

### RTL Styling

```blazor
@page "/datetimepicker-rtl-style"
@using Syncfusion.Blazor.Calendars

<div dir="rtl" class="rtl-container">
    <SfDateTimePicker TValue="DateTime?" 
                      EnableRtl="true"
                      CssClass="rtl-input">
    </SfDateTimePicker>
</div>

<style>
    .rtl-container {
        max-width: 400px;
        margin: 20px auto;
        padding: 20px;
        background: #f9f9f9;
    }
    
    .rtl-input {
        direction: rtl;
        text-align: right;
    }
    
    .rtl-input .e-input {
        text-align: right;
        padding-right: 15px;
    }
</style>
```

## Islamic Calendar

### Enable Islamic Calendar

```blazor
@page "/datetimepicker-islamic"
@using Syncfusion.Blazor.Calendars

<div>
    <label>Select Hijri Date:</label>
    <SfDateTimePicker TValue="DateTime?" 
                      CalendarMode="CalendarType.Islamic">
    </SfDateTimePicker>
</div>
```

### Combined Gregorian and Islamic Calendars

```blazor
@page "/datetimepicker-dual-calendar"
@using Syncfusion.Blazor.Calendars

<div style="display: flex; gap: 20px;">
    <div>
        <h4>Gregorian Calendar</h4>
        <SfDateTimePicker TValue="DateTime?" 
                          @bind-Value="selectedDate"
                          CalendarMode="CalendarType.Gregorian">
        </SfDateTimePicker>
    </div>
    
    <div dir="rtl">
        <h4>Islamic Calendar (Hijri)</h4>
        <SfDateTimePicker TValue="DateTime?" 
                          @bind-Value="selectedDate"
                          CalendarMode="CalendarType.Islamic"
                          EnableRtl="true">
        </SfDateTimePicker>
    </div>
</div>

@code {
    private DateTime? selectedDate;
}
```

## Accessibility Features

### WCAG Compliance

The DateTimePicker is built with WCAG 2.1 AA compliance:

```blazor
@page "/datetimepicker-accessible"
@using Syncfusion.Blazor.Calendars

<div>
    <label for="accessible-datetime">Select Date and Time:</label>
    <SfDateTimePicker TValue="DateTime?" 
                      ID="accessible-datetime"
                      Placeholder="Enter date and time"
                      Aria-Label="Date and time picker">
    </SfDateTimePicker>
    <small id="helper-text">Format: MM/DD/YYYY HH:MM</small>
</div>

<style>
    label {
        display: block;
        margin-bottom: 8px;
        font-weight: 600;
        color: #333;
    }
    
    small {
        display: block;
        margin-top: 5px;
        color: #666;
        font-size: 12px;
    }
</style>
```

### Focus Management

```blazor
@page "/datetimepicker-focus"
@using Syncfusion.Blazor.Calendars

<div>
    <h2 tabindex="0">Date Selection Form</h2>
    
    <form>
        <div>
            <label for="start-date">Start Date:</label>
            <SfDateTimePicker TValue="DateTime?" 
                              ID="start-date"
                              TabIndex="1">
            </SfDateTimePicker>
        </div>
        
        <div>
            <label for="end-date">End Date:</label>
            <SfDateTimePicker TValue="DateTime?" 
                              ID="end-date"
                              TabIndex="2">
            </SfDateTimePicker>
        </div>
        
        <button type="submit" tabindex="3">Submit</button>
    </form>
</div>
```

## ARIA Support

### ARIA Attributes

```blazor
@page "/datetimepicker-aria"
@using Syncfusion.Blazor.Calendars

<div>
    <label for="appointment" id="appointment-label">
        Select Appointment Date and Time
    </label>
    
    <SfDateTimePicker TValue="DateTime?" 
                      ID="appointment"
                      AriaLabel="Appointment date and time picker"
                      AriaLabelledBy="appointment-label"
                      Aria-Describedby="appointment-help">
    </SfDateTimePicker>
    
    <div id="appointment-help" role="complementary">
        <p>Please select a date between today and 30 days in the future.</p>
        <p>Use arrow keys to navigate the calendar.</p>
    </div>
</div>
```

### Screen Reader Announcements

```blazor
@page "/datetimepicker-screen-reader"
@using Syncfusion.Blazor.Calendars

<div>
    <SfDateTimePicker TValue="DateTime?" 
                      Placeholder="Select date">
        <DateTimePickerEvents TValue="DateTime?" ValueChange="@OnDateSelectedForAnnouncement"></DateTimePickerEvents>
    </SfDateTimePicker>
    
    <!-- Live region for screen reader announcements -->
    <div aria-live="polite" role="status" id="announcement" style="display: none;">
        @announcement
    </div>
</div>

@code {
    private string announcement = "";
    
    private void OnDateSelectedForAnnouncement(ChangedEventArgs<DateTime?> args)
    {
        if (args.Value.HasValue)
        {
            announcement = $"Date selected: {args.Value.Value.ToString("D")}";
        }
    }
}
```

## Keyboard Navigation

### Standard Keyboard Shortcuts

The DateTimePicker supports these keyboard shortcuts:

| Key | Action |
|-----|--------|
| `Up Arrow` | Move to previous week (date picker) |
| `Down Arrow` | Move to next week (date picker) |
| `Left Arrow` | Move to previous day |
| `Right Arrow` | Move to next day |
| `Home` | Move to first day of month |
| `End` | Move to last day of month |
| `Page Up` | Move to previous month |
| `Page Down` | Move to next month |
| `Enter` | Select date, accept and close |
| `Escape` | Close popup |
| `Alt + Up` | Close popup |
| `Alt + Down` | Open popup |
| `Tab` | Move to next control |
| `Shift + Tab` | Move to previous control |

### Keyboard Navigation Example

```blazor
@page "/datetimepicker-keyboard"
@using Syncfusion.Blazor.Calendars

<div>
    <h3>Keyboard Navigation</h3>
    
    <p>Try these keyboard shortcuts:</p>
    <ul>
        <li>Arrow Keys - Navigate dates</li>
        <li>Page Up/Down - Change month</li>
        <li>Enter - Select date</li>
        <li>Escape - Close popup</li>
    </ul>
    
    <label for="keyboard-datetime">Press Alt+Down to open picker:</label>
    <SfDateTimePicker TValue="DateTime?" 
                      ID="keyboard-datetime"
                      Placeholder="Use keyboard to navigate">
    </SfDateTimePicker>
</div>
```

## Complete Accessible Example

```blazor
@page "/datetimepicker-complete-accessible"
@using Syncfusion.Blazor.Calendars
@using System.Globalization

<div class="accessible-form">
    <h1>Accessible Appointment Booking</h1>
    
    <form>
        <div class="form-group">
            <label for="user-name">Your Name <span aria-label="required">*</span></label>
            <input type="text" 
                   id="user-name" 
                   placeholder="Enter your name"
                   required>
        </div>
        
        <div class="form-group">
            <label for="appointment-datetime">
                Select Appointment Date & Time <span aria-label="required">*</span>
            </label>
            <SfDateTimePicker TValue="DateTime?" 
                              ID="appointment-datetime"
                              Min="@minAppointmentDate"
                              Max="@maxAppointmentDate"
                              AriaLabel="Appointment date and time"
                              Placeholder="MM/DD/YYYY HH:MM"
                              TabIndex="1">
            </SfDateTimePicker>
            <small id="datetime-help">
                Select a date within the next 30 days during business hours.
            </small>
        </div>
        
        <div class="form-group">
            <label for="language-preference">Language Preference:</label>
            <select id="language-preference" @onchange="OnLanguageChanged">
                <option value="en-US">English (US)</option>
                <option value="es-ES">Spanish</option>
                <option value="fr-FR">French</option>
                <option value="de-DE">German</option>
                <option value="ar-SA">Arabic</option>
            </select>
        </div>
        
        <button type="submit" tabindex="2">Book Appointment</button>
    </form>
</div>

<style>
    .accessible-form {
        max-width: 500px;
        margin: 20px auto;
        padding: 20px;
        background: white;
        border-radius: 8px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    }
    
    .form-group {
        margin-bottom: 20px;
    }
    
    label {
        display: block;
        margin-bottom: 8px;
        font-weight: 600;
        color: #333;
    }
    
    label span[aria-label="required"] {
        color: red;
        margin-left: 2px;
    }
    
    input, select {
        width: 100%;
        padding: 10px;
        border: 2px solid #ddd;
        border-radius: 4px;
        font-size: 14px;
        font-family: inherit;
    }
    
    input:focus, select:focus {
        outline: none;
        border-color: #007bff;
        box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.1);
    }
    
    small {
        display: block;
        margin-top: 5px;
        color: #666;
        font-size: 12px;
    }
    
    button {
        width: 100%;
        padding: 12px;
        background-color: #28a745;
        color: white;
        border: none;
        border-radius: 4px;
        font-size: 16px;
        font-weight: 600;
        cursor: pointer;
        transition: background-color 0.3s;
    }
    
    button:hover {
        background-color: #218838;
    }
    
    button:focus {
        outline: 2px solid #28a745;
        outline-offset: 2px;
    }
</style>

@code {
    private string currentLanguage = "en-US";
    private DateTime minAppointmentDate = DateTime.Now;
    private DateTime maxAppointmentDate = DateTime.Now.AddDays(30);
    
    private void OnLanguageChanged(ChangeEventArgs e)
    {
        currentLanguage = e.Value.ToString();
    }
}
```

---

**Related Topics:**
- [Getting Started](getting-started.md) - Basic setup
- [Date & Time Formatting](date-time-formatting.md) - Locale-specific formatting
- [Data Binding & Events](data-binding-and-events.md) - Event handling and interaction
