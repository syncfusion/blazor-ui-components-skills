# Accessibility

## WCAG 2.2 AA Compliance Overview

The Syncfusion Blazor DateRangePicker component follows accessibility standards and guidelines:

| Standard | Level | Status |
|----------|-------|--------|
| WCAG 2.2 | AA | ✓ Fully Compliant |
| Section 508 | Partial | ◐ Partially Compliant |
| ADA (Americans with Disabilities Act) | - | ✓ Compliant |
| Screen Reader Support | - | ✓ Full Support |
| Keyboard Navigation | - | ✓ Full Support |
| Color Contrast | - | ✓ Compliant |
| Mobile Access | - | ✓ Supported |

## WAI-ARIA Attributes

The DateRangePicker component uses WAI-ARIA attributes to communicate state to assistive technologies:

### aria-expanded

Indicates whether the calendar popup is open or closed:

```html
<!-- When closed -->
<input aria-expanded="false" />

<!-- When open -->
<input aria-expanded="true" />
```

### aria-disabled

Indicates the disabled state of the component:

```html
<!-- Normal state -->
<input aria-disabled="false" />

<!-- Disabled state -->
<input aria-disabled="true" />
```

### aria-activedescendant

Indicates the currently focused date in the calendar grid:

```html
<input aria-activedescendant="sf-drp-date-20240115" />
```

### aria-label and aria-labelledby

Provide accessible names for the component:

```razor
@using Syncfusion.Blazor.Calendars

<!-- Using aria-label -->
<SfDateRangePicker TValue="DateTime?" 
    aria-label="Select reservation dates">
</SfDateRangePicker>

<!-- Using aria-labelledby -->
<label id="date-label">Travel Dates</label>
<SfDateRangePicker TValue="DateTime?" 
    aria-labelledby="date-label">
</SfDateRangePicker>
```

## Keyboard Navigation

The DateRangePicker supports comprehensive keyboard navigation following WAI-ARIA practices:

### Input Navigation

Use these keys before opening the calendar popup:

| Windows Key | Mac Key | Action |
|-------------|---------|--------|
| Alt + ↓ | ⌥ + ↓ | Opens the calendar popup |
| Alt + ↑ | ⌥ + ↑ | Closes the calendar popup |
| Esc | Esc | Closes the popup |
| Tab | Tab | Moves to next element |
| Shift + Tab | ⇧ + Tab | Moves to previous element |

### Calendar Navigation

Navigate within the open calendar using:

| Windows Key | Mac Key | Action |
|-------------|---------|--------|
| ↑ | ↑ | Focuses the same day of the previous week |
| ↓ | ↓ | Focuses the same day of the next week |
| ← | ← | Focuses the previous day |
| → | → | Focuses the next day |
| Home | Home | Focuses the first day of the month |
| End | End | Focuses the last day of the month |
| Page Up | Page Up | Focuses the same date of the previous month |
| Page Down | Page Down | Focuses the same date of the next month |
| Shift + Page Up | ⇧ + Page Up | Focuses the same date of the previous year |
| Shift + Page Down | ⇧ + Page Down | Focuses the same date of the next year |
| Ctrl + Home | ⌘ + Home | Focuses the first date of the current year |
| Ctrl + End | ⌘ + End | Focuses the last date of the current year |
| Enter | Enter | Selects the currently focused date |
| Alt + → | ⌥ + → | Focuses forward through popup container |
| Alt + ← | ⌥ + ← | Focuses backward through popup container |

### Month Navigation

Navigate months and years in the calendar header:

```
- Click navigation arrows to move between months
- Use Page Up/Page Down to jump months
- Use Shift+Page Up/Page Down to jump years
- Use Ctrl+Home/End to jump to year boundaries
```

## Example Keyboard Navigation

### Keyboard Navigation Code

Implement custom keyboard handling if needed:

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    @ref="DateRangeRef"
    @onkeydown="@HandleKeyDown"
    Placeholder="Select dates">
</SfDateRangePicker>

@code {
    private SfDateRangePicker<DateTime?> DateRangeRef;
    
    private async Task HandleKeyDown(KeyboardEventArgs args)
    {
        if (args.Key == "Enter")
        {
            // Handle Enter key
            await DateRangeRef.HidePopupAsync();
        }
        else if (args.Key == "Escape")
        {
            // Handle Escape key
            await DateRangeRef.HidePopupAsync();
        }
        else if (args.CtrlKey && args.Key == "Home")
        {
            // Jump to start of year
        }
    }
}
```

### Programmatic Focus Management

Focus the component for keyboard navigation:

```razor
<button @onclick="FocusDatePicker">Open DatePicker</button>

<SfDateRangePicker TValue="DateTime?" 
    @ref="DatePickerRef"
    id="date-picker">
</SfDateRangePicker>

@code {
    private SfDateRangePicker<DateTime?> DatePickerRef;
    
    private async Task FocusDatePicker()
    {
        // Show popup
        await DatePickerRef.ShowPopupAsync();
        
        // Focus the input for keyboard navigation
        // (handled automatically by component)
    }
}
```

## Screen Reader Support

The DateRangePicker announces information to screen readers:

### Announcements

Screen readers announce:
- Component type: "Calendar date range picker"
- Current state: "Popup open", "Popup closed"
- Selected dates: "Start date: January 15, 2024"
- Date focus: "January 20, 2024"
- Navigation: "Next month", "Previous month"

### Testing with Screen Readers

Test with popular screen readers:

**NVDA (Windows, Free):**
- Download from: https://www.nvaccess.org/
- Enable reading mode (Ctrl+Alt+N)
- Navigate with Tab and arrow keys

**JAWS (Windows, Commercial):**
- Popular commercial screen reader
- Follows standard keyboard navigation patterns

**VoiceOver (Mac/iOS):**
- Built-in to macOS
- Enable with Cmd+F5
- Navigate with VO+arrow keys

**TalkBack (Android):**
- Built-in screen reader
- Swipe gestures for navigation

### Custom Screen Reader Text

Provide additional context:

```razor
@using Syncfusion.Blazor.Calendars

<div role="region" aria-label="Booking date selection">
    <label id="booking-label">Hotel Booking Dates</label>
    
    <SfDateRangePicker TValue="DateTime?" 
        aria-labelledby="booking-label"
        aria-describedby="date-help">
    </SfDateRangePicker>
    
    <small id="date-help">
        Select your check-in and check-out dates. 
        Dates must be within the next 6 months.
    </small>
</div>
```

## Color Contrast

The DateRangePicker meets WCAG AA color contrast requirements:

### Default Contrast Ratios

- **Text on background:** 7:1 (exceeds AA requirement of 4.5:1)
- **UI elements:** 3:1 (meets AA requirement)
- **Focus indicators:** 3:1 minimum

### Ensuring Custom Styling Maintains Contrast

If customizing colors, maintain sufficient contrast:

```razor
<style>
    /* Good contrast: 5.2:1 ratio */
    .custom-daterange .e-input {
        background-color: #ffffff;
        color: #333333;
    }
    
    /* Avoid: Insufficient contrast 2.5:1 */
    .poor-contrast .e-input {
        background-color: #ffffff;
        color: #aaaaaa;  /* Too light - insufficient contrast */
    }
</style>
```

### Check Contrast with Tools

Use online contrast checkers:
- **WebAIM Contrast Checker:** https://webaim.org/resources/contrastchecker/
- **Chrome DevTools:** Inspect element → Accessibility tab
- **Lighthouse (Chrome):** Built-in audit tool

## Mobile Device Support

The DateRangePicker is fully accessible on mobile devices:

### Touch Navigation

- **Single tap** - Selects date
- **Swipe left/right** - Navigates between months
- **Double tap** - Zooms calendar if needed

### Mobile Accessibility Features

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" 
    Placeholder="Tap to select dates">
</SfDateRangePicker>
```

- Large touch targets (minimum 48x48 pixels)
- Adequate spacing between elements
- Responsive design for various screen sizes
- Landscape and portrait support

### Testing on Mobile

Test with:
- **Android Accessibility Scanner**: https://play.google.com/store/apps/details?id=com.google.android.apps.accessibility.auditor
- **iOS Accessibility Inspector**: Built-in to Xcode
- **Mobile screen readers**: TalkBack (Android), VoiceOver (iOS)

## Focus Management

### Visible Focus Indicators

The component shows clear focus indicators:

```razor
<style>
    /* Default focus indicator */
    .e-input:focus {
        outline: 2px solid #0066cc;
        outline-offset: 2px;
    }
</style>
```

### Custom Focus Styling

Ensure focus is always visible:

```razor
<SfDateRangePicker TValue="DateTime?" 
    CssClass="custom-focus"
    Placeholder="Select dates">
</SfDateRangePicker>

<style>
    /* High contrast focus indicator */
    .custom-focus .e-input:focus {
        outline: 3px solid #ff6b35;
        outline-offset: 2px;
        box-shadow: 0 0 0 4px rgba(255, 107, 53, 0.1);
    }
</style>
```

### Tab Order

Ensure logical tab order in your form:

```razor
<form>
    <!-- Tab order: 1 -->
    <input type="text" tabindex="1" />
    
    <!-- Tab order: 2 -->
    <SfDateRangePicker TValue="DateTime?" tabindex="2"></SfDateRangePicker>
    
    <!-- Tab order: 3 -->
    <button tabindex="3">Submit</button>
</form>
```

## Accessible Form Integration

### Proper Labels

Always provide labels for accessibility:

```razor
@using Syncfusion.Blazor.Calendars

<!-- ✓ Good: Associated label -->
<label for="booking-dates">Booking Dates</label>
<SfDateRangePicker TValue="DateTime?" 
    id="booking-dates">
</SfDateRangePicker>

<!-- ✗ Bad: No label -->
<SfDateRangePicker TValue="DateTime?"></SfDateRangePicker>
```

### Error Messages

Provide accessible error feedback:

```razor
<div role="alert" aria-live="polite">
    @if (ShowError)
    {
        <span style="color: red;">Please select a valid date range</span>
    }
</div>

<label for="dates">Travel Dates</label>
<SfDateRangePicker TValue="DateTime?" 
    @bind-Value="DateRange"
    id="dates"
    aria-describedby="error-message">
</SfDateRangePicker>
```

### Help Text

Provide accessible help:

```razor
<label for="checkout">Check-out Date</label>
<SfDateRangePicker TValue="DateTime?" 
    id="checkout"
    aria-describedby="checkout-help">
</SfDateRangePicker>
<small id="checkout-help">
    Must be after check-in date
</small>
```

## Accessibility Validation

### Using Axe DevTools

The component passes automated accessibility testing with axe-core:

1. Install Chrome extension: [axe DevTools](https://chrome.google.com/webstore/detail/axe-devtools-web-accessibility-testing/lhdoppojpmngadmnkpklempisson)
2. Right-click and select "Scan page with axe DevTools"
3. Check for violations in DateRangePicker

### Manual Testing Checklist

- [ ] Keyboard navigation works with Tab, Arrow keys, Enter
- [ ] Focus visible on all interactive elements
- [ ] Screen reader announces component correctly
- [ ] Color contrast meets WCAG AA (4.5:1 for text)
- [ ] No keyboard traps
- [ ] Mobile touch targets minimum 48x48 pixels
- [ ] Error messages announced to screen readers
- [ ] Labels properly associated

### Browser Accessibility Inspector

Test in your browser:

```
Chrome: F12 → More Tools → Accessibility
Firefox: F12 → Inspector → Accessibility
Edge: F12 → Elements → Accessibility
```

## ARIA Live Regions

Announce dynamic changes to users:

```razor
<div aria-live="polite" aria-atomic="true">
    @if (DateRange.HasValue)
    {
        <p>Selected range: @DateRange.Value.ToString("MMM dd, yyyy")</p>
    }
</div>

<SfDateRangePicker TValue="DateTime?" 
    @bind-Value="DateRange">
</SfDateRangePicker>

@code {
    private DateTime? DateRange { get; set; }
}
```

## Resources

- **WCAG 2.2 Guidelines**: https://www.w3.org/WAI/WCAG22/
- **WAI-ARIA Authoring**: https://www.w3.org/WAI/ARIA/apg/
- **WebAIM**: https://webaim.org/
- **A11y Project**: https://www.a11yproject.com/
- **Syncfusion Accessibility Docs**: https://blazor.syncfusion.com/documentation/common/accessibility

## Next Steps

- Read [customization-and-styling.md](customization-and-styling.md) for visual design
- Read [events-and-interactions.md](events-and-interactions.md) for handling user interactions
- Read [range-selection.md](range-selection.md) for data binding
