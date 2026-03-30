# Styling & Customization

## Table of Contents
- [CSS Structure](#css-structure)
- [Common Customizations](#common-customizations)
- [Practical Examples](#practical-examples)
- [Themes & Color Schemes](#themes--color-schemes)
- [Special Date Styling](#special-date-styling)
- [Best Practices](#best-practices)

---

## CSS Structure

### Calendar DOM Elements

Understanding the CSS structure helps target specific elements:

```
.e-calendar (main container)
├── .e-header (top navigation)
│   ├── .e-title (month/year display)
│   ├── span (previous button)
│   └── span (next button)
├── .e-content (date grid)
│   ├── thead (weekday headers)
│   └── tbody (day cells)
│       └── td span.e-day (individual day)
└── .e-footer (bottom - if visible)
    └── .e-btn.e-today (Today button)
```

### CSS Specificity Note

Default Syncfusion classes use `.e-calendar` prefix. When overriding, ensure sufficient specificity:

```css
/* May not override - check your specificity */
.e-calendar .e-day { background-color: red; }

/* Better - more specific */
.e-calendar .e-content td span.e-day { background-color: red; }

/* If still not working, check theme priority */
.e-bootstrap5 .e-calendar .e-day { background-color: red !important; }
```

---

## Common Customizations

### 1. Background Color & Borders

**Customize container styling:**

```css
/* Main calendar container */
.e-calendar {
    background-color: #f5f5f5;
    border: 2px solid #333;
    border-radius: 8px;
    padding: 10px;
}
```

### 2. Day Cell Styling

**Style individual day cells:**

```css
/* Normal day cells */
.e-calendar .e-content span.e-day {
    border: 1px solid #ddd;
    border-radius: 4px;
    padding: 8px;
    font-weight: 500;
}

/* Hover state */
.e-calendar .e-content td:hover span.e-day {
    background-color: #e3f2fd;
    color: #1976d2;
    cursor: pointer;
}

/* Focus state */
.e-calendar .e-content td:focus span.e-day {
    outline: 2px solid #1976d2;
}
```

### 3. Selected Date Styling

**Emphasize the selected date:**

```css
/* Selected date */
.e-calendar .e-content td.e-selected span.e-day {
    background-color: #1976d2;
    color: white;
    font-weight: bold;
}

/* Selected with focus */
.e-calendar .e-content td.e-selected.e-focused-date span.e-day {
    box-shadow: 0 0 0 3px rgba(25, 118, 210, 0.2);
}
```

### 4. Header Styling

**Customize the month/year display:**

```css
/* Title (month/year) */
.e-calendar .e-header .e-title {
    font-size: 18px;
    font-weight: 600;
    color: #333;
}

/* Navigation buttons */
.e-calendar .e-header span {
    color: #666;
    font-size: 16px;
    transition: color 0.2s ease;
}

.e-calendar .e-header span:hover {
    color: #1976d2;
}
```

### 5. Weekday Header Styling

**Style the weekday row (Sun, Mon, Tue, etc.):**

```css
/* Weekday header row */
.e-calendar .e-content thead {
    background-color: #e3f2fd;
    font-weight: bold;
    color: #1976d2;
}

.e-calendar .e-content thead th {
    padding: 8px;
    text-align: center;
}
```

### 6. Today Button

**Style the footer Today button:**

```css
/* Today button */
.e-calendar .e-btn.e-today {
    background-color: #4caf50;
    color: white;
    border: none;
    padding: 6px 12px;
    border-radius: 4px;
    font-weight: 500;
}

.e-calendar .e-btn.e-today:hover {
    background-color: #45a049;
}
```

---

## Practical Examples

### Example 1: Light Modern Theme

```razor
@using Syncfusion.Blazor.Calendars

<h3>Modern Light Calendar</h3>

<SfCalendar TValue="DateTime?"></SfCalendar>

<style>
    .e-calendar {
        background: white;
        border: 1px solid #e0e0e0;
        border-radius: 12px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
        padding: 16px;
    }
    
    .e-calendar .e-content span.e-day {
        border-radius: 6px;
        transition: all 0.2s ease;
    }
    
    .e-calendar .e-content td:hover span.e-day {
        background-color: #f0f0f0;
        transform: scale(1.05);
    }
    
    .e-calendar .e-content td.e-selected span.e-day {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        border-radius: 6px;
        box-shadow: 0 4px 12px rgba(102, 126, 234, 0.4);
    }
</style>
```

### Example 2: Dark Theme

```razor
@using Syncfusion.Blazor.Calendars

<h3>Dark Mode Calendar</h3>

<SfCalendar TValue="DateTime?"></SfCalendar>

<style>
    .e-calendar {
        background-color: #1e1e1e;
        border: 1px solid #333;
        border-radius: 8px;
        color: #e0e0e0;
    }
    
    .e-calendar .e-header .e-title {
        color: #fff;
        font-weight: 600;
    }
    
    .e-calendar .e-content span.e-day {
        color: #e0e0e0;
    }
    
    .e-calendar .e-content td:hover span.e-day {
        background-color: #333;
    }
    
    .e-calendar .e-content td.e-selected span.e-day {
        background-color: #6200ea;
        color: white;
    }
    
    .e-calendar .e-content thead {
        background-color: #2a2a2a;
        color: #90caf9;
    }
</style>
```

### Example 3: Minimal Clean Design

```razor
@using Syncfusion.Blazor.Calendars

<h3>Minimal Calendar</h3>

<SfCalendar TValue="DateTime?"></SfCalendar>

<style>
    .e-calendar {
        background: white;
        border: none;
        padding: 12px;
    }
    
    .e-calendar .e-content span.e-day {
        border: none;
        font-size: 14px;
        padding: 6px 4px;
    }
    
    .e-calendar .e-content td:hover span.e-day {
        background: #f5f5f5;
    }
    
    .e-calendar .e-content td.e-selected span.e-day {
        background: #000;
        color: white;
    }
    
    .e-calendar .e-header .e-title {
        font-size: 16px;
    }
</style>
```

### Example 4: Corporate Formal

```razor
@using Syncfusion.Blazor.Calendars

<h3>Corporate Calendar</h3>

<SfCalendar TValue="DateTime?"></SfCalendar>

<style>
    .e-calendar {
        background: white;
        border: 2px solid #003366;
        border-radius: 4px;
        font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    }
    
    .e-calendar .e-header {
        background: #003366;
        color: white;
    }
    
    .e-calendar .e-header .e-title {
        color: white;
        font-size: 16px;
        font-weight: 600;
    }
    
    .e-calendar .e-header span {
        color: white;
    }
    
    .e-calendar .e-content span.e-day {
        font-weight: 500;
        color: #333;
    }
    
    .e-calendar .e-content td.e-selected span.e-day {
        background: #003366;
        color: white;
        border-radius: 2px;
    }
</style>
```

---

## Themes & Color Schemes

### Using Built-in Syncfusion Themes

The appearance is controlled primarily by the theme CSS included in `index.html` or `_Layout.cshtml`:

```html
<!-- Bootstrap 5 (Light) -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Bootstrap 5 (Dark) -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5-dark.css" rel="stylesheet" />

<!-- Material (Light) -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- Material (Dark) -->
<link href="_content/Syncfusion.Blazor.Themes/material-dark.css" rel="stylesheet" />

<!-- Fluent (Light) -->
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />

<!-- Fluent (Dark) -->
<link href="_content/Syncfusion.Blazor.Themes/fluent-dark.css" rel="stylesheet" />
```

### Theme-Based Customization

Override theme-specific classes:

```css
/* For Bootstrap 5 theme */
.e-bootstrap5 .e-calendar .e-day {
    background-color: #e7f3ff;
}

/* For Material theme */
.e-material .e-calendar .e-day {
    background-color: #f3e5f5;
}
```

---

## Special Date Styling

### Highlight Holidays

```razor
@using Syncfusion.Blazor.Calendars

<h3>Calendar with Holidays</h3>

<SfCalendar TValue="DateTime?">
    <CalendarEvents TValue="DateTime?" OnRenderDayCell="@HighlightHolidays"></CalendarEvents>
</SfCalendar>

<style>
    .holiday {
        background-color: #ffcccc !important;
        color: #cc0000 !important;
        font-weight: bold;
    }
</style>

@code {
    private HashSet<(int month, int day)> Holidays = new()
    {
        (1, 1),   // New Year
        (12, 25), // Christmas
        (7, 4)    // Independence Day
    };
    
    private void HighlightHolidays(RenderDayCellEventArgs args)
    {
        if (Holidays.Contains((args.Date.Month, args.Date.Day)))
        {
            args.ClassList.Add("holiday");
        }
    }
}
```

### Disabled/Unavailable Dates

```razor
@using Syncfusion.Blazor.Calendars

<h3>Availability Calendar</h3>

<SfCalendar TValue="DateTime?">
    <CalendarEvents TValue="DateTime?" OnRenderDayCell="@MarkUnavailable"></CalendarEvents>
</SfCalendar>

<style>
    .unavailable {
        background-color: #cccccc !important;
        opacity: 0.5;
        cursor: not-allowed !important;
    }
</style>

@code {
    private HashSet<int> UnavailableDates = new() { 5, 12, 19, 26 };
    
    private void MarkUnavailable(RenderDayCellEventArgs args)
    {
        if (UnavailableDates.Contains(args.Date.Day))
        {
            args.IsDisabled = true;
            args.ClassList.Add("unavailable");
        }
    }
}
```

### Upcoming Events Indicator

```css
/* Add a colored dot to dates with events */
.event-dot::after {
    content: "•";
    position: absolute;
    bottom: 2px;
    color: #ff6b6b;
    font-size: 14px;
}
```

---

## Best Practices

### 1. Maintain Accessibility

**Do:**
- Use sufficient color contrast (WCAG AA)
- Don't rely on color alone for information
- Maintain keyboard navigation styling
- Include focus indicators

**Example:**
```css
.e-calendar .e-content td span.e-day:focus {
    outline: 2px solid #1976d2;
    outline-offset: 2px;
}
```

### 2. Responsive Design

**Do:**
- Test on mobile devices
- Adjust padding/font-size for smaller screens
- Ensure touch targets are 44x44px minimum

**Example:**
```css
@media (max-width: 600px) {
    .e-calendar {
        padding: 8px;
    }
    
    .e-calendar .e-content span.e-day {
        padding: 8px;
        font-size: 14px;
    }
}
```

### 3. Performance

**Do:**
- Minimize CSS specificity
- Avoid heavy animations
- Use CSS classes instead of inline styles
- Cache compiled CSS

### 4. Consistency

**Do:**
- Match your app's design system
- Use consistent color palette
- Maintain typography hierarchy
- Use same spacing/sizing across components

### 5. Testing

**Test:**
- Multiple browsers (Chrome, Firefox, Safari, Edge)
- Different themes (light/dark)
- Zoom levels (100-200%)
- Mobile and desktop
- Screen readers

---

## CSS Troubleshooting

### Issue: Styles Not Applied

**Possible causes:**
1. CSS specificity too low
2. Theme stylesheet overriding
3. CSS file not loaded
4. Browser cache

**Solutions:**
```css
/* Increase specificity */
.e-bootstrap5.e-calendar .e-content span.e-day { }

/* Use !important (last resort) */
.e-calendar .e-day { color: red !important; }

/* Check browser DevTools (F12) */
/* Look for overriding styles in Elements panel */
```

### Issue: Hover Effects Not Working

**Check:** `.e-content td:hover span.e-day` selector

**Ensure table structure is intact:**
```html
<table>
  <tbody>
    <tr>
      <td><span class="e-day">1</span></td>
    </tr>
  </tbody>
</table>
```

### Issue: Colors Not Matching Theme

**Verify theme is loaded:**
```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```

**Check theme class on body:**
```html
<body class="e-bootstrap5">
```

---

## Advanced Customization

### Gradient Backgrounds

```css
.e-calendar .e-content td.e-selected span.e-day {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
}
```

### Shadow Effects

```css
.e-calendar {
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.1), 
                0 4px 8px rgba(0, 0, 0, 0.05);
}
```

### Animations

```css
.e-calendar .e-content span.e-day {
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.e-calendar .e-content td:hover span.e-day {
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
}
```

### Border Radius Customization

```css
.e-calendar {
    border-radius: 16px;
    overflow: hidden;
}
```
