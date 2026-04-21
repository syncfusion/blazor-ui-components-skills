# Working with Data

This guide explains how to bind data to Stock Charts using various data sources including lists, remote APIs, Entity Framework, and dynamic collections.

## Table of Contents
- [Data Binding Methods](#data-binding-methods)
- [List Binding](#list-binding)
- [ExpandoObject Binding](#expandoobject-binding)
- [Observable Collection](#observable-collection)
- [Remote Data with SfDataManager](#remote-data-with-sfdatamanager)
- [Entity Framework Integration](#entity-framework-integration)
- [Data Transformation Patterns](#data-transformation-patterns)
- [Handling No Data](#handling-no-data)
- [Performance Considerations](#performance-considerations)
- [Common Issues](#common-issues)

## Data Binding Methods

The Stock Chart supports multiple data binding approaches:

- **List Binding** - Bind to `IEnumerable` collections (most common)
- **Remote Data** - Fetch from APIs using `SfDataManager` with OData, Web API
- **Entity Framework** - Connect to databases via EF Core
- **Observable Collection** - Dynamic data with automatic updates
- **ExpandoObject / DynamicObject** - Runtime type flexibility

## List Binding

### Basic List Binding

Bind an `IEnumerable` collection directly to the `DataSource` property:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL Stock Price">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Candle" 
                          XName="Date" 
                          High="High" 
                          Low="Low" 
                          Open="Open" 
                          Close="Close" 
                          Volume="Volume">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double High { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
        public double Volume { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5, Volume = 12000000 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7, Volume = 15000000 },
        new StockInfo { Date = new DateTime(2024, 01, 15), Open = 183.7, High = 190.5, Low = 182.5, Close = 188.2, Volume = 18000000 }
    };
}
```

###Property Mapping

Map data class properties to chart using these attributes:

| Attribute | Purpose | Example |
|-----------|---------|---------|
| `XName` | X-axis value (Date/Time) | `XName="Date"` |
| `Open` | Opening price | `Open="Open"` |
| `High` | Highest price | `High="High"` |
| `Low` | Lowest price | `Low="Low"` |
| `Close` | Closing price | `Close="Close"` |
| `Volume` | Trading volume (optional) | `Volume="Volume"` |
| `YName` | Single Y value (Line/Spline) | `YName="Close"` |

### Loading Data from API in OnInitialized

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL Stock Price">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Candle" 
                          XName="Date" 
                          High="High" 
                          Low="Low" 
                          Open="Open" 
                          Close="Close" 
                          Volume="Volume">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public List<StockInfo> StockData { get; set; } = new();

    protected override async Task OnInitializedAsync()
    {
        // Load from API
        StockData = await Http.GetFromJsonAsync<List<StockInfo>>("api/stocks/AAPL");
    }
}
```

## ExpandoObject Binding

Use `ExpandoObject` when data structure is determined at runtime:

```razor
@using Syncfusion.Blazor.Charts
@using System.Dynamic

<SfStockChart Title="Dynamic Stock Data">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Candle" 
                          XName="X" 
                          High="High" 
                          Low="Low" 
                          Open="Open" 
                          Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public List<ExpandoObject> StockData { get; set; } = new();
    private Random random = new Random();

    protected override void OnInitialized()
    {
        var dates = new List<DateTime> 
        { 
            new DateTime(2024, 01, 01), 
            new DateTime(2024, 01, 08), 
            new DateTime(2024, 01, 15) 
        };

        StockData = dates.Select(date =>
        {
            dynamic d = new ExpandoObject();
            d.X = date;
            d.Open = random.Next(175, 185);
            d.High = random.Next(188, 195);
            d.Low = random.Next(170, 180);
            d.Close = random.Next(180, 190);
            return d;
        }).Cast<ExpandoObject>().ToList();
    }
}
```

**When to use:**
- Parsing JSON with unknown structure
- Building dynamic dashboards
- Plugin systems with flexible data

## Observable Collection

Use `ObservableCollection` for automatically updating charts when data changes:

```razor
@using Syncfusion.Blazor.Charts
@using System.Collections.ObjectModel

<SfStockChart Title="Real-Time Stock Price">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Candle" 
                          XName="Date" 
                          High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

<button @onclick="AddDataPoint">Add Data Point</button>
<button @onclick="RemoveLastPoint">Remove Last Point</button>

@code {
    public ObservableCollection<StockInfo> StockData { get; set; }

    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double High { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
    }

    protected override void OnInitialized()
    {
        StockData = new ObservableCollection<StockInfo>
        {
            new StockInfo { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5 },
            new StockInfo { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7 }
        };
    }

    private void AddDataPoint()
    {
        var lastDate = StockData.Last().Date;
        StockData.Add(new StockInfo 
        { 
            Date = lastDate.AddDays(7),
            Open = 183.7, High = 190.5, Low = 182.5, Close = 188.2 
        });
    }

    private void RemoveLastPoint()
    {
        if (StockData.Count > 0)
            StockData.RemoveAt(StockData.Count - 1);
    }
}
```

**When to use:**
- Real-time data feeds
- Live price updates
- Interactive dashboards
- WebSocket connections

## Remote Data with SfDataManager

### OData Binding

```razor
@using Syncfusion.Blazor.Charts
@using Syncfusion.Blazor.Data

<SfStockChart Title="Stock Data from OData">
    <SfDataManager Url="https://api.example.com/odata/Stocks" 
                   CrossDomain="true" 
                   Adaptor="Adaptors.ODataAdaptor">
    </SfDataManager>
    
    <StockChartPrimaryXAxis ValueType="Syncfusion.Blazor.Charts.ValueType.DateTime">
    </StockChartPrimaryXAxis>
    
    <StockChartSeriesCollection>
        <StockChartSeries XName="Date" 
                          High="High" 
                          Low="Low" 
                          Open="Open" 
                          Close="Close" 
                          Type="ChartSeriesType.Candle">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>
```

### Web API Binding

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Stock Data from Web API">
    <SfDataManager Url="/api/stocks/AAPL" 
                   Adaptor="Adaptors.WebApiAdaptor">
    </SfDataManager>
    
    <StockChartSeriesCollection>
        <StockChartSeries XName="Date" 
                          High="High" Low="Low" Open="Open" Close="Close" 
                          Type="ChartSeriesType.Candle">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>
```

### Custom Headers and Parameters

```razor
<SfDataManager Url="/api/stocks" 
               Adaptor="Adaptors.WebApiAdaptor">
    <SfDataManagerHeaders>
        <SfDataManagerHeader Key="Authorization" Value="Bearer YOUR_TOKEN"></SfDataManagerHeader>
    </SfDataManagerHeaders>
</SfDataManager>
```

## Entity Framework Integration

### Create DbContext

```csharp
using Microsoft.EntityFrameworkCore;

public class StockDbContext : DbContext
{
    public virtual DbSet<StockPrice> StockPrices { get; set; }

    protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
    {
        optionsBuilder.UseSqlServer("YourConnectionString");
    }
}

public class StockPrice
{
    public int Id { get; set; }
    public string Symbol { get; set; }
    public DateTime Date { get; set; }
    public double Open { get; set; }
    public double High { get; set; }
    public double Low { get; set; }
    public double Close { get; set; }
    public long Volume { get; set; }
}
```

### Data Access Layer

```csharp
public class StockDataService
{
    private readonly StockDbContext _context;

    public StockDataService(StockDbContext context)
    {
        _context = context;
    }

    public async Task<List<StockPrice>> GetStockPricesAsync(string symbol, DateTime startDate, DateTime endDate)
    {
        return await _context.StockPrices
            .Where(s => s.Symbol == symbol && s.Date >= startDate && s.Date <= endDate)
            .OrderBy(s => s.Date)
            .ToListAsync();
    }
}
```

### Use in Component

```razor
@using Syncfusion.Blazor.Charts
@inject StockDataService StockService

<SfStockChart Title="@($"{SelectedSymbol} - Historical Prices")">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Candle" 
                          XName="Date" 
                          High="High" Low="Low" Open="Open" Close="Close" Volume="Volume">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    private string SelectedSymbol = "AAPL";
    private List<StockPrice> StockData { get; set; } = new();

    protected override async Task OnInitializedAsync()
    {
        var startDate = DateTime.Now.AddYears(-1);
        var endDate = DateTime.Now;
        StockData = await StockService.GetStockPricesAsync(SelectedSymbol, startDate, endDate);
    }
}
```

## Data Transformation Patterns

### Filtering Data

```csharp
@code {
    private DateTime FilterStartDate = DateTime.Now.AddMonths(-6);
    
    private List<StockInfo> FilteredData => AllStockData
        .Where(s => s.Date >= FilterStartDate)
        .ToList();
}
```

### Aggregating Data (Daily to Weekly)

```csharp
@code {
    private List<StockInfo> ConvertToWeekly(List<StockInfo> dailyData)
    {
        return dailyData
            .GroupBy(s => CultureInfo.CurrentCulture.Calendar.GetWeekOfYear(
                s.Date, CalendarWeekRule.FirstDay, DayOfWeek.Monday))
            .Select(g => new StockInfo
            {
                Date = g.First().Date,
                Open = g.First().Open,
                High = g.Max(s => s.High),
                Low = g.Min(s => s.Low),
                Close = g.Last().Close,
                Volume = g.Sum(s => s.Volume)
            })
            .ToList();
    }
}
```

### Calculating Adjusted Prices

```csharp
@code {
    private List<StockInfo> ApplySplitAdjustment(List<StockInfo> data, DateTime splitDate, double splitRatio)
    {
        return data.Select(s =>
        {
            if (s.Date < splitDate)
            {
                return new StockInfo
                {
                    Date = s.Date,
                    Open = s.Open / splitRatio,
                    High = s.High / splitRatio,
                    Low = s.Low / splitRatio,
                    Close = s.Close / splitRatio,
                    Volume = s.Volume * splitRatio
                };
            }
            return s;
        }).ToList();
    }
}
```

## Handling No Data

Display a custom message when data is unavailable:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart @ref="stockChart" Title="AAPL Stock Price">
    <NoDataTemplate>
        <div class="noDataTemplateContainerStyle" style="border: 2px solid orange; display: row-flex; align-items: center; justify-content: center; align-content: center; white-space: normal; text-align: center; width: inherit; height: inherit; font-weight: bolder; font-size: medium;">
            <div><img src="images/common/no-data.png" alt="No Data" style="height: 150px;" /></div>
            <div style="font-size:15px;"><strong>No data available to display.</strong></div>
        </div>
    </NoDataTemplate>
    <ChildContent>
        <StockChartChartBorder Width="0"></StockChartChartBorder>
        <StockChartSeriesCollection>
            <StockChartSeries DataSource="@Visible" Type="ChartSeriesType.Candle" XName="Date" High="High" Low="Low" Open="Open" Close="Close" Volume="Volume"/>
        </StockChartSeriesCollection>
        <StockChartLegendSettings Visible="true"></StockChartLegendSettings>
    </ChildContent>
</SfStockChart>

<style>
    .noDataTemplateContainerStyle {
        background-color: #fafafa;
        color: #000000;
    }
</style>

<div style="margin-top: 20px;">
    <button class="btn btn-primary" @onclick="LoadData">Load Data</button>
</div>

@code {
    private SfStockChart stockChart;

    private bool HasData { get; set; }
    public class ChartData
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
        public double High { get; set; }
        public double Volume { get; set; }
    }

    public List<ChartData> StockDetails = new()
    {
        new ChartData { Date = new DateTime(2012, 04, 02), Open = 85.9757, High = 90.6657, Low = 85.7685, Close = 90.5257, Volume = 660187068 },
        new ChartData { Date = new DateTime(2012, 04, 09), Open = 89.4471, High = 92, Low = 86.2157, Close = 86.4614, Volume = 912634864 },
        new ChartData { Date = new DateTime(2012, 04, 16), Open = 87.1514, High = 88.6071, Low = 81.4885, Close = 81.8543, Volume = 1221746066 },
        new ChartData { Date = new DateTime(2012, 04, 23), Open = 81.5157, High = 88.2857, Low = 79.2857, Close = 86.1428, Volume = 965935749 },
        new ChartData { Date = new DateTime(2012, 04, 30), Open = 85.4, High =  85.4857, Low = 80.7385, Close = 80.75, Volume = 615249365 },
        new ChartData { Date = new DateTime(2012, 05, 07), Open = 80.2143, High = 82.2685, Low = 79.8185, Close = 80.9585, Volume = 541742692 },
        new ChartData { Date = new DateTime(2012, 05, 14), Open = 80.3671, High = 81.0728, Low = 74.5971, Close = 75.7685, Volume = 708126233 },
        new ChartData { Date = new DateTime(2012, 05, 21), Open = 76.3571, High = 82.3571, Low = 76.2928, Close = 80.3271, Volume = 682076215 },
        new ChartData { Date = new DateTime(2012, 05, 28), Open = 81.5571, High = 83.0714, Low = 80.0743, Close = 80.1414, Volume = 480059584 }
    };

    private void LoadData()
    {
        ShowData = true;
        stockChart.UpdateStockChart();
    }

    private bool ShowData = false;
    private IEnumerable<ChartData> Visible => ShowData ? StockDetails : new List<ChartData>();
}
```

## Performance Considerations

### Large Datasets

When working with thousands of data points:

1. **Use data aggregation** - Show daily data for recent periods, weekly/monthly for older data
2. **Implement virtualization** - Load data on-demand as user zooms/scrolls
3. **Enable lazy loading** - Fetch additional data only when needed

```csharp
@code {
    private async Task LoadDataForDateRange(DateTime start, DateTime end)
    {
        // Only load visible range
        StockData = await StockService.GetPricesAsync(Symbol, start, end);
        StateHasChanged();
    }
}
```

### Caching Strategy

```csharp
@code {
    private Dictionary<string, List<StockInfo>> _cache = new();

    private async Task<List<StockInfo>> GetCachedData(string symbol)
    {
        if (!_cache.ContainsKey(symbol))
        {
            _cache[symbol] = await LoadFromAPI(symbol);
        }
        return _cache[symbol];
    }
}
```

## Common Issues

### Issue: Chart shows no data

**Solutions:**
1. Verify `DataSource` is not null or empty
2. Check property names match exactly (case-sensitive)
3. Ensure DateTime values are valid
4. Confirm numeric values are not NaN or Infinity

### Issue: Date axis not formatting correctly

**Solutions:**
1. Ensure `XName` property points to DateTime field
2. Set `ValueType="Syncfusion.Blazor.Charts.ValueType.DateTime"` on X-axis
3. Check DateTime values are in correct timezone

### Issue: Performance issues with large datasets

**Solutions:**
1. Limit initial data load to recent dates
2. Implement data aggregation (daily → weekly → monthly)
3. Use ObservableCollection only when needed
4. Consider server-side paging for very large datasets
