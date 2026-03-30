# Customization & Styling

## Table of Contents
- [First Day of Week](#first-day-of-week)
- [Placeholder Text](#placeholder-text)
- [CSS Class Customization](#css-class-customization)
- [Popup Behavior](#popup-behavior)
- [Week Number Display](#week-number-display)
- [Visual Appearance](#visual-appearance)
- [Theme Integration](#theme-integration)

## First Day of Week

### Setting First Day of Week

Customize which day starts the week in the calendar using the `FirstDayOfWeek` property:

```razor
@using Syncfusion.Blazor.Calendars

@* Set Monday (1) as first day of week *@
<SfDateRangePicker TValue="DateTime?" 
    FirstDayOfWeek="1"
    Placeholder="Select a range">
</SfDateRangePicker>
```

**Day of Week Values:**
- `0` = Sunday (default for en-US)
- `1` = Monday
- `2` = Tuesday
- `3` = Wednesday
- `4` = Thursday
- `5` = Friday
- `6` = Saturday

### Culture-Based First Day

Different cultures prefer different first days:

```razor
<SfDateRangePicker TValue="DateTime?" 
    FirstDayOfWeek="@GetFirstDayForCulture()">
</SfDateRangePicker>

@code {
    private int GetFirstDayForCulture()
    {
        var culture = System.Globalization.CultureInfo.CurrentCulture;
        
        // Most European cultures use Monday (1)
        if (culture.Name.StartsWith("de") || culture.Name.StartsWith("fr"))
            return 1;  // Monday
        
        // US and others use Sunday (0)
        return 0;  // Sunday
    }
}
```

### Dynamic First Day

Update first day based on user preferences:

```razor
@using Syncfusion.Blazor.Calendars

<label>Week starts on:</label>
<select @onchange="@((ChangeEventArgs e) => FirstDay = int.Parse(e.Value.ToString()))">
    <option value="0">Sunday</option>
    <option value="1">Monday</option>
    <option value="6">Saturday</option>
</select>

<SfDateRangePicker TValue="DateTime?" FirstDayOfWeek="@FirstDay"></SfDateRangePicker>

@code {
    private int FirstDay { get; set; } = 0;
}
```

## Placeholder Text

### Basic Placeholder

Display helpful text when no date is selected:

```razor
<SfDateRangePicker TValue="DateTime?" 
    Placeholder="Select check-in and check-out dates">
</SfDateRangePicker>
```

### Dynamic Placeholder

Change placeholder based on context:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    Placeholder="@GetPlaceholder()">
</SfDateRangePicker>

@code {
    private string GetPlaceholder()
    {
        var today = DateTime.Today;
        return today.Month == 12 ? "Select holiday dates" : "Select dates";
    }
}
```

### Placeholder in Labels

Combine with proper labels for clarity:

```razor
<div>
    <label for="booking-dates">Booking Period:</label>
    <SfDateRangePicker TValue="DateTime?" 
        Placeholder="From date - To date"
        Id="booking-dates">
    </SfDateRangePicker>
</div>
```

## CSS Class Customization

### Using CssClass Property

Apply custom CSS classes to the component:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    Placeholder="Select dates"
    CssClass="custom-daterange">
</SfDateRangePicker>

<style>
    .custom-daterange .e-input {
        background-color: #f0f0f0;
        border: 2px solid #007bff;
    }
    
    .custom-daterange .e-calendar {
        font-size: 14px;
    }
</style>
```

### Multiple Custom Classes

Stack multiple CSS classes:

```razor
<SfDateRangePicker TValue="DateTime?" 
    CssClass="large-input error-border rounded-corners"
    Placeholder="Select range">
</SfDateRangePicker>

<style>
    .large-input .e-input {
        font-size: 16px;
        padding: 12px;
    }
    
    .error-border .e-input {
        border-color: #dc3545;
    }
    
    .rounded-corners .e-input {
        border-radius: 8px;
    }
</style>
```

### Conditional Styling

Apply classes based on state:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    @bind-Value="DateRange"
    CssClass="@GetCssClass()">
</SfDateRangePicker>

@code {
    private DateTime? DateRange { get; set; }
    
    private string GetCssClass()
    {
        return DateRange.HasValue ? "date-selected" : "no-date-selected";
    }
}

<style>
    .date-selected .e-input {
        border-color: #28a745;
        background-color: #f0fdf4;
    }
    
    .no-date-selected .e-input {
        border-color: #ccc;
    }
</style>
```

### Dark Mode Styling

Create dark theme variant:

```razor
<SfDateRangePicker TValue="DateTime?" 
    CssClass="@(IsDarkMode ? "dark-mode" : "")"
    Placeholder="Select dates">
</SfDateRangePicker>

@code {
    private bool IsDarkMode { get; set; } = true;
}

<style>
    .dark-mode .e-input {
        background-color: #2d2d2d;
        color: #ffffff;
        border-color: #444;
    }
    
    .dark-mode .e-calendar {
        background-color: #1e1e1e;
        color: #ffffff;
    }
    
    .dark-mode .e-calendar .e-header {
        background-color: #2d2d2d;
    }
</style>
```

## Popup Behavior

### Opening Popup on Input Click

By default, the popup opens when you click the input field. The DateRangePicker handles this automatically:

```razor
<SfDateRangePicker TValue="DateTime?" 
    Placeholder="Click to open calendar">
</SfDateRangePicker>
```

### Custom Icon

Add a custom button or icon to open the popup:

```razor
@using Syncfusion.Blazor.Calendars

<div style="display: flex; gap: 10px;">
    <SfDateRangePicker TValue="DateTime?" 
        @ref="DatePickerRef"
        Placeholder="Select dates">
    </SfDateRangePicker>
    
    <button @onclick="OpenPopup" title="Pick dates">
        📅
    </button>
</div>

@code {
    private SfDateRangePicker<DateTime?> DatePickerRef;
    
    private async Task OpenPopup()
    {
        await DatePickerRef.ShowPopupAsync();
    }
}
```

### Programmatic Popup Control

Open and close the popup from code:

```razor
@using Syncfusion.Blazor.Calendars

<button @onclick="ShowCalendar">Show Calendar</button>
<button @onclick="HideCalendar">Hide Calendar</button>

<SfDateRangePicker TValue="DateTime?" 
    @ref="DateRangeRef"
    @bind-Value="SelectedRange">
</SfDateRangePicker>

@code {
    private SfDateRangePicker<DateTime?> DateRangeRef;
    private DateTime? SelectedRange { get; set; }
    
    private async Task ShowCalendar()
    {
        await DateRangeRef.ShowPopupAsync();
    }
    
    private async Task HideCalendar()
    {
        await DateRangeRef.HidePopupAsync();
    }
}
```

### Close on Selection

Control when popup closes after date selection (handled automatically, but can be customized):

```razor
<SfDateRangePicker TValue="DateTime?" 
    @bind-Value="DateRange"
    Placeholder="Closes after selection">
</SfDateRangePicker>

@code {
    private DateTime? DateRange { get; set; }
}
```

## Week Number Display

### Enable Week Numbers

Display week numbers in the calendar (ISO 8601 format):

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    WeekNumber="true"
    Placeholder="Select dates">
</SfDateRangePicker>
```

This shows week numbers (1-52/53) along with the date grid, useful for business reporting and planning.

### Week Number Use Cases

**Project Planning:**
```razor
<SfDateRangePicker TValue="DateTime?" 
    WeekNumber="true"
    Placeholder="Select sprint period">
</SfDateRangePicker>
```

**Fiscal Calendar:**
```razor
@* Displays week numbers for fiscal year tracking *@
<SfDateRangePicker TValue="DateTime?" 
    WeekNumber="true"
    Min="@FiscalYearStart"
    Max="@FiscalYearEnd">
</SfDateRangePicker>

@code {
    private DateTime FiscalYearStart = new(2024, 4, 1);
    private DateTime FiscalYearEnd = new(2025, 3, 31);
}
```

### Styling Week Numbers

Customize appearance of week numbers:

```razor
<SfDateRangePicker TValue="DateTime?" 
    WeekNumber="true"
    CssClass="custom-weeks">
</SfDateRangePicker>

<style>
    .custom-weeks .e-week-number {
        background-color: #f0f0f0;
        color: #666;
        font-weight: bold;
        min-width: 35px;
    }
</style>
```

## Visual Appearance

### Input Field Styling

Customize the input field appearance:

```razor
<SfDateRangePicker TValue="DateTime?" 
    Placeholder="Select dates"
    CssClass="outlined-input">
</SfDateRangePicker>

<style>
    .outlined-input .e-input {
        border: 2px solid #e0e0e0;
        border-radius: 4px;
        padding: 10px 12px;
        font-size: 14px;
        transition: border-color 0.3s;
    }
    
    .outlined-input .e-input:focus {
        border-color: #007bff;
        outline: none;
    }
</style>
```

### Calendar Header Styling

Customize calendar header appearance:

```razor
<style>
    .e-calendar .e-header {
        background: linear-gradient(to right, #007bff, #0056b3);
        color: white;
        font-weight: bold;
    }
    
    .e-calendar .e-header button {
        color: white;
    }
</style>
```

### Highlight Today's Date

Customize today's date appearance:

```razor
<style>
    .e-calendar .e-today {
        background-color: #fff3cd;
        border: 2px solid #ffc107;
        border-radius: 4px;
    }
</style>
```

### Selected Date Range Styling

Customize selected range appearance:

```razor
<style>
    .e-calendar .e-range {
        background-color: #e7f3ff;
    }
    
    .e-calendar .e-range-start,
    .e-calendar .e-range-end {
        background-color: #007bff;
        color: white;
        border-radius: 4px;
    }
</style>
```

## Theme Integration

### Available Themes

Apply different theme stylesheets in your HTML:

**Bootstrap 5:**
```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```

**Material Design:**
```html
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />
```

**Fluent UI:**
```html
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />
```

**Tailwind CSS:**
```html
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />
```

### Theme-Aware Components

Match component styling to active theme:

```razor
@using Syncfusion.Blazor.Calendars

<select @onchange="@((ChangeEventArgs e) => ChangeTheme(e.Value.ToString()))">
    <option value="bootstrap5">Bootstrap 5</option>
    <option value="material">Material</option>
    <option value="fluent">Fluent</option>
</select>

<SfDateRangePicker TValue="DateTime?" Placeholder="Select dates"></SfDateRangePicker>

@code {
    private void ChangeTheme(string theme)
    {
        // Theme CSS files are loaded in index.html
        // This updates the application theme
    }
}
```

### Dark Mode Theme

Use dark theme variant:

```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5-dark.css" rel="stylesheet" />
```

Or create custom dark mode:

```razor
<SfDateRangePicker TValue="DateTime?" 
    CssClass="@(IsDarkMode ? "dark-theme" : "")">
</SfDateRangePicker>

@code {
    private bool IsDarkMode { get; set; }
}

<style>
    :root.dark-theme {
        --text-color: #e0e0e0;
        --bg-color: #1e1e1e;
        --border-color: #444;
    }
</style>
```

## Next Steps

- Read [events-and-interactions.md](events-and-interactions.md) for handling user interactions
- Read [data-and-globalization.md](data-and-globalization.md) for localization options
- Read [accessibility.md](accessibility.md) for keyboard navigation
