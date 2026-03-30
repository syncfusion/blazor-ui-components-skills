---
name: syncfusion-blazor-calendars
description: Comprehensive guide for implementing Syncfusion Blazor calendar components including Calendar, DatePicker, DateRangePicker, DateTimePicker, and TimePicker. Use this when building date selection interfaces, date range selection, date/time input, calendar displays, date constraints, formatting, globalization, and accessible calendar experiences in Blazor applications. Covers all calendar components from Syncfusion.Blazor.Calendars package.
metadata:
    author: "Syncfusion Inc"
    version: "33.1.44"
    category: "Calendars"
---

# Implementing Syncfusion Blazor Calendars

## Calendar

The Calendar component is a lightweight, feature-rich date selection component that provides flexible date picking, multiple views (month/year/decade), date range constraints, localization, and accessibility compliance. Use this skill whenever you need to implement date selection, calendar display, or date-based UI interactions.

### When to Use This Skill

**Use this skill when:**
- Building date pickers, calendar widgets, or date selection interfaces
- Adding date range constraints (Min/Max dates)
- Implementing multi-month or drill-down date selection (Year/Decade views)
- Handling date value changes and events
- Styling calendars or customizing appearance
- Supporting international date formats and RTL layouts
- Creating accessible calendar interfaces with keyboard navigation

**Related components:**
- **DatePicker** (date input with calendar dropdown)
- **DateRangePicker** (date range selection)
- **DateTimePicker** (date + time selection)
- **Scheduler** (complex date-based scheduling)

### Component Overview

| Feature | Details |
|---------|---------|
| **Views** | Month (default), Year, Decade - hierarchical drill-down navigation |
| **Selection** | Single date, date ranges (Min/Max), multi-select |
| **Data Types** | `DateTime`, `DateTime?`, `DateOnly` (.NET 6+) |
| **Binding** | One-way, two-way (`@bind-Value`), dynamic |
| **Events** | `ValueChange`, `OnRenderDayCell`, `Created`, `Destroyed`, `Navigated` |
| **Localization** | Multi-language, RTL support, locale customization |
| **Accessibility** | WCAG 2.2 AA, keyboard navigation, screen reader support |
| **Styling** | CSS customization, theme integration, special date highlights |

### Documentation & Navigation Guide

#### Getting Started
📄 **Read:** [references/getting-started.md](references/calendar-getting-started.md)
- Installation & NuGet packages
- Web & Server app setup
- Blazor namespaces & service registration
- Basic calendar implementation
- Min/Max date constraints

#### Calendar Views & Navigation
📄 **Read:** [references/calendar-views.md](references/calendar-calendar-views.md)
- Month, Year, Decade views
- Start & Depth properties
- View restrictions & drill-down navigation
- Restricting date selection depth

#### Date Selection & Data Binding
📄 **Read:** [references/date-selection-binding.md](references/calendar-date-selection-binding.md)
- Value property & date binding
- One-way vs two-way binding (`@bind-Value`)
- DateOnly support (.NET 6+)
- Dynamic value changes
- Date range constraints

#### Events & Interactions
📄 **Read:** [references/events-handling.md](references/calendar-events-handling.md)
- ValueChange event (date selection)
- Selected/DeSelected events (multi-selection)
- OnRenderDayCell event (custom rendering)
- Created & Destroyed lifecycle events
- Navigated event (view changes)
- Event args & reactive patterns
- Using CalendarEvents child component

#### Styling & Customization
📄 **Read:** [references/styling-appearance.md](references/calendar-styling-appearance.md)
- CSS customization patterns
- Background colors, borders, hover states
- Day cell styling
- Header & title customization
- Today button & selected date styling
- Special date highlighting

#### Localization & Globalization
📄 **Read:** [references/localization-globalization.md](references//calendar-localization-globalization.md)
- Locale support & language files
- Right-to-Left (RTL) layouts
- Islamic calendar & custom calendars
- Week customization (first day of week)
- Date format localization

#### Accessibility & Keyboard Navigation
📄 **Read:** [references/accessibility.md](references/calendar-accessibility.md)
- WCAG 2.2 AA compliance
- Screen reader support (WAI-ARIA)
- Keyboard navigation shortcuts
- Focus management & ARIA attributes

#### Advanced Features & Patterns
📄 **Read:** [references/advanced-features.md](references/calendar-advanced-features.md)
- Show/hide other month dates
- Custom cell rendering patterns
- Multiple date selection
- Week number display
- Performance optimization
- Common integration patterns

### Quick Start Example

```razor
@using Syncfusion.Blazor.Calendars

<!-- Basic Calendar -->
<SfCalendar TValue="DateTime?" Value="@SelectedDate"></SfCalendar>

<!-- Calendar with Date Range -->
<SfCalendar TValue="DateTime?" 
    Value="@SelectedDate" 
    Min="@MinDate" 
    Max="@MaxDate">
</SfCalendar>

<!-- Calendar with Value Change Event -->
<SfCalendar TValue="DateTime?" @bind-Value="@SelectedDate">
    <CalendarEvents TValue="DateTime?" ValueChange="@OnDateChanged"></CalendarEvents>
</SfCalendar>

<!-- Multi-Selection Calendar -->
<SfCalendar TValue="DateTime?" 
    IsMultiSelection="true"
    @bind-Values="@SelectedDates">
    <CalendarEvents TValue="DateTime?" 
        Selected="@OnDateSelected"
        DeSelected="@OnDateDeselected">
    </CalendarEvents>
</SfCalendar>

@code {
    public DateTime? SelectedDate { get; set; } = DateTime.Now;
    public DateTime[] SelectedDates { get; set; } = new DateTime[] { };

    private void OnDateChanged(ChangedEventArgs<DateTime?> args)
    {
        SelectedDate = args.Value;
        // Handle date change
    }
    
    private void OnDateSelected(SelectedEventArgs<DateTime?> args)
    {
        // Handle date added to selection
    }
    
    private void OnDateDeselected(DeSelectedEventArgs<DateTime?> args)
    {
        // Handle date removed from selection
    }
}
```

### Common Patterns

#### Pattern 1: Date Range Selection
Define Min and Max boundaries to constrain user selection:
```razor
<SfCalendar TValue="DateTime?" Value="@SelectedDate" 
    Min="@MinDate" Max="@MaxDate"></SfCalendar>
```

#### Pattern 2: Two-Way Data Binding
Keep Calendar in sync with component state:
```razor
<p>Selected: @SelectedDate</p>
<SfCalendar TValue="DateTime?" @bind-Value="@SelectedDate"></SfCalendar>
```

#### Pattern 3: Custom Cell Rendering
Highlight or customize specific date cells during render:
```razor
<SfCalendar TValue="DateTime?">
    <CalendarEvents TValue="DateTime?" OnRenderDayCell="@CustomizeCell"></CalendarEvents>
</SfCalendar>
```

#### Pattern 4: View Restrictions
Limit navigation to specific view levels (e.g., Year → Month only):
```razor
<SfCalendar TValue="DateTime?" 
    Start="CalendarView.Year" 
    Depth="CalendarView.Month"></SfCalendar>
```

#### Pattern 5: Localization
Render calendar in specific language/culture:
```razor
<SfCalendar TValue="DateTime?" EnableRtl="false"></SfCalendar>
```

### Key Props & When to Use

| Property | Type | Purpose | Example |
|----------|------|---------|---------|
| `Value` | `TValue` | Selected date (one-way binding) | `Value="@SelectedDate"` |
| `@bind-Value` | `TValue` | Two-way data binding | `@bind-Value="@SelectedDate"` |
| `Values` | `DateTime[]` | Multiple selected dates (multi-select mode) | `Values="@SelectedDates"` |
| `@bind-Values` | `DateTime[]` | Two-way binding for multi-selection | `@bind-Values="@SelectedDates"` |
| `IsMultiSelection` | `bool` | Enable multiple date selection | `IsMultiSelection="true"` |
| `Min` | `DateTime` | Earliest selectable date (inclusive) | `Min="@minSelectableDate"` |
| `Max` | `DateTime` | Latest selectable date (inclusive) | `Max="@maxSelectableDate"` |
| `Start` | `CalendarView` | Initial view (Month/Year/Decade) | `Start="CalendarView.Year"` |
| `Depth` | `CalendarView` | Deepest view level for drilling | `Depth="CalendarView.Month"` |
| `ShowTodayButton` | `bool` | Display "Today" button in footer | `ShowTodayButton="true"` |
| `CalendarMode` | `CalendarType` | Calendar system (Gregorian/Islamic) | `CalendarMode="CalendarType.Islamic"` |
| `WeekRule` | `CalendarWeekRule` | Week numbering calculation rule | `WeekRule="CalendarWeekRule.FirstDay"` |
| `DayHeaderFormat` | `DayHeaderFormats` | Day header display format | `DayHeaderFormat="DayHeaderFormats.Short"` |
| `EnableRtl` | `bool` | Right-to-left layout | `EnableRtl="true"` |
| `WeekNumber` | `bool` | Display week numbers in calendar | `WeekNumber="true"` |
| `FirstDayOfWeek` | `int` | First day (0=Sunday, 1=Monday) | `FirstDayOfWeek="1"` |
| `CssClass` | `string` | Custom CSS class for styling | `CssClass="custom-calendar"` |
| `Enabled` | `bool` | Enable/disable component | `Enabled="false"` |

### Common Use Cases

1. **Appointment Booking**: Calendar with date range (available days) + event handling
2. **Date Filter**: Multi-view calendar for drilling down to specific date
3. **Deadline Selection**: Min/Max constraints to valid date ranges
4. **Report Viewing**: Year/Month view for period selection
5. **International App**: RTL + locale support for multi-language UIs
6. **Accessible Forms**: Keyboard-navigable date input with screen reader support

### Working with Events

All calendar components use **child event components** to declare event handlers. This approach provides type safety and clear separation of concerns.

#### Event Child Components

- **Calendar**: Use `<CalendarEvents>` child component
- **DatePicker**: Use `<DatePickerEvents>` child component
- **DateRangePicker**: Use `<DateRangePickerEvents>` child component
- **DateTimePicker**: Use `<DateTimePickerEvents>` child component
- **TimePicker**: Use `<TimePickerEvents>` child component

#### Example: Calendar with Events

```razor
<SfCalendar TValue="DateTime?" @bind-Value="@SelectedDate">
    <CalendarEvents TValue="DateTime?" 
        ValueChange="@OnValueChanged"
        OnRenderDayCell="@OnDayCellRender"
        Navigated="@OnNavigated">
    </CalendarEvents>
</SfCalendar>

@code {
    private DateTime? SelectedDate { get; set; }
    
    private void OnValueChanged(ChangedEventArgs<DateTime?> args)
    {
        Console.WriteLine($"Date changed: {args.Value}");
    }
    
    private void OnDayCellRender(RenderDayCellEventArgs args)
    {
        // Disable weekends
        if (args.Date.DayOfWeek == DayOfWeek.Saturday || args.Date.DayOfWeek == DayOfWeek.Sunday)
        {
            args.IsDisabled = true;
        }
    }
    
    private void OnNavigated(NavigatedEventArgs args)
    {
        Console.WriteLine($"Navigated to: {args.View}");
    }
}
```

#### Common Event Patterns

**Pattern 1: Value Change with Validation**
```razor
<SfDatePicker TValue="DateTime?" @bind-Value="@OrderDate">
    <DatePickerEvents TValue="DateTime?" ValueChange="@ValidateAndUpdate"></DatePickerEvents>
</SfDatePicker>

@code {
    private DateTime? OrderDate { get; set; }
    
    private void ValidateAndUpdate(ChangedEventArgs<DateTime?> args)
    {
        if (args.Value.HasValue && args.Value.Value > DateTime.Now.AddMonths(6))
        {
            // Show error - date too far in future
            OrderDate = DateTime.Now;
        }
        else
        {
            OrderDate = args.Value;
        }
    }
}
```

**Pattern 2: Popup Lifecycle Management**
```razor
<SfDateTimePicker TValue="DateTime?" @bind-Value="@AppointmentTime">
    <DateTimePickerEvents TValue="DateTime?" 
        OnOpen="@HandlePopupOpen"
        OnClose="@HandlePopupClose">
    </DateTimePickerEvents>
</SfDateTimePicker>

@code {
    private DateTime? AppointmentTime { get; set; }
    
    private void HandlePopupOpen(PopupObjectArgs args)
    {
        Console.WriteLine("Popup opened");
        // Can cancel opening: args.Cancel = true;
    }
    
    private void HandlePopupClose(PopupObjectArgs args)
    {
        Console.WriteLine("Popup closed");
    }
}
```

**Pattern 3: Custom Cell Rendering**
```razor
<SfCalendar TValue="DateTime?" @bind-Value="@EventDate">
    <CalendarEvents TValue="DateTime?" OnRenderDayCell="@CustomizeCells"></CalendarEvents>
</SfCalendar>

@code {
    private DateTime? EventDate { get; set; }
    private List<DateTime> EventDates = new List<DateTime> 
    { 
        DateTime.Now.AddDays(5),
        DateTime.Now.AddDays(10)
    };
    
    private void CustomizeCells(RenderDayCellEventArgs args)
    {
        // Highlight event dates
        if (EventDates.Any(d => d.Date == args.Date.Date))
        {
            args.CellData.ClassList = "e-special-date";
        }
    }
}
```

#### Event Argument Types Reference

**IMPORTANT:** Different components use different event argument types for the `ValueChange` event:

| Component | ValueChange Event Argument Type | Notes |
|-----------|--------------------------------|-------|
| **Calendar** | `ChangedEventArgs<TValue>` | For single and multi-date selection |
| **DatePicker** | `ChangedEventArgs<TValue>` | Includes `Value`, `IsInteracted`, `Event` properties |
| **DateTimePicker** | `ChangedEventArgs<TValue>` | Same as DatePicker |
| **TimePicker** | `ChangeEventArgs<TValue>` | Note: Different class name (no 'd') |
| **DateRangePicker** | `RangePickerEventArgs<TValue>` | Includes `StartDate` and `EndDate` properties |

**Key Differences:**
- `ChangedEventArgs<TValue>` (with 'd') - Used by Calendar, DatePicker, DateTimePicker
  - Properties: `Value`, `Values` (for multi-selection), `IsInteracted`, `Event`, `Element`, `Name`
- `ChangeEventArgs<TValue>` (no 'd') - Used by TimePicker only
  - Properties: `Value`, `Text`, `IsInteracted`, `Event`, `Element`
- `RangePickerEventArgs<TValue>` - Used by DateRangePicker only
  - Properties: `StartDate`, `EndDate`, `Text`, `Value`, `IsInteracted`, `Event`, `Element`

---

## DatePicker

A comprehensive skill for implementing and customizing the DatePicker component. This skill helps you install, configure, and integrate DatePicker across Blazor Server, Blazor WebAssembly, and Blazor Web App projects.

### When to Use This Skill

Use this skill when you need to:
- Install and set up Syncfusion DatePicker in any Blazor project type
- Implement basic date selection with input validation
- Configure one-way and two-way data binding
- Set custom date formats and input formats
- Apply date range constraints (min/max dates)
- Highlight or disable special dates
- Handle DatePicker events (ValueChange, OnClose, Focus, Blur)
- Globalize and localize the component (different languages/calendars)
- Support Islamic Calendar or other calendar systems
- Use DateOnly (.NET 6+) with DatePicker
- Style and customize appearance (readonly, disabled states, themes)
- Implement keyboard navigation and accessibility
- Troubleshoot data binding, formatting, or event issues
- Optimize performance in Blazor Server vs WebAssembly
- Integrate date selection with forms and APIs

### Component Overview

The **DatePicker** component provides an intuitive calendar UI for users to select single dates. It supports:
- **Multiple Input Methods:** Direct typing, calendar picker, or both
- **Date Constraints:** Min/max date ranges, disabled dates, special highlights
- **Flexible Formatting:** Custom display formats and input formats
- **Global Support:** Multiple languages, calendars (Gregorian, Islamic), RTL
- **Accessibility:** WCAG compliance, keyboard navigation, screen reader support
- **Event System:** Value changes, popup open/close, focus/blur events
- **Theme Support:** Bootstrap, Material, Fluent, Tailwind CSS

### Installation Quick Start

#### 1. Install NuGet Package

```bash
dotnet add package Syncfusion.Blazor.Calendars
dotnet add package Syncfusion.Blazor.Themes
```

#### 2. Import in `_Imports.razor`

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Calendars
```

#### 3. Register in `Program.cs`

```csharp
builder.Services.AddSyncfusionBlazor();
```

#### 4. Add Theme in `App.razor` or Layout

```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

#### 5. Use in Component

```razor
<SfDatePicker TValue="DateTime?" @bind-Value="@selectedDate">
    <DatePickerEvents TValue="DateTime?" ValueChange="@OnDateChange"></DatePickerEvents>
</SfDatePicker>

@code {
    DateTime? selectedDate = new DateTime(2026, 3, 18);
    
    void OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        selectedDate = args.Value;
    }
}
```

### Documentation Navigation Guide

Choose a reference based on your task:

#### Getting Started
📄 **Read:** [references/getting-started.md](references/datepicker-getting-started.md)
- Installation in Blazor Server, WebAssembly, and Web App
- NuGet package setup
- Theme configuration
- Service registration
- Basic DatePicker example

#### Data Binding
📄 **Read:** [references/data-binding.md](references/datepicker-data-binding.md)
- One-way binding with `Value` property
- Two-way binding with `@bind-Value`
- Dynamic value updates and null handling
- Binding to DateTime vs DateTime?

#### Date Formats & Input
📄 **Read:** [references/date-formats-and-input.md](references/datepicker-date-formats-and-input.md)
- Display format customization (d/M/yyyy, dd/MM/yyyy, etc.)
- Input format patterns
- Placeholder text configuration
- Format parsing and validation
- Custom format examples

#### Date Range & Constraints
📄 **Read:** [references/date-range-and-constraints.md](references/datepicker-date-range-and-constraints.md)
- Min/Max date restrictions
- Disabled specific dates
- Special date highlighting
- Date validation logic
- Preventing invalid selections

#### Events & Interaction
📄 **Read:** [references/events.md](references/datepicker-events.md)
- ValueChange event handling
- OnOpen/OnClose events (popup lifecycle)
- Selected event (date selection in calendar)
- Focus/Blur events
- Cleared event (clear button clicked)
- Created/Destroyed lifecycle events
- Navigated event (calendar view navigation)
- OnRenderDayCell event (custom day cell rendering)
- Using DatePickerEvents child component

#### Advanced Features
📄 **Read:** [references/advanced-features.md](references/datepicker-advanced-features.md)
- Islamic Calendar support
- DateOnly (.NET 6+) support
- Globalization and localization
- Week number display
- Different view modes
- Mask support and input masking

#### Styling & Appearance
📄 **Read:** [references/styling-and-appearance.md](references/datepicker-styling-and-appearance.md)
- CSS customization and themes
- Readonly state styling
- Disabled state styling
- RTL (right-to-left) support
- Theme switching (Bootstrap, Material, Fluent, Tailwind)
- Custom styling examples

#### Accessibility
📄 **Read:** [references/accessibility.md](references/datepicker-accessibility.md)
- WCAG 2.1 compliance
- Keyboard navigation (arrow keys, Enter, Escape)
- ARIA attributes and roles
- Screen reader support
- Color contrast and visual indicators
- Focus management

#### Server vs WebAssembly
📄 **Read:** [references/server-vs-webassembly.md](references/datepicker-server-vs-webassembly.md)
- Blazor Server vs WebAssembly differences
- Blazor Web App (.NET 8+) setup
- Render modes (InteractiveServer, InteractiveWebAssembly)
- Performance considerations
- Event interactivity

#### Troubleshooting
📄 **Read:** [references/troubleshooting.md](references/datepicker-troubleshooting.md)
- Data binding issues
- Date format problems
- Event handling problems
- Styling and layout issues
- Performance optimization
- Common error messages and solutions

### Common Patterns

#### Pattern 1: Basic Date Selection
```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              Placeholder="Select a date"></SfDatePicker>
```
Use when: User needs a simple, standard date picker for form input.

#### Pattern 2: Date Range with Constraints
```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate"
              Min="@minDate" Max="@maxDate"></SfDatePicker>
```
Use when: Date must fall within a specific range (e.g., future dates only, event dates).

#### Pattern 3: Two-Way Binding
```razor
<SfDatePicker TValue="DateTime?" @bind-Value="@formData.BirthDate"></SfDatePicker>
```
Use when: DatePicker must automatically update parent component state or form data.

#### Pattern 4: Event-Driven Logic
```razor
<SfDatePicker TValue="DateTime?">
    <DatePickerEvents TValue="DateTime?" ValueChange="@OnDateSelected"></DatePickerEvents>
</SfDatePicker>

@code {
    void OnDateSelected(ChangedEventArgs<DateTime?> args)
    {
        // Trigger calculations, API calls, or validation
    }
}
```
Use when: Selection should trigger workflows, calculations, or dependent updates.

#### Pattern 5: Formatted Display with Custom Input
```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate"
              Format="dd/MM/yyyy" Placeholder="DD/MM/YYYY"></SfDatePicker>
```
Use when: Date display must match regional or business requirements.

### Key Properties

| Property | Type | Purpose |
|----------|------|---------|
| `Value` | `TValue` | The selected date (one-way binding) |
| `@bind-Value` | `TValue` | Two-way data binding |
| `Min` | `DateTime` | Earliest selectable date |
| `Max` | `DateTime` | Latest selectable date |
| `Format` | `string` | Display format pattern (e.g., "dd/MM/yyyy") |
| `Placeholder` | `string` | Input placeholder text |
| `AllowEdit` | `bool` | Allow manual text input editing |
| `StrictMode` | `bool` | Restrict to valid dates only |
| `EnableMask` | `bool` | Enable input masking for date entry |
| `OpenOnFocus` | `bool` | Open calendar popup when input is focused |
| `FloatLabelType` | `FloatLabelType` | Float label behavior (Auto/Always/Never) |
| `InputFormats` | `string[]` | Array of acceptable input formats |
| `Enabled` | `bool` | Enable/disable component |
| `Readonly` | `bool` | Read-only mode (no user input) |
| `ShowClearButton` | `bool` | Show clear button to reset value |
| `WeekNumber` | `bool` | Display week numbers in calendar |
| `Start` | `CalendarView` | Initial calendar view (Month/Year/Decade) |
| `Depth` | `CalendarView` | Deepest view level allowed |
| `ZIndex` | `int` | Z-index for popup positioning |
| `Width` | `string` | Component width (e.g., "300px") |
| `CssClass` | `string` | Custom CSS class for styling |

### Performance Tips

1. **Use DateTime? (nullable)** for optional date fields—avoids default date confusion
2. **Lazy-load DatePicker** in modals or expandable sections—reduces initial render cost
3. **Minimize format conversions** in event handlers—cache formatted strings when possible
4. **Use InteractiveWebAssembly** for date-heavy UIs—offloads processing to client
5. **Batch value updates** in loops—avoid rapid re-renders with multiple ValueChange triggers

---

## DateRangePicker

A comprehensive skill for implementing and customizing the DateRangePicker component. This component enables users to select date ranges with calendar pickers, providing features for range constraints, customization, accessibility, and globalization.

### When to Use This Skill

Use this skill when you need to:
- Install and set up the DateRangePicker component in Blazor applications
- Create date range selection interfaces
- Set minimum and maximum date constraints
- Bind component values to data models
- Customize placeholder text, first day of week, and visual appearance
- Implement keyboard navigation and accessibility features
- Handle component events (change, blur, focus, etc.)
- Configure globalization and localization for different cultures
- Support DateOnly type for .NET 6+ projects
- Open popups on input click or programmatically
- Display week numbers in the calendar
- Apply custom CSS styling and theming
- Work with RTL (right-to-left) languages
- Debug common DateRangePicker issues

### Component Overview

**SfDateRangePicker** is a calendar-based input component that allows users to select a continuous date range. Key characteristics:

- **Dual Calendar View**: Shows two calendar months for easy range selection
- **Range Constraints**: Min/Max properties enforce date restrictions
- **Flexible Data Binding**: Works with DateTime, DateTime?, and DateOnly types
- **Customizable**: First day of week, placeholder, popup behavior, styling
- **Accessible**: Full WCAG 2.2 AA compliance with keyboard navigation
- **Globalized**: Supports multiple cultures, RTL, and date formatting
- **Event-Driven**: Comprehensive event system for user interactions
- **Mobile-Friendly**: Touch-optimized calendar interface

### Documentation and Navigation Guide

#### Getting Started
📄 **Read:** [references/getting-started.md](references/daterangepicker-getting-started.md)
- Installation via NuGet packages
- Namespace imports and service registration
- Basic component setup (WebAssembly and Server apps)
- Adding theme stylesheet
- Setting Min/Max date constraints
- Rendering your first DateRangePicker

#### Range Selection & Data Binding
📄 **Read:** [references/range-selection.md](references/daterangepicker-range-selection.md)
- Single vs range date selection modes
- Two-way data binding with @bind
- Programmatic range updates
- Clearing date selections
- Using DateOnly type for .NET 6+
- Value change handling

#### Customization & Styling
📄 **Read:** [references/customization-and-styling.md](references/daterangepicker-customization-and-styling.md)
- Configuring first day of week
- Setting placeholder text
- Custom CSS class application
- Opening popup on input click
- Displaying week numbers
- Appearance and visual customization
- Theme integration

#### Data & Globalization
📄 **Read:** [references/data-and-globalization.md](references/daterangepicker-data-and-globalization.md)
- Data binding patterns and best practices
- DateOnly type support and usage
- Globalization configuration
- Culture-specific date formatting
- Right-to-left (RTL) language support
- Localization for international audiences

#### Events & Interactions
📄 **Read:** [references/events-and-interactions.md](references/daterangepicker-events-and-interactions.md)
- ValueChange event (range selection complete)
- RangeSelected event (individual date selections)
- OnOpen/OnClose events (popup lifecycle)
- Focus/Blur events
- Cleared event (clear button clicked)
- Created/Destroyed lifecycle events
- Navigated event (calendar view navigation)
- OnRenderDayCell event (custom day cell rendering)
- Using DateRangePickerEvents child component
- Event argument types (RangePickerEventArgs<TValue>)

#### Accessibility
📄 **Read:** [references/accessibility.md](references/daterangepicker-accessibility.md)
- WCAG 2.2 AA compliance overview
- Keyboard navigation shortcuts
- Calendar month navigation keys
- ARIA attributes and screen reader support
- Mobile device accessibility features
- Testing and validating accessibility

### Quick Start

#### Basic DateRangePicker

```razor
@using Syncfusion.Blazor.Calendars

<SfDateRangePicker TValue="DateTime?" Placeholder="Select a date range"></SfDateRangePicker>
```

#### With Date Constraints

```razor
<SfDateRangePicker TValue="DateTime?" 
    Placeholder="Choose a range" 
    Min="@MinDate" 
    Max="@MaxDate">
</SfDateRangePicker>

@code {
    public DateTime MinDate { get; set; } = new DateTime(2024, 1, 1);
    public DateTime MaxDate { get; set; } = new DateTime(2024, 12, 31);
}
```

#### With Data Binding (StartDate/EndDate)

```razor
<SfDateRangePicker TValue="DateTime?" 
    @bind-StartDate="StartDate" 
    @bind-EndDate="EndDate">
</SfDateRangePicker>

@code {
    private DateTime? StartDate { get; set; }
    private DateTime? EndDate { get; set; }
}
```

#### With Range Constraints

```razor
<SfDateRangePicker TValue="DateTime?" 
    MinDays="3" 
    MaxDays="30"
    @bind-StartDate="StartDate" 
    @bind-EndDate="EndDate">
</SfDateRangePicker>

@code {
    // User must select range between 3 and 30 days
    private DateTime? StartDate { get; set; }
    private DateTime? EndDate { get; set; }
}
```

#### With Preset Ranges

```razor
<SfDateRangePicker TValue="DateTime?">
    <DateRangePickerPresets>
        <DateRangePickerPreset Label="Last 7 Days" Start="@last7DaysStart" End="@currentDate"></DateRangePickerPreset>
        <DateRangePickerPreset Label="Last 30 Days" Start="@last30DaysStart" End="@currentDate"></DateRangePickerPreset>
        <DateRangePickerPreset Label="This Month" Start="@currentMonthStart" End="@currentDate"></DateRangePickerPreset>
    </DateRangePickerPresets>
</SfDateRangePicker>

@code {
    private DateTime currentDate = DateTime.Now;
    private DateTime last7DaysStart = DateTime.Now.AddDays(-7);
    private DateTime last30DaysStart = DateTime.Now.AddDays(-30);
    private DateTime currentMonthStart = new DateTime(DateTime.Now.Year, DateTime.Now.Month, 1);
}
```

### Key Properties

| Property | Type | Purpose |
|----------|------|---------|
| `StartDate` | `TValue` | Start date of the range (one-way binding) |
| `@bind-StartDate` | `TValue` | Two-way binding for start date |
| `EndDate` | `TValue` | End date of the range (one-way binding) |
| `@bind-EndDate` | `TValue` | Two-way binding for end date |
| `Value` | `Object` | Combined range value (legacy) |
| `Min` | `DateTime` | Earliest selectable date |
| `Max` | `DateTime` | Latest selectable date |
| `MinDays` | `int?` | Minimum required days in range |
| `MaxDays` | `int?` | Maximum allowed days in range |
| `Presets` | `List<Presets>` | Predefined date range presets |
| `Separator` | `string` | Separator between start and end dates (default: "-") |
| `Format` | `string` | Display format pattern |
| `Placeholder` | `string` | Input placeholder text |
| `AllowEdit` | `bool` | Allow manual text input editing |
| `StrictMode` | `bool` | Restrict to valid dates only |
| `OpenOnFocus` | `bool` | Open popup when input is focused |
| `FloatLabelType` | `FloatLabelType` | Float label behavior |
| `Enabled` | `bool` | Enable/disable component |
| `Readonly` | `bool` | Read-only mode |
| `ShowClearButton` | `bool` | Show clear button |
| `WeekNumber` | `bool` | Display week numbers |
| `FirstDayOfWeek` | `int` | First day (0=Sunday, 1=Monday) |
| `ZIndex` | `int` | Z-index for popup |
| `Width` | `string` | Component width |
| `CssClass` | `string` | Custom CSS class |

### Common Patterns

#### Pattern 1: Range Selection with Validation

```razor
<SfDateRangePicker TValue="DateTime?" 
    @bind-Value="BookingRange" 
    Min="@minBookingDate"
    Placeholder="Select booking dates">
</SfDateRangePicker>

@code {
    private DateTime? BookingRange { get; set; }
    private DateTime minBookingDate = DateTime.Today;
}
```

#### Pattern 2: Event Handling

```razor
<SfDateRangePicker TValue="DateTime?" 
    @bind-StartDate="@StartDate"
    @bind-EndDate="@EndDate">
    <DateRangePickerEvents TValue="DateTime?" ValueChange="@OnDateRangeChange"></DateRangePickerEvents>
</SfDateRangePicker>

@code {
    private DateTime? StartDate { get; set; }
    private DateTime? EndDate { get; set; }
    
    private void OnDateRangeChange(RangePickerEventArgs<DateTime?> args)
    {
        // Handle date range selection
        Console.WriteLine($"Start: {args.StartDate}, End: {args.EndDate}");
    }
}
```

#### Pattern 3: Multiple Instances with Different Constraints

```razor
<div>
    <h4>Q1 Reporting Period (Jan-Mar)</h4>
    <SfDateRangePicker TValue="DateTime?" 
        Min="@new DateTime(2024, 1, 1)" 
        Max="@new DateTime(2024, 3, 31)"
        Placeholder="Select dates">
    </SfDateRangePicker>
</div>

<div>
    <h4>Q2 Reporting Period (Apr-Jun)</h4>
    <SfDateRangePicker TValue="DateTime?" 
        Min="@new DateTime(2024, 4, 1)" 
        Max="@new DateTime(2024, 6, 30)"
        Placeholder="Select dates">
    </SfDateRangePicker>
</div>
```

---

## DateTimePicker

A comprehensive guide for implementing and customizing the DateTimePicker component. The DateTimePicker enables users to select both date and time values with support for multiple formats, masks, globalization, and extensive customization options.

### When to Use This Skill

Use this skill when you need to:
- Install and set up the DateTimePicker component in a Blazor application
- Implement date and time selection functionality
- Format date/time values using custom format strings
- Apply input masks and validate user input
- Handle DateTimePicker events (ValueChange, OnOpen, OnClose, etc.)
- Bind the component to data models
- Set minimum and maximum date/time constraints
- Highlight special dates or disable specific dates
- Implement globalization (RTL, multiple locales)
- Support Islamic calendar or other calendar systems
- Customize component appearance and styling
- Configure placeholder text and input validation
- Enable strict mode for input validation
- Display week numbers in the calendar

### Documentation

#### Getting Started
📄 **Read:** [references/getting-started.md](references/datetimepicker-getting-started.md)
- Installation and NuGet package setup
- Basic component initialization in Blazor Server/Web App
- Minimal working example
- Required imports and configurations

#### Date & Time Formatting
📄 **Read:** [references/date-time-formatting.md](references/datetimepicker-date-time-formatting.md)
- Format string patterns and standard formats
- Custom date/time format configurations
- Locale-specific formatting
- Parse and display format handling

#### Data Binding & Events
📄 **Read:** [references/data-binding-and-events.md](references/datetimepicker-data-binding-and-events.md)
- Two-way binding for date/time values
- ValueChange event (date/time selection complete)
- OnOpen/OnClose events (popup lifecycle)
- Selected event (date selection in calendar)
- OnItemRender event (custom time list item rendering)
- Focus/Blur events
- Cleared event (clear button clicked)
- Created/Destroyed lifecycle events
- Navigated event (calendar view navigation)
- OnRenderDayCell event (custom day cell rendering)
- Using DateTimePickerEvents child component
- State management and data flow patterns

#### Date Ranges & Special Dates
📄 **Read:** [references/date-ranges-and-special-dates.md](references/datetimepicker-date-ranges-and-special-dates.md)
- Setting minimum and maximum date constraints
- Restricting date range selection
- Highlighting special dates
- Disabling specific dates or date ranges
- Week number configuration

#### Masking & Input Validation
📄 **Read:** [references/masking-and-input-validation.md](references/datetimepicker-masking-and-input-validation.md)
- Input mask patterns for date/time entry
- Placeholder text configuration
- Input validation rules
- Strict mode enforcement and input behavior
- Validation edge cases

#### Customization & Styling
📄 **Read:** [references/customization-and-styling.md](references/datetimepicker-customization-and-styling.md)
- CSS customization and class overrides
- Theme integration (Bootstrap, Material, Fluent, Tailwind)
- Component appearance and styling options
- Custom CSS properties and styling patterns
- Visual customization examples

#### Globalization & Accessibility
📄 **Read:** [references/globalization-and-accessibility.md](references/datetimepicker-globalization-and-accessibility.md)
- Globalization support (RTL, multiple locales)
- Islamic calendar implementation and configuration
- Accessibility features (WCAG compliance, keyboard navigation)
- ARIA attributes and screen reader support
- Internationalization best practices

### Quick Start

Here's a minimal working example to get started with the DateTimePicker:

```blazor
@page "/datetimepicker-demo"
@using Syncfusion.Blazor.Calendars

<div>
    <label>Select Date and Time:</label>
    <SfDateTimePicker TValue="DateTime?" 
                      @bind-Value="@selectedDateTime"
                      Placeholder="Choose date and time">
        <DateTimePickerEvents TValue="DateTime?" ValueChange="@OnValueChanged"></DateTimePickerEvents>
    </SfDateTimePicker>
    
    @if (selectedDateTime.HasValue)
    {
        <p>Selected: @selectedDateTime.Value.ToString("g")</p>
    }
</div>

@code {
    private DateTime? selectedDateTime;
    
    private void OnValueChanged(ChangedEventArgs<DateTime?> args)
    {
        selectedDateTime = args.Value;
        Console.WriteLine($"Date changed to: {selectedDateTime}");
    }
}
```

### Common Patterns

#### Pattern 1: With Date Range Constraints
```blazor
<SfDateTimePicker TValue="DateTime?" 
                  Min="@minDate" 
                  Max="@maxDate"
                  Value="selectedDateTime">
</SfDateTimePicker>

@code {
    private DateTime minDate = new DateTime(2024, 1, 1);
    private DateTime maxDate = new DateTime(2024, 12, 31);
    private DateTime? selectedDateTime;
}
```

#### Pattern 2: With Format Customization
```blazor
<SfDateTimePicker TValue="DateTime?" 
                  Format="dd/MM/yyyy HH:mm"
                  Value="selectedDateTime">
</SfDateTimePicker>
```

#### Pattern 3: With Input Mask
```blazor
<SfDateTimePicker TValue="DateTime?" 
                  EnableMask="true"
                  Format="dd/MM/yyyy HH:mm"
                  Value="selectedDateTime">
    <DateTimePickerMaskPlaceholder Day="dd" Month="MM" Year="yyyy" Hour="HH" Minute="mm"></DateTimePickerMaskPlaceholder>
</SfDateTimePicker>
```

#### Pattern 4: With Data Binding
```blazor
<SfDateTimePicker TValue="DateTime?" 
                  @bind-Value="model.CreatedDate">
    <DateTimePickerEvents TValue="DateTime?" ValueChange="@OnDateChanged"></DateTimePickerEvents>
</SfDateTimePicker>
```

### Key Properties

| Property | Type | Purpose |
|----------|------|---------|
| `Value` | `TValue` | Gets or sets the selected date-time value |
| `@bind-Value` | `TValue` | Two-way data binding |
| `Format` | `string` | Display format (e.g., "dd/MM/yyyy HH:mm") |
| `TimeFormat` | `string` | Format for time portion only (e.g., "HH:mm") |
| `Placeholder` | `string` | Placeholder text for the input field |
| `Min` | `DateTime` | Minimum selectable date (date portion) |
| `Max` | `DateTime` | Maximum selectable date (date portion) |
| `MinTime` | `DateTime` | Minimum selectable time |
| `MaxTime` | `DateTime` | Maximum selectable time |
| `Step` | `int` | Time interval step in minutes (default: 30) |
| `ScrollTo` | `DateTime?` | Initial scroll position in time list |
| `AllowEdit` | `bool` | Allows manual text input |
| `StrictMode` | `bool` | Enforces strict input validation |
| `EnableMask` | `bool` | Enable input masking for date/time entry |
| `OpenOnFocus` | `bool` | Opens popup on input focus |
| `FloatLabelType` | `FloatLabelType` | Float label behavior |
| `Enabled` | `bool` | Enables/disables the component |
| `Readonly` | `bool` | Makes the component read-only |
| `ShowClearButton` | `bool` | Show clear button |
| `ShowTodayButton` | `bool` | Displays today button in calendar |
| `WeekNumber` | `bool` | Display week numbers |
| `FirstDayOfWeek` | `int` | First day (0=Sunday, 1=Monday) |
| `Start` | `CalendarView` | Initial calendar view |
| `Depth` | `CalendarView` | Deepest view level |
| `ZIndex` | `int` | Z-index for popup |
| `Width` | `string` | Component width |
| `CssClass` | `string` | Custom CSS class |

---

## TimePicker

A comprehensive skill for implementing and customizing the **TimePicker** component. This skill covers installation, configuration, data binding, events, styling, accessibility, internationalization, and advanced features like input masking.

### When to Use This Skill

Use this skill when you need to:
- Install and set up the TimePicker component in Blazor applications
- Create basic and advanced TimePicker implementations
- Configure time formats (12-hour, 24-hour, custom formats)
- Set up data binding (one-way, two-way, dynamic)
- Handle TimePicker events (ValueChange, OnOpen, OnClose, Blur, Focus, etc.)
- Customize appearance and styling with CSS
- Implement input masking and validation
- Configure accessibility (keyboard navigation, ARIA attributes, screen readers)
- Add globalization support (localization, RTL for Arabic/Hebrew)
- Set time intervals and step values
- Implement full-screen mode on mobile devices

### Component Overview

The **SfTimePicker** is a Blazor input component for time selection. It provides:
- Popup time picker interface with scrollable time list
- Configurable time formats and intervals
- Two-way data binding support
- Built-in validation and masking
- Full accessibility compliance (WCAG 2.2, Section 508)
- RTL support for international audiences
- Mobile-responsive with full-screen mode
- Keyboard navigation shortcuts

### Quick Start Example

#### Basic TimePicker

```razor
@using Syncfusion.Blazor.Calendars

<SfTimePicker TValue="DateTime?" Placeholder="Select a time"></SfTimePicker>
```

#### TimePicker with Value Binding

```razor
@using Syncfusion.Blazor.Calendars

<p>Selected Time: @SelectedTime</p>

<SfTimePicker TValue="DateTime?" @bind-Value="@SelectedTime"></SfTimePicker>

@code {
    public DateTime? SelectedTime { get; set; } = DateTime.Now;
}
```

#### TimePicker with Format and Step

```razor
@using Syncfusion.Blazor.Calendars

<SfTimePicker TValue="DateTime?" 
    Value="@TimeValue" 
    Format="HH:mm"
    Step=60
    Placeholder="Select time (24-hour)">
</SfTimePicker>

@code {
    public DateTime TimeValue { get; set; } = DateTime.Now;
}
```

### Key Props Reference

### Key Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `Value` | `TValue` | Current time value (one-way binding) | null |
| `@bind-Value` | `TValue` | Two-way data binding | - |
| `Placeholder` | `string` | Placeholder text in input | "Select a time" |
| `Format` | `string` | Display format string (HH:mm, hh:mm tt) | Culture-based |
| `InputFormats` | `string[]` | Accepted input format patterns | Culture-based |
| `Step` | `int` | Time interval in minutes | 30 |
| `ScrollTo` | `DateTime?` | Initial scroll position in time list | null |
| `Min` | `DateTime` | Minimum selectable time | 00:00 |
| `Max` | `DateTime` | Maximum selectable time | 23:59 |
| `AllowEdit` | `bool` | Allow manual text input | true |
| `StrictMode` | `bool` | Enforce strict input validation | false |
| `EnableMask` | `bool` | Enable input masking | false |
| `OpenOnFocus` | `bool` | Open popup when input is focused | false |
| `FloatLabelType` | `FloatLabelType` | Float label behavior | Never |
| `Enabled` | `bool` | Enable/disable component | true |
| `Readonly` | `bool` | Read-only mode | false |
| `ShowClearButton` | `bool` | Show clear button | false |
| `EnableRtl` | `bool` | Enable right-to-left (RTL) direction | false |
| `FullScreen` | `bool` | Full-screen mode on mobile | false |
| `ZIndex` | `int` | Z-index for popup | 1000 |
| `Width` | `string` | Component width | "100%" |
| `CssClass` | `string` | Custom CSS class | null |
| `TabIndex` | `int` | Tab order index | 0 |

### Documentation Navigation

#### Getting Started
📄 **Read:** [references/getting-started.md](references/timepicker-getting-started.md)
- NuGet package installation
- Namespace imports
- Service registration in Program.cs
- Theme setup and inclusion
- Basic component implementation
- Step property and time intervals

#### Time Formats
📄 **Read:** [references/time-formats.md](references/timepicker-time-formats.md)
- Display format customization
- Input format configuration
- Standard vs custom format strings
- Culture-based default formats
- 12-hour and 24-hour format examples
- When to use each format option

#### Data Binding
📄 **Read:** [references/data-binding.md](references/timepicker-data-binding.md)
- One-way binding patterns
- Two-way binding with @bind-Value
- Dynamic value updates
- DateTime and DateTime? type handling
- Value change event integration

#### Events and Handlers
📄 **Read:** [references/events-and-handlers.md](references/timepicker-events-and-handlers.md)
- ValueChange event (EventCallback<ChangeEventArgs<TValue>>)
- OnOpen/OnClose popup events
- Selected event (time selection in list)
- OnItemRender event (custom time list item rendering)
- Focus/Blur input events
- Cleared event (clear button clicked)
- Created/Destroyed lifecycle events
- Using TimePickerEvents child component
- Event handler implementation patterns

#### Styling and Appearance
📄 **Read:** [references/styling-and-appearance.md](references/timepicker-styling-and-appearance.md)
- CSS classes for customization
- Container element styling
- Icon element styling
- Popup and list item styling
- Full-screen mode for mobile devices
- Theme integration (Bootstrap, Material, Fluent)

#### Accessibility and Globalization
📄 **Read:** [references/accessibility-and-globalization.md](references/timepicker-accessibility-and-globalization.md)
- WCAG 2.2 compliance and standards
- Keyboard navigation shortcuts (arrow keys, Enter, Esc)
- WAI-ARIA attributes and roles
- RTL (right-to-left) support
- Localization and culture configuration
- Screen reader support

#### Mask Support
📄 **Read:** [references/mask-support.md](references/timepicker-mask-support.md)
- EnableMask property for input masking
- TimePickerMaskPlaceholder directive
- Custom placeholder characters (Hour, Minute, Second)
- Format-specific masking patterns
- Input validation with masks

### Common Patterns

#### Pattern 1: Simple Time Selection

When users need a basic time picker without complex requirements:

```razor
<SfTimePicker TValue="DateTime?" 
    @bind-Value="@AppointmentTime"
    Placeholder="Select appointment time">
</SfTimePicker>
```

#### Pattern 2: Business Hours Only

When time selection should be limited to specific hours:

```razor
<SfTimePicker TValue="DateTime?" 
    @bind-Value="@BusinessTime"
    Min="@new DateTime(2026, 1, 1, 09, 0, 0)"
    Max="@new DateTime(2026, 1, 1, 17, 0, 0)"
    Step=30
    Format="HH:mm">
</SfTimePicker>

@code {
    public DateTime? BusinessTime { get; set; }
}
```

#### Pattern 3: 12-Hour Format with AM/PM

When displaying time in 12-hour format:

```razor
<SfTimePicker TValue="DateTime?" 
    @bind-Value="@UserTime"
    Format="hh:mm tt"
    Placeholder="Select time (12-hour)">
</SfTimePicker>

@code {
    public DateTime? UserTime { get; set; } = DateTime.Now;
}
```

#### Pattern 4: With Event Handling

When tracking time selection changes:

```razor
<SfTimePicker TValue="DateTime?">
    <TimePickerEvents TValue="DateTime?" ValueChange="@OnTimeChanged"></TimePickerEvents>
</SfTimePicker>

@code {
    private void OnTimeChanged(ChangeEventArgs<DateTime?> args)
    {
        Console.WriteLine($"Time selected: {args.Value}");
    }
}
```

### Common Use Cases

1. **Appointment Booking**: Time selection for scheduling appointments with business hours validation
2. **Time Tracking**: Recording start/end times for timesheets or activity logs
3. **Shift Management**: Selecting shift times in HR/workforce applications
4. **Travel Booking**: Departure/arrival time selection for flights, trains
5. **Delivery Windows**: Setting preferred delivery time slots in e-commerce
6. **Broadcast Scheduling**: Selecting broadcast times for media applications
7. **Meeting Scheduler**: Time selection for virtual meetings with timezone support
8. **Gym Booking**: Class time selection in fitness management apps

### Prerequisites

- Blazor WebAssembly or Blazor Server project (.NET 7 or later)
- NuGet package: `Syncfusion.Blazor.Calendars`
- NuGet package: `Syncfusion.Blazor.Themes`
- Syncfusion service registered in `Program.cs`

---

**Ready to implement Calendar components?** Start with Getting Started guides above for setup, then navigate to other references based on your specific needs.

---

**Next Steps:** Start with the Getting Started guide for your chosen component, then navigate to other references based on your specific task.

For detailed implementation patterns, complete code examples, and troubleshooting guidance, read the appropriate reference file based on your specific needs.

````