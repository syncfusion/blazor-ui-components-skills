# API Enums and Types — Complete Reference

Comprehensive reference for all enums and types used in the Syncfusion Blazor Gantt Chart API.

## Table of Contents
- [Scheduling](#scheduling)
- [Task Management](#task-management)
- [Grid & UI](#grid--ui)
- [Data Operations](#data-operations)
- [View & Display](#view--display)

---

## Scheduling

### ScheduleMode
Controls how task schedules are calculated and validated.

**Values:**
- **`Auto`** — System automatically calculates task dates based on dependencies and constraints
- **`Manual`** — User has full control over task dates; system doesn't auto-calculate
- **`Custom`** — Mix of auto and manual; each task can specify its own mode via `Manual` field

**Usage:**
```csharp
<SfGantt TaskMode="ScheduleMode.Auto">
    <GanttTaskFields Manual="Manual"></GanttTaskFields>
</SfGantt>
```

**When to Use:**
- `Auto`: Standard project management with automatic scheduling
- `Manual`: Full user control, ignore dependencies for schedule
- `Custom`: Per-task scheduling mode control

---

### DurationUnit
Specifies the unit of measurement for task duration.

**Values:**
- **`Minute`** — Duration measured in minutes
- **`Hour`** — Duration measured in hours
- **`Day`** — Duration measured in days (default)
- **`Week`** — Duration measured in weeks
- **`Month`** — Duration measured in months
- **`Year`** — Duration measured in years

**Usage:**
```csharp
<SfGantt DurationUnit="DurationUnit.Day" DataSource="@Tasks">
</SfGantt>
```

**Notes:**
- Duration field values must match the unit (e.g., "5" = 5 days if `DurationUnit.Day`)
- Working hours and holidays affect actual calendar time for Day/Week/Month units

---

### DependencyType
Defines task dependency relationships.

**Values:**
- **`FS`** — Finish-to-Start: Successor starts when predecessor finishes
- **`SS`** — Start-to-Start: Both tasks start simultaneously
- **`FF`** — Finish-to-Finish: Both tasks finish simultaneously
- **`SF`** — Start-to-Finish: Successor finishes when predecessor starts

**Usage:**
```csharp
<SfGantt EnablePredecessorValidation="true" 
         DependencyTypes="@(new List<DependencyType>() { DependencyType.FS, DependencyType.SS })">
</SfGantt>

// In task data: Predecessor = "3FS+2" (Task 3, Finish-to-Start, 2-day lag)
```

**When to Use:**
- `FS`: Most common; task B starts after task A finishes
- `SS`: Tasks must start together (e.g., parallel workflows)
- `FF`: Tasks must finish together (e.g., coordinated deliverables)
- `SF`: Rare; used in just-in-time scenarios

---

## Task Management

### TaskType
Determines which task field remains constant during work calculations.

**Values:**
- **`FixedUnit`** — Resource units remain constant; duration/work adjust (default)
- **`FixedDuration`** — Duration remains constant; work/units adjust
- **`FixedWork`** — Work remains constant; duration/units adjust

**Usage:**
```csharp
<SfGantt TaskType="TaskType.FixedUnit">
    <GanttTaskFields Work="Work"></GanttTaskFields>
</SfGantt>

// Or per-task:
public class TaskData {
    public TaskType TaskType { get; set; }
}
```

**Calculation Formulas:**
- Work = Duration × Units
- When `FixedUnit`: Changing Duration recalculates Work
- When `FixedDuration`: Changing Units recalculates Work
- When `FixedWork`: Changing Duration recalculates Units

**Example:**
```csharp
// FixedUnit: 5 days × 2 units = 10 days work
// Change duration to 10 days → 10 days × 2 units = 20 days work

// FixedWork: 10 days work with 2 units = 5 days duration
// Change to 1 unit → 10 days work ÷ 1 unit = 10 days duration

// FixedDuration: 5 days with 10 days work = 2 units
// Change work to 20 days → 20 days ÷ 5 days = 4 units
```

---

### WorkUnit
Specifies the unit for work effort tracking.

**Values:**
- **`Hour`** — Work measured in hours
- **`Day`** — Work measured in days
- **`Minute`** — Work measured in minutes

**Usage:**
```csharp
<SfGantt WorkUnit="WorkUnit.Hour">
    <GanttTaskFields Work="Work"></GanttTaskFields>
</SfGantt>
```

**Notes:**
- Work field values stored as numbers (e.g., 40 = 40 hours)
- Affects work-based calculations with resource assignments

---

### RowPosition
Specifies where to add new tasks relative to existing rows.

**Values:**
- **`Top`** — Add as first row in the grid
- **`Bottom`** — Add as last row in the grid
- **`Above`** — Add above the specified row index
- **`Below`** — Add below the specified row index
- **`Child`** — Add as child of the specified row

**Usage:**
```csharp
TaskData newTask = new TaskData() { TaskId = 100, TaskName = "New Task" };
await Gantt.AddRecordAsync(newTask, 5, RowPosition.Below);
```

**When to Use:**
- `Top`/`Bottom`: Adding root-level tasks without specific position
- `Above`/`Below`: Inserting tasks at specific positions
- `Child`: Creating subtasks under parent tasks

---

## Grid & UI

### EditMode
Controls how users edit tasks.

**Values:**
- **`Auto`** — Both grid and taskbar editing enabled
- **`Dialog`** — Edit only via dialog (double-click or edit button)
- **`Taskbar`** — Edit only via taskbar drag/resize

**Usage:**
```csharp
<SfGantt>
    <GanttEditSettings Mode="EditMode.Auto" AllowEditing="true"></GanttEditSettings>
</SfGantt>
```

**When to Use:**
- `Auto`: Full editing flexibility (recommended)
- `Dialog`: Enforce structured data entry, prevent accidental changes
- `Taskbar`: Quick visual adjustments only

---

### SelectionMode
Defines what can be selected in the grid.

**Values:**
- **`Row`** — Select entire rows only
- **`Cell`** — Select individual cells only
- **`Both`** — Select either rows or cells

**Usage:**
```csharp
<SfGantt AllowSelection="true">
    <GanttSelectionSettings Mode="SelectionMode.Row" Type="SelectionType.Multiple">
    </GanttSelectionSettings>
</SfGantt>
```

---

### SelectionType
Controls single vs multiple selection.

**Values:**
- **`Single`** — Select one row/cell at a time
- **`Multiple`** — Select multiple rows/cells (Ctrl+Click, Shift+Click)

**Usage:**
```csharp
<GanttSelectionSettings Mode="SelectionMode.Row" Type="SelectionType.Multiple">
</GanttSelectionSettings>
```

---

### CellSelectionMode
Defines cell selection behavior (when `SelectionMode="Cell"`).

**Values:**
- **`Flow`** — Cells selected flowing like text (left-to-right, wrap to next row)
- **`Box`** — Rectangular selection area
- **`BoxWithBorder`** — Rectangular selection with border highlight

**Usage:**
```csharp
<GanttSelectionSettings Mode="SelectionMode.Cell" 
                         CellSelectionMode="CellSelectionMode.Box">
</GanttSelectionSettings>
```

---

### GridLine
Controls grid cell border visibility.

**Values:**
- **`None`** — No grid lines
- **`Horizontal`** — Horizontal lines only (default)
- **`Vertical`** — Vertical lines only
- **`Both`** — Both horizontal and vertical lines

**Usage:**
```csharp
<SfGantt GridLines="GridLine.Both">
</SfGantt>
```

---

### SplitterView
Defines splitter position between grid and chart.

**Values:**
- **`Default`** — Use configured splitter position
- **`Grid`** — Show only grid section (chart hidden)
- **`Chart`** — Show only chart section (grid hidden)

**Usage:**
```csharp
await Gantt.SetSplitterPositionAsync(SplitterView.Grid);
await Gantt.SetSplitterPositionAsync(SplitterView.Chart);
await Gantt.SetSplitterPositionAsync(SplitterView.Default);
```

**When to Use:**
- `Grid`: Focus on data entry and review
- `Chart`: Focus on visual timeline and dependencies
- `Default`: Balanced view

---

## Data Operations

### SortDirection
Specifies sort order.

**Values:**
- **`Ascending`** — Sort A→Z, 0→9, oldest→newest
- **`Descending`** — Sort Z→A, 9→0, newest→oldest

**Usage:**
```csharp
await Gantt.SortByColumnAsync("TaskName", SortDirection.Ascending, false);

// Or declaratively:
<GanttSortSettings>
    <GanttSortColumns>
        <GanttSortColumn Field="TaskId" Direction="SortDirection.Descending"></GanttSortColumn>
    </GanttSortColumns>
</GanttSortSettings>
```

---

### FilterType
Controls filter UI style.

**Values:**
- **`Menu`** — Traditional filter menu with operators (default)
- **`Excel`** — Excel-style checkbox filter with search
- **`FilterBar`** — Filter row below headers
- **`CheckBox`** — Checkbox list filter

**Usage:**
```csharp
<SfGantt AllowFiltering="true">
    <GanttFilterSettings Type="FilterType.Excel"></GanttFilterSettings>
</SfGantt>
```

**When to Use:**
- `Menu`: Advanced filtering with operators (contains, equals, etc.)
- `Excel`: Quick multi-value selection
- `FilterBar`: Immediate type-ahead filtering

---

### FilterHierarchyMode
Controls hierarchical data filtering behavior.

**Values:**
- **`Parent`** — Show parent rows if they match filter
- **`Child`** — Show child rows if they match filter
- **`Both`** — Show both parents and children if either matches
- **`None`** — Show only exact matches (ignore hierarchy)

**Usage:**
```csharp
<GanttFilterSettings HierarchyMode="FilterHierarchyMode.Both">
</GanttFilterSettings>
```

**Example:**
- Filter: "Design" with `Both` → Shows parent "Project" + child "Design Phase"
- Filter: "Design" with `None` → Shows only "Design Phase"

---

## View & Display

### ViewType
Switches between project and resource views.

**Values:**
- **`ProjectView`** — Standard task-centric view (default)
- **`ResourceView`** — Resource-centric view with tasks grouped by resource

**Usage:**
```csharp
<SfGantt ViewType="ViewType.ResourceView">
    <GanttResource DataSource="@Resources" TResources="ResourceModel"></GanttResource>
    <GanttAssignmentFields DataSource="@Assignments"></GanttAssignmentFields>
</SfGantt>
```

**When to Use:**
- `ProjectView`: Manage tasks and timeline
- `ResourceView`: Manage resource allocation and workload

---

### TimelineViewMode
Defines timeline scale granularity.

**Values:**
- **`None`** — No timeline
- **`Hour`** — Hourly timeline
- **`Day`** — Daily timeline (default)
- **`Week`** — Weekly timeline
- **`Month`** — Monthly timeline
- **`Year`** — Yearly timeline

**Usage:**
```csharp
<GanttTimelineSettings TimelineViewMode="TimelineViewMode.Week">
</GanttTimelineSettings>
```

**Auto-Zoom:**
Timeline automatically adjusts based on project duration, but can be locked with `TimelineViewMode`.

---

## Common Enum Usage Patterns

### Multiple Enums in Configuration
```csharp
<SfGantt DataSource="@Tasks"
         TaskMode="ScheduleMode.Auto"
         TaskType="TaskType.FixedUnit"
         DurationUnit="DurationUnit.Day"
         GridLines="GridLine.Both"
         AllowFiltering="true"
         AllowSelection="true">
    
    <GanttTaskFields TaskType="TaskType" Manual="IsManual"></GanttTaskFields>
    
    <GanttEditSettings Mode="EditMode.Auto" AllowEditing="true"></GanttEditSettings>
    
    <GanttSelectionSettings Mode="SelectionMode.Row" Type="SelectionType.Multiple">
    </GanttSelectionSettings>
    
    <GanttFilterSettings Type="FilterType.Excel" HierarchyMode="FilterHierarchyMode.Both">
    </GanttFilterSettings>
</SfGantt>
```

### Programmatic Enum Usage
```csharp
// Add task with position
await Gantt.AddRecordAsync(newTask, index, RowPosition.Below);

// Set splitter view
await Gantt.SetSplitterPositionAsync(SplitterView.Chart);

// Sort data
await Gantt.SortByColumnAsync("TaskName", SortDirection.Ascending, true);

// Add dependency
Gantt.AddPredecessor(12, "4FS+2"); // Task 4, Finish-to-Start, 2-day lag
```

---

## Type Aliases & Interfaces

### Key Types
- **`TValue`** — Generic type for task data model
- **`TResources`** — Generic type for resource data model
- **`TAssignment`** — Generic type for resource assignment data model
- **`TSegments`** — Generic type for task segment data model

**Usage:**
```csharp
<SfGantt TValue="TaskData" DataSource="@Tasks">
    <GanttResource TResources="ResourceModel" DataSource="@Resources">
    </GanttResource>
    <GanttAssignmentFields TAssignment="AssignmentModel" DataSource="@Assignments">
    </GanttAssignmentFields>
</SfGantt>
```

---

## Cross-References

- **Related References:**

---
