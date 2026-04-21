# Data Binding

Learn how to bind data from various sources to the Syncfusion Blazor Range Selector, including local collections, remote data, and different data types.

## API Reference - Data Binding

**Related Classes & Properties:**
- `RangeNavigatorSeries` - Series data configuration
- `RangeNavigatorSeriesCollection` - Series container
- `SfDataManager` - Remote data binding (part of Syncfusion.Blazor.Data)
- `DataManagerRequest` - Query configuration

**Series Data Properties:**
- `DataSource` - object - Data collection source
- `XName` - string - X-axis field name
- `YName` - string - Y-axis field name
- `Query` - DataManagerRequest - Data manager query
- `Type` - RangeNavigatorType - Series visualization type

**Value Type Enum - RangeValueType:**
- `DateTime` - For date/time data
- `Double` - For numeric data
- `Logarithmic` - For exponential data

**Key Component Properties:**
- `ValueType` - RangeValueType - Data type to use
- `Interval` - int - Axis interval value
- `IntervalType` - RangeIntervalType - How to space intervals
- `LabelFormat` - string - Format for axis labels
- `LogBase` - double - Base for logarithmic scaling

**Events:**
- `Changed` - ChangedEventArgs - Fires when range changes
- `Loaded` - RangeLoadedEventArgs - Fires when component loads

For complete API details, see [api-reference.md](api-reference.md).

## Table of Contents

- [Overview](#overview)
- [Local Data Sources](#local-data-sources)
  - [List Collection](#list-collection)
  - [Array Data](#array-data)
  - [ExpandoObject](#expandoobject)
- [Remote Data Binding](#remote-data-binding)
  - [Using SfDataManager](#using-sfdatamanager)
  - [Web API Integration](#web-api-integration)
- [DateTime Data](#datetime-data)
- [Numeric Data](#numeric-data)
- [Logarithmic Data](#logarithmic-data)
- [Dynamic Updates](#dynamic-updates)
- [Observable Collections](#observable-collections)
- [Data Transformation](#data-transformation)
- [Best Practices](#best-practices)

## Overview

The Range Selector can bind to various data sources through the `DataSource` property of `RangeNavigatorSeries`. Data binding supports:

- **Local Data**: List, Array, ExpandoObject
- **Remote Data**: Web API, OData services via SfDataManager
- **Data Types**: DateTime, Numeric, Logarithmic
- **Dynamic Updates**: Real-time data refresh

## Local Data Sources

### List Collection

The most common approach is binding to a `List<T>` collection:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close"
                              Type="RangeNavigatorType.Area">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
        public double Volume { get; set; }
    }
    
    public object Range = new DateTime[] 
    { 
        new DateTime(2023, 01, 01), 
        new DateTime(2023, 12, 31) 
    };
    
    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2023, 01, 01), Close = 100, Volume = 1000000 },
        new StockInfo { Date = new DateTime(2023, 02, 01), Close = 105, Volume = 1200000 },
        new StockInfo { Date = new DateTime(2023, 03, 01), Close = 102, Volume = 1100000 },
        new StockInfo { Date = new DateTime(2023, 04, 01), Close = 108, Volume = 1300000 },
        new StockInfo { Date = new DateTime(2023, 05, 01), Close = 112, Volume = 1400000 },
        new StockInfo { Date = new DateTime(2023, 06, 01), Close = 115, Volume = 1500000 }
    };
}
```

### Array Data

Bind to arrays for simple data structures:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.Double">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@TemperatureData" 
                              XName="Day" 
                              YName="Temp">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public class TemperatureReading
    {
        public int Day { get; set; }
        public double Temp { get; set; }
    }
    
    public object Range = new double[] { 1, 30 };
    
    public TemperatureReading[] TemperatureData = new TemperatureReading[]
    {
        new TemperatureReading { Day = 1, Temp = 72 },
        new TemperatureReading { Day = 5, Temp = 75 },
        new TemperatureReading { Day = 10, Temp = 78 },
        new TemperatureReading { Day = 15, Temp = 82 },
        new TemperatureReading { Day = 20, Temp = 80 },
        new TemperatureReading { Day = 25, Temp = 77 },
        new TemperatureReading { Day = 30, Temp = 73 }
    };
}
```

### ExpandoObject

Use `ExpandoObject` for dynamic property names:

```razor
@using Syncfusion.Blazor.Charts
@using System.Dynamic

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@DynamicData" 
                              XName="timestamp" 
                              YName="value">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public object Range;
    public List<ExpandoObject> DynamicData = new List<ExpandoObject>();
    
    protected override void OnInitialized()
    {
        // Create dynamic data
        for (int i = 0; i < 12; i++)
        {
            dynamic dataPoint = new ExpandoObject();
            dataPoint.timestamp = new DateTime(2023, i + 1, 1);
            dataPoint.value = 100 + (i * 5);
            dataPoint.category = $"Month {i + 1}";
            
            DynamicData.Add(dataPoint);
        }
        
        Range = new DateTime[] 
        { 
            new DateTime(2023, 01, 01), 
            new DateTime(2023, 12, 31) 
        };
    }
}
```

## Remote Data Binding

### Using SfDataManager

Bind to remote data sources using `SfDataManager`. IMPORTANT: avoid pointing examples directly at public third-party services in shipped or interactive samples. Treat all remote data as untrusted and use a trusted, authenticated backend or a local/mock dataset during development. See the "Security Considerations" section below for mitigation strategies.

Example using a placeholder (replace with your internal API or a validated proxy):

```razor
@using Syncfusion.Blazor.Charts
@using Syncfusion.Blazor.Data

<!-- Use a trusted API or proxy that validates and sanitizes returned data -->
<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries XName="OrderDate" 
                              YName="Freight"
                              Type="RangeNavigatorType.Area">
            <SfDataManager Url="/api/proxy/orders" 
                           Adaptor="Adaptors.ODataV4Adaptor">
            </SfDataManager>
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    // Use a safe, internal endpoint or a mocked dataset during development.
    // The endpoint should enforce authentication, return only required fields,
    // and validate query parameters to prevent abuse.
    public DateTime[] Range = new DateTime[] 
    { 
        new DateTime(1996, 01, 01), 
        new DateTime(1998, 12, 31) 
    };
}
```

### Web API Integration

Fetch data from your own Web API:

```razor
@using Syncfusion.Blazor.Charts
@inject HttpClient Http

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@SalesData" 
                              XName="Date" 
                              YName="Amount">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@if (isLoading)
{
    <p>Loading data...</p>
}

@code {
    public class SalesRecord
    {
        public DateTime Date { get; set; }
        public decimal Amount { get; set; }
    }
    
    public object Range;
    public List<SalesRecord> SalesData;
    private bool isLoading = true;
    
    protected override async Task OnInitializedAsync()
    {
        try
        {
            // Fetch data from API
            SalesData = await Http.GetFromJsonAsync<List<SalesRecord>>("api/sales");
            
            if (SalesData != null && SalesData.Any())
            {
                // Set initial range to full data span
                Range = new DateTime[] 
                { 
                    SalesData.Min(s => s.Date), 
                    SalesData.Max(s => s.Date) 
                };
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error loading data: {ex.Message}");
            SalesData = new List<SalesRecord>();
        }
        finally
        {
            isLoading = false;
        }
    }
}
```

### Custom Data Service

Create a reusable data service:

```csharp
// Services/DataService.cs
public interface IDataService
{
    Task<List<StockInfo>> GetStockDataAsync(string symbol);
}

public class DataService : IDataService
{
    private readonly HttpClient _http;
    
    public DataService(HttpClient http)
    {
        _http = http;
    }
    
    public async Task<List<StockInfo>> GetStockDataAsync(string symbol)
    {
        var response = await _http.GetFromJsonAsync<List<StockInfo>>($"api/stocks/{symbol}");
        return response ?? new List<StockInfo>();
    }
}
```

```razor
@using Syncfusion.Blazor.Charts
@inject IDataService DataService

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public Range Range;
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double High { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
        public double Volume { get; set; }
    }
    public List<StockInfo> StockData;
    
    protected override async Task OnInitializedAsync()
    {
        StockData = await DataService.GetStockDataAsync("AAPL");
        
        if (StockData.Any())
        {
            Range = new DateTime[] 
            { 
                StockData.First().Date, 
                StockData.Last().Date 
            };
        }
    }
}
```

## DateTime Data

### Basic DateTime Binding

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  LabelFormat="MMM-yy"
                  IntervalType="RangeIntervalType.Months">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@TimeSeriesData" 
                              XName="Timestamp" 
                              YName="Value">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public class TimeSeriesPoint
    {
        public DateTime Timestamp { get; set; }
        public double Value { get; set; }
    }
    
    public object Range = new DateTime[] 
    { 
        new DateTime(2023, 01, 01), 
        new DateTime(2023, 06, 30) 
    };
    
    public List<TimeSeriesPoint> TimeSeriesData = GenerateTimeSeriesData();
    
    private static List<TimeSeriesPoint> GenerateTimeSeriesData()
    {
        var data = new List<TimeSeriesPoint>();
        var startDate = new DateTime(2023, 01, 01);
        var random = new Random();
        
        for (int i = 0; i < 365; i++)
        {
            data.Add(new TimeSeriesPoint
            {
                Timestamp = startDate.AddDays(i),
                Value = 100 + random.Next(-20, 20)
            });
        }
        
        return data;
    }
}
```

### DateTime Formatting

Control how dates are displayed:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  LabelFormat="dd-MMM"
                  IntervalType="RangeIntervalType.Days"
                  Interval="7">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@WeeklyData" 
                              XName="Week" 
                              YName="Sales">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public object Range = new DateTime[] 
    { 
        new DateTime(2023, 01, 01), 
        new DateTime(2023, 06, 30) 
    };

    public class WeeklySalesPoint
    {
        public DateTime Week { get; set; }   // Start date of the week
        public double Sales { get; set; }
    }

    public List<WeeklySalesPoint> WeeklyData = GenerateWeeklyData();

    private static List<WeeklySalesPoint> GenerateWeeklyData()
    {
        var data = new List<WeeklySalesPoint>();
        var startDate = new DateTime(2023, 01, 01);
        var random = new Random();

        // Generate weekly data for 52 weeks
        for (int i = 0; i < 52; i++)
        {
            data.Add(new WeeklySalesPoint
            {
                Week = startDate.AddDays(i * 7),
                Sales = 100 + random.Next(-20, 20)
            });
        }

        return data;
    }
}
```

### DateTime with Time Component

Include hours and minutes:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-Value="@Range"
                  ValueType="RangeValueType.DateTime"
                  LabelFormat="HH:mm"
                  IntervalType="RangeIntervalType.Hours">

    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@HourlyData" 
                              XName="Hour" 
                              YName="Traffic" 
                              Type="RangeNavigatorType.Line">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>

</SfRangeNavigator>

@code {
    public class TrafficData
    {
        public DateTime Hour { get; set; }
        public int Traffic { get; set; }
    }

    public object Range = new DateTime[]
    {
        DateTime.Today,
        DateTime.Today.AddHours(23)
    };

    public List<TrafficData> HourlyData = GenerateHourlyData();

    private static List<TrafficData> GenerateHourlyData()
    {
        var data = new List<TrafficData>();
        var startHour = DateTime.Today;
        var random = new Random();
        for (int i = 0; i < 24; i++)
        {
            data.Add(new TrafficData
            {
                Hour = startHour.AddHours(i),
                Traffic = 50 + random.Next(0, 150)
            });
        }
        return data;
    }
}
```

## Numeric Data

### Integer Values

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.Double"
                  LabelFormat="n0"
                  Interval="10">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@ScoreData" 
                              XName="Week" 
                              YName="Score">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public class WeeklyScore
    {
        public int Week { get; set; }
        public int Score { get; set; }
    }
    
    public object Range = new double[] { 0, 52 };
    
    public List<WeeklyScore> ScoreData = new List<WeeklyScore>
    {
        new WeeklyScore { Week = 1, Score = 75 },
        new WeeklyScore { Week = 10, Score = 82 },
        new WeeklyScore { Week = 20, Score = 88 },
        new WeeklyScore { Week = 30, Score = 85 },
        new WeeklyScore { Week = 40, Score = 90 },
        new WeeklyScore { Week = 52, Score = 95 }
    };
}
```

### Decimal Values

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.Double"
                  LabelFormat="n2"
                  Interval="0.5">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@PriceData" 
                              XName="Month" 
                              YName="Price">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public class MonthlyPrice
    {
        public double Month { get; set; }
        public decimal Price { get; set; }
    }
    
    public object Range = new double[] { 0, 12 };
    
    public List<MonthlyPrice> PriceData = new List<MonthlyPrice>
    {
        new MonthlyPrice { Month = 0, Price = 9.99m },
        new MonthlyPrice { Month = 3, Price = 10.49m },
        new MonthlyPrice { Month = 6, Price = 10.99m },
        new MonthlyPrice { Month = 9, Price = 11.49m },
        new MonthlyPrice { Month = 12, Price = 11.99m }
    };
}
```

### Currency Values

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.Double"
                  LabelFormat="C0">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@RevenueData" 
                              XName="Quarter" 
                              YName="Revenue">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public class QuarterlyRevenue
    {
        public int Quarter { get; set; }
        public decimal Revenue { get; set; }
    }
    
    public object Range = new double[] { 1, 4 };
    
    public List<QuarterlyRevenue> RevenueData = new List<QuarterlyRevenue>
    {
        new QuarterlyRevenue { Quarter = 1, Revenue = 1250000 },
        new QuarterlyRevenue { Quarter = 2, Revenue = 1450000 },
        new QuarterlyRevenue { Quarter = 3, Revenue = 1350000 },
        new QuarterlyRevenue { Quarter = 4, Revenue = 1650000 }
    };
}
```

## Logarithmic Data

For data spanning multiple orders of magnitude:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.Logarithmic"
                  LogBase="10"
                  Interval="1">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@ExponentialData" 
                              XName="X" 
                              YName="Y"
                              Type="RangeNavigatorType.Line">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public class ExponentialPoint
    {
        public double X { get; set; }
        public double Y { get; set; }
    }
    
    public object Range = new double[] { 1, 1000000 };
    
    public List<ExponentialPoint> ExponentialData = new List<ExponentialPoint>
    {
        new ExponentialPoint { X = 1, Y = 1 },
        new ExponentialPoint { X = 10, Y = 10 },
        new ExponentialPoint { X = 100, Y = 100 },
        new ExponentialPoint { X = 1000, Y = 1000 },
        new ExponentialPoint { X = 10000, Y = 10000 },
        new ExponentialPoint { X = 100000, Y = 100000 },
        new ExponentialPoint { X = 1000000, Y = 1000000 }
    };
}
```

## Dynamic Updates

### Refresh Data on Demand

```razor
@using Syncfusion.Blazor.Charts

<button @onclick="RefreshData" class="btn btn-primary">Refresh Data</button>

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@LiveData" 
                              XName="Timestamp" 
                              YName="Value">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public object Range;
    public List<DataPoint> LiveData = new List<DataPoint>();

    public class DataPoint
    {
        public DateTime Timestamp { get; set; }
        public double Value { get; set; }
    }
    
    protected override void OnInitialized()
    {
        RefreshData();
    }
    
    private void RefreshData()
    {
        LiveData.Clear();
        var now = DateTime.Now;
        var random = new Random();
        
        for (int i = 0; i < 100; i++)
        {
            LiveData.Add(new DataPoint
            {
                Timestamp = now.AddHours(-100 + i),
                Value = 100 + random.Next(-30, 30)
            });
        }
        
        Range = new DateTime[] 
        { 
            LiveData.First().Timestamp, 
            LiveData.Last().Timestamp 
        };
        
        StateHasChanged();
    }
}
```

### Real-Time Updates with Timer

```razor
@using System.Timers
@using Syncfusion.Blazor.Charts
@implements IDisposable

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  LabelFormat="HH:mm:ss">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@RealtimeData" 
                              XName="Time" 
                              YName="Value"
                              Type="RangeNavigatorType.Line">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

<p>Last Update: @lastUpdate.ToString("HH:mm:ss")</p>

@code {
    public class RealtimePoint
    {
        public DateTime Time { get; set; }
        public double Value { get; set; }
    }
    
    public object Range;
    public List<RealtimePoint> RealtimeData = new List<RealtimePoint>();
    private Timer updateTimer;
    private Random random = new Random();
    private DateTime lastUpdate;
    
    protected override void OnInitialized()
    {
        // Initialize with some data
        var now = DateTime.Now;
        for (int i = 0; i < 60; i++)
        {
            RealtimeData.Add(new RealtimePoint
            {
                Time = now.AddSeconds(-60 + i),
                Value = 100 + random.Next(-20, 20)
            });
        }
        
        Range = new DateTime[] 
        { 
            RealtimeData.First().Time, 
            RealtimeData.Last().Time 
        };
        
        // Set up timer for updates
        updateTimer = new Timer(1000); // Update every second
        updateTimer.Elapsed += UpdateData;
        updateTimer.Start();
    }
    
    private void UpdateData(object sender, ElapsedEventArgs e)
    {
        InvokeAsync(() =>
        {
            // Add new data point
            var lastValue = RealtimeData.Last().Value;
            RealtimeData.Add(new RealtimePoint
            {
                Time = DateTime.Now,
                Value = lastValue + random.Next(-5, 5)
            });
            
            // Keep only last 60 points
            if (RealtimeData.Count > 60)
            {
                RealtimeData.RemoveAt(0);
            }
            
            // Update range to show latest data
            Range = new DateTime[] 
            { 
                RealtimeData.First().Time, 
                RealtimeData.Last().Time 
            };
            
            lastUpdate = DateTime.Now;
            StateHasChanged();
        });
    }
    
    public void Dispose()
    {
        updateTimer?.Dispose();
    }
}
```

## Observable Collections

Use `ObservableCollection` for automatic UI updates:

```razor
@using Syncfusion.Blazor.Charts
@using System.Collections.ObjectModel

<button @onclick="AddDataPoint">Add Data Point</button>
<button @onclick="RemoveDataPoint">Remove Last Point</button>

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.Double">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@ObservableData" 
                              XName="X" 
                              YName="Y">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

<p>Data Points: @ObservableData.Count</p>

@code {
    public class DataPoint
    {
        public double X { get; set; }
        public double Y { get; set; }
    }
    
    public object Range = new double[] { 0, 10 };
    public ObservableCollection<DataPoint> ObservableData = new ObservableCollection<DataPoint>
    {
        new DataPoint { X = 0, Y = 10 },
        new DataPoint { X = 2, Y = 15 },
        new DataPoint { X = 4, Y = 12 },
        new DataPoint { X = 6, Y = 18 },
        new DataPoint { X = 8, Y = 20 },
        new DataPoint { X = 10, Y = 17 }
    };
    
    private Random random = new Random();
    
    private void AddDataPoint()
    {
        var lastX = ObservableData.Any() ? ObservableData.Max(d => d.X) : 0;
        ObservableData.Add(new DataPoint
        {
            X = lastX + 2,
            Y = 10 + random.Next(0, 20)
        });
        
        // Update range
        Range = new double[] { 0, lastX + 2 };
    }
    
    private void RemoveDataPoint()
    {
        if (ObservableData.Any())
        {
            ObservableData.RemoveAt(ObservableData.Count - 1);
        }
    }
}
```

## Data Transformation

### Aggregating Data

Transform detailed data into aggregated form:

```razor
@code {
    public class DailyData
    {
        public DateTime Date { get; set; }
        public decimal Amount { get; set; }
    }
    
    public class MonthlyData
    {
        public DateTime Month { get; set; }
        public decimal TotalAmount { get; set; }
    }
    
    private List<MonthlyData> AggregateToMonthly(List<DailyData> dailyData)
    {
        return dailyData
            .GroupBy(d => new DateTime(d.Date.Year, d.Date.Month, 1))
            .Select(g => new MonthlyData
            {
                Month = g.Key,
                TotalAmount = g.Sum(d => d.Amount)
            })
            .OrderBy(m => m.Month)
            .ToList();
    }
}
```

### Filtering Data

Pre-filter data before binding:

```razor
@code {
    private List<StockInfo> GetFilteredData(string symbol, DateTime startDate, DateTime endDate)
    {
        return AllStockData
            .Where(s => s.Symbol == symbol)
            .Where(s => s.Date >= startDate && s.Date <= endDate)
            .OrderBy(s => s.Date)
            .ToList();
    }
}
```

### Sorting Data

Always ensure data is sorted by X-axis values:

```razor
@code {
    protected override void OnInitialized()
    {
        // Sort data by date
        StockData = UnsortedData
            .OrderBy(d => d.Date)
            .ToList();
    }
}
```

## Security Considerations

When binding to remote data, treat all external content as untrusted. Follow these mitigations to reduce risk when your skill or examples access remote sources:

- **Prefer trusted/internal APIs or mocks:** Do not point shipped examples or interactive demos at arbitrary public endpoints. Use internal, authenticated APIs or local/mock data for samples.
- **Server-side validation & proxy:** Query third-party sources from a backend you control. Validate request parameters, filter and transform results to only the fields required, and reject unexpected payloads.
- **Whitelist domains and fields:** Only allow requests to known domains and only surface necessary properties to the UI.
- **Avoid dynamic deserialization:** Prefer strongly-typed DTOs; avoid deserializing into `dynamic`/`ExpandoObject` when data origin is untrusted.
- **Validate schema and types:** Enforce JSON schema validation or manual checks (dates, numeric ranges, string lengths) before binding to the chart.
- **Sanitize content:** Never render untrusted HTML or script coming from remote data into the page or templates.
- **Timeouts, rate limits, and auth:** Use timeouts, enforce rate limits, and require authentication/authorization on proxy endpoints.
- **Logging & monitoring:** Log failed validations and unusual payloads; monitor for suspicious activity.
- **Fail-safe UI:** If validation fails, show an error or fallback to mocked/sample data rather than binding raw external data.

Example server-side pattern (ASP.NET Core minimal proxy):

```csharp
[HttpGet("/api/proxy/orders")]
public async Task<IActionResult> GetOrders()
{
    // Validate and sanitize incoming query parameters here
    var client = _httpClientFactory.CreateClient("trusted-data-source");
    var resp = await client.GetAsync("/odata/Orders?$select=OrderDate,Freight");
    if (!resp.IsSuccessStatusCode) return StatusCode((int)resp.StatusCode);

    var payload = await resp.Content.ReadFromJsonAsync<List<OrderDto>>();
    // Additional validation on payload (date ranges, sizes)
    return Ok(payload);
}
```

After this proxy, point your `SfDataManager` at `/api/proxy/orders` so your frontend only receives validated, minimal data.

## Best Practices

### 1. Sort Data

Always sort data by X-axis values in ascending order:

```csharp
StockData = StockData.OrderBy(s => s.Date).ToList();
```

### 2. Handle Null Data

Check for null or empty data:

```razor
@if (Data != null && Data.Any())
{
    <SfRangeNavigator @bind-Value="@Range" ValueType="RangeValueType.DateTime">
        <RangeNavigatorSeriesCollection>
            <RangeNavigatorSeries DataSource="@Data" XName="Date" YName="Value">
            </RangeNavigatorSeries>
        </RangeNavigatorSeriesCollection>
    </SfRangeNavigator>
}
else
{
    <p>No data available.</p>
}
```

### 3. Match Property Names

Ensure `XName` and `YName` match your data properties exactly:

```csharp
public class SalesData
{
    public DateTime OrderDate { get; set; }  // Use "OrderDate" in XName
    public decimal Revenue { get; set; }      // Use "Revenue" in YName
}
```

```razor
<RangeNavigatorSeries DataSource="@Sales" 
                      XName="OrderDate"     <!-- Match property name -->
                      YName="Revenue">      <!-- Match property name -->
```

### 4. Use Appropriate Value Types

Match `ValueType` to your data:

```razor
<!-- DateTime data -->
<SfRangeNavigator ValueType="RangeValueType.DateTime">

<!-- Numeric data -->
<SfRangeNavigator ValueType="RangeValueType.Double">
```

### 5. Handle Large Datasets

For large datasets, consider data virtualization or grouping:

```razor
<SfRangeNavigator EnableGrouping="true" 
                  GroupBy="RangeIntervalType.Months">
```

## Complete Example

Here's a comprehensive data binding example:

```razor
@page "/data-binding-demo"
@using Syncfusion.Blazor.Charts
@inject HttpClient Http

<div class="container">
    <h3>Stock Price Range Selector</h3>
    
    <div class="controls">
        <label>Select Stock:</label>
        <select @bind="SelectedSymbol" @bind:after="LoadStockData">
            <option value="AAPL">Apple (AAPL)</option>
            <option value="MSFT">Microsoft (MSFT)</option>
            <option value="GOOGL">Google (GOOGL)</option>
        </select>
        
        <button @onclick="RefreshData" class="btn btn-primary">Refresh</button>
    </div>
    
    @if (isLoading)
    {
        <div class="loading">Loading stock data...</div>
    }
    else if (StockData != null && StockData.Any())
    {
        <SfRangeNavigator @bind-Value="@Range" 
                          ValueType="RangeValueType.DateTime"
                          LabelFormat="MMM-yy"
                          IntervalType="RangeIntervalType.Months">
            <RangeNavigatorRangeTooltipSettings Enable="true"></RangeNavigatorRangeTooltipSettings>
            <RangeNavigatorSeriesCollection>
                <RangeNavigatorSeries DataSource="@StockData" 
                                      XName="Date" 
                                      YName="Close"
                                      Type="RangeNavigatorType.Area"
                                      Fill="rgba(33, 150, 243, 0.3)">
                    <RangeNavigatorSeriesBorder Color="#2196F3" Width="2"></RangeNavigatorSeriesBorder>
                </RangeNavigatorSeries>
            </RangeNavigatorSeriesCollection>
        </SfRangeNavigator>
        
        <div class="statistics">
            <h4>Selected Period Statistics</h4>
            <div class="stats-grid">
                <div class="stat-item">
                    <span class="stat-label">Start Price:</span>
                    <span class="stat-value">${FilteredData.FirstOrDefault()?.Close:F2}</span>
                </div>
                <div class="stat-item">
                    <span class="stat-label">End Price:</span>
                    <span class="stat-value">${FilteredData.LastOrDefault()?.Close:F2}</span>
                </div>
                <div class="stat-item">
                    <span class="stat-label">High:</span>
                    <span class="stat-value">${FilteredData.Max(d => d.Close):F2}</span>
                </div>
                <div class="stat-item">
                    <span class="stat-label">Low:</span>
                    <span class="stat-value">${FilteredData.Min(d => d.Close):F2}</span>
                </div>
                <div class="stat-item">
                    <span class="stat-label">Average:</span>
                    <span class="stat-value">${FilteredData.Average(d => d.Close):F2}</span>
                </div>
                <div class="stat-item">
                    <span class="stat-label">Data Points:</span>
                    <span class="stat-value">{FilteredData.Count}</span>
                </div>
            </div>
        </div>
    }
    else
    {
        <div class="error">No data available. Please try again.</div>
    }
</div>

@code {
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
        public double Volume { get; set; }
    }
    
    public object Range;
    public List<StockInfo> StockData;
    public List<StockInfo> FilteredData
    {
        get
        {
            if (StockData == null || Range == null)
                return new List<StockInfo>();

            var dateRange = Range as DateTime[];
            if (dateRange == null || dateRange.Length < 2)
                return new List<StockInfo>();

            var start = dateRange[0];
            var end = dateRange[1];

            return StockData
                .Where(s => s.Date >= start && s.Date <= end)
                .ToList();
        }
    }
    private string SelectedSymbol = "AAPL";
    private bool isLoading = false;
    
    protected override async Task OnInitializedAsync()
    {
        await LoadStockData();
    }
    
    private async Task LoadStockData()
    {
        isLoading = true;

        try
        {
            // Recommended: call an internal proxy or validated API endpoint.
            // Do NOT call arbitrary third-party URLs from the client.
            // Example (commented):
            // var response = await Http.GetFromJsonAsync<List<StockInfo>>($"/api/proxy/stocks/{SelectedSymbol}");
            // if (response != null && response.Count > 0 && response.Count <= 100000)
            // {
            //     // Basic schema & value validation
            //     var valid = response.All(s => s.Date > DateTime.MinValue && s.Close >= 0 && s.Close < 1_000_000_000);
            //     if (valid)
            //     {
            //         StockData = response.OrderBy(s => s.Date).ToList();
            //     }
            // }

            // Fallback for demos: generate sample data locally instead of binding to an untrusted remote source
            if (StockData == null || !StockData.Any())
            {
                StockData = GenerateStockData(SelectedSymbol);
            }

            // Final safety checks before using data for analytics/UI
            if (StockData != null && StockData.Any())
            {
                StockData = StockData.OrderBy(s => s.Date).ToList();
                Range = new DateTime[] 
                { 
                    StockData.First().Date, 
                    StockData.Last().Date 
                };
            }
        }
        catch (Exception ex)
        {
            Console.WriteLine($"Error loading data: {ex.Message}");
            StockData = new List<StockInfo>();
        }
        finally
        {
            isLoading = false;
        }
    }
    
    private async Task RefreshData()
    {
        await LoadStockData();
    }
    
    private static List<StockInfo> GenerateStockData(string symbol)
    {
        var data = new List<StockInfo>();
        var startDate = DateTime.Now.AddYears(-2);
        var random = new Random(symbol.GetHashCode());
        double price = 100 + random.Next(50, 200);
        
        for (int i = 0; i < 500; i++)
        {
            price += random.Next(-5, 6);
            price = Math.Max(50, price);
            
            data.Add(new StockInfo
            {
                Date = startDate.AddDays(i),
                Close = price,
                Volume = random.Next(1000000, 5000000)
            });
        }
        
        return data;
    }
}

<style>
    .container {
        padding: 20px;
        max-width: 1200px;
        margin: 0 auto;
    }
    
    .controls {
        margin-bottom: 20px;
        display: flex;
        gap: 15px;
        align-items: center;
    }
    
    .controls select {
        padding: 8px;
        border: 1px solid #ddd;
        border-radius: 4px;
    }
    
    .loading, .error {
        padding: 20px;
        text-align: center;
        background-color: #f8f9fa;
        border-radius: 5px;
    }
    
    .error {
        background-color: #f8d7da;
        color: #721c24;
    }
    
    .statistics {
        margin-top: 30px;
        padding: 20px;
        background-color: #f8f9fa;
        border-radius: 8px;
    }
    
    .stats-grid {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
        gap: 15px;
        margin-top: 15px;
    }
    
    .stat-item {
        padding: 15px;
        background: white;
        border-radius: 5px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    
    .stat-label {
        display: block;
        font-size: 12px;
        color: #666;
        margin-bottom: 5px;
    }
    
    .stat-value {
        display: block;
        font-size: 20px;
        font-weight: bold;
        color: #2196F3;
    }
</style>
```

