# Adaptors in Blazor DataManager

Adaptors tell `SfDataManager` how to translate data operations into requests for a specific service type, and how to parse the responses back. Each remote service type has a matching built-in adaptor.

## Table of Contents
- [Adaptor Selection Guide](#adaptor-selection-guide)
- [UrlAdaptor](#urladaptor)
- [ODataAdaptor](#odataadaptor)
- [ODataV4Adaptor](#odatav4adaptor)
- [WebApiAdaptor](#webapiadaptor)
- [CustomAdaptor](#customadaptor)
- [Server Response Format](#server-response-format)

---

## Adaptor Selection Guide

Choose the adaptor that matches your backend:

| Backend / Service Type | Adaptor |
|---|---|
| Custom REST API that returns `{ result, count }` | `Adaptors.UrlAdaptor` |
| OData v3 endpoint | `Adaptors.ODataAdaptor` |
| OData v4 endpoint | `Adaptors.ODataV4Adaptor` |
| ASP.NET Web API with OData query option support | `Adaptors.WebApiAdaptor` |
| GraphQL service | `Adaptors.GraphQLAdaptor` |
| Non-standard source requiring full custom logic | `Adaptors.CustomAdaptor` |

> The `Adaptor` property is set on `SfDataManager`. `GraphQLAdaptor` and `CustomAdaptor` require additional configuration — see the dedicated reference files for those.

---

## UrlAdaptor

The `UrlAdaptor` is the **base adaptor for remote services**. Use it when connecting to any REST endpoint that does not implement OData or GraphQL, but returns data in the expected `{ result, count }` JSON format.

**Key points:**
- Converts DataManager query operations into HTTP requests
- Server must return a JSON object with `result` (data array) and `count` (total records)
- Supports paging, sorting, and filtering through query parameters

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Grids

<SfGrid TValue="EmployeeData" ID="Grid" AllowPaging="true">
    <SfDataManager Url="https://blazor.syncfusion.com/services/production/api/gridurldata"
                   Adaptor="Adaptors.UrlAdaptor">
    </SfDataManager>
    <GridColumns>
        <GridColumn Field="@nameof(EmployeeData.EmployeeID)" HeaderText="Employee ID"
                    TextAlign="TextAlign.Center" Width="120"></GridColumn>
        <GridColumn Field="@nameof(EmployeeData.EmployeeName)" HeaderText="First Name" Width="130"></GridColumn>
        <GridColumn Field="@nameof(EmployeeData.Designation)" HeaderText="Title" Width="120"></GridColumn>
    </GridColumns>
</SfGrid>

@code {
    public class EmployeeData
    {
        public int EmployeeID { get; set; }
        public string EmployeeName { get; set; }
        public string Designation { get; set; }
    }
}
```

**Required server response format:**

```json
{
    "result": [{ "EmployeeID": 1, "EmployeeName": "Nancy", "Designation": "VP" }],
    "count": 67
}
```

---

## ODataAdaptor

Use `ODataAdaptor` when the service implements the **OData v3 protocol**. It automatically generates OData-compliant query parameters for paging (`$top`, `$skip`), sorting (`$orderby`), and filtering (`$filter`).

**Key points:**
- Ideal for services exposing standard OData v3 endpoints
- Automatically formats queries per OData spec
- Requires `{ result, count }` response structure

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Grids

<SfGrid TValue="OrderData" ID="OrdersGrid" AllowPaging="true">
    <SfDataManager Url="https://services.odata.org/Northwind/Northwind.svc/Orders"
                   Adaptor="Adaptors.ODataAdaptor">
    </SfDataManager>
    <GridColumns>
        <GridColumn Field="@nameof(OrderData.OrderID)" HeaderText="Order ID"
                    TextAlign="TextAlign.Center" Width="120"></GridColumn>
        <GridColumn Field="@nameof(OrderData.CustomerID)" HeaderText="Customer Name" Width="130"></GridColumn>
        <GridColumn Field="@nameof(OrderData.EmployeeID)" HeaderText="Employee ID" Width="120"></GridColumn>
    </GridColumns>
</SfGrid>

@code {
    public class OrderData
    {
        public int OrderID { get; set; }
        public string? CustomerID { get; set; }
        public int EmployeeID { get; set; }
    }
}
```

---

## ODataV4Adaptor

Use `ODataV4Adaptor` when the service implements **OData v4**, which offers advanced query capabilities like `$apply` for aggregation. Configuration is identical to `ODataAdaptor` — only the adaptor value differs.

**Key points:**
- Implements OData v4 protocol for standardized access
- Automatically generates v4-compliant query parameters
- Use when your service URL includes `/V4/` or explicitly supports OData v4

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Grids

<SfGrid TValue="EmployeeData" ID="Grid" AllowPaging="true">
    <SfDataManager Url="https://services.odata.org/V4/Northwind/Northwind.svc/Orders/"
                   Adaptor="Adaptors.ODataV4Adaptor">
    </SfDataManager>
    <GridColumns>
        <GridColumn Field=@nameof(EmployeeData.OrderID) TextAlign="TextAlign.Center"
                    HeaderText="Order ID" Width="120"></GridColumn>
        <GridColumn Field=@nameof(EmployeeData.CustomerID) TextAlign="TextAlign.Center"
                    HeaderText="Customer Name" Width="130"></GridColumn>
        <GridColumn Field=@nameof(EmployeeData.EmployeeID) TextAlign="TextAlign.Center"
                    HeaderText="Employee ID" Width="120"></GridColumn>
    </GridColumns>
</SfGrid>

@code {
    public class EmployeeData
    {
        public int OrderID { get; set; }
        public string CustomerID { get; set; }
        public int EmployeeID { get; set; }
    }
}
```

---

## WebApiAdaptor

Use `WebApiAdaptor` for **ASP.NET Web API endpoints that understand OData query options**. It extends `ODataAdaptor` to work with Web API controllers that parse OData-style query strings.

**Key points:**
- Works with Web API endpoints that accept OData query options (`$top`, `$skip`, `$filter`, `$orderby`)
- Server must return `{ result, count }` JSON format
- Most common adaptor for ASP.NET Core backend + Syncfusion Grid scenarios

```cshtml
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Grids

<SfGrid TValue="Order" AllowPaging="true">
    <SfDataManager Url="https://blazor.syncfusion.com/services/production/api/Orders/"
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
    public class Order
    {
        public int? OrderID { get; set; }
        public string CustomerID { get; set; }
        public DateTime? OrderDate { get; set; }
        public double? Freight { get; set; }
    }
}
```

**Expected server response:**

```json
{
    "result": [{ "OrderID": 10248, "CustomerID": "VINET", "Freight": 32.38 }],
    "count": 830
}
```

---

## CustomAdaptor

When none of the built-in adaptors fit your requirements, implement a custom adaptor by deriving from `DataAdaptor`. Assign the type to `AdaptorInstance` and set `Adaptor` to `Adaptors.CustomAdaptor`.

```razor
<SfDataManager AdaptorInstance="@typeof(MyCustomAdaptor)" Adaptor="Adaptors.CustomAdaptor">
</SfDataManager>
```

> For full implementation details, see `references/custom-binding.md`.

---

## Server Response Format

All remote adaptors (Url, OData, ODataV4, WebApi) expect the server to return a JSON object with these properties when paging is enabled:

| Property | Type | Description |
|---|---|---|
| `result` | Array | The current page of records |
| `count` | Number | Total number of records across all pages |

```json
{
    "result": [{ "OrderID": 10248 }, { "OrderID": 10249 }],
    "count": 830
}
```

This structure allows `SfDataManager` to correctly configure pager controls and virtual scrolling in the bound component.
