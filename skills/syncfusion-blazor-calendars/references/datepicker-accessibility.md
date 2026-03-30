# Accessibility in Blazor DatePicker

This guide ensures your DatePicker implementation meets WCAG 2.1 accessibility standards for all users.

## WCAG Compliance

DatePicker must be accessible to users with disabilities including visual, motor, and cognitive impairments.

### WCAG 2.1 Level AA Compliance

```razor
<label for="datepicker">Select Event Date:</label>
<SfDatePicker TValue="DateTime?" 
              Value="@selectedDate"
              Placeholder="DD/MM/YYYY"
              aria-label="Event date picker"
              aria-describedby="date-help">
</SfDatePicker>

<small id="date-help">Format: DD/MM/YYYY. Use arrow keys to navigate the calendar.</small>

@code {
    DateTime? selectedDate;
}
```

**Key Points:**
- Proper label association with `for` and `id`
- `aria-label` describes component purpose
- `aria-describedby` links to help text
- Instructions for keyboard navigation

## Keyboard Navigation

Users must navigate and control DatePicker using keyboard only.

### Supported Keyboard Keys

| Key | Action |
|-----|--------|
| `Tab` | Move to DatePicker, move between fields |
| `Shift+Tab` | Move backward through fields |
| `Enter` | Open calendar popup, select date |
| `Escape` | Close calendar popup |
| `Arrow Up` | Previous week |
| `Arrow Down` | Next week |
| `Arrow Left` | Previous day |
| `Arrow Right` | Next day |
| `Page Up` | Previous month |
| `Page Down` | Next month |
| `Alt+Page Up` | Previous year |
| `Alt+Page Down` | Next year |
| `Home` | First day of month |
| `End` | Last day of month |

### Keyboard Example

```razor
<SfDatePicker TValue="DateTime?" 
              Value="@selectedDate"
              Placeholder="Select date (use arrow keys to navigate)">
</SfDatePicker>

<p>
    <kbd>Tab</kbd> - Focus | <kbd>Enter</kbd> - Open | 
    <kbd>↑↓←→</kbd> - Navigate | <kbd>Esc</kbd> - Close
</p>

@code {
    DateTime? selectedDate;
}
```

### Focus Management

```razor
<button @onclick="@(() => datePicker?.Focus())">Focus DatePicker</button>

<SfDatePicker @ref="datePicker" TValue="DateTime?" Value="@selectedDate"></SfDatePicker>

@code {
    SfDatePicker<DateTime?> datePicker;
    DateTime? selectedDate;
}
```

Programmatically manage focus for better keyboard navigation flow.

## ARIA Attributes and Roles

Semantic HTML and ARIA make components understandable to assistive technologies.

### Required ARIA Attributes

```razor
<SfDatePicker TValue="DateTime?" 
              Value="@selectedDate"
              aria-label="Birth date selection"
              aria-expanded="false"
              role="combobox">
</SfDatePicker>

@code {
    DateTime? selectedDate;
}
```

**ARIA Attributes:**
- `aria-label` - Describes component purpose
- `aria-expanded` - Indicates if popup is open/closed
- `role="combobox"` - Identifies as date/calendar control

### ARIA with States

```razor
<SfDatePicker TValue="DateTime?" 
              Value="@selectedDate"
              Enabled="@(!isDisabled)"
              aria-label="Event date"
              aria-disabled="@isDisabled"
              aria-invalid="@hasError"
              aria-errormessage="error-text">
</SfDatePicker>

<span id="error-text" style="color: red; display:@(hasError ? "block" : "none")">
    Invalid date selected
</span>

@code {
    DateTime? selectedDate;
    bool isDisabled = false;
    bool hasError = false;
}
```

### ARIA Live Regions

```razor
<SfDatePicker TValue="DateTime?" 
              aria-live="polite"
              aria-atomic="true">
    <DatePickerEvents TValue="DateTime?" ValueChange="@OnDateChange"></DatePickerEvents>
</SfDatePicker>

<div aria-live="polite" role="status">
    @if (selectedDate.HasValue)
    {
        <span>Selected: @selectedDate.Value.ToString("dddd, dd MMMM yyyy")</span>
    }
</div>

@code {
    DateTime? selectedDate;

    void OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        selectedDate = args.Value;
    }
}
```

Screen readers announce date selection changes with `aria-live="polite"`.

## Screen Reader Support

Ensure DatePicker works with screen readers like NVDA, JAWS, and VoiceOver.

### Label Association

```razor
<label for="event-date">Event Date:</label>
<SfDatePicker TValue="DateTime?" 
              id="event-date"
              Value="@selectedDate">
</SfDatePicker>

@code {
    DateTime? selectedDate;
}
```

Screen reader announces: "Event Date, edit, text"

### Descriptive Placeholder

```razor
<SfDatePicker TValue="DateTime?" 
              Placeholder="Enter date as DD/MM/YYYY"
              aria-describedby="date-format-help">
</SfDatePicker>

<span id="date-format-help">
    Use DD/MM/YYYY format. Navigate calendar with arrow keys.
</span>

@code {
}
```

### Error Messages

```razor
<SfDatePicker TValue="DateTime?" 
              @bind-Value="@selectedDate"
              aria-invalid="@hasError"
              aria-errormessage="date-error">
</SfDatePicker>

<span id="date-error" role="alert" style="color: red; display:@(hasError ? "block" : "none")">
    Please select a date in the future
</span>

@code {
    DateTime? selectedDate;
    bool hasError => selectedDate.HasValue && selectedDate < DateTime.Now.Date;
}
```

Screen reader announces errors with `role="alert"`.

## Color Contrast

Ensure sufficient contrast between foreground and background for low-vision users.

### Minimum Contrast Ratios (WCAG AA)
- **Normal text:** 4.5:1
- **Large text:** 3:1
- **UI components:** 3:1

### High Contrast Example

```razor
<div style="background-color: #ffffff; color: #000000;">
    <SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>
</div>

<style>
    /* Ensure input text is dark on light background */
    .e-input {
        color: #000000;
        background-color: #ffffff;
    }
    
    /* High contrast focus indicator */
    .e-input:focus {
        border: 3px solid #0066cc;
        outline: 2px solid #0066cc;
    }
</style>

@code {
    DateTime? selectedDate;
}
```

### Check Contrast Tools
- WebAIM Contrast Checker
- Paciello Group Color Contrast Analyzer
- Browser dev tools accessibility inspector

## Focus Indicators

Provide visible focus indicators for keyboard users.

### Clear Focus Indicator

```razor
<style>
    .e-input-group:focus-within {
        outline: 3px solid #0066cc;
        outline-offset: 2px;
    }
    
    .e-input-group:focus-within .e-input {
        box-shadow: 0 0 0 4px rgba(0, 102, 204, 0.25);
    }
</style>

<SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>

@code {
    DateTime? selectedDate;
}
```

### Motion Preferences

```razor
<style>
    @media (prefers-reduced-motion: reduce) {
        .e-input-group {
            transition: none;
        }
    }
</style>
```

Respect user preferences for reduced motion.

## Touch Accessibility

Optimize for touch devices and mobile users.

### Touch Target Size

```razor
<div style="padding: 10px;">
    <SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>
</div>

<style>
    /* Ensure touch targets are at least 48x48 pixels */
    .e-input-group {
        min-height: 48px;
        min-width: 48px;
    }
</style>

@code {
    DateTime? selectedDate;
}
```

### Mobile-Friendly Popup

```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate"
              PopupWidth="100%">
</SfDatePicker>

<style>
    @media (max-width: 576px) {
        .e-datepicker .e-popup {
            width: 90vw !important;
            max-width: 400px;
        }
    }
</style>

@code {
    DateTime? selectedDate;
}
```

## Cognitive Accessibility

Support users with cognitive disabilities.

### Clear Instructions

```razor
<div>
    <label for="meeting-date">Meeting Date</label>
    <SfDatePicker TValue="DateTime?" 
                  id="meeting-date"
                  Value="@selectedDate"
                  Placeholder="DD/MM/YYYY"
                  Format="dd/MM/yyyy">
    </SfDatePicker>
    
    <p style="font-size: 14px; margin-top: 5px;">
        Click the field or use arrow keys to select a date. 
        Type in format: DD/MM/YYYY
    </p>
</div>

@code {
    DateTime? selectedDate;
}
```

### Simple, Consistent Language

- Use plain language, avoid jargon
- Consistent button labels and terminology
- Limit cognitive load with focused options

### Help and Error Prevention

```razor
<SfDatePicker TValue="DateTime?" 
              @bind-Value="@selectedDate"
              Min="@minSelectableDate"
              Format="dd/MM/yyyy"
              Placeholder="DD/MM/YYYY">
</SfDatePicker>

<div style="margin-top: 10px; padding: 10px; background-color: #e7f3ff; border-left: 4px solid #2196F3;">
    <strong>ℹ️ Tip:</strong> You can select today or any future date.
</div>

@code {
    DateTime? selectedDate;
    private DateTime minSelectableDate = DateTime.Now.Date;
}
```

## Testing Accessibility

### Keyboard Navigation Test

1. Tab to DatePicker
2. Press Enter to open calendar
3. Use arrow keys to navigate dates
4. Press Enter to select
5. Verify focus order is logical

### Screen Reader Test

Test with:
- NVDA (Windows, free)
- JAWS (Windows, commercial)
- VoiceOver (macOS/iOS, built-in)
- TalkBack (Android, built-in)

Verify:
- Label is announced
- Current selection is announced
- Error messages are announced
- Navigation instructions are clear

### Contrast Test

```razor
<!-- Run through WAVE or Axe accessibility checker -->
<SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>

@code {
    DateTime? selectedDate;
}
```

## Accessible Example

Complete accessible DatePicker:

```razor
<div class="form-group">
    <label for="event-date" class="form-label">
        Event Date <span aria-label="required">*</span>
    </label>
    
    <SfDatePicker TValue="DateTime?" 
                  id="event-date"
                  @bind-Value="@eventDate"
                  Min="@minEventDate"
                  Format="dd/MM/yyyy"
                  Placeholder="DD/MM/YYYY"
                  aria-label="Event date selection"
                  aria-describedby="date-help"
                  aria-invalid="@hasError"
                  aria-errormessage="date-error">
    </SfDatePicker>
    
    <small id="date-help">
        Format: DD/MM/YYYY. Select today or future dates. Use arrow keys to navigate.
    </small>
    
    @if (hasError)
    {
        <div id="date-error" role="alert" style="color: #d32f2f; margin-top: 5px;">
            ⚠️ Please select a valid future date
        </div>
    }
</div>

<style>
    .form-group {
        margin-bottom: 20px;
    }
    
    .form-label {
        display: block;
        font-weight: bold;
        margin-bottom: 5px;
    }
    
    .e-input-group:focus-within {
        outline: 3px solid #0066cc;
        outline-offset: 2px;
    }
    
    @media (prefers-reduced-motion: reduce) {
        * {
            animation: none !important;
            transition: none !important;
        }
    }
</style>

@code {
    DateTime? eventDate;
    private DateTime minEventDate = DateTime.Now.Date;
    bool hasError => eventDate.HasValue && eventDate < DateTime.Now.Date;
}
```

## Troubleshooting

### Issue: Screen reader not announcing selection
**Solution:** Add ARIA live region with `aria-live="polite"`

### Issue: Keyboard focus not visible
**Solution:** Add explicit focus outline in CSS

### Issue: Color contrast failing
**Solution:** Use WebAIM checker, ensure text/background ratio ≥4.5:1

### Issue: Touch targets too small
**Solution:** Ensure minimum 48x48 pixel touch area

## Accessibility Resources

- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [Syncfusion Accessibility](https://www.syncfusion.com/accessibility)
- [ARIA Authoring Guide](https://www.w3.org/WAI/ARIA/apg/)
- [Accessible Rich Internet Applications](https://www.w3.org/TR/wai-aria/)

## Next Steps

- **Styling:** Apply [accessible styling and themes](styling-and-appearance.md)
- **Events:** Handle [accessibility-aware events](events.md)
- **Testing:** Validate [with accessibility tools](troubleshooting.md)
