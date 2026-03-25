# Paging in Syncfusion Blazor TreeGrid

> **Required namespaces:**
> - Include `@using Syncfusion.Blazor.Grids;` for TreeGridPageSettings and paging events
> - Include `@using Syncfusion.Blazor.Navigations;` when using `PagerTemplateContext` in custom pager templates

Paging divides data into discrete pages. It is the standard approach for large datasets when virtual scrolling is not suitable.

## Table of Contents
- [Enable Paging](#enable-paging)
- [Page Settings](#page-settings)
- [Page Size Mode](#page-size-mode)
- [Pager Position](#pager-position)
- [Programmatic Navigation](#programmatic-navigation)
- [Pager Template](#pager-template)
- [Pager with Page Size Dropdown](#pager-with-page-size-dropdown)
- [Paging vs. Virtualization — Decision Guide](#paging-vs-virtualization--decision-guide)
- [Paging Events](#paging-events)
- [Common Pitfalls & Compiler Errors](#common-pitfalls--compiler-errors)

---

## Enable Paging

```razor
@using Syncfusion.Blazor.TreeGrid

<SfTreeGrid DataSource="@TreeGridData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            AllowPaging="true"
            TreeColumnIndex="1">
    <TreeGridPageSettings PageCount="2" PageSize="2" PageSizeMode="PageSizeMode.Root" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"  Width="80"  TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Priority" HeaderText="Priority" Width="80" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public List<TreeData.BusinessObject> TreeGridData { get; set; }

    protected override void OnInitialized()
    {
        this.TreeGridData = TreeData.GetSelfDataSource().ToList();
    }
}
```

> 💡 Better performance can be achieved by using tree grid paging to fetch only a pre-defined number of records from the data source.

---

## Page Settings

```razor
<TreeGridPageSettings PageSize="15"
                      PageSizes="@(new int[] { 5, 10, 15, 25, 50 })"
                      CurrentPage="1"
                      PageCount="5" />
```

| Property | Description |
|---|---|
| `PageSize` | Rows per page (default: 12) |
| `PageSizes` | Shows a dropdown for user to change page size; pass `true` for default sizes or an array |
| `CurrentPage` | Initial page number (1-based) |
| `PageCount` | Number of page buttons shown in pager |

**Allow user to change page size:**
```razor
<TreeGridPageSettings PageSize="10" PageSizes="true" />
<!-- Or specific options: -->
<TreeGridPageSettings PageSize="10" PageSizes="@(new object[] { 10, 25, 50, "All" })" />
```

---

## Page Size Mode

The `PageSizeMode` property of `TreeGridPageSettings` defines two behaviors in TreeGrid paging to display a specific number of records on the current page.

| Mode | Description |
|---|---|
| **Root** | **Default mode.** The number of root nodes or the **0th-level** records to be displayed per page is based on `PageSize` property. Only root level or 0th level records are considered in records count. Child records are displayed separately after expanding parent rows. |
| **All** | Page size is calculated using the entire hierarchy, including both root and child records. |

> ⚠️ **Note:** The **All** mode of **PageSizeMode** is not supported with remote data binding in the TreeGrid.

**Example - Root Mode (Default):**
```razor
@using Syncfusion.Blazor.TreeGrid

<SfTreeGrid DataSource="@TreeData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            AllowPaging="true"
            TreeColumnIndex="1">
    <TreeGridPageSettings PageSize="2"
                          PageCount="2"
                          PageSizeMode="PageSizeMode.Root" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="Task ID" Width="80" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Progress" HeaderText="Progress" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
    </TreeGridColumns>
</SfTreeGrid>
```

**Example - All Mode:**
```razor
@using Syncfusion.Blazor.TreeGrid

<SfTreeGrid DataSource="@TreeData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            AllowPaging="true"
            TreeColumnIndex="1">
    <TreeGridPageSettings PageSize="5"
                          PageCount="2"
                          PageSizeMode="PageSizeMode.All" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="Task ID" Width="80" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Progress" HeaderText="Progress" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
    </TreeGridColumns>
</SfTreeGrid>
```

---

## Pager Position

```razor
<SfTreeGrid ...>
    <TreeGridPageSettings PageSize="10" />
    <!-- Pager renders at bottom by default -->
    <!-- To place at top, use CSS override or custom template -->
</SfTreeGrid>
```

---

## Programmatic Navigation

Navigate pages from code:

```razor
<SfTreeGrid @ref="TreeRef" AllowPaging="true" ...>
    ...
</SfTreeGrid>
<button @onclick="GoToPage3">Go to Page 3</button>
<button @onclick="NextPage">Next Page</button>

@code {
    private SfTreeGrid<TaskItem> TreeRef;

    private async Task GoToPage3()
    {
        await TreeRef.GoToPageAsync(3);
    }

    private async Task NextPage()
    {
        var currentPage = TreeRef.PageSettings.CurrentPage;
        await TreeRef.GoToPageAsync(currentPage + 1);
    }
}
```

---

## Pager Template

Customize the pager UI with a template. You can use custom elements inside the pager instead of default elements by specifying the `Template` property of `TreeGridPageSettings`.

Within the pager `Template`, access the `PagerTemplateContext` context to retrieve vital paging values such as `CurrentPage`, `PageSize`, `PageCount`, `TotalPage`, and `TotalRecordCount`. These values provide the data necessary to display and manage the pager effectively.

When implementing a custom pager template, you can design the layout of pager controls as desired. However, for actual paging functionality, integrate the `GoToPageAsync` method. This method handles navigation to a specific page based on the page number input.

> ⚠️ **Required namespace for custom pager templates:** Include `@using Syncfusion.Blazor.Navigations;` to use `PagerTemplateContext`

**Example — NumericTextBox in Pager Template:**
```razor
@using Syncfusion.Blazor.TreeGrid
@using Syncfusion.Blazor.Grids
@using Syncfusion.Blazor.Inputs
@using Syncfusion.Blazor.Navigations

<SfTreeGrid @ref="TreeGrid" DataSource="@TreeData" Height="312"
            IdMapping="TaskID" ParentIdMapping="ParentID"
            TreeColumnIndex="1" AllowPaging="true">
    <TreeGridPageSettings PageSize="@pageSize">
        <Template>
            @{
                var Paging = (context as PagerTemplateContext);
                <div>
                    <div>
                        <SfNumericTextBox TValue="int" Format="###" Step="1" Min="1" Max="5"
                                          Placeholder="Select Page Size" Width="200px">
                            <NumericTextBoxEvents TValue="int" ValueChange="@CalculatePageSize">
                            </NumericTextBoxEvents>
                        </SfNumericTextBox>
                    </div>
                    <div style="margin-top:5px;margin-left:30px;border: none; display: inline-block">
                        <span> of @totalPages pages (@TreeData.Count items)</span>
                    </div>
                </div>
            }
        </Template>
    </TreeGridPageSettings>
    <TreeGridColumns>
        <TreeGridColumn Field="TaskID"    HeaderText="Task ID"    Width="80"  TextAlign="TextAlign.Right" />
        <TreeGridColumn Field="TaskName"  HeaderText="Task Name"  Width="170" />
        <TreeGridColumn Field="StartDate" HeaderText="Start Date" Format="d"  Type="ColumnType.Date" Width="145" TextAlign="TextAlign.Right" />
        <TreeGridColumn Field="Duration"  HeaderText="Duration"   Width="100" TextAlign="TextAlign.Right" />
        <TreeGridColumn Field="Progress"  HeaderText="Progress"   Width="110" />
        <TreeGridColumn Field="Priority"  HeaderText="Priority"   Width="100" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    private List<SelfReferenceData> TreeData { get; set; }
    SfTreeGrid<SelfReferenceData> TreeGrid;
    public int pageSize { get; set; } = 3;
    public int totalPages => (int)Math.Ceiling((double)TreeData.Count / (pageSize * 6));

    protected override void OnInitialized()
    {
        TreeData = SelfReferenceData.GetTree().Take(90).ToList();
    }

    private async Task CalculatePageSize(Syncfusion.Blazor.Inputs.ChangeEventArgs<int> args)
    {
        await TreeGrid.GoToPageAsync(args.Value);
    }
}
```

**Template Context Properties:**
- `CurrentPage` — Current page number
- `TotalPages` — Total number of pages
- `TotalRecordCount` — Total number of records
- `PageSize` — Records per page
- `PageCount` — Number of page buttons shown

> ⚠️ **Note:** `TotalPages` is a property of `PagerTemplateContext` and is **only available inside the pager `<Template>`**. It is **not** a property of `TreeGridPageSettings`.

---

## Pager with Page Size Dropdown

Enable users to dynamically change the number of records displayed per page using the `PageSizes` property. The pager dropdown allows changing the page size on-the-fly.

```razor
@using Syncfusion.Blazor.TreeGrid

<SfTreeGrid DataSource="@TreeData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            AllowPaging="true"
            TreeColumnIndex="1">
    <TreeGridPageSettings PageSize="2"
                          PageCount="2"
                          PageSizeMode="PageSizeMode.Root"
                          PageSizes="@(new List<int>() { 2, 5, 10 })" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="Task ID" Width="80" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Priority" HeaderText="Priority" Width="80" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public List<TreeData.BusinessObject> TreeData { get; set; }
    
    protected override void OnInitialized()
    {
        this.TreeData = TreeData.GetSelfDataSource().ToList();
    }
}
```

**Configuration Options:**
- `PageSizes="true"` — Uses default page size options
- `PageSizes="@(new List<int>() { 5, 10, 25 })"` — Custom page size options
- `PageSizes="@(new object[] { 10, 25, 50, "All" })"` — Include "All" option to display all records

---

## Paging vs. Virtualization — Decision Guide

| Scenario | Recommended |
|---|---|
| < 5,000 records | Paging (simple, accessible) |
| 5,000–100,000 records | Virtualization (row virtual) |
| > 100,000 records | Virtualization + lazy loading |
| Need to jump to specific page | Paging |
| Need infinite scroll feel | Virtualization |
| Export all data | Paging (easier to export all pages) |
| Accessibility / keyboard nav important | Paging |

See [virtualization.md](virtualization.md) for virtual scroll setup.

---

## Paging Events

```razor
<TreeGridEvents TValue="TaskItem" PageChanged="PageChangedHandler" OnActionBegin="OnPageChange" PageChanging="PageChangingHandler" />

@code {
    private void OnPageChange(ActionEventArgs<TaskItem> args)
    {
        if (args.RequestType == Syncfusion.Blazor.Grids.Action.Paging)
            Console.WriteLine($"Navigated to page: {args.CurrentPage}");
    }

    public async Task PageChangedHandler (GridPageChangedEventArgs args) 
    { 
        args.CurrentPage = 2; // Sets the current page number. 
    }

    public async Task PageChangingHandler (GridPageChangingEventArgs args) 
    { 
        args.CurrentPage = 2; // Sets the current page number. 
    }
}
```

---

## Common Pitfalls & Compiler Errors

### CS1662 / CS1503 — Wrong `@onchange` Handler Type in Pager Template

**Errors:**
- `CS1662: Cannot convert lambda expression to intended delegate type because some of the return types in the block are not implicitly convertible to the delegate return type`
- `CS1503: Argument 2: cannot convert from 'method group' to 'Microsoft.AspNetCore.Components.EventCallback'`

**Cause:** When a native HTML `<select>` inside a pager `<Template>` uses `@onchange`, Blazor expects the handler to accept `Microsoft.AspNetCore.Components.ChangeEventArgs`. Using `Syncfusion.Blazor.Inputs.ChangeEventArgs<T>` instead causes the compiler to fail matching the method group to the `EventCallback` delegate.

**❌ Wrong — causes CS1662 / CS1503:**
```csharp
private async Task ChangePageSize(Syncfusion.Blazor.Inputs.ChangeEventArgs<TaskData> e)
{
    if (int.TryParse(e.Value.ToString(), out int newPageSize)) { ... }
}
```

**✅ Correct — use `Microsoft.AspNetCore.Components.ChangeEventArgs`:**
```csharp
private async Task ChangePageSize(Microsoft.AspNetCore.Components.ChangeEventArgs e)
{
    if (TreeGrid != null && int.TryParse(e.Value?.ToString(), out int newPageSize))
    {
        TreeGrid.PageSettings.PageSize = newPageSize;
        await TreeGrid.RefreshAsync();
    }
}
```

> 💡 `Syncfusion.Blazor.Inputs.ChangeEventArgs<T>` is only for **Syncfusion input component** events (e.g., `SfDropDownList`, `SfNumericTextBox`). A plain HTML `<select>` with `@onchange` always uses Blazor's native `Microsoft.AspNetCore.Components.ChangeEventArgs`.

---

### CS1662 — Inline `async` Lambdas in `@onclick` inside Pager Template

**Error:** `CS1662: Cannot convert lambda expression to intended delegate type`

**Cause:** Inline `async () => await ...` lambdas used directly in `@onclick` within a Razor `<Template>` block confuse the compiler — it cannot implicitly match the `async Task` lambda to Blazor's `EventCallback` delegate in that context.

**❌ Wrong:**
```razor
<button @onclick="async () => await TreeGrid.GoToPageAsync(1)">First</button>
```

**✅ Correct — extract to a named method:**
```razor
<button @onclick="FirstPage">First</button>

@code {
    private async Task FirstPage()
    {
        if (TreeGrid != null)
            await TreeGrid.GoToPageAsync(1);
    }
}
```

> 💡 Always use **named `async Task` methods** for `@onclick` handlers inside `<Template>` blocks. Inline async lambdas work fine in regular markup but are unreliable inside Razor template fragments.

---
