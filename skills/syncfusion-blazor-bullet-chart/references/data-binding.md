# Data Binding

This guide covers all data binding patterns for the Syncfusion Blazor Bullet Chart, from simple single-value scenarios to complex remote data sources with dynamic updates.

## Table of Contents
- [Single Data Point Binding](#single-data-point-binding)
- [Multiple Data Points with CategoryField](#multiple-data-points-with-categoryfield)
- [Local Data Sources](#local-data-sources)
- [Remote Data with SfDataManager](#remote-data-with-sfdatamanager)
- [Dynamic Data Updates](#dynamic-data-updates)

## Single Data Point Binding

The simplest scenario binds one data point representing a single KPI or metric.

### Basic Single Value

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@RevenueData" 
               ValueField="ActualRevenue" 
               TargetField="TargetRevenue" 
               Minimum="0" 
               Maximum="300" 
               Interval="50"
               Title="Q1 Revenue (in thousands $)">
    <BulletChartRangeCollection>
        <BulletChartRange End="150" Color="#d32f2f"></BulletChartRange>
        <BulletChartRange End="250" Color="#ffa726"></BulletChartRange>
        <BulletChartRange End="300" Color="#66bb6a"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class RevenueMetric
    {
        public double ActualRevenue { get; set; }
        public double TargetRevenue { get; set; }
    }
    
    public List<RevenueMetric> RevenueData = new List<RevenueMetric>
    {
        new RevenueMetric { ActualRevenue = 270, TargetRevenue = 250 }
    };
}
```

**Result:** A horizontal bullet chart showing actual revenue ($270K) vs target ($250K).

### Single Value with Multiple Targets

Track one metric against multiple benchmark values:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@PerformanceData" 
               ValueField="CurrentScore" 
               TargetField="MinimumTarget,ExpectedTarget,StretchTarget" 
               TargetTypes="new List<TargetType> { TargetType.Rect, TargetType.Circle, TargetType.Cross }"
               Minimum="0" 
               Maximum="100" 
               Interval="20"
               Title="Employee Performance Score">
    <BulletChartRangeCollection>
        <BulletChartRange End="40" Color="#d32f2f"></BulletChartRange>
        <BulletChartRange End="70" Color="#ffa726"></BulletChartRange>
        <BulletChartRange End="100" Color="#66bb6a"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class PerformanceMetric
    {
        public double CurrentScore { get; set; }
        public double MinimumTarget { get; set; }
        public double ExpectedTarget { get; set; }
        public double StretchTarget { get; set; }
    }
    
    public List<PerformanceMetric> PerformanceData = new List<PerformanceMetric>
    {
        new PerformanceMetric 
        { 
            CurrentScore = 78, 
            MinimumTarget = 60,
            ExpectedTarget = 75,
            StretchTarget = 90
        }
    };
}
```

## Multiple Data Points with CategoryField

Display multiple KPIs or categories using the `CategoryField` property. This creates a vertical layout with category labels.

### Multiple Categories Example

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@SalesData" 
               ValueField="ActualSales" 
               TargetField="TargetSales" 
               CategoryField="ProductName"
               Height="400"
               Minimum="0" 
               Maximum="100" 
               Interval="20"
               Title="Product Sales Performance">
    <BulletChartMinorTickLines Width="0"></BulletChartMinorTickLines>
    <BulletChartRangeCollection>
        <BulletChartRange End="35" Color="#d32f2f"></BulletChartRange>
        <BulletChartRange End="70" Color="#ffa726"></BulletChartRange>
        <BulletChartRange End="100" Color="#66bb6a"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class SalesMetric
    {
        public double ActualSales { get; set; }
        public double TargetSales { get; set; }
        public string ProductName { get; set; }
    }
    
    public List<SalesMetric> SalesData = new List<SalesMetric>
    {
        new SalesMetric { ActualSales = 85, TargetSales = 75, ProductName = "Product A" },
        new SalesMetric { ActualSales = 60, TargetSales = 70, ProductName = "Product B" },
        new SalesMetric { ActualSales = 95, TargetSales = 80, ProductName = "Product C" },
        new SalesMetric { ActualSales = 45, TargetSales = 65, ProductName = "Product D" }
    };
}
```

**Result:** Four horizontal bullet charts stacked vertically, each labeled with the product name.

### Time-Series Data with Categories

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@MonthlyRevenue" 
               ValueField="Revenue" 
               TargetField="Quota" 
               CategoryField="Month"
               Height="500"
               Minimum="0" 
               Maximum="500" 
               Interval="100"
               Title="2026 Monthly Revenue Performance">
    <BulletChartRangeCollection>
        <BulletChartRange End="200" Color="#ffebee"></BulletChartRange>
        <BulletChartRange End="350" Color="#fff3e0"></BulletChartRange>
        <BulletChartRange End="500" Color="#e8f5e9"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class MonthlyMetric
    {
        public double Revenue { get; set; }
        public double Quota { get; set; }
        public string Month { get; set; }
    }
    
    public List<MonthlyMetric> MonthlyRevenue = new List<MonthlyMetric>
    {
        new MonthlyMetric { Revenue = 380, Quota = 350, Month = "Jan" },
        new MonthlyMetric { Revenue = 420, Quota = 400, Month = "Feb" },
        new MonthlyMetric { Revenue = 310, Quota = 350, Month = "Mar" },
        new MonthlyMetric { Revenue = 450, Quota = 400, Month = "Apr" },
        new MonthlyMetric { Revenue = 390, Quota = 400, Month = "May" },
        new MonthlyMetric { Revenue = 480, Quota = 450, Month = "Jun" }
    };
}
```

## Local Data Sources

### List<T> Data Source

The most common pattern using strongly-typed lists:

```razor
@using Syncfusion.Blazor.Charts

<!-- Bind to the chart -->
<SfBulletChart DataSource="@DepartmentKPIs" 
               ValueField="Performance" 
               TargetField="Target" 
               CategoryField="Department"
               Height="400"
               Minimum="0" 
               Maximum="100">
    <BulletChartRangeCollection>
        <BulletChartRange End="50"></BulletChartRange>
        <BulletChartRange End="75"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class KPIMetric
    {
        public string Department { get; set; }
        public double Performance { get; set; }
        public double Target { get; set; }
        public string Status { get; set; }
    }
    
    public List<KPIMetric> DepartmentKPIs = new List<KPIMetric>
    {
        new KPIMetric { Department = "Sales", Performance = 92, Target = 85, Status = "Excellent" },
        new KPIMetric { Department = "Marketing", Performance = 78, Target = 80, Status = "Good" },
        new KPIMetric { Department = "Support", Performance = 88, Target = 90, Status = "Good" },
        new KPIMetric { Department = "R&D", Performance = 65, Target = 75, Status = "Below Target" }
    };
}
```

### Array Data Source

Use arrays for simple numeric data:

```razor
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@ArrayData" 
               ValueField="Values" 
               TargetField="Target" 
               CategoryField="Label"
               Height="300">
    <BulletChartRangeCollection>
        <BulletChartRange End="60"></BulletChartRange>
        <BulletChartRange End="80"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class SimpleMetric
    {
        public double[] Values { get; set; }
        public double Target { get; set; }
        public string Label { get; set; }
    }
    
    public List<SimpleMetric> ArrayData = new List<SimpleMetric>
    {
        new SimpleMetric { Values = new double[] { 75 }, Target = 80, Label = "Q1" },
        new SimpleMetric { Values = new double[] { 85 }, Target = 80, Label = "Q2" },
        new SimpleMetric { Values = new double[] { 92 }, Target = 85, Label = "Q3" }
    };
}
```

### ExpandoObject for Dynamic Properties

Use `ExpandoObject` when property names are determined at runtime:

```razor
@using System.Dynamic
@using Syncfusion.Blazor.Charts

<SfBulletChart DataSource="@DynamicData" 
               ValueField="Current" 
               TargetField="Goal" 
               CategoryField="Category"
               Height="350"
               Minimum="0" 
               Maximum="100">
    <BulletChartRangeCollection>
        <BulletChartRange End="40"></BulletChartRange>
        <BulletChartRange End="70"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public List<ExpandoObject> DynamicData { get; set; } = new();

    protected override void OnInitialized()
    {
        DynamicData = new List<ExpandoObject>();
        
        dynamic item1 = new ExpandoObject();
        item1.Category = "Customer Satisfaction";
        item1.Current = 88.0;
        item1.Goal = 85.0;
        DynamicData.Add(item1);
        
        dynamic item2 = new ExpandoObject();
        item2.Category = "On-Time Delivery";
        item2.Current = 92.0;
        item2.Goal = 95.0;
        DynamicData.Add(item2);
        
        dynamic item3 = new ExpandoObject();
        item3.Category = "Quality Score";
        item3.Current = 78.0;
        item3.Goal = 80.0;
        DynamicData.Add(item3);
    }
}
```

## Remote Data with SfDataManager

Fetch data from REST APIs, OData services, or remote endpoints using `SfDataManager`.

### Security Considerations for Remote Data

When binding to external or remote data sources, avoid fetching arbitrary third-party URLs or accepting untrusted endpoint values at runtime. Pulling remote content directly into the client can expose your application to data poisoning, excessive network requests, information disclosure, and other risks. Use these safe practices:

- Validate and allow-list endpoints: only call known, trusted hosts and endpoints.
- Prefer server-side proxies or endpoints you control rather than calling third-party URLs directly from client code.
- Use `IHttpClientFactory` (DI) and configure timeouts, retry limits, and headers.
- Deserialize only into well-defined DTO types; avoid dynamic parsing of untrusted JSON.
- Sanitize and validate any fields returned from remote services before use in the UI.
- Log and monitor remote fetches; fail closed on validation errors.

Follow these patterns in examples below instead of directly using untrusted `dm.Url` values.

### Basic Remote Data Binding

```razor
@using Syncfusion.Blazor.Charts
@using Syncfusion.Blazor.Data

<SfBulletChart TValue="SalesMetric" 
               ValueField="Sales" 
               TargetField="Target" 
               CategoryField="Region"
               Height="400"
               Minimum="0" 
               Maximum="500">
    <!-- Use a server-side endpoint or proxy you control instead of a public third-party URL. -->
    <SfDataManager Url="/api/sales-data" Adaptor="Adaptors.WebApiAdaptor"></SfDataManager>
    <BulletChartRangeCollection>
        <BulletChartRange End="200"></BulletChartRange>
        <BulletChartRange End="350"></BulletChartRange>
        <BulletChartRange End="500"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class SalesMetric
    {
        public string Region { get; set; }
        public double Sales { get; set; }
        public double Target { get; set; }
    }
}
```

### OData Service Binding

```razor
@using Syncfusion.Blazor.Charts
@using Syncfusion.Blazor.Data

<SfBulletChart TValue="PerformanceData" 
               ValueField="ActualValue" 
               TargetField="TargetValue" 
               CategoryField="Metric"
               Height="450">
    <!-- Do not call public third-party OData services directly from client code; use a server-side proxy or allow-listed endpoint. -->
    <SfDataManager Url="/api/odata/Products" 
                   Adaptor="Adaptors.ODataV4Adaptor"></SfDataManager>
    <BulletChartRangeCollection>
        <BulletChartRange End="100"></BulletChartRange>
        <BulletChartRange End="200"></BulletChartRange>
        <BulletChartRange End="300"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class PerformanceData
    {
        public string Metric { get; set; }
        public double ActualValue { get; set; }
        public double TargetValue { get; set; }
    }
}
```

### Custom Adaptor for Complex APIs

```razor
@using Syncfusion.Blazor.Charts
@using Syncfusion.Blazor.Data

<SfBulletChart TValue="KPIData" 
               ValueField="Value" 
               TargetField="Benchmark" 
               CategoryField="KPI"
               Height="400">
    <!-- Use a fixed, server-side endpoint or an allow-listed service here. Avoid accepting arbitrary URLs from untrusted sources. -->
    <SfDataManager AdaptorInstance="@typeof(CustomDataAdaptor)" 
                   Url="/api/kpi-dashboard"></SfDataManager>
    <BulletChartRangeCollection>
        <BulletChartRange End="60"></BulletChartRange>
        <BulletChartRange End="80"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class KPIData
    {
        public string KPI { get; set; }
        public double Value { get; set; }
        public double Benchmark { get; set; }
    }

    // Secure CustomDataAdaptor example:
    // - Validates the request URL against an allow-list
    // - Uses IHttpClientFactory (via DI) rather than creating raw HttpClient instances
    // - Deserializes into a known DTO type only
    // - Handles errors and timeouts safely
    public class CustomDataAdaptor : DataAdaptor
    {
        private static readonly string[] AllowedHosts = new[] { "api.mycompany.com" };
        private readonly IHttpClientFactory _httpFactory;

        public CustomDataAdaptor() : this(null) { }

        // For Blazor usage, register and inject IHttpClientFactory in your component or service and pass it in
        public CustomDataAdaptor(IHttpClientFactory httpFactory)
        {
            _httpFactory = httpFactory ?? new DefaultHttpClientFactory();
        }

        public override async Task<object> ReadAsync(DataManagerRequest dm, string key = null)
        {
            // Validate dm.Url to prevent fetching arbitrary third-party resources.
            // Allow only relative (same-origin) URLs or absolute URLs that match the allow-list.
            if (string.IsNullOrWhiteSpace(dm.Url))
                return null;

            if (Uri.TryCreate(dm.Url, UriKind.Absolute, out var uri))
            {
                if (!AllowedHosts.Contains(uri.Host, StringComparer.OrdinalIgnoreCase))
                    return null; // Reject requests to non-allow-listed hosts
            }
            else
            {
                // dm.Url is relative (e.g., "/api/...") — allow, assumes same-origin HttpClient configuration
            }

            var client = _httpFactory.CreateClient();
            client.Timeout = TimeSpan.FromSeconds(10);

            try
            {
                using var response = await client.GetAsync(uri);
                if (!response.IsSuccessStatusCode)
                    return null;

                // Parse into known DTO list only
                var data = await response.Content.ReadFromJsonAsync<List<KPIData>>();
                if (data == null)
                    return null;

                return dm.RequiresCounts ? new DataResult() { Result = data, Count = data.Count } : (object)data;
            }
            catch (OperationCanceledException) // timeout
            {
                return null;
            }
            catch (Exception)
            {
                // Log exception in real application
                return null;
            }
        }
    }

    // Minimal default factory for examples; in real apps register IHttpClientFactory in DI container
    internal class DefaultHttpClientFactory : IHttpClientFactory
    {
        public HttpClient CreateClient(string name = null) => new HttpClient();
    }
}
```

## Dynamic Data Updates

Update bullet chart data in response to user actions or real-time events.

### Button-Triggered Update

```razor
@using Syncfusion.Blazor.Charts

<button class="btn btn-primary" @onclick="RefreshData">Refresh Data</button>

<SfBulletChart DataSource="@SalesMetrics" 
               ValueField="Current" 
               TargetField="Goal" 
               CategoryField="Month"
               Height="400"
               Minimum="0" 
               Maximum="500">
    <BulletChartRangeCollection>
        <BulletChartRange End="200"></BulletChartRange>
        <BulletChartRange End="350"></BulletChartRange>
        <BulletChartRange End="500"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class MonthlySales
    {
        public string Month { get; set; }
        public double Current { get; set; }
        public double Goal { get; set; }
    }
    
    public List<MonthlySales> SalesMetrics { get; set; } = new();
    
    protected override void OnInitialized()
    {
        LoadInitialData();
    }
    
    private void LoadInitialData()
    {
        SalesMetrics = new List<MonthlySales>
        {
            new MonthlySales { Month = "Jan", Current = 350, Goal = 400 },
            new MonthlySales { Month = "Feb", Current = 420, Goal = 400 },
            new MonthlySales { Month = "Mar", Current = 380, Goal = 400 }
        };
    }
    
    private void RefreshData()
    {
        // Simulate fetching new data
        Random random = new Random();
        SalesMetrics = new List<MonthlySales>
        {
            new MonthlySales { Month = "Jan", Current = random.Next(300, 500), Goal = 400 },
            new MonthlySales { Month = "Feb", Current = random.Next(300, 500), Goal = 400 },
            new MonthlySales { Month = "Mar", Current = random.Next(300, 500), Goal = 400 }
        };
        
        StateHasChanged();
    }
}
```

### Timer-Based Real-Time Updates

```razor
@using Syncfusion.Blazor.Charts
@using System.Timers
@implements IDisposable

<h3>Live Server Metrics</h3>
<p>Last Updated: @lastUpdateTime.ToString("HH:mm:ss")</p>

<SfBulletChart DataSource="@ServerMetrics" 
               ValueField="CurrentLoad" 
               TargetField="MaxCapacity" 
               CategoryField="ServerName"
               Height="400"
               Minimum="0" 
               Maximum="100">
    <BulletChartRangeCollection>
        <BulletChartRange End="50" Color="#66bb6a"></BulletChartRange>
        <BulletChartRange End="75" Color="#ffa726"></BulletChartRange>
        <BulletChartRange End="100" Color="#d32f2f"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    public class ServerMetric
    {
        public string ServerName { get; set; }
        public double CurrentLoad { get; set; }
        public double MaxCapacity { get; set; }
    }
    
    public List<ServerMetric> ServerMetrics { get; set; } = new();
    private Timer updateTimer;
    private DateTime lastUpdateTime = DateTime.Now;
    
    protected override void OnInitialized()
    {
        LoadServerData();
        
        updateTimer = new Timer(5000); // Update every 5 seconds
        updateTimer.Elapsed += async (sender, e) => await UpdateServerData();
        updateTimer.Start();
    }
    
    private void LoadServerData()
    {
        ServerMetrics = new List<ServerMetric>
        {
            new ServerMetric { ServerName = "Web-01", CurrentLoad = 45, MaxCapacity = 90 },
            new ServerMetric { ServerName = "Web-02", CurrentLoad = 62, MaxCapacity = 90 },
            new ServerMetric { ServerName = "DB-01", CurrentLoad = 78, MaxCapacity = 85 }
        };
    }
    
    private async Task UpdateServerData()
    {
        await InvokeAsync(() =>
        {
            Random random = new Random();
            foreach (var metric in ServerMetrics)
            {
                // Simulate load fluctuation
                metric.CurrentLoad = Math.Max(20, Math.Min(95, metric.CurrentLoad + random.Next(-10, 15)));
            }
            
            lastUpdateTime = DateTime.Now;
            StateHasChanged();
        });
    }
    
    public void Dispose()
    {
        updateTimer?.Dispose();
    }
}
```

### API Polling Pattern

```razor
@using Syncfusion.Blazor.Charts
@using System.Net.Http.Json

<button class="btn btn-success" @onclick="StartPolling" disabled="@isPolling">Start Monitoring</button>
<button class="btn btn-danger" @onclick="StopPolling" disabled="@(!isPolling)">Stop Monitoring</button>

<SfBulletChart DataSource="@LiveMetrics" 
               ValueField="Value" 
               TargetField="Target" 
               CategoryField="MetricName"
               Height="450">
    <BulletChartRangeCollection>
        <BulletChartRange End="40"></BulletChartRange>
        <BulletChartRange End="70"></BulletChartRange>
        <BulletChartRange End="100"></BulletChartRange>
    </BulletChartRangeCollection>
</SfBulletChart>

@code {
    [Inject] private HttpClient Http { get; set; }
    
    public class LiveMetric
    {
        public string MetricName { get; set; }
        public double Value { get; set; }
        public double Target { get; set; }
    }
    
    public List<LiveMetric> LiveMetrics { get; set; } = new();
    private System.Threading.Timer pollTimer;
    private bool isPolling = false;
    
    private void StartPolling()
    {
        isPolling = true;
        pollTimer = new System.Threading.Timer(async _ => await FetchLatestData(), null, 0, 10000);
    }
    
    private void StopPolling()
    {
        isPolling = false;
        pollTimer?.Dispose();
    }
    
    private async Task FetchLatestData()
    {
        try
        {
            var data = await Http.GetFromJsonAsync<List<LiveMetric>>("api/live-metrics");
            
            await InvokeAsync(() =>
            {
                LiveMetrics = data ?? new List<LiveMetric>();
                StateHasChanged();
            });
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error fetching data: {ex.Message}");
        }
    }
    
    public void Dispose()
    {
        pollTimer?.Dispose();
    }
}
```

## Troubleshooting

### Issue: Chart displays but shows no data

**Solutions:**
1. Verify `ValueField` matches your data class property name (case-sensitive)
2. Check that `DataSource` is not null or empty
3. Ensure `Minimum` and `Maximum` encompass your data values
4. Confirm data types are numeric (double, int, decimal)

```razor
<!-- Debugging example -->
@code {
    protected override void OnInitialized()
    {
        Console.WriteLine($"Data Count: {RevenueData?.Count ?? 0}");
        
        if (RevenueData != null && RevenueData.Any())
        {
            var first = RevenueData.First();
            Console.WriteLine($"First item - Revenue: {first.ActualRevenue}, Target: {first.TargetRevenue}");
        }
    }
}
```

### Issue: CategoryField not displaying labels

**Solutions:**
1. Ensure `CategoryField` property name is spelled correctly
2. Verify the category property is of type `string`
3. Check that `Height` is sufficient for multiple categories
4. Confirm data source contains the category field

### Issue: Remote data not loading

**Solutions:**
1. Check browser console for CORS errors
2. Verify API endpoint is accessible
3. Ensure `TValue` generic type matches API response structure
4. Test API response with tools like Postman
5. Add error handling to `DataAdaptor`
6. Ensure you are not passing untrusted, arbitrary URLs into client-side data adaptors. Validate or allow-list endpoints and prefer calling server-side endpoints you control (proxies) rather than fetching third-party URLs directly from client code.

### Issue: Dynamic updates not reflecting

**Solutions:**
1. Call `StateHasChanged()` after data updates
2. Create new list instance rather than modifying existing: `DataSource = new List<T>(updatedItems)`
3. Use `InvokeAsync()` when updating from background threads
4. Verify component lifecycle events