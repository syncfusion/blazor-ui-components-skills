---
name: syncfusion-blazor-scheduler
description: Implement Syncfusion Blazor Scheduler component for comprehensive appointment and event scheduling. Use this when building calendar interfaces, managing appointments with CRUD operations, implementing multiple view modes, setting up recurring events, or handling timezone-aware scheduling. This skill covers views (day, week, month, timeline, agenda), drag-and-drop functionality, resizing, resources, localization, data binding, state persistence, and advanced customization options for scheduling applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Scheduler Components"
---

# Syncfusion Blazor Scheduler Component

## When to Use This Skill

**Use this skill whenever the user needs to:**
- Create or configure a Blazor Scheduler component
- Bind local or remote data
- Manage appointments, events, or calendar data
- Set up different calendar views (Day, Week, Month, Timeline, Agenda)
- Implement recurring events or recurrence patterns
- Handle appointment CRUD operations (create, read, update, delete)
- Configure appointment drag-and-drop or resizing
- Work with resources or grouping
- Set up timezones or localization
- Customize Scheduler appearance or behavior
- Handle events and callbacks
- Persist state or manage data binding
- Enable accessibility features
- Export or print scheduler data
- Set up virtual scrolling for large datasets
- Use clipboard copy, cut, or Paste features
- Configure custom toolbar

## Component Overview

The Syncfusion Blazor Scheduler is a comprehensive calendar component that allows you to:
- Display appointments in multiple views (Day, Week, Month, Timeline, Agenda, Year)
- Manage appointments with full CRUD operations and complete control
- Support recurring events with iCalendar-compatible recurrence rules
- Enable drag-and-drop and resizing functionality for intuitive scheduling
- Handle multiple resources and flexible grouping models
- Support timezone-aware appointments with IANA timezone conversion
- Customize appointment appearance with templates, EventRendered events, and CSS classes
- Prevent or customize overlapping appointments (AllowOverlap property)
- Configure blocking time slots and read-only appointments
- Display interactive tooltips with custom templates
- Persist state and manage local/remote data binding
- Filter, retrieve, and refresh appointments dynamically
- Provide keyboard navigation, accessibility (WCAG 2.2), and RTL support

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation via NuGet packages
- Setting up Blazor project
- Adding required namespaces and services
- Basic component initialization
- First minimal working example

### Appointments and Event Fields
📄 **Read:** [references/appointments.md](references/appointments.md)
- Built-in appointment fields (Id, Subject, StartTime, EndTime, Location, Description, Timezone, etc.)
- Custom field mapping, binding, and validation
- Event field settings with labels, defaults, and validation rules
- Adding custom fields to appointments
- Normal, spanned, all-day, and blocked events
- Read-only appointments and slot availability
- Overlapping event customization and prevention (AllowOverlap)
- Appointment appearance with EventRendered styling
- Tooltips (built-in, custom templates, conditional)
- Appointment selection and retrieval (GetEventsAsync, GetCurrentViewEvents)
- Filtering appointments with Query
- Refresh appointments with RefreshEventsAsync

### Appointment Customization
📄 **Read:** [references/appointment-customization.md](references/appointment-customization.md)
- Template-based event customization
- EventRendered event for styling
- CssClass property for bulk styling
- Custom attributes and classes
- Event appearance modifications

### Drag and Drop
📄 **Read:** [references/appointment-drag-and-drop.md](references/appointment-drag-and-drop.md)
- Enable/disable drag-and-drop functionality
- Single and multi-drag operations
- Drag event handling and prevention
- Scroll control during drag
- Navigation trigger on edges
- Drag intervals and external sources
- Opening editor on drag completion

### Resizing Events
📄 **Read:** [references/appointment-resizing.md](references/appointment-resizing.md)
- Enable/disable resizing functionality
- Scroll control during resize
- Scroll speed configuration
- Resize time intervals
- Preventing specific resizing actions

### Scheduler Views
📄 **Read:** [references/views.md](references/views.md)
- 12 available view modes (Day, Week, WorkWeek, Month, Agenda, MonthAgenda, TimelineDay, TimelineWeek, TimelineWorkWeek, TimelineMonth, TimelineYear, Year)
- Setting current/active view with @bind-CurrentView property
- View-specific configuration properties table
- ScheduleView component properties (Option, IsSelected, DateFormat, Readonly, ShowWeekend, WorkDays, etc.)
- Day view with hourly time slots and single-day display
- Day view with Interval property for multiple consecutive days (2-day, 3-day views)
- Week view displaying 7-day week (Sunday-Saturday) with all appointments
- Week view with FirstDayOfWeek customization (1=Monday, 0=Sunday)
- Work Week view showing only working days (Mon-Fri by default)
- Custom working days per view with WorkDays int array (0=Sun, 1=Mon, etc.)
- Month view with full month calendar grid layout
- Month view with MaxEventsPerRow property to limit/expand events display
- Agenda view displaying upcoming events in chronological list format (7 days default)
- Agenda view with AgendaDaysCount property for custom date range
- Agenda view with AllowVirtualScrolling for large datasets
- Month Agenda view combining month calendar with agenda list below
- Timeline Day view showing single day in horizontal layout
- Timeline Week view showing 7-day week horizontally
- Timeline Work Week view showing working days horizontally
- Timeline Month view showing entire month horizontally
- Timeline Year view showing full year horizontally with Orientation customization
- Year view displaying mini-calendar with month selection and appointment indicators
- Extending view intervals with Interval property (3-day, 2-week, quarterly, annual views)
- Display Name customization for extended views with DisplayName property
- Setting specific start and end hours with StartHour/EndHour (e.g., "09:00"-"18:00")
- Time range limiting to business hours only
- Hide weekends in calendar views using ShowWeekend="false"
- Display ISO week numbers with ShowWeekNumber="true"
- Readonly mode per view using Readonly="true" to prevent CRUD operations
- View-specific templates (EventTemplate, DateHeaderTemplate, CellTemplate, etc.)
- Event template customization per view for different rendering
- Date header template customization per view
- Cell templates and custom cell content per view
- DateFormat customization per view for date display
- Switching views programmatically with CurrentView property binding
- View navigation via header toolbar with built-in view buttons
- Resource header templates per view for grouped schedules
- Resource-based view configuration and display options

### Events and Interactions
📄 **Read:** [references/events.md](references/events.md)
- All Scheduler events (ActionBegin, ActionCompleted, Created, Destroyed, etc.)
- Event arguments and action types
- Two-way binding and callbacks
- DataBinding and DataBound events
- Event selection and retrieval

### Recurring Events
📄 **Read:** [references/recurring-events.md](references/recurring-events.md)
- Recurrence rule syntax and iCalendar format
- FREQ, INTERVAL, COUNT, UNTIL properties
- BYDAY, BYMONTHDAY, BYMONTH configurations
- Creating recurring events
- Exception dates and editing occurrences
- Edit/delete following events

### CRUD Operations
📄 **Read:** [references/crud-actions.md](references/crud-actions.md)
- Create appointments via editor or AddEventAsync
- Inline appointment creation
- Update appointments with proper validation
- Delete single or multiple appointments
- Server-side database operations
- Restricting actions based on criteria

### Data Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Local data binding with List collections
- Remote data binding with HTTP services
- DataSource configuration
- Query-based filtering
- Refresh appointments dynamically
- DataSource property and event settings
- Load data on demand
- Set custom headers for authentication/authorization

### Customization and Styling
📄 **Read:** [references/scheduler-styling.md](references/scheduler-styling.md)
- CSS customization and theming
- Custom styling with CssClass
- Style overrides for components
- Color schemes and themes
- Responsive design adjustments

### Cell Customization
📄 **Read:** [references/cell-customization.md](references/cell-customization.md)
- Cell dimensions in vertical and timeline views
- CellTemplate for custom content
- CellHeaderTemplate for headers
- OnRenderCell event for dynamic styling
- Weekend and specific date styling
- MinDate and MaxDate restrictions
- Multi-cell and multi-row selection

### Header Bar and Toolbar Customization
📄 **Read:** [references/header-bar.md](references/header-bar.md)
- ShowHeaderBar property to display or hide header
- ScheduleToolBar component for toolbar configuration
- Built-in toolbar items (ScheduleToolBarPrevious, Next, DateRange, Today, Views, NewEvent)
- ScheduleToolBarCustom for adding custom elements to toolbar
- ItemType options for custom items (Button, Input, Spacer, Separator)
- Creating custom toolbar with buttons for export, print, and other actions
- Adding dropdown controls to toolbar for filtering by resources
- Real-world toolbar configuration example with owner resource filtering
- EnableAdaptiveUI property for responsive/mobile-friendly layout
- Toolbar consolidation and "New Event" button in adaptive mode
- DateHeaderTemplate for customizing date cells in day/week views
- OnRenderCell event with ElementType.DateHeader for dynamic date header styling
- Weekend and special date styling in date header
- DateRangeTemplate for customizing displayed date range text
- TimelineYear view header customization with DayHeaderTemplate
- MonthHeaderTemplate for month headers in TimelineYear
- HeaderIndentTemplate for customizing left indent area in timeline views
- Vertical and horizontal orientation in timeline views
- Resource-grouped view headers
- Combining multiple templates for advanced header layouts

### Header Rows
📄 **Read:** [references/header-rows.md](references/header-rows.md)
- ScheduleHeaderRow component structure and properties
- HeaderRowType enum options (Year, Month, Week, Date, Hour)
- View compatibility for each header row type
- Basic multi-level header row setup
- Year and month header rows for long-range timelines
- Week number header rows for weekly organization
- Displaying complete year timeline with Interval=12
- Day and hour header rows for detailed scheduling
- Customizing header rows with Template property
- Template context and accessing Date in templates
- Header row styling with CSS classes
- Color customization for different header levels
- Header rows with resource-grouped views
- Combining multiple header rows (2-5 levels)
- Order and hierarchy guidelines for header rows
- Localization in header rows by culture
- ISO week number calculations and display
- Common patterns (quarterly, weekly, daily, resource views)
- Performance considerations with multiple rows

### Editor Template
📄 **Read:** [references/editor-template.md](references/editor-template.md)
- Default editor window and double-click behavior
- Customizing editor field labels with Title property
- Field validation using ValidationRules (Required, RegexPattern, MinLength, MaxLength)
- Editing default time duration with OnPopupOpen event
- Preventing editor/quick popups by canceling PopupOpenEventArgs
- Popup types (Editor, QuickInfo, EditEventInfo, ViewEventInfo, EventContainer, etc.)
- Opening editor window manually with OpenEditorAsync method
- Creating completely custom editor using EditorTemplate
- Customizing editor header and footer with EditorHeaderTemplate/EditorFooterTemplate
- Adding resource dropdown options to editor template
- Adding recurrence pattern fields to editor template
- Applying DataAnnotations validation in editor templates
- Quick info popups on single-click (cell and event)
- Customizing quick popup appearance with ScheduleQuickInfoTemplates
- Quick popup template types (Cell, Event, Both) for different popup scenarios
- Custom header, content, footer for quick popups
- Disabling quick popups with ShowQuickInfo or OnPopupOpen event
- Multiple cell selection and QuickInfoOnSelectionEnd property
- AllowMultiRowSelection for multi-day selection behavior
- More events indicator popup when cell exceeds capacity
- Preventing more events popup with MoreEventsClicked event
- CloseQuickInfoPopupAsync method to programmatically close popups

### Clipboard Operations
📄 **Read:** [references/clipboard.md](references/clipboard.md)
- Copy/paste appointments
- Copy event data to clipboard
- Paste handling and validation
- Multi-appointment operations

### Resources and Grouping
📄 **Read:** [references/resources.md](references/resources.md)
- ScheduleResource component structure and generic parameters
- Resource field mapping (Field, TextField, IdField, ColorField, etc.)
- Complete resource fields reference table
- Local resource data binding with List collections
- Binding resources with ExpandoObject for dynamic models
- Binding resources with DynamicObject custom implementation
- Binding resources with ObservableCollection for dynamic updates
- Multiple resources without visual grouping (AllowMultiple property)
- Single-level resource grouping with ScheduleGroup
- Multi-level hierarchical resource grouping (GroupIDField)
- One-to-One grouping with ByGroupID="false"
- Grouping resources by date (ByDate="true") in calendar views
- Shared events across resources (AllowGroupEdit="true")
- Simple resource header customization with templates
- Multiple column resource headers for timeline views
- Expand and collapse resources with ExpandedField
- Resource header tooltips with HeaderTooltipTemplate
- Resource color selection with ResourceColorField
- Custom working hours per resource (StartHourField, EndHourField)
- Custom working days per resource (WorkDaysField)

### Timescale Configuration
📄 **Read:** [references/timescale.md](references/timescale.md)
- Timescale slots and intervals
- Major and minor timescale settings
- Custom timescale format
- Time display options
- Work hours configuration

### Working Hours
📄 **Read:** [references/working-hours.md](references/working-hours.md)
- Setting working hours range
- Work hour highlighting
- Highlight color and styling
- Business hours for resources
- Non-working hour customization

### Timezones
📄 **Read:** [references/timezone.md](references/timezone.md)
- IANA timezone support
- StartTimezone and EndTimezone fields
- Timezone conversion
- Daylight saving time handling
- Timezone-aware scheduling

### Localization
📄 **Read:** [references/localization.md](references/localization.md)
- Setting date format (DateFormat property)
- Time mode configuration (TimeFormat property)
- Right-to-left (RTL) support (EnableRtl property)

### Virtual Scrolling
📄 **Read:** [references/virtual-scrolling.md](references/virtual-scrolling.md)
- Virtual scrolling overview and when to use (100+ resources, 5,000+ appointments)
- Enable AllowVirtualScrolling in timeline views (TimelineDay, TimelineWeek, TimelineWorkWeek, TimelineMonth, TimelineYear)
- Virtual scrolling in Agenda view for large upcoming event lists
- Virtual scrolling in Timeline Month view with resource grouping
- Virtual scrolling benefits (reduced memory, smooth scrolling, DOM optimization)
- VirtualResourceCount property to control visible resources at once (default 30)
- VirtualResourceCount configuration for different screen sizes (20-50 standard, 100+ for large displays)
- Virtual scrolling with 300+ resources and sparse appointments
- Virtual scrolling with resource grouping and hierarchical organization
- Virtual scrolling combined with custom templates (ResourceHeaderTemplate, EventTemplate)
- Resource header templates with virtual scrolling for metadata display
- Event templates with virtual scrolling for custom event rendering
- Lazy loading appointments with EnableLazyLoading="true" property
- Lazy loading on-demand approach for viewport-based data retrieval
- Lazy loading with server-side appointment filtering by ResourceId
- Server-side controller implementation for lazy loading (ResourceId query params)
- Lazy loading parameters (StartDate, EndDate, ResourceId filtering)
- Virtual scrolling with DataManager for remote data binding
- Virtual scrolling and lazy loading combined for massive datasets
- Performance optimization with virtual scrolling (VirtualResourceCount tuning)
- Performance optimization with sparse data (not all resources have events daily)
- Performance optimization with EnableMaxHeight, AllowKeyboard settings
- CSS optimization for virtual scrolling (will-change property, min-height)
- Virtual scrolling limitations (not in Month/Year/MonthAgenda views)
- Virtual scrolling limitations (limited support in TimelineYear Vertical Orientation)
- When virtual scrolling is NOT beneficial (small datasets <100 resources)
- Combining virtual scrolling with MaxEventsPerRow for cell optimization
- Virtual scrolling with scroll-on-demand and automatic resource loading
- Memory management in virtual scrolling scenarios
- Testing virtual scrolling performance with actual large datasets

### State Persistence
📄 **Read:** [references/state-persistence.md](references/state-persistence.md)
- Persisted properties (view, date)
- Local storage integration

### Exporting and Importing
📄 **Read:** [references/exporting.md](references/exporting.md)
- Excel export to XLSX/CSV using ExportToExcelAsync
- Exporting with custom fields and field filtering
- Exporting recurring event occurrences as individual records (IncludeOccurrences)
- Exporting custom event data subset with CustomData option
- Customizing column headers with ExportFieldInfo
- Export with custom filename (FileName property)
- Excel file formats (XLSX vs CSV via ExportType)
- Export with specific date format (DateFormat option)
- Customize Excel sheet before export with OnExcelExporting event
- ExcelExport API for cell formatting, headers, styles
- ICS calendar export to .ics format (Outlook, Google Calendar compatible)
- ICS export with custom filename
- Importing events from external ICS files with ImportICalendarAsync
- File uploader integration for importing ICS files
- Merging imported events with existing appointments
- PrintAsync method without options (default print)
- PrintAsync method with PrintOptions (custom width/height)
- Print current view with all visible appointments

### Dimensions
📄 **Read:** [references/dimensions.md](references/dimensions.md)
- Height and width configuration
- Responsive sizing
- Auto-height for rows
- Cell and column dimensions
- Container constraints

### Row Auto Height
📄 **Read:** [references/row-auto-height.md](references/row-auto-height.md)
- Enable/disable auto height
- Row height calculation
- Content overflow handling
- Responsive row sizing

### Performance and WebAssembly
📄 **Read:** [references/webassembly-performance.md](references/webassembly-performance.md)
- Avoid unnecessary component renders using `@key` directive
- Use PreventRender to reduce re-renders after scheduler events

### Accessibility
📄 **Read:** [references/accessibility.md](references/accessibility.md)
- WCAG 2.2 Level A & AA compliance
- WAI-ARIA attributes and roles
- Keyboard navigation shortcuts

---

## Quick Start Example

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
        <ScheduleView Option="View.Agenda"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code{
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Conference", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0),
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0)
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public string Location { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public string Description { get; set; }
        public bool IsAllDay { get; set; }
        public string RecurrenceRule { get; set; }
        public string RecurrenceException { get; set; }
        public Nullable<int> RecurrenceID { get; set; }
    }
}
```

---

## Common Patterns

### 1. **CRUD Operations**
- **Create:** Use `AddEventAsync()` method or double-click cells for editor
- **Read:** Use `GetEventsAsync()` or `GetCurrentViewEvents()` or `GetBlockEventsAsync()`
- **Update:** Modify appointment and use `SaveEventAsync()`
- **Delete:** Use `DeleteEventAsync()` method or delete key for multi-select
- **Check availability:** Use `IsSlotAvailableAsync()` to prevent overlaps

### 2. **Appointment Management**
- **Readonly appointments:** Set `IsReadonly = true` on individual events or use `Readonly` property
- **Block time slots:** Set `IsBlock = true` to prevent scheduling on those slots
- **Overlapping control:** Use `AllowOverlap="false"` to prevent overlaps
- **Custom fields:** Add custom properties beyond built-in fields (Status, Priority, etc.)
- **Field mapping:** Use `ScheduleField` tags for custom field names

### 3. **Appearance Customization**
- **Templates:** Use `<Template>` for custom appointment HTML
- **EventRendered:** Apply dynamic CSS classes based on data
- **CssClass:** Apply predefined CSS classes from appointment data
- **Tooltips:** Enable with `EnableTooltip="true"` and customize with `TooltipTemplate`
- **Cell styling:** Use `OnRenderCell` event or `CellTemplate`

### 4. **Event Handling**
- **ActionBegin:** Intercept before operations (validate, cancel, check overlap)
- **ActionCompleted:** Execute after successful operations
- **EventRendered:** Customize appointment appearance
- **OnDragStart / OnResizeStart:** Control drag/resize behavior
- **TooltipOpening:** Conditionally show/hide tooltips

### 5. **View Management**
- Set `CurrentView` property to change active view
- Configure each view independently with view-specific properties
- Use `DisplayName` for custom view labels
- Extend views with `Interval` property (e.g., 3-day view)

### 6. **Recurring Events**
- Use standard iCalendar `RecurrenceRule` format (FREQ, INTERVAL, COUNT, UNTIL)
- Add `RecurrenceException` dates to exclude occurrences
- Use `RecurrenceID` for edited occurrences
- Support daily, weekly, monthly, yearly patterns with FREQ property

### 7. **Data Binding**
- Local: Bind `DataSource` to List<AppointmentData>
- Remote: Bind to HTTP service returning data
- Use `Query` property for server-side filtering
- Call `RefreshEventsAsync()` to reload data without full refresh

---

## Key Props and Configuration

| Property | Purpose | Example |
|----------|---------|---------|
| `Height` | Scheduler height in pixels/percentage | `Height="650px"` |
| `Width` | Scheduler width in pixels/percentage | `Width="100%"` |
| `CurrentView` | Active view (Day, Week, Month, etc.) | `CurrentView="View.Week"` |
| `SelectedDate` | Currently displayed date | `@bind-SelectedDate="@date"` |
| `AllowDragAndDrop` | Enable/disable drag functionality | `AllowDragAndDrop="true"` |
| `AllowResizing` | Enable/disable resize functionality | `AllowResizing="true"` |
| `AllowMultiDrag` | Allow dragging multiple appointments | `AllowMultiDrag="true"` |
| `AllowOverlap` | Prevent overlapping appointments | `AllowOverlap="false"` |
| `AllowInline` | Quick inline creation on cell click | `AllowInline="true"` |
| `Readonly` | View-only mode (no CRUD) | `Readonly="true"` |
| `MinDate` | Minimum date boundary | `MinDate="new DateTime(2026,1,1)"` |
| `MaxDate` | Maximum date boundary | `MaxDate="new DateTime(2026,12,31)"` |
| `CssClass` | Custom CSS class for styling | `CssClass="custom-scheduler"` |
| `EnableMaxHeight` | Appointments occupy full cell height | `EnableMaxHeight="true"` |
| `EnableIndicator` | Show indicator for multiple appointments | `EnableIndicator="true"` |
| `EnableAutoRowHeight` | Auto-adjust row heights for content | `EnableAutoRowHeight="true"` |

---

## Common Use Cases

1. **Employee Scheduling** - Multiple resources with drag-drop and approval workflow
2. **Meeting Rooms Reservation** - Resource-based booking with timezone support
3. **Doctor's Appointments** - Recurring slots with patient blocking
4. **Project Task Management** - Timeline views with progress tracking
5. **Educational Timetable** - Multiple classes with resource grouping
6. **Shift Management** - Working hours, shift patterns with recurrence
7. **Event Calendar** - Public events with recurrence and filtering
8. **Field Service Dispatch** - Mobile-friendly with offline support

---

**Next Steps:** Read the appropriate reference file based on your use case to understand specific features and implementation details.
