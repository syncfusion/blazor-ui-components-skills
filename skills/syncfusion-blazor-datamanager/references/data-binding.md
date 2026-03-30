# Data Binding in Blazor DataManager

The `SfDataManager` supports two primary data binding approaches: **local** (in-memory) and **remote** (external service). Choose based on dataset size, update frequency, and where you want operations like sorting and filtering to execute.

## Table of Contents
- [Local Data Binding](#local-data-binding)
- [Remote Data Binding](#remote-data-binding)
- [Choosing Local vs Remote](#choosing-local-vs-remote)
- [SfGrid Examples](#sfgrid-examples)

---

## Local Data Binding

Local data binding connects a component to data already loaded in application memory. Assign the in-memory collection to the `Json` property of `SfDataManager`.

**When to use local binding:**
- Dataset is already available in memory (fetched once, doesn't change often)
- Small to medium-sized collections
- You want all operations (filtering, sorting, paging, grouping) to run in the browser without network calls
- No external service is involved

**Key benefits:**
- No network latency for subsequent operations
- Simple configuration — no adaptor needed
- All data operations execute client-side automatically

```cshtml
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Grids

<SfGrid TValue="EmployeeData" ID="Grid">
    <SfDataManager Json="@Employees"></SfDataManager>
    <GridColumns>
        <GridColumn Field="@nameof(EmployeeData.EmployeeID)" HeaderText="Employee ID"
                    TextAlign="TextAlign.Center" Width="120"></GridColumn>
        <GridColumn Field="@nameof(EmployeeData.Name)" HeaderText="First Name" Width="130"></GridColumn>
        <GridColumn Field="@nameof(EmployeeData.Title)" HeaderText="Title" Width="120"></GridColumn>
    </GridColumns>
</SfGrid>

@code {
    public class EmployeeData
    {
        public int EmployeeID { get; set; }
        public string Name { get; set; }
        public string Title { get; set; }
    }

    private List<EmployeeData> Employees { get; set; } = new()
    {
        new EmployeeData { EmployeeID = 1, Name = "Nancy Fuller", Title = "Vice President" },
        new EmployeeData { EmployeeID = 2, Name = "Steven Buchanan", Title = "Sales Manager" },
        new EmployeeData { EmployeeID = 3, Name = "Janet Leverling", Title = "Sales Representative" },
        new EmployeeData { EmployeeID = 4, Name = "Andrew Davolio", Title = "Inside Sales Coordinator" },
        new EmployeeData { EmployeeID = 5, Name = "Steven Peacock", Title = "Inside Sales Coordinator" }
    };
}
```

---

## Remote Data Binding

Remote data binding connects a component to data hosted on an external service. Set the `Url` property to the service endpoint and specify the `Adaptor` property so `SfDataManager` knows how to format requests and parse responses.

**When to use remote binding:**
- Large datasets that should not be fully loaded into memory
- Data that changes frequently and must be fetched dynamically
- Server-side filtering, sorting, and paging for performance

**Key benefits:**
- Integrates with external APIs and services
- Supports large-scale data without memory overhead
- Operations like filtering and paging can execute server-side

⚠️ **CRITICAL SECURITY REQUIREMENT**: Remote endpoints must be:
- **Trusted and authenticated** — only use services you control or have contractually verified
- **HTTPS only** — enforce encrypted communication
- **Validated on every request** — whitelist allowed URLs in your application configuration
- **Monitored for suspicious activity** — log all remote requests

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Grids

<!-- SECURITY: Use string variable for endpoint URL with whitelist validation -->
<SfGrid TValue="Order" ID="Grid" AllowPaging="true">
    <SfDataManager Url="@RemoteODataEndpointUrl"
                   Adaptor="Adaptors.ODataAdaptor">
    </SfDataManager>
    <GridColumns>
        <GridColumn Field="@nameof(Order.OrderID)" HeaderText="Order ID" IsPrimaryKey="true"
                    TextAlign="TextAlign.Right" Width="120"></GridColumn>
        <GridColumn Field="@nameof(Order.CustomerID)" HeaderText="Customer Name" Width="150"></GridColumn>
        <GridColumn Field="@nameof(Order.OrderDate)" HeaderText="Order Date" Format="d"
                    Type="ColumnType.Date" TextAlign="TextAlign.Right" Width="130"></GridColumn>
        <GridColumn Field="@nameof(Order.Freight)" HeaderText="Freight" Format="C2"
                    TextAlign="TextAlign.Right" Width="120"></GridColumn>
    </GridColumns>
</SfGrid>

@code {
    // Whitelist of trusted OData endpoints
    private static readonly HashSet<string> TrustedODataEndpoints = new()
    {
        "https://api.yourtrusted-domain.com/odata/"
    };

    // String variable for endpoint URL
    private string RemoteODataEndpointUrl { get; set; } = string.Empty;

    protected override void OnInitialized()
    {
        // Define endpoint and validate against whitelist
        const string endpoint = "https://api.yourtrusted-domain.com/odata/Orders";
        
        if (!TrustedODataEndpoints.Any(trusted => endpoint.StartsWith(trusted)))
            throw new InvalidOperationException($"Security validation failed: endpoint '{endpoint}' is not in the trusted list");
        
        RemoteODataEndpointUrl = endpoint;
    }

    public class Order
    {
        public int? OrderID { get; set; }
        public string CustomerID { get; set; }
        public DateTime? OrderDate { get; set; }
        public double? Freight { get; set; }
    }
}
```

---

## Choosing Local vs Remote

| Criteria | Local Binding (`Json`) | Remote Binding (`Url` + `Adaptor`) |
|---|---|---|
| Data location | Already in memory | External service/API |
| Dataset size | Small to medium | Any (including large) |
| Network requests | None after initial load | Per operation (or server-side batch) |
| Operations | Client-side | Server-side or client-side |
| Configuration complexity | Minimal | Requires matching adaptor |
| Offline support | Yes (data already in memory) | Use `Offline="true"` property |

> When you have a remote service but want to avoid repeated network requests, enable `Offline="true"` on `SfDataManager`. This fetches the full dataset once and processes all subsequent operations client-side.

---

## SfGrid Examples

Both examples below produce the same Grid UI — the difference is where data comes from and where operations run.

### Local — full collection in `@code`

```cshtml
<SfGrid TValue="Product">
    <SfDataManager Json="@Products"></SfDataManager>
    <GridColumns>
        <GridColumn Field="ProductID" HeaderText="ID" Width="80"></GridColumn>
        <GridColumn Field="ProductName" HeaderText="Product" Width="150"></GridColumn>
        <GridColumn Field="Price" HeaderText="Price" Format="C2" Width="120"></GridColumn>
    </GridColumns>
</SfGrid>

@code {
    public class Product { public int ProductID; public string ProductName; public double Price; }
    public List<Product> Products = new() { /* ... */ };
}
```

### Remote — data fetched from Web API

```cshtml
<SfGrid TValue="Product" AllowPaging="true">
    <!-- SECURITY: Use string variable for endpoint URL with whitelist validation -->
    <SfDataManager Url="@WebApiEndpointUrl"
                   Adaptor="Adaptors.WebApiAdaptor">
    </SfDataManager>
    <GridColumns>
        <GridColumn Field="ProductID" HeaderText="ID" Width="80"></GridColumn>
        <GridColumn Field="ProductName" HeaderText="Product" Width="150"></GridColumn>
        <GridColumn Field="Price" HeaderText="Price" Format="C2" Width="120"></GridColumn>
    </GridColumns>
</SfGrid>

@code {
    // Whitelist of trusted Web API endpoints
    private static readonly HashSet<string> TrustedWebApiEndpoints = new()
    {
        "https://api.yourtrusted-domain.com/api/"
    };

    // String variable for endpoint URL
    private string WebApiEndpointUrl { get; set; } = string.Empty;

    protected override void OnInitialized()
    {
        // Define endpoint and validate against whitelist
        const string endpoint = "https://api.yourtrusted-domain.com/api/products";
        
        if (!TrustedWebApiEndpoints.Any(trusted => endpoint.StartsWith(trusted)))
            throw new InvalidOperationException($"Security validation failed: endpoint '{endpoint}' is not in the trusted list");
        
        WebApiEndpointUrl = endpoint;
    }

    public class Product 
    { 
        public int ProductID { get; set; }
        public string ProductName { get; set; }
        public double Price { get; set; }
    }
}
```
