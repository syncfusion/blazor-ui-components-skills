# API — Settings Classes Reference

## Table of Contents
- [GanttEditSettings](#gantteditsettings)
- [GanttSelectionSettings](#ganttselectionsettings)
- [GanttFilterSettings](#ganttfiltersettings)
- [GanttSortSettings](#ganttsortsettings)
- [GanttSearchSettings](#ganttsearchsettings)
- [GanttTimelineSettings](#gantttimelinesettings)
- [GanttTimelineTierSettings](#gantttimelinetiersettings)
- [GanttSplitterSettings](#ganttsplittersettings)
- [GanttTooltipSettings](#gantttooltipsettings)
- [GanttLabelSettings](#ganttlabelsettings)
- [GanttTaskbarSettings](#gantttaskbarsettings)
- [GanttCriticalPathSettings](#ganttcriticalpathsettings)
- [GanttKeySettings](#ganttkeysettings)
- [GanttColumnChooserSettings](#ganttcolumnchoosersettings)
- [Export Settings Classes](#export-settings-classes)
- [Usage Notes](#usage-notes)
- [Related References](#related-references)

---
Comprehensive reference for Gantt configuration classes. Each settings class controls specific aspects of the Gantt component behavior. These are typically declared as child components within `<SfGantt>`.

---

## GanttEditSettings

Configures CRUD operations and editing behavior.

### Properties
- **`AllowAdding`** (`bool`) — Enable adding new records. Default: false
- **`AllowEditing`** (`bool`) — Enable editing existing records. Default: false
- **`AllowDeleting`** (`bool`) — Enable deleting records. Default: false
- **`AllowTaskbarEditing`** (`bool`) — Enable taskbar resizing, dragging, progress resizing, and predecessor drawing. Default: false
- **`AllowSchedulingOnDrag`** (`bool`) — Allow click-and-drag on chart to schedule dates. Default: false
- **`Mode`** (`EditMode`) — Edit mode (Auto/Dialog). Auto = cell edit in grid, dialog in chart. Default: Auto
- **`NewRowPosition`** (`RowPosition`) — Where to add new records (Top/Bottom/Above/Below/Child). Default: Top
- **`ShowDeleteConfirmDialog`** (`bool`) — Show confirmation before deleting. Default: false
- **`Validator`** (`RenderFragment<object>?`) — Custom validation component for edit form. Default: null

### Example
```csharp
<SfGantt DataSource="@TaskCollection" Toolbar="@(new List<string>() { "Add", "Edit", "Delete", "Update", "Cancel" })">
    <GanttTaskFields Id="TaskId" Name="TaskName" StartDate="StartDate" 
                     EndDate="EndDate" Duration="Duration" ParentID="ParentId">
    </GanttTaskFields>
    <GanttEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true"
                       AllowTaskbarEditing="true" ShowDeleteConfirmDialog="true">
    </GanttEditSettings>
</SfGantt>
```

---

## GanttSelectionSettings

Configures row and cell selection behavior.

### Properties
- **`Mode`** (`SelectionMode`) — Selection mode (Row/Cell/Both). Default: Row
- **`Type`** (`SelectionType`) — Selection type (Single/Multiple). Default: Single
- **`CellSelectionMode`** (`CellSelectionMode`) — Cell selection mode (Flow/Box/BoxWithBorder). Default: Flow
- **`EnableToggle`** (`bool`) — Allow toggling selection by clicking selected row. Default: false
- **`PersistSelection`** (`bool`) — Persist selection across operations (sort/filter). Default: false
- **`AllowDragSelection`** (`bool`) — Enable drag-to-select multiple rows/cells. Default: false

### Example
```csharp
<SfGantt DataSource="@TaskCollection">
    <GanttSelectionSettings Mode="SelectionMode.Both" 
                            Type="SelectionType.Multiple" 
                            EnableToggle="true"
                            AllowDragSelection="true">
    </GanttSelectionSettings>
</SfGantt>
```

---

## GanttFilterSettings

Configures filtering behavior.

### Properties
- **`FilterType`** (`FilterType`) — Filter UI type (Menu/Excel). Default: Menu
  - `Menu`: Standard filter menu
  - `Excel`: Advanced filter with checkboxes, search, and custom templates
- **`HierarchyMode`** (`FilterHierarchyMode`) — How to filter hierarchical data:
  - `Parent`: Show filtered record with parent
  - `Child`: Show filtered record with children
  - `Both`: Show with both parent and children
  - `None`: Show only filtered record (default)
- **`Columns`** (`List<PredicateModel>`) — Initial filter column definitions
- **`IgnoreAccent`** (`bool`) — Ignore diacritic characters/accents. Default: false
- **`Operators`** (`object`) — Custom filter operators by data type

### Example
```csharp
<SfGantt DataSource="@TaskCollection" AllowFiltering="true">
    <GanttFilterSettings FilterType="FilterType.Excel" 
                         HierarchyMode="FilterHierarchyMode.Parent">
    </GanttFilterSettings>
</SfGantt>
```

---

## GanttSortSettings

Configures sorting behavior.

### Properties
- **`Columns`** (`List<GanttSortDescriptor>`) — Initial sort columns and directions
- **`AllowUnsort`** (`bool`) — Allow unsort by clicking sorted column header. Default: true

### Example
```csharp
<SfGantt DataSource="@TaskCollection" AllowSorting="true" AllowMultiSorting="true">
    <GanttSortSettings AllowUnsort="true">
        <GanttSortDescriptors>
            <GanttSortDescriptor Field="TaskId" Direction="SortDirection.Descending"></GanttSortDescriptor>
            <GanttSortDescriptor Field="TaskName" Direction="SortDirection.Ascending"></GanttSortDescriptor>
        </GanttSortDescriptors>
    </GanttSortSettings>
</SfGantt>
```

---

## GanttSearchSettings

Configures search behavior.

### Properties
- **`Fields`** (`string[]`) — Columns to search. Default: all columns
- **`Key`** (`string`) — Search keyword. Default: empty
- **`Operator`** (`Operator`) — Search operator (Contains/Equals/StartsWith/EndsWith). Default: Contains
- **`HierarchyMode`** (`FilterHierarchyMode`) — Search mode for hierarchical data (Parent/Child/Both/None)
- **`IgnoreCase`** (`bool`) — Case-insensitive search. Default: true
- **`IgnoreAccent`** (`bool`) — Ignore accents (remote data only). Default: false

### Example
```csharp
<SfGantt DataSource="@TaskCollection" Toolbar="@(new List<string>() { "Search" })">
    <GanttSearchSettings Fields="@(new string[] { "TaskName", "Duration" })" 
                         Operator="Operator.Contains" 
                         IgnoreCase="true">
    </GanttSearchSettings>
</SfGantt>
```

---

## GanttTimelineSettings

Configures timeline display and behavior.

### Properties
- **`TopTier`** (`GanttTimelineTierSettings`) — Top tier settings (unit, format, count)
- **`BottomTier`** (`GanttTimelineTierSettings`) — Bottom tier settings
- **`TimelineUnitSize`** (`int`) — Width of timeline cell in pixels. Default: 33
- **`TimelineViewMode`** (`TimelineViewMode`) — Timeline view mode (None/Hour/Day/Week/Month/Year). Default: None
- **`ShowTooltip`** (`bool`) — Show tooltip on timeline cell hover. Default: true
- **`UpdateTimescaleView`** (`bool`) — Auto-update timescale on edit. Default: true
- **`WeekStartDay`** (`int`) — Week start day (0=Sunday, 1=Monday, etc.). Default: 0
- **`WeekendBackground`** (`string?`) — Weekend cell background color. Default: null

### Example
```csharp
<SfGantt DataSource="@TaskCollection">
    <GanttTimelineSettings TimelineUnitSize="60" 
                           TimelineViewMode="TimelineViewMode.Week"
                           WeekStartDay="1"
                           ShowTooltip="true">
        <GanttTopTierSettings Unit="TimelineViewMode.Month" Format="MMM yyyy">
        </GanttTopTierSettings>
        <GanttBottomTierSettings Unit="TimelineViewMode.Week" Format="dd">
        </GanttBottomTierSettings>
    </GanttTimelineSettings>
</SfGantt>
```

---

## GanttTimelineTierSettings

Configures individual timeline tier (top or bottom).

### Properties
- **`Unit`** (`TimelineViewMode`) — Time unit (Hour/Day/Week/Month/Year)
- **`Format`** (`string`) — Date format string (e.g., "MMM dd, yyyy")
- **`Count`** (`int`) — Number of units per cell. Default: 1
- **`Formatter`** (`string` or `RenderFragment`) — Custom format function/template

---

## GanttSplitterSettings

Configures splitter pane (divider between grid and chart).

### Properties
- **`Position`** (`string`) — Initial splitter position in pixels or percentage (e.g., "250px", "30%"). Default: 250px
- **`ColumnIndex`** (`int`) — Column index where splitter is positioned. Default: -1
- **`Minimum`** (`string`) — Minimum width of grid part. Default: null
- **`SeparatorSize`** (`double`) — Splitter bar width in pixels. Default: 4
- **`View`** (`SplitterView`) — Predefined view (Default/Grid/Chart). Default: Default
  - `Default`: Show both grid and chart
  - `Grid`: Show grid only
  - `Chart`: Show chart only
- **`Collapsible`** (`bool`) — Enable expand/collapse icon near splitter. Default: false

### Example
```csharp
<SfGantt DataSource="@TaskCollection" Height="450px" Width="100%">
    <GanttSplitterSettings Position="30%" 
                           Minimum="200px" 
                           SeparatorSize="6"
                           Collapsible="true">
    </GanttSplitterSettings>
</SfGantt>
```

---

## GanttTooltipSettings<TValue>

Configures tooltip behavior and templates.

### Properties
- **`ShowTooltip`** (`bool`) — Enable tooltips. Default: true
- **`TaskbarTemplate`** (`RenderFragment<TValue>`) — Custom taskbar tooltip template
- **`ConnectorLineTemplate`** (`RenderFragment<TValue>`) — Custom connector line tooltip template
- **`EditingTemplate`** (`RenderFragment<TValue>`) — Custom editing tooltip template
- **`Template`** (`RenderFragment<TValue>`) — General tooltip template

### Example
```csharp
<SfGantt DataSource="@TaskCollection">
    <GanttTooltipSettings ShowTooltip="true" TValue="TaskData">
        <TaskbarTemplate>
            @{
                var task = context as TaskData;
                <div>
                    <strong>@task.TaskName</strong><br />
                    Duration: @task.Duration days<br />
                    Progress: @task.Progress%
                </div>
            }
        </TaskbarTemplate>
    </GanttTooltipSettings>
</SfGantt>
```

---

## GanttLabelSettings<TValue>

Configures taskbar label display.

### Properties
- **`LeftLabel`** (`string`) — Field name for left taskbar label
- **`RightLabel`** (`string`) — Field name for right taskbar label
- **`TaskLabel`** (`string`) — Field name for task label inside taskbar
- **`LeftLabelTemplate`** (`RenderFragment<TValue>`) — Custom left label template
- **`RightLabelTemplate`** (`RenderFragment<TValue>`) — Custom right label template
- **`TaskLabelTemplate`** (`RenderFragment<TValue>`) — Custom task label template

### Example
```csharp
<SfGantt DataSource="@TaskCollection">
    <GanttLabelSettings LeftLabel="TaskName" 
                        RightLabel="Progress" 
                        TaskLabel="Duration"
                        TValue="TaskData">
    </GanttLabelSettings>
</SfGantt>
```

---

## GanttTaskbarSettings

Configures advanced taskbar behavior.

### Properties
- **`EnableMultiTaskbar`** (`bool`) — Enable rendering multiple taskbars for same resource. Default: false
- **`AllowTaskbarDragAndDrop`** (`bool`) — Enable taskbar drag-and-drop between rows. Default: false
- **`ShowParentTaskbar`** (`bool`) — Show parent taskbar. Default: true

### Example
```csharp
<SfGantt DataSource="@TaskCollection">
    <GanttTaskbarSettings EnableMultiTaskbar="true" 
                          AllowTaskbarDragAndDrop="true">
    </GanttTaskbarSettings>
</SfGantt>
```

---

## GanttCriticalPathSettings

Configures critical path highlighting.

### Properties
- **`SlackValue`** (`int`) — Slack threshold in days for critical path. Default: 0

### Example
```csharp
<SfGantt DataSource="@TaskCollection" EnableCriticalPath="true">
    <GanttCriticalPathSettings SlackValue="2">
    </GanttCriticalPathSettings>
</SfGantt>
```

---

## GanttKeySettings

Configures keyboard shortcuts.

### Properties
Various keyboard shortcut mappings (e.g., `MoveLeftCell`, `MoveRightCell`, `DeleteRecord`, etc.)

### Example
```csharp
<SfGantt DataSource="@TaskCollection">
    <GanttKeySettings MoveLeftCell="Shift+LeftArrow" 
                      MoveRightCell="Shift+RightArrow">
    </GanttKeySettings>
</SfGantt>
```

---

## GanttColumnChooserSettings

Configures column chooser behavior.

### Properties
Properties for customizing which columns appear in the column chooser and their order.

---

## Export Settings Classes

### GanttPdfExportProperties
Properties for PDF export: `FileName`, `PageOrientation`, `PageSize`, `Theme`, `IncludeHiddenColumns`, custom headers/footers.

### GanttExcelExportProperties  
Properties for Excel/CSV export: `FileName`, `IncludeHiddenColumns`, `ExcelTheme`.

### PdfMultiPageSettings
Multi-page PDF export configuration.

### PdfLabelSettings
PDF label customization.

---

## Usage Notes

- **Nested Components**: Settings classes are declared as child components within `<SfGantt>`:
  ```csharp
  <SfGantt>
      <GanttEditSettings ... />
      <GanttSelectionSettings ... />
  </SfGantt>
  ```

- **Two-Way Binding**: Most settings support property binding for dynamic configuration.

- **Events**: Many settings classes trigger events on the parent `SfGantt` component (e.g., editing triggers `OnActionBegin`, `OnActionComplete`).

- **Grid Parity**: Several settings (Selection, Filter, Sort, Search) mirror Syncfusion Grid component equivalents for consistency.

---
