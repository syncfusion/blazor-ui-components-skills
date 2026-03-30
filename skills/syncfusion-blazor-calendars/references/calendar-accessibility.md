# Accessibility & Keyboard Navigation

## Table of Contents
- [Accessibility Standards](#accessibility-standards)
- [ARIA Attributes](#aria-attributes)
- [Keyboard Navigation](#keyboard-navigation)
- [Screen Reader Support](#screen-reader-support)
- [Testing for Accessibility](#testing-for-accessibility)
- [Best Practices](#best-practices)

---

## Accessibility Standards

The Syncfusion Blazor Calendar meets strict accessibility standards:

### Compliance Levels

| Standard | Status | Details |
|----------|--------|---------|
| **WCAG 2.2 Level AA** | ✅ Compliant | Web Content Accessibility Guidelines |
| **WCAG 2.2 Level AAA** | ⚠️ Partial | Enhanced accessibility |
| **Section 508** | ⚠️ Partial | US Federal accessibility |
| **ADA** | ✅ Compliant | Americans with Disabilities Act |
| **ARIA 1.2** | ✅ Compliant | Accessible Rich Internet Applications |

### What This Means

- **Keyboard accessible** - Fully navigable via keyboard
- **Screen reader compatible** - Works with NVDA, JAWS, VoiceOver
- **Color contrast** - Meets WCAG AA (4.5:1 for text)
- **Focus management** - Clear visible focus indicators
- **Semantic HTML** - Proper structure for assistive tech

---

## ARIA Attributes

### What are ARIA Attributes?

ARIA (Accessible Rich Internet Applications) attributes enhance semantics for assistive technologies:

```html
<div role="grid" aria-label="Date picker calendar">
    <div role="gridcell" aria-selected="false" aria-disabled="false">5</div>
    <div role="gridcell" aria-selected="true" aria-disabled="false">15</div>
</div>
```

### Calendar ARIA Attributes

| Attribute | Purpose | Example |
|-----------|---------|---------|
| `role="grid"` | Calendar is a grid structure | Applied to calendar table |
| `role="gridcell"` | Each day is a grid cell | Applied to each day `<td>` |
| `aria-label` | Text label for screen reader | "Previous month" on nav buttons |
| `aria-selected` | Current selected date | `aria-selected="true"` |
| `aria-disabled` | Out-of-range or disabled dates | `aria-disabled="true"` |
| `aria-activedescendant` | Currently focused item | Points to focused date ID |
| `tabindex` | Keyboard focus order | `tabindex="0"` for interactive |

### Example: ARIA Implementation

Syncfusion automatically adds ARIA attributes:

```html
<!-- Automatic ARIA markup (rendered by Syncfusion) -->
<table role="grid" aria-label="Date picker calendar">
    <thead>
        <tr role="row">
            <th>Sun</th>
            <th>Mon</th>
            <!-- ... -->
        </tr>
    </thead>
    <tbody>
        <tr role="row">
            <td role="gridcell" aria-selected="false">1</td>
            <td role="gridcell" aria-selected="true" class="e-selected">15</td>
            <td role="gridcell" aria-disabled="true">30</td>
        </tr>
    </tbody>
</table>
```

### Screen Reader Announcement

When user focuses on a date cell, screen reader announces:
```
"15, selected, Tuesday, March 2024"
```

---

## Keyboard Navigation

### Supported Keyboard Shortcuts

The Calendar provides comprehensive keyboard support:

#### Navigation Keys

| Key | Action |
|-----|--------|
| `↑` | Move to same day of previous week |
| `↓` | Move to same day of next week |
| `←` | Move to previous day |
| `→` | Move to next day |
| `Home` | Move to first day of month |
| `End` | Move to last day of month |
| `Page Up` | Move to same date of previous month |
| `Page Down` | Move to same date of next month |
| `Shift + Page Up` | Move to same date of previous year |
| `Shift + Page Down` | Move to same date of next year |

#### View Navigation

| Key Combination | Action |
|-----------------|--------|
| `Ctrl + ↑` | Drill-up to broader view (Month → Year) |
| `Ctrl + ↓` | Drill-down to detailed view (Year → Month) |
| `Ctrl + Home` | Jump to first date of year |
| `Ctrl + End` | Jump to last date of year |

#### Selection & Entry

| Key | Action |
|-----|--------|
| `Enter` | Select focused date |
| `Space` | Select/toggle focused date |

### Example: Testing Keyboard Navigation

```razor
@using Syncfusion.Blazor.Calendars

<h3>Calendar - Try Keyboard Navigation</h3>

<p><strong>Instructions:</strong> Click the calendar and use arrow keys to navigate.</p>

<SfCalendar TValue="DateTime?" @bind-Value="@SelectedDate"></SfCalendar>

<div style="margin-top: 20px; padding: 10px; background-color: #f0f0f0; border-radius: 4px;">
    <h4>Keyboard Shortcuts:</h4>
    <ul>
        <li><strong>Arrow Keys:</strong> Navigate days/months</li>
        <li><strong>Home/End:</strong> Go to first/last day of month</li>
        <li><strong>Page Up/Down:</strong> Previous/next month</li>
        <li><strong>Ctrl + Arrow:</strong> Change view level</li>
        <li><strong>Enter:</strong> Select focused date</li>
    </ul>
    <p style="margin-top: 10px;"><strong>Selected Date:</strong> @SelectedDate?.ToString("dddd, dd MMMM yyyy")</p>
</div>

@code {
    public DateTime? SelectedDate { get; set; } = DateTime.Now;
}
```

### Keyboard Navigation Flow

1. **Tab into calendar** - Focus moves to calendar container
2. **Arrow keys** - Navigate cells (24 cells visible on screen)
3. **Enter** - Select focused date
4. **Tab out** - Focus moves to next interactive element
5. **Shift+Tab** - Move backward through focus order

---

## Screen Reader Support

### Supported Screen Readers

- **NVDA** (Windows, free)
- **JAWS** (Windows, commercial)
- **VoiceOver** (Mac/iOS, built-in)
- **TalkBack** (Android, built-in)

### What Screen Reader Users Hear

When navigating calendar:

```
// Initially loading calendar
"Calendar, month view. March 2024. Instructions: Use arrow keys to navigate dates."

// Focusing on a date
"15, selected, Tuesday, March 19, 2024"

// Focusing on disabled date (out of range)
"30, disabled, Friday, March 29, 2024"

// Drilling up to year view
"Year 2024, list of months"

// Focusing on month
"March 2024, clickable"
```

### Best Practices for Screen Reader Users

1. **Provide context** - Explain calendar purpose before component
2. **Label calendar** - Use descriptive `aria-label`
3. **Announce selections** - Read back selected date
4. **Guide navigation** - Explain available keyboard shortcuts

### Example: Screen Reader Friendly Component

```razor
@using Syncfusion.Blazor.Calendars

<h3>Accessible Appointment Booking</h3>

<!-- Instruction text for screen reader users -->
<div role="region" aria-label="Instructions for using calendar">
    <p>Select your appointment date using the calendar below. Use arrow keys to navigate, Enter to select.</p>
</div>

<!-- Calendar with descriptive label -->
<div>
    <label for="appointment-calendar">Appointment Date (Required)</label>
    <SfCalendar TValue="DateTime?" 
        @bind-Value="@AppointmentDate"
        id="appointment-calendar"
        aria-label="Select appointment date - available dates shown">
    </SfCalendar>
</div>

<!-- Announce selected date -->
<div role="status" aria-live="polite" aria-atomic="true">
    @if (AppointmentDate != null)
    {
        <p>Appointment selected for @AppointmentDate.Value.ToString("dddd, MMMM d, yyyy")</p>
    }
</div>

@code {
    public DateTime? AppointmentDate { get; set; }
}
```

---

## Testing for Accessibility

### Manual Testing Checklist

#### Keyboard Navigation
- [ ] Can tab into calendar
- [ ] Arrow keys move focus predictably
- [ ] Page Up/Down work
- [ ] Can enter/year views with Ctrl+arrow
- [ ] Enter/Space select dates
- [ ] Can tab out of calendar
- [ ] No keyboard traps

#### Screen Reader
- [ ] Calendar identified as grid/calendar
- [ ] Date cells have proper roles
- [ ] Selected/disabled states announced
- [ ] Month/year header announced
- [ ] Navigation buttons labeled
- [ ] Instructions provided or documented

#### Visual Design
- [ ] Focus indicator clearly visible
- [ ] Color not sole indicator (redundant cues)
- [ ] Text contrast ≥4.5:1 (AA) or ≥7:1 (AAA)
- [ ] Text is resizable to 200%
- [ ] No text smaller than 12px (unless necessary)

#### Mobile/Touch
- [ ] Touch targets ≥44x44px
- [ ] Zoom works correctly
- [ ] Screen reader gestures work

### Automated Testing Tools

#### Browser DevTools Accessibility Audit

1. Press F12 (Developer Tools)
2. Go to "Lighthouse" tab
3. Select "Accessibility"
4. Run audit
5. Review violations

#### Free Tools

- **axe DevTools** - Browser extension
- **Accessibility Checker** - Web validator
- **WebAIM WAVE** - Web accessibility evaluator
- **Lighthouse** - Built into Chrome DevTools

### Example: Running Accessibility Audit

```bash
# Using axe-core in Playwright tests
npm install --save-dev @axe-core/playwright

# Test calendar component for violations
const axe = require('axe-core');
// Run axe on calendar element
// Check results for violations
```

---

## Best Practices

### 1. Always Label Form Controls

**Bad:**
```razor
<SfCalendar TValue="DateTime?" @bind-Value="@Date"></SfCalendar>
```

**Good:**
```razor
<label for="event-date">Event Date (Required)</label>
<SfCalendar TValue="DateTime?" 
    @bind-Value="@Date"
    id="event-date"
    aria-describedby="date-help">
</SfCalendar>
<p id="date-help">Select a date at least 7 days in the future.</p>
```

### 2. Provide Error Messages

**Bad:**
```razor
@if (!IsValidDate(Date))
{
    <p style="color: red;">Invalid date</p>
}
```

**Good:**
```razor
<div role="alert" aria-live="assertive">
    @if (!IsValidDate(Date))
    {
        <p style="color: red;">❌ Invalid date: must be in future</p>
    }
</div>
```

### 3. Ensure Sufficient Color Contrast

**Bad:**
```css
/* Light gray on white - less than 3:1 contrast */
.e-calendar .e-day { color: #d0d0d0; }
```

**Good:**
```css
/* Dark gray on white - more than 4.5:1 contrast */
.e-calendar .e-day { color: #333333; }
```

### 4. Avoid Color-Only Indicators

**Bad:**
```razor
<!-- Red dates are unavailable (color-blind users won't know) -->
<span style="background-color: red">5</span>
```

**Good:**
```razor
<!-- Red + disabled state + icon -->
<span style="background-color: red" aria-disabled="true">
    5 <span aria-label="not available">✗</span>
</span>
```

### 5. Provide Keyboard Alternatives

**Bad:**
```razor
<div @onmouseenter="ShowPopup">Hover for info</div>
```

**Good:**
```razor
<button @onmouseenter="ShowPopup" @onfocus="ShowPopup">
    Info <span aria-label="keyboard accessible">ℹ️</span>
</button>
```

### 6. Test with Real Users

The best accessibility practice is testing with actual users with disabilities:
- Keyboard-only users
- Screen reader users
- Low vision users
- Mobile/touch users
- Color-blind users

### 7. Document Accessibility Features

In your component documentation:
```markdown
## Accessibility

This calendar component:
- Supports keyboard navigation (arrow keys, Enter, Page Up/Down)
- Works with screen readers (NVDA, JAWS, VoiceOver)
- Meets WCAG 2.2 Level AA standards
- Includes focus indicators
- Supports high contrast mode

Keyboard shortcuts:
- ↑/↓/←/→: Navigate dates
- Enter: Select date
- Ctrl+↑: Zoom out to year view
- Ctrl+↓: Zoom in to day view
```

---

## Accessibility in Your Applications

### Creating Inclusive Date Selection Experience

```razor
@using Syncfusion.Blazor.Calendars

<div class="booking-section" role="region" aria-label="Appointment booking form">
    
    <!-- Instructions -->
    <div class="instructions" role="note">
        <h2>Book Your Appointment</h2>
        <p>Use the calendar below to select your preferred date. 
           Keyboard users: Use arrow keys to navigate, Enter to select.
           Screen reader users: Available dates will be announced.</p>
    </div>
    
    <!-- Calendar -->
    <fieldset>
        <legend>Select Appointment Date (Required)</legend>
        
        <SfCalendar TValue="DateTime?" 
            @bind-Value="@AppointmentDate"
            Min="@minAppointmentDate"
            Max="@maxAppointmentDate">
            <CalendarEvents TValue="DateTime?" 
                ValueChange="@OnDateChanged"
                OnRenderDayCell="@MarkBooked">
            </CalendarEvents>
        </SfCalendar>
    </fieldset>
    
    <!-- Confirmation -->
    <div role="status" aria-live="polite" aria-atomic="true" class="confirmation">
        @if (AppointmentDate != null)
        {
            <p>✓ Appointment confirmed for <strong>@AppointmentDate.Value.ToString("dddd, MMMM d, yyyy")</strong></p>
        }
    </div>
    
    <!-- Error Messages -->
    @if (HasError)
    {
        <div role="alert" class="error-message">
            <p>❌ @ErrorMessage</p>
        </div>
    }
    
    <!-- Submit -->
    <button @onclick="SubmitBooking" disabled="@(AppointmentDate == null)">
        Confirm Booking
    </button>
</div>

@code {
    public DateTime? AppointmentDate { get; set; }
    public bool HasError { get; set; }
    public string ErrorMessage { get; set; } = "";
    private DateTime minAppointmentDate = DateTime.Now;
    private DateTime maxAppointmentDate = DateTime.Now.AddDays(90);
    
    private HashSet<int> BookedDates = new() { 5, 12, 19, 26 };
    
    private void OnDateChanged(ChangedEventArgs<DateTime?> args)
    {
        if (args.Value != null && BookedDates.Contains(args.Value.Value.Day))
        {
            HasError = true;
            ErrorMessage = "This date is already booked. Please select another.";
            AppointmentDate = null;
        }
        else
        {
            HasError = false;
        }
    }
    
    private void MarkBooked(RenderDayCellEventArgs args)
    {
        if (BookedDates.Contains(args.Date.Day))
        {
            args.IsDisabled = true;
            args.ClassList.Add("booked");
        }
    }
    
    private async Task SubmitBooking()
    {
        if (AppointmentDate == null)
            return;
        
        // Submit appointment
        Console.WriteLine($"Booking appointment for {AppointmentDate}");
    }
}

<style>
    .booking-section {
        max-width: 600px;
        margin: 20px auto;
        padding: 20px;
    }
    
    .instructions {
        background-color: #e3f2fd;
        padding: 12px;
        border-left: 4px solid #1976d2;
        margin-bottom: 20px;
    }
    
    .confirmation {
        color: #4caf50;
        padding: 10px;
        background-color: #e8f5e9;
        border-radius: 4px;
        margin: 10px 0;
    }
    
    .error-message {
        color: #d32f2f;
        padding: 10px;
        background-color: #ffebee;
        border-radius: 4px;
        margin: 10px 0;
    }
    
    button:disabled {
        opacity: 0.5;
        cursor: not-allowed;
    }
    
    button:focus {
        outline: 3px solid #1976d2;
        outline-offset: 2px;
    }
</style>
```

---

## Resources

- [WCAG 2.2 Guidelines](https://www.w3.org/WAI/WCAG22/quickref/)
- [ARIA Authoring Practices Guide](https://www.w3.org/WAI/ARIA/apg/)
- [WebAIM Articles](https://webaim.org/)
- [Syncfusion Accessibility](https://www.syncfusion.com/accessibility)
