---
name: syncfusion-blazor-gantt-chart
description: Implements Syncfusion Blazor Gantt for project scheduling. Covers SfGantt, GanttTaskFields, GanttColumns, GanttEditSettings, taskbar editing, dependencies, resources, timeline, critical path, baselines, and Excel/PDF export. Applicable for Blazor Gantt charts, task scheduling, WBS, and Syncfusion.Blazor.Gantt integration.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion Blazor Gantt Chart

## When to Use This Skill

Use this skill when the user needs to:

- **Implement project scheduling interfaces** similar to Microsoft Project with visual timeline and task management
- **Display hierarchical task structures** with parent-child relationships, dependencies, and progress tracking
- **Enable interactive timeline editing** through drag-and-drop taskbars, resize operations, and dependency linking
- **Manage project resources** including personnel allocation, workload tracking, and overallocation detection
- **Configure complex timelines** with custom zoom levels, working hours, holidays, time zones, and duration units
- **Export project data** to Excel, CSV, or PDF formats with customizable layouts and styling
- **Handle remote data** through Entity Framework, Dapper, custom adaptors, or REST API connections
- **Implement critical path analysis** for identifying tasks that impact project completion dates
- **Create resource views** showing task allocation and utilization across team members and equipment

This skill provides comprehensive guidance for the `SfGantt<TValue>` component, covering all aspects from basic setup through advanced features like split tasks, baseline tracking, and custom PDF exports.

---

## Documentation and Navigation Guide

This skill is organized into 14 major sections. Each section contains focused reference files covering specific features. Use this navigation guide to identify which reference to read based on the user's needs.

---

### 1. Getting Started

**When to read:** Initial setup, installation, platform-specific configuration

📄 **Read:** [references/getting-started-webassembly.md](references/getting-started-webassembly.md)
- NuGet package installation for Blazor WebAssembly
- Basic component setup and registration
- CSS theme imports and configuration
- First minimal Gantt chart example
- Task data model and field mapping

📄 **Read:** [references/getting-started-server.md](references/getting-started-server.md)
- Blazor Server-specific setup and services
- Service registration in Program.cs
- Component initialization differences

---

### 2. Data Binding and Remote Data

**When to read:** Setting up data sources, connecting to databases, custom data adaptors

📄 **Read:** [references/data-binding-and-field-mapping.md](references/data-binding-and-field-mapping.md)
- Hierarchical and self-referential data structures
- GanttTaskFields configuration and mapping
- Required vs optional fields
- SfDataManager integration
- Load-on-demand for large datasets

📄 **Read:** [references/remote-data-adaptors.md](references/remote-data-adaptors.md)
- UrlAdaptor for REST APIs
- Custom DataAdaptor implementation
- Handling CRUD, search, filter, sort operations
- Injecting services and passing parameters

---

### 3. Task Management

**When to read:** Creating, editing, deleting tasks, dependencies, scheduling, undo/redo, critical path

📄 **Read:** [references/task-scheduling-and-hierarchy.md](references/task-scheduling-and-hierarchy.md)
- Nested hierarchical vs self-referential (ParentID) data structures
- Task types: parent (rollup), child (work item), milestone (zero-duration), unscheduled
- Auto-scheduling and manual scheduling approaches
- Duration units: Day, Hour, Minute
- Hierarchy effects: parent rollup, expand/collapse, indent/outdent

📄 **Read:** [references/task-crud-operations.md](references/task-crud-operations.md)
- Enable editing — GanttEditSettings (AllowAdding, AllowEditing, AllowDeleting, AllowTaskbarEditing)
- Edit modes — Auto, Dialog, Taskbar
- Adding tasks via toolbar, context menu, or programmatic API
- Dialog customization — expose only selected fields in the edit form
- Remote CRUD — InsertUrl, UpdateUrl, RemoveUrl, BatchUrl
- Delete behavior — cascade rules and child handling

📄 **Read:** [references/task-dependencies-and-validation.md](references/task-dependencies-and-validation.md)
- Dependency types: FS, SS, FF, SF
- Predecessor field mapping via GanttTaskFields.Dependency
- Dependency string format (e.g., `3FS`, `5SS+2d`)
- Circular dependency detection
- Auto-scheduling interaction with dependency changes

📄 **Read:** [references/critical-path-and-analysis.md](references/critical-path-and-analysis.md)
- EnableCriticalPath — highlights critical tasks in red automatically
- GanttCriticalPathSettings.SlackValue — widen critical zone to near-critical tasks
- "CriticalPath" toolbar item — user-facing toggle
- QueryChartRowInfo + GanttTaskModel.IsCritical — custom critical taskbar color/style
- GetCriticalTasksAsync() — retrieve critical task list programmatically
- Calculation rules: progress < 100%, dependency-driven, recalculates on every change

📄 **Read:** [references/undo-redo-and-state.md](references/undo-redo-and-state.md)
- EnableUndoRedo + UndoRedoActions list — track 23 action types (Add, Edit, Delete, TaskbarEdit, Sort, Filter, ZoomIn/Out, Indent/Outdent, RowDragAndDrop, etc.)
- MaxUndoRedoSteps — limit history size (default: 20)
- UndoAsync() / RedoAsync() — programmatic undo/redo
- "Undo" / "Redo" toolbar items — keyboard shortcut Ctrl+Z / Ctrl+Y
- OnUndoRedo event — react after undo/redo with CanUndo/CanRedo state
- State preservation patterns — expand/collapse, selection, zoom, filters, scroll position

---

### 4. Timeline Configuration

**When to read:** Timeline views, zoom levels, working hours, holidays, time zones, effort tracking

📄 **Read:** [references/timeline-configuration.md](references/timeline-configuration.md)
- Single-tier vs dual-tier timeline layouts
- GanttTimelineSettings, GanttTopTierSettings, GanttBottomTierSettings
- TimelineViewMode units: Hour, Day, Week, Month, Year
- TimelineUnitSize — controls cell width / density
- Format strings for tier labels
- ProjectStartDate / ProjectEndDate for visible date range

📄 **Read:** [references/zooming-and-view-control.md](references/zooming-and-view-control.md)
- ZoomIn, ZoomOut, ZoomToFit toolbar items
- ZoomToFit — fits the entire schedule into the visible viewport
- Programmatic zoom via toolbar or code
- Recommended defaults: day/week for operational, week/month for roadmap views

📄 **Read:** [references/working-time-holidays-timezone.md](references/working-time-holidays-timezone.md)
- DayWorkingTime — define working hours per day (e.g., 8-hour day)
- WorkWeek — specify which days are working days
- GanttHolidays / GanttHoliday — non-working dates with From/To/Label
- Timezone considerations for multi-region teams
- Impact on task duration calculations and auto-scheduling

📄 **Read:** [references/work-and-effort-tracking.md](references/work-and-effort-tracking.md)
- Work field mapping in GanttTaskFields.Work
- Task types: FixedDuration, FixedWork, FixedUnit
- Work = Duration × WorkingHoursPerDay × (Unit / 100)
- Resource allocation units affect work calculation
- WorkUnit property (Hour, Day, Minute)

---

### 5. Resources

**When to read:** Resource assignment, resource views, overallocation detection, assignment CRUD

📄 **Read:** [references/resources-and-allocation.md](references/resources-and-allocation.md)
- GanttResource — DataSource, Id, Name, MaxUnits, Group field mappings
- GanttAssignmentFields — PrimaryKey, TaskID, ResourceID, Units
- ResourceData model (ResourceId, ResourceName, MaxUnit) + AssignmentModel (PrimaryId, TaskID, ResourceId, Unit)
- Assign resources via cell editing (GanttResourceColumn), dialog, or programmatically
- AddResourceAssignmentAsync / UpdateResourceAssignmentAsync / DeleteResourceAssignmentAsync
- Task types with resources: FixedDuration, FixedWork, FixedUnit
- ViewType.ResourceView — resource-centric hierarchy (resources as parents, tasks as children)
- ShowOverallocation="true" — highlights over-allocated date ranges in resource view
- Over-allocation = daily work from all tasks > DayWorkingHours × MaxUnits / 100
- GanttLabelSettings.RightLabel="Resources" — show resource names in taskbar label

📄 **Read:** [references/assignment-fields-and-resource-mapping.md](references/assignment-fields-and-resource-mapping.md)
- GanttAssignmentFields\<TValue, TAssignment\> class and properties
- Resource assignment collection and foreign key relationships
- AddResourceAssignmentAsync, UpdateResourceAssignmentAsync, DeleteResourceAssignmentAsync
- Many-to-many task-resource mapping patterns
- Resource allocation units and percentage tracking

---

### 6. Task Segments and Splitting

**When to read:** Split tasks, task segments, interrupted schedules, paused work

📄 **Read:** [references/segment-fields-and-split-tasks.md](references/segment-fields-and-split-tasks.md)
- GanttSegmentFields\<TValue, TSegments\> — PrimaryKey, TaskID, StartDate, EndDate field mappings
- Segment data model: separate collection linked to tasks via TaskID
- SplitTaskAsync — split a task at a specified date programmatically
- MergeTaskAsync — merge segments back into a single task
- UI-based splitting via context menu or dialog
- Segment taskbar rendering — multiple connected bars with gaps
- Use cases: planned breaks, resource unavailability, approval pauses

---

### 7. Visualization

**When to read:** Taskbar templates, baselines, markers, labels, interactive taskbar editing

📄 **Read:** [references/taskbar-and-templates.md](references/taskbar-and-templates.md)
- GanttTemplates.TaskbarTemplate — custom RenderFragment for taskbar content
- GanttTooltipSettings.TaskbarTemplate — custom tooltip content
- ShowBaseline / BaselineColor / BaselineStart / BaselineEnd — planned vs actual comparison
- TaskbarHeight / RowHeight — size control
- Performance note: combine complex templates with EnableVirtualization

📄 **Read:** [references/markers-and-labels.md](references/markers-and-labels.md)
- GanttEventMarkers / GanttEventMarker — vertical timeline lines with Date, Text, CssClass
- GanttLabelSettings — LeftLabel, RightLabel, TopLabel with field name or template
- Data markers via TaskbarTemplate — conditional icons based on task properties
- Limit markers to essential dates to avoid timeline clutter

📄 **Read:** [references/taskbar-editing.md](references/taskbar-editing.md)
- AllowTaskbarEditing="true" in GanttEditSettings
- Drag to move task — updates StartDate and EndDate
- Resize left/right handle — adjusts start or end date
- Drag progress handle — updates Progress field
- TaskbarEdited event — receives TaskbarEditedEventArgs\<TValue\>
- OnActionBegin — validate or cancel taskbar edits via args.Cancel

---

### 8. Grid Columns

**When to read:** Column configuration, types, formatting, show/hide, autofit, validation, reordering, resizing, frozen columns, column menu

📄 **Read:** [references/column-basics-and-formatting.md](references/column-basics-and-formatting.md)
- GanttColumn definition (Field, HeaderText, Width, TextAlign, Format, Type, Visible)
- Column types including DateOnly, TimeOnly, Checkbox
- Number formatting (N, C, P variants) and date formatting (custom pattern strings)
- AutoFit columns — AutoFitColumnsAsync called in DataBound event
- Show or hide columns programmatically — ShowColumnsAsync / HideColumnsAsync
- TreeColumnIndex — move expand/collapse tree icons to a different column
- HideAtMedia — responsive columns via CSS media queries
- Per-column action controls: AllowEditing, AllowFiltering, AllowSorting, AllowReordering, AllowResizing

📄 **Read:** [references/column-interactions.md](references/column-interactions.md)
- Column reordering — AllowReordering on SfGantt and per-column override
- Column resizing — AllowResizing, MinWidth, MaxWidth; double-click to autofit
- Frozen columns — FrozenColumns (first N) vs IsFrozen per column (not combinable)
- Freeze direction — FreezeDirection.Left / FreezeDirection.Right with IsFrozen
- Draggable freeze line — AllowFreezeLineMoving
- Column menu — ShowColumnMenu on SfGantt with required companions (AllowSorting, AllowFiltering, AllowResizing)
- Built-in column menu items: Sort Ascending, Sort Descending, AutoFit, AutoFit All, Column Chooser, Filter
- Disable column menu per column — GanttColumn.ShowColumnMenu="false"
- Column chooser — ShowColumnChooser + "ColumnChooser" toolbar item

📄 **Read:** [references/column-validation-and-editing.md](references/column-validation-and-editing.md)
- Enable editing — GanttEditSettings, IsPrimaryKey on key column
- Inline ValidationRules — Required, RangeLength, Range, Number, Min, Max, Messages dictionary
- Data annotation validation — [Required], [StringLength], [Range] on model class
- Custom validation attribute — inherit ValidationAttribute, override IsValid()
- Custom validator component — GanttEditSettings.Validator + ValidatorTemplateContext + ShowValidationMessage + IDisposable
- Per-column EditType — DefaultEdit, NumericEdit, DatePickerEdit, DateTimePickerEdit, BooleanEdit, DropDownEdit
- Note: validation not supported for Resource column

---

### 9. Rows and Selection

**When to read:** Row operations, cell/row selection modes

📄 **Read:** [references/rows-and-height.md](references/rows-and-height.md)
- RowHeight configuration
- AutoFitRows feature
- Row template customization
- Row-level styling

📄 **Read:** [references/selection-modes.md](references/selection-modes.md)
- SelectionSettings (Mode, Type, CellSelectionMode)
- Row selection (Single/Multiple)
- Cell selection (Box/Flow/BoxWithBorder)
- Programmatic selection APIs (SelectRow, SelectCell)

---

### 10. Interactions

**When to read:** User interactions, filtering, sorting, searching

📄 **Read:** [references/filtering-and-search.md](references/filtering-and-search.md)
- FilterSettings (Type: Menu, Excel)
- FilterHierarchyMode (Parent, Child, Both, None)
- Search functionality (SearchSettings)
- Column-level filtering

📄 **Read:** [references/sorting-operations.md](references/sorting-operations.md)
- AllowSorting configuration
- Single and multi-column sorting
- SortSettings (Columns collection)
- Programmatic sorting APIs

📄 **Read:** [references/context-menu-and-toolbar.md](references/context-menu-and-toolbar.md)
- Toolbar built-in items (18 items) and custom items with Align support
- Mixed built-in + custom toolbar using List\<Object\>
- Enable/disable toolbar items dynamically via EnableItems method
- Context menu — EnableContextMenu, built-in items, custom items, sub-menus
- Disable context menu for specific columns via ContextMenuOpen event
- ContextMenuItemClicked event for custom item handling

📄 **Read:** [references/touch-and-interaction-modes.md](references/touch-and-interaction-modes.md)
- Touch interaction support
- EnableTouch property
- Splitter for resizing grid/chart panes
- SplitterSettings configuration

📄 **Read:** [references/clipboard-and-accessibility.md](references/clipboard-and-accessibility.md)
- Copy/paste functionality
- Clipboard module integration
- Keyboard navigation
- Accessibility features

---

### 11. Performance

**When to read:** Optimization strategies, large datasets

📄 **Read:** [references/virtualization-and-performance.md](references/virtualization-and-performance.md)
- EnableRowVirtualization for large datasets
- Row and column virtualization
- Frozen columns (FrozenColumns property)
- WebAssembly performance optimization tips
- Best practices for handling 10,000+ tasks

---

### 12. Export

**When to read:** PDF and Excel export features

📄 **Read:** [references/pdf-export.md](references/pdf-export.md)
- PdfExport() method
- PdfExportProperties configuration
- Multi-page PDF export
- Header and footer customization
- Page size and styling in PDF
- Template-based PDF export

📄 **Read:** [references/excel-export.md](references/excel-export.md)
- ExcelExport() method
- ExcelExportProperties configuration
- Column formatting in Excel
- Hierarchical data export
- Custom cell styling

---

### 13. API Reference

**When to read:** Detailed API documentation lookup

📄 **Read:** [references/gantt-methods-properties.md](references/gantt-methods-properties.md)
- SfGantt<TValue> main component
- GanttTaskFields mapping
- GanttColumn configuration
- GanttResource properties
- GanttSegment for task splitting

📄 **Read:** [references/api-settings-classes.md](references/api-settings-classes.md)
- GanttEditSettings (AllowAdding, AllowEditing, AllowDeleting, Mode)
- GanttSelectionSettings (Mode, Type, CellSelectionMode)
- GanttFilterSettings (Type, HierarchyMode)
- GanttSortSettings and GanttSearchSettings
- GanttTimelineSettings and GanttSplitterSettings

📄 **Read:** [references/api-event-args.md](references/api-event-args.md)
- GanttActionEventArgs<TValue>
- BeforeTooltipRenderEventArgs<TValue>
- BeforeCopyEventArgs
- Other event argument classes

📄 **Read:** [references/api-enums-and-types.md](references/api-enums-and-types.md)
- EditMode (Auto, Dialog, Taskbar)
- DependencyType (FS, SF, FF, SS)
- DurationUnit (Day, Hour, Minute)
- FilterType, ViewType, GridLine enums
- Other enum types
---

## Quick Start Example

```razor
@page "/gantt"
@using Syncfusion.Blazor.Gantt

<SfGantt DataSource="@TaskCollection" Height="450px" Width="100%">
    <GanttTaskFields Id="TaskId" 
                     Name="TaskName" 
                     StartDate="StartDate" 
                     EndDate="EndDate"
                     Duration="Duration" 
                     Progress="Progress" 
                     ParentID="ParentId"
                     Dependency="Predecessor">
    </GanttTaskFields>
</SfGantt>

@code {
    private List<TaskData> TaskCollection { get; set; }

    protected override void OnInitialized()
    {
        TaskCollection = new List<TaskData>
        {
            new TaskData { TaskId = 1, TaskName = "Project Initiation", StartDate = new DateTime(2024, 04, 01), EndDate = new DateTime(2024, 04, 05), Progress = 100 },
            new TaskData { TaskId = 2, TaskName = "Identify Site location", StartDate = new DateTime(2024, 04, 01), Duration = 4, Progress = 50, ParentId = 1 },
            new TaskData { TaskId = 3, TaskName = "Perform Soil test", StartDate = new DateTime(2024, 04, 01), Duration = 3, Progress = 70, Predecessor = "2", ParentId = 1 },
        };
    }

    public class TaskData
    {
        public int TaskId { get; set; }
        public string TaskName { get; set; }
        public DateTime StartDate { get; set; }
        public DateTime? EndDate { get; set; }
        public int? Duration { get; set; }
        public int Progress { get; set; }
        public int? ParentId { get; set; }
        public string Predecessor { get; set; }
    }
}
```

---

## Common Patterns

### Pattern 1: Full CRUD with Dialog Editing

```razor
<SfGantt DataSource="@TaskCollection" Toolbar="@ToolbarItems">
    <GanttTaskFields Id="TaskId" Name="TaskName" StartDate="StartDate" 
                     EndDate="EndDate" Duration="Duration" Progress="Progress" 
                     ParentID="ParentId" Dependency="Predecessor">
    </GanttTaskFields>
    <GanttEditSettings AllowAdding="true" 
                       AllowEditing="true" 
                       AllowDeleting="true"
                       AllowTaskbarEditing="true"
                       Mode="EditMode.Dialog">
    </GanttEditSettings>
</SfGantt>

@code {
    private List<string> ToolbarItems = new List<string> { "Add", "Edit", "Delete" };
}
```

### Pattern 2: Critical Path with Resource Allocation

```razor
<SfGantt DataSource="@TaskCollection" 
         Resources="@ResourceCollection"
         EnableCriticalPath="true">
    <GanttTaskFields Id="TaskId" Name="TaskName" StartDate="StartDate" 
                     EndDate="EndDate" Duration="Duration" 
                     ResourceInfo="Resources" Work="Work">
    </GanttTaskFields>
    <GanttResource DataSource="@ResourceCollection" 
                   Id="ResourceId" 
                   Name="ResourceName">
    </GanttResource>
    <GanttCriticalPathSettings SlackValue="1"></GanttCriticalPathSettings>
</SfGantt>
```

### Pattern 3: Remote Data with Entity Framework

```razor
<SfGantt TValue="TaskData">
    <SfDataManager Adaptor="Adaptors.UrlAdaptor" Url="api/gantt"></SfDataManager>
    <GanttTaskFields Id="TaskId" Name="TaskName" StartDate="StartDate" 
                     EndDate="EndDate" Duration="Duration" ParentID="ParentId">
    </GanttTaskFields>
</SfGantt>
```

### Pattern 4: Export with Custom Styling

```razor
<SfGantt @ref="GanttRef" 
         DataSource="@TaskCollection" 
         AllowPdfExport="true" 
         AllowExcelExport="true"
         Toolbar="@ToolbarItems">
    <GanttTaskFields Id="TaskId" Name="TaskName" StartDate="StartDate" EndDate="EndDate">
    </GanttTaskFields>
    <GanttEvents TValue="TaskData" OnToolbarClick="ToolbarClickHandler"></GanttEvents>
</SfGantt>

@code {
    private SfGantt<TaskData> GanttRef;
    private List<string> ToolbarItems = new List<string> { "PdfExport", "ExcelExport" };

    private async Task ToolbarClickHandler(ClickEventArgs args)
    {
        if (args.Item.Id == "PdfExport")
            await GanttRef.ExportToPdfAsync();
        else if (args.Item.Id == "ExcelExport")
            await GanttRef.ExportToExcelAsync();
    }
}
```

---

## Key Configuration Points

### Essential Properties

- **DataSource**: Task collection (hierarchical or self-referential)
- **GanttTaskFields**: Maps task properties (Id, Name, StartDate, Duration, ParentID, Dependency)
- **Height/Width**: Component dimensions
- **Toolbar**: Built-in actions (Add, Edit, Delete, Search, Export)
- **GanttEditSettings**: CRUD permissions and edit mode

### Timeline Configuration

- **DurationUnit**: Default duration unit (Day, Hour, Minute)
- **ProjectStartDate/ProjectEndDate**: Timeline boundaries
- **GanttTimelineSettings**: Timeline tiers, format, zoom levels
- **DayWorkingTime**: Working hours per day
- **Holidays**: Non-working dates

### Visual Customization

- **GanttTaskbarSettings**: Taskbar height and styling
- **GanttLabelSettings**: Left, right, top labels
- **GanttTemplates**: Custom taskbar, tooltip, milestone templates
- **BaselineColor/ConnectorLineBackground**: Visual styling

### Performance Optimization

- **EnableRowVirtualization**: For large datasets (1000+ tasks)
- **EnableColumnVirtualization**: For many columns
- **LoadChildOnDemand**: Lazy-load child tasks
- **TreeColumnIndex**: Specify hierarchy column

---

## Common Use Cases

1. **Project Management Dashboard**: Full CRUD, dependencies, critical path, resource allocation
2. **Construction Scheduling**: Baseline tracking, split tasks, holidays, custom working hours
3. **Resource Planning**: Resource view, overallocation detection, work-based scheduling
4. **Sprint Planning**: Timeline zooming, milestone tracking, progress indicators
5. **Manufacturing Timeline**: Multi-level hierarchy, dependency validation, PDF export

---

## Next Steps

After reviewing this navigation guide:

1. **For initial setup**: Start with section 1 (Getting Started) based on your platform
2. **For data connectivity**: Review section 2 (Data Binding) for your data source type
3. **For specific features**: Navigate to the relevant section and read focused reference files
4. **For API details**: Consult section 13 (API Reference) for properties, methods, and classes

> **Events reference**: Event argument types are documented in `references/api-event-args.md`. Individual feature files (e.g., `taskbar-editing.md`, `filtering-and-search.md`, `sorting-operations.md`, `selection-modes.md`) each document the events specific to that feature with handler signatures and examples.

Each reference file provides complete implementation details, code examples, edge cases, and troubleshooting guidance for its specific topic.










