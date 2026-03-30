# Calendar Views & Navigation

## Table of Contents
- [View Types Overview](#view-types-overview)
- [Start Property](#start-property)
- [Depth Property](#depth-property)
- [View Restrictions](#view-restrictions)
- [Drill-Down Navigation](#drill-down-navigation)
- [Common Patterns](#common-patterns)
- [Examples](#examples)

---

## View Types Overview

The Blazor Calendar provides three hierarchical views for date selection:

| View | Level | Displays | Use Case |
|------|-------|----------|----------|
| **Month** | Detailed | Days in current month | Precise date selection |
| **Year** | Middle | Months in current year | Month-level selection |
| **Decade** | Broad | Years in current decade | Year-level selection |

### View Hierarchy

```
Decade (broadest)
   ↓
  Year
   ↓
 Month (most detailed)
```

---

## Start Property

### Purpose

The `Start` property sets the **initial view** displayed when the Calendar renders. It also acts as the **upper limit** for drill-up navigation (you cannot navigate to broader views than Start).

### Syntax

```razor
<SfCalendar TValue="DateTime?" Start="CalendarView.Month"></SfCalendar>
```

### Values

- `CalendarView.Month` - Shows day grid (default)
- `CalendarView.Year` - Shows month grid
- `CalendarView.Decade` - Shows year grid

### Examples

#### Example 1: Start with Month View (Default)

```razor
@using Syncfusion.Blazor.Calendars

<SfCalendar TValue="DateTime?" Start="CalendarView.Month"></SfCalendar>
```

**Output:** Calendar displays current month with days. Clicking month header allows drill-up to Year view.

#### Example 2: Start with Year View

```razor
@using Syncfusion.Blazor.Calendars

<SfCalendar TValue="DateTime?" Start="CalendarView.Year"></SfCalendar>
```

**Output:** Calendar shows current year with months. Cannot navigate beyond Year view (no Decade view available). Clicking a month drills down to that month's days.

#### Example 3: Start with Decade View

```razor
@using Syncfusion.Blazor.Calendars

<SfCalendar TValue="DateTime?" Start="CalendarView.Decade"></SfCalendar>
```

**Output:** Calendar displays years in current decade (e.g., 2020-2029). Cannot navigate beyond Decade. Clicking a year drills down to year's months.

---

## Depth Property

### Purpose

The `Depth` property sets the **deepest (most detailed) view** allowed. It acts as the **lower limit** for drill-down navigation. You cannot drill deeper than the Depth view.

### Syntax

```razor
<SfCalendar TValue="DateTime?" Depth="CalendarView.Month"></SfCalendar>
```

### Values

- `CalendarView.Month` - Most detailed allowed
- `CalendarView.Year` - Month-level is forbidden
- `CalendarView.Decade` - Both Month and Year forbidden

### Important Rules

1. **Depth must be "smaller" (more detailed) than Start**, otherwise navigation is disabled
2. If `Start == Depth`, the Calendar view is fixed and cannot change
3. Both properties must follow this hierarchy:
   ```
   Start ≥ Depth (in terms of detail level)
   Decade ≥ Year ≥ Month
   ```

### Examples

#### Example 1: Year Selection Only

Restrict to Year view - cannot drill down to individual days:

```razor
@using Syncfusion.Blazor.Calendars

<SfCalendar TValue="DateTime?" 
    Start="CalendarView.Year" 
    Depth="CalendarView.Year">
</SfCalendar>
```

**Output:** Calendar shows months; cannot drill down to individual days. Clicking a month does nothing (view is locked).

#### Example 2: Decade to Year Selection

Users drill from decade → year, but not to individual days:

```razor
@using Syncfusion.Blazor.Calendars

<SfCalendar TValue="DateTime?" 
    Start="CalendarView.Decade" 
    Depth="CalendarView.Year">
</SfCalendar>
```

**Output:** Shows years in decade. Clicking a year shows its months. Cannot drill to individual days.

#### Example 3: Full Range (Default)

Allow all three views:

```razor
@using Syncfusion.Blazor.Calendars

<SfCalendar TValue="DateTime?" 
    Start="CalendarView.Decade" 
    Depth="CalendarView.Month">
</SfCalendar>
```

**Output:** Decade → Year → Month hierarchy fully enabled.

---

## View Restrictions

### Use Cases

View restrictions are useful for:
1. **Budget Selection** - Allow year selection, not individual days
2. **Report Period** - Select month ranges, not specific dates
3. **Preference Selection** - Choose decade/era, not precise dates
4. **Simplified UX** - Remove navigation complexity for specific use cases

### Restriction Patterns

#### Pattern 1: Month View Only (No Navigation)

```razor
<SfCalendar TValue="DateTime?" 
    Start="CalendarView.Month" 
    Depth="CalendarView.Month">
</SfCalendar>
```

**Use:** Display current month only; no drilling or breadcrumb navigation.

#### Pattern 2: Year Selection Only

```razor
<SfCalendar TValue="DateTime?" 
    Start="CalendarView.Year" 
    Depth="CalendarView.Year">
</SfCalendar>
```

**Use:** Pick a month; cannot select specific day or navigate to other years.

#### Pattern 3: Decade → Month (Skip Year)

```razor
<SfCalendar TValue="DateTime?" 
    Start="CalendarView.Decade" 
    Depth="CalendarView.Month">
</SfCalendar>
```

**Behavior:** Decade → Month (no Year intermediate). Clicking a year shows month grid directly.

---

## Drill-Down Navigation

### What is Drill-Down?

Navigation from broader to more detailed views:
- Click month header in Month view → goes to Year view
- Click year header in Year view → goes to Decade view

### Drill-Down Boundaries

- **Start** property limits drill-up (cannot go broader)
- **Depth** property limits drill-down (cannot go deeper)

### Example: Restricted Drill-Down

```razor
@using Syncfusion.Blazor.Calendars

<h3>Select a date from March onwards (2024)</h3>

<SfCalendar TValue="DateTime?" 
    Start="CalendarView.Year" 
    Depth="CalendarView.Month"
    Min="@(new DateTime(2024, 3, 1))"
    Value="@SelectedDate">
</SfCalendar>

@code {
    public DateTime? SelectedDate { get; set; } = DateTime.Now;
}
```

**Behavior:**
1. Displays year view (months)
2. Months before March are disabled
3. Clicking a month shows that month's days
4. Cannot click month header to go back to decade view (Start is Year)

---

## Common Patterns

### Pattern 1: Year Picker

Select year only, then month/day selected elsewhere:

```razor
<SfCalendar TValue="DateTime?" 
    Start="CalendarView.Decade" 
    Depth="CalendarView.Year">
</SfCalendar>
```

### Pattern 2: Month Picker

Select month within current year:

```razor
<SfCalendar TValue="DateTime?" 
    Start="CalendarView.Year" 
    Depth="CalendarView.Year">
</SfCalendar>
```

### Pattern 3: Standard Date Picker

Full three-level navigation:

```razor
<SfCalendar TValue="DateTime?" 
    Start="CalendarView.Decade" 
    Depth="CalendarView.Month">
</SfCalendar>
```

### Pattern 4: Today-Only View

Display specific month as fixed view:

```razor
<SfCalendar TValue="DateTime?" 
    Start="CalendarView.Month" 
    Depth="CalendarView.Month"
    Value="@DateTime.Now">
</SfCalendar>
```

---

## Examples

### Example 1: Basic View Navigation

```razor
@using Syncfusion.Blazor.Calendars

<h3>Calendar with Year/Month/Decade Navigation</h3>

<p>Selected: @SelectedDate?.ToString("dd/MM/yyyy")</p>

<SfCalendar TValue="DateTime?" 
    @bind-Value="@SelectedDate"
    Start="CalendarView.Decade" 
    Depth="CalendarView.Month">
</SfCalendar>

@code {
    public DateTime? SelectedDate { get; set; } = DateTime.Now;
}
```

**Interaction:**
1. Shows decade (e.g., 2020-2029)
2. Click a year → shows that year's months
3. Click a month → shows that month's days
4. Click a day → date selected

### Example 2: Year Constraint + View Restriction

```razor
@using Syncfusion.Blazor.Calendars

<h3>Select Year Between 2022-2025</h3>

<SfCalendar TValue="DateTime?" 
    Value="@SelectedDate"
    Start="CalendarView.Decade" 
    Depth="CalendarView.Year"
    Min="@(new DateTime(2022, 1, 1))"
    Max="@(new DateTime(2025, 12, 31))">
</SfCalendar>

@code {
    public DateTime? SelectedDate { get; set; } = new DateTime(2024, 1, 1);
}
```

**Behavior:** Year selection (months per year) with years outside 2022-2025 disabled.

### Example 3: Switchable Views

```razor
@using Syncfusion.Blazor.Calendars

<h3>Choose View Level</h3>

<div>
    <button @onclick="() => SetDepth(CalendarView.Month)">Day Picker</button>
    <button @onclick="() => SetDepth(CalendarView.Year)">Month Picker</button>
    <button @onclick="() => SetDepth(CalendarView.Decade)">Year Picker</button>
</div>

<p>Selected: @SelectedDate</p>

<SfCalendar TValue="DateTime?" 
    Value="@SelectedDate"
    Start="CalendarView.Decade" 
    Depth="@CurrentDepth">
</SfCalendar>

@code {
    public DateTime? SelectedDate { get; set; } = DateTime.Now;
    public CalendarView CurrentDepth { get; set; } = CalendarView.Month;
    
    private void SetDepth(CalendarView view)
    {
        CurrentDepth = view;
    }
}
```

**Output:** Buttons toggle between Day/Month/Year selection modes.

### Example 4: Restricted Historic Dates

```razor
@using Syncfusion.Blazor.Calendars

<h3>Select a historical date (1900-1950)</h3>

<SfCalendar TValue="DateTime?" 
    Value="@SelectedDate"
    Start="CalendarView.Decade" 
    Depth="CalendarView.Month"
    Min="@(new DateTime(1900, 1, 1))"
    Max="@(new DateTime(1950, 12, 31))">
</SfCalendar>

@code {
    public DateTime? SelectedDate { get; set; } = new DateTime(1925, 6, 15);
}
```

**Output:** Calendar starts in 1900s decade view; full drill-down available; years/months outside 1900-1950 are disabled.

---

## Keyboard Navigation

When views are unrestricted, use keyboard shortcuts for navigation:

| Key | Action |
|-----|--------|
| `Ctrl + ↑` | Drill-up to broader view (Month → Year → Decade) |
| `Ctrl + ↓` | Drill-down to detailed view (Decade → Year → Month) |
| `Arrow Keys` | Navigate within current view |
| `Enter` | Select current item |

---

## Edge Cases & Best Practices

### Edge Case 1: Invalid Start/Depth Combination

**Bad:**
```razor
<SfCalendar Start="CalendarView.Month" Depth="CalendarView.Year"></SfCalendar>
```

**Problem:** Depth (Year) is broader than Start (Month), which violates hierarchy. Navigation disabled.

**Fix:**
```razor
<SfCalendar Start="CalendarView.Year" Depth="CalendarView.Month"></SfCalendar>
```

### Edge Case 2: Fixed View

If `Start == Depth`, the view never changes:

```razor
<SfCalendar Start="CalendarView.Year" Depth="CalendarView.Year"></SfCalendar>
```

**Result:** Month picker with no drill-up/down capability.

### Best Practices

1. **Always ensure:** `Start ≥ Depth` in hierarchy (Decade ≥ Year ≥ Month)
2. **Combine with Min/Max** for date range + view restriction
3. **Test keyboard navigation** if drill-down is enabled
4. **Provide feedback** on selected view (show selected year/month in header)
5. **Document restrictions** for users (e.g., "Year selection only")
