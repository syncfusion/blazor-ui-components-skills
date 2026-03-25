# Custom Binding in Blazor DataManager

Custom binding lets you implement your own data retrieval and manipulation logic by creating a class that inherits from `DataAdaptor`. Use this when built-in adaptors don't fit your data source or when you need custom business rules during CRUD operations.

## Table of Contents
- [When to Use Custom Binding](#when-to-use-custom-binding)
- [DataAdaptor Abstract Class](#dataadaptor-abstract-class)
- [Performing Data Operations (Read)](#performing-data-operations-read)
- [Performing CRUD Operations](#performing-crud-operations)
- [Full Implementation Example](#full-implementation-example)

---

## When to Use Custom Binding

Custom binding is the right choice when:
- Data comes from a source that doesn't match any built-in adaptor (e.g., a non-standard REST API, a local database via EF Core, a third-party SDK)
- You need to apply custom business rules during CRUD operations (e.g., validation before insert, audit logging on delete)
- You want full control over how filtering, sorting, and paging are applied

---

## DataAdaptor Abstract Class

`DataAdaptor` is the abstract base class you derive from. Override the methods relevant to your use case:

```csharp
public abstract class DataAdaptor
{
    // Read operations
    public virtual object Read(DataManagerRequest dataManagerRequest, string key = null)
    public virtual Task<object> ReadAsync(DataManagerRequest dataManagerRequest, string key = null)

    // Insert operations
    public virtual object Insert(DataManager dataManager, object data, string key)
    public virtual Task<object> InsertAsync(DataManager dataManager, object data, string key)

    // Update operations
    public virtual object Update(DataManager dataManager, object data, string keyField, string key)
    public virtual Task<object> UpdateAsync(DataManager dataManager, object data, string keyField, string key)

    // Delete operations
    public virtual object Remove(DataManager dataManager, object data, string keyField, string key)
    public virtual Task<object> RemoveAsync(DataManager dataManager, object data, string keyField, string key)

    // Batch CRUD
    public virtual object BatchUpdate(DataManager dataManager, object changedRecords, object addedRecords,
        object deletedRecords, string keyField, string key, int? dropIndex)
    public virtual Task<object> BatchUpdateAsync(DataManager dataManager, object changedRecords,
        object addedRecords, object deletedRecords, string keyField, string key, int? dropIndex)
}
```

You only need to override the methods your scenario requires. If `Read`/`ReadAsync` is not overridden, the default handler processes the request.

---

## Performing Data Operations (Read)

Override `Read` or `ReadAsync` to apply searching, sorting, filtering, and paging using the built-in `DataOperations` helper class. The `DataManagerRequest` parameter provides all the query details.

**DataOperations methods available:**

| Method | What it does |
|---|---|
| `DataOperations.PerformSearching(source, search)` | Apply text search |
| `DataOperations.PerformSorting(source, sorted)` | Apply sort descriptors |
| `DataOperations.PerformFiltering(source, where, operator)` | Apply filter conditions |
| `DataOperations.PerformSkip(source, skip)` | Skip records for paging |
| `DataOperations.PerformTake(source, take)` | Take records for paging |
| `DataUtil.PerformAggregation(source, aggregates)` | Calculate sum/avg/min/max |

**Return type rules:**
- When `dm.RequiresCounts == true` → return a `DataResult` object with `Result` (data) and `Count` (total)
- When `dm.RequiresCounts == false` → return the collection directly

```csharp
public override object Read(DataManagerRequest dm, string key = null)
{
    IEnumerable<Order> dataSource = Orders;

    if (dm.Search?.Count > 0)
        dataSource = DataOperations.PerformSearching(dataSource, dm.Search);

    if (dm.Sorted?.Count > 0)
        dataSource = DataOperations.PerformSorting(dataSource, dm.Sorted);

    if (dm.Where?.Count > 0)
        dataSource = DataOperations.PerformFiltering(dataSource, dm.Where, dm.Where[0].Operator);

    int count = dataSource.Count();

    if (dm.Skip != 0)
        dataSource = DataOperations.PerformSkip(dataSource, dm.Skip);

    if (dm.Take != 0)
        dataSource = DataOperations.PerformTake(dataSource, dm.Take);

    return dm.RequiresCounts
        ? new DataResult() { Result = dataSource, Count = count }
        : (object)dataSource;
}
```

---

## Performing CRUD Operations

Override the relevant methods to handle data changes. The `SfGrid` triggers these automatically when editing is configured.

```csharp
public override object Insert(DataManager dm, object value, string key)
{
    Orders.Insert(0, value as Order);
    return value;
}

public override object Remove(DataManager dm, object value, string keyField, string key)
{
    Orders.Remove(Orders.FirstOrDefault(o => o.OrderID == int.Parse(value.ToString())));
    return value;
}

public override object Update(DataManager dm, object value, string keyField, string key)
{
    var existing = Orders.FirstOrDefault(o => o.OrderID == (value as Order).OrderID);
    if (existing != null)
    {
        existing.CustomerID = (value as Order).CustomerID;
        existing.Freight = (value as Order).Freight;
    }
    return value;
}

public override object BatchUpdate(DataManager dm, object changed, object added, object deleted,
    string keyField, string key, int? dropIndex)
{
    if (changed is IEnumerable<Order> changedRecords)
        foreach (var rec in changedRecords)
        {
            var existing = Orders.FirstOrDefault(o => o.OrderID == rec.OrderID);
            if (existing != null) existing.CustomerID = rec.CustomerID;
        }

    if (added is IEnumerable<Order> addedRecords)
        foreach (var rec in addedRecords)
            Orders.Add(rec);

    if (deleted is IEnumerable<Order> deletedRecords)
        foreach (var rec in deletedRecords)
            Orders.RemoveAll(o => o.OrderID == rec.OrderID);

    return Orders;
}
```

---

## Full Implementation Example

This example shows the complete pattern: the custom adaptor class defined inline in the Razor component, bound to a Grid with full CRUD support.

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Grids

<SfGrid TValue="Order" ID="Grid" AllowSorting="true" AllowFiltering="true" AllowPaging="true"
        Toolbar="@(new List<string>() { "Add", "Delete", "Update", "Cancel" })">
    <SfDataManager AdaptorInstance="@typeof(CustomAdaptor)" Adaptor="Adaptors.CustomAdaptor">
    </SfDataManager>
    <GridPageSettings PageSize="8"></GridPageSettings>
    <GridEditSettings AllowEditing="true" AllowDeleting="true" AllowAdding="true"
                      Mode="@EditMode.Normal">
    </GridEditSettings>
    <GridColumns>
        <GridColumn Field="@nameof(Order.OrderID)" HeaderText="Order ID" IsPrimaryKey="true"
                    TextAlign="TextAlign.Center" Width="140"></GridColumn>
        <GridColumn Field="@nameof(Order.CustomerID)" HeaderText="Customer Name" Width="150"></GridColumn>
        <GridColumn Field="@nameof(Order.Freight)" HeaderText="Freight" Width="150"></GridColumn>
    </GridColumns>
</SfGrid>

@code {
    public static List<Order> Orders { get; set; } = new();

    protected override void OnInitialized()
    {
        Orders = Enumerable.Range(1, 75).Select(x => new Order()
        {
            OrderID = 1000 + x,
            CustomerID = new[] { "ALFKI", "ANANTR", "ANTON", "BLONP", "BOLID" }[new Random().Next(5)],
            Freight = 2.1 * x
        }).ToList();
    }

    public class Order
    {
        public int OrderID { get; set; }
        public string? CustomerID { get; set; }
        public double Freight { get; set; }
    }

    public class CustomAdaptor : DataAdaptor
    {
        public override object Read(DataManagerRequest dm, string key = null)
        {
            IEnumerable<Order> dataSource = Orders;

            if (dm.Search?.Count > 0)
                dataSource = DataOperations.PerformSearching(dataSource, dm.Search);

            if (dm.Sorted?.Count > 0)
                dataSource = DataOperations.PerformSorting(dataSource, dm.Sorted);

            if (dm.Where?.Count > 0)
                dataSource = DataOperations.PerformFiltering(dataSource, dm.Where, dm.Where[0].Operator);

            int count = dataSource.Count();

            if (dm.Skip != 0)
                dataSource = DataOperations.PerformSkip(dataSource, dm.Skip);

            if (dm.Take != 0)
                dataSource = DataOperations.PerformTake(dataSource, dm.Take);

            return dm.RequiresCounts
                ? new DataResult() { Result = dataSource, Count = count }
                : (object)dataSource;
        }

        public override object Insert(DataManager dm, object value, string key)
        {
            Orders.Insert(0, value as Order);
            return value;
        }

        public override object Remove(DataManager dm, object value, string keyField, string key)
        {
            Orders.Remove(Orders.FirstOrDefault(o => o.OrderID == int.Parse(value.ToString())));
            return value;
        }

        public override object Update(DataManager dm, object value, string keyField, string key)
        {
            var data = Orders.FirstOrDefault(o => o.OrderID == (value as Order).OrderID);
            if (data != null)
            {
                data.CustomerID = (value as Order).CustomerID;
                data.Freight = (value as Order).Freight;
            }
            return value;
        }

        public override object BatchUpdate(DataManager dm, object changed, object added,
            object deleted, string keyField, string key, int? dropIndex)
        {
            if (changed is IEnumerable<Order> changedRecords)
                foreach (var rec in changedRecords)
                {
                    var existing = Orders.FirstOrDefault(o => o.OrderID == rec.OrderID);
                    if (existing != null) existing.CustomerID = rec.CustomerID;
                }

            if (added is IEnumerable<Order> addedRecords)
                foreach (var rec in addedRecords)
                    Orders.Add(rec);

            if (deleted is IEnumerable<Order> deletedRecords)
                foreach (var rec in deletedRecords)
                    Orders.RemoveAll(o => o.OrderID == rec.OrderID);

            return Orders;
        }
    }
}
```

**Key configuration points:**
- `AdaptorInstance="@typeof(CustomAdaptor)"` — references the adaptor class type
- `Adaptor="Adaptors.CustomAdaptor"` — tells DataManager to use the custom path
- `IsPrimaryKey="true"` on the key column — required for Update and Remove operations to work correctly
- `GridEditSettings` — required for CRUD; set `Mode` based on your UX preference (`Normal`, `Dialog`, `Batch`)
