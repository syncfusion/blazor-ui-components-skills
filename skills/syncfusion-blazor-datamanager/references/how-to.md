# How-To Guides for Blazor DataManager

Focused guides for common DataManager configuration scenarios.

## Table of Contents
- [Adding Custom HTTP Headers](#adding-custom-http-headers)
- [Enabling Offline Mode](#enabling-offline-mode)

---

## Adding Custom HTTP Headers

Use the `Headers` property to attach custom key-value pairs to every outbound HTTP request made by `SfDataManager`. This is the standard approach for passing authentication tokens, tenant identifiers, or localization context without modifying the request payload.

**When to use:**
- Sending Bearer tokens or API keys for authenticated endpoints
- Including a tenant identifier in multi-tenant applications
- Adding localization or versioning headers required by the server

**Key points:**
- Works with all built-in remote adaptors: `WebApiAdaptor`, `ODataAdaptor`, `UrlAdaptor`, etc.
- Headers are included automatically every time the DataManager connects to any bound component (`SfGrid`, `SfChart`, `SfListView`, etc.)
- Update headers at runtime (e.g., on token refresh) — the property is reactive

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Grids

<!-- SECURITY: Use string variable for endpoint URL with whitelist validation -->
<SfGrid TValue="Order" AllowPaging="true">
    <GridPageSettings PageSize="10"></GridPageSettings>
    <SfDataManager Headers="@HeaderData"
                   Url="@AuthenticatedWebApiEndpointUrl"
                   Adaptor="Adaptors.WebApiAdaptor">
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
    // Whitelist of trusted Web API endpoints
    private static readonly HashSet<string> TrustedWebApiEndpoints = new()
    {
        "https://blazor.syncfusion.com/services/production/api/",
        "https://api.yourtrusted-domain.com/api/"
    };

    // String variable for endpoint URL
    private string AuthenticatedWebApiEndpointUrl { get; set; } = string.Empty;

    // Authentication headers
    private IDictionary<string, string> HeaderData { get; set; } = new Dictionary<string, string>();

    protected override void OnInitialized()
    {
        // Define endpoint and validate against whitelist
        const string endpoint = "https://blazor.syncfusion.com/services/production/api/Orders/";
        
        if (!TrustedWebApiEndpoints.Any(trusted => endpoint.StartsWith(trusted)))
            throw new InvalidOperationException($"Security validation failed: endpoint '{endpoint}' is not in the trusted list");
        
        AuthenticatedWebApiEndpointUrl = endpoint;

        // Configure headers with authentication token (retrieve from secure storage in production)
        HeaderData = new Dictionary<string, string>
        {
            { "Authorization", "Bearer <token>" },  // Replace with actual token from secure storage
            { "X-Tenant-ID", "Tenant123" }
        };
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

**Note:** Keep sensitive data such as tokens in headers rather than in the URL or request body — this reduces payload size and improves security.

---

## Enabling Offline Mode

Set `Offline="true"` on `SfDataManager` to fetch the complete dataset **once** from the remote service and then execute all subsequent operations (filtering, sorting, paging, grouping) client-side without additional network requests.

**When to use:**
- You have a remote service but the dataset is reasonably sized and doesn't change frequently
- You want to reduce network traffic while still sourcing from a remote endpoint
- You need operations to continue working if connectivity becomes intermittent after the initial load
- You're testing or demoing and want to avoid repeated API calls

**Key points:**
- On first render, `SfDataManager` fetches the **complete** collection from the remote endpoint — ensure the server returns all records (not just one page) when `Offline` is enabled
- All filtering, sorting, paging, and grouping then run in the browser against the cached `Json` property
- Compatible with `ODataAdaptor`, `ODataV4Adaptor`, and `WebApiAdaptor`

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Grids

<!-- SECURITY: Use string variable for endpoint URL with whitelist validation -->
<SfGrid TValue="EmployeeData" ID="Grid" AllowPaging="true">
    <SfDataManager Url="@OfflineODataEndpointUrl"
                   Adaptor="Adaptors.ODataAdaptor"
                   Offline="true">
    </SfDataManager>
    <GridColumns>
        <GridColumn Field="@nameof(EmployeeData.OrderID)" HeaderText="Order ID"
                    Width="120" TextAlign="TextAlign.Center" />
        <GridColumn Field="@nameof(EmployeeData.CustomerID)" HeaderText="Customer Name"
                    Width="130" TextAlign="TextAlign.Center" />
        <GridColumn Field="@nameof(EmployeeData.EmployeeID)" HeaderText="Employee ID"
                    Width="120" TextAlign="TextAlign.Center" />
    </GridColumns>
</SfGrid>

@code {
    // Whitelist of trusted OData endpoints
    private static readonly HashSet<string> TrustedODataEndpoints = new()
    {
        "https://services.odata.org/Northwind/Northwind.svc/",
        "https://api.yourtrusted-domain.com/odata/"
    };

    // String variable for endpoint URL
    private string OfflineODataEndpointUrl { get; set; } = string.Empty;

    protected override void OnInitialized()
    {
        // Define endpoint and validate against whitelist
        const string endpoint = "https://services.odata.org/Northwind/Northwind.svc/Orders";
        
        if (!TrustedODataEndpoints.Any(trusted => endpoint.StartsWith(trusted)))
            throw new InvalidOperationException($"Security validation failed: endpoint '{endpoint}' is not in the trusted list");
        
        OfflineODataEndpointUrl = endpoint;
    }

    public class EmployeeData
    {
        public int OrderID { get; set; }
        public string CustomerID { get; set; }
        public int EmployeeID { get; set; }
    }
}
```

**Gotcha:** If the remote endpoint returns paginated results by default, ensure the server-side paging is disabled or the page size is set to "all records" when `Offline="true"` — otherwise only the first page will be cached and available for client-side operations.
