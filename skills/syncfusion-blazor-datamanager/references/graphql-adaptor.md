# GraphQL Adaptor in Blazor DataManager

The `GraphQLAdaptor` enables `SfDataManager` to interact with GraphQL services. Unlike REST adaptors, it uses queries and mutations to fetch data and perform CRUD operations. Configuration is done through `GraphQLAdaptorOptions`.

## Table of Contents
- [Overview](#overview)
- [Fetching Data with a Query](#fetching-data-with-a-query)
- [CRUD Mutations](#crud-mutations)
- [Batch Editing](#batch-editing)
- [DataManagerRequest Model](#datamanagerrequest-model)
- [Server-Side Setup](#server-side-setup)

---

## Overview

To use `GraphQLAdaptor`:

1. Set `Adaptor="Adaptors.GraphQLAdaptor"` on `SfDataManager`
2. Set `Url` to your **trusted, authenticated** GraphQL endpoint
3. Configure `GraphQLAdaptorOptions` with:
   - `Query` — the GraphQL query string
   - `ResolverName` — maps the response to `data.{ResolverName}`
   - `Mutation` (optional) — for CRUD operations

⚠️ **SECURITY CRITICAL**: 
- Only connect to GraphQL services you **control or explicitly trust**
- Use **HTTPS only** and **authenticate all requests**
- Validate the GraphQL endpoint URL against a whitelist
- Sanitize all response data before rendering to UI
- Implement rate limiting and request monitoring

The GraphQL server resolver receives a `DataManagerRequest` input variable and must return a JSON response with `count`, `result`, and optionally `aggregates`.

---

## Fetching Data with a Query

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Grids

<!-- SECURITY: Use string variable for GraphQL endpoint URL with whitelist validation -->
<SfGrid TValue="Order" AllowPaging="true" PageSettings="new PageSettings { PageSize = 10 }">
    <SfDataManager Url="@GraphQLEndpointUrl"
                   GraphQLAdaptorOptions="@adaptorOptions"
                   Adaptor="Adaptors.GraphQLAdaptor">
    </SfDataManager>
    <GridColumns>
        <GridColumn Field="@nameof(Order.OrderID)" HeaderText="Order ID" Width="120"
                    TextAlign="TextAlign.Center"></GridColumn>
        <GridColumn Field="@nameof(Order.CustomerID)" HeaderText="Customer Name" Width="150"></GridColumn>
        <GridColumn Field="@nameof(Order.OrderDate)" HeaderText="Order Date"
                    Type="ColumnType.Date" Format="d" Width="130"></GridColumn>
        <GridColumn Field="@nameof(Order.Freight)" HeaderText="Freight" Format="C2"
                    TextAlign="TextAlign.Right" Width="120"></GridColumn>
    </GridColumns>
</SfGrid>

@code {
    // Whitelist of trusted GraphQL endpoints
    private static readonly HashSet<string> TrustedGraphQLEndpoints = new()
    {
        "https://api.yourtrusted-domain.com/graphql"
    };

    // String variable for GraphQL endpoint URL
    private string GraphQLEndpointUrl { get; set; } = string.Empty;

    protected override void OnInitialized()
    {
        // Define endpoint and validate against whitelist
        const string endpoint = "https://api.yourtrusted-domain.com/graphql";
        
        if (!TrustedGraphQLEndpoints.Contains(endpoint))
            throw new InvalidOperationException($"Security validation failed: GraphQL endpoint '{endpoint}' is not in the trusted list");
        
        GraphQLEndpointUrl = endpoint;
    }

    private GraphQLAdaptorOptions adaptorOptions = new GraphQLAdaptorOptions
    {
        Query = @"
            query ordersData($dataManager: DataManagerRequestInput!) {
                ordersData(dataManager: $dataManager) {
                    count,
                    result { OrderID, CustomerID, OrderDate, Freight },
                    aggregates
                }
            }",
        ResolverName = "OrdersData"
    };

    public class Order
    {
        public int? OrderID { get; set; }
        public string CustomerID { get; set; }
        public DateTime? OrderDate { get; set; }
        public double? Freight { get; set; }
    }
}
```

> `ResolverName` must match the resolver field name in the GraphQL response under `data.{ResolverName}` (case-insensitive match).

---

## CRUD Mutations

Define mutations for Insert, Update, and Delete in `GraphQLAdaptorOptions.Mutation`. The Grid triggers these automatically when editing is enabled.

### Configuration (Blazor component)

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data

<!-- SECURITY: Use string variable for GraphQL endpoint URL with whitelist validation -->
<SfDataManager Url="@GraphQLCrudEndpointUrl"
               GraphQLAdaptorOptions="@_adaptorOptions"
               Adaptor="Adaptors.GraphQLAdaptor">
</SfDataManager>

@code {
    // Whitelist of trusted GraphQL endpoints
    private static readonly HashSet<string> TrustedGraphQLEndpoints = new()
    {
        "https://api.yourtrusted-domain.com/graphql"
    };

    // String variable for GraphQL endpoint URL
    private string GraphQLCrudEndpointUrl { get; set; } = string.Empty;

    protected override void OnInitialized()
    {
        // Define endpoint and validate against whitelist
        const string endpoint = "https://api.yourtrusted-domain.com/graphql";
        
        if (!TrustedGraphQLEndpoints.Contains(endpoint))
            throw new InvalidOperationException($"Security validation failed: GraphQL endpoint '{endpoint}' is not in the trusted list");
        
        GraphQLCrudEndpointUrl = endpoint;
    }

    private GraphQLAdaptorOptions _adaptorOptions = new GraphQLAdaptorOptions
    {
        Query = @"
            query ordersData($dataManager: DataManagerRequestInput!) {
                ordersData(dataManager: $dataManager) {
                    count,
                    result { OrderID, CustomerID, OrderDate, Freight },
                    aggregates
                }
            }",
        Mutation = new GraphQLMutation
        {
            Insert = @"
                mutation create($record: OrderInput!, $index: Int!, $action: String!, $additionalParameters: Any) {
                    createOrder(order: $record, index: $index, action: $action, additionalParameters: $additionalParameters) {
                        OrderID, CustomerID, OrderDate, Freight
                    }
                }",
            Update = @"
                mutation update($record: OrderInput!, $action: String!, $primaryColumnName: String!, $primaryColumnValue: Int!, $additionalParameters: Any) {
                    updateOrder(order: $record, action: $action, primaryColumnName: $primaryColumnName, primaryColumnValue: $primaryColumnValue, additionalParameters: $additionalParameters) {
                        OrderID, CustomerID, OrderDate, Freight
                    }
                }",
            Delete = @"
                mutation delete($primaryColumnValue: Int!, $action: String!, $primaryColumnName: String!, $additionalParameters: Any) {
                    deleteOrder(primaryColumnValue: $primaryColumnValue, action: $action, primaryColumnName: $primaryColumnName, additionalParameters: $additionalParameters) {
                        OrderID, CustomerID, OrderDate, Freight
                    }
                }"
        },
        ResolverName = "OrdersData"
    };
}
```

### Mutation Parameters Reference

**Insert:**

| Parameter | Description |
|---|---|
| `record` | New record to insert |
| `index` | Position to insert at |
| `action` | Operation type (e.g., "Add") |
| `additionalParameters` | Optional custom data |

**Update:**

| Parameter | Description |
|---|---|
| `record` | Updated record |
| `action` | Operation type (e.g., "Edit") |
| `primaryColumnName` | Name of the primary key field |
| `primaryColumnValue` | Value of the primary key |
| `additionalParameters` | Optional custom data |

**Delete:**

| Parameter | Description |
|---|---|
| `primaryColumnValue` | Primary key value of record to delete |
| `action` | Operation type (e.g., "Delete") |
| `primaryColumnName` | Name of the primary key field |
| `additionalParameters` | Optional custom data |

### Server-Side Mutation Resolvers

```csharp
public class GraphQLMutation
{
    public Order CreateOrder(Order order, int index, string action,
        [GraphQLType(typeof(AnyType))] IDictionary<string, object> additionalParameters)
    {
        var list = GraphQLQuery.Orders;
        var safeIndex = Math.Clamp(index, 0, list.Count);
        list.Insert(safeIndex, order);
        return order;
    }

    public Order UpdateOrder(Order order, string action, string primaryColumnName,
        int primaryColumnValue,
        [GraphQLType(typeof(AnyType))] IDictionary<string, object> additionalParameters)
    {
        var existing = GraphQLQuery.Orders.FirstOrDefault(x => x.OrderID == primaryColumnValue);
        if (existing == null) return order;
        existing.OrderID = order.OrderID;
        existing.CustomerID = order.CustomerID;
        existing.Freight = order.Freight;
        existing.OrderDate = order.OrderDate;
        return existing;
    }

    public Order DeleteOrder(int primaryColumnValue, string action, string primaryColumnName,
        [GraphQLType(typeof(AnyType))] IDictionary<string, object> additionalParameters)
    {
        var target = GraphQLQuery.Orders.FirstOrDefault(x => x.OrderID == primaryColumnValue);
        if (target != null) GraphQLQuery.Orders.Remove(target);
        return target;
    }
}
```

---

## Batch Editing

Batch editing sends all pending Insert, Update, and Delete operations in a **single GraphQL request**. Configure via `Mutation.Batch`.

```cshtml
@code {
    private GraphQLAdaptorOptions _adaptorOptions = new GraphQLAdaptorOptions
    {
        Query = @"
            query ordersData($dataManager: DataManagerRequestInput!) {
                ordersData(dataManager: $dataManager) {
                    count,
                    result { OrderID, CustomerID, OrderDate, Freight },
                    aggregates
                }
            }",
        Mutation = new GraphQLMutation
        {
            Batch = @"
                mutation batch(
                    $changed: [OrderInput!], $added: [OrderInput!], $deleted: [OrderInput!],
                    $action: String!, $primaryColumnName: String!,
                    $additionalParameters: Any, $dropIndex: Int
                ) {
                    batchUpdate(
                        changed: $changed, added: $added, deleted: $deleted,
                        action: $action, primaryColumnName: $primaryColumnName,
                        additionalParameters: $additionalParameters, dropIndex: $dropIndex
                    ) { OrderID, CustomerID, OrderDate, Freight }
                }"
        },
        ResolverName = "OrdersData"
    };
}
```

**Batch mutation parameters:**

| Parameter | Description |
|---|---|
| `changed` | Records to update |
| `added` | Records to insert |
| `deleted` | Records to remove |
| `action` | Operation type |
| `primaryColumnName` | Primary key field name |
| `additionalParameters` | Optional custom data |
| `dropIndex` | Insert position for drag-and-drop |

### Server-Side Batch Resolver

```csharp
public class GraphQLMutation
{
    public List<Order> BatchUpdate(
        List<Order>? changed,
        List<Order>? added,
        List<Order>? deleted,
        string action,
        string primaryColumnName,
        [GraphQLType(typeof(AnyType))] IDictionary<string, object>? additionalParameters,
        int? dropIndex)
    {
        // Update existing records
        if (changed != null && changed.Count > 0)
        {
            foreach (var changedOrder in changed)
            {
                var target = GraphQLQuery.Orders.FirstOrDefault(e => e.OrderID == changedOrder.OrderID);
                if (target != null)
                {
                    target.CustomerID = changedOrder.CustomerID;
                    target.OrderDate = changedOrder.OrderDate;
                    target.Freight = changedOrder.Freight;
                }
            }
        }

        // Insert new records — respect drag-and-drop index if provided
        if (added != null && added.Count > 0)
        {
            if (dropIndex.HasValue)
            {
                var index = Math.Clamp(dropIndex.Value, 0, GraphQLQuery.Orders.Count);
                GraphQLQuery.Orders.InsertRange(index, added);
            }
            else
            {
                GraphQLQuery.Orders.AddRange(added);
            }
        }

        // Delete records
        if (deleted != null && deleted.Count > 0)
        {
            foreach (var deletedOrder in deleted)
            {
                var target = GraphQLQuery.Orders.FirstOrDefault(e => e.OrderID == deletedOrder.OrderID);
                if (target != null)
                    GraphQLQuery.Orders.Remove(target);
            }
        }

        return GraphQLQuery.Orders;
    }
}
```

**Key points:**
- `InsertRange` with a clamped `dropIndex` handles drag-and-drop row reordering safely
- Primary key fields (`OrderID`) should **not** be overwritten during updates
- The method returns the full updated collection — the Grid reconciles the state client-side

---

## DataManagerRequest Model

The GraphQL server resolver receives a `DataManagerRequestInput` variable containing all query parameters from the DataManager. Use this **full model** — including all `[GraphQLName]` annotations and supporting classes — when implementing a server-side resolver:

```csharp
public class DataManagerRequest
{
    [GraphQLName("Skip")]
    public int Skip { get; set; }

    [GraphQLName("Take")]
    public int Take { get; set; }

    [GraphQLName("RequiresCounts")]
    public bool RequiresCounts { get; set; } = false;

    [GraphQLName("Params")]
    [GraphQLType(typeof(AnyType))]
    public IDictionary<string, object> Params { get; set; }

    [GraphQLName("Aggregates")]
    [GraphQLType(typeof(AnyType))]
    public List<Aggregate>? Aggregates { get; set; }

    [GraphQLName("Search")]
    public List<SearchFilter>? Search { get; set; }

    [GraphQLName("Sorted")]
    public List<Sort>? Sorted { get; set; }

    [GraphQLName("Where")]
    [GraphQLType(typeof(AnyType))]
    public List<WhereFilter>? Where { get; set; }

    [GraphQLName("Group")]
    public List<string>? Group { get; set; }

    [GraphQLName("antiForgery")]
    public string? antiForgery { get; set; }

    [GraphQLName("Table")]
    public string? Table { get; set; }

    [GraphQLName("IdMapping")]
    public string? IdMapping { get; set; }

    [GraphQLName("Select")]
    public List<string>? Select { get; set; }

    [GraphQLName("Expand")]
    public List<string>? Expand { get; set; }

    [GraphQLName("Distinct")]
    public List<string>? Distinct { get; set; }

    [GraphQLName("ServerSideGroup")]
    public bool? ServerSideGroup { get; set; }

    [GraphQLName("LazyLoad")]
    public bool? LazyLoad { get; set; }

    [GraphQLName("LazyExpandAllGroup")]
    public bool? LazyExpandAllGroup { get; set; }
}

public class Aggregate
{
    [GraphQLName("Field")]
    public string Field { get; set; }

    [GraphQLName("Type")]
    public string Type { get; set; }
}

public class SearchFilter
{
    [GraphQLName("Fields")]
    public List<string> Fields { get; set; }

    [GraphQLName("Key")]
    public string Key { get; set; }

    [GraphQLName("Operator")]
    public string Operator { get; set; }

    [GraphQLName("IgnoreCase")]
    public bool IgnoreCase { get; set; }
}

public class Sort
{
    [GraphQLName("Name")]
    public string Name { get; set; }

    [GraphQLName("Direction")]
    public string Direction { get; set; }

    [GraphQLName("Comparer")]
    [GraphQLType(typeof(AnyType))]
    public object Comparer { get; set; }
}

public class WhereFilter
{
    [GraphQLName("Field")]
    public string? Field { get; set; }

    [GraphQLName("IgnoreCase")]
    public bool? IgnoreCase { get; set; }

    [GraphQLName("IgnoreAccent")]
    public bool? IgnoreAccent { get; set; }

    [GraphQLName("IsComplex")]
    public bool? IsComplex { get; set; }

    [GraphQLName("Operator")]
    public string? Operator { get; set; }

    [GraphQLName("Condition")]
    public string? Condition { get; set; }

    [GraphQLName("value")]
    [GraphQLType(typeof(AnyType))]
    public object? value { get; set; }

    [GraphQLName("predicates")]
    public List<WhereFilter>? predicates { get; set; }
}
```

The resolver uses these parameters to apply searching, sorting, filtering, and paging before returning the response.

---

## Server-Side Setup

### GraphQL Query Resolver

Implement a `GraphQLQuery` class that consumes `DataManagerRequest` and applies data operations using the `DataOperations` helper. Return a `ReturnType<T>` response that includes `Result`, `Count`, and optionally `Aggregates`.

```csharp
public class GraphQLQuery
{
    public ReturnType<Order> OrdersData(DataManagerRequest dataManager)
    {
        IEnumerable<Order> result = Orders;

        if (dataManager.Search != null && dataManager.Search.Count > 0)
            result = DataOperations.PerformSearching(result, dataManager.Search);

        if (dataManager.Sorted != null && dataManager.Sorted.Count > 0)
            result = DataOperations.PerformSorting(result, dataManager.Sorted);

        if (dataManager.Where != null && dataManager.Where.Count > 0)
            result = DataOperations.PerformFiltering(result, dataManager.Where, dataManager.Where[0].Operator);

        int count = result.Count();

        if (dataManager.Skip != 0)
            result = DataOperations.PerformSkip(result, dataManager.Skip);

        if (dataManager.Take != 0)
            result = DataOperations.PerformTake(result, dataManager.Take);

        if (dataManager.Aggregates != null)
        {
            IDictionary<string, object> aggregates = DataUtil.PerformAggregation(result, dataManager.Aggregates);
            return new ReturnType<Order>() { Count = count, Result = result, Aggregates = aggregates };
        }

        return dataManager.RequiresCounts
            ? new ReturnType<Order>() { Result = result, Count = count }
            : new ReturnType<Order>() { Result = result };
    }

    public static List<Order> Orders { get; set; } = Enumerable.Range(1, 75).Select(x => new Order
    {
        OrderID = 1000 + x,
        CustomerID = new[] { "ALFKI", "ANANTR", "ANTON", "BLONP", "BOLID" }[x % 5],
        OrderDate = new DateTime(2023, 1, 1).AddDays(x),
        Freight = Math.Round(2.1 * x, 2)
    }).ToList();
}

// Response wrapper returned by all GraphQL query resolvers
public class ReturnType<T>
{
    public int Count { get; set; }
    public IEnumerable<T> Result { get; set; }

    [GraphQLType(typeof(AnyType))]
    public IDictionary<string, object> Aggregates { get; set; }
}
```

> `ResolverName` in `GraphQLAdaptorOptions` must match the method name (case-insensitive). For the resolver `OrdersData(...)` above, set `ResolverName = "OrdersData"`.

### Register Schema and Configure CORS in `Program.cs`

```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services
    .AddGraphQLServer()
    .AddQueryType<GraphQLQuery>()
    .AddMutationType<GraphQLMutation>();

// SECURITY: Restrict CORS to only the specific Blazor app origin
// Never use wildcard (*) in production
builder.Services.AddCors(options =>
{
    options.AddPolicy("AllowSpecificOrigin", cors =>
    {
        cors.WithOrigins("https://your-blazor-app-url")  // Use HTTPS only
            .AllowAnyHeader()
            .AllowAnyMethod()
            .AllowCredentials();
    });
});

var app = builder.Build();
app.UseCors("AllowSpecificOrigin");
app.MapGraphQL();
```
