# Columns in Syncfusion Blazor TreeGrid

> **Namespace note:** `TextAlign`, `ColumnType`, `EditType`, and `ClipMode` belong to `Syncfusion.Blazor.Grids`, **not** `Syncfusion.Blazor.TreeGrid`. Always qualify them with the full namespace (e.g., `Syncfusion.Blazor.Grids.TextAlign.Right`, `Syncfusion.Blazor.Grids.ColumnType.Date`) to avoid CS0104 ambiguity errors.

## Table of Contents
- [Basic Column Definition](#basic-column-definition)
- [Column Properties Reference](#column-properties-reference)
- [Complex Data Binding](#complex-data-binding)
- [Column Format](#column-format)
- [Column Type](#column-type)
- [Checkbox Column](#checkbox-column)
- [Stacked Headers](#stacked-headers)
- [Column Template](#column-template)
- [Header Template](#header-template)
- [Header Text](#header-text)
- [Column Chooser](#column-chooser)
- [Column Menu](#column-menu)
- [Column Reorder](#column-reorder)
- [Column Resizing](#column-resizing)
- [Column Spanning](#column-spanning)
- [Auto-Fit Columns](#auto-fit-columns)
- [Freeze Columns](#freeze-columns)
- [Responsive Columns](#responsive-columns)
- [Show or Hide Columns by External Button](#show-or-hide-columns-by-external-button)

---

## Basic Column Definition

Column definitions act as the data source schema for the TreeGrid and determine how values render. TreeGrid operations such as sorting, filtering, and searching operate based on the column definitions. The `Field` property of the `TreeGridColumn` component is required to map data source values to TreeGrid columns.

> If the column `Field` does not match a property in the data source, the column cells render empty. The `TreeColumnIndex` property denotes the column used to expand and collapse child rows.

```razor
<SfTreeGrid DataSource="@Data" IdMapping="Id" ParentIdMapping="ParentId" TreeColumnIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="Id"       HeaderText="Task ID"   Width="80"
                        IsPrimaryKey="true"
                        TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Name"     HeaderText="Task Name" Width="200" />
        <TreeGridColumn Field="Status"   HeaderText="Status"    Width="120" />
        <TreeGridColumn Field="Progress" HeaderText="Progress%" Width="100"
                        Format="P0"
                        TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
    </TreeGridColumns>
</SfTreeGrid>
```

---

## Column Properties Reference

| Property | Type | Description |
|---|---|---|
| `Field` | string | Maps to the data source property (required) |
| `HeaderText` | string | Column header label (defaults to Field name) |
| `Width` | string/int | Column width (px or %) |
| `MinWidth` | string | Minimum width (used with resizing) |
| `MaxWidth` | string | Maximum width |
| `TextAlign` | `Syncfusion.Blazor.Grids.TextAlign` | Left / Center / Right / Justify |
| `HeaderTextAlign` | `Syncfusion.Blazor.Grids.TextAlign` | Header-only text alignment |
| `Type` | `Syncfusion.Blazor.Grids.ColumnType` | String, Integer, Decimal, Double, Long, Boolean, Date, DateTime, DateOnly, TimeOnly, CheckBox, None |
| `Format` | string | Value format string (e.g., `"d"`, `"C2"`, `"P0"`) |
| `IsPrimaryKey` | bool | Marks primary key column (required for editing) |
| `Visible` | bool | Show/hide the column |
| `AllowSorting` | bool | Enable/disable sort for this column |
| `AllowFiltering` | bool | Enable/disable filter for this column |
| `AllowEditing` | bool | Enable/disable editing for this column |
| `AllowReordering` | bool | Allow drag-reorder for this column |
| `AllowResizing` | bool | Allow resize handle for this column |
| `ClipMode` | ClipMode | EllipsisWithTooltip / Clip / Ellipsis |
| `CustomAttributes` | object | Add HTML attributes to column cells |
| `ValueAccessor` | Func | Custom value transform function |
| `DisableHtmlEncode` | bool | Render HTML content in cells (see cell.md) |
| `ShowCheckbox` | bool | Renders checkboxes in the existing column |
| `AutoCheckHierarchy` | bool | Enables hierarchical checkbox selection |
| `DisplayAsCheckBox` | bool | Renders boolean values as checkboxes |
| `LockColumn` | bool | Locks column to first position; disables reordering |
| `HideAtMedia` | string | CSS media query to toggle column visibility responsively |
| `ShowInColumnChooser` | bool | Include/exclude column from the column chooser dialog |
| `ShowColumnMenu` | bool | Enable/disable column menu for this column |
| `AutoSpan` | `AutoSpanMode` | Controls cell spanning behavior for this column |
| `IsFrozen` | bool | Freezes the column in place |
| `Freeze` | `FreezeDirection` | Direction to freeze the column (Left/Right) |

---

## Complex Data Binding

Complex data binding can be achieved by using the dot (`.`) operator in the column `Field`. In the following example, **Task.TaskName** and **Task.Duration** are complex data fields.

```razor
<TreeGridColumn Field="Task.TaskName" HeaderText="Task Name" Width="160" />
<TreeGridColumn Field="Task.Duration" HeaderText="Duration"  Width="100" />
```

```csharp
public class BusinessObject
{
    public int         TaskId   { get; set; }
    public int?        ParentId { get; set; }
    public TaskDetails Task     { get; set; }   // nested object
    public int         Progress { get; set; }
    public string      Priority { get; set; }
}

public class TaskDetails
{
    public string TaskName { get; set; }
    public int    Duration { get; set; }
}
```

### Expando data binding

TreeGrid supports complex data binding with `ExpandoObject`. **Task.TaskName** and **Task.Duration** are complex data fields using ExpandoObject.

```razor
<SfTreeGrid DataSource="@TreeData" AllowPaging="true" IdMapping="TaskID" ParentIdMapping="ParentID" TreeColumnIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskID" HeaderText="Task ID" Width="80"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="Task.TaskName" HeaderText="Task Name" Width="160"></TreeGridColumn>
        <TreeGridColumn Field="Task.Duration" HeaderText="Duration" Width="160"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="Progress" HeaderText="Progress" Width="100"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="Priority" HeaderText="Priority" Width="80"></TreeGridColumn>
    </TreeGridColumns>
</SfTreeGrid>
```

---

## Column Format

To format cell values based on culture, use the `Format` property of the `TreeGridColumn` component. The TreeGrid uses the **Internationalization** library to format number values.

```razor
<!-- Currency -->
<TreeGridColumn Field="Price" HeaderText="Price" Format="C2"
    Type="Syncfusion.Blazor.Grids.ColumnType.Integer"
    TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
<!-- Percentage -->
<TreeGridColumn Field="Progress" HeaderText="Progress" Format="P0"
    Type="Syncfusion.Blazor.Grids.ColumnType.Integer"
    TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
<!-- Short date -->
<TreeGridColumn Field="StartDate" HeaderText="Start Date" Format="yMd"
    Type="Syncfusion.Blazor.Grids.ColumnType.Date" />
<!-- Long date/time -->
<TreeGridColumn Field="OrderDate" HeaderText="Order Date" Format="dd/MM/yyyy HH:mm"
    Type="Syncfusion.Blazor.Grids.ColumnType.DateTime" />
```

> By default, number and date values are formatted using the **en-US** locale.

### Number formatting

| Format | Description | Remarks |
|--------|-------------|---------|
| `N` | Denotes numeric type | Follow with a precision specifier, such as `N2` or `N3`, to control decimal places |
| `C` | Denotes currency type | Follow with a precision specifier, such as `C2` or `C3`, to control decimal places |
| `P` | Denotes percentage type | Expects input from 0 to 1. For example, 0.2 formats as 20%. Follow with a precision specifier such as `P2` or `P3` |

### Date formatting

Use built-in date format strings to format date values. For built-in date formats, specify the `Format` property as a string (e.g., `"d"`, `"yMd"`).

```razor
<TreeGridColumn Field="OrderDate" HeaderText="Order Date" Format="yMd"
    Type="Syncfusion.Blazor.Grids.ColumnType.Date" Width="160" />
```

---

## Column Type

Column type can be specified using the `Type` property of `TreeGridColumn`. It specifies the type of data the column binds. If the `Format` is defined for a column, the column uses `Type` to select the appropriate format option (**number** or **date**).

Always set `Type` for non-string columns so filtering and sorting work correctly.

> **Important:** If the `Type` is not defined, it will be determined from the first record of the `DataSource`. Syncfusion Blazor TreeGrid only supports `ColumnType.Integer` for numeric columns. Do not use `ColumnType.Number`.

```razor
<TreeGridColumn Field="TaskId"    Type="Syncfusion.Blazor.Grids.ColumnType.Integer" />
<TreeGridColumn Field="StartDate" Type="Syncfusion.Blazor.Grids.ColumnType.Date" />
<TreeGridColumn Field="IsActive"  Type="Syncfusion.Blazor.Grids.ColumnType.Boolean" />
```

| Column Type | Description |
|---|---|
| `String` | Represents text data. This is the default type when `Type` is not explicitly defined. |
| `Decimal` | Displays decimal numeric values. |
| `Double` | Displays double-precision floating-point values. |
| `Integer` | Represents integer numeric values. |
| `Long` | Represents long integer values. |
| `None` | Indicates no specific data type. |
| `Boolean` | Displays boolean values, typically rendered by default as text (true/false) or as checkboxes when `DisplayAsCheckBox` is enabled. |
| `Date` | Displays date values with comprehensive formatting support. |
| `DateTime` | Displays date and time values, offering various formatting options. |
| `DateOnly` | Represents `DateOnly` values (available in .NET 7 and later). |
| `TimeOnly` | Represents `TimeOnly` values (available in .NET 7 and later). |
| `CheckBox` | Renders a checkbox within the column, primarily used for row selection. |

**Example of using Integer for numeric columns:**

```razor
<!-- Currency values - use Integer with Format="C0" -->
<TreeGridColumn Field="PlannedBudget" HeaderText="Budget" 
                Type="Syncfusion.Blazor.Grids.ColumnType.Integer" Format="C0"
                TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />

<!-- Percentage values - use Integer with Format="P0" -->
<TreeGridColumn Field="Progress" HeaderText="Progress" 
                Type="Syncfusion.Blazor.Grids.ColumnType.Integer" Format="P0"
                TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />

<!-- Plain integers -->
<TreeGridColumn Field="Duration" HeaderText="Duration (Days)" 
                Type="Syncfusion.Blazor.Grids.ColumnType.Integer"
                TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
```

---

## Checkbox Column

To render checkboxes in the existing column, set the `ShowCheckbox` property of the `TreeGridColumn` as **true**.

It is also possible to select the rows hierarchically using checkboxes in the Tree Grid by enabling the `AutoCheckHierarchy` property. When a parent record checkbox is checked, all child record checkboxes will also get checked.

```razor
<SfTreeGrid IdMapping="TaskId" ParentIdMapping="ParentId" DataSource="@TreeGridData"
            TreeColumnIndex="1" AutoCheckHierarchy="true">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="Task ID" Width="80"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" ShowCheckbox="true" Width="90"></TreeGridColumn>
        <TreeGridColumn Field="StartDate" HeaderText="Start Date"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"
            Format="yMd" Type="Syncfusion.Blazor.Grids.ColumnType.Date" Width="90"></TreeGridColumn>
        <TreeGridColumn Field="Duration" HeaderText="Duration"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" Width="80"></TreeGridColumn>
        <TreeGridColumn Field="Progress" HeaderText="Progress"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" Width="80"></TreeGridColumn>
    </TreeGridColumns>
</SfTreeGrid>
```


---

## Stacked Headers

Group related columns under a shared header row using nested `TreeGridColumns`:

```razor
<TreeGridColumns>
    <TreeGridColumn HeaderText="Order Details" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right">
        <TreeGridColumns>
            <TreeGridColumn Field="OrderId"   HeaderText="Order ID"   Width="110"
                TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
            <TreeGridColumn Field="OrderName" HeaderText="Order Name" Width="220"
                TextAlign="Syncfusion.Blazor.Grids.TextAlign.Left" />
            <TreeGridColumn Field="OrderDate" HeaderText="Order Date" Width="120"
                TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"
                Format="yMd" Type="Syncfusion.Blazor.Grids.ColumnType.Date" />
        </TreeGridColumns>
    </TreeGridColumn>
    <TreeGridColumn HeaderText="Shipment Details" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Center">
        <TreeGridColumns>
            <TreeGridColumn Field="ShipmentCategory" HeaderText="Shipment Category" Width="170" />
            <TreeGridColumn Field="Units"             HeaderText="Units"             Width="220"
                TextAlign="Syncfusion.Blazor.Grids.TextAlign.Left" />
            <TreeGridColumn Field="ShippedDate"      HeaderText="Shipment Date"     Width="120"
                TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"
                Format="yMd" Type="Syncfusion.Blazor.Grids.ColumnType.Date" />
        </TreeGridColumns>
    </TreeGridColumn>
</TreeGridColumns>
```

---

## Column Template

The Column template has options to display custom element value or content in the column. You can use the `Template` of the `TreeGridColumn` component to specify the custom content. Inside the `Template`, you can access the data using the implicit named parameter **context**.

> The column template feature is used to render the customized element value in the UI for a particular column. Data operations like filtering, sorting, etc., will not work based on the column template values. It will be handled based on the values you have provided to the particular column in the datasource.

```razor
<SfTreeGrid DataSource="@TreeData" IdMapping="EmployeeID" ParentIdMapping="ParentId" TreeColumnIndex="0">
    <TreeGridColumns>
        <TreeGridColumn Field="Name" HeaderText="Name" Width="160"></TreeGridColumn>
        <TreeGridColumn HeaderText="Employee Image" Width="140">
            <Template>
                @{
                    var employee = (context as Employee);
                    <div class="image">
                        <img src="@UriHelper.ToAbsoluteUri($"images/TreeGrid/{employee.Name}.png")"
                             alt="@employee.EmployeeID" />
                    </div>
                }
            </Template>
        </TreeGridColumn>
        <TreeGridColumn Field="Designation" HeaderText="Designation" Width="170"></TreeGridColumn>
        <TreeGridColumn Field="EmpID" HeaderText="Employee ID" Width="120"></TreeGridColumn>
    </TreeGridColumns>
</SfTreeGrid>
```

> Tree Grid actions such as editing, filtering, and sorting depend upon the column `Field`. If the `Field` is not specified in the template column, the tree grid actions cannot be performed.

### Using conditions inside template

Template elements can be rendered based on conditions inside the `Template` of the `TreeGridColumn` component.

```razor
<TreeGridColumn HeaderText="Template Column" Width="90">
    <Template>
        @{
            var row = (context as SelfReferenceData);
            @if (row.Duration > 1)
            {
                <SfCheckBox Checked="true"></SfCheckBox>
            }
            else
            {
                <SfCheckBox Checked="false"></SfCheckBox>
            }
        }
    </Template>
</TreeGridColumn>
```

### Using hyperlink column

You can use the `Template` property of the `TreeGridColumn` to render hyperlinks and use `NavigationManager` for routing.

```razor
<TreeGridColumn HeaderText="User profile" TextAlign="TextAlign.Center" Width="120">
    <Template>
        @{
            var Employee = (context as EmployeeData);
            <div><a href="#" @onclick="@(() => Navigate(Employee))">View</a></div>
        }
    </Template>
</TreeGridColumn>

@code {
    private void Navigate(EmployeeData Employee)
    {
        UriHelper.NavigateTo($"{ Employee.Link}/{Employee.EmployeeID.ToString()}/{Employee.Name}/{ Employee.Title}");
    }
}
```

---

## Header Template

Customize the header element using the `HeaderTemplate` property:

```razor
<TreeGridColumn Field="Name" HeaderText="Name" Width="160">
    <HeaderTemplate>
        <div>
            <span class="e-icon-userlogin e-icons employee"></span> Name
        </div>
    </HeaderTemplate>
</TreeGridColumn>

<TreeGridColumn Field="Priority" Width="120">
    <HeaderTemplate>
        <span>⚑ Priority</span>
    </HeaderTemplate>
</TreeGridColumn>
```

---

## Column Chooser

The column chooser has options to show or hide columns dynamically. It can be enabled by defining the `ShowColumnChooser` property as **true** and adding `"ColumnChooser"` to the `Toolbar`.

```razor
<SfTreeGrid IdMapping="TaskId" ParentIdMapping="ParentId" DataSource="@TreeGridData"
            TreeColumnIndex="1" ShowColumnChooser="true" Toolbar="@ToolbarItems">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"    HeaderText="Task ID"    Width="80"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="TaskName"  HeaderText="Task Name"  Width="90"
            ShowInColumnChooser="false"></TreeGridColumn>
        <TreeGridColumn Field="StartDate" HeaderText="Start Date"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"
            Format="yMd" Type="Syncfusion.Blazor.Grids.ColumnType.Date" Width="90"></TreeGridColumn>
        <TreeGridColumn Field="Duration"  HeaderText="Duration"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" Width="80"></TreeGridColumn>
        <TreeGridColumn Field="Progress"  HeaderText="Progress"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" Width="80"></TreeGridColumn>
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public string[] ToolbarItems = new string[] { "ColumnChooser" };
}
```

> Column names can be hidden in the column chooser by defining the `ShowInColumnChooser` property as **false**.

### Open column chooser by external button

The column chooser can be opened programmatically using the `OpenColumnChooserAsync` method:

```csharp
public async Task Show()
{
    await this.TreeGrid.OpenColumnChooserAsync(200, 50);
}
```

### Text wrapping in column chooser

Allow long column names to wrap across multiple lines in the column chooser dialog by setting `AllowTextWrap` to **true** on `TreeGridColumnChooserSettings`:

```razor
<SfTreeGrid ShowColumnChooser="true" Toolbar="@ToolbarItems">
    <TreeGridColumnChooserSettings AllowTextWrap="true"></TreeGridColumnChooserSettings>
    <TreeGridColumns>
        ...
    </TreeGridColumns>
</SfTreeGrid>
```

### Template support in column chooser

Customize the column chooser using `Template` and `FooterTemplate` of the `TreeGridColumnChooserSettings` component. Use `TreeGridColumnChooserItem` inside the `Template` and access `ColumnChooserTemplateContext` for column details.

```razor
<SfTreeGrid ShowColumnChooser="true" Toolbar="@(new List<string>() { "ColumnChooser" })">
    <TreeGridColumnChooserSettings>
        <Template>
            @{
                var cxt = context as ColumnChooserTemplateContext;
                @foreach (var column in cxt.Columns)
                {
                    <TreeGridColumnChooserItem Column="column"></TreeGridColumnChooserItem>
                }
            }
        </Template>
        <FooterTemplate>
            @{
                var cxt = context as ColumnChooserFooterTemplateContext;
                var visibles = cxt.Columns.Where(x => x.Visible).Select(x => x.HeaderText).ToArray();
                var hiddens  = cxt.Columns.Where(x => !x.Visible).Select(x => x.HeaderText).ToArray();
            }
            <SfButton IsPrimary="true" OnClick="@(async () => {
                await TreeGrid.ShowColumnsAsync(visibles);
                await TreeGrid.HideColumnsAsync(hiddens); })">Ok</SfButton>
            <SfButton @onclick="@(async () => await cxt.CancelAsync())">Cancel</SfButton>
        </FooterTemplate>
    </TreeGridColumnChooserSettings>
    ...
</SfTreeGrid>
```

### Column chooser with group template

Group columns inside the column chooser by wrapping `TreeGridColumnChooserItem` inside `TreeGridColumnChooserItemGroup`.

### Programmatic column access

⚠️ **CRITICAL:** When accessing columns programmatically, use `GetColumnsAsync()` (async method), **NOT** `GetColumns()`. The synchronous method does not exist in Syncfusion Blazor TreeGrid.

```csharp
// ✅ CORRECT - Use async/await
private async Task ToggleColumnVisibility(string fieldName)
{
    var columns = await TreeGridRef.GetColumnsAsync();  // ✅ MUST await GetColumnsAsync()
    var column = columns.FirstOrDefault(col => col.Field == fieldName);
    if (column != null)
    {
        column.Visible = !column.Visible;
        await TreeGridRef.RefreshAsync();
    }
}

// ❌ WRONG - Do NOT use GetColumns()
// var columns = TreeGridRef.GetColumns();  // ERROR: This method does not exist
```

Other async methods for columns:
```csharp
await TreeGridRef.GetColumnsAsync();                                            // Get all columns
await TreeGridRef.ShowColumnsAsync(new string[] { "Task ID", "Duration" });    // Show columns by HeaderText
await TreeGridRef.HideColumnsAsync(new string[] { "Task ID", "Duration" });    // Hide columns by HeaderText
await TreeGridRef.ReorderColumnsAsync(new List<string> { "Field1" }, "Field2"); // Reorder columns
await TreeGridRef.AutoFitColumnsAsync(new string[] { });                        // Auto-fit all columns
await TreeGridRef.AutoFitColumnsAsync(new string[] { "TaskName" });             // Auto-fit specific columns
```

---

## Column Menu

The column menu has options to integrate features like sorting, filtering, and autofit. It shows a menu with the integrated feature when users click on an icon in the column header. To enable column menu, set the `ShowColumnMenu` property as **true**.

```razor
<SfTreeGrid IdMapping="TaskId" ParentIdMapping="ParentId" DataSource="@TreeGridData"
            TreeColumnIndex="1" ShowColumnMenu="true" AllowFiltering="true"
            AllowSorting="true" AllowResizing="true">
    <TreeGridFilterSettings Type="Syncfusion.Blazor.TreeGrid.FilterType.Menu" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" MinWidth="170" MaxWidth="250" Width="180"></TreeGridColumn>
        <TreeGridColumn Field="Duration" HeaderText="Duration"  MinWidth="50" MaxWidth="150"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" Width="80"></TreeGridColumn>
        <TreeGridColumn Field="Progress" HeaderText="Progress"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" Width="80"></TreeGridColumn>
    </TreeGridColumns>
</SfTreeGrid>
```

**Default Column Menu Items** (automatically enabled):

| Item | Description |
|------|-------------|
| `SortAscending` | Sort the current column in ascending order |
| `SortDescending` | Sort the current column in descending order |
| `AutoFit` | Auto fit the current column |
| `AutoFitAll` | Auto fit all columns |
| `Filter` | Show the filter option as given in `FilterSettings.Type` |

Disable column menu for specific columns:
```razor
<TreeGridColumn Field="Id" ShowColumnMenu="false" />
```

> The column menu can be disabled for a particular column by defining the `ShowColumnMenu` property of the `TreeGridColumn` as **false**.

### Column Menu Event Handling

⚠️ **CRITICAL:** Use the correct event args type `ColumnMenuClickEventArgs`, NOT `ColumnMenuItemClickEventArgs`.

```csharp
// ✅ CORRECT - Use ColumnMenuClickEventArgs
private async Task OnColumnMenuItemClicked(ColumnMenuClickEventArgs args)
{
    var columnName = args.Column?.HeaderText;
    var menuItemId = args.Item?.Id;
    var menuItemText = args.Item?.Text;
    
    // Handle menu item click
    if(args.Item?.Id == "SortAscending")
    {
        // Sorting ascending logic
    }
    else if(args.Item?.Id == "SortDescending")
    {
        // Sorting descending logic
    }
}

// ❌ WRONG - Do NOT use ColumnMenuItemClickEventArgs
// private async Task OnColumnMenuItemClicked(ColumnMenuItemClickEventArgs args)  // ERROR: Type doesn't exist
```

Available properties in `ColumnMenuClickEventArgs`:
- `Column` – The column on which menu is clicked
- `Item` – The menu item that was clicked
- `Element` – The menu element reference

---

## Column Reorder

Reordering can be done by drag and drop of a particular column header from one index to another index within the tree grid. To enable reordering, set the `AllowReordering` property to **true**.

```razor
<SfTreeGrid IdMapping="TaskId" ParentIdMapping="ParentId" AllowReordering="true"
            DataSource="@TreeGridData" TreeColumnIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" MinWidth="170" MaxWidth="250" Width="180"></TreeGridColumn>
        <TreeGridColumn Field="Duration" HeaderText="Duration"  MinWidth="50" MaxWidth="150"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" Width="80"></TreeGridColumn>
        <TreeGridColumn Field="Progress" HeaderText="Progress"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" Width="80"></TreeGridColumn>
    </TreeGridColumns>
</SfTreeGrid>
```

> You can disable reordering a particular column by setting the `AllowReordering` property of the `TreeGridColumn` to **false**.

### Reorder single column

```csharp
private async Task ReorderColumn()
{
    await TreeGrid.ReorderColumnsAsync(new List<string>() { "TaskName" }, "Duration");
}
```

### Reorder multiple columns

```csharp
private async Task ReorderMultipleColumns()
{
    await TreeGrid.ReorderColumnsAsync(new List<string>() { "TaskName", "Duration" }, "Progress");
}
```

### Programmatic Column Reordering

⚠️ **CRITICAL:** Use `ReorderColumnsAsync()` with a `List<string>` of fields, NOT `ReorderColumnAsync()` with two strings.

```csharp
// ✅ CORRECT - Use ReorderColumnsAsync with List<string>
await TreeGridRef.ReorderColumnsAsync(new List<string> { "TaskName" }, "Progress");
await TreeGridRef.ReorderColumnsAsync(new List<string> { "Name", "Email" }, "Salary");

// ❌ WRONG - Do NOT use ReorderColumnAsync or pass plain strings
// await TreeGridRef.ReorderColumnAsync("TaskName", "Progress");  // ERROR: Wrong API
```

Available event args:
- **ColumnReorderingEventArgs** – Fired before reorder (can cancel via `Cancel` property)
  - `Column` – Column being reordered
  - `ColumnIndex` – Target index
  - `Cancel` – Set to true to prevent reordering
- **ColumnReorderedEventArgs** – Fired after reorder completes
  - `Column` – Column that was reordered
  - `ColumnIndex` – New column index

---

## Column Resizing

Column width can be resized by clicking and dragging the right edge of the column header. Each column can be auto resized by double-clicking the right edge of the column header to fit the width of that column based on the widest cell content. To enable column resize, set the `AllowResizing` property to **true**.

```razor
<SfTreeGrid AllowResizing="true">
    ...
</SfTreeGrid>
```

> You can disable resizing for a particular column by setting the `AllowResizing` property of the `TreeGridColumn` component to **false**. In RTL mode, you can click and drag the left edge of the header cell to resize the column.

### Min and max width

Column resize can be restricted between minimum and maximum width by defining the `MinWidth` and `MaxWidth` properties in `TreeGridColumn`:

```razor
<TreeGridColumn Field="TaskName" HeaderText="Task Name" MinWidth="170" MaxWidth="250" Width="180" />
<TreeGridColumn Field="Duration" HeaderText="Duration"  MinWidth="50"  MaxWidth="150" Width="80"  />
```

### Touch interaction

When the right edge of the header cell is tapped, a floating handler will be visible over the right border of the column. To resize the column, tap and drag the floating handler as needed. You can AutoFit a column by using the Column menu of the tree grid.

Programmatically auto-fit a column:
```csharp
await TreeRef.AutoFitColumnsAsync(new string[] { "TaskName" });
```

Prevent resizing on a specific column:
```razor
<TreeGridColumn Field="Id" AllowResizing="false" />
```

---

## Column Spanning

Column spanning in the Blazor TreeGrid provides automatic vertical merging of adjacent cells within the same column when identical values are detected. This improves readability by consolidating repeated values into a single taller cell, which is especially useful when the same value appears across consecutive rows.

### Automatic Column Spanning

Enable column spanning by setting the `AutoSpan` property of the `SfTreeGrid` component to `AutoSpanMode.Column`:

```razor
<SfTreeGrid @ref="TreeGridInstance" DataSource="@TreeData" IdMapping="Id"
            ParentIdMapping="ParentId" AutoSpan="AutoSpanMode.Column"
            TreeColumnIndex="1" GridLines="GridLine.Both">
    <TreeGridColumns>
        <TreeGridColumn Field="Id"     HeaderText="ID"        Width="80"
            TextAlign="TextAlign.Center" IsPrimaryKey></TreeGridColumn>
        <TreeGridColumn Field="Name"   HeaderText="Developer" Width="150"></TreeGridColumn>
        <TreeGridColumn Field="Slot1"  HeaderText="10:00 - 11:00 AM" Width="150"></TreeGridColumn>
        <TreeGridColumn Field="Slot2"  HeaderText="11:00 - 12:00 AM" Width="150"></TreeGridColumn>
        <TreeGridColumn Field="Status" HeaderText="Status"    Width="150"></TreeGridColumn>
    </TreeGridColumns>
</SfTreeGrid>
```

### AutoSpan Modes

The `AutoSpanMode` enumeration provides multiple options for cell merging behavior:

| Mode | Description |
|------|-------------|
| `AutoSpanMode.None` | Disables automatic cell spanning (default). |
| `AutoSpanMode.Row` | Enables horizontal merging across columns within the same row. |
| `AutoSpanMode.Column` | Enables vertical merging of adjacent cells with identical values in the same column. |
| `AutoSpanMode.HorizontalAndVertical` | Enables both horizontal and vertical merging. Executes row merging first, followed by column merging. |

### Disable column spanning for specific columns

Disable spanning on individual columns by setting their `AutoSpan` property to `AutoSpanMode.None`:

```razor
<TreeGridColumn Field="TotalBudget" HeaderText="Planned Budget" Width="140"
    TextAlign="TextAlign.Right" Format="C0" AutoSpan="AutoSpanMode.None" />
<TreeGridColumn Field="PaidToDate"  HeaderText="Actual Spend"   Width="140"
    TextAlign="TextAlign.Right" Format="C0" AutoSpan="AutoSpanMode.None" />
```

### Controlling spanning at TreeGrid and column levels

The spanning behavior is determined by how the TreeGrid-level and column-level `AutoSpan` settings interact. Column-level `AutoSpan` can only narrow the spanning directions permitted by the TreeGrid.

| TreeGrid AutoSpan | Column AutoSpan | Effective Behavior |
|---|---|---|
| `None` | Any | No spanning. TreeGrid disables it globally. |
| `Row` | `None` | No spanning. Column explicitly disables it. |
| `Row` | `Row` | Row spanning only. |
| `Row` | `Column` | No spanning. TreeGrid only allows row spanning. |
| `Row` | `HorizontalAndVertical` | Row spanning only. TreeGrid only allows row spanning. |
| `Column` | `None` | No spanning. Column explicitly disables it. |
| `Column` | `Row` | No spanning. TreeGrid only allows column spanning. |
| `Column` | `Column` | Column spanning only. |
| `Column` | `HorizontalAndVertical` | Column spanning only. TreeGrid only allows column spanning. |
| `HorizontalAndVertical` | `None` | No spanning. Column explicitly disables both directions. |
| `HorizontalAndVertical` | `Row` | Row spanning only. Column narrows to Row. |
| `HorizontalAndVertical` | `Column` | Column spanning only. Column narrows to Column. |
| `HorizontalAndVertical` | `HorizontalAndVertical` | Row and Column spanning. |

### Programmatic cell merging

Manually merge cells using the `MergeCellsAsync` method. Use `MergeCellInfo` to define the rectangular region:

| Property | Type | Description |
|----------|------|-------------|
| `RowIndex` | int | Zero-based index of the anchor row (top-left cell). |
| `ColumnIndex` | int | Zero-based index of the anchor column (top-left cell). |
| `RowSpan` | int (optional) | Number of rows to span. Defaults to 1. |
| `ColumnSpan` | int (optional) | Number of columns to span. Defaults to 1. |

```csharp
// Single merge
await TreeRef.MergeCellsAsync(new MergeCellInfo
{
    RowIndex = 1,
    ColumnIndex = 1,
    ColumnSpan = 2
});

// Batch merge
await TreeRef.MergeCellsAsync(new[]
{
    new MergeCellInfo { RowIndex = 0, ColumnIndex = 0, ColumnSpan = 2 },
    new MergeCellInfo { RowIndex = 5, ColumnIndex = 0, ColumnSpan = 3 },
    new MergeCellInfo { RowIndex = 7, ColumnIndex = 0, ColumnSpan = 2 }
});
```

### Clearing spanning programmatically

Use `UnmergeCellsAsync` to remove specific merged regions, or `UnmergeAllAsync` to reset all merges:

| Method | Parameter | Description |
|--------|-----------|-------------|
| `UnmergeCellsAsync` | `UnmergeCellInfo` | Removes a single merged area identified by its anchor cell. |
| `UnmergeCellsAsync` | `IEnumerable<UnmergeCellInfo>` | Removes multiple merged areas in one batch operation. |
| `UnmergeAllAsync` | – | Removes all merged regions in the current view. |

```csharp
// Remove a specific merged region
await TreeRef.UnmergeCellsAsync(new UnmergeCellInfo { RowIndex = 1, ColumnIndex = 1 });

// Remove multiple merged regions
await TreeRef.UnmergeCellsAsync(new[]
{
    new UnmergeCellInfo { RowIndex = 0, ColumnIndex = 0 },
    new UnmergeCellInfo { RowIndex = 5, ColumnIndex = 0 }
});

// Remove all merged regions
await TreeRef.UnmergeAllAsync();
```

### Limitations

Column spanning is not compatible with:
- Virtualization
- Infinite Scrolling
- Row Drag and Drop
- Column Virtualization
- Detail Template
- Editing
- Export

---

## Auto-Fit Columns

The `AutoFitColumnsAsync` method resizes the column to fit the widest cell's content without wrapping. A specific column can be autofitted at initial rendering by invoking `AutoFitColumnsAsync` in the `DataBound` event.

```razor
<SfTreeGrid @ref="TreeGrid" DataSource="@TreeGridData" IdMapping="TaskId"
            ParentIdMapping="ParentId" TreeColumnIndex="1">
    <TreeGridEvents DataBound="OnDataBound" TValue="TreeData.BusinessObject"></TreeGridEvents>
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160"></TreeGridColumn>
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="Priority" HeaderText="Priority"  Width="80"></TreeGridColumn>
    </TreeGridColumns>
</SfTreeGrid>

@code {
    SfTreeGrid<TreeData.BusinessObject> TreeGrid;

    private async Task OnDataBound(object e)
    {
        await this.TreeGrid.AutoFitColumnsAsync(new string[] { "TaskName" }); // specific column
        // await this.TreeGrid.AutoFitColumnsAsync(new string[] { });         // all columns
    }
}
```

> All columns can be autofitted by invoking the `AutoFitColumnsAsync` method without specific column names (pass an empty array).

### ⚠️ CRITICAL Constraints

- **Always Pass Parameter**: `AutoFitColumnsAsync()` requires a parameter (unlike other methods)
- **Empty Array = All Columns**: Use `new string[] { }` to fit all columns
- **Specific Columns**: Pass field names in the array to fit only those columns
- **Component Lifecycle**: Best called in `DataBound` event or `OnAfterRenderAsync(bool firstRender)` where `firstRender == true`

---

## Lock Columns

Columns can be locked by using the `LockColumn` property. The locked columns will be moved to the first position and cannot be reordered.

```razor
<SfTreeGrid IdMapping="TaskId" ParentIdMapping="ParentId" DataSource="@TreeGridData"
            TreeColumnIndex="1" AllowReordering="true">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"    HeaderText="Task ID"    Width="80"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="TaskName"  HeaderText="Task Name"  Width="90"></TreeGridColumn>
        <TreeGridColumn Field="StartDate" HeaderText="Start Date"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"
            Format="yMd" Type="Syncfusion.Blazor.Grids.ColumnType.Date" Width="90"></TreeGridColumn>
        <TreeGridColumn Field="Duration"  LockColumn="true" HeaderText="Duration"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" Width="80"></TreeGridColumn>
        <TreeGridColumn Field="Progress"  HeaderText="Progress"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" Width="80"></TreeGridColumn>
    </TreeGridColumns>
</SfTreeGrid>
```

---

## Freeze Columns

Fix columns to the left (or right) so they stay visible during horizontal scroll:

```razor
<SfTreeGrid FrozenColumns="2" ...>
    <!-- First 2 columns will be frozen/fixed -->
    <TreeGridColumns>
        <TreeGridColumn Field="Id"       Width="80" />   <!-- frozen -->
        <TreeGridColumn Field="TaskName" Width="200" />  <!-- frozen -->
        <TreeGridColumn Field="Duration" Width="100" />
        <TreeGridColumn Field="Progress" Width="100" />
    </TreeGridColumns>
</SfTreeGrid>
```

Or freeze specific columns individually:
```razor
<TreeGridColumn Field="Id" IsFrozen="true" Freeze="FreezeDirection.Left" />
```

---

## Responsive Columns

The column visibility can be toggled based on media queries defined in the `HideAtMedia` property. The `HideAtMedia` accepts valid [CSS Media Queries](http://cssmediaqueries.com/what-are-css-media-queries.html).

```razor
<SfTreeGrid IdMapping="TaskId" ParentIdMapping="ParentId" DataSource="@TreeGridData" TreeColumnIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"    HeaderText="Task ID"   Width="80"
            HideAtMedia="max-width: 700px"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="TaskName"  HeaderText="Task Name" Width="90"></TreeGridColumn>
        <TreeGridColumn Field="StartDate" HeaderText="Start Date"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"
            Format="yMd" Type="Syncfusion.Blazor.Grids.ColumnType.Date" Width="90"></TreeGridColumn>
        <TreeGridColumn Field="Duration"  HeaderText="Duration"
            HideAtMedia="max-width: 500px"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" Width="80"></TreeGridColumn>
    </TreeGridColumns>
</SfTreeGrid>
```

---

## Show or Hide Columns by External Button

The tree grid columns can be shown or hidden dynamically using the external buttons by invoking the `ShowColumnsAsync` or `HideColumnsAsync` method.

```razor
<button @onclick="HideColumns">Hide Column</button>
<button @onclick="ShowColumns">Show Column</button>

<SfTreeGrid @ref="TreeGrid" IdMapping="TaskId" ParentIdMapping="ParentId"
            DataSource="@TreeGridData" TreeColumnIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"    HeaderText="Task ID"    Width="80"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="TaskName"  HeaderText="Task Name"  Width="90"></TreeGridColumn>
        <TreeGridColumn Field="StartDate" HeaderText="Start Date"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"
            Format="yMd" Type="Syncfusion.Blazor.Grids.ColumnType.Date" Width="90"></TreeGridColumn>
        <TreeGridColumn Field="Duration"  HeaderText="Duration"
            TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" Width="80"></TreeGridColumn>
    </TreeGridColumns>
</SfTreeGrid>

@code {
    SfTreeGrid<TreeData.BusinessObject> TreeGrid;
    public string[] ColumnItems = new string[] { "Task ID", "Duration" };

    private async Task HideColumns()
    {
        await this.TreeGrid.HideColumnsAsync(ColumnItems);  // hide by HeaderText
    }
    private async Task ShowColumns()
    {
        await this.TreeGrid.ShowColumnsAsync(ColumnItems);  // show by HeaderText
    }
}
```

---
