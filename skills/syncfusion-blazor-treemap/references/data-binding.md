# Data Binding

## Table of Contents
- [Overview](#overview)
- [Flat Data](#flat-data)
- [Hierarchical Data](#hierarchical-data)
- [Local and Remote Sources](#local-and-remote-sources)
- [Data Adaptor Patterns](#data-adaptor-patterns)
- [Edge Cases](#edge-cases)

## Overview
This file explains how to bind flat and hierarchical data to the TreeMap, including common patterns for `GroupPath`, `WeightValuePath`, and transforming datasets.

## Flat Data
- Provide an array of objects and set `WeightValuePath` to the numeric value field.

## Hierarchical Data
- Use `GroupPath` or nested collections to represent levels. Flatten or adapt JSON when required.

## Local and Remote Sources
- Local: in-memory collections or JSON files.
- Remote: fetch data only from trusted internal APIs (or local packaged files). Do NOT bind directly to untrusted third-party endpoints; proxy third-party feeds through a server where you can authenticate, validate, sanitize, and enforce rate limits and payload-size limits.

## Data Adaptor Patterns
- Normalize remote responses to objects with `GroupPath` and `WeightValuePath`.

## Edge Cases
- Empty points, null values, and inconsistent nesting require sanitization before binding.
# Data Binding in Blazor TreeMap Component

## Table of Contents

- [Overview](#overview)
- [Understanding Data Structures](#understanding-data-structures)
  - [Flat Data Structure](#flat-data-structure)
  - [Hierarchical Data Structure](#hierarchical-data-structure)
- [Key Properties for Data Binding](#key-properties-for-data-binding)
  - [DataSource Property](#datasource-property)
  - [WeightValuePath](#weightvaluepath)
  - [RangeColorValuePath](#rangecolorvaluepath)
  - [EqualColorValuePath](#equalcolorvaluepath)
  - [ColorValuePath](#colorvaluepath)
- [Local Data Binding](#local-data-binding)
  - [Binding IEnumerable Collections](#binding-ienumerable-collections)
  - [Loading JSON Data](#loading-json-data)
  - [Best Practices for Local Data](#best-practices-for-local-data)
- [Remote Data Binding](#remote-data-binding)
  - [Using SfDataManager](#using-sfdatamanager)
  - [OData Services](#odata-services)
  - [OData V4 Services](#odata-v4-services)
  - [Web API Integration](#web-api-integration)
- [Entity Framework Integration](#entity-framework-integration)
  - [Creating DBContext Class](#creating-dbcontext-class)
  - [Data Access Layer](#data-access-layer)
  - [Web API Controller Setup](#web-api-controller-setup)
  - [Binding TreeMap to Entity Framework](#binding-treemap-to-entity-framework)
  - [Creating DBContext Class](#creating-dbcontext-class)
  - [Data Access Layer](#data-access-layer)
  - [Web API Controller Setup](#web-api-controller-setup)
  - [Binding TreeMap to Entity Framework](#binding-treemap-to-entity-framework)
- [Data Transformation Techniques](#data-transformation-techniques)
- [Troubleshooting](#troubleshooting)
- [Performance Considerations](#performance-considerations)

## Overview

Data binding is the fundamental mechanism that connects your data source to the TreeMap component. The TreeMap uses the `DataSource` property to accept data in various formats, enabling visualization of hierarchical or flat data structures. Proper data binding ensures accurate representation of your data through size, color, and hierarchical organization of TreeMap items.

## IMPORTANT: Security-first data binding policy

Do NOT bind the TreeMap directly to untrusted third-party endpoints or user-supplied URLs. The component can ingest remote data which may contain malicious payloads, large nested objects, or crafted values that can affect rendering, expose sensitive information, or cause denial-of-service. Follow these mandatory rules:

- Only bind to internal, authenticated APIs that you control, or to local/sample data packaged with the application.
- If you must use third-party data, proxy it through your server where you can validate, sanitize, and enforce rate limits and size caps.
- Never perform unauthenticated client-side fetches from arbitrary external hosts.
- Include server-side validation and sanitization for all numeric and string fields returned to the client.
- Keep the client-side examples in this repository limited to local sample data or clearly-labeled, internal API placeholders.

See the "Validation Checklist for Remote Data Binding" section for required server-side and deployment controls.

## ⚠️ Security Considerations for Data Binding

### Third-Party Data Exposure Risk

When binding TreeMap to remote or third-party data sources (public APIs, OData endpoints, or external Web APIs), you expose your application to potential security risks:

**Key Risks:**
- **Malicious Data Injection**: Untrusted data can be crafted to execute unintended behavior
- **XSS (Cross-Site Scripting)**: If data is rendered directly without sanitization, it can execute malicious scripts
- **Data Tampering**: Public APIs can be compromised or intercepted, leading to data integrity issues
- **Visualization Manipulation**: Malicious numeric or categorical data can skew charts or mislead users
- **Performance Attacks**: Large or nested payloads can cause DoS (Denial of Service) attacks

### Security Best Practices

**1. Only Bind to Trusted Internal APIs**

- ✅ Your organization's internal APIs (with authentication)
- ✅ APIs you own and control
- ✅ Vetted third-party APIs with strong security records
- ❌ Do NOT use public/untrusted OData or demo endpoints

**2. Always Use Authentication**

Use JWT/OAuth 2.0 tokens with your API endpoints:

```csharp
@code {
    public Dictionary<string, string> AuthHeaders { get; set; }
    
    protected override void OnInitialized()
    {
        AuthHeaders = new Dictionary<string, string>
        {
            { "Authorization", $"Bearer {GetSecureToken()}" }
        };
    }
    
    private string GetSecureToken()
    {
        // Retrieve JWT token from secure authentication service
        return "your-jwt-token";
    }
}
```

**3. Implement Server-Side Data Validation**

Add validation and sanitization on your backend API:

```csharp
[Authorize]
[HttpGet("api/orders")]
public async Task<IActionResult> GetOrders()
{
    // Get authenticated user
    var userId = User.FindFirst(ClaimTypes.NameIdentifier)?.Value;
    if (string.IsNullOrEmpty(userId))
        return Unauthorized();
    
    // Fetch data with user-specific authorization
    var orders = await db.Orders
        .Where(o => o.UserId == userId)
        .ToListAsync();
    
    // Validate and sanitize data before returning
    foreach (var order in orders)
    {
        // Clamp numeric values
        if (order.Freight < 0) order.Freight = 0;
        if (order.Freight > 100000) order.Freight = 100000;
        
        // HTML-encode string properties
        order.ShipCity = System.Web.HttpUtility.HtmlEncode(order.ShipCity);
    }
    
    return Ok(orders);
}
```

**4. Use HTTPS Only**

- Always use `https://` URLs for all API endpoints
- Ensure your application validates SSL/TLS certificates
- In production, consider certificate pinning for critical APIs

### When to Use Remote Data Binding Safely

**Safe Scenarios:**
- ✅ Binding to your organization's internal APIs with proper authentication
- ✅ Public APIs with data you own and control
- ✅ Third-party APIs with strong track records and security certifications
- ✅ APIs behind corporate firewalls and VPNs
- ✅ APIs that return validated and sanitized data

**Unsafe Scenarios:**
- ❌ Completely untrusted public APIs
- ❌ User-supplied API URLs without validation
- ❌ APIs without proper authentication/authorization
- ❌ APIs that return unvalidated HTML or JavaScript
- ❌ APIs with no rate limiting or abuse protection

### Validation Checklist for Remote Data Binding

- [ ] API endpoint uses HTTPS
- [ ] API requires authentication (JWT, OAuth 2.0, etc.)
- [ ] API has authorization controls
- [ ] Data is validated on server-side before returning to client
- [ ] Response payload size is limited (pagination implemented)
- [ ] Rate limiting is enforced
- [ ] Request/response logging is enabled for audit trails
- [ ] CORS is properly configured with specific origins
- [ ] Client-side sanitization of display data is implemented
- [ ] Error responses don't expose sensitive information

## Understanding Data Structures

### Flat Data Structure

Flat data represents a simple list of items without nested relationships. Each item in the collection is at the same level, making it ideal for visualizing simple datasets where hierarchy is not required.

**When to use flat data:**
- Single-level categorization
- Simple comparisons across items
- Direct property mapping without nesting

**Example: GDP Comparison**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="GDP" TValue="GDPReport" DataSource="GrowthReports">
    <TreeMapLeafItemSettings LabelPath="Country">
    </TreeMapLeafItemSettings>
</SfTreeMap>

@code {
    public class GDPReport
    {
        public string Country { get; set; }
        public double GDP { get; set; }
        public double Percentage { get; set; }
        public double Rank { get; set; }
    }
    
    public List<GDPReport> GrowthReports = new List<GDPReport> {
        new GDPReport {Country = "United States", GDP=17946, Percentage=11.08, Rank=1},
        new GDPReport {Country="China", GDP=10866, Percentage= 28.42, Rank=2},
        new GDPReport {Country="Japan", GDP=4123, Percentage=-30.78, Rank=3},
        new GDPReport {Country="Germany", GDP=3355, Percentage=-5.19, Rank=4},
        new GDPReport {Country="United Kingdom", GDP=2848, Percentage=8.28, Rank=5},
        new GDPReport {Country="France", GDP=2421, Percentage=-9.69, Rank=6},
        new GDPReport {Country="India", GDP=2073, Percentage=13.65, Rank=7},
        new GDPReport {Country="Italy", GDP=1814, Percentage=-12.45, Rank=8},
        new GDPReport {Country="Brazil", GDP=1774, Percentage=-27.88, Rank=9},
        new GDPReport {Country="Canada", GDP=1550, Percentage=-15.02, Rank=10}
    };
}
```

### Hierarchical Data Structure

Hierarchical data represents nested relationships where items can contain child items at multiple levels. This structure is perfect for visualizing organizational data, geographical regions, or any multi-level categorization.

**When to use hierarchical data:**
- Multi-level organizational structures
- Nested categorizations
- Drill-down requirements
- Parent-child relationships

**Example: Population by Continent and States**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Population" DataSource="PopulationReport">
    <TreeMapLeafItemSettings LabelPath="Name" Fill="#0077b3">
        <TreeMapLeafBorder Width="0.5" Color="black"></TreeMapLeafBorder>
    </TreeMapLeafItemSettings>
    <TreeMapLevels>
        <TreeMapLevel GroupPath="Continent" Fill="#004466">
            <TreeMapLevelBorder Width="0.5" Color="black"></TreeMapLevelBorder>
        </TreeMapLevel>
        <TreeMapLevel GroupPath="States" Fill="#0099e6">
            <TreeMapLevelBorder Width="0.5" Color="black"></TreeMapLevelBorder>
        </TreeMapLevel>
    </TreeMapLevels>
</SfTreeMap>

@code{
    public List<object> PopulationReport { get; set; } = new List<object>
    {
        new {
            Continent = new List<object> { new {
            Name= "Africa",
            Population= 1216130000,
            States= new List<object> { new {
                Name= "Eastern Africa",
                Population= 410637987,
                Region= new List<object> { new {
                    Name= "Ethiopia",
                    Population= 107534882
                    }}
                },
            new {
                Name= "Middle Africa",
                Population= 158562976,
                Region= new List<object>{ new {
                        Name= "Democratic, Republic of the Congo",
                        Population= 84004989
                    }}
                    }
                }
            }}
        },
        new {
            Continent= new List<object> { new {
                Name= "Asia",
                Population= 4436224000,
                States= new List<object> { new {
                        Name= "Central Asia",
                        Population= 69787760,
                        Region= new List<object> { new {
                            Name= "Uzbekistan",
                            Population= 32364996
                        }}
                    },
                    new {
                        Name= "Eastern Asia",
                        Population= 1641908531,
                        Region= new List<object> { new {
                            Name= "China",
                            Population= 1415045928
                        }}
                    }
                }
            }}
        }
    };
}
```

**Key Difference:** Flat data uses simple properties, while hierarchical data uses nested collections with `List<object>` or custom typed collections.

## Key Properties for Data Binding

### DataSource Property

The `DataSource` property is the primary mechanism for providing data to the TreeMap. It accepts various collection types:

- `List<T>` - Strongly typed lists
- `IEnumerable<T>` - Any enumerable collection
- `object` collections - For hierarchical data
- `SfDataManager` - For remote data sources

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="@MyDataCollection" TValue="MyDataType" WeightValuePath="Value">
</SfTreeMap>
```

**Important:** Always specify the `TValue` generic parameter to match your data type.

### WeightValuePath

The `WeightValuePath` property determines the size of each TreeMap item. It should reference a numeric property in your data source.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="GDP" TValue="GDPReport" DataSource="GrowthReports">
</SfTreeMap>
```

**Rules:**
- Must reference a numeric property (int, double, decimal, float)
- Larger values create larger rectangles
- Cannot be null or negative
- Zero values create items with minimal size

### RangeColorValuePath

Used with range color mapping to apply colors based on numeric value ranges. This property specifies which data field contains the values for color determination.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" RangeColorValuePath="Count" 
           TValue="Fruit" DataSource="Fruits">
    <TreeMapLeafItemSettings LabelPath="FruitName">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping StartRange="500" EndRange="3000" 
                                    Color='new string[] { "Orange" }'>
            </TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping StartRange="3000" EndRange="5000" 
                                    Color='new string[] { "Green" }'>
            </TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

### EqualColorValuePath

Used with equal color mapping to apply specific colors to items with matching values. Ideal for categorical data.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" EqualColorValuePath="Brand" 
           TValue="Car" DataSource="Cars">
    <TreeMapLeafItemSettings LabelPath="Name">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping LeafValue="Ford" Color='new string[] { "green" }'>
            </TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Audi" Color='new string[] { "red" }'>
            </TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

### ColorValuePath

Directly binds colors from the data source, allowing each item to have its own color specified in the data.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" ColorValuePath="Color" 
           TValue="Fruit" DataSource="Fruits">
    <TreeMapLeafItemSettings LabelPath="Name"></TreeMapLeafItemSettings>
</SfTreeMap>

@code {
    public class Fruit
    {
        public string Name { get; set; }
        public double Count { get; set; }
        public string Color { get; set; }
    }
    
    public List<Fruit> Fruits = new List<Fruit> {
        new Fruit { Name="Apple", Count=5000, Color = "red" },
        new Fruit { Name="Mango", Count=3000, Color="blue" },
        new Fruit { Name="Orange", Count=2300, Color="green" }
    };
}
```

## Local Data Binding

### Binding IEnumerable Collections

The TreeMap component works seamlessly with any `IEnumerable` collection, including List, Array, ObservableCollection, and custom collections.

**Example: Using List with Palette**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="GrowthReports" TValue="GDPReport" 
           Palette="@Palette" WeightValuePath="GDP">
    <TreeMapLeafItemSettings LabelPath="CountryName">
        <TreeMapLeafLabelStyle Color="White"></TreeMapLeafLabelStyle>
    </TreeMapLeafItemSettings>
</SfTreeMap>

@code{
    public class GDPReport
    {
        public string CountryName { get; set; }
        public double GDP { get; set; }
        public double Percentage { get; set; }
        public int Rank { get; set; }
    }
    
    public string[] Palette = new string[] { "#87CEFA", "#87CEEB" };
    
    public List<GDPReport> GrowthReports = new List<GDPReport> {
        new GDPReport {CountryName="United States", GDP=17946, Percentage=11.08, Rank=1},
        new GDPReport {CountryName="China", GDP=10866, Percentage= 28.42, Rank=2},
        new GDPReport {CountryName="Japan", GDP=4123, Percentage=-30.78, Rank=3},
        new GDPReport {CountryName="Germany", GDP=3355, Percentage=-5.19, Rank=4},
        new GDPReport {CountryName="United Kingdom", GDP=2848, Percentage=8.28, Rank=5},
        new GDPReport {CountryName="France", GDP=2421, Percentage=-9.69, Rank=6},
        new GDPReport {CountryName="India", GDP=2073, Percentage=13.65, Rank=7},
        new GDPReport {CountryName="Italy", GDP=1814, Percentage=-12.45, Rank=8},
        new GDPReport {CountryName="Brazil", GDP=1774, Percentage=-27.88, Rank=9},
        new GDPReport {CountryName="Canada", GDP=1550, Percentage=-15.02, Rank=10}
    };
}
```

### Loading JSON Data

For scenarios where data is stored in JSON files, use `HttpClient` to fetch and deserialize the data.

```cshtml
@using Syncfusion.Blazor.TreeMap
@inject NavigationManager Navigation
@inject HttpClient Http

@if (GrowthReports == null)
{
    <p><em>Loading TreeMap component...</em></p>
}
else
{
    <SfTreeMap WeightValuePath="GDP" TValue="GDPReport" DataSource="GrowthReports">
        <TreeMapLeafItemSettings LabelPath="Country">
        </TreeMapLeafItemSettings>
    </SfTreeMap>
}

@code{
    public List<GDPReport> GrowthReports { get; set; }
    
    protected async override Task OnInitializedAsync()
    {
        try
        {
            // Use only local packaged JSON or your own secure, authenticated APIs.
            // Never fetch arbitrary third-party URLs from the client.
            GrowthReports = await Http.GetFromJsonAsync<List<GDPReport>>(Navigation.ToAbsoluteUri("sample-data/product-growth.json"));

            // Basic client-side validation/sanitization (do not rely on client checks alone)
            if (GrowthReports == null)
                GrowthReports = new List<GDPReport>();

            foreach (var item in GrowthReports)
            {
                if (item.GDP < 0) item.GDP = 0;
                item.Country = System.Net.WebUtility.HtmlEncode(item.Country ?? string.Empty);
            }
        }
        catch (Exception)
        {
            // Loading failed: prefer showing a safe fallback and logging the error.
            GrowthReports = new List<GDPReport>();
        }
    }
    
    public class GDPReport
    {
        public string Country { get; set; }
        public int GDP { get; set; }
        public double Percentage { get; set; }
        public int Rank { get; set; }
    }
}
```

**JSON File Structure (sample-data/product-growth.json):**

```json
[
    {"Country": "United States", "GDP": 17946, "Percentage": 11.08, "Rank": 1},
    {"Country": "China", "GDP": 10866, "Percentage": 28.42, "Rank": 2},
    {"Country": "Japan", "GDP": 4123, "Percentage": -30.78, "Rank": 3}
]
```

### Best Practices for Local Data

1. **Use strongly-typed collections** for compile-time safety
2. **Initialize data in OnInitializedAsync** for async operations
3. **Handle null checks** before rendering TreeMap
4. **Use loading indicators** during data fetch
5. **Implement error handling** for failed data loads

```cshtml
@code{
    protected override async Task OnInitializedAsync()
    {
        try
        {
            GrowthReports = await LoadDataAsync();
        }
        catch (Exception ex)
        {
            ErrorMessage = $"Failed to load data: {ex.Message}";
        }
    }
}
```

## Remote Data Binding

### ⚠️ Security Warning: Remote Data Sources

**Before proceeding with remote data binding, review the [Security Considerations section](#️-security-considerations-for-data-binding) above.** 

Do NOT bind TreeMap to untrusted public APIs. Always validate, sanitize, and authenticate remote data sources. Only bind to APIs you own, control, or explicitly trust.

### Using SfDataManager

The `SfDataManager` component provides a unified interface for accessing remote data sources. When using `SfDataManager`, you must explicitly specify the `TValue` parameter.

**Key Points:**
- Supports multiple adaptors (OData, ODataV4, WebApi, Url)
- Handles query operations automatically
- Provides built-in error handling
- Supports paging and filtering
- **MUST be paired with server-side validation and authorization**

### OData Services (Internal APIs Only)

OData (Open Data Protocol) is a standardized REST-based protocol for querying and updating data. **Only use with internal, authenticated APIs.**

```cshtml
@using Syncfusion.Blazor.TreeMap
@using Syncfusion.Blazor.Data

<!-- ✅ SECURE: Uses internal API with authentication -->
<SfTreeMap TValue="OrderDetails" WeightValuePath="Freight" Palette="@Palette">
    <SfDataManager Url="https://your-internal-api.yourdomain.com/api/odata/orders" 
                   Adaptor="Syncfusion.Blazor.Adaptors.ODataAdaptor"
                   Headers="@AuthHeaders">
    </SfDataManager>
    <TreeMapTitleSettings Text="Order Details">
    </TreeMapTitleSettings>
    <TreeMapLeafItemSettings LabelPath="ShipCountry">
        <TreeMapLeafBorder Color="white" Width="0.5">
        </TreeMapLeafBorder>
    </TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible="true">
    </TreeMapTooltipSettings>
</SfTreeMap>

@code{
    public string[] Palette = new string[] { 
        "#C33764", "#AB3566", "#993367", "#853169", 
        "#742F6A", "#632D6C", "#532C6D", "#412A6F", 
        "#312870", "#1D2671" 
    };
    
    // Include authentication headers
    public Dictionary<string, string> AuthHeaders { get; set; }
    
    protected override void OnInitialized()
    {
        AuthHeaders = new Dictionary<string, string>
        {
            { "Authorization", $"Bearer {GetSecureToken()}" }
        };
    }
    
    private string GetSecureToken()
    {
        // Retrieve JWT token from secure storage
        // Never hardcode tokens!
        return "your-jwt-token";
    }
    
    public class OrderDetails
    {
        public int OrderID { get; set; }
        public string OrderDate { get; set; }
        public string CustomerID { get; set; }
        public string ShipCountry { get; set; }
        public double Freight { get; set; }
    }
}
```

### OData V4 Services (Internal APIs Only)

OData V4 is an enhanced version of OData with improved functionality and performance. **Use only with internal, authenticated APIs.**

```cshtml
@using Syncfusion.Blazor.TreeMap
@using Syncfusion.Blazor.Data

<!-- ✅ SECURE: Uses internal API with authentication -->
<SfTreeMap TValue="OrderDetails" WeightValuePath="Freight" Palette="@Palette">
    <SfDataManager Url="https://your-internal-api.yourdomain.com/api/odata/v4/orders" 
                   Adaptor="Syncfusion.Blazor.Adaptors.ODataV4Adaptor"
                   Headers="@AuthHeaders">
    </SfDataManager>
    <TreeMapTitleSettings Text="Order Details">
    </TreeMapTitleSettings>
    <TreeMapLeafItemSettings LabelPath="ShipCountry">
        <TreeMapLeafBorder Color="white" Width="0.5">
        </TreeMapLeafBorder>
    </TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible="true"></TreeMapTooltipSettings>
</SfTreeMap>

@code{
    public string[] Palette = new string[] { 
        "#C33764", "#AB3566", "#993367", "#853169", 
        "#742F6A", "#632D6C", "#532C6D", "#412A6F", 
        "#312870", "#1D2671" 
    };
    
    public Dictionary<string, string> AuthHeaders { get; set; }
    
    protected override void OnInitialized()
    {
        AuthHeaders = new Dictionary<string, string>
        {
            { "Authorization", $"Bearer {GetSecureToken()}" }
        };
    }
    
    private string GetSecureToken()
    {
        // Retrieve from secure authentication service
        return "your-jwt-token";
    }
    
    public class OrderDetails
    {
        public int OrderID { get; set; }
        public string OrderDate { get; set; }
        public string CustomerID { get; set; }
        public string ShipCountry { get; set; }
        public double Freight { get; set; }
    }
}
```

### Web API Integration (Recommended Secure Approach)

Use `WebApiAdaptor` with your own backend Web API that implements proper security controls.

```cshtml
@using Syncfusion.Blazor.TreeMap
@using Syncfusion.Blazor.Data

<!-- ✅ SECURE: Uses your backend API with auth headers -->
<SfTreeMap TValue="OrderDetails" WeightValuePath="Freight" Palette="@Palette">
    <SfDataManager Url="https://your-domain.com/api/secure/orders" 
                   Adaptor="Syncfusion.Blazor.Adaptors.WebApiAdaptor"
                   Headers="@AuthHeaders">
    </SfDataManager>
    <TreeMapTitleSettings Text="Order Details">
    </TreeMapTitleSettings>
    <TreeMapLeafItemSettings LabelPath="ShipCity">
        <TreeMapLeafBorder Color="white" Width="0.5">
        </TreeMapLeafBorder>
    </TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible="true">
    </TreeMapTooltipSettings>
</SfTreeMap>

@code{
    public string[] Palette = new string[] { 
        "#C33764", "#AB3566", "#993367", "#853169", 
        "#742F6A", "#632D6C", "#532C6D", "#412A6F", 
        "#312870", "#1D2671" 
    };
    
    public Dictionary<string, string> AuthHeaders { get; set; }
    
    protected override void OnInitialized()
    {
        AuthHeaders = new Dictionary<string, string>
        {
            { "Authorization", $"Bearer {GetSecureToken()}" }
        };
    }
    
    private string GetSecureToken()
    {
        // Retrieve JWT from secure service
        return "your-jwt-token";
    }
    
    public class OrderDetails
    {
        public int OrderID { get; set; }
        public string OrderDate { get; set; }
        public string CustomerID { get; set; }
        public string ShipCity { get; set; }
        public double Freight { get; set; }
    }
}
```

**Backend API Controller with Security:**

```csharp
using Microsoft.AspNetCore.Authorization;
using Microsoft.AspNetCore.Mvc;

namespace YourApi.Controllers
{
    [Route("api/[controller]")]
    [ApiController]
    [Authorize] // Require authentication
    public class SecureOrdersController : ControllerBase
    {
        private readonly IOrderService _orderService;
        
        public SecureOrdersController(IOrderService orderService)
        {
            _orderService = orderService;
        }
        
        [HttpGet("orders")]
        public async Task<ActionResult<IEnumerable<OrderDetails>>> GetOrders()
        {
            try
            {
                // Get authenticated user
                var userId = User.FindFirst(System.Security.Claims.ClaimTypes.NameIdentifier)?.Value;
                if (string.IsNullOrEmpty(userId))
                    return Unauthorized("User not authenticated");
                
                // Fetch data with user-specific authorization
                var orders = await _orderService.GetUserOrdersAsync(userId);
                
                // Validate and sanitize data
                ValidateOrders(orders);
                
                return Ok(orders);
            }
            catch (Exception ex)
            {
                // Log error securely (never expose stack trace to client)
                LogError(ex);
                return StatusCode(500, "An error occurred retrieving orders");
            }
        }
        
        private void ValidateOrders(List<OrderDetails> orders)
        {
            foreach (var order in orders)
            {
                // Validate numeric ranges
                if (order.Freight < 0) order.Freight = 0;
                if (order.Freight > 100000) order.Freight = 100000;
                
                // Sanitize string properties
                order.ShipCity = System.Web.HttpUtility.HtmlEncode(order.ShipCity);
                order.CustomerID = System.Web.HttpUtility.HtmlEncode(order.CustomerID);
            }
        }
        
        private void LogError(Exception ex)
        {
            // Log to application log, not to client response
            System.Diagnostics.Debug.WriteLine($"Error retrieving orders: {ex.Message}");
        }
    }
}
```

## Entity Framework Integration

### Creating DBContext Class

Create a DBContext class to establish a connection to your Microsoft SQL Server database.

```csharp
using Microsoft.EntityFrameworkCore;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using EFTreeMap.Data;

namespace EFTreeMap.Data
{
    public class OrderContext : DbContext
    {
        public virtual DbSet<Order> Orders { get; set; }

        protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
        {
            if (!optionsBuilder.IsConfigured)
            {
                // Configure the context to connect to a Microsoft SQL Server database
                optionsBuilder.UseSqlServer(
                    @"Data Source=(LocalDB)\MSSQLLocalDB;
                      AttachDbFilename='D:\blazor\EFTreeMap\App_Data\NORTHWND.MDF';
                      Integrated Security=True;Connect Timeout=30");
            }
        }
    }
    
    public class Order
    {
        [Key]
        public int? OrderID { get; set; }
        [Required]
        public string CustomerID { get; set; }
        [Required]
        public int EmployeeID { get; set; }
    }
}
```

### Data Access Layer

Create a data access layer to retrieve records from the database table.

```csharp
using Microsoft.EntityFrameworkCore;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using EFTreeMap.Data;

namespace EFTreeMap.Data
{
    public class OrderDataAccessLayer
    {
        OrderContext db = new OrderContext();
        
        // Get all Orders details
        public DbSet<Order> GetAllOrders()
        {
            try
            {
                return db.Orders;
            }
            catch
            {
                throw;
            }
        }
    }
}
```

### Web API Controller Setup

Create a Web API Controller that allows the TreeMap to consume data from Entity Framework.

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Http;
using Microsoft.AspNetCore.Mvc;
using Microsoft.Extensions.Primitives;
using EFTreeMap.Data;

namespace EFTreeMap.Controller
{
    [Route("api/[controller]")]
    [ApiController]
    public class DefaultController : ControllerBase
    {
        OrderDataAccessLayer db = new OrderDataAccessLayer();
        
        [HttpGet]
        public object Get()
        {
            IQueryable<Order> data = db.GetAllOrders().AsQueryable();
            var count = data.Count();
            var queryString = Request.Query;
            
            if (queryString.Keys.Contains("$inlinecount"))
            {
                StringValues Skip;
                StringValues Take;
                int skip = (queryString.TryGetValue("$skip", out Skip)) ? 
                           Convert.ToInt32(Skip[0]) : 0;
                int top = (queryString.TryGetValue("$top", out Take)) ? 
                          Convert.ToInt32(Take[0]) : data.Count();
                return new { Items = data.Skip(skip).Take(top), Count = count };
            }
            else
            {
                return data;
            }
        }
    }
}
```

### Binding TreeMap to Entity Framework

**Method 1: Direct Binding**

```cshtml
@inject OrderDataAccessLayer OrderData
@using EFTreeMap.Data
@using Syncfusion.Blazor.TreeMap

<SfTreeMap TValue="Order" WeightValuePath="OrderID" 
           Palette="@Palette" DataSource="@OrderData.GetAllOrders()">
    <TreeMapTitleSettings Text="Order Details">
    </TreeMapTitleSettings>
    <TreeMapLeafItemSettings LabelPath="CustomerID">
        <TreeMapLeafBorder Color="white" Width="0.5">
        </TreeMapLeafBorder>
    </TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible="true"></TreeMapTooltipSettings>
</SfTreeMap>

@code{
    public string[] Palette = new string[] { 
        "#C33764", "#AB3566", "#993367", "#853169", 
        "#742F6A", "#632D6C", "#532C6D", "#412A6F", 
        "#312870", "#1D2671" 
    };
}
```

**Method 2: Using Web API with SfDataManager**

```cshtml
@using Syncfusion.Blazor.TreeMap
@using Syncfusion.Blazor.Data

<SfTreeMap TValue="Order" WeightValuePath="OrderID" Palette="@Palette">
    <SfDataManager Url="api/Default" 
                   Adaptor="Syncfusion.Blazor.Adaptors.WebApiAdaptor">
    </SfDataManager>
    <TreeMapTitleSettings Text="Order Details">
    </TreeMapTitleSettings>
    <TreeMapLeafItemSettings LabelPath="CustomerID">
        <TreeMapLeafBorder Color="white" Width="0.5">
        </TreeMapLeafBorder>
    </TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible="true"></TreeMapTooltipSettings>
</SfTreeMap>

@code{
    public string[] Palette = new string[] { 
        "#C33764", "#AB3566", "#993367", "#853169", 
        "#742F6A", "#632D6C", "#532C6D", "#412A6F", 
        "#312870", "#1D2671" 
    };
}
```

**Startup.cs Configuration:**

```csharp
using Newtonsoft.Json.Serialization;

namespace BlazorApplication
{
    public class Startup
    {
        public void ConfigureServices(IServiceCollection services)
        {
            services.AddSingleton<OrderDataAccessLayer>();
            
            // Add services for controllers
            services.AddControllers().AddNewtonsoftJson(options =>
            {
                options.SerializerSettings.ContractResolver = new DefaultContractResolver();
            });
        }

        public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
        {
            app.UseEndpoints(endpoints =>
            {
                // Add endpoints for controller actions
                endpoints.MapDefaultControllerRoute();
            });
        }
    }
}
```

## Data Transformation Techniques

When working with complex data structures, transformation may be necessary to match TreeMap requirements.

**Example: Flattening Nested Data**

```cshtml
@code {
    private List<FlattenedData> TransformHierarchicalData(List<HierarchicalData> source)
    {
        var result = new List<FlattenedData>();
        
        foreach (var continent in source)
        {
            foreach (var country in continent.Countries)
            {
                result.Add(new FlattenedData
                {
                    Continent = continent.Name,
                    Country = country.Name,
                    Population = country.Population
                });
            }
        }
        
        return result;
    }
}
```

**Example: Aggregating Data**

```cshtml
@code {
    private List<AggregatedData> AggregateData(List<DetailedData> source)
    {
        return source
            .GroupBy(x => x.Category)
            .Select(g => new AggregatedData
            {
                Category = g.Key,
                TotalValue = g.Sum(x => x.Value),
                ItemCount = g.Count()
            })
            .ToList();
    }
}
```

## Troubleshooting

### Data Not Displaying

**Problem:** TreeMap renders but shows no data.

**Solutions:**
1. Verify `WeightValuePath` references a numeric property
2. Check that `TValue` matches your data type
3. Ensure `DataSource` is not null or empty
4. Confirm property names match exactly (case-sensitive)

```cshtml
@code {
    // Add diagnostic output
    protected override void OnAfterRender(bool firstRender)
    {
        if (firstRender)
        {
            Console.WriteLine($"Data Count: {GrowthReports?.Count ?? 0}");
            Console.WriteLine($"First Item: {GrowthReports?.FirstOrDefault()?.Country}");
        }
    }
}
```

### Incorrect Item Sizes

**Problem:** TreeMap items have unexpected sizes.

**Solutions:**
1. Check for zero or negative values in `WeightValuePath` property
2. Verify numeric data types (use double/decimal for precision)
3. Consider data scale and normalization

### Remote Data Not Loading

**Problem:** Remote data fails to load or displays errors.

**Solutions:**
1. Check CORS configuration on API
2. Verify API endpoint URL
3. Ensure correct Adaptor is used
4. Check network tab for HTTP errors
5. Validate response data format

```cshtml
@code {
    // Add error handling
    private string ErrorMessage { get; set; }
    
    protected override async Task OnInitializedAsync()
    {
        try
        {
            // Your data loading code
        }
        catch (HttpRequestException ex)
        {
            ErrorMessage = $"Network error: {ex.Message}";
        }
        catch (Exception ex)
        {
            ErrorMessage = $"Error: {ex.Message}";
        }
    }
}
```

### Hierarchical Data Not Grouping

**Problem:** Hierarchical data displays flat without grouping.

**Solutions:**
1. Add `TreeMapLevels` configuration
2. Specify `GroupPath` for each level
3. Verify data structure matches expected hierarchy
4. Check collection property names

## Performance Considerations

### Large Datasets

For datasets with thousands of items:

1. **Implement pagination** for remote data
2. **Use virtual scrolling** if available
3. **Consider data aggregation** before binding
4. **Limit initial display** and provide drill-down

```cshtml
@code {
    private const int MaxInitialItems = 100;
    
    private List<GDPReport> GetOptimizedData()
    {
        return AllData
            .OrderByDescending(x => x.GDP)
            .Take(MaxInitialItems)
            .ToList();
    }
}
```

### Memory Management

1. **Dispose of HttpClient** properly
2. **Clear unused data** from memory
3. **Use pagination** for remote sources
4. **Implement lazy loading** for hierarchical data

### Rendering Optimization

1. **Avoid frequent data rebinding**
2. **Use `ShouldRender`** to control updates
3. **Batch data updates** when possible
4. **Cache transformed data**

```cshtml
@code {
    private List<GDPReport> _cachedData;
    
    protected override bool ShouldRender()
    {
        // Control rendering based on data changes
        return _dataChanged;
    }
}
```

### Best Practices Summary

1. ✅ Use strongly-typed collections
2. ✅ Handle null/empty data gracefully
3. ✅ Implement loading indicators
4. ✅ Add error boundaries
5. ✅ Validate data before binding
6. ✅ Use appropriate adaptor for remote data
7. ✅ Consider performance for large datasets
8. ✅ Cache frequently accessed data
9. ✅ Test with various data sizes
10. ✅ Document data structure requirements
