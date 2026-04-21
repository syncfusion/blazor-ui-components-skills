# Data Binding in 3D Charts

Complete guide to binding data to Syncfusion Blazor 3D Chart component using various data sources: IEnumerable lists, ExpandoObject, DynamicObject, remote data with SfDataManager, and handling empty points.

## Table of Contents
- [Overview](#overview)
- [List Binding (IEnumerable)](#list-binding-ienumerable)
- [ExpandoObject Binding](#expandoobject-binding)
- [DynamicObject Binding](#dynamicobject-binding)
- [Remote Data Binding](#remote-data-binding)
- [Empty Points Handling](#empty-points-handling)
- [Performance Optimization](#performance-optimization)

## Overview

The Blazor 3D Chart component supports multiple data binding methods through the `DataSource` property. The chart uses SfDataManager for advanced data operations and supports both client-side and remote data binding.

**Supported Data Sources:**
- IEnumerable collections (List, Array, etc.)
- Expando objects (dynamic properties at runtime)
- Dynamic objects (custom dynamic behavior)
- Remote data via REST APIs
- OData v4 services
- Web API endpoints

**Key Properties:**
- **DataSource**: Data collection to bind
- **XName**: Property name for X-axis values (case-sensitive)
- **YName**: Property name for Y-axis values (case-sensitive)

## List Binding (IEnumerable)

The most common data binding method using strongly-typed lists or collections.

### Basic List Binding

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales Report" 
           WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData" 
                       XName="Month" 
                       YName="Sales" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class SalesInfo
    {
        public string Month { get; set; }
        public double Sales { get; set; }
    }

    public List<SalesInfo> SalesData = new List<SalesInfo>
    {
        new SalesInfo { Month = "Jan", Sales = 35 },
        new SalesInfo { Month = "Feb", Sales = 28 },
        new SalesInfo { Month = "Mar", Sales = 34 },
        new SalesInfo { Month = "Apr", Sales = 32 },
        new SalesInfo { Month = "May", Sales = 40 },
        new SalesInfo { Month = "Jun", Sales = 32 },
        new SalesInfo { Month = "Jul", Sales = 35 }
    };
}
```

### DateTime Data Binding

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Inflation - Consumer Price" 
           Width="100%" 
           WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <Chart3DPrimaryXAxis LabelFormat="yyyy" 
                         ValueType="Syncfusion.Blazor.Chart3D.ValueType.DateTime" 
                         LabelRotationAngle="-45" 
                         LabelPlacement="Syncfusion.Blazor.Chart3D.LabelPlacement.BetweenTicks">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ConsumerReports" 
                       XName="Date" 
                       YName="Value" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class TimeSeriesData
    {
        public DateTime Date { get; set; }
        public double Value { get; set; }
    }

    public List<TimeSeriesData> ConsumerReports = new List<TimeSeriesData>
    {
        new TimeSeriesData { Date = new DateTime(2005, 01, 01), Value = 21 },
        new TimeSeriesData { Date = new DateTime(2006, 01, 01), Value = 24 },
        new TimeSeriesData { Date = new DateTime(2007, 01, 01), Value = 36 },
        new TimeSeriesData { Date = new DateTime(2008, 01, 01), Value = 38 },
        new TimeSeriesData { Date = new DateTime(2009, 01, 01), Value = 54 },
        new TimeSeriesData { Date = new DateTime(2010, 01, 01), Value = 57 },
        new TimeSeriesData { Date = new DateTime(2011, 01, 01), Value = 70 }
    };
}
```

### Multiple Series Binding

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Comparative Analysis" 
           WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ProductData" 
                       Name="Product A" 
                       XName="Category" 
                       YName="ProductA" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@ProductData" 
                       Name="Product B" 
                       XName="Category" 
                       YName="ProductB" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@ProductData" 
                       Name="Product C" 
                       XName="Category" 
                       YName="ProductC" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class MultiSeriesData
    {
        public string Category { get; set; }
        public double ProductA { get; set; }
        public double ProductB { get; set; }
        public double ProductC { get; set; }
    }

    public List<MultiSeriesData> ProductData = new List<MultiSeriesData>
    {
        new MultiSeriesData { Category = "Q1", ProductA = 45, ProductB = 38, ProductC = 52 },
        new MultiSeriesData { Category = "Q2", ProductA = 50, ProductB = 42, ProductC = 48 },
        new MultiSeriesData { Category = "Q3", ProductA = 42, ProductB = 45, ProductC = 55 },
        new MultiSeriesData { Category = "Q4", ProductA = 55, ProductB = 48, ProductC = 60 }
    };
}
```

### Async Data Loading

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Async Data Loading" 
           WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@ChartData" 
                       XName="Category" 
                       YName="Value" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class ChartDataPoint
    {
        public string Category { get; set; }
        public double Value { get; set; }
    }

    public List<ChartDataPoint> ChartData { get; set; }

    protected override async Task OnInitializedAsync()
    {
        // Simulate async data fetch
        await Task.Delay(1000); // Replace with actual API call
        
        ChartData = await FetchDataAsync();
    }

    private async Task<List<ChartDataPoint>> FetchDataAsync()
    {
        // Simulate API call
        await Task.Delay(500);
        
        return new List<ChartDataPoint>
        {
            new ChartDataPoint { Category = "A", Value = 35 },
            new ChartDataPoint { Category = "B", Value = 28 },
            new ChartDataPoint { Category = "C", Value = 34 },
            new ChartDataPoint { Category = "D", Value = 32 },
            new ChartDataPoint { Category = "E", Value = 40 }
        };
    }
}
```

## ExpandoObject Binding

Use ExpandoObject for dynamic data structures when property names are unknown at compile time.

### Basic ExpandoObject Binding

```razor
@using Syncfusion.Blazor.Chart3D
@using System.Dynamic

<SfChart3D Title="Dynamic Data Visualization" 
           WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category" 
                         LabelRotationAngle="-45" 
                         LabelPlacement="Syncfusion.Blazor.Chart3D.LabelPlacement.BetweenTicks">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@DynamicData" 
                       XName="X" 
                       YName="Y" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    private List<string> Countries = new List<string> 
    { 
        "South Korea", "India", "Germany", "Italy", "Russia" 
    };
    
    private Random RandomGenerator = new Random();
    
    public List<ExpandoObject> DynamicData { get; set; } = new List<ExpandoObject>();

    protected override void OnInitialized()
    {
        DynamicData = Enumerable.Range(0, 5).Select((index) =>
        {
            dynamic dataPoint = new ExpandoObject();
            dataPoint.X = Countries[index];
            dataPoint.Y = RandomGenerator.Next(20, 80);
            return dataPoint;
        }).Cast<ExpandoObject>().ToList<ExpandoObject>();
    }
}
```

### Multi-Property ExpandoObject

```razor
@using Syncfusion.Blazor.Chart3D
@using System.Dynamic

<SfChart3D WallColor="transparent" EnableRotation="true" RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    
    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@MultiDynamicData" Name="Series 1" XName="Category" YName="Value1" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
        <Chart3DSeries DataSource="@MultiDynamicData" Name="Series 2" XName="Category" YName="Value2" Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public List<ExpandoObject> MultiDynamicData { get; set; } = new List<ExpandoObject>();
    private Random random = new Random();

    protected override void OnInitialized()
    {
        var categories = new[] { "Q1", "Q2", "Q3", "Q4" };
        
        MultiDynamicData = categories.Select(cat =>
        {
            dynamic dp = new ExpandoObject();
            dp.Category = cat;
            dp.Value1 = random.Next(20, 60);
            dp.Value2 = random.Next(30, 70);
            return dp;
        }).Cast<ExpandoObject>().ToList();
    }
}
```

## DynamicObject Binding

For advanced scenarios requiring custom dynamic behavior beyond ExpandoObject capabilities.

### Custom DynamicObject Implementation

```razor
@using Syncfusion.Blazor.Chart3D
@using System.Dynamic

<SfChart3D Title="Custom Dynamic Object" 
           WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.DateTime" 
                         LabelRotationAngle="-45" 
                         LabelPlacement="Syncfusion.Blazor.Chart3D.LabelPlacement.BetweenTicks">
    </Chart3DPrimaryXAxis>
    
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@CustomDynamicData" 
                       XName="X" 
                       YName="Y" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    private List<DateTime> Dates = new List<DateTime> 
    { 
        new DateTime(2005, 01, 01), new DateTime(2006, 01, 01), 
        new DateTime(2007, 01, 01), new DateTime(2008, 01, 01), 
        new DateTime(2009, 01, 01), new DateTime(2010, 01, 01), 
        new DateTime(2011, 01, 01) 
    };
    
    private Random RandomGenerator = new Random();
    
    public List<DynamicDictionary> CustomDynamicData = new List<DynamicDictionary>();

    protected override void OnInitialized()
    {
        CustomDynamicData = Enumerable.Range(0, 7).Select((index) =>
        {
            dynamic dataPoint = new DynamicDictionary();
            dataPoint.X = Dates[index];
            dataPoint.Y = RandomGenerator.Next(20, 80);
            return dataPoint;
        }).Cast<DynamicDictionary>().ToList<DynamicDictionary>();
    }

    public class DynamicDictionary : DynamicObject
    {
        Dictionary<string, object> dictionary = new Dictionary<string, object>();

        public override bool TryGetMember(GetMemberBinder binder, out object result)
        {
            string name = binder.Name;
            return dictionary.TryGetValue(name, out result);
        }

        public override bool TrySetMember(SetMemberBinder binder, object value)
        {
            dictionary[binder.Name] = value;
            return true;
        }

        public override IEnumerable<string> GetDynamicMemberNames()
        {
            return this.dictionary?.Keys;
        }
    }
}
```

## Remote Data Binding

Bind 3D charts to remote data sources using SfDataManager with various adaptors.

### WebApiAdaptor

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Remote Data - Web API" 
           WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <!-- Use a server-side endpoint or proxy instead of binding directly to a public third-party URL. -->
    <SfDataManager Url="/api/chart" 
                   Adaptor="Adaptors.WebApiAdaptor">
    </SfDataManager>

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category" 
                         LabelRotationAngle="-45" 
                         LabelPlacement="Syncfusion.Blazor.Chart3D.LabelPlacement.BetweenTicks">
    </Chart3DPrimaryXAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries XName="FoodName" 
                       YName="Price" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>
```

### ODataV4Adaptor

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="OData v4 Service" 
           WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <!-- Prefer a server-side OData proxy endpoint to avoid direct public OData bindings in production. -->
    <SfDataManager Url="/api/odata/Orders" 
                   Adaptor="Adaptors.ODataV4Adaptor">
    </SfDataManager>

    <Chart3DPrimaryXAxis Title="Orders" 
                         ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category" 
                         LabelRotationAngle="-45" 
                         LabelPlacement="Syncfusion.Blazor.Chart3D.LabelPlacement.BetweenTicks">
    </Chart3DPrimaryXAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries XName="OrderID" 
                       YName="Freight" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>
```

### Custom Query with Parameters

```razor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Filtered Data" 
           WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <!-- Use a server-side filtered OData endpoint or proxy. Keep query logic server-side when possible. -->
    <SfDataManager Url="/api/odata/Orders" 
                   Adaptor="Adaptors.ODataV4Adaptor">
    </SfDataManager>

    <Chart3DPrimaryXAxis Title="Orders" 
                         ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category" 
                         LabelRotationAngle="-45" 
                         LabelPlacement="Syncfusion.Blazor.Chart3D.LabelPlacement.BetweenTicks">
    </Chart3DPrimaryXAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries Query="@CustomQuery" 
                       XName="OrderID" 
                       YName="Freight" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public Query CustomQuery { get; set; }

    protected override void OnInitialized()
    {
        // Take top 10 orders where Freight > 300
        CustomQuery = new Query()
            .Take(10)
            .Where("Freight", "GreaterThan", 300, false);
    }
}
```

### Custom Web API Example

```razor
@using Syncfusion.Blazor.Data
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Custom API Data" WallColor="transparent" EnableRotation="true" 
           RotationAngle="7" TiltAngle="10" Depth="100">
    
    <!-- Point the DataManager to a secured server endpoint. Do not hardcode public URLs or secrets in client code. -->
    <SfDataManager Url="/api/sales" 
                   Adaptor="Adaptors.WebApiAdaptor">
    </SfDataManager>

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries Query="@ApiQuery" 
                       XName="ProductName" 
                       YName="TotalSales" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public Query ApiQuery { get; set; }

    protected override void OnInitialized()
    {
        ApiQuery = new Query()
            .AddParams("year", "2024")
            .AddParams("region", "North")
            .Take(20)
            .Sort("TotalSales", "descending");
    }
}
```

---

## Security Considerations

When using remote data with `SfDataManager`, avoid binding directly to public third-party endpoints in client code. Use the following best practices:

- **Server-side proxy**: Create a server-side API (relative paths like `/api/...`) that forwards requests to external services. This avoids exposing third-party URLs or credentials in client code.
- **Keep secrets server-side**: Store API keys, tokens, and credentials on the server (e.g., environment variables) and never embed them in the Blazor client.
- **Use HTTPS/TLS**: Ensure all endpoints use HTTPS and valid certificates.
- **CORS and allowed origins**: Configure CORS on your server to allow only trusted origins; avoid enabling permissive CORS in production.
- **Authentication & authorization**: Protect data endpoints with proper auth (JWT, cookie auth, OAuth) and enforce authorization checks server-side.
- **Validate and sanitize data**: Validate responses from external services on the server before returning them to the client.
- **Rate limiting & caching**: Implement rate limiting and caching on the server to protect downstream APIs and improve performance.
- **Environment-specific URLs**: Use configuration (appsettings, environment variables) to set endpoint URLs per environment rather than hardcoding.

Example recommended flow:

1. Blazor client requests `/api/chart` on your server.
2. Your server authenticates the request, adds any necessary API keys, and forwards a request to the third-party service.
3. Server sanitizes and returns the minimal required data to the client.

This pattern prevents leaking third-party endpoints and credentials, reduces CORS exposure, and centralizes security controls.

## Empty Points Handling

Handle null, undefined, or missing data points with Chart3DEmptyPointSettings.

### Gap Mode (Default)

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales with Missing Data" 
           WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category" 
                         LabelRotationAngle="-45" 
                         LabelPlacement="Syncfusion.Blazor.Chart3D.LabelPlacement.BetweenTicks">
    </Chart3DPrimaryXAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesReports" 
                       XName="Month" 
                       YName="Sales" 
                       Type="Chart3DSeriesType.Column">
            <Chart3DEmptyPointSettings Mode="Syncfusion.Blazor.Chart3D.EmptyPointMode.Gap">
            </Chart3DEmptyPointSettings>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class SalesData
    {
        public string Month { get; set; }
        public double? Sales { get; set; }  // Nullable double for missing values
    }

    public List<SalesData> SalesReports = new List<SalesData>
    {
        new SalesData { Month = "Jan", Sales = 35 },
        new SalesData { Month = "Feb", Sales = 28 },
        new SalesData { Month = "Mar", Sales = null },  // Missing data
        new SalesData { Month = "Apr", Sales = 32 },
        new SalesData { Month = "May", Sales = 40 },
        new SalesData { Month = "Jun", Sales = null },  // Missing data
        new SalesData { Month = "Jul", Sales = 35 }
    };
}
```

### Average Mode

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales with Averaged Missing Data"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="7"
           TiltAngle="10"
           Depth="100">

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"
                         LabelRotationAngle="-45"
                         LabelPlacement="Syncfusion.Blazor.Chart3D.LabelPlacement.BetweenTicks">
    </Chart3DPrimaryXAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesReports"
                       XName="Month"
                       YName="Sales"
                       Type="Chart3DSeriesType.Column">
            <Chart3DEmptyPointSettings Mode="Syncfusion.Blazor.Chart3D.EmptyPointMode.Average"
                                       Fill="lightblue">
            </Chart3DEmptyPointSettings>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class SalesData
    {
        public string Month { get; set; }
        public double? Sales { get; set; }  // nullable for missing points
    }

    public List<SalesData> SalesReports = new List<SalesData>
    {
        new SalesData { Month = "Jan", Sales = 42 },
        new SalesData { Month = "Feb", Sales = 38 },
        new SalesData { Month = "Mar", Sales = null },   // Missing → will be averaged
        new SalesData { Month = "Apr", Sales = 47 },
        new SalesData { Month = "May", Sales = 53 },
        new SalesData { Month = "Jun", Sales = null },   // Missing → will be averaged
        new SalesData { Month = "Jul", Sales = 50 }
    };
}
```

### Zero Mode

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales with Zero-Handled Missing Data"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="7"
           TiltAngle="10"
           Depth="100">

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"
                         LabelRotationAngle="-45"
                         LabelPlacement="Syncfusion.Blazor.Chart3D.LabelPlacement.BetweenTicks">
    </Chart3DPrimaryXAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesReports"
                       XName="Month"
                       YName="Sales"
                       Type="Chart3DSeriesType.Column">
            <Chart3DEmptyPointSettings Mode="Syncfusion.Blazor.Chart3D.EmptyPointMode.Zero"
                                       Fill="lightgray">
            </Chart3DEmptyPointSettings>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class SalesData
    {
        public string Month { get; set; }
        public double? Sales { get; set; }  // Nullable → required for empty points
    }

    public List<SalesData> SalesReports = new List<SalesData>
    {
        new SalesData { Month = "Jan", Sales = 45 },
        new SalesData { Month = "Feb", Sales = 52 },
        new SalesData { Month = "Mar", Sales = null },   // Missing → will be shown as ZERO
        new SalesData { Month = "Apr", Sales = 60 },
        new SalesData { Month = "May", Sales = null },   // Missing → will be shown as ZERO
        new SalesData { Month = "Jun", Sales = 58 },
        new SalesData { Month = "Jul", Sales = 63 }
    };
}
```

### Custom Empty Point Color

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D WallColor="transparent" EnableRotation="true" RotationAngle="7" TiltAngle="10" Depth="100">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@DataWithNulls" XName="Month" YName="Value" Type="Chart3DSeriesType.Column">
            <Chart3DEmptyPointSettings Mode="Syncfusion.Blazor.Chart3D.EmptyPointMode.Average" 
                                       Fill="#FF6347">
            </Chart3DEmptyPointSettings>
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class ChartData
    {
        public string Month { get; set; }
        public double? Value { get; set; }
    }

    public List<ChartData> DataWithNulls = new List<ChartData>
    {
        new ChartData { Month = "Jan", Value = 35 },
        new ChartData { Month = "Feb", Value = 28 },
        new ChartData { Month = "Mar", Value = double.NaN },  // Using NaN for empty
        new ChartData { Month = "Apr", Value = 32 },
        new ChartData { Month = "May", Value = 40 },
        new ChartData { Month = "Jun", Value = 32 },
        new ChartData { Month = "Jul", Value = 35 },
        new ChartData { Month = "Aug", Value = double.NaN },
        new ChartData { Month = "Sep", Value = 38 }
    };
}
```

**Empty Point Modes:**
- **Gap**: Leave space for missing data (default)
- **Zero**: Treat missing values as zero
- **Average**: Calculate average of adjacent points
- **Drop**: Remove empty points entirely

## Performance Optimization

### Best Practices for Large Datasets

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D EnableRotation="false"
           WallColor="transparent" 
           RotationAngle="0" 
           TiltAngle="10" 
           Depth="80">
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@PaginatedData" 
                       XName="Category" 
                       YName="Value" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

<button @onclick="LoadMoreData">Load More</button>

@code {
    public class DataPoint
    {
        public string Category { get; set; }
        public double Value { get; set; }
    }

    private List<DataPoint> AllData = new List<DataPoint>();
    public List<DataPoint> PaginatedData { get; set; } = new List<DataPoint>();
    private int pageSize = 20;
    private int currentPage = 0;

    protected override void OnInitialized()
    {
        // Initialize large dataset
        AllData = Enumerable.Range(1, 1000).Select(i => new DataPoint
        {
            Category = $"Item {i}",
            Value = new Random().Next(10, 100)
        }).ToList();

        LoadMoreData();
    }

    private void LoadMoreData()
    {
        var nextBatch = AllData.Skip(currentPage * pageSize).Take(pageSize).ToList();
        PaginatedData.AddRange(nextBatch);
        currentPage++;
        StateHasChanged();
    }
}
```

### Data Update Strategy

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D EnableRotation="false"
           WallColor="transparent" 
           RotationAngle="0" 
           TiltAngle="10" 
           Depth="80">
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@LiveData" 
                       XName="Category" 
                       YName="Value" 
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>
@code {
    private Timer dataUpdateTimer;
    public List<RealTimeData> LiveData { get; set; } = new List<RealTimeData>();

    protected override void OnInitialized()
    {
        // Initialize with initial data
        LiveData = GetInitialData();
        
        // Set up timer for real-time updates
        dataUpdateTimer = new Timer(UpdateData, null, TimeSpan.Zero, TimeSpan.FromSeconds(5));
    }

    private void UpdateData(object state)
    {
        InvokeAsync(() =>
        {
            // Update only changed data points
            var updatedData = GetLatestData();
            LiveData = updatedData;
            StateHasChanged();
        });
    }

    public void Dispose()
    {
        dataUpdateTimer?.Dispose();
    }
}
```

**Performance Tips:**
1. **Limit data points**: Keep data under 1000 points for 3D charts
2. **Disable rotation**: Set `EnableRotation="false"` for static charts
3. **Reduce depth**: Lower `Depth` value (50-100) for faster rendering
4. **Use pagination**: Load data in batches
5. **Optimize updates**: Update only changed data, not entire dataset
6. **Debounce updates**: Don't update more than once per second
7. **Use SfDataManager**: For server-side filtering and paging

This comprehensive guide covers all data binding scenarios for Blazor 3D Charts. Choose the appropriate method based on your data source and requirements!
