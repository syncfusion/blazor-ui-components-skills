# Range Configuration

Learn how to configure range selection behavior, value binding, and programmatic range updates in the Syncfusion Blazor Range Selector.

## API Reference - Range Configuration

**Related Classes & Enums:**
- `SfRangeNavigator` - Main component
- `RangeValueType` - Enum: DateTime, Double, Logarithmic
- `ChangedEventArgs` - Event args when range changes
- `RangeResizeEventArgs` - Event args during/after resize
- `RangeNavigatorAnimation` - Animation settings

**Key Properties:**
- `Value` - DateTime[] or double[] - Selected range [start, end]
- `ValueType` - RangeValueType - Specify data type
- `LogBase` - double - Base for logarithmic axis (default: 10)
- `Interval` - int - Axis interval value
- `IntervalType` - RangeIntervalType - Interval type
- `AllowSnapping` - bool - Snap thumbs to data points
- `EnableGrouping` - bool - Enable data grouping

**Events:**
- `Changed` - Fires when range selection changes
- `Resizing` - Fires while dragging thumbs
- `Resized` - Fires after drag completes

For complete API details, see [api-reference.md](api-reference.md).

## Table of Contents

- [Overview](#overview)
- [Value Property](#value-property)
    - [DateTime Range](#datetime-range)
    - [Numeric Range](#numeric-range)
    - [Logarithmic Range](#logarithmic-range)
- [Value Binding](#value-binding)
    - [One-Way Binding](#one-way-binding)
    - [Two-Way Binding](#two-way-binding)
    - [Event-Based Binding](#event-based-binding)
- [Thumb Dragging](#thumb-dragging)
    - [Enable Dragging](#enable-dragging)
    - [Disable Dragging](#disable-dragging)
    - [Snapping to Data Points](#snapping-to-data-points)
- [Label Selection](#label-selection)
- [Programmatic Range Updates](#programmatic-range-updates)
    - [Update Range from Code](#update-range-from-code)
    - [Validate Range Selection](#validate-range-selection)
- [Initial Range Configuration](#initial-range-configuration)
    - [Default to Recent Data](#default-to-recent-data)
    - [Default to Full Data Range](#default-to-full-data-range)
    - [Default to Specific Period](#default-to-specific-period)
- [Best Practices](#best-practices)
    - [1. Match Value Type to Data](#1-match-value-type-to-data)
    - [2. Sort Data](#2-sort-data)
    - [3. Use Two-Way Binding](#3-use-two-way-binding)
    - [4. Provide Visual Feedback](#4-provide-visual-feedback)
- [Complete Example](#complete-example)

## Overview
Configure range selection using `Value` property for two-way binding, `ValueType` for data type specification, and event handlers like `Changed` for range updates.

The Range Selector allows users to select a specific range from data using draggable thumbs or by tapping on labels. This guide covers all aspects of configuring range values, selection behavior, and value binding patterns.

## Value Property
Define selected range using `Value` property as an array [start, end] with data type specified by `ValueType` (DateTime, Double, Logarithmic).

The `Value` property defines the selected range as an array containing start and end values.

### DateTime Range
Configure date-based ranges using `Value="DateTime[]"` with `ValueType="RangeValueType.DateTime"`:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator Value="@SelectedRange" 
                  ValueType="RangeValueType.DateTime">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@SalesDetails" 
                              XName="Date" 
                              YName="Sales"
                              Type="RangeNavigatorType.Area">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

<p>Selected: @SelectedRange[0].ToShortDateString() to @SelectedRange[1].ToShortDateString()</p>

@code {
    public DateTime[] SelectedRange = new DateTime[] 
    { 
        new DateTime(2023, 01, 01),  // Start date
        new DateTime(2023, 06, 30)   // End date
    };

    public class SalesData
    {
        public DateTime Date { get; set; }
        public double Sales { get; set; }
    }
 
    public List<SalesData> SalesDetails = GetSalesData();

    public static List<SalesData> GetSalesData()
    {
        var data = new List<SalesData>();
        var random = new Random();
        var startDate = new DateTime(2023, 01, 01);
        double sales = 200;

        for (int i = 0; i < 181; i++) // Jan 1 to Jun 30
        {
            sales += random.NextDouble() * 20 - 10; // fluctuate
            sales = Math.Max(sales, 50); // floor value

            data.Add(new SalesData
            {
                Date = startDate.AddDays(i),
                Sales = Math.Round(sales, 2)
            });
        }

        return data;
    }
}
```

### Numeric Range
Configure numeric ranges using `Value="double[]"` with `ValueType="RangeValueType.Double"` and `LabelFormat` for display:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator Value="@TemperatureRange" 
                  ValueType="RangeValueType.Double"
                  Interval="5"
                  LabelFormat="n0">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@TemperatureDetails" 
                              XName="Day" 
                              YName="Temperature"
                              Type="RangeNavigatorType.Line">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

<p>Temperature Range: @TemperatureRange[0]°F to @TemperatureRange[1]°F</p>

@code {
    public double[] TemperatureRange = new double[] { 10, 20 };
    
    public List<TemperatureData> TemperatureDetails = GetTemperatureData();

    public class TemperatureData
    {
        public double Day { get; set; }
        public double Temperature { get; set; }
    }

    public static List<TemperatureData> GetTemperatureData()
    {
        var data = new List<TemperatureData>();
        var random = new Random();
        double temperature = 70;

        for (int day = 1; day <= 30; day++)
        {
            temperature += random.NextDouble() * 6 - 3; // fluctuate ±3°F
            temperature = Math.Max(45, Math.Min(temperature, 100)); // clamp range

            data.Add(new TemperatureData
            {
                Day = day,
                Temperature = Math.Round(temperature, 1)
            });
        }

        return data;
    }
}
```

### Logarithmic Range
Configure logarithmic ranges using `ValueType="RangeValueType.Logarithmic"` with `LogBase` property for custom logarithm base:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator Value="@LogRange"
                  ValueType="RangeValueType.Logarithmic"
                  LogBase="10"
                  Height="160px"
                  LabelFormat="n0">

    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@ExponentialData"
                              XName="X"
                              YName="Y"
                              Type="RangeNavigatorType.StepLine"
                              Width="2" />
    </RangeNavigatorSeriesCollection>

</SfRangeNavigator>

<p>
    <strong>Selected Log Range:</strong>
    @LogRange[0] → @LogRange[1]
</p>

@code {
    public double[] LogRange = new double[] { 1, 2 };

    public List<DataPoint> ExponentialData = new();

    protected override void OnInitialized()
    {
        ExponentialData = GetExponentialData();
    }

    private List<DataPoint> GetExponentialData()
    {
        var data = new List<DataPoint>();
        for (int x = 0; x <= 6; x++)
        {
            data.Add(new DataPoint
            {
                X = x + 1,
                Y = Math.Pow(10, x / 1.0)
            });
        }

        return data;
    }

    public class DataPoint
    {
        public double X { get; set; }
        public double Y { get; set; }
    }
}
```

## Value Binding
Synchronize component values using `Value` property with one-way, two-way bindings, or `Changed` event handler for custom logic.

### One-Way Binding
Implement one-way binding using `Value="@variable"` where the component reads the initial value but doesn't update it:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator Value="@InitialRange" 
                  ValueType="RangeValueType.DateTime">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@Data" XName="Date" YName="Value">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public DateTime[] InitialRange = new DateTime[] 
    { 
        DateTime.Now.AddMonths(-3), 
        DateTime.Now 
    };
}
```

### Two-Way Binding
Implement two-way binding using `@bind-Value="@variable"` for automatic synchronization when users adjust the range:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-Value="@SelectedRange" 
                  ValueType="RangeValueType.DateTime"
                  LabelFormat="MMM yy">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close"
                              Type="RangeNavigatorType.Area">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

<!-- Range automatically updates when user drags thumbs -->
<div class="range-display">
    <h4>Currently Viewing:</h4>
    <p>From: @SelectedRange[0].ToString("MMM dd, yyyy")</p>
    <p>To: @SelectedRange[1].ToString("MMM dd, yyyy")</p>
    <p>Duration: @((SelectedRange[1] - SelectedRange[0]).TotalDays.ToString("N0")) days</p>
</div>

@code {
    public DateTime[] SelectedRange = new DateTime[] 
    { 
        new DateTime(2023, 01, 01), 
        new DateTime(2023, 12, 31) 
    };
}
```

### Event-Based Binding
Handle range changes using `RangeNavigatorEvents` with `Changed` handler and `ChangedEventArgs` parameter for custom processing:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-value="@CurrentRange"
                  ValueType="RangeValueType.DateTime"
                  Height="150px">

    <RangeNavigatorEvents Changed="OnRangeChanged" />

    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@OrderDetails"
                              XName="OrderDate"
                              YName="Amount"
                              Type="RangeNavigatorType.Area" />
    </RangeNavigatorSeriesCollection>

</SfRangeNavigator>

<div class="analytics">
    <h4>Analytics for Selected Period</h4>
    <p><strong>Total Orders:</strong> @TotalOrders</p>
    <p><strong>Total Revenue:</strong> @TotalRevenue.ToString("C")</p>
    <p><strong>Average Order:</strong> @AverageOrder.ToString("C")</p>
</div>

@code {
    public object CurrentRange = new DateTime[]
    {
        DateTime.Today.AddMonths(-1),
        DateTime.Today
    };

    public List<OrderData> OrderDetails = new();

    private int TotalOrders;
    private decimal TotalRevenue;
    private decimal AverageOrder;

    protected override void OnInitialized()
    {
        OrderDetails = GetOrderData();
        DateTime[] range = CurrentRange as DateTime[] ?? new DateTime[2];
        OnRangeChanged(new ChangedEventArgs
        {
            Start = range[0],
            End = range[1]
        });
    }

    private void OnRangeChanged(ChangedEventArgs args)
    {
        if (args.Start is DateTime start && args.End is DateTime end)
        {
            CurrentRange = new DateTime[] { start, end };

            var filteredOrders = OrderDetails
                .Where(o => o.OrderDate >= start && o.OrderDate <= end)
                .ToList();

            TotalOrders = filteredOrders.Count;
            TotalRevenue = filteredOrders.Sum(o => o.Amount);
            AverageOrder = TotalOrders > 0 ? TotalRevenue / TotalOrders : 0;
        }
    }

    private List<OrderData> GetOrderData()
    {
        var data = new List<OrderData>();
        var random = new Random();
        var startDate = DateTime.Today.AddMonths(-6);

        for (int i = 0; i < 180; i++)
        {
            data.Add(new OrderData
            {
                OrderDate = startDate.AddDays(i),
                Amount = Math.Round((decimal)(random.NextDouble() * 500 + 50), 2)
            });
        }

        return data;
    }

    public class OrderData
    {
        public DateTime OrderDate { get; set; }
        public decimal Amount { get; set; }
    }
}
```

## Thumb Dragging
Control thumb interaction using default dragging behavior or `AllowSnapping` property to align with data points.

### Enable Dragging
Enable thumb dragging by default on `SfRangeNavigator` component allowing users to adjust the range:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime">
    <!-- Thumbs are draggable by default -->
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@Data" XName="Date" YName="Value">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>
```

### Disable Dragging
Disable range changes by handling the `Changed` event and preventing updates or using event handlers to lock the range:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator Value="@LockedRange" 
                  ValueType="RangeValueType.DateTime"
                  Changed="PreventRangeChange">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@Data" XName="Date" YName="Value">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public DateTime[] LockedRange = new DateTime[] 
    { 
        new DateTime(2023, 01, 01), 
        new DateTime(2023, 12, 31) 
    };
    
    private void PreventRangeChange(ChangedEventArgs args)
    {
        // Don't update the range
        // Range remains locked
    }
}
```

### Snapping to Data Points
Enable snap behavior using `AllowSnapping="true"` property to align thumbs with data point positions:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  AllowSnapping="true">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@WeeklyData" 
                              XName="Week" 
                              YName="Sales">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public DateTime[] Range;
    
    public List<WeeklyData> WeeklyData = new List<WeeklyData>
    {
        new WeeklyData { Week = new DateTime(2023, 01, 01), Sales = 1000 },
        new WeeklyData { Week = new DateTime(2023, 01, 08), Sales = 1200 },
        new WeeklyData { Week = new DateTime(2023, 01, 15), Sales = 1100 },
        // Thumbs will snap to these exact dates
    };
}
```

## Label Selection
Enable quick range selection by clicking axis labels using `IntervalType` and `LabelFormat` properties for label positioning:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  IntervalType="RangeIntervalType.Months"
                  LabelFormat="MMM">
    <!-- Users can click month labels to select that month -->
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@MonthlyData" 
                              XName="Month" 
                              YName="Revenue">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

<p>Tip: Click any month label to select that entire month!</p>

@code {
    public DateTime[] Range;
    public List<MonthlyData> MonthlyData = GetMonthlyData();
}
```

## Programmatic Range Updates
Modify range programmatically using `Value` property assignments or StateHasChanged() for manual updates.

### Update Range from Code
Change the selected range programmatically by modifying the `Value` property and calling StateHasChanged():

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-Value="@CurrentRange" 
                  ValueType="RangeValueType.DateTime"
                  LabelFormat="MMM yy">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@Data" XName="Date" YName="Value">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

<div class="controls">
    <button @onclick="SelectLastMonth">Last Month</button>
    <button @onclick="SelectLastQuarter">Last Quarter</button>
    <button @onclick="SelectLastYear">Last Year</button>
    <button @onclick="SelectYearToDate">Year to Date</button>
    <button @onclick="SelectAll">All Data</button>
</div>

@code {
    public DateTime[] CurrentRange = new DateTime[] 
    { 
        DateTime.Now.AddMonths(-3), 
        DateTime.Now 
    };
    
    private void SelectLastMonth()
    {
        var now = DateTime.Now;
        CurrentRange = new DateTime[] 
        { 
            now.AddMonths(-1), 
            now 
        };
    }
    
    private void SelectLastQuarter()
    {
        var now = DateTime.Now;
        CurrentRange = new DateTime[] 
        { 
            now.AddMonths(-3), 
            now 
        };
    }
    
    private void SelectLastYear()
    {
        var now = DateTime.Now;
        CurrentRange = new DateTime[] 
        { 
            now.AddYears(-1), 
            now 
        };
    }
    
    private void SelectYearToDate()
    {
        var now = DateTime.Now;
        CurrentRange = new DateTime[] 
        { 
            new DateTime(now.Year, 1, 1), 
            now 
        };
    }
    
    private void SelectAll()
    {
        if (Data.Any())
        {
            CurrentRange = new DateTime[] 
            { 
                Data.Min(d => d.Date), 
                Data.Max(d => d.Date) 
            };
        }
    }
}
```

### Validate Range Selection
Add validation logic in the `Changed` event handler to enforce range constraints and business rules:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator Value="@ValidatedRange" 
                  ValueType="RangeValueType.DateTime"
                  Changed="ValidateAndApplyRange">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@Data" XName="Date" YName="Value">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@if (!string.IsNullOrEmpty(ValidationMessage))
{
    <div class="alert alert-warning">@ValidationMessage</div>
}

@code {
    public DateTime[] ValidatedRange = new DateTime[] 
    { 
        DateTime.Now.AddMonths(-6), 
        DateTime.Now 
    };
    
    private string ValidationMessage = "";
    
    private void ValidateAndApplyRange(ChangedEventArgs args)
    {
        var start = args.Value[0];
        var end = args.Value[1];
        var duration = (end - start).TotalDays;
        
        // Minimum range: 7 days
        if (duration < 7)
        {
            ValidationMessage = "Please select a range of at least 7 days.";
            return;
        }
        
        // Maximum range: 2 years
        if (duration > 730)
        {
            ValidationMessage = "Please select a range of less than 2 years.";
            return;
        }
        
        // Valid range
        ValidationMessage = "";
        ValidatedRange = args.Value;
        StateHasChanged();
    }
}
```

## Initial Range Configuration
Initialize the selected range using `Value` property at component startup with calculated or predefined date ranges.

### Default to Recent Data
Set initial `Value` property to show recent data by calculating offset from current date:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator Value="@RecentRange" 
                  ValueType="RangeValueType.DateTime"
                  LabelFormat="MMM yy">
</SfRangeNavigator>

@code {
    public DateTime[] RecentRange = new DateTime[] 
    { 
        DateTime.Now.AddDays(-30),  // Last 30 days
        DateTime.Now 
    };
}
```

### Default to Full Data Range
Calculate initial `Value` range from data bounds using Min/Max from datasource in OnInitialized():

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator Value="@FullRange" 
                  ValueType="RangeValueType.DateTime"
                  LabelFormat="MMM yy">
</SfRangeNavigator>

@code {
    protected override void OnInitialized()
    {
        if (StockData.Any())
        {
            FullRange = new DateTime[] 
            { 
                StockData.Min(d => d.Date), 
                StockData.Max(d => d.Date) 
            };
        }
    }
    
    public DateTime[] FullRange;
    public List<StockInfo> StockData = GetStockData();
}
```

### Default to Specific Period
Set initial `Value` property to represent a specific business period (quarter, fiscal year, etc.):

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator Value="@QuarterRange" 
                  ValueType="RangeValueType.DateTime"
                  LabelFormat="MMM yy">
</SfRangeNavigator>

@code {
    protected override void OnInitialized()
    {
        // Q1 2023
        QuarterRange = new DateTime[] 
        { 
            new DateTime(2023, 01, 01), 
            new DateTime(2023, 03, 31) 
        };
    }
    
    public DateTime[] QuarterRange;
}
```

## Best Practices
Follow configuration guidelines using appropriate `ValueType`, two-way binding, and data validation for reliable range selection.

### 1. Match Value Type to Data
Ensure `ValueType` property matches your data structure (DateTime, Double, or Logarithmic):

```razor
@using Syncfusion.Blazor.Charts

<!-- DateTime data -->
<SfRangeNavigator Value="@DateRange" ValueType="RangeValueType.DateTime">

<!-- Numeric data -->
<SfRangeNavigator Value="@NumericRange" ValueType="RangeValueType.Double">

<!-- Logarithmic data -->
<SfRangeNavigator Value="@LogRange" ValueType="RangeValueType.Logarithmic">
```

### 2. Sort Data
Sort datasource by X-axis values (Date or numeric) before binding to ensure proper range visualization:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator ValueType="RangeValueType.DateTime"
                  DataSource="SortedData"
                  XName="Date"
                  YName="Value"
                  IntervalType="RangeIntervalType.Days"
                  LabelFormat="dd/MM/yyyy"
                  Height="120">

</SfRangeNavigator>

@code {
    public List<DataPoint> SortedData = RawData
        .OrderBy(d => d.Date)
        .ToList();
}
```

### 3. Use Two-Way Binding
Use `@bind-Value` directive for automatic synchronization between component and variable:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-Value="@Range" ...>

@code {}
```

### 4. Provide Visual Feedback
Display current selection to users by binding to `Value` property and showing range information:

```razor
<p>Viewing: @Range[0].ToShortDateString() to @Range[1].ToShortDateString()</p>
```

## Complete Example
Implement comprehensive range configuration using `@bind-Value`, `Changed` event handler, `ValueType`, and programmatic range updates:

```razor
@page "/range-config-demo"
@using Syncfusion.Blazor.Charts

<div class="container">
    <h3>Sales Data Range Selector</h3>
    
    <SfRangeNavigator @bind-Value="@SelectedRange" 
                      ValueType="RangeValueType.DateTime"
                      LabelFormat="MMM yy"
                      IntervalType="RangeIntervalType.Months"
                      AllowSnapping="true">
            <RangeNavigatorEvents Changed="OnRangeChanged" />
        <RangeNavigatorRangeTooltipSettings Enable="true" DisplayMode="TooltipDisplayMode.Always">
        </RangeNavigatorRangeTooltipSettings>
        <RangeNavigatorSeriesCollection>
            <RangeNavigatorSeries DataSource="@SalesData" 
                                  XName="Date" 
                                  YName="Sales"
                                  Type="RangeNavigatorType.Area">
            </RangeNavigatorSeries>
        </RangeNavigatorSeriesCollection>
    </SfRangeNavigator>
    
    @{
        DateTime[] range = SelectedRange as DateTime[] ?? new DateTime[2];
    }
    <div class="range-info">
        <h4>Selected Period</h4>
        <p>Start: @range[0].ToString("MMMM dd, yyyy")</p>
        <p>End: @range[1].ToString("MMMM dd, yyyy")</p>
        <p>Duration: @((range[1] - range[0]).TotalDays.ToString("N0")) days</p>
        <p>Total Sales: @FilteredTotal.ToString("C")</p>
    </div>
    
    <div class="quick-select">
        <h4>Quick Select</h4>

        <button class="btn btn-sm btn-outline-primary"
                @onclick='() => SetRange(1, "month")'>1M</button>

        <button class="btn btn-sm btn-outline-primary"
                @onclick='() => SetRange(3, "month")'>3M</button>

        <button class="btn btn-sm btn-outline-primary"
                @onclick='() => SetRange(6, "month")'>6M</button>

        <button class="btn btn-sm btn-outline-primary"
                @onclick="SelectYTD">YTD</button>

        <button class="btn btn-sm btn-outline-primary"
                @onclick='() => SetRange(1, "year")'>1Y</button>

        <button class="btn btn-sm btn-outline-primary"
                @onclick="SelectAll">All</button>
    </div>
</div>

@code {
    public class SalesRecord
    {
        public DateTime Date { get; set; }
        public decimal Sales { get; set; }
    }
    
    public object SelectedRange = new DateTime[] 
    { 
        DateTime.Now.AddMonths(-6), 
        DateTime.Now 
    };
    
    public List<SalesRecord> SalesData = GenerateSalesData();
    private decimal FilteredTotal = 0;
    
    protected override void OnInitialized()
    {
        CalculateTotal();
    }
    
    private void OnRangeChanged(ChangedEventArgs args)
    {
        DateTime start = args.Start as DateTime? ?? DateTime.Now;
        DateTime end = args.End as DateTime? ?? DateTime.Now;
        SelectedRange = new DateTime[] { start, end };
        CalculateTotal();
    }
    
    private void CalculateTotal()
    {
        if (SelectedRange is not DateTime[] range || range.Length < 2)
            return;

        DateTime start = range[0];
        DateTime end = range[1];

        FilteredTotal = SalesData
            .Where(s => s.Date >= start && s.Date <= end)
            .Sum(s => s.Sales);
    }
    
    private void SetRange(int value, string unit)
    {
        var end = DateTime.Now;
        var start = unit == "month" 
            ? end.AddMonths(-value) 
            : end.AddYears(-value);
        
        SelectedRange = new DateTime[] { start, end };
        CalculateTotal();
    }
    
    private void SelectYTD()
    {
        var now = DateTime.Now;
        SelectedRange = new DateTime[] 
        { 
            new DateTime(now.Year, 1, 1), 
            now 
        };
        CalculateTotal();
    }
    
    private void SelectAll()
    {
        if (SalesData.Any())
        {
            SelectedRange = new DateTime[] 
            { 
                SalesData.Min(s => s.Date), 
                SalesData.Max(s => s.Date) 
            };
            CalculateTotal();
        }
    }
    
    private static List<SalesRecord> GenerateSalesData()
    {
        var data = new List<SalesRecord>();
        var startDate = DateTime.Now.AddYears(-2);
        var random = new Random();
        
        for (int i = 0; i < 730; i++) // 2 years of daily data
        {
            data.Add(new SalesRecord
            {
                Date = startDate.AddDays(i),
                Sales = 10000 + random.Next(-2000, 3000)
            });
        }
        
        return data;
    }
}

<style>
    .container {
        padding: 20px;
    }
    
    .range-info {
        margin-top: 20px;
        padding: 15px;
        background-color: #f8f9fa;
        border-radius: 5px;
    }
    
    .quick-select {
        margin-top: 20px;
    }
    
    .quick-select button {
        margin-right: 10px;
        margin-bottom: 10px;
    }
</style>
```

