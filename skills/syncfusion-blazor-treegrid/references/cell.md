# Cell Customization in Syncfusion Blazor TreeGrid

> **Namespace note:** `ClipMode` and `TextAlign` belong to `Syncfusion.Blazor.Grids`, **not** `Syncfusion.Blazor.TreeGrid`. Always qualify them as `Syncfusion.Blazor.Grids.ClipMode.Ellipsis` and `Syncfusion.Blazor.Grids.TextAlign.Right` to avoid CS0104 ambiguity errors.

## Table of Contents
- [HTML Content in Cells](#html-content-in-cells)
- [Customize Cell Styles](#customize-cell-styles)
- [Auto Wrap](#auto-wrap)
- [Grid Lines](#grid-lines)
- [Clip Mode](#clip-mode)

---

## HTML Content in Cells

By default, HTML tags are encoded (displayed as plain text). To render actual HTML content in cells and headers, set `DisableHtmlEncode="true"` on the column to disable the encoding. This allows HTML tags to be rendered as actual HTML instead of plain text.

```razor
@using Syncfusion.Blazor.TreeGrid

<SfTreeGrid IdMapping="TaskId" DataSource="@TreeGridData" ParentIdMapping="ParentId" TreeColumnIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="<span> Task ID </span>" Width="80" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" DisableHtmlEncode="true" />
        <TreeGridColumn Field="TaskName" HeaderText="<span> Task Name </span>" Width="160" DisableHtmlEncode="true" />
        <TreeGridColumn Field="Duration" HeaderText="<span> Duration </span>" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" DisableHtmlEncode="true" />
        <TreeGridColumn Field="Progress" HeaderText="<span> Progress </span>" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" DisableHtmlEncode="true" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public List<BusinessObject> TreeGridData { get; set; }
    
    protected override void OnInitialized()
    {
        this.TreeGridData = new List<BusinessObject>
        {
            new BusinessObject { TaskId = 1, TaskName = "Parent Task 1", StartDate = new DateTime(2017, 10, 23), Duration = 10, Progress = 70, ParentId = null },
            new BusinessObject { TaskId = 2, TaskName = "Child task 1", StartDate = new DateTime(2017, 10, 23), Duration = 4, Progress = 80, ParentId = 1 },
            new BusinessObject { TaskId = 3, TaskName = "Child Task 2", StartDate = new DateTime(2017, 10, 24), Duration = 5, Progress = 65, ParentId = 2 },
            new BusinessObject { TaskId = 4, TaskName = "Child task 3", StartDate = new DateTime(2017, 10, 25), Duration = 6, Progress = 77, ParentId = 3 }
        };
    }
}

public class BusinessObject
{
    public int TaskId { get; set; }
    public string TaskName { get; set; }
    public DateTime? StartDate { get; set; }
    public int? Duration { get; set; }
    public int? Progress { get; set; }
    public int? ParentId { get; set; }
}
```

> ⚠️ Only use with trusted data. Enabling this on user-supplied content can expose XSS vulnerabilities.

---

## Customize Cell Styles

The appearance of cells can be customized by using the `QueryCellInfo` event. The `QueryCellInfo` event triggers for every cell. In that event handler, you can get `QueryCellInfoEventArgs` that contains the details of the cell.

```razor
@using Syncfusion.Blazor.TreeGrid

<SfTreeGrid DataSource="@TreeGridData" IdMapping="TaskId" ParentIdMapping="ParentId" TreeColumnIndex="1">
    <TreeGridEvents QueryCellInfo="OnQueryCellInfo" TValue="TreeData" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="Task ID" Width="90" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="100" />
        <TreeGridColumn Field="Duration" HeaderText="Duration" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Priority" HeaderText="Priority" Width="100" />
    </TreeGridColumns>
</SfTreeGrid>

<style>    
    .intro {
        background-color: #336c12;
        color: white;
    }
    .intro1 {
        background-color: #7b2b1d;
        color: white;
    }
</style>

@code {
    public List<TreeData> TreeGridData { get; set; }

    protected override void OnInitialized()
    {
        this.TreeGridData = new List<TreeData>
        {
            new TreeData { TaskId = 1, TaskName = "Parent Task 1", Duration = 10, Progress = 70, Priority = "Critical", ParentId = null },
            new TreeData { TaskId = 2, TaskName = "Child task 1", Duration = 4, Progress = 80, Priority = "Low", ParentId = 1 },
            new TreeData { TaskId = 3, TaskName = "Child Task 2", Duration = 5, Progress = 65, Priority = "Critical", ParentId = 2 },
            new TreeData { TaskId = 4, TaskName = "Child task 3", Duration = 6, Progress = 77, Priority = "High", ParentId = 3 },
            new TreeData { TaskId = 5, TaskName = "Parent Task 2", Duration = 10, Progress = 70, Priority = "Critical", ParentId = null },
            new TreeData { TaskId = 6, TaskName = "Child task 1", Duration = 4, Progress = 80, Priority = "Critical", ParentId = 5 },
            new TreeData { TaskId = 7, TaskName = "Child Task 2", Duration = 5, Progress = 65, Priority = "Low", ParentId = 5 },
            new TreeData { TaskId = 8, TaskName = "Child task 3", Duration = 6, Progress = 77, Priority = "High", ParentId = 5 },
            new TreeData { TaskId = 9, TaskName = "Child task 4", Duration = 6, Progress = 77, Priority = "Low", ParentId = 5 }
        };
    }

    private void OnQueryCellInfo(QueryCellInfoEventArgs<TreeData> args)
    {
        if (args.Column.Field == "Progress" && args.Data.Progress > 70 && args.Data.Progress <= 100)
        {
            args.Cell.AddClass(new[] { "intro" });
        }
        else if (args.Column.Field == "Progress" && args.Data.Progress > 20)
        {
            args.Cell.AddClass(new[] { "intro1" });
        }
    }
}

public class TreeData
{
    public int TaskId { get; set; }
    public string TaskName { get; set; }
    public DateTime? StartDate { get; set; }
    public int? Duration { get; set; }
    public int? Progress { get; set; }
    public string Priority { get; set; }
    public int? ParentId { get; set; }
}
```

---

## Auto Wrap

The auto wrap allows the cell content of the tree grid to wrap to the next line when it exceeds the boundary of the cell width. The cell content wrapping works based on the position of white space between words. To enable auto wrap, set the `AllowTextWrap` property to **true**.

> **Note:** When a column width is not specified, then auto wrap of columns will be adjusted with respect to the tree grid's width.

```razor
@using Syncfusion.Blazor.TreeGrid

<SfTreeGrid DataSource="@TreeGridData" IdMapping="TaskId" ParentIdMapping="ParentId" TreeColumnIndex="1" AllowTextWrap="true">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="Task ID" Width="80" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="100" />
        <TreeGridColumn Field="StartDate" HeaderText="Start Date" Format="yMd" Type="Syncfusion.Blazor.Grids.ColumnType.Date" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Duration" HeaderText="Duration" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Priority" HeaderText="Priority" Width="100" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public List<BusinessObject> TreeGridData { get; set; }
    
    protected override void OnInitialized()
    {
        this.TreeGridData = new List<BusinessObject>
        {
            new BusinessObject { TaskId = 1, TaskName = "Parent Task 1", StartDate = new DateTime(2017, 10, 23), Duration = 10, Progress = 70, Priority = "Critical", ParentId = null },
            new BusinessObject { TaskId = 2, TaskName = "Child task 1", StartDate = new DateTime(2017, 10, 23), Duration = 4, Progress = 80, Priority = "Low", ParentId = 1 },
            new BusinessObject { TaskId = 3, TaskName = "Child Task 2", StartDate = new DateTime(2017, 10, 24), Duration = 5, Progress = 65, Priority = "Critical", ParentId = 2 },
            new BusinessObject { TaskId = 4, TaskName = "Child task 3", StartDate = new DateTime(2017, 10, 25), Duration = 6, Progress = 77, Priority = "High", ParentId = 3 }
        };
    }
}

public class BusinessObject
{
    public int TaskId { get; set; }
    public string TaskName { get; set; }
    public DateTime? StartDate { get; set; }
    public int? Duration { get; set; }
    public int? Progress { get; set; }
    public string Priority { get; set; }
    public int? ParentId { get; set; }
}
```

---

## Grid Lines

The `GridLines` property controls cell border display and can be defined by the `GridLines` property.

The available modes of grid lines are:

| Modes | Actions |
|-------|---------|
| Both | Displays both the horizontal and vertical grid lines |
| None | No grid lines are displayed |
| Horizontal | Displays the horizontal grid lines only |
| Vertical | Displays the vertical grid lines only |
| Default | Displays grid lines based on the theme |

```razor
@using Syncfusion.Blazor.TreeGrid

<SfTreeGrid IdMapping="TaskId" ParentIdMapping="ParentId" DataSource="@TreeGridData" TreeColumnIndex="1" GridLines="Syncfusion.Blazor.Grids.GridLine.None">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="Task ID" Width="80" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="100" />
        <TreeGridColumn Field="StartDate" HeaderText="Start Date" Format="yMd" Type="Syncfusion.Blazor.Grids.ColumnType.Date" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Duration" HeaderText="Duration" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Priority" HeaderText="Priority" Width="100" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public List<BusinessObject> TreeGridData { get; set; }
    
    protected override void OnInitialized()
    {
        this.TreeGridData = new List<BusinessObject>
        {
            new BusinessObject { TaskId = 1, TaskName = "Parent Task 1", StartDate = new DateTime(2017, 10, 23), Duration = 10, Progress = 70, Priority = "Critical", ParentId = null },
            new BusinessObject { TaskId = 2, TaskName = "Child task 1", StartDate = new DateTime(2017, 10, 23), Duration = 4, Progress = 80, Priority = "Low", ParentId = 1 },
            new BusinessObject { TaskId = 3, TaskName = "Child Task 2", StartDate = new DateTime(2017, 10, 24), Duration = 5, Progress = 65, Priority = "Critical", ParentId = 2 },
            new BusinessObject { TaskId = 4, TaskName = "Child task 3", StartDate = new DateTime(2017, 10, 25), Duration = 6, Progress = 77, Priority = "High", ParentId = 3 }
        };
    }
}

public class BusinessObject
{
    public int TaskId { get; set; }
    public string TaskName { get; set; }
    public DateTime? StartDate { get; set; }
    public int? Duration { get; set; }
    public int? Progress { get; set; }
    public string Priority { get; set; }
    public int? ParentId { get; set; }
}
```

> **Note:** By default, the tree grid renders with **Default** mode.

---

## Clip Mode

The clip mode provides options to display its overflow cell content and it can be defined by the `ClipMode` property.

There are three types of `ClipMode`:

* **Clip**: Truncates the cell content when it overflows its area.
* **Ellipsis**: Displays ellipsis when the cell content overflows its area.
* **EllipsisWithTooltip**: Displays ellipsis when the cell content overflows its area, also it will display the tooltip while hover on ellipsis is applied.

```razor
@using Syncfusion.Blazor.TreeGrid

<SfTreeGrid IdMapping="TaskId" ParentIdMapping="ParentId" DataSource="@TreeGridData" TreeColumnIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="Task ID" Width="80" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="100" ClipMode="Syncfusion.Blazor.Grids.ClipMode.EllipsisWithTooltip" />
        <TreeGridColumn Field="StartDate" HeaderText="Start Date" ClipMode="Syncfusion.Blazor.Grids.ClipMode.Ellipsis" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Duration" HeaderText="Duration" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Priority" HeaderText="Priority" Width="60" ClipMode="Syncfusion.Blazor.Grids.ClipMode.Clip" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public List<BusinessObject> TreeGridData { get; set; }
    
    protected override void OnInitialized()
    {
        this.TreeGridData = new List<BusinessObject>
        {
            new BusinessObject { TaskId = 1, TaskName = "Parent Task 1", StartDate = new DateTime(2017, 10, 23), Duration = 10, Progress = 70, Priority = "Critical", ParentId = null },
            new BusinessObject { TaskId = 2, TaskName = "Child task 1", StartDate = new DateTime(2017, 10, 23), Duration = 4, Progress = 80, Priority = "Low", ParentId = 1 },
            new BusinessObject { TaskId = 3, TaskName = "Child Task 2", StartDate = new DateTime(2017, 10, 24), Duration = 5, Progress = 65, Priority = "Critical", ParentId = 2 },
            new BusinessObject { TaskId = 4, TaskName = "Child task 3", StartDate = new DateTime(2017, 10, 25), Duration = 6, Progress = 77, Priority = "High", ParentId = 3 }
        };
    }
}

public class BusinessObject
{
    public int TaskId { get; set; }
    public string TaskName { get; set; }
    public DateTime? StartDate { get; set; }
    public int? Duration { get; set; }
    public int? Progress { get; set; }
    public string Priority { get; set; }
    public int? ParentId { get; set; }
}
```

> **Note:** By default, `ClipMode` value is **Ellipsis**.

---
