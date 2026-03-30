# Data Binding — Syncfusion Blazor Pivot Table

## ⚠️ SECURITY NOTICE

**CRITICAL**: This documentation contains examples for connecting to remote data sources. **Always use trusted, authenticated data sources only.** Never bind to untrusted URLs, user-provided endpoints, or public APIs without proper validation and security controls. See the [Security Best Practices](#security-best-practices) section below.

## Table of Contents
- [Security Best Practices](#security-best-practices)
- [Local JSON / IEnumerable Binding](#local-json--ienumerable-binding)
- [SfDataManager with JsonAdaptor](#sfdatamanager-with-jsonadaptor)
- [Remote Data (WebAPI / OData)](#remote-data-webapi--odata)
- [OLAP Cube Data Source](#olap-cube-data-source)
- [Dynamic Data Objects](#dynamic-data-objects)
- [ExpandAll on Load](#expandall-on-load)
- [Refresh Data Programmatically](#refresh-data-programmatically)

---

## Security Best Practices

### Critical Security Guidelines

When implementing data binding for pivot tables, follow these essential security practices:

#### 1. **Use Trusted Data Sources Only**

✅ **RECOMMENDED**: Local in-memory data
```csharp
// Safe: Local data list
public List<ProductDetails> data { get; set; }
protected override void OnInitialized()
{
    data = ProductDetails.GetProductData().ToList();
}
```

❌ **AVOID**: Untrusted remote endpoints
```csharp
// UNSAFE: Never use arbitrary or user-provided URLs
Url = userProvidedUrl  // Security risk!
```

#### 2. **Implement Authentication for Remote Sources**

If you must use remote data, always use authenticated endpoints:

```csharp
// Use authenticated API with proper configuration
<SfDataManager Url="@Configuration["ApiEndpoint"]" 
               Adaptor="Adaptors.WebApiAdaptor"
               Headers="@headers">
</SfDataManager>

@code {
    private object[] headers = new object[] {
        new { Authorization = $"Bearer {AuthToken}" }
    };
}
```

#### 3. **Validate and Sanitize Data**

Always validate data received from external sources:

```csharp
protected override async Task OnInitializedAsync()
{
    var response = await Http.GetFromJsonAsync<List<ProductDetails>>(trustedEndpoint);
    
    // Validate data structure
    if (ValidateDataStructure(response))
    {
        data = SanitizeData(response);
    }
}
```

#### 4. **Use Configuration Settings**

Store API endpoints in configuration, never hardcode:

```json
// appsettings.json
{
  "ApiEndpoint": "https://your-trusted-api.com/data",
  "AllowedOrigins": ["https://your-domain.com"]
}
```

### Security Risks to Avoid

| Risk | Description | Mitigation |
|------|-------------|------------|
| **Indirect Prompt Injection** | Malicious data manipulating AI behavior | Validate and sanitize all external data |
| **Data Exfiltration** | Unauthorized access to sensitive data | Use authentication and authorization |
| **SSRF Attacks** | Server-side request forgery via URLs | Whitelist allowed endpoints |
| **XSS via Data** | Malicious scripts in data fields | Sanitize and escape all data |
| **Credential Exposure** | API keys or tokens in code | Use configuration and secure storage |

---

## Local JSON / IEnumerable Binding

Assign a `List<T>` (or any `IEnumerable<T>`) directly to the `DataSource` property inside `PivotViewDataSourceSettings`. This is the default and most common approach.

```razor
@using Syncfusion.Blazor.PivotView

<SfPivotView TValue="ProductDetails">
    <PivotViewDataSourceSettings DataSource="@data">
        <PivotViewColumns>
            <PivotViewColumn Name="Year"></PivotViewColumn>
            <PivotViewColumn Name="Quarter"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="Country"></PivotViewRow>
            <PivotViewRow Name="Products"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Sold" Caption="Unit Sold"></PivotViewValue>
            <PivotViewValue Name="Amount" Caption="Sold Amount"></PivotViewValue>
        </PivotViewValues>
        <PivotViewFormatSettings>
            <PivotViewFormatSetting Name="Amount" Format="C"></PivotViewFormatSetting>
        </PivotViewFormatSettings>
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    public List<ProductDetails> data { get; set; }
    protected override void OnInitialized()
    {
        data = ProductDetails.GetProductData().ToList();
    }
}
```

---

## SfDataManager with JsonAdaptor

Use `SfDataManager` with `JsonAdaptor` when you need to pass in-memory JSON (e.g., deserialized from an API response) without binding it as a typed list.

```razor
@using Syncfusion.Blazor.PivotView
@using Syncfusion.Blazor.Data

<SfPivotView TValue="ProductDetails" Width="1500" Height="300">
    <PivotViewDataSourceSettings TValue="ProductDetails" EnableSorting="true">
        <SfDataManager Json="@data" Adaptor="Syncfusion.Blazor.Adaptors.JsonAdaptor">
        </SfDataManager>
        <PivotViewColumns>
            <PivotViewColumn Name="Year"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="Country"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Sold" Caption="Units Sold"></PivotViewValue>
        </PivotViewValues>
    </PivotViewDataSourceSettings>
    <PivotViewGridSettings ColumnWidth="120"></PivotViewGridSettings>
</SfPivotView>

@code {
    ProductDetails[] data { get; set; }
    protected override void OnInitialized()
    {
        data = ProductDetails.GetProductData().ToArray();
    }
}
```

---

## Remote Data (WebAPI / OData)

⚠️ **SECURITY WARNING**: Only use trusted, authenticated endpoints. Never bind to user-provided or untrusted URLs.

Point `SfDataManager` at a **trusted and authenticated** API endpoint and choose the appropriate adaptor:
- `WebApiAdaptor` — REST API returning JSON
- `ODataV4Adaptor` — OData v4 protocol
- `UrlAdaptor` — Custom endpoint with Syncfusion query format

```razor
@using Syncfusion.Blazor.PivotView
@using Syncfusion.Blazor.Data
@inject IConfiguration Configuration

<SfPivotView TValue="OrderData" Height="450">
    <PivotViewDataSourceSettings TValue="OrderData" ExpandAll="false">
        <!-- ⚠️ SECURITY: Only use controlled, trusted endpoints -->
        <SfDataManager Url="@Configuration["ApiEndpoint"]"
                       Adaptor="Syncfusion.Blazor.Adaptors.WebApiAdaptor" CrossDomain="true">
        </SfDataManager>
        <PivotViewColumns>
            <PivotViewColumn Name="Year"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="ProductName"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Quantity" Caption="Qty Sold"></PivotViewValue>
            <PivotViewValue Name="UnitPrice" Caption="Total Revenue"></PivotViewValue>
        </PivotViewValues>
    </PivotViewDataSourceSettings>
</SfPivotView>
```

> **Note:** The API must support `$filter`, `$top`, and `$skip` query parameters for the adaptor to function correctly. For custom APIs, return data in `{ result: [], count: N }` format with `UrlAdaptor`.

---

## OLAP Cube Data Source

Connect to OLAP cubes (SQL Server Analysis Services, SSAS) using `PivotViewOlapDataSourceSettings`:

```razor
@using Syncfusion.Blazor.PivotView

<SfPivotView TValue="string">
    <PivotViewDataSourceSettings TValue="string"
        ProviderType="ProviderType.SSAS"
        Catalog="Adventure Works DW 2008 SE"
        Cube="Adventure Works"
        Url="https://demos.syncfusion.com/olap/msmdpump.dll"
        LocaleIdentifier="1033" EnableSorting="true">
        <PivotViewColumns>
            <PivotViewColumn Name="[Product].[Product Categories]" Caption="Product Categories">
            </PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="[Customer].[Customer Geography]" Caption="Customer Geography">
            </PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="[Measures].[Customer Count]" Caption="Customer Count">
            </PivotViewValue>
            <PivotViewValue Name="[Measures].[Internet Sales Amount]" Caption="Internet Sales">
            </PivotViewValue>
        </PivotViewValues>
    </PivotViewDataSourceSettings>
</SfPivotView>
```

> **Tip:** OLAP-specific features like MDX queries and member leveling require the toolbar's MDX option to be enabled via `ToolbarItems.MDX`.

---

## Dynamic Data Objects

The Pivot Table supports `ExpandoObject` and other dynamic types when the schema is not known at compile time:

```razor
<SfPivotView TValue="ExpandoObject">
    <PivotViewDataSourceSettings DataSource="@data">
        ...
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    List<ExpandoObject> data = new();
    protected override void OnInitialized()
    {
        dynamic row1 = new ExpandoObject();
        row1.Country = "France"; row1.Year = "FY 2023"; row1.Amount = 5000;
        data.Add(row1);
        // add more rows...
    }
}
```

---

## ExpandAll on Load

By default, only top-level members are shown. Set `ExpandAll="true"` to expand all hierarchy levels on initial render:

```razor
<PivotViewDataSourceSettings DataSource="@data" ExpandAll="true">
```

> **Performance note:** `ExpandAll` on large datasets increases initial render time. Prefer expanding specific members programmatically for better UX.

---

## Refresh Data Programmatically

### Using RefreshAsync Method

The `RefreshAsync()` method provides dynamic and asynchronous refreshing of the Pivot Table. Use it to update the component after programmatic changes to the data source or report configuration.

#### Method Signature

```csharp
Task RefreshAsync(bool updateDataSource = false)
```

**Parameters:**
- `updateDataSource` (bool): 
  - `true`: Full refresh including data re-processing (complete engine rebuild)
  - `false`: Layout-only refresh (default, faster)

#### When to Use RefreshAsync

- After programmatically adding/removing fields from axes
- After changing aggregation types or calculated fields
- After modifying filter or sort settings
- When data source properties change
- After updating formatting rules

#### Full Refresh Example

Use `true` parameter when the underlying data has changed or when adding/removing fields:

```razor
<button @onclick="AddProductField">Add Product to Rows</button>

<SfPivotView @ref="pivotView" TValue="ProductDetails">
    <PivotViewDataSourceSettings DataSource="@data">
        <PivotViewColumns>
            <PivotViewColumn Name="Year"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="Country"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Sold" Caption="Units Sold"></PivotViewValue>
            <PivotViewValue Name="Amount" Caption="Sales Amount"></PivotViewValue>
        </PivotViewValues>
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    SfPivotView<ProductDetails> pivotView;
    List<ProductDetails> data = ProductDetails.GetProductData().ToList();

    private async Task AddProductField()
    {
        // Add new field programmatically
        // Note: In practice, you'd manipulate the report configuration
        
        // Full refresh to rebuild the engine with new field
        await pivotView.RefreshAsync(true);
    }
}
```

#### Layout Refresh Example

Use `false` (or omit parameter) for UI-only changes that don't require data reprocessing:

```razor
<button @onclick="ChangeFormat">Update Formatting</button>

<SfPivotView @ref="pivotView" TValue="ProductDetails">
    <PivotViewDataSourceSettings DataSource="@data">
        <!-- configuration -->
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    SfPivotView<ProductDetails> pivotView;

    private async Task ChangeFormat()
    {
        // Modify formatting or other display properties
        
        // Layout refresh only (faster)
        await pivotView.RefreshAsync(false);
        // or simply: await pivotView.RefreshAsync();
    }
}
```

#### Updating Data and Refreshing

When data changes, refresh to reflect updates:

```razor
<button @onclick="LoadNewData">Load Updated Data</button>

<SfPivotView @ref="pivotView" TValue="ProductDetails">
    <PivotViewDataSourceSettings DataSource="@data">
        <PivotViewColumns>
            <PivotViewColumn Name="Year"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="Country"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Sold" Caption="Units Sold"></PivotViewValue>
        </PivotViewValues>
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    SfPivotView<ProductDetails> pivotView;
    List<ProductDetails> data;

    protected override void OnInitialized()
    {
        data = ProductDetails.GetProductData().ToList();
    }

    private async Task LoadNewData()
    {
        // Update data source
        data = await FetchLatestDataFromDatabase();
        
        // Full refresh to process new data
        await pivotView.RefreshAsync(true);
    }
}
```

#### Performance Considerations

| Refresh Type | Speed | Use When |
|--------------|-------|----------|
| **Full Refresh** (`true`) | Slower | Data changed, fields added/removed, structure modified |
| **Layout Refresh** (`false`) | Faster | Formatting changes, UI-only updates, display properties |

**Best Practice:** Use layout refresh (false) when possible for better performance.

### Alternative: Data Reassignment Pattern

For simple data updates, you can reassign the data collection to trigger a re-render:

```razor
<SfPivotView TValue="ProductDetails" @ref="pivot">
    <PivotViewDataSourceSettings DataSource="@data"> ... </PivotViewDataSourceSettings>
</SfPivotView>
<button @onclick="Refresh">Refresh</button>

@code {
    SfPivotView<ProductDetails> pivot;
    List<ProductDetails> data = ProductDetails.GetProductData().ToList();

    void Refresh()
    {
        // Replace list reference so Blazor detects the change
        data = FetchNewData().ToList();
        StateHasChanged();
    }
}
```

> **Note:** Data reassignment works for simple scenarios. Use `RefreshAsync()` for complex updates or when you need fine control over the refresh behavior.
