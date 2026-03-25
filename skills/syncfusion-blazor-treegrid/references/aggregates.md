# Aggregates in Syncfusion Blazor TreeGrid

> **Required namespace:** Include `@using Syncfusion.Blazor.Grids;` for `AggregateType` (Grids namespace) and aggregate event arguments.

> **Namespace note:** `AggregateType` belongs to `Syncfusion.Blazor.Grids`, **not** `Syncfusion.Blazor.TreeGrid`. Always qualify it as `Syncfusion.Blazor.Grids.AggregateType.Sum` (or `.Average`, `.Min`, `.Max`, etc.) to avoid CS0104 ambiguity errors.

Aggregate values are displayed in the TreeGrid footer and in parent row footer for child row aggregate values. It is configured through `TreeGridAggregateColumn`. The `Field` and `Type` are the minimum properties required to represent an aggregate column.

By default, the aggregate value can be displayed in the TreeGrid footer, and footer of child rows. To show the aggregate value in one of the cells, use the `FooterTemplate`.

## Table of Contents
- [Built-in Aggregate Types](#built-in-aggregate-types)
- [Aggregate Row Layout Rule](#aggregate-row-layout-rule)
- [Footer Aggregate](#footer-aggregate)
- [How to Format Aggregate Value](#how-to-format-aggregate-value)
- [Child Summary Aggregates](#child-summary-aggregates)
- [Custom Aggregates](#custom-aggregates)
- [Limitations](#limitations)

---

## Built-in Aggregate Types

The aggregate type should be specified in the `Type` property to configure an aggregate column.

| Type | Description |
|---|---|
| `Syncfusion.Blazor.Grids.AggregateType.Sum` | Total of all values |
| `Syncfusion.Blazor.Grids.AggregateType.Average` | Mean value |
| `Syncfusion.Blazor.Grids.AggregateType.Min` | Minimum value |
| `Syncfusion.Blazor.Grids.AggregateType.Max` | Maximum value |
| `Syncfusion.Blazor.Grids.AggregateType.Count` | Number of records |
| `Syncfusion.Blazor.Grids.AggregateType.TrueCount` | Count of `true` values in a boolean column |
| `Syncfusion.Blazor.Grids.AggregateType.FalseCount` | Count of `false` values in a boolean column |

---

## Aggregate Row Layout Rule

**Key principle: Control aggregate rows by how fields are distributed across `<TreeGridAggregate>` blocks.**

- **Similar-field aggregates in SAME row** → Place multiple `<TreeGridAggregateColumn>` entries on **different fields** within a **single** `<TreeGridAggregate>` block
  - Example: Sum(Duration) + TrueCount(Approved) → One `<TreeGridAggregate>` block with both columns
  - Result: Single footer row showing both aggregates side-by-side

- **Same-field aggregates in SEPARATE rows** → For multiple aggregates on the **same field** (e.g., Min + Max on Duration), use **separate** `<TreeGridAggregate>` blocks
  - Example: Min(Duration) in Row 1, Max(Duration) in Row 2 → Two `<TreeGridAggregate>` blocks, each with one column
  - Result: Two separate footer rows, one for each aggregate

**Example structure:**
```razor
<TreeGridAggregates>
    <!-- Row 1: Different fields in same row -->
    <TreeGridAggregate ShowChildSummary="false">
        <TreeGridAggregateColumns>
            <TreeGridAggregateColumn Field="Duration" Type="Sum" />
            <TreeGridAggregateColumn Field="Approved" Type="TrueCount" />
        </TreeGridAggregateColumns>
    </TreeGridAggregate>

    <!-- Row 2: Same field, separate aggregate -->
    <TreeGridAggregate ShowChildSummary="false">
        <TreeGridAggregateColumns>
            <TreeGridAggregateColumn Field="Duration" Type="Average" />
        </TreeGridAggregateColumns>
    </TreeGridAggregate>
</TreeGridAggregates>
```

---

## Footer Aggregate

Footer aggregate value is calculated for all the rows and is displayed in the footer cells. Use the `FooterTemplate` to render the aggregate value in footer cells.

> The aggregate values must be accessed inside the template using their corresponding `AggregateType`.

```razor
@using TreeGridComponent.Data;
@using Syncfusion.Blazor.TreeGrid;

<SfTreeGrid DataSource="@TreeGridData" IdMapping="TaskId" ParentIdMapping="ParentId" AllowPaging="true" TreeColumnIndex="1">
    <TreeGridAggregates>
        <TreeGridAggregate ShowChildSummary="false">
            <TreeGridAggregateColumns>
                <TreeGridAggregateColumn Field="Duration" Type="Syncfusion.Blazor.Grids.AggregateType.Sum" Format="C2">
                    <FooterTemplate>
                        @{
                            var sumvalue = (context as Syncfusion.Blazor.Grids.AggregateTemplateContext);
                            <div>
                                <p>Sum: @sumvalue.Sum</p>
                            </div>
                        }
                    </FooterTemplate>
                </TreeGridAggregateColumn>
                <TreeGridAggregateColumn Field="Approved" Type="Syncfusion.Blazor.Grids.AggregateType.TrueCount" Format="C2">
                    <FooterTemplate>
                        @{
                            var truecount = (context as Syncfusion.Blazor.Grids.AggregateTemplateContext);
                            <div>
                                <p>Approved: @truecount.TrueCount</p>
                            </div>
                        }
                    </FooterTemplate>
                </TreeGridAggregateColumn>
            </TreeGridAggregateColumns>
        </TreeGridAggregate>
    </TreeGridAggregates>
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="Task ID" Width="80" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="100"></TreeGridColumn>
        <TreeGridColumn Field="Duration" HeaderText="Duration" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="Progress" HeaderText="Progress" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="Approved" HeaderText="Approved" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Center" DisplayAsCheckBox="true" Width="100">
        </TreeGridColumn>
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public List<TreeData> TreeGridData { get; set; }

    public Syncfusion.Blazor.Grids.AggregateTemplateContext model = new Syncfusion.Blazor.Grids.AggregateTemplateContext();

    protected override void OnInitialized()
    {
        this.TreeGridData = TreeData.GetSelfDataSource().ToList();
    }
}
```

**Model class:**
```csharp
namespace TreeGridComponent.Data {

    public class TreeData
    {
        public int TaskId { get; set; }
        public string TaskName { get; set; }
        public DateTime? StartDate { get; set; }
        public int? Duration { get; set; }
        public int? Progress { get; set; }
        public bool Approved { get; set; }
        public int? ParentId { get; set; }

        public static List<TreeData> GetSelfDataSource()
        {
            List<TreeData> TreeDataCollection = new List<TreeData>();
            TreeDataCollection.Add(new TreeData() { TaskId = 1, TaskName = "Parent Task 1", Duration = 10, Progress = 70, Approved = true, ParentId = null });
            TreeDataCollection.Add(new TreeData() { TaskId = 2, TaskName = "Child task 1", Duration = 4, Progress = 80, Approved = false, ParentId = 1 });
            TreeDataCollection.Add(new TreeData() { TaskId = 3, TaskName = "Child Task 2", Duration = 5, Progress = 65, Approved = true, ParentId = 2 });
            TreeDataCollection.Add(new TreeData() { TaskId = 4, TaskName = "Child task 3", Duration = 6, Approved = false, Progress = 77, ParentId = 3 });
            TreeDataCollection.Add(new TreeData() { TaskId = 5, TaskName = "Parent Task 2", Duration = 10, Progress = 70, Approved = true, ParentId = null });
            TreeDataCollection.Add(new TreeData() { TaskId = 6, TaskName = "Child task 1", Duration = 4, Progress = 80, Approved = false, ParentId = 5 });
            TreeDataCollection.Add(new TreeData() { TaskId = 7, TaskName = "Child Task 2", Duration = 5, Progress = 65, Approved = true, ParentId = 5 });
            TreeDataCollection.Add(new TreeData() { TaskId = 8, TaskName = "Child task 3", Duration = 6, Progress = 77, Approved = false, ParentId = 5 });
            TreeDataCollection.Add(new TreeData() { TaskId = 9, TaskName = "Child task 4", Duration = 6, Progress = 77, Approved = true, ParentId = 5 });
            return TreeDataCollection;
        }
    }
}
```

---

## How to Format Aggregate Value

The aggregate value result can be formatted by using the `Format` property on `TreeGridAggregateColumn`.

```razor
@using TreeGridComponent.Data;
@using Syncfusion.Blazor.TreeGrid;

<SfTreeGrid DataSource="@TreeGridData" IdMapping="TaskId" ParentIdMapping="ParentId" AllowPaging="true" TreeColumnIndex="1">
    <TreeGridAggregates>
        <TreeGridAggregate ShowChildSummary="false">
            <TreeGridAggregateColumns>
                <TreeGridAggregateColumn Field="Duration" Type="Syncfusion.Blazor.Grids.AggregateType.Sum" Format="C2">
                    <FooterTemplate>
                        @{
                            var sumvalue = (context as Syncfusion.Blazor.Grids.AggregateTemplateContext);
                            <div>
                                <p>Sum: @sumvalue.Sum</p>
                            </div>
                        }
                    </FooterTemplate>
                </TreeGridAggregateColumn>
            </TreeGridAggregateColumns>
        </TreeGridAggregate>
    </TreeGridAggregates>
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="Task ID" Width="80" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="100"></TreeGridColumn>
        <TreeGridColumn Field="Duration" HeaderText="Duration" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="Progress" HeaderText="Progress" Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="Approved" HeaderText="Approved" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Center" DisplayAsCheckBox="true" Width="100">
        </TreeGridColumn>
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public List<TreeData> TreeGridData { get; set; }

    public Syncfusion.Blazor.Grids.AggregateTemplateContext model = new Syncfusion.Blazor.Grids.AggregateTemplateContext();

    protected override void OnInitialized()
    {
        this.TreeGridData = TreeData.GetSelfDataSource().ToList();
    }
}
```

> Use the `Format` property (e.g., `Format="C2"`) on `TreeGridAggregateColumn` to format aggregate values — do not use `.ToString()` formatting inside the template.

---

## Child Summary Aggregates

By default, the aggregate values are shown for all rows including child rows in the footer. Set `ShowChildSummary="true"` on `<TreeGridAggregate>` to display aggregate subtotals in the parent row footer for their child data.

| `ShowChildSummary` | Behavior |
|---|---|
| `false` (default) | Aggregate shown only in the grid footer for all records |
| `true` | Aggregate also shown in each parent row's summary row, calculated from that parent's children |

```razor
<SfTreeGrid DataSource="@TreeGridData" IdMapping="TaskId" ParentIdMapping="ParentId" TreeColumnIndex="1">
    <TreeGridAggregates>
        <TreeGridAggregate ShowChildSummary="true">
            <TreeGridAggregateColumns>
                <TreeGridAggregateColumn Field="Duration" Type="Syncfusion.Blazor.Grids.AggregateType.Sum" Format="C2">
                    <FooterTemplate>
                        @{
                            var sumvalue = (context as Syncfusion.Blazor.Grids.AggregateTemplateContext);
                            <div>
                                <p>Sum: @sumvalue.Sum</p>
                            </div>
                        }
                    </FooterTemplate>
                </TreeGridAggregateColumn>
            </TreeGridAggregateColumns>
        </TreeGridAggregate>
    </TreeGridAggregates>
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="Task ID"   Width="80"  TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="160" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" TextAlign="Syncfusion.Blazor.Grids.TextAlign.Right" />
    </TreeGridColumns>
</SfTreeGrid>
```

---

## Custom Aggregates

Custom aggregates enable calculations beyond the built-in types. Implement custom logic directly in the `FooterTemplate` using LINQ and helper methods with safe component references.

**Key pattern:**
- Use `@ref="TreeGrid"` (nullable type) to safely reference the TreeGrid component
- Use `GetCurrentViewRecords()` to access currently visible records (pagination-aware)
- Implement helper methods with try-catch for error handling
- Place all aggregates in a **single** `<TreeGridAggregate>` block for single-row footer display
- Handle null TreeGrid reference with fallback to `TreeGridData`

**Complete working example with dynamic category filtering:**

```razor
@page "/custom-aggregates"
@using Syncfusion.Blazor.TreeGrid
@using Syncfusion.Blazor.Grids
@using Syncfusion.Blazor.DropDowns
@using System.Linq

<SfTreeGrid @ref="TreeGrid"
            DataSource="@TreeGridData"
            IdMapping="ID"
            ParentIdMapping="ParentID"
            Height="400"
            TreeColumnIndex="1">
    <TreeGridAggregates>
        <!-- Single Row: Multiple custom aggregates with dropdown filter -->
        <TreeGridAggregate ShowChildSummary="false">
            <TreeGridAggregateColumns>
                <!-- Custom 1: Sum by Category -->
                <TreeGridAggregateColumn Field="TotalPrice"
                                         Type="Syncfusion.Blazor.Grids.AggregateType.Sum">
                    <FooterTemplate>
                        @{
                            var _ = (context as Syncfusion.Blazor.Grids.AggregateTemplateContext);
                            <span>
                                @SumForSelectedCategory().ToString("C2")
                            </span>
                        }
                    </FooterTemplate>
                </TreeGridAggregateColumn>

                <!-- Custom 2: Category Dropdown Filter -->
                <TreeGridAggregateColumn Field="ShipmentCategory"
                                         Type="Syncfusion.Blazor.Grids.AggregateType.Sum">
                    <FooterTemplate>
                        @{
                            var _ = (context as Syncfusion.Blazor.Grids.AggregateTemplateContext);
                            <div class="treeDropDown">
                                <span style="margin-right: 5px;">Category: </span>
                                <span>
                                    <SfDropDownList TValue="string" TItem="string"
                                                    DataSource="@Categories"
                                                    @bind-Value="SelectedCategory" />
                                </span>
                            </div>
                        }
                    </FooterTemplate>
                </TreeGridAggregateColumn>
            </TreeGridAggregateColumns>
        </TreeGridAggregate>
    </TreeGridAggregates>

    <TreeGridColumns>
        <TreeGridColumn Field="ShipID" HeaderText="Ship ID" Width="80" Visible="false" IsPrimaryKey="true" TextAlign="TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="Name" HeaderText="Orders" Width="120" ClipMode="ClipMode.EllipsisWithTooltip"></TreeGridColumn>
        <TreeGridColumn Field="ShipmentCategory" HeaderText="Category" Width="130"></TreeGridColumn>
        <TreeGridColumn Field="OrderDate" HeaderText="Ordered Date" Format="d" Type="ColumnType.Date" Width="130" TextAlign="TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="ShippedDate" HeaderText="Shipped Date" Format="d" Type="ColumnType.Date" Width="130" TextAlign="TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="Units" HeaderText="Units" Width="80" TextAlign="TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="UnitPrice" HeaderText="Unit Price" Format="c2" Width="90" TextAlign="TextAlign.Right"></TreeGridColumn>
        <TreeGridColumn Field="TotalPrice" HeaderText="Total Price" Format="C" Width="100" TextAlign="TextAlign.Right"></TreeGridColumn>
    </TreeGridColumns>
</SfTreeGrid>

<style>
    .treeDropDown {
        display: flex;
        align-items: baseline;
    }
</style>

@code {
    private SfTreeGrid<ShipmentData>? TreeGrid;
    private string SelectedCategory = "Seafood";
    private readonly List<string> Categories = new()
    {
        "Seafood",
        "Dairy",
        "Edible"
    };
    public List<ShipmentData> TreeGridData { get; set; } = new();

    protected override void OnInitialized()
    {
        TreeGridData = ShipmentData.GetShipmentData().ToList();
    }

    /// <summary>
    /// Safe helper: Sum only the visible (current view) rows that match the selected category.
    /// Handles null TreeGrid reference with fallback to TreeGridData.
    /// </summary>
    private decimal SumForSelectedCategory()
    {
        IEnumerable<ShipmentData> view;
        if (TreeGrid != null)
        {
            var records = TreeGrid.GetCurrentViewRecords();
            view = records.OfType<ShipmentData>();
        }
        else
        {
            view = TreeGridData;
        }
        return view.Where(x => x.ShipmentCategory == SelectedCategory).Sum(x => (decimal)(x.TotalPrice ?? 0));
    }

    public class ShipmentData
    {
        public int? ID { get; set; }
        public int? ShipID { get; set; }
        public string? Name { get; set; }
        public int? Units { get; set; }
        public string? Category { get; set; }
        public int? UnitPrice { get; set; }
        public int? Price { get; set; }
        public int? TotalPrice { get; set; }
        public string? ShipmentCategory { get; set; }
        public DateTime? ShippedDate { get; set; } = null;
        public DateTime? OrderDate { get; set; } = null;
        public string? OrderReached { get; set; } = null;
        public int? ParentID { get; set; }

        private static void CalculateTotalPrices(List<ShipmentData> data)
        {
            var parents = data.Where(d => d.ParentID == null).ToList();
            foreach (var parent in parents)
            {
                var children = data.Where(d => d.ParentID == parent.ID).ToList();
                parent.TotalPrice = children.Sum(c => c.Price ?? 0);
            }
            foreach (var item in data.Where(d => d.ParentID != null))
            {
                item.TotalPrice = item.Price;
            }
        }

        public static List<ShipmentData> GetShipmentData()
        {
            List<ShipmentData> DataCollection = new()
            {
                new ShipmentData { ID = 0, Name = "Order 1", ParentID = null },
                new ShipmentData { ID = 1, ShipID = 4789, Name = "Mackerel", Category = "Seafood", Units = 25, UnitPrice = 12, Price = 300, OrderDate = new DateTime(2025, 3, 3), ShippedDate = new DateTime(2025, 3, 10), ShipmentCategory = "Seafood", OrderReached = "No", ParentID = 0 },
                new ShipmentData { ID = 2, ShipID = 4833, Name = "Herrings", Category = "Seafood", Units = 28, UnitPrice = 11, Price = 308, OrderDate = new DateTime(2025, 8, 5), ShippedDate = new DateTime(2025, 8, 15), ShipmentCategory = "Seafood", OrderReached = "Yes", ParentID = 0 },
                new ShipmentData { ID = 3, ShipID = 4567, Name = "Preserved Olives", Category = "Edible", Units = 22, UnitPrice = 9, Price = 198, OrderDate = new DateTime(2025, 6, 10), ShippedDate = new DateTime(2025, 6, 17), ShipmentCategory = "Edible", OrderReached = "Yes", ParentID = 0 },
                new ShipmentData { ID = 4, ShipID = 4899, Name = "Sweet corn", Category = "Edible", Units = 27, UnitPrice = 7, Price = 189, OrderDate = new DateTime(2025, 7, 12), ShippedDate = new DateTime(2025, 7, 19), ShipmentCategory = "Edible", OrderReached = "No", ParentID = 0 },
                new ShipmentData { ID = 5, Name = "Order 2", ParentID = null },
                new ShipmentData { ID = 6, ShipID = 5678, Name = "Tilapias", Category = "Seafood", Units = 24, UnitPrice = 15, Price = 360, OrderDate = new DateTime(2025, 2, 5), ShippedDate = new DateTime(2025, 2, 12), ShipmentCategory = "Seafood", OrderReached = "Yes", ParentID = 5 },
                new ShipmentData { ID = 7, ShipID = 5990, Name = "White Shrimp", Category = "Seafood", Units = 30, UnitPrice = 7, Price = 210, OrderDate = new DateTime(2025, 5, 22), ShippedDate = new DateTime(2025, 5, 29), ShipmentCategory = "Seafood", OrderReached = "Yes", ParentID = 5 },
                new ShipmentData { ID = 8, ShipID = 5476, Name = "Fresh Cheese", Category = "Dairy", Units = 26, UnitPrice = 12, Price = 312, OrderDate = new DateTime(2025, 6, 8), ShippedDate = new DateTime(2025, 6, 15), ShipmentCategory = "Dairy", OrderReached = "Yes", ParentID = 5 },
                new ShipmentData { ID = 9, ShipID = 5788, Name = "Blue Veined Cheese", Category = "Dairy", Units = 29, UnitPrice = 15, Price = 435, OrderDate = new DateTime(2025, 7, 10), ShippedDate = new DateTime(2025, 7, 17), ShipmentCategory = "Dairy", OrderReached = "No", ParentID = 5 },
                new ShipmentData { ID = 10, ShipID = 5896, Name = "Butter", Category = "Dairy", Units = 21, UnitPrice = 9, Price = 189, OrderDate = new DateTime(2025, 9, 18), ShippedDate = new DateTime(2025, 9, 25), ShipmentCategory = "Dairy", OrderReached = "Yes", ParentID = 5 }
            };
            CalculateTotalPrices(DataCollection);
            return DataCollection;
        }
    }
}
```

---

## Limitations

- By default, Footer Aggregate (total aggregate) is shown only for the current page records and not for the entire DataSource. To aggregate across all page records, set an adaptor in `SfDataManager`.

---
