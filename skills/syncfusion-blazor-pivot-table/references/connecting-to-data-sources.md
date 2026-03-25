# Connecting to Data Sources — Syncfusion Blazor Pivot Table

## Table of Contents
- [Overview](#overview)
- [Microsoft SQL Server](#microsoft-sql-server)
- [MySQL](#mysql)
- [PostgreSQL](#postgresql)
- [MongoDB](#mongodb)
- [Oracle](#oracle)
- [Snowflake](#snowflake)
- [Elasticsearch](#elasticsearch)
- [Web API Pattern (Recommended for Production)](#web-api-pattern-recommended-for-production)

---

## Overview

For connecting databases to the Blazor Pivot Table, the recommended pattern is:
1. Create a **Blazor Server** or **Web API** service that queries the database
2. Return data as `List<T>` or JSON
3. Bind the result to `PivotViewDataSourceSettings.DataSource`

Direct database access from Blazor WASM is not possible; use a server-side service or Web API.

---

## Microsoft SQL Server

**Install:** `System.Data.SqlClient`

```razor
@using Syncfusion.Blazor.PivotView
@using System.Data
@using System.Data.SqlClient

<SfPivotView TValue="OrderDetails" Width="800" Height="360">
    <PivotViewDataSourceSettings TValue="OrderDetails" DataSource="@dataSource">
        <PivotViewColumns>
            <PivotViewColumn Name="Product"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="Date"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Amount"></PivotViewValue>
        </PivotViewValues>
        <PivotViewFormatSettings>
            <PivotViewFormatSetting Name="Amount" Format="C2"></PivotViewFormatSetting>
        </PivotViewFormatSettings>
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    private List<OrderDetails> dataSource { get; set; }

    protected override void OnInitialized()
    {
        string conStr = "Server=localhost;Database=SalesDB;Trusted_Connection=True;";
        string query = "SELECT Product, Date, Country, Quantity, Amount FROM Orders";

        using var connection = new SqlConnection(conStr);
        connection.Open();
        using var adapter = new SqlDataAdapter(new SqlCommand(query, connection));
        var table = new DataTable();
        adapter.Fill(table);

        dataSource = (from DataRow row in table.Rows
                      select new OrderDetails
                      {
                          Product = row["Product"].ToString(),
                          Date    = row["Date"].ToString(),
                          Country = row["Country"].ToString(),
                          Quantity = Convert.ToInt32(row["Quantity"]),
                          Amount  = Convert.ToDouble(row["Amount"])
                      }).ToList();
    }

    public class OrderDetails
    {
        public string Product { get; set; }
        public string Date { get; set; }
        public string Country { get; set; }
        public int Quantity { get; set; }
        public double Amount { get; set; }
    }
}
```

---

## MySQL

**Install:** `MySql.Data` (or `MySqlConnector`)

```csharp
// Program.cs or service class
using MySql.Data.MySqlClient;

string conStr = "Server=localhost;Database=salesdb;User=root;Password=yourpass;";
string query  = "SELECT * FROM orders";

using var connection = new MySqlConnection(conStr);
connection.Open();
using var adapter = new MySqlDataAdapter(query, connection);
var table = new DataTable();
adapter.Fill(table);

// Convert DataTable rows to List<OrderDetails> as shown above
```

Bind the resulting `List<T>` to `DataSource` in `PivotViewDataSourceSettings`.

---

## PostgreSQL

**Install:** `Npgsql`

```csharp
using Npgsql;

string conStr = "Host=localhost;Database=salesdb;Username=postgres;Password=yourpass;";
string query  = "SELECT * FROM orders";

using var connection = new NpgsqlConnection(conStr);
connection.Open();
using var adapter = new NpgsqlDataAdapter(query, connection);
var table = new DataTable();
adapter.Fill(table);
```

---

## MongoDB

**Install:** `MongoDB.Driver`

```csharp
using MongoDB.Driver;

var client     = new MongoClient("mongodb://localhost:27017");
var database   = client.GetDatabase("salesdb");
var collection = database.GetCollection<OrderDetails>("orders");

// Fetch all documents and convert to list
List<OrderDetails> dataSource = collection.Find(FilterDefinition<OrderDetails>.Empty)
                                           .ToList();
```

Bind `dataSource` to `PivotViewDataSourceSettings.DataSource`.

---

## Oracle

**Install:** `Oracle.ManagedDataAccess.Core`

```csharp
using Oracle.ManagedDataAccess.Client;

string conStr = "Data Source=localhost/ORCL;User Id=hr;Password=yourpass;";
string query  = "SELECT * FROM orders";

using var connection = new OracleConnection(conStr);
connection.Open();
using var adapter = new OracleDataAdapter(query, connection);
var table = new DataTable();
adapter.Fill(table);
```

---

## Snowflake

**Install:** `Snowflake.Data`

```csharp
using Snowflake.Data.Client;

string conStr = "account=myaccount;user=myuser;password=mypass;db=SALES;schema=PUBLIC;warehouse=COMPUTE_WH;";

using var connection = new SnowflakeDbConnection { ConnectionString = conStr };
connection.Open();
using var command = connection.CreateCommand();
command.CommandText = "SELECT * FROM ORDERS";
using var reader = command.ExecuteReader();

var dataSource = new List<OrderDetails>();
while (reader.Read())
{
    dataSource.Add(new OrderDetails
    {
        Product  = reader["PRODUCT"].ToString(),
        Amount   = Convert.ToDouble(reader["AMOUNT"]),
        // ... map other fields
    });
}
```

---

## Elasticsearch

**Install:** `NEST` (Elasticsearch .NET client)

```csharp
using Nest;

var settings = new ConnectionSettings(new Uri("http://localhost:9200"))
    .DefaultIndex("orders");
var client = new ElasticClient(settings);

var response = client.Search<OrderDetails>(s => s.MatchAll().Size(10000));
List<OrderDetails> dataSource = response.Documents.ToList();
```

---

## Web API Pattern (Recommended for Production)

For Blazor WASM or large-scale apps, expose data via a Web API controller and use `SfDataManager` with `WebApiAdaptor`:

**API Controller (`OrdersController.cs`):**
```csharp
[ApiController]
[Route("api/[controller]")]
public class OrdersController : ControllerBase
{
    private readonly IDbService _db;

    [HttpGet]
    public IActionResult Get() => Ok(_db.GetOrderData());
}
```

**Blazor Component:**
```razor
@using Syncfusion.Blazor.Data

<SfPivotView TValue="OrderDetails" Height="450">
    <PivotViewDataSourceSettings TValue="OrderDetails">
        <SfDataManager Url="/api/orders"
                       Adaptor="Syncfusion.Blazor.Adaptors.WebApiAdaptor">
        </SfDataManager>
        <PivotViewColumns>
            <PivotViewColumn Name="Product"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="Country"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Amount"></PivotViewValue>
        </PivotViewValues>
    </PivotViewDataSourceSettings>
</SfPivotView>
```

> **Why prefer Web API?** It decouples data access from UI, supports caching, authentication, and works with all Blazor hosting models including WASM.
