# Custom Binding in Syncfusion Blazor TreeGrid

## Table of Contents
- [Overview](#overview)
- [DataAdaptor Class](#dataadaptor-class)
- [Read / ReadAsync Override](#read--readasync-override)
- [CRUD Overrides](#crud-overrides)
- [Inject Service into Custom Adaptor](#inject-service-into-custom-adaptor)
- [Custom Adaptor as Component](#custom-adaptor-as-component)
- [Full Example with Paging & Sorting](#full-example-with-paging--sorting)
- [Limitations](#limitations)

---

## Overview

Custom binding lets you take full manual control over data operations (fetch, insert, update, delete) using the `DataAdaptor` class. Use this when:
- Your API has non-standard response formats
- You need to apply complex business logic during data fetch
- You want to intercept and transform data before it reaches the TreeGrid

> **Limitation:** Custom binding only supports **self-referential (flat) data** — `IdMapping` and `ParentIdMapping`. Hierarchical (`ChildMapping`) is not supported.

---

## DataAdaptor Class

Create a class that inherits `DataAdaptor` and override the methods you need.

### Available Method Signatures

The `DataAdaptor` abstract class provides both synchronous and asynchronous method signatures that can be overridden in your custom adaptor:

```csharp
public abstract class DataAdaptor
{
    /// <summary>
    /// Performs data Read operation synchronously.
    /// </summary>
    public virtual object Read(DataManagerRequest dataManagerRequest, string key = null)

    /// <summary>
    /// Performs data Read operation asynchronously.
    /// </summary>
    public virtual Task<object> ReadAsync(DataManagerRequest dataManagerRequest, string key = null)

    /// <summary>
    /// Performs Insert operation synchronously.
    /// </summary>
    public virtual object Insert(DataManager dataManager, object data, string key)
    
    /// <summary>
    /// Performs Insert operation asynchronously.
    /// </summary>
    public virtual Task<object> InsertAsync(DataManager dataManager, object data, string key)

    /// <summary>
    /// Performs Remove operation synchronously.
    /// </summary>
    public virtual object Remove(DataManager dataManager, object data, string keyField, string key)

    /// <summary>
    /// Performs Remove operation asynchronously.
    /// </summary>
    public virtual Task<object> RemoveAsync(DataManager dataManager, object data, string keyField, string key)

    /// <summary>
    /// Performs Update operation synchronously.
    /// </summary>
    public virtual object Update(DataManager dataManager, object data, string keyField, string key)

    /// <summary>
    /// Performs Update operation asynchronously.
    /// </summary>
    public virtual Task<object> UpdateAsync(DataManager dataManager, object data, string keyField, string key)

    /// <summary>
    /// Performs Batch CRUD operations synchronously.
    /// </summary>
    public virtual object BatchUpdate(DataManager dataManager, object changedRecords, object addedRecords, object deletedRecords, string keyField, string key, int? dropIndex)

    /// <summary>
    /// Performs Batch CRUD operations asynchronously.
    /// </summary>
    public virtual Task<object> BatchUpdateAsync(DataManager dataManager, object changedRecords, object addedRecords, object deletedRecords, string keyField, string key, int? dropIndex)
}
```

Override the methods you need based on your data operations (synchronous vs. asynchronous).

### Basic Example

Here's a simple custom adaptor with a Read override:

```csharp
using Syncfusion.Blazor;
using Syncfusion.Blazor.Data;

public class CustomAdaptor : DataAdaptor
{
    // Static data for demonstration
    public static List<BusinessObject> TreeData = SampleData.GetTreeData();

    // Override Read for synchronous data fetch
    public override object Read(DataManagerRequest dm, string key = null)
    {
        IEnumerable<BusinessObject> DataSource = TreeData;
        int count = DataSource.Cast<BusinessObject>().Count();

        // Apply paging
        if (dm.Skip != 0) DataSource = DataSource.Skip(dm.Skip);
        if (dm.Take != 0) DataSource = DataSource.Take(dm.Take);

        return dm.RequiresCounts
            ? new DataResult { Result = DataSource, Count = count }
            : (object)DataSource;
    }
}
```

---

## Read / ReadAsync Override

**Synchronous Read:**
```csharp
public override object Read(DataManagerRequest dm, string key = null)
{
    IEnumerable<BusinessObject> data = MyDataStore.GetAll();
    int totalCount = data.Count();

    // Sorting
    if (dm.Sorted?.Count > 0)
        data = DataOperations.PerformSorting(data, dm.Sorted);

    // Filtering
    if (dm.Where != null)
        data = DataOperations.PerformFiltering(data, dm.Where, dm.Where[0].Operator);

    // Searching
    if (dm.Search?.Count > 0)
        data = DataOperations.PerformSearching(data, dm.Search);

    int filteredCount = data.Count();

    // Paging
    if (dm.Skip != 0) data = data.Skip(dm.Skip);
    if (dm.Take != 0) data = data.Take(dm.Take);

    return dm.RequiresCounts
        ? new DataResult { Result = data, Count = filteredCount }
        : (object)data;
}
```

**Asynchronous ReadAsync:**
```csharp
public override async Task<object> ReadAsync(DataManagerRequest dm, string key = null)
{
    var data = await MyApiService.GetDataAsync();
    int count = data.Count;

    if (dm.Skip != 0) data = data.Skip(dm.Skip).ToList();
    if (dm.Take != 0) data = data.Take(dm.Take).ToList();

    return dm.RequiresCounts
        ? new DataResult { Result = data, Count = count }
        : (object)data;
}
```

> **Important Note:** If the `DataManagerRequest.RequiresCounts` value is **true**, then the Read/ReadAsync return value must be of `DataResult` with properties:
> - `Result` - the collection of records
> - `Count` - the total number of records
>
> If `DataManagerRequest.RequiresCounts` is **false**, simply return the collection of records.
>
> If the Read/ReadAsync method is not overridden in the custom adaptor, it will be handled by the default read handler.

---

## CRUD Overrides

> **Note:** While using batch editing in TreeGrid, use `BatchUpdate` / `BatchUpdateAsync` method to handle the corresponding CRUD operation.

```csharp
// INSERT
public override object Insert(DataManager dm, object value, string key)
{
    var record = (BusinessObject)value;
    record.TaskId = TreeData.Max(t => t.TaskId) + 1;
    TreeData.Insert(0, record);
    return value;
}

// UPDATE
public override object Update(DataManager dm, object value, string keyField, string key)
{
    var updated = (BusinessObject)value;
    var existing = TreeData.FirstOrDefault(t => t.TaskId == updated.TaskId);
    if (existing != null)
    {
        existing.TaskName = updated.TaskName;
        existing.Duration = updated.Duration;
        existing.Progress = updated.Progress;
    }
    return value;
}

// DELETE
public override object Remove(DataManager dm, object value, string keyField, string key)
{
    int id = (int)value;
    TreeData.RemoveAll(t => t.TaskId == id);
    return value;
}

// BATCH (for Batch edit mode)
public override object BatchUpdate(DataManager dm, object changed, object added,
    object deleted, string keyField, string key, int? dropIndex)
{
    if (changed is List<BusinessObject> changedList)
        foreach (var item in changedList)
            Update(dm, item, keyField, key);

    if (added is List<BusinessObject> addedList)
        foreach (var item in addedList)
            Insert(dm, item, key);

    if (deleted is List<BusinessObject> deletedList)
        foreach (var item in deletedList)
            Remove(dm, item.TaskId, keyField, key);

    return new { addedRecords = added, changedRecords = changed, deletedRecords = deleted };
}
```

**Asynchronous CRUD Methods:**

For asynchronous operations, override the async versions:

```csharp
// INSERT ASYNC
public override async Task<object> InsertAsync(DataManager dm, object value, string key)
{
    var record = (BusinessObject)value;
    record.TaskId = TreeData.Max(t => t.TaskId) + 1;
    TreeData.Insert(0, record);
    await Task.Delay(10); // Simulate async operation
    return value;
}

// UPDATE ASYNC
public override async Task<object> UpdateAsync(DataManager dm, object value, string keyField, string key)
{
    var updated = (BusinessObject)value;
    var existing = TreeData.FirstOrDefault(t => t.TaskId == updated.TaskId);
    if (existing != null)
    {
        existing.TaskName = updated.TaskName;
        existing.Duration = updated.Duration;
        existing.Progress = updated.Progress;
    }
    await Task.Delay(10); // Simulate async operation
    return value;
}

// REMOVE ASYNC
public override async Task<object> RemoveAsync(DataManager dm, object value, string keyField, string key)
{
    int id = (int)value;
    TreeData.RemoveAll(t => t.TaskId == id);
    await Task.Delay(10); // Simulate async operation
    return value;
}

// BATCH UPDATE ASYNC
public override async Task<object> BatchUpdateAsync(DataManager dm, object changed, object added,
    object deleted, string keyField, string key, int? dropIndex)
{
    if (changed is List<BusinessObject> changedList)
        foreach (var item in changedList)
            await UpdateAsync(dm, item, keyField, key);

    if (added is List<BusinessObject> addedList)
        foreach (var item in addedList)
            await InsertAsync(dm, item, key);

    if (deleted is List<BusinessObject> deletedList)
        foreach (var item in deletedList)
            await RemoveAsync(dm, item.TaskId, keyField, key);

    return new { addedRecords = added, changedRecords = changed, deletedRecords = deleted };
}
```

---

## Inject Service into Custom Adaptor

To inject services (like a data access layer or API service) into your custom adaptor, register both the adaptor and the service in `Program.cs`:

```csharp
// Program.cs
builder.Services.AddSingleton<TaskDataAccessLayer>();
builder.Services.AddScoped<CustomAdaptor>();
builder.Services.AddScoped<MyApiService>();
```

Then use the `[Inject]` attribute in your custom adaptor:

```csharp
public class CustomAdaptor : DataAdaptor
{
    [Inject]
    public TaskDataAccessLayer DataService { get; set; }

    public override object Read(DataManagerRequest dm, string key = null)
    {
        // Use injected service to fetch data
        IEnumerable<BusinessObject> data = DataService.GetTree();
        
        // Apply sorting, filtering, paging...
        if (dm.Sorted?.Count > 0)
            data = DataOperations.PerformSorting(data, dm.Sorted);

        if (dm.Where?.Count > 0)
            data = DataOperations.PerformFiltering(data, dm.Where, dm.Where[0].Operator);

        int count = data.Count();
        if (dm.Skip != 0) data = data.Skip(dm.Skip);
        if (dm.Take != 0) data = data.Take(dm.Take);

        return dm.RequiresCounts 
            ? new DataResult { Result = data, Count = count } 
            : (object)data;
    }
}
```

In your component:

```razor
<SfTreeGrid TValue="BusinessObject" IdMapping="TaskId" ParentIdMapping="ParentId" TreeColumnIndex="1">
    <SfDataManager AdaptorInstance="@typeof(CustomAdaptor)" Adaptor="Adaptors.CustomAdaptor" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="Task ID" Width="80" IsPrimaryKey="true" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="200" />
    </TreeGridColumns>
</SfTreeGrid>
```

---

## Custom Adaptor as Component

Create a custom adaptor as a Blazor component by extending `DataAdaptor` from `OwningComponentBase`. This approach allows you to request multiple services via `ScopedServices`.

**Step 1:** Register your service in `Program.cs`:

```csharp
builder.Services.AddScoped<SelfReferenceData>();
```

**Step 2:** Create a custom adaptor component (e.g., `CustomAdaptorComponent.razor`):

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
@using Newtonsoft.Json

@inherits DataAdaptor

<CascadingValue Value="@this">
    @ChildContent
</CascadingValue>

@code {
    [Parameter]
    [JsonIgnore]
    public RenderFragment ChildContent { get; set; }

    SelfReferenceData TreeDataService;

    public override object Read(DataManagerRequest dm, string key = null)
    {
        // Access service via ScopedServices
        TreeDataService = (SelfReferenceData)ScopedServices.GetService(typeof(SelfReferenceData));
        IEnumerable<SelfReferenceData> data = TreeDataService.GetTree().Take(30);

        // Apply operations
        if (dm.Search?.Count > 0)
            data = DataOperations.PerformSearching(data, dm.Search);

        if (dm.Sorted?.Count > 0)
            data = DataOperations.PerformSorting(data, dm.Sorted);

        if (dm.Where?.Count > 0)
            data = DataOperations.PerformFiltering(data, dm.Where, dm.Where[0].Operator);

        int count = data.Count();

        if (dm.Skip != 0) data = data.Skip(dm.Skip);
        if (dm.Take != 0) data = data.Take(dm.Take);

        return dm.RequiresCounts 
            ? new DataResult { Result = data, Count = count } 
            : (object)data;
    }
}
```

**Step 3:** Use the custom adaptor component in your TreeGrid:

```razor
<SfTreeGrid TValue="SelfReferenceData" 
            IdMapping="TaskID" 
            ParentIdMapping="ParentID" 
            HasChildMapping="isParent"
            TreeColumnIndex="1" 
            AllowPaging="true" 
            AllowSorting="true"
            Toolbar="@(new List<string> { "Add", "Edit", "Delete", "Update", "Cancel" })">
    <SfDataManager Adaptor="Adaptors.CustomAdaptor">
        <CustomAdaptorComponent />
    </SfDataManager>
    <TreeGridPageSettings PageSize="3" />
    <TreeGridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true" Mode="EditMode.Row" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskID" HeaderText="Task ID" Width="80" IsPrimaryKey="true" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="145" />
        <TreeGridColumn Field="Duration" HeaderText="Duration" Width="100" />
    </TreeGridColumns>
</SfTreeGrid>
```

> **Note:** When creating a custom adaptor as a component, use `OwningComponentBase` to access scoped services through the `ScopedServices` property.

---

## Full Example with Paging & Sorting

**Component (.razor):**
```razor
@using Syncfusion.Blazor.TreeGrid
@using Syncfusion.Blazor.Data

<SfTreeGrid TValue="BusinessObject"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            TreeColumnIndex="1"
            AllowPaging="true"
            AllowSorting="true"
            Toolbar="@(new List<string> { "Add", "Edit", "Delete", "Update", "Cancel" })">
    <SfDataManager AdaptorInstance="@typeof(CustomAdaptor)" Adaptor="Adaptors.CustomAdaptor" />
    <TreeGridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true" Mode="EditMode.Row" />
    <TreeGridPageSettings PageSize="5" />
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId"   HeaderText="ID"        Width="80"  IsPrimaryKey="true" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="200" />
        <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="100" />
        <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="100" />
    </TreeGridColumns>
</SfTreeGrid>
```

---

## Limitations

| Limitation | Detail |
|---|---|
| Self-referential only | `ChildMapping` (hierarchical) data is not supported |
| Async CRUD | For async CRUD, override `InsertAsync`, `UpdateAsync`, `RemoveAsync` |
| DataOperations | Use `Syncfusion.Blazor.DataOperations` helpers for sorting, filtering, searching |
| Batch mode | Override `BatchUpdate` when using `EditMode.Batch` |

---
