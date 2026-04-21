# Data Binding

## Table of Contents
- [DataSource Property](#datasource-property)
- [Simple Array Binding](#simple-array-binding)
    - [Numeric Arrays](#numeric-arrays)
    - [Integer Arrays](#integer-arrays)
- [Object Collection Binding](#object-collection-binding)
    - [Basic Object Binding](#basic-object-binding)
    - [Numeric X-Axis Objects](#numeric-x-axis-objects)
    - [DateTime X-Axis Objects](#datetime-x-axis-objects)
- [ValueType Options](#valuetype-options)
    - [SparklineValueType.Numeric (Default)](#sparklinevaluetypenumeric-default)
    - [SparklineValueType.Category](#sparklinevaluetypecategory)
    - [SparklineValueType.DateTime](#sparklinevaluetypedatetime)
- [Dynamic Data Updates](#dynamic-data-updates)
    - [Updating Data Source](#updating-data-source)
    - [Real-Time Data Updates](#real-time-data-updates)
    - [Async Data Loading](#async-data-loading)
- [Data Transformation](#data-transformation)
    - [Filtering Data](#filtering-data)
    - [Aggregating Data](#aggregating-data)
- [Common Data Binding Patterns](#common-data-binding-patterns)
- [Troubleshooting](#troubleshooting)

This guide covers all data source configurations for Syncfusion Blazor Sparkline Charts, from simple arrays to complex object collections.

## DataSource Property

The `DataSource` property accepts various data structures:

- **Simple arrays:** `double[]`, `int[]`, `float[]`
- **Object collections:** `List<T>`, `IEnumerable<T>`
- **DataManager:** For remote data binding

## Simple Array Binding

### Numeric Arrays

The simplest form - direct numeric array binding:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@SimpleData" 
              Height="50px" 
              Width="200px">
</SfSparkline>

@code {
    public double[] SimpleData = new double[] { 3, 6, 4, 1, 3, 2, 5 };
}
```

**When to use:** Quick prototyping, inline data, single value series.

### Integer Arrays

```razor'
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@DailyCounts" 
              Height="50px" 
              Width="250px"
              Type="SparklineType.Column">
</SfSparkline>

@code {
    public int[] DailyCounts = new int[] { 120, 156, 142, 189, 165, 178, 195 };
}
```

## Object Collection Binding

### Basic Object Binding

Bind to custom objects using `XName` and `YName`:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@SalesData" 
              TValue="SalesInfo"
              XName="Month" 
              YName="Sales"
              ValueType="SparklineValueType.Category"
              Height="60px" 
              Width="300px">
</SfSparkline>

@code {
    public class SalesInfo
    {
        public string Month { get; set; }
        public double Sales { get; set; }
    }

    public List<SalesInfo> SalesData = new List<SalesInfo>
    {
        new SalesInfo { Month = "Jan", Sales = 35000 },
        new SalesInfo { Month = "Feb", Sales = 28000 },
        new SalesInfo { Month = "Mar", Sales = 34000 },
        new SalesInfo { Month = "Apr", Sales = 32000 },
        new SalesInfo { Month = "May", Sales = 40000 },
        new SalesInfo { Month = "Jun", Sales = 32000 }
    };
}
```

**Key Properties:**
- `TValue="SalesInfo"` - Generic type parameter
- `XName="Month"` - Property name for X-axis
- `YName="Sales"` - Property name for Y-axis
- `ValueType="Category"` - Required when X-axis is string-based

### Numeric X-Axis Objects

When X-axis values are numeric:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@TemperatureData" 
              TValue="TempReading"
              XName="Hour" 
              YName="Temperature"
              Height="60px" 
              Width="300px">
</SfSparkline>

@code {
    public class TempReading
    {
        public int Hour { get; set; }
        public double Temperature { get; set; }
    }

    public List<TempReading> TemperatureData = new List<TempReading>
    {
        new TempReading { Hour = 0, Temperature = 18.5 },
        new TempReading { Hour = 3, Temperature = 16.2 },
        new TempReading { Hour = 6, Temperature = 15.8 },
        new TempReading { Hour = 9, Temperature = 22.1 },
        new TempReading { Hour = 12, Temperature = 28.4 },
        new TempReading { Hour = 15, Temperature = 31.2 },
        new TempReading { Hour = 18, Temperature = 26.7 },
        new TempReading { Hour = 21, Temperature = 21.3 }
    };
}
```

**Note:** ValueType defaults to `Numeric` - no need to specify.

### DateTime X-Axis Objects

For time-series data with DateTime:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@StockPrices" 
              TValue="StockData"
              XName="Date" 
              YName="Price"
              ValueType="SparklineValueType.DateTime"
              Height="60px" 
              Width="350px">
    <SparklineTooltipSettings TValue="StockData" 
                              Visible="true" 
                              Format="${Date:MMM dd}: $${Price}">
    </SparklineTooltipSettings>
</SfSparkline>

@code {
    public class StockData
    {
        public DateTime Date { get; set; }
        public double Price { get; set; }
    }

    public List<StockData> StockPrices = new List<StockData>
    {
        new StockData { Date = new DateTime(2026, 1, 1), Price = 145.20 },
        new StockData { Date = new DateTime(2026, 1, 2), Price = 147.50 },
        new StockData { Date = new DateTime(2026, 1, 3), Price = 146.80 },
        new StockData { Date = new DateTime(2026, 1, 6), Price = 149.30 },
        new StockData { Date = new DateTime(2026, 1, 7), Price = 151.00 },
        new StockData { Date = new DateTime(2026, 1, 8), Price = 148.90 }
    };
}
```

## ValueType Options

The `ValueType` property determines how X-axis values are interpreted:

### SparklineValueType.Numeric (Default)

For numeric X-axis values:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@data" 
              TValue="DataPoint"
              XName="Index" 
              YName="Value"
              Height="80px"
              Width="400px"
              LineWidth="2"
              ValueType="SparklineValueType.Numeric">
</SfSparkline>

@code {
    private List<DataPoint> data = new()
    {
        new DataPoint { Index = 1, Value = 10 },
        new DataPoint { Index = 2, Value = 20 },
        new DataPoint { Index = 3, Value = 15 },
        new DataPoint { Index = 4, Value = 30 }
    };

    public class DataPoint
    {
        public int Index { get; set; }
        public double Value { get; set; }
    }
}
```

**Use when:** X-axis is int, double, float, decimal

### SparklineValueType.Category

For string/category X-axis values:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@data" 
              TValue="DataPoint"
              XName="Category" 
              YName="Value"
              Height="80px"
             Width="400px"
             LineWidth="2"
              ValueType="SparklineValueType.Category">
</SfSparkline>

@code {
    private List<DataPoint> data = new()
    {
        new DataPoint { Category = "Jan", Value = 10 },
        new DataPoint { Category = "Feb", Value = 20 },
        new DataPoint { Category = "Mar", Value = 15 },
        new DataPoint { Category = "Apr", Value = 30 },
        new DataPoint { Category = "May", Value = 18 }
    };

    public class DataPoint
    {
        public string Category { get; set; } = string.Empty;
        public double Value { get; set; }
    }
}

```

**Use when:** X-axis is string (months, names, categories)

### SparklineValueType.DateTime

For date/time X-axis values:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@data" 
              TValue="DataPoint"
              XName="Timestamp" 
              YName="Value" 
              Height="80px"
              Width="400px"
              LineWidth="3
              ValueType="SparklineValueType.DateTime">
</SfSparkline>
@code {
    private List<DataPoint> data = new()
    {
        new DataPoint { Timestamp = DateTime.Now.AddMinutes(-20), Value = 10 },
        new DataPoint { Timestamp = DateTime.Now.AddMinutes(-15), Value = 25 },
        new DataPoint { Timestamp = DateTime.Now.AddMinutes(-10), Value = 15 },
        new DataPoint { Timestamp = DateTime.Now.AddMinutes(-5),  Value = 35 },
        new DataPoint { Timestamp = DateTime.Now,               Value = 20 }
    };

    public class DataPoint
    {
        public DateTime Timestamp { get; set; }
        public double Value { get; set; }
    }
}
```

**Use when:** X-axis is DateTime

## Dynamic Data Updates

### Updating Data Source

Replace the entire data source:

```razor
@using Syncfusion.Blazor.Charts
<button @onclick="UpdateData">Refresh Data</button>

<SfSparkline @ref="sparklineObj"
              DataSource="@CurrentData" 
              Height="50px" 
              Width="250px">
</SfSparkline>

@code {
    private SfSparkline<double> sparklineObj;
    public double[] CurrentData = new double[] { 3, 6, 4, 1, 3, 2, 5 };

    private void UpdateData()
    {
        Random random = new Random();
        CurrentData = Enumerable.Range(1, 7)
                                .Select(_ => random.Next(1, 10))
                                .Select(x => (double)x)
                                .ToArray();
        StateHasChanged();
    }
}
```

### Real-Time Data Updates

Add new points incrementally:

```razor
@using Syncfusion.Blazor.Charts
<button @onclick="AddDataPoint">Add Point</button>

<SfSparkline DataSource="@LiveData" 
              Height="50px" 
              Width="300px"
              LineWidth="2">
</SfSparkline>

@code {
    private List<double> LiveData = new List<double> { 5, 7, 4, 8, 6 };

    private void AddDataPoint()
    {
        Random random = new Random();
        var newData = LiveData.ToList();
        newData.Add(random.Next(1, 10));
        
        // Keep only last 20 points
        if (newData.Count > 20)
        {
            newData.RemoveAt(0);
        }
        LiveData = newData;
        StateHasChanged();
    }
}
```

### Async Data Loading

Load data asynchronously from API:

```razor
@using Syncfusion.Blazor.Charts
@if (isLoading)
{
    <p>Loading data...</p>
}
else
{
    <SfSparkline DataSource="@ApiData" 
                  TValue="MetricData"
                  XName="Timestamp" 
                  YName="Value"
                  Height="60px" 
                  Width="300px">
    </SfSparkline>
}

@code {
    public class MetricData
    {
        public DateTime Timestamp { get; set; }
        public double Value { get; set; }
    }

    private bool isLoading = true;
    private List<MetricData> ApiData = new List<MetricData>();

    protected override async Task OnInitializedAsync()
    {
        await LoadDataAsync();
    }

    private async Task LoadDataAsync()
    {
        isLoading = true;
        
        // Simulate API call
        await Task.Delay(1000);
        
        ApiData = FetchDataFromApi();
        isLoading = false;
        StateHasChanged();
    }

    private List<MetricData> FetchDataFromApi()
    {
        // Replace with actual API call
        return Enumerable.Range(0, 10)
            .Select(i => new MetricData 
            { 
                Timestamp = DateTime.Now.AddHours(-i), 
                Value = new Random().Next(50, 100) 
            })
            .Reverse()
            .ToList();
    }
}
```

## Data Transformation

### Filtering Data

Show only relevant data points:

```razor
@using Syncfusion.Blazor.Charts
<button @onclick="() => FilterByThreshold(50)">Show Above 50</button>
<button @onclick="ShowAllData">Show All</button>

<SfSparkline DataSource="@FilteredData" 
              Height="50px" 
              Width="250px">
</SfSparkline>

@code {
    private double[] AllData = new double[] { 30, 65, 42, 78, 55, 38, 82, 45, 70 };
    private double[] FilteredData;

    protected override void OnInitialized()
    {
        FilteredData = AllData;
    }

    private void FilterByThreshold(double threshold)
    {
        FilteredData = AllData.Where(x => x >= threshold).ToArray();
        StateHasChanged();
    }

    private void ShowAllData()
    {
        FilteredData = AllData;
        StateHasChanged();
    }
}
```

### Aggregating Data

Group and aggregate before displaying:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@MonthlyAverages" 
              TValue="MonthlyData"
              XName="Month" 
              YName="Average"
              ValueType="SparklineValueType.Category"
              Height="60px" 
              Width="300px">
</SfSparkline>

@code {
    public class DailyReading
    {
        public DateTime Date { get; set; }
        public double Value { get; set; }
    }

    public class MonthlyData
    {
        public string Month { get; set; }
        public double Average { get; set; }
    }

    private List<DailyReading> DailyData = new List<DailyReading>(); // Populated elsewhere
    private List<MonthlyData> MonthlyAverages;

    protected override void OnInitialized()
    {
        // Group by month and calculate averages
        MonthlyAverages = DailyData
            .GroupBy(d => d.Date.ToString("MMM yyyy"))
            .Select(g => new MonthlyData
            {
                Month = g.Key,
                Average = g.Average(d => d.Value)
            })
            .ToList();
    }
}
```

## Common Data Binding Patterns

### Pattern 1: Database Entity Binding

Bind directly to EF Core entities:

```razor
@using Syncfusion.Blazor.Charts
@inject ApplicationDbContext DbContext

<SfSparkline DataSource="@orders" 
              TValue="Order"
              XName="OrderDate" 
              YName="TotalAmount"
              ValueType="SparklineValueType.DateTime"
              Height="60px" 
              Width="300px">
</SfSparkline>

@code {
    private List<Order> orders;

    protected override async Task OnInitializedAsync()
    {
        orders = await DbContext.Orders
            .Where(o => o.OrderDate >= DateTime.Now.AddMonths(-1))
            .OrderBy(o => o.OrderDate)
            .ToListAsync();
    }
}
```

### Pattern 2: JSON API Data

Deserialize and bind JSON data:

```razor
@inject HttpClient Http
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@metrics" 
              TValue="ApiMetric"
              XName="Date" 
              YName="Count"
              Height="60px" 
              Width="300px">
</SfSparkline>

@code {
    public class ApiMetric
    {
        public string Date { get; set; }
        public int Count { get; set; }
    }

    private List<ApiMetric> metrics;

    protected override async Task OnInitializedAsync()
    {
        metrics = await Http.GetFromJsonAsync<List<ApiMetric>>("api/metrics");
    }
}
```

### Pattern 3: Computed Properties

Use calculated fields for Y-axis:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@products" 
              TValue="Product"
              XName="Name" 
              YName="ProfitMargin"
              ValueType="SparklineValueType.Category"
              Height="60px" 
              Width="300px">
</SfSparkline>

@code {
    public class Product
    {
        public string Name { get; set; }
        public double Cost { get; set; }
        public double Price { get; set; }
        
        // Computed property for sparkline
        public double ProfitMargin => ((Price - Cost) / Price) * 100;
    }

    private List<Product> products = new List<Product>
    {
        new Product { Name = "Widget A", Cost = 25, Price = 50 },
        new Product { Name = "Widget B", Cost = 35, Price = 60 },
        new Product { Name = "Widget C", Cost = 40, Price = 70 }
    };
}
```

## Troubleshooting

### Issue: No Data Displayed

**Symptoms:** Sparkline renders but shows no line/bars

**Solutions:**
1. Verify DataSource is not null or empty
2. Check XName/YName match property names exactly (case-sensitive)
3. Ensure ValueType matches X-axis data type
4. Provide TValue for object collections
5. Check data values are not all the same (no variation)

### Issue: "Cannot implicitly convert type"

**Symptoms:** Compilation error with DataSource

**Solutions:**
1. Ensure TValue matches your data model type
2. Use correct array type (double[], int[], etc.)
3. Check generic type parameters

### Issue: DateTime Not Rendering Correctly

**Symptoms:** DateTime X-axis shows incorrectly

**Solutions:**
1. Set `ValueType="SparklineValueType.DateTime"`
2. Ensure XName property is actually DateTime type
3. Check DateTime values are valid and sequential

**Data binding is the foundation of sparklines - ensure it's configured correctly for optimal results.**
