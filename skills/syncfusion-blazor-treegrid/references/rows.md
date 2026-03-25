# Rows in Syncfusion Blazor TreeGrid

## Table of Contents
- [Customize Row Appearance](#customize-row-appearance)
  - [Styling Alternate Rows](#styling-alternate-rows)
  - [Accessing Row Model Information Programmatically](#accessing-row-model-information-programmatically)
- [Row Height](#row-height)
- [Row Template](#row-template)
  - [Row Template with Formatting](#row-template-with-formatting)
  - [Limitations](#limitations)
- [Detail Template](#detail-template)
- [Row Drag and Drop](#row-drag-and-drop)
  - [Drag and Drop within Tree Grid](#drag-and-drop-within-tree-grid)
  - [Drag and Drop to Another Tree Grid](#drag-and-drop-to-another-tree-grid)
  - [Drag and Drop Events](#drag-and-drop-events)
- [Row Spanning](#row-spanning)
  - [AutoSpan Mode](#autospan-mode)
  - [Disable Spanning for Specific Columns](#disable-spanning-for-specific-columns)
  - [Controlling Spanning at TreeGrid and Column Levels](#controlling-spanning-at-treegrid-and-column-levels)
  - [Applying Row Spanning Programmatically](#applying-row-spanning-programmatically)
  - [Clearing Spanning Programmatically](#clearing-spanning-programmatically)
- [Expand and Collapse](#expand-and-collapse)

---

## Customize Row Appearance

The appearance of a row can be customized by using the `RowDataBound` event. The `RowDataBound` event triggers for every row. In the event handler, the **args** is achieved which contains the details of the row.

```razor
<SfTreeGrid DataSource="@TreeGridData" ParentIdMapping="ParentId" IdMapping="TaskId" TreeColumnIndex="1">
    <TreeGridEvents RowDataBound="OnRowDataBound" TValue="TreeData" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="90" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="100" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Priority" HeaderText="Priority"  Width="100" />
    </TreeGridColumns>
</SfTreeGrid>

<style>
    .equal-5 {
        background-color: #336c12;
    }
    .above-6 {
        background-color: #7b2b1d;
    }
</style>

@code {
    public List<TreeData> TreeGridData { get; set; }

    protected override void OnInitialized()
    {
        this.TreeGridData = TreeData.GetSelfDataSource().ToList();
    }

    private void OnRowDataBound(RowDataBoundEventArgs<TreeData> args)
    {
        if (args.Data.Duration == 5)
        {
            args.Row.AddClass(new string[] { "equal-5" });
        }
        else if (args.Data.Duration > 6)
        {
            args.Row.AddClass(new string[] { "above-6" });
        }
    }
}
```

---

## Styling Alternate Rows

The tree grid's alternative rows' background color can be changed by overriding the `.e-altrow` class:

```css
.e-treegrid .e-altrow {
    background-color: #fafafa;
}
```

```razor
<SfTreeGrid DataSource="@TreeGridData" ParentIdMapping="ParentId" IdMapping="TaskId" TreeColumnIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
    </TreeGridColumns>
</SfTreeGrid>

<style>
    .e-treegrid .e-altrow {
        background-color: #fafafa;
    }
</style>
```

---

## Accessing Row Model Information Programmatically

The Blazor Tree Grid provides a method called `GetRowModel` that can be used to obtain the values associated with row model details. These details include the level, expanded status, and child records status of a record.

```razor
<SfButton OnClick="TreeProps" CssClass="e-primary" IsPrimary="true" Content="Get selected rowcell index" />

<SfTreeGrid @ref="TreeGrid" DataSource="@TreeGridData" IdMapping="TaskId" ParentIdMapping="ParentId" TreeColumnIndex="1">
    <TreeGridSelectionSettings Mode="SelectionMode.Cell" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Priority" HeaderText="Priority"  Width="80" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    SfTreeGrid<TreeData> TreeGrid;
    public List<TreeData> TreeGridData { get; set; }

    protected override void OnInitialized()
    {
        this.TreeGridData = TreeData.GetSelfDataSource().ToList();
    }

    public async Task TreeProps()
    {
        var treeProps = await this.TreeGrid.GetRowModel(this.TreeGrid.GetCurrentViewRecords().ToList()[0]);
        var level          = treeProps.Level;
        var expanded       = treeProps.IsExpanded;
        var childRecords   = treeProps.HasChildRecords;
    }
}
```

**`GetRowModel` return properties:**

| Property | Description |
|---|---|
| `Level` | The depth level of the row in the tree hierarchy |
| `IsExpanded` | Whether the row is currently expanded |
| `HasChildRecords` | Whether the row has child records |

---

## Row Height

The row height of tree grid rows can be customized through the `RowHeight` property. The `RowHeight` property changes the row height of the entire tree grid rows.

Set a uniform height for all rows:

```razor
<SfTreeGrid RowHeight="50" ...>
    ...
</SfTreeGrid>
```

### Customize Row Height for a Particular Row

The row height for a particular row can be customized using the `RowDataBound` event by adding a `row-height` custom class on the required row element:

```razor
<SfTreeGrid DataSource="@TreeGridData" ParentIdMapping="ParentId" IdMapping="TaskId" RowHeight="60" TreeColumnIndex="1">
    <TreeGridEvents TValue="TreeData.BusinessObject" RowDataBound="RowBound" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
    </TreeGridColumns>
</SfTreeGrid>

<style>
    .row-height {
        height: 90px !important;
    }
</style>

@code {
    public List<TreeData.BusinessObject> TreeGridData { get; set; }

    protected override void OnInitialized()
    {
        this.TreeGridData = TreeData.GetSelfDataSource().ToList();
    }

    public void RowBound(RowDataBoundEventArgs<TreeData.BusinessObject> args)
    {
        if (args.Data.TaskId == 3)
        {
            args.Row.AddClass(new string[] { "row-height" });
        }
    }
}
```

> **Note:** Use `AddClass()` with a CSS class to control per-row height. The CSS class sets the height using `!important` to override the default row height.

---

## Row Template

The `RowTemplate` has an option to customize the look and behavior of the tree grid rows. The `RowTemplate` property accepts either the **template** string or HTML elements.

```razor
<SfTreeGrid Height="335" DataSource="@TreeData" IdMapping="EmployeeID" ParentIdMapping="ParentId"
            TreeColumnIndex="0" RowHeight="83" GridLines="Syncfusion.Blazor.Grids.GridLine.Vertical">
    <TreeGridTemplates>
        <RowTemplate>
            <td style="padding-left:18px; border-bottom: 0.5px solid #e0e0e0;">
                <RowTemplateTreeColumn>
                    @{
                        var cntxt = context as Employee;
                        <div>@(cntxt.EmpID)</div>
                    }
                </RowTemplateTreeColumn>
            </td>
            <td style="padding: 10px 0px 0px 20px; border-bottom: 0.5px solid #e0e0e0;">
                <div style="font-size:14px;">
                    @((context as Employee).FullName)
                </div>
            </td>
            <td style="border-bottom: 0.5px solid #e0e0e0;">
                <div>
                    <div style="display:inline-block;">
                        <div style="padding:5px;">@((context as Employee).Address)</div>
                        <div style="padding:5px;">@((context as Employee).Country)</div>
                        <div style="padding:5px;font-size:12px;">@((context as Employee).Contact)</div>
                    </div>
                </div>
            </td>
            <td style="padding-left: 20px; border-bottom: 0.5px solid #e0e0e0;">
                <div>@((context as Employee).Designation)</div>
            </td>
        </RowTemplate>
    </TreeGridTemplates>
    <TreeGridColumns>
        <TreeGridColumn Field="EmpID"       HeaderText="Employee ID"      Width="160" />
        <TreeGridColumn Field="Name"        HeaderText="Employee Name" />
        <TreeGridColumn Field="Address"     HeaderText="Employee Details"  Width="340" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Designation" HeaderText="Designation" />
    </TreeGridColumns>
</SfTreeGrid>
```

> **Note:** When using `RowTemplate`, wrap it in `<TreeGridTemplates>`. The first column with tree expansion must use `<RowTemplateTreeColumn>`. The number of `<td>` elements must match the number of columns.

---

## Row Template with Formatting

If `RowTemplate` is used, the value cannot be formatted inside the template using the `Format` property. In that case, a function should be defined globally to format the value and invoke it inside the template.

```razor
<SfTreeGrid ID="TreeGrid" Height="335" DataSource="@TreeData" IdMapping="EmployeeID" ParentIdMapping="ParentId"
            TreeColumnIndex="0" RowHeight="83" GridLines="Syncfusion.Blazor.Grids.GridLine.Vertical">
    <TreeGridTemplates>
        <RowTemplate>
            <td style="padding-left:18px; border-bottom: 0.5px solid #e0e0e0;">
                <RowTemplateTreeColumn>
                    @{
                        var cntxt = context as Employee;
                        <div>@(cntxt.EmpID)</div>
                    }
                </RowTemplateTreeColumn>
            </td>
            <td style="padding: 10px 0px 0px 20px; border-bottom: 0.5px solid #e0e0e0;">
                <div style="font-size:14px;">
                    @((context as Employee).FullName)
                </div>
            </td>
            <td style="border-bottom: 0.5px solid #e0e0e0;">
                <div>
                    <div style="display:inline-block;">
                        <div style="padding:5px;">@((context as Employee).Address)</div>
                        <div style="padding:5px;">@((context as Employee).Country)</div>
                        <div style="padding:5px;font-size:12px;">@((context as Employee).Contact)</div>
                    </div>
                </div>
            </td>
            <td style="padding-left: 20px; border-bottom: 0.5px solid #e0e0e0;">
                <div>@Format((context as Employee).DOB)</div>
            </td>
        </RowTemplate>
    </TreeGridTemplates>
    <TreeGridColumns>
        <TreeGridColumn Field="EmpID"   HeaderText="Employee ID"      Width="160" />
        <TreeGridColumn Field="Name"    HeaderText="Employee Name" />
        <TreeGridColumn Field="Address" HeaderText="Employee Details"  Width="340" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="DOB"     HeaderText="DOB" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public IEnumerable<Employee> TreeData { get; set; }

    protected override void OnInitialized()
    {
        this.TreeData = Employee.GetTemplateData();
    }

    public object Format(DateTime? DOB)
    {
        return DOB.Value.ToLocalTime();
    }
}
```

---

## Limitations

Row template feature is not compatible with all the features which are available in the tree grid and it has limited features support. Here the features which are compatible with row template feature are listed out:

* Filtering
* Paging
* Sorting
* Scrolling
* Searching
* Rtl
* Context Menu
* State Persistence

---

## Detail Template

The detail template provides additional information about a particular row. By expanding the parent row, the child rows are expanded along with their detail template. The `DetailTemplate` property accepts either the template string or HTML elements.

```razor
<SfTreeGrid DataSource="@TreeData" TValue="TaskItem"
            IdMapping="TaskId" ParentIdMapping="ParentId" TreeColumnIndex="1">
    <TreeGridTemplates>
        <DetailTemplate>
            @{
                var task = context as TaskItem;
            }
            <div style="padding:12px; background:#f9f9f9">
                <h4>@task.TaskName — Details</h4>
                <p><strong>Description:</strong> @task.Description</p>
                <p><strong>Assigned To:</strong> @task.AssignedTo</p>
                <p><strong>Notes:</strong> @task.Notes</p>
            </div>
        </DetailTemplate>
    </TreeGridTemplates>
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="ID"        Width="80" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="220" />
        <TreeGridColumn Field="Priority" HeaderText="Priority"  Width="120" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public IEnumerable<Employee> TreeData { get; set; }

    protected override void OnInitialized()
    {
        this.TreeData = Employee.GetTemplateData();
    }
}
```

> **Note:** `DetailTemplate` must be wrapped in `<TreeGridTemplates>`. A detail expand icon appears in the leftmost column automatically. By expanding the parent row, child rows expand along with their detail template. Context is cast to the model class: `context as Employee`.

---

## Row Drag and Drop

Rows can be reordered within a TreeGrid, or dragged and dropped to another TreeGrid or custom control, by setting `AllowRowDragAndDrop` to `true`.

### Drag and Drop within Tree Grid

Row drag-and-drop enables moving rows within the same TreeGrid using the drag icon. To enable this, set `AllowRowDragAndDrop` to `true`. Rows can be dropped above, below, or as a child of the target row, based on the drop indicator.

```razor
<SfTreeGrid AllowRowDragAndDrop="true" ...>
    <TreeGridColumns>
        <!-- Optional: show drag icon column -->
        <TreeGridColumn HeaderText="" Width="40">
            <Template>
                <span class="e-icons e-drag"></span>
            </Template>
        </TreeGridColumn>
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="200" />
        ...
    </TreeGridColumns>
    <TreeGridEvents TValue="TaskItem"
                    RowDropping="OnRowDroppingHandler"
                    RowDropped="OnRowDroppedHandler" />
</SfTreeGrid>
```

> **Note:**
> - Enable selection to use row drag-and-drop.
> - For multiple-row drag, set `SelectionSettings.Type` to `Multiple`.
> - `IsPrimaryKey` must be set on a column to perform row drag-and-drop operations.

---

### Drag and Drop to Another Tree Grid

To drag and drop between two TreeGrids, enable `AllowRowDragAndDrop` on both grids and set the `TargetID` in `TreeGridRowDropSettings` to the other grid's ID.

```razor
<div style="float: left; width:49%">
    <SfTreeGrid ID="Grid" DataSource="@TreeGridData" AllowRowDragAndDrop="true"
                AllowSelection="true" AllowPaging="true"
                IdMapping="TaskId" ParentIdMapping="ParentId" TreeColumnIndex="1">
        <TreeGridSelectionSettings Type="Syncfusion.Blazor.Grids.SelectionType.Multiple" />
        <TreeGridRowDropSettings TargetID="DestGrid" />
        <TreeGridColumns>
            <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80" IsPrimaryKey="true" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
            <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
            <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        </TreeGridColumns>
    </SfTreeGrid>
</div>
<div style="float: right; width:49%">
    <SfTreeGrid ID="DestGrid" DataSource="@SecondGrid" AllowRowDragAndDrop="true"
                AllowSelection="true" AllowPaging="true"
                IdMapping="TaskId" ParentIdMapping="ParentId" TreeColumnIndex="1">
        <TreeGridSelectionSettings Type="Syncfusion.Blazor.Grids.SelectionType.Multiple" />
        <TreeGridRowDropSettings TargetID="Grid" />
        <TreeGridColumns>
            <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80" IsPrimaryKey="true" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
            <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
            <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        </TreeGridColumns>
    </SfTreeGrid>
</div>

@code {
    public List<WrapData> TreeGridData { get; set; }
    public List<WrapData> SecondGrid   { get; set; } = new List<WrapData>();

    protected override void OnInitialized()
    {
        this.TreeGridData = WrapData.GetWrapData().ToList();
    }
}
```

> **Note:** Set `IsPrimaryKey` on a column to enable row drag-and-drop operations across grids.

---

### Drag and Drop Events

The following events are triggered while dragging and dropping TreeGrid rows:

| Event | Description |
|---|---|
| `RowDragStarting` | Triggers when a row drag operation starts |
| `RowDropped` | Triggers when the dragged row is dropped on the target element |

```razor
<SfTreeGrid DataSource="@TreeGridData" AllowRowDragAndDrop="true"
            IdMapping="TaskId" ParentIdMapping="ParentId" TreeColumnIndex="1">
    <TreeGridEvents TValue="WrapData"
                    RowDragStarting="OnRowDragStarting"
                    RowDropped="OnRowDropped" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80" IsPrimaryKey="true" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="240" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    private void OnRowDragStarting(RowDragStartingEventArgs<WrapData> args)
    {
        // Fires when a row drag starts
        // args.Data contains the dragged row(s)
    }

    private void OnRowDropped(RowDroppedEventArgs<WrapData> args)
    {
        // Fires after the dragged row is dropped
        // args.Data contains the dropped row(s)
    }
}
```

---

## Row Spanning

Row spanning in the Blazor TreeGrid merges adjacent cells with identical values horizontally within the same row. This reduces visual repetition and presents grouped data in a compact, readable format. It is especially useful when multiple columns repeat the same value and can be merged.

Row spanning is enabled by setting the `AutoSpan` property of the `SfTreeGrid` component to `AutoSpanMode.Row`. When activated, the TreeGrid evaluates each row and merges adjacent cells with identical values across continuous columns. The merging process is automatic and declarative, requiring no custom logic or data transformation.

```razor
@using Syncfusion.Blazor.Grids
@using Syncfusion.Blazor.TreeGrid

<SfTreeGrid @ref="TreeGridInstance" DataSource="@TreeData" IdMapping="Id" EnableHover
            ParentIdMapping="ParentId" AutoSpan="AutoSpanMode.Row" TreeColumnIndex="1" GridLines="GridLine.Both">
    <TreeGridColumns>
        <TreeGridColumn Field="Id"    HeaderText="ID"                Width="80"  TextAlign="TextAlign.Center" IsPrimaryKey="true" />
        <TreeGridColumn Field="Name"  HeaderText="Developer"         Width="150" />
        <TreeGridColumn Field="Slot1" HeaderText="10:00 - 11:00 AM"  Width="150" />
        <TreeGridColumn Field="Slot2" HeaderText="11:00 - 12:00 AM"  Width="150" />
        <TreeGridColumn Field="Slot3" HeaderText="12:00 - 13:00 PM"  Width="150" />
        <TreeGridColumn Field="Slot4" HeaderText="01:00 - 02:00 PM"  Width="150" />
        <TreeGridColumn Field="Slot5" HeaderText="02:00 - 03:00 PM"  Width="150" />
        <TreeGridColumn Field="Slot6" HeaderText="03:00 - 04:00 PM"  Width="150" />
        <TreeGridColumn Field="Slot7" HeaderText="04:00 - 05:00 PM"  Width="150" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public SfTreeGrid<DeveloperSchedule> TreeGridInstance;
    public List<DeveloperSchedule> TreeData { get; set; }

    protected override void OnInitialized()
    {
        TreeData = DeveloperSchedule.GetTree().ToList();
    }
}
```

---

## AutoSpan Mode

Row spanning is part of the broader `AutoSpanMode` enumeration, which provides multiple options for customizing cell merging behavior in the Blazor TreeGrid. The available modes are:

| Enum Value | Description |
|---|---|
| `AutoSpanMode.None` | Disables automatic cell spanning, keeping every cell isolated (Default Mode) |
| `AutoSpanMode.Row` | Enables horizontal merging across columns within the same row |
| `AutoSpanMode.Column` | Enables vertical merging of adjacent cells with identical values in the same column |
| `AutoSpanMode.HorizontalAndVertical` | Enables both horizontal and vertical merging. Executes row merging first, followed by column merging |

---

## Disable Spanning for Specific Columns

Spanning in the Blazor TreeGrid can be disabled at the column level by setting the `AutoSpan` property of the `TreeGridColumn` component to `AutoSpanMode.None`. This configuration provides fine-grained control, allowing automatic spanning to be applied globally while excluding specific columns where merging is not desired.

```razor
<SfTreeGrid TValue="ProjectTask" @ref="TreeGridRef" DataSource="@TreeGridData"
            ChildMapping="Children" TreeColumnIndex="0" AutoSpan="AutoSpanMode.Row">
    <TreeGridColumns>
        <TreeGridColumn Field="ActivityName" HeaderText="Phase Name"     Width="250" />
        <TreeGridColumn Field="Status"       HeaderText="Status"         Width="140" TextAlign="TextAlign.Center" />
        <TreeGridColumn Field="Supervisor"   HeaderText="Supervisor"     Width="180" />
        <TreeGridColumn Field="Team"         HeaderText="Crew"           Width="200" />
        <!-- Disable spanning for these specific columns -->
        <TreeGridColumn Field="TotalBudget"  HeaderText="Planned Budget" Width="140" TextAlign="TextAlign.Right" Format="C0" AutoSpan="AutoSpanMode.None" />
        <TreeGridColumn Field="PaidToDate"   HeaderText="Actual Spend"   Width="140" TextAlign="TextAlign.Right" Format="C0" AutoSpan="AutoSpanMode.None" />
    </TreeGridColumns>
</SfTreeGrid>
```

---

## Controlling Spanning at TreeGrid and Column Levels

The spanning behavior is determined by how the TreeGrid-level and column-level `AutoSpan` settings interact. When spanning is disabled at the TreeGrid level, all spanning directions are turned off globally, and column settings cannot override this restriction. Column-level `AutoSpan` can only narrow the spanning directions permitted by the TreeGrid.

| TreeGrid AutoSpan | Column AutoSpan | Effective Behavior |
|---|---|---|
| None | None | No spanning. Both TreeGrid and column explicitly disable spanning. |
| None | Row | No spanning. TreeGrid-level disables all spanning. |
| None | Column | No spanning. TreeGrid-level disables all spanning. |
| None | HorizontalAndVertical | No spanning. TreeGrid-level disables all spanning. |
| Row | None | No spanning when all columns are set to None. Column explicitly disables spanning. |
| Row | Row | Row spanning only. Both TreeGrid and column enable row spanning. |
| Row | Column | No spanning. TreeGrid only allows row spanning; column cannot enable column spanning. |
| Row | HorizontalAndVertical | Row spanning only. TreeGrid only allows row spanning. |
| Column | None | No spanning when all columns are set to None. Column explicitly disables spanning. |
| Column | Row | No spanning. TreeGrid only allows column spanning; column cannot enable row spanning. |
| Column | Column | Column spanning only. Both TreeGrid and column enable column spanning. |
| Column | HorizontalAndVertical | Column spanning only. TreeGrid only allows column spanning. |
| HorizontalAndVertical | None | No spanning. Column explicitly disables both directions. |
| HorizontalAndVertical | Row | Row spanning only. TreeGrid allows both; column narrows to Row. |
| HorizontalAndVertical | Column | Column spanning only. TreeGrid allows both; column narrows to Column. |
| HorizontalAndVertical | HorizontalAndVertical | Row and Column spanning. Both TreeGrid and column enable both directions. |

---

## Applying Row Spanning Programmatically

In addition to automatic cell merging, the Blazor TreeGrid provides API support for manually merging cells when custom layout behavior is required. This functionality is available through the `MergeCellsAsync` method, which enables the definition of rectangular regions of cells to be merged programmatically.

Use `MergeCellsAsync` to manually merge cells by defining rectangular regions. This method supports both single and batch merging:

| Parameter | Type | Description |
|---|---|---|
| `info` | `MergeCellInfo` | Defines a single rectangular cell region to be merged |
| `infos` | `IEnumerable<MergeCellInfo>` | Specifies multiple cell regions to be merged in a batch operation |

**`MergeCellInfo` properties:**

| Property | Type | Description |
|---|---|---|
| `RowIndex` | `int` | The zero-based index of the anchor row (top-left cell of the merged region) |
| `ColumnIndex` | `int` | The zero-based index of the anchor column (top-left cell of the merged region) |
| `RowSpan` | `int` (optional) | The number of rows to span, starting from the anchor cell. Defaults to 1 |
| `ColumnSpan` | `int` (optional) | The number of columns to span, starting from the anchor cell. Defaults to 1 |

```razor
@using Syncfusion.Blazor.Grids
@using Syncfusion.Blazor.TreeGrid
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="MergeCellsAsync">Merge Cell</SfButton>
<SfButton OnClick="MergeMultipleCellsAsync">Merge Multiple Cells</SfButton>

<SfTreeGrid TValue="ProjectTask" @ref="TreeGrid" DataSource="@Tasks"
            ChildMapping="Children" TreeColumnIndex="0" AllowPaging="true" GridLines="GridLine.Both">
    <TreeGridColumns>
        <TreeGridColumn Field="@nameof(ProjectTask.ActivityName)" HeaderText="Activity Name" Width="180" />
        <TreeGridColumn Field="@nameof(ProjectTask.StartDate)"    HeaderText="Start Date"    Format="d" Type="ColumnType.Date" Width="130" />
        <TreeGridColumn Field="@nameof(ProjectTask.EndDate)"      HeaderText="End Date"      Format="d" Type="ColumnType.Date" Width="130" />
        <TreeGridColumn Field="@nameof(ProjectTask.Supervisor)"   HeaderText="Supervisor"    Width="150" />
        <TreeGridColumn Field="@nameof(ProjectTask.Progress)"     HeaderText="Progress"      Width="100" />
        <TreeGridColumn Field="@nameof(ProjectTask.Status)"       HeaderText="Status"        Width="120" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    private SfTreeGrid<ProjectTask> TreeGrid;
    public List<ProjectTask> Tasks { get; set; }

    // Merge a single cell region
    public async Task MergeCellsAsync()
    {
        await TreeGrid.MergeCellsAsync(new MergeCellInfo
        {
            RowIndex    = 1,
            ColumnIndex = 1,
            ColumnSpan  = 2,
        });
    }

    // Merge multiple cell regions in batch
    public async Task MergeMultipleCellsAsync()
    {
        await TreeGrid.MergeCellsAsync(new[]
        {
            new MergeCellInfo { RowIndex = 0, ColumnIndex = 0, ColumnSpan = 2 },
            new MergeCellInfo { RowIndex = 5, ColumnIndex = 0, ColumnSpan = 3 },
            new MergeCellInfo { RowIndex = 7, ColumnIndex = 0, ColumnSpan = 2 }
        });
    }

    protected override void OnInitialized()
    {
        Tasks = ProjectTask.GetSpanData();
    }
}
```

---

## Clearing Spanning Programmatically

The Blazor TreeGrid provides API support to manually remove merged regions when restoration of individual cells is required. This functionality is achieved using the `UnmergeCellsAsync` methods and the `UnmergeAllAsync` method.

| Method | Parameter | Type | Description |
|---|---|---|---|
| `UnmergeCellsAsync` | `info` | `UnmergeCellInfo` | Removes a single merged area identified by its anchor cell (top-left of the merged region) |
| `UnmergeCellsAsync` | `infos` | `IEnumerable<UnmergeCellInfo>` | Removes multiple merged areas in one combined operation, improving performance by reducing re-renders |
| `UnmergeAllAsync` | — | — | Removes all merged regions in the current view, restoring every cell to its original state |

**`UnmergeCellInfo` properties:**

| Property | Type | Description |
|---|---|---|
| `RowIndex` | `int` | The zero-based index of the anchor row (top-left cell of the merged region) |
| `ColumnIndex` | `int` | The zero-based index of the anchor column (top-left cell of the merged region) |

```razor
@using Syncfusion.Blazor.Grids
@using Syncfusion.Blazor.TreeGrid
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="MergeCellsAsync">Merge Cell</SfButton>
<SfButton OnClick="UnMergeCell">UnMerge Cell</SfButton>
<SfButton OnClick="MergeMultipleCellsAsync">Merge Multiple Cells</SfButton>
<SfButton OnClick="UnMergeCells">UnMerge Multiple Cells</SfButton>
<SfButton OnClick="UnMergeAllCells">UnMerge All Cells</SfButton>

<SfTreeGrid TValue="ProjectTask" @ref="TreeGrid" DataSource="@Tasks"
            ChildMapping="Children" TreeColumnIndex="0" AllowPaging="true" GridLines="GridLine.Both">
    <TreeGridColumns>
        <TreeGridColumn Field="@nameof(ProjectTask.ActivityName)" HeaderText="Activity Name" Width="180" />
        <TreeGridColumn Field="@nameof(ProjectTask.StartDate)"    HeaderText="Start Date"    Format="d" Type="ColumnType.Date" Width="130" />
        <TreeGridColumn Field="@nameof(ProjectTask.EndDate)"      HeaderText="End Date"      Format="d" Type="ColumnType.Date" Width="130" />
        <TreeGridColumn Field="@nameof(ProjectTask.Supervisor)"   HeaderText="Supervisor"    Width="150" />
        <TreeGridColumn Field="@nameof(ProjectTask.Progress)"     HeaderText="Progress"      Width="100" />
        <TreeGridColumn Field="@nameof(ProjectTask.Status)"       HeaderText="Status"        Width="120" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    private SfTreeGrid<ProjectTask> TreeGrid;
    public List<ProjectTask> Tasks { get; set; }

    public async Task MergeCellsAsync()
    {
        await TreeGrid.MergeCellsAsync(new MergeCellInfo
        {
            RowIndex    = 1,
            ColumnIndex = 1,
            ColumnSpan  = 2,
        });
    }

    public async Task UnMergeCell()
    {
        await TreeGrid.UnmergeCellsAsync(new UnmergeCellInfo
        {
            RowIndex    = 1,
            ColumnIndex = 1,
        });
    }

    public async Task MergeMultipleCellsAsync()
    {
        await TreeGrid.MergeCellsAsync(new[]
        {
            new MergeCellInfo { RowIndex = 0, ColumnIndex = 0, ColumnSpan = 2 },
            new MergeCellInfo { RowIndex = 5, ColumnIndex = 0, ColumnSpan = 3 },
            new MergeCellInfo { RowIndex = 7, ColumnIndex = 0, ColumnSpan = 2 }
        });
    }

    public async Task UnMergeCells()
    {
        await TreeGrid.UnmergeCellsAsync(new[]
        {
            new UnmergeCellInfo { RowIndex = 0, ColumnIndex = 0 },
            new UnmergeCellInfo { RowIndex = 5, ColumnIndex = 0 },
            new UnmergeCellInfo { RowIndex = 7, ColumnIndex = 0 }
        });
    }

    public async Task UnMergeAllCells()
    {
        await TreeGrid.UnmergeAllAsync();
    }

    protected override void OnInitialized()
    {
        Tasks = ProjectTask.GetSpanData();
    }
}
```

---

## Expand and Collapse

**Expand/collapse events:**

> ⚠️ **All four expand/collapse event args are generic and require the model type argument:**
> - `RowExpandingEventArgs<TValue>` — fires before expand (cancellable)
> - `RowExpandedEventArgs<TValue>` — fires after expand
> - `RowCollapsingEventArgs<TValue>` — fires before collapse (cancellable)
> - `RowCollapsedEventArgs<TValue>` — fires after collapse
>
> **Do NOT use the non-generic forms** (`RowExpandingEventArgs`, `RowExpandedEventArgs`, etc.) — they will cause a compile error: *"requires 1 type argument"*.

**Event args key properties:**

| Property | Available On | Description |
|---|---|---|
| `args.Data` | All four events | The record object (`TValue`) of the expanding/collapsing row |
| `args.Cancel` | `RowExpandingEventArgs<T>`, `RowCollapsingEventArgs<T>` | Set `true` to prevent the expand/collapse |
| `args.ExpandAll` | `RowExpandingEventArgs<T>` | Set `true` to auto-expand all child rows when this row expands |

```razor
<SfTreeGrid @ref="TreeRef" ...>
    <TreeGridEvents TValue="TaskItem"
                    Expanding="OnExpandingHandler"
                    Expanded="OnExpandedHandler"
                    Collapsing="OnCollapsingHandler"
                    Collapsed="OnCollapsedHandler" />
</SfTreeGrid>

@code {
    private SfTreeGrid<TaskItem> TreeRef;

    /// <summary>
    /// Fires BEFORE a row expands. (Cancellable)
    /// ✅ Correct: RowExpandingEventArgs&lt;TaskItem&gt; — generic type required
    /// ❌ Wrong:   RowExpandingEventArgs               — causes CS0305 compile error
    /// </summary>
    private void OnExpandingHandler(RowExpandingEventArgs<TaskItem> args)
    {
        // Access the expanding row's data
        var taskName = args.Data?.TaskName;

        // Auto-expand all child rows when this row expands
        args.ExpandAll = true;

        // Cancel expand if needed
        // args.Cancel = true;
    }

    /// <summary>
    /// Fires AFTER a row expands.
    /// ✅ Correct: RowExpandedEventArgs&lt;TaskItem&gt; — generic type required
    /// </summary>
    private void OnExpandedHandler(RowExpandedEventArgs<TaskItem> args)
    {
        var taskName = args.Data?.TaskName;
        // Post-expand logic here
    }

    /// <summary>
    /// Fires BEFORE a row collapses. (Cancellable)
    /// ✅ Correct: RowCollapsingEventArgs&lt;TaskItem&gt; — generic type required
    /// ❌ Wrong:   RowCollapsingEventArgs               — causes CS0305 compile error
    /// </summary>
    private void OnCollapsingHandler(RowCollapsingEventArgs<TaskItem> args)
    {
        var taskName = args.Data?.TaskName;

        // Cancel collapse if needed
        // args.Cancel = true;
    }

    /// <summary>
    /// Fires AFTER a row collapses.
    /// ✅ Correct: RowCollapsedEventArgs&lt;TaskItem&gt; — generic type required
    /// </summary>
    private void OnCollapsedHandler(RowCollapsedEventArgs<TaskItem> args)
    {
        var taskName = args.Data?.TaskName;
        // Post-collapse logic here
    }
}
```

**Programmatically expand/collapse rows:**

### Public Method Reference

| Method | Parameter | Description |
|---|---|---|
| `ExpandAllAsync()` | — | Expands every parent row in the grid |
| `CollapseAllAsync()` | — | Collapses every parent row in the grid |
| `ExpandAtLevelAsync(level)` | `int` level (1-based depth) | Expands rows up to the specified depth level |
| `CollapseAtLevelAsync(level)` | `int` level (1-based depth) | Collapses rows from the specified depth level |
| `ExpandRowAsync(record)` | **Record object** (`TValue`) | Expands a specific row — pass the **full record object** |
| `CollapseRowAsync(record)` | **Record object** (`TValue`) | Collapses a specific row — pass the **full record object** |
| `ExpandByKeyAsync(key)` | **Primary key value** | Expands a specific row — pass the **primary key value** (e.g. TaskId) |
| `CollapseByKeyAsync(key)` | **Primary key value** | Collapses a specific row — pass the **primary key value** (e.g. TaskId) |

> ⚠️ **Critical distinction:**
> - `ExpandRowAsync` / `CollapseRowAsync` — accept the **record object** (`TaskItem` instance)
> - `ExpandByKeyAsync` / `CollapseByKeyAsync` — accept the **primary key value** (e.g. `int TaskId`)
> - Do **NOT** pass `TaskId` to `ExpandRowAsync` or the record object to `ExpandByKeyAsync`

```razor
@code {
    private SfTreeGrid<TaskItem> TreeRef;
    private TaskItem SelectedRecord;

    // ── Expand / Collapse ALL ──────────────────────────────────────────────
    private async Task ExpandAll()   => await TreeRef.ExpandAllAsync();
    private async Task CollapseAll() => await TreeRef.CollapseAllAsync();

    // ── Expand / Collapse BY LEVEL ─────────────────────────────────────────
    // level is 1-based: 1 = first children of root, 2 = grandchildren, etc.
    private async Task ExpandToLevel1()  => await TreeRef.ExpandAtLevelAsync(1);
    private async Task ExpandToLevel2()  => await TreeRef.ExpandAtLevelAsync(2);
    private async Task CollapseLevel1()  => await TreeRef.CollapseAtLevelAsync(1);

    // ── Expand / Collapse BY RECORD OBJECT ────────────────────────────────
    // ✅ Pass the full record object (TaskItem) — NOT the primary key value
    private async Task ExpandRow()   => await TreeRef.ExpandRowAsync(SelectedRecord);
    private async Task CollapseRow() => await TreeRef.CollapseRowAsync(SelectedRecord);

    // ── Expand / Collapse BY PRIMARY KEY VALUE ─────────────────────────────
    // ✅ Pass the primary key VALUE (e.g. TaskId int) — NOT the record object
    private async Task ExpandByKey()   => await TreeRef.ExpandByKeyAsync(SelectedRecord.TaskId);
    private async Task CollapseByKey() => await TreeRef.CollapseByKeyAsync(SelectedRecord.TaskId);
}
```

**Initial expanded state:**
```razor
<SfTreeGrid ExpandStateMapping="IsExpanded" ...>
    ...
</SfTreeGrid>

@code {
    // Add IsExpanded property to model to control initial expand state per row
    public class TaskItem
    {
        public int  TaskId     { get; set; }
        public int? ParentId   { get; set; }
        public string TaskName { get; set; }
        public bool IsExpanded { get; set; } = true; // default expanded
    }
}
```

---
