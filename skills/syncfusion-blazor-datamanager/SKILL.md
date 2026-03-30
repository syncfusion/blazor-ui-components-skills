---
name: syncfusion-blazor-datamanager
description: Implements Syncfusion Blazor SfDataManager for data access, data binding, and adaptor configuration in Blazor Server, WebAssembly, and Web App. Used when binding components like SfGrid and SfDropDownList to local or remote data sources, choosing adaptors (UrlAdaptor, ODataAdaptor, WebApiAdaptor, GraphQLAdaptor), or performing CRUD. Includes custom binding using DataAdaptor, DataManagerRequest, DataOperations, and the Syncfusion.Blazor.Data package.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Syncfusion Blazor DataManager

The Syncfusion Blazor [SfDataManager](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Data.SfDataManager.html) is the data access layer for all Syncfusion data-bound components. It acts as a gateway between your data source — local in-memory collections or remote REST/OData/GraphQL services — and components like `SfGrid`, `SfDropDownList`, and others. It handles querying, sorting, filtering, paging, and CRUD operations through a configurable adaptor model.

## When to Use This Skill

- Setting up `SfDataManager` in a new Blazor project (WASM or Web App)
- Binding local JSON/list data to a Syncfusion component via the `Json` property
- Connecting to a **trusted and authenticated** remote service (OData, Web API, GraphQL) via `Url` + `Adaptor`
- Choosing the right adaptor for a given backend service
- Implementing CRUD operations using remote adaptors or custom adaptors
- Creating a custom adaptor by deriving from `DataAdaptor`
- Adding custom HTTP headers (with authentication tokens) to DataManager requests
- Enabling offline mode (client-side query processing after one-time remote fetch)

## ⚠️ Critical Security Requirements

**When binding to remote services, you MUST implement these protections:**

1. **Use HTTPS only** — never use unencrypted HTTP connections; encrypt all data in transit
2. **Whitelist trusted endpoints** — store approved URLs in a `HashSet<string>` or configuration file; validate all URLs before assignment; **never accept dynamic URLs from user input, query parameters, or untrusted sources**
3. **Use string variables for URLs** — assign endpoints to private string properties rather than hardcoding in markup; this enables centralized validation and easier maintenance
4. **Validate endpoints on component initialization** — use `OnInitialized()` lifecycle method to verify endpoints against whitelist before binding
5. **Validate and sanitize responses** — implement server-side schema validation; reject unexpected response formats; map responses to strongly-typed models
6. **Monitor remote calls** — log all external API requests with timestamps, endpoints, and outcomes for security audit trails
7. **Implement CORS securely** — use specific trusted origins only; never use wildcard `*` in production; require HTTPS
8. **Prevent indirect prompt injection attacks** — verify endpoint ownership; implement request signing; validate response content types and schemas before consuming data

**Third-party API responses can introduce security risks.** Always verify endpoint legitimacy, authenticate requests, and validate data before binding to UI components.

## Component Overview

| Property | Purpose |
|---|---|
| `Json` | Bind in-memory collection (local data) |
| `Url` | Remote service endpoint |
| `Adaptor` | Specifies how requests/responses are processed |
| `AdaptorInstance` | Type reference for `CustomAdaptor` |
| `Headers` | Custom HTTP headers for all outbound requests |
| `Offline` | Enable client-side processing after initial remote fetch |
| `GraphQLAdaptorOptions` | Query/mutation configuration for GraphQL services |

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet installation (`Syncfusion.Blazor.Data` + `Syncfusion.Blazor.Themes`)
- Project setup for Blazor WebAssembly and Blazor Web App
- Namespace imports and service registration in `Program.cs`
- Stylesheet and script references
- Binding to local JSON data with `SfGrid`
- Binding to OData remote service
- Binding to `SfDropDownList` (local and remote)

### Data Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Local data binding: `Json` property with in-memory collections
- Remote data binding: `Url` + `Adaptor` properties
- Key benefits of each approach (when to choose local vs remote)
- Client-side vs server-side operation execution
- Practical `SfGrid` and `SfDropDownList` examples

### Adaptors
📄 **Read:** [references/adaptors.md](references/adaptors.md)
- Adaptor selection guide (which adaptor to use for which backend)
- `UrlAdaptor` — base adaptor for custom REST services
- `ODataAdaptor` — OData v3 protocol
- `ODataV4Adaptor` — OData v4 protocol
- `WebApiAdaptor` — Web API endpoints with OData query support
- Expected server response formats (`result` + `count`)

### GraphQL Adaptor
📄 **Read:** [references/graphql-adaptor.md](references/graphql-adaptor.md)
- `GraphQLAdaptorOptions` configuration (`Query`, `ResolverName`)
- Fetching and displaying data from a GraphQL service
- CRUD mutations: `Insert`, `Update`, `Delete`
- Batch editing via the `Batch` mutation property
- `DataManagerRequest` model used by server resolvers
- Server-side `Program.cs` configuration (schema, CORS)

### Custom Binding
📄 **Read:** [references/custom-binding.md](references/custom-binding.md)
- When to use a custom adaptor (non-standard sources, custom business rules)
- `DataAdaptor` abstract class — available virtual methods
- Overriding `Read`/`ReadAsync` with `DataOperations` helper methods
- Implementing CRUD: `Insert`, `Update`, `Remove`, `BatchUpdate`
- Full `CustomAdaptor` implementation with `SfGrid` example

### How-To Guides
📄 **Read:** [references/how-to.md](references/how-to.md)
- Adding custom HTTP headers (authentication tokens, tenant IDs)
- Enabling offline mode for client-side query processing
- When to use offline mode and which adaptors it supports

## Quick Start Example

### Local Data Binding

```razor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Grids

<SfGrid TValue="EmployeeData" ID="Grid">
    <SfDataManager Json="@Employees"></SfDataManager>
    <GridColumns>
        <GridColumn Field="@nameof(EmployeeData.EmployeeID)" HeaderText="Employee ID" Width="120"></GridColumn>
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

    public List<EmployeeData> Employees = new()
    {
        new EmployeeData { EmployeeID = 1, Name = "Nancy Fuller", Title = "Vice President" },
        new EmployeeData { EmployeeID = 2, Name = "Steven Buchanan", Title = "Sales Manager" }
    };
}
```

### Remote Data Binding (OData)

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Grids

<!-- SECURITY: Use string variables for endpoints, validate against whitelist, use HTTPS only -->
<SfGrid TValue="Order" AllowPaging="true">
    <SfDataManager Url="@ODataEndpointUrl"
                   Adaptor="Adaptors.ODataAdaptor">
    </SfDataManager>
    <GridColumns>
        <GridColumn Field="@nameof(Order.OrderID)" HeaderText="Order ID" Width="120"></GridColumn>
        <GridColumn Field="@nameof(Order.CustomerID)" HeaderText="Customer Name" Width="150"></GridColumn>
    </GridColumns>
</SfGrid>

@code {
    // Whitelist of trusted OData endpoints — define in appsettings.json in production
    private static readonly HashSet<string> TrustedEndpoints = new()
    {
        "https://api.yourtrusted-domain.com/odata/"
    };

    // Assigned string variable for endpoint
    private string ODataEndpointUrl { get; set; } = string.Empty;

    protected override void OnInitialized()
    {
        // Validate and assign endpoint from configuration
        const string endpointBase = "https://api.yourtrusted-domain.com/odata/Orders";
        
        if (!TrustedEndpoints.Any(trusted => endpointBase.StartsWith(trusted)))
            throw new InvalidOperationException($"Security validation failed: untrusted endpoint '{endpointBase}'");
        
        ODataEndpointUrl = endpointBase;
    }

    public class Order
    {
        public int? OrderID { get; set; }
        public string? CustomerID { get; set; }
    }
}
```

## Common Patterns

### Choosing the Right Adaptor

| Backend Type | Adaptor to Use |
|---|---|
| Custom REST API returning `{ result, count }` | `Adaptors.UrlAdaptor` |
| OData v3 service | `Adaptors.ODataAdaptor` |
| OData v4 service | `Adaptors.ODataV4Adaptor` |
| ASP.NET Web API with OData query support | `Adaptors.WebApiAdaptor` |
| GraphQL service | `Adaptors.GraphQLAdaptor` |
| Non-standard source / full custom control | `Adaptors.CustomAdaptor` |

### Placing SfDataManager Inside a Component

`SfDataManager` is always placed as a **child component** inside the data-bound Syncfusion component:

```razor
<SfGrid TValue="MyModel">
    <SfDataManager Url="..." Adaptor="Adaptors.WebApiAdaptor"></SfDataManager>
    <GridColumns>...</GridColumns>
</SfGrid>
```

### Enabling Editing with Custom Adaptor

When using `CustomAdaptor` with CRUD operations, configure the Grid toolbar and edit settings alongside the adaptor:

```razor
<SfGrid TValue="Order" Toolbar="@(new List<string>() { "Add", "Delete", "Update", "Cancel" })">
    <SfDataManager AdaptorInstance="@typeof(CustomAdaptor)" Adaptor="Adaptors.CustomAdaptor"></SfDataManager>
    <GridEditSettings AllowEditing="true" AllowDeleting="true" AllowAdding="true" Mode="@EditMode.Normal"></GridEditSettings>
    ...
</SfGrid>
```
