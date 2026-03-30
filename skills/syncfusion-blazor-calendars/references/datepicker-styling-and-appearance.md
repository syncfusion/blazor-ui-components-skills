# Styling and Appearance in Blazor DatePicker

This guide covers CSS customization, theme selection, state styling, and appearance options for the DatePicker component.

## Theme Selection

Choose from built-in Syncfusion themes.

### Bootstrap 5 Theme

```html
<!-- In App.razor or layout -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```

Modern Bootstrap 5 styling with clean, minimal appearance.

### Material Design Theme

```html
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />
```

Google Material Design with rounded corners and shadows.

### Fluent UI Theme

```html
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />
```

Microsoft Fluent design system styling.

### Tailwind CSS Theme

```html
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />
```

Tailwind-inspired minimal and utility-first design.

### Switch Themes Dynamically

```razor
<div>
    <label>Select Theme:</label>
    <select @onchange="@((ChangeEventArgs e) => ChangeTheme(e.Value.ToString()))">
        <option value="bootstrap5">Bootstrap 5</option>
        <option value="material">Material</option>
        <option value="fluent">Fluent</option>
        <option value="tailwind">Tailwind</option>
    </select>
</div>

<SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>

@code {
    DateTime? selectedDate;

    void ChangeTheme(string theme)
    {
        var link = document.querySelector("link[href*='themes']");
        if (link != null)
        {
            link.SetAttribute("href", $"_content/Syncfusion.Blazor.Themes/{theme}.css");
        }
    }
}
```

## Readonly and Disabled States

Control user interaction with the DatePicker.

### Readonly State

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              Readonly="true">
</SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
}
```

User can view and select dates, but cannot type directly into the input field.

### Disabled State

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              Enabled="false">
</SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
}
```

Component is completely disabled; user cannot interact at all.

### Toggle Between States

```razor
<button @onclick="@(() => isDisabled = !isDisabled)">
    @(isDisabled ? "Enable" : "Disable")
</button>

<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              Enabled="@(!isDisabled)"
              Readonly="@isReadonly">
</SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
    bool isDisabled = false;
    bool isReadonly = false;
}
```

### Conditional Styling Based on State

```razor
<div class="@(isDisabled ? "disabled-state" : "")">
    <SfDatePicker TValue="DateTime?" Value="@selectedDate" 
                  Enabled="@(!isDisabled)">
    </SfDatePicker>
</div>

<style>
    .disabled-state {
        opacity: 0.5;
        pointer-events: none;
    }
</style>

@code {
    DateTime? selectedDate;
    bool isDisabled = false;
}
```

## RTL Support

Enable right-to-left layout for Arabic and other RTL languages.

### Automatic RTL

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate">
</SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
}
```

RTL is automatically enabled for Arabic locale.

### Explicit RTL

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              EnableRtl="true">
</SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
}
```

### RTL Container

```razor
<div dir="rtl">
    <SfDatePicker TValue="DateTime?" Value="@selectedDate" 
                  EnableRtl="true">
    </SfDatePicker>
</div>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
}
```

## Custom CSS Classes

Apply custom styling by adding CSS classes.

### Adding Custom Class

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              CssClass="my-custom-picker">
</SfDatePicker>

<style>
    .my-custom-picker {
        border: 2px solid #007bff;
        border-radius: 8px;
    }
    
    .my-custom-picker .e-input {
        font-size: 16px;
        padding: 10px;
    }
</style>

@code {
    DateTime? selectedDate;
}
```

### Multiple Custom Classes

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              CssClass="highlight-border rounded-picker">
</SfDatePicker>

<style>
    .highlight-border {
        border: 2px solid #ffc107;
    }
    
    .rounded-picker {
        border-radius: 12px;
    }
</style>

@code {
    DateTime? selectedDate;
}
```

## Input Field Styling

Customize the input field appearance.

### Input Width

```razor
<div style="width: 300px;">
    <SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>
</div>

@code {
    DateTime? selectedDate;
}
```

### Placeholder Styling

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              Placeholder="Select a date">
</SfDatePicker>

<style>
    .e-input-group .e-input::placeholder {
        color: #999;
        font-style: italic;
    }
</style>

@code {
    DateTime? selectedDate;
}
```

### Focus Border Color

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>

<style>
    .e-input-group.e-control-wrapper.e-input-focus {
        border-color: #28a745;
        box-shadow: 0 0 0 0.2rem rgba(40, 167, 69, 0.25);
    }
</style>

@code {
    DateTime? selectedDate;
}
```

## Calendar Popup Styling

Customize the calendar dropdown appearance.

### Popup Width

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate"
              PopupWidth="350px">
</SfDatePicker>

@code {
    DateTime? selectedDate;
}
```

### Custom Popup Style

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>

<style>
    .e-datepicker .e-popup {
        border-radius: 8px;
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.2);
    }
    
    .e-datepicker .e-calendar {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    }
</style>

@code {
    DateTime? selectedDate;
}
```

## Icon and Button Styling

Customize icons and buttons within the DatePicker.

### Clear Button Styling

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              ShowClearButton="true">
</SfDatePicker>

<style>
    .e-clear-icon {
        color: #dc3545;
        font-size: 14px;
    }
</style>

@code {
    DateTime? selectedDate;
}
```

## Dark Mode Support

Implement dark theme styling.

### Dark Theme CSS

```html
<!-- In App.razor -->
<link href="_content/Syncfusion.Blazor.Themes/material-dark.css" rel="stylesheet" />
```

### Dark Mode Toggle

```razor
<button @onclick="@(() => isDarkMode = !isDarkMode)">
    @(isDarkMode ? "☀️ Light Mode" : "🌙 Dark Mode")
</button>

<SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>

<style>
    @if (isDarkMode)
    {
        <style>
            body {
                background-color: #1e1e1e;
                color: #e0e0e0;
            }
            
            .e-input-group {
                background-color: #2d2d2d;
            }
        </style>
    }
</style>

@code {
    DateTime? selectedDate;
    bool isDarkMode = false;
}
```

## Special Date Styling

Style specific dates with custom appearance.

### Highlight Today

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              SpecialDates="@specialDates">
</SfDatePicker>

<style>
    .today-highlight {
        background-color: #ffc107 !important;
        font-weight: bold;
    }
</style>

@code {
    DateTime? selectedDate;
    
    List<SpecialDate> specialDates = new()
    {
        new SpecialDate { Date = DateTime.Now.Date, CssClass = "today-highlight" }
    };
}
```

### Highlight Weekends

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              SpecialDates="@weekendDates">
</SfDatePicker>

<style>
    .weekend {
        background-color: #e0e0e0;
        opacity: 0.7;
    }
</style>

@code {
    DateTime? selectedDate;
    
    List<SpecialDate> weekendDates => GetWeekends();
    
    List<SpecialDate> GetWeekends()
    {
        var dates = new List<SpecialDate>();
        var current = new DateTime(2026, 3, 1);
        var end = new DateTime(2026, 3, 31);
        
        while (current <= end)
        {
            if (current.DayOfWeek == DayOfWeek.Saturday || 
                current.DayOfWeek == DayOfWeek.Sunday)
            {
                dates.Add(new SpecialDate { Date = current, CssClass = "weekend" });
            }
            current = current.AddDays(1);
        }
        
        return dates;
    }
}
```

## Responsive Styling

Make DatePicker responsive across screen sizes.

### Mobile-Friendly DatePicker

```razor
<div class="datepicker-container">
    <SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>
</div>

<style>
    @media (max-width: 576px) {
        .datepicker-container {
            width: 100%;
        }
        
        .e-datepicker {
            width: 100% !important;
        }
    }
</style>

@code {
    DateTime? selectedDate;
}
```

### Flexbox Layout

```razor
<div style="display: flex; gap: 10px;">
    <div style="flex: 1;">
        <label>Start Date:</label>
        <SfDatePicker TValue="DateTime?" @bind-Value="@startDate"></SfDatePicker>
    </div>
    
    <div style="flex: 1;">
        <label>End Date:</label>
        <SfDatePicker TValue="DateTime?" @bind-Value="@endDate"></SfDatePicker>
    </div>
</div>

@code {
    DateTime? startDate;
    DateTime? endDate;
}
```

## Accessibility Styling

Ensure proper color contrast and visual indicators.

### High Contrast Mode

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>

<style>
    @media (prefers-contrast: more) {
        .e-input-group {
            border-width: 3px;
            font-weight: bold;
        }
    }
</style>

@code {
    DateTime? selectedDate;
}
```

### Focus Indicator

```razor
<style>
    .e-input-group:focus-within {
        box-shadow: 0 0 0 3px rgba(0, 123, 255, 0.5);
        outline: 2px solid #007bff;
    }
</style>
```

## Troubleshooting

### Issue: Theme CSS not loading
**Solution:** Verify CSS path in App.razor: `_content/Syncfusion.Blazor.Themes/{theme}.css`

### Issue: Custom styles not applying
**Solution:** Use `!important` for overriding Syncfusion styles, or increase CSS specificity

### Issue: Readonly/Disabled styling not visible
**Solution:** Check if component has default disabled styling; add explicit CSS

### Issue: Dark mode flickering
**Solution:** Load dark theme CSS conditionally or apply theme before rendering

## Next Steps

- **Accessibility:** Ensure [keyboard navigation and ARIA support](accessibility.md)
- **Responsive:** Configure [mobile and responsive layouts](server-vs-webassembly.md)
- **Advanced:** Customize [special calendars and themes](advanced-features.md)
