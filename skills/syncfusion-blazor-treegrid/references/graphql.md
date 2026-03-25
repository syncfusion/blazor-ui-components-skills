# GraphQL Adaptor in Syncfusion Blazor TreeGrid

## Table of Contents
- [Overview](#overview)
- [Configure a GraphQL Server](#configure-a-graphql-server)
- [Connecting TreeGrid to GraphQL Service](#connecting-treegrid-to-graphql-service)
- [DataManagerRequestInput Class](#datamanagerrequestinput-class)
- [Handling Searching](#handling-searching-operation)
- [Handling Filtering](#handling-filtering-operation)
- [Handling Sorting](#handling-sorting-operation)
- [Handling Paging](#handling-paging-operation)
- [CRUD Operations via Mutation](#crud-operations-via-mutation)
  - [Insert](#insert-operation)
  - [Update](#update-operation)
  - [Delete](#delete-operation)

---

## Overview

GraphQL is a powerful query language for APIs, enabling precise data fetching. The Syncfusion Blazor TreeGrid integrates with GraphQL servers using [GraphQLAdaptor](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Data.GraphQLAdaptor.html) inside `SfDataManager`. It supports:

- **Read** with paging, sorting, filtering, and search
- **CRUD** (Create, Read, Update, Delete) via GraphQL mutations
- **Hierarchical / parent-child** data with `IdMapping` / `ParentIdMapping`

The adaptor expects responses in the shape:
```json
{
  "data": {
    "resolverName": {
      "count": 100,
      "result": [...],
      "items": [...]
    }
  }
}
```

---

## Configure a GraphQL Server

Use **Hot Chocolate** (`HotChocolate.AspNetCore`) for the GraphQL server.

### Step 1 – Install Hot Chocolate
```powershell
Install-Package HotChocolate.AspNetCore
```

### Step 2 – Create the Model
```csharp
// Models/EmployeeData.cs
public class EmployeeData
{
    [GraphQLName("employeeID")] public string EmployeeID { get; set; } = default!;
    [GraphQLName("firstName")]  public string? FirstName { get; set; }
    [GraphQLName("lastName")]   public string? LastName  { get; set; }
    [GraphQLName("title")]      public string? Title     { get; set; }
    [GraphQLName("managerID")]  public string? ManagerID { get; set; }
    [GraphQLName("hasChild")]   public bool HasChild     { get; set; }
    [GraphQLName("name")]       public string? Name      { get; set; }
    [GraphQLName("location")]   public string? Location  { get; set; }
    // ... additional fields as needed
}
```

### Step 3 – Define GraphQL Query Resolver
```csharp
// GraphQLQuery.cs
public class GraphQLQuery
{
    public EmployeesDataResponse EmployeesData(DataManagerRequestInput dataManager)
    {
        // Detect parent ID for child slices
        string? parentId = TryGetParentIdFromParams(dataManager?.Params)
                        ?? TryGetParentIdFromWhere(dataManager?.Where);

        if (!string.IsNullOrWhiteSpace(parentId))
        {
            var children = _data
                .Where(d => string.Equals(d.ManagerID, parentId, StringComparison.OrdinalIgnoreCase))
                .ToList();
            return new EmployeesDataResponse { Count = children.Count, Result = children, Items = children };
        }

        // Root records with paging
        var roots = _data.Where(d => string.IsNullOrWhiteSpace(d.ManagerID)).ToList();
        int total  = roots.Count;
        IEnumerable<EmployeeData> page = roots;
        if (dataManager?.Skip is int sk && dataManager.Take is int tk)
            page = page.Skip(sk).Take(tk);

        var list = page.ToList();
        return new EmployeesDataResponse { Count = total, Result = list, Items = list };
    }

    private static List<EmployeeData> _data = EmployeeData.GetAllRecords();
}

// Response wrapper
public class EmployeesDataResponse
{
    [GraphQLName("count")]  public int Count { get; set; }
    [GraphQLName("result")] public List<EmployeeData> Result { get; set; } = new();
    [GraphQLName("items")]  public List<EmployeeData> Items  { get; set; } = new();
}
```

### Step 4 – Configure Program.cs
```csharp
var builder = WebApplication.CreateBuilder(args);
builder.Services.AddGraphQLServer().AddQueryType<GraphQLQuery>();

// Enable CORS for the Blazor client
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowBlazorApp", policy =>
        policy.WithOrigins("https://localhost:xxxx")
              .AllowAnyHeader()
              .AllowAnyMethod()
              .AllowCredentials());
});

var app = builder.Build();
app.UseRouting();
app.UseCors("AllowBlazorApp");
app.MapGraphQL();   // maps to /graphql by default
app.Run();
```

---

## Connecting TreeGrid to GraphQL Service

### Blazor Component Setup

```razor
@rendermode InteractiveServer
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.TreeGrid

<SfTreeGrid TValue="EmployeeData"
            IdMapping="EmployeeID"
            ParentIdMapping="ManagerID"
            HasChildMapping="HasChild"
            TreeColumnIndex="1"
            AllowPaging="true">
    <SfDataManager Url="https://localhost:xxxx/graphql/"
                   Adaptor="Adaptors.GraphQLAdaptor"
                   GraphQLAdaptorOptions="@adaptorOptions" />
    <TreeGridPageSettings PageSize="5" />
    <TreeGridColumns>
        <TreeGridColumn Field="EmployeeID" HeaderText="ID" IsPrimaryKey="true" Width="140" />
        <TreeGridColumn Field="Name"       HeaderText="Name"  Width="180" />
        <TreeGridColumn Field="Title"      HeaderText="Title" Width="170" />
        <TreeGridColumn Field="Location"   HeaderText="Location" Width="140" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    private const string HrQuery = @"
    query employeesData($dataManager: DataManagerRequestInput!) {
      employeesData(dataManager: $dataManager) {
        count
        result {
          employeeID managerID hasChild name lastName title location
          dateJoined salaryPerMonth email
        }
      }
    }";

    private GraphQLAdaptorOptions adaptorOptions = new GraphQLAdaptorOptions
    {
        Query        = HrQuery,
        ResolverName = "employeesData"   // must match GraphQL field name (camelCase)
    };
}
```

**Key properties:**
| Property | Description |
|---|---|
| `Query` | GraphQL query string sent on every read |
| `ResolverName` | Matches the GraphQL field name in the response (`camelCase`) |
| `Mutation` | GraphQL mutations for CRUD (Insert / Update / Delete) |

---

## DataManagerRequestInput Class

The TreeGrid sends this object as the `$dataManager` variable in every request:

```csharp
public class DataManagerRequestInput
{
    [GraphQLName("Skip")]  public int Skip  { get; set; }
    [GraphQLName("Take")]  public int Take  { get; set; }
    [GraphQLName("RequiresCounts")] public bool RequiresCounts { get; set; }

    [GraphQLName("Params")]    [GraphQLType(typeof(AnyType))] public IDictionary<string, object> Params { get; set; }
    [GraphQLName("Search")]    public List<SearchFilter>?  Search  { get; set; }
    [GraphQLName("Sorted")]    public List<Sort>?          Sorted  { get; set; }
    [GraphQLName("Where")]     [GraphQLType(typeof(AnyType))] public List<WhereFilter>? Where { get; set; }
    [GraphQLName("Group")]     public List<string>?        Group   { get; set; }
    [GraphQLName("LazyLoad")]  public bool? LazyLoad { get; set; }
    // ... additional properties (IdMapping, Select, Expand, Distinct, etc.)
}
```

**Child-row detection:** When the user expands a parent row, the TreeGrid sends either:
- `Params["parentId"]` — preferred check
- `Where` containing `ManagerID == <parentId>` — fallback check

---

## Handling Searching Operation

Enable the toolbar search item and process `dataManager.Search` on the server:

**Blazor:**
```razor
<SfTreeGrid ...
            Toolbar="@(new List<string>{ "Search" })"
            AllowPaging="true">
    <SfDataManager ... GraphQLAdaptorOptions="@adaptorOptions" />
    ...
</SfTreeGrid>
```

**Server-side search logic:**
```csharp
private static IEnumerable<EmployeeData> ApplySearch(
    IEnumerable<EmployeeData> data, List<SearchFilter> searches)
{
    foreach (var s in searches)
    {
        if (string.IsNullOrWhiteSpace(s.Key)) continue;
        var cmp = s.IgnoreCase ? StringComparison.OrdinalIgnoreCase : StringComparison.Ordinal;
        var fields = s.Fields ?? new List<string> { "name", "title", "location" };

        data = data.Where(e => fields.Any(f =>
        {
            var val = GetFieldString(e, f);
            return !string.IsNullOrEmpty(val) && val.IndexOf(s.Key, cmp) >= 0;
        }));
    }
    return data;
}
```

**Search applies to both roots and children slices** — run `ApplySearch` after detecting the parent slice:
```csharp
// In EmployeesData resolver after getting children/roots list:
data = ApplySearch(data, dataManager?.Search ?? new List<SearchFilter>());
```

---

## Handling Filtering Operation

Enable `AllowFiltering` and process `dataManager.Where` on the server:

**Blazor:**
```razor
<SfTreeGrid ... AllowFiltering="true">
```

**Server-side filter logic (supporting nested AND/OR predicates):**
```csharp
private static IEnumerable<EmployeeData> ApplyWhereExcludingParent(
    IEnumerable<EmployeeData> data, List<WhereFilter> where)
{
    if (where == null || where.Count == 0) return data;

    bool Match(EmployeeData e, WhereFilter f)
    {
        // Skip ManagerID conditions — those are used for parent detection, not filtering
        if ((f.Field ?? "").Equals("ManagerID", StringComparison.OrdinalIgnoreCase)) return true;

        var op    = (f.Operator ?? "equal").ToLowerInvariant();
        var cmp   = (f.IgnoreCase ?? true) ? StringComparison.OrdinalIgnoreCase : StringComparison.Ordinal;
        var right = f.Value?.ToString() ?? "";
        var left  = GetFieldString(e, f.Field ?? "") ?? "";

        return op switch
        {
            "equal" or "eq"         => string.Equals(left, right, cmp),
            "notequal" or "ne"      => !string.Equals(left, right, cmp),
            "contains"              => left.IndexOf(right, cmp) >= 0,
            "startswith"            => left.StartsWith(right, cmp),
            "endswith"              => left.EndsWith(right, cmp),
            _                       => string.Equals(left, right, cmp)
        };
    }

    IEnumerable<EmployeeData> ApplyNode(IEnumerable<EmployeeData> src, WhereFilter f)
    {
        if (f.IsComplex == true && f.Predicates?.Count > 0)
        {
            var condition = (f.Condition ?? "and").ToLowerInvariant();
            if (condition == "or")
            {
                var bag = new List<EmployeeData>();
                var seen = new HashSet<string>(StringComparer.OrdinalIgnoreCase);
                foreach (var p in f.Predicates)
                    foreach (var item in ApplyNode(src, p))
                        if (seen.Add(item.EmployeeID)) bag.Add(item);
                return bag;
            }
            else
            {
                var cur = src;
                foreach (var p in f.Predicates) cur = ApplyNode(cur, p);
                return cur;
            }
        }
        return src.Where(e => Match(e, f));
    }

    var result = data;
    foreach (var f in where) result = ApplyNode(result, f);
    return result;
}
```

---

## Handling Sorting Operation

Enable `AllowSorting` and process `dataManager.Sorted` on the server:

**Blazor:**
```razor
<SfTreeGrid ... AllowSorting="true">
```

**Server-side sort logic:**
```csharp
private static List<EmployeeData> SortListStable(
    List<EmployeeData> list, List<Sort>? sorts)
{
    if (sorts == null || sorts.Count == 0)
        return list.OrderBy(x => x.EmployeeID, StringComparer.OrdinalIgnoreCase).ToList();

    IOrderedEnumerable<EmployeeData>? ordered = null;
    for (int i = 0; i < sorts.Count; i++)
    {
        var s    = sorts[i];
        bool desc = s.Direction?.StartsWith("desc", StringComparison.OrdinalIgnoreCase) ?? false;

        Func<EmployeeData, object?> key = s.Name switch
        {
            "employeeID"     => e => e.EmployeeID,
            "name"           => e => e.Name,
            "title"          => e => e.Title,
            "location"       => e => e.Location,
            "dateJoined"     => e => e.DateJoined,
            "salaryPerMonth" => e => e.SalaryPerMonth,
            "priority"       => e => e.Priority,
            "progress"       => e => e.Progress,
            _                => e => e.EmployeeID
        };

        ordered = i == 0
            ? (desc ? list.OrderByDescending(key) : list.OrderBy(key))
            : (desc ? ordered!.ThenByDescending(key) : ordered!.ThenBy(key));
    }
    return ordered!.ThenBy(x => x.EmployeeID, StringComparer.OrdinalIgnoreCase).ToList();
}
```

Apply sorting in the resolver before paging:
```csharp
roots = SortListStable(roots, dataManager?.Sorted);
// ...then apply Skip/Take
```

---

## Handling Paging Operation

Enable `AllowPaging` and process `dataManager.Skip` / `dataManager.Take` on the server.

The **Count** in the response must reflect the **total un-paged root record count** so the TreeGrid renders the correct number of pager pages.

```csharp
var roots = _data.Where(d => string.IsNullOrWhiteSpace(d.ManagerID)).ToList();
int total = roots.Count;   // total before paging — required for pager

IEnumerable<EmployeeData> page = roots;
if (dataManager?.Skip is int sk && dataManager.Take is int tk)
    page = page.Skip(sk).Take(tk);

return new EmployeesDataResponse
{
    Count  = total,        // total count for pager
    Result = page.ToList(),
    Items  = page.ToList()
};
```

**Important:** Child slices (expanding a parent) must **not** apply paging — return all children with `Count = children.Count`.

---

## CRUD Operations via Mutation

### Setup in Blazor
```csharp
private GraphQLAdaptorOptions adaptorOptions = new GraphQLAdaptorOptions
{
    Query        = HrQuery,
    ResolverName = "employeesData",
    Mutation     = new GraphQLMutation
    {
        Insert = @"
        mutation create($record: EmployeeDataInput!, $index: Int!, $action: String!, $additionalParameters: Any) {
          createEmployee(record: $record, index: $index, action: $action, additionalParameters: $additionalParameters) {
            employeeID managerID hasChild name lastName title location
            dateJoined salaryPerMonth email
          }
        }",
        Update = @"
        mutation update($record: EmployeeDataInput!, $action: String!, $primaryColumnName: String!, $primaryColumnValue: String!, $additionalParameters: Any) {
          updateEmployee(record: $record, action: $action, primaryColumnName: $primaryColumnName, primaryColumnValue: $primaryColumnValue, additionalParameters: $additionalParameters) {
            employeeID managerID hasChild name lastName title location
            dateJoined salaryPerMonth email
          }
        }",
        Delete = @"
        mutation delete($primaryColumnValue: String!, $action: String!, $primaryColumnName: String!, $additionalParameters: Any) {
          deleteEmployee(primaryColumnValue: $primaryColumnValue, action: $action, primaryColumnName: $primaryColumnName, additionalParameters: $additionalParameters) {
            employeeID managerID hasChild name lastName title location
          }
        }"
    }
};
```

Enable editing on the TreeGrid:
```razor
<SfTreeGrid ... Toolbar="@(new List<string> { "Add", "Edit", "Delete", "Update", "Cancel" })">
    <TreeGridEditSettings AllowAdding="true" AllowEditing="true" AllowDeleting="true"
                          Mode="EditMode.Row" />
```

---

### Insert Operation

**Parameters sent to server:**

| Parameter | Description |
|---|---|
| `record` | The new record to insert |
| `index` | Position to insert at |
| `action` | `"Add"` |
| `additionalParameters` | Optional extra data |

**Server implementation:**
```csharp
public EmployeeData CreateEmployee(
    EmployeeData record,
    int index,
    string action,
    [GraphQLType(typeof(AnyType))] IDictionary<string, object> additionalParameters)
{
    var employees = EmployeeData.GetAllRecords();
    var entity = new EmployeeData
    {
        EmployeeID = record.EmployeeID,
        ManagerID  = record.ManagerID,
        Name       = record.Name,
        // ... copy all fields
    };
    // Update parent HasChild flag
    if (!string.IsNullOrWhiteSpace(entity.ManagerID))
    {
        var manager = employees.FirstOrDefault(e =>
            string.Equals(e.EmployeeID, entity.ManagerID, StringComparison.OrdinalIgnoreCase));
        if (manager != null) manager.HasChild = true;
    }
    if (index >= 0 && index <= employees.Count) employees.Insert(index, entity);
    else employees.Add(entity);
    return entity;
}
```

---

### Update Operation

**Parameters sent to server:**

| Parameter | Description |
|---|---|
| `record` | Updated record data |
| `action` | `"Update"` |
| `primaryColumnName` | Primary key field name (e.g., `"EmployeeID"`) |
| `primaryColumnValue` | Primary key value of the record to update |
| `additionalParameters` | Optional extra data |

**Server implementation:**
```csharp
public EmployeeData? UpdateEmployee(
    EmployeeData record,
    string action,
    string primaryColumnName,
    string primaryColumnValue,
    [GraphQLType(typeof(AnyType))] IDictionary<string, object> additionalParameters)
{
    var employees = EmployeeData.GetAllRecords();
    var existing  = employees.FirstOrDefault(x =>
        string.Equals(x.EmployeeID, primaryColumnValue, StringComparison.OrdinalIgnoreCase));
    if (existing == null) return null;

    if (record.Name != null)     existing.Name     = record.Name;
    if (record.Title != null)    existing.Title    = record.Title;
    if (record.Location != null) existing.Location = record.Location;
    // ... update remaining fields

    return existing;
}
```

---

### Delete Operation

**Parameters sent to server:**

| Parameter | Description |
|---|---|
| `primaryColumnValue` | Primary key value of record to delete |
| `action` | `"Delete"` |
| `primaryColumnName` | Primary key field name |
| `additionalParameters` | Optional extra data |

**Server implementation:**
```csharp
public EmployeeData? DeleteEmployee(
    string primaryColumnValue,
    string action,
    string primaryColumnName,
    [GraphQLType(typeof(AnyType))] IDictionary<string, object> additionalParameters)
{
    var employees  = EmployeeData.GetAllRecords();
    var toDelete   = employees.FirstOrDefault(x =>
        string.Equals(x.EmployeeID, primaryColumnValue, StringComparison.OrdinalIgnoreCase));
    if (toDelete == null) return null;

    // Remove the record and all its descendants
    var idsToRemove = new HashSet<string>(StringComparer.OrdinalIgnoreCase);
    CollectWithDescendants(employees, toDelete.EmployeeID, idsToRemove);
    employees.RemoveAll(e => idsToRemove.Contains(e.EmployeeID));

    // Update parent HasChild flag
    if (!string.IsNullOrWhiteSpace(toDelete.ManagerID))
    {
        var manager = employees.FirstOrDefault(e =>
            string.Equals(e.EmployeeID, toDelete.ManagerID, StringComparison.OrdinalIgnoreCase));
        if (manager != null)
            manager.HasChild = employees.Any(e =>
                string.Equals(e.ManagerID, manager.EmployeeID, StringComparison.OrdinalIgnoreCase));
    }
    return toDelete;
}

private static void CollectWithDescendants(
    List<EmployeeData> all, string rootId, HashSet<string> bag)
{
    if (bag.Contains(rootId)) return;
    bag.Add(rootId);
    foreach (var c in all.Where(e =>
        string.Equals(e.ManagerID, rootId, StringComparison.OrdinalIgnoreCase))
        .Select(e => e.EmployeeID))
        CollectWithDescendants(all, c, bag);
}
```

---

## Common Pitfalls

| Issue | Cause | Fix |
|---|---|---|
| Pager shows wrong page count | `Count` not set or includes children | Return `total` (root records only) in `Count` |
| Children don't expand | Parent ID detection fails | Check both `Params["parentId"]` and `Where` clauses |
| CORS error | Missing CORS policy | Add `AddCors` + `UseCors` in server `Program.cs` |
| `ResolverName` mismatch | Case mismatch | Use exact `camelCase` GraphQL field name |
| ManagerID filter leaks into where-filter | Not excluded | Skip `ManagerID == equal` conditions in `ApplyWhereExcludingParent` |
| Deleted record leaves orphans | Children not removed | Recursively collect descendants before delete |
