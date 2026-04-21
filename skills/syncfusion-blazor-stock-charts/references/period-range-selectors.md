# Period and Range Selectors Reference

This reference guide covers the Period Selector and Range Selector features in Syncfusion Blazor Stock Chart, enabling users to select specific time periods or custom date ranges for data analysis.

## Table of Contents

- [Overview](#overview)
- [Period Selector](#period-selector)
  - [Default Periods](#default-periods)
  - [Custom Period Configuration](#custom-period-configuration)
  - [IntervalType Options](#intervaltype-options)
  - [Setting Selected Period](#setting-selected-period)
  - [Visibility Control](#visibility-control)
- [Range Selector](#range-selector)
  - [Enabling Range Selector](#enabling-range-selector)
  - [Range Selection Methods](#range-selection-methods)
  - [Customizing Range Navigator](#customizing-range-navigator)
- [When to Use](#when-to-use)
- [Complete Examples](#complete-examples)
- [Common Issues and Troubleshooting](#common-issues-and-troubleshooting)
- [Best Practices](#best-practices)

## Overview

The Stock Chart provides two powerful navigation controls using the `EnablePeriodSelector` and `EnableSelector` properties:

- **Period Selector**: Predefined buttons for quick time period selection (1M, 3M, 6M, YTD, 1Y, All)
- **Range Selector**: Interactive slider for selecting custom date ranges by dragging thumbs

Both selectors are enabled by default and work together to provide flexible data filtering and analysis capabilities.

## Period Selector

The Period Selector displays predefined time period buttons that allow users to quickly filter chart data using the `StockChartPeriods` collection.

### Default Periods

By default, the Stock Chart includes standard period buttons using the `StockChartPeriod` component with `IntervalType` and `Interval` properties. You can customize these using the `StockChartPeriods` collection:

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL Stock Price">
    <StockChartPeriods>
        <StockChartPeriod IntervalType=RangeIntervalType.Minutes Interval="1" Text='1m'></StockChartPeriod>
        <StockChartPeriod IntervalType=RangeIntervalType.Minutes Interval="30" Text='30m'></StockChartPeriod>
        <StockChartPeriod IntervalType=RangeIntervalType.Hours Interval="1" Text='1h'></StockChartPeriod>
        <StockChartPeriod IntervalType=RangeIntervalType.Hours Interval="12" Text='12h' Selected="true"></StockChartPeriod>
        <StockChartPeriod Text="1D"></StockChartPeriod>
    </StockChartPeriods>

    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockDetails" Type="ChartSeriesType.Candle" XName="Date" High="High" Low="Low" Open="Open" Close="Close" Name="google"></StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public class ChartData
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
        public double High { get; set; }
        public double Volume { get; set; }
    }

    public List<ChartData> StockDetails = new List<ChartData>
    {
        new ChartData { Date = new DateTime(2012, 04, 02), Open = 85.9757, High = 90.6657, Low = 85.7685, Close = 90.5257, Volume = 660187068 },
        new ChartData { Date = new DateTime(2012, 04, 09), Open = 89.4471, High = 92, Low = 86.2157, Close = 86.4614, Volume = 912634864 },
        new ChartData { Date = new DateTime(2012, 04, 16), Open = 87.1514, High = 88.6071, Low = 81.4885, Close = 81.8543, Volume = 1221746066 },
        new ChartData { Date = new DateTime(2012, 04, 23), Open = 81.5157, High = 88.2857, Low = 79.2857, Close = 86.1428, Volume = 965935749 },
        new ChartData { Date = new DateTime(2012, 04, 30), Open = 85.4, High = 85.4857, Low = 80.7385, Close = 80.75, Volume = 615249365 },
        new ChartData { Date = new DateTime(2012, 05, 07), Open = 80.2143, High = 82.2685, Low = 79.8185, Close = 80.9585, Volume = 541742692 },
        new ChartData { Date = new DateTime(2012, 05, 14), Open = 80.3671, High = 81.0728, Low = 74.5971, Close = 75.7685, Volume = 708126233 },
        new ChartData { Date = new DateTime(2012, 05, 21), Open = 76.3571, High = 82.3571, Low = 76.2928, Close = 80.3271, Volume = 682076215 },
        new ChartData { Date = new DateTime(2012, 05, 28), Open = 81.5571, High = 83.0714, Low = 80.0743, Close = 80.1414, Volume = 480059584 }
   };
}
```

### Custom Period Configuration

Create custom period buttons for intraday, weekly, or any time interval using the `Text`, `IntervalType`, and `Interval` properties:

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Intraday Trading - AAPL">
    <StockChartPrimaryXAxis ValueType="Syncfusion.Blazor.Charts.ValueType.DateTime">
    </StockChartPrimaryXAxis>

    <StockChartPeriods>
        <StockChartPeriod Text="1m"
                          IntervalType="RangeIntervalType.Minutes"
                          Interval="1" />
        <StockChartPeriod Text="5m"
                          IntervalType="RangeIntervalType.Minutes"
                          Interval="5" />
        <StockChartPeriod Text="15m"
                          IntervalType="RangeIntervalType.Minutes"
                          Interval="15" />
        <StockChartPeriod Text="30m"
                          IntervalType="RangeIntervalType.Minutes"
                          Interval="30" />
        <StockChartPeriod Text="1h"
                          IntervalType="RangeIntervalType.Hours"
                          Interval="1" />
        <StockChartPeriod Text="1D"
                          IntervalType="RangeIntervalType.Days"
                          Interval="1"
                          Selected="true" />
    </StockChartPeriods>

    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@IntradayPrices"
                          Type="ChartSeriesType.Line"
                          XName="Time"
                          YName="Price"
                          Name="AAPL">
        </StockChartSeries>
    </StockChartSeriesCollection>

</SfStockChart>

@code {
    public class IntradayPrice
    {
        public DateTime Time { get; set; }
        public double Price { get; set; }
    }

    public List<IntradayPrice> IntradayPrices = new()
    {
        new IntradayPrice { Time = new DateTime(2024, 01, 15, 9, 30, 0), Price = 182.10 },
        new IntradayPrice { Time = new DateTime(2024, 01, 15, 9, 35, 0), Price = 182.45 },
        new IntradayPrice { Time = new DateTime(2024, 01, 15, 9, 40, 0), Price = 182.20 },
        new IntradayPrice { Time = new DateTime(2024, 01, 15, 9, 45, 0), Price = 182.80 },
        new IntradayPrice { Time = new DateTime(2024, 01, 15, 9, 50, 0), Price = 183.10 },
        new IntradayPrice { Time = new DateTime(2024, 01, 15, 9, 55, 0), Price = 182.95 },
        new IntradayPrice { Time = new DateTime(2024, 01, 15, 10, 00, 0), Price = 183.40 },
        new IntradayPrice { Time = new DateTime(2024, 01, 15, 10, 05, 0), Price = 183.75 },
        new IntradayPrice { Time = new DateTime(2024, 01, 15, 10, 10, 0), Price = 183.55 },
        new IntradayPrice { Time = new DateTime(2024, 01, 15, 10, 15, 0), Price = 184.00 }
    };
}
```

### IntervalType Options

The `IntervalType` property supports the following values:

| IntervalType | Description | Use Case |
|-------------|-------------|----------|
| `Auto` | Automatically determines interval | General purpose |
| `Years` | Interval in years | Long-term analysis (1Y, 5Y) |
| `Months` | Interval in months | Medium-term trends (1M, 3M, 6M) |
| `Weeks` | Interval in weeks | Short-term patterns |
| `Days` | Interval in days | Daily trading analysis |
| `Hours` | Interval in hours | Intraday hour-level |
| `Minutes` | Interval in minutes | High-frequency trading |
| `Seconds` | Interval in seconds | Tick-level data |
| `Quarter` | Quarterly intervals | Business quarter analysis |

### Setting Selected Period

Mark a period as selected by default using the `Selected` property on the `StockChartPeriod` component:

```cshtml
<StockChartPeriods>
    <StockChartPeriod Text="1M" IntervalType="RangeIntervalType.Months" Interval="1"></StockChartPeriod>
    <StockChartPeriod Text="3M" IntervalType="RangeIntervalType.Months" Interval="3"></StockChartPeriod>
    <StockChartPeriod Text="6M" IntervalType="RangeIntervalType.Months" Interval="6" Selected="true"></StockChartPeriod>
    <StockChartPeriod Text="1Y" IntervalType="RangeIntervalType.Years" Interval="1"></StockChartPeriod>
</StockChartPeriods>
```

### Visibility Control

Toggle the Period Selector visibility using the `EnablePeriodSelector` property on the `SfStockChart` component:

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Stock Price" EnablePeriodSelector="false">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle" 
                         XName="Date" High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public class ChartData
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
        public double High { get; set; }
        public double Volume { get; set; }
    }

    public List<ChartData> StockData = new List<ChartData>
    {
        new ChartData { Date = new DateTime(2012, 04, 02), Open = 85.9757, High = 90.6657, Low = 85.7685, Close = 90.5257, Volume = 660187068 },
        new ChartData { Date = new DateTime(2012, 04, 09), Open = 89.4471, High = 92, Low = 86.2157, Close = 86.4614, Volume = 912634864 },
        new ChartData { Date = new DateTime(2012, 04, 16), Open = 87.1514, High = 88.6071, Low = 81.4885, Close = 81.8543, Volume = 1221746066 },
        new ChartData { Date = new DateTime(2012, 04, 23), Open = 81.5157, High = 88.2857, Low = 79.2857, Close = 86.1428, Volume = 965935749 },
        new ChartData { Date = new DateTime(2012, 04, 30), Open = 85.4, High = 85.4857, Low = 80.7385, Close = 80.75, Volume = 615249365 },
        new ChartData { Date = new DateTime(2012, 05, 07), Open = 80.2143, High = 82.2685, Low = 79.8185, Close = 80.9585, Volume = 541742692 },
        new ChartData { Date = new DateTime(2012, 05, 14), Open = 80.3671, High = 81.0728, Low = 74.5971, Close = 75.7685, Volume = 708126233 },
        new ChartData { Date = new DateTime(2012, 05, 21), Open = 76.3571, High = 82.3571, Low = 76.2928, Close = 80.3271, Volume = 682076215 },
        new ChartData { Date = new DateTime(2012, 05, 28), Open = 81.5571, High = 83.0714, Low = 80.0743, Close = 80.1414, Volume = 480059584 }
   };
}
```

## Range Selector

The Range Selector provides an interactive slider at the bottom of the chart for precise date range selection using the `EnableSelector` property.

### Enabling Range Selector

The Range Selector is enabled by default. Control it with the `EnableSelector` property on the `SfStockChart` component:

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL Stock Price" EnableSelector="true">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle" 
                         XName="Date" High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public class ChartData
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
        public double High { get; set; }
        public double Volume { get; set; }
    }

    public List<ChartData> StockData = new List<ChartData>
    {
        new ChartData { Date = new DateTime(2012, 04, 02), Open = 85.9757, High = 90.6657, Low = 85.7685, Close = 90.5257, Volume = 660187068 },
        new ChartData { Date = new DateTime(2012, 04, 09), Open = 89.4471, High = 92, Low = 86.2157, Close = 86.4614, Volume = 912634864 },
        new ChartData { Date = new DateTime(2012, 04, 16), Open = 87.1514, High = 88.6071, Low = 81.4885, Close = 81.8543, Volume = 1221746066 },
        new ChartData { Date = new DateTime(2012, 04, 23), Open = 81.5157, High = 88.2857, Low = 79.2857, Close = 86.1428, Volume = 965935749 },
        new ChartData { Date = new DateTime(2012, 04, 30), Open = 85.4, High = 85.4857, Low = 80.7385, Close = 80.75, Volume = 615249365 },
        new ChartData { Date = new DateTime(2012, 05, 07), Open = 80.2143, High = 82.2685, Low = 79.8185, Close = 80.9585, Volume = 541742692 },
        new ChartData { Date = new DateTime(2012, 05, 14), Open = 80.3671, High = 81.0728, Low = 74.5971, Close = 75.7685, Volume = 708126233 },
        new ChartData { Date = new DateTime(2012, 05, 21), Open = 76.3571, High = 82.3571, Low = 76.2928, Close = 80.3271, Volume = 682076215 },
        new ChartData { Date = new DateTime(2012, 05, 28), Open = 81.5571, High = 83.0714, Low = 80.0743, Close = 80.1414, Volume = 480059584 }
   };
}
```

### Range Selection Methods

Users can select ranges in three ways using the range selector control:

1. **Dragging Thumbs**: Click and drag the left or right thumb to adjust the range
2. **Tapping Labels**: Click on axis labels to set range boundaries
3. **Date Range Button**: Use the built-in date picker for precise date selection

### Customizing Range Navigator

Customize the range navigator appearance and behavior using properties like `Width`, `Opacity`, and axis properties:

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL Stock Price" EnableSelector="true">
    <StockChartPrimaryXAxis>
        <StockChartAxisMajorGridLines Width="0"></StockChartAxisMajorGridLines>
    </StockChartPrimaryXAxis>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Area" 
                         XName="Date" YName="Close" Opacity="0.5">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public class StockChartData
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double High { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
        public double Volume { get; set; }
    }

    public List<StockChartData> StockData = new List<StockChartData>
    {
        new StockChartData { Date = new DateTime(2012, 01, 01), Open = 75.50, High = 78.00, Low = 74.20, Close = 77.30, Volume = 550000000 },
        new StockChartData { Date = new DateTime(2012, 02, 01), Open = 77.30, High = 81.50, Low = 76.10, Close = 80.20, Volume = 620000000 },
        new StockChartData { Date = new DateTime(2012, 03, 01), Open = 80.20, High = 85.60, Low = 79.50, Close = 84.10, Volume = 710000000 },
        new StockChartData { Date = new DateTime(2012, 04, 01), Open = 84.10, High = 88.90, Low = 82.30, Close = 86.40, Volume = 680000000 },
        new StockChartData { Date = new DateTime(2012, 05, 01), Open = 86.40, High = 90.20, Low = 84.70, Close = 88.50, Volume = 720000000 }
    };
}
```

## When to Use

### Use Period Selector When:
- Providing quick access to common time periods (1M, 3M, 6M, YTD, 1Y, All)
- Users need to compare data across standard intervals
- Building dashboards for financial or business analytics
- Supporting intraday trading analysis with minute/hour intervals
- Users are familiar with standard period-based navigation

### Use Range Selector When:
- Users need precise control over custom date ranges
- Analyzing specific events or time windows
- Comparing arbitrary periods not covered by predefined buttons
- Working with large datasets requiring focused examination
- Users prefer drag-based interactive selection

### Use Both Together When:
- Offering maximum flexibility (recommended for most cases)
- Supporting both quick access and detailed analysis
- Users have varying levels of technical expertise
- Building comprehensive financial analysis tools

## Complete Examples

### Financial Dashboard with Combined Selectors

This example demonstrates using `EnablePeriodSelector` and `EnableSelector` properties together:

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL Stock Performance Dashboard" 
              EnablePeriodSelector="true" 
              EnableSelector="true">
    
    <StockChartPeriods>
        <StockChartPeriod Text="1M" IntervalType="RangeIntervalType.Months" Interval="1"></StockChartPeriod>
        <StockChartPeriod Text="3M" IntervalType="RangeIntervalType.Months" Interval="3"></StockChartPeriod>
        <StockChartPeriod Text="6M" IntervalType="RangeIntervalType.Months" Interval="6" Selected="true"></StockChartPeriod>
        <StockChartPeriod Text="YTD"></StockChartPeriod>
        <StockChartPeriod Text="1Y" IntervalType="RangeIntervalType.Years" Interval="1"></StockChartPeriod>
        <StockChartPeriod Text="All"></StockChartPeriod>
    </StockChartPeriods>
    
    <StockChartPrimaryXAxis>
        <StockChartAxisMajorGridLines Color="#E0E0E0"></StockChartAxisMajorGridLines>
    </StockChartPrimaryXAxis>
    
    <StockChartPrimaryYAxis>
        <StockChartAxisLineStyle Color="#E0E0E0"></StockChartAxisLineStyle>
        <StockChartAxisMajorTickLines Color="#E0E0E0"></StockChartAxisMajorTickLines>
    </StockChartPrimaryYAxis>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle" 
                         XName="Date" High="High" Low="Low" Open="Open" 
                         Close="Close" Volume="Volume" Name="AAPL">
        </StockChartSeries>
    </StockChartSeriesCollection>
    
    <StockChartTooltipSettings Enable="true"></StockChartTooltipSettings>
    <StockChartCrosshairSettings Enable="true"></StockChartCrosshairSettings>
</SfStockChart>

@code {
    public class StockChartData
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double High { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
        public double Volume { get; set; }
    }

    public List<StockChartData> StockData { get; set; } = GenerateStockData();

    private static List<StockChartData> GenerateStockData()
    {
        var data = new List<StockChartData>();
        var random = new Random();
        var startDate = new DateTime(2021, 01, 01);
        double price = 100.0;

        for (int i = 0; i < 365; i++)
        {
            var change = (random.NextDouble() - 0.5) * 5;
            var open = price;
            var close = price + change;
            var high = Math.Max(open, close) + random.NextDouble() * 2;
            var low = Math.Min(open, close) - random.NextDouble() * 2;

            data.Add(new StockChartData
            {
                Date = startDate.AddDays(i),
                Open = Math.Round(open, 2),
                High = Math.Round(high, 2),
                Low = Math.Round(low, 2),
                Close = Math.Round(close, 2),
                Volume = random.Next(500000000, 1000000000)
            });

            price = close;
        }

        return data;
    }
}
```

### Crypto Trading Chart with Intraday Periods

This example uses custom `IntervalType` and `Interval` properties with minute-level periods:

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="BTC/USD - 24H Trading" 
              EnablePeriodSelector="true" 
              EnableSelector="true"
              Theme="Theme.MaterialDark">
    
    <StockChartPeriods>
        <StockChartPeriod Text="5m" IntervalType="RangeIntervalType.Minutes" Interval="5"></StockChartPeriod>
        <StockChartPeriod Text="15m" IntervalType="RangeIntervalType.Minutes" Interval="15"></StockChartPeriod>
        <StockChartPeriod Text="30m" IntervalType="RangeIntervalType.Minutes" Interval="30" Selected="true"></StockChartPeriod>
        <StockChartPeriod Text="1H" IntervalType="RangeIntervalType.Hours" Interval="1"></StockChartPeriod>
        <StockChartPeriod Text="4H" IntervalType="RangeIntervalType.Hours" Interval="4"></StockChartPeriod>
        <StockChartPeriod Text="1D" IntervalType="RangeIntervalType.Days" Interval="1"></StockChartPeriod>
    </StockChartPeriods>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@CryptoData" Type="ChartSeriesType.HiloOpenClose" 
                         XName="Time" High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
    
    <StockChartCrosshairSettings Enable="true"></StockChartCrosshairSettings>
    <StockChartTooltipSettings Enable="true" Format="Time: ${point.x}<br/>Price: $${point.close}"></StockChartTooltipSettings>
</SfStockChart>

@code {
    public class CryptoDataPoint
    {
        public DateTime Time { get; set; }
        public double Open { get; set; }
        public double High { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
    }

    public List<CryptoDataPoint> CryptoData { get; set; } = new List<CryptoDataPoint>();

    protected override void OnInitialized()
    {
        CryptoData = GenerateCryptoData();
    }

    private List<CryptoDataPoint> GenerateCryptoData()
    {
        var data = new List<CryptoDataPoint>();
        var random = new Random();
        var startTime = DateTime.Today;
        double price = 50000.0;

        for (int i = 0; i < 288; i++) // 24 hours in 5-minute intervals
        {
            var change = (random.NextDouble() - 0.5) * 500;
            var open = price;
            var close = price + change;
            var high = Math.Max(open, close) + random.NextDouble() * 100;
            var low = Math.Min(open, close) - random.NextDouble() * 100;

            data.Add(new CryptoDataPoint
            {
                Time = startTime.AddMinutes(i * 5),
                Open = Math.Round(open, 2),
                High = Math.Round(high, 2),
                Low = Math.Round(low, 2),
                Close = Math.Round(close, 2)
            });

            price = close;
        }

        return data;
    }
}
```

## Common Issues and Troubleshooting

### Issue 1: Period Selector Buttons Not Visible

**Problem**: Period selector buttons don't appear on the chart.

**Solution**: Ensure `EnablePeriodSelector` is set to `true` (default) and periods are defined using `StockChartPeriods`:

```cshtml
<SfStockChart EnablePeriodSelector="true">
    <StockChartPeriods>
        <StockChartPeriod Text="1M" IntervalType="RangeIntervalType.Months" Interval="1"></StockChartPeriod>
        <!-- Add more periods -->
    </StockChartPeriods>
</SfStockChart>
```

### Issue 2: Selected Period Not Working

**Problem**: Setting the `Selected` property to `"true"` doesn't highlight the expected period.

**Solution**: Only one `StockChartPeriod` should have `Selected="true"`. If multiple periods have this property, the last one takes precedence:

```cshtml
<StockChartPeriods>
    <StockChartPeriod Text="1M" IntervalType="RangeIntervalType.Months" Interval="1"></StockChartPeriod>
    <StockChartPeriod Text="6M" IntervalType="RangeIntervalType.Months" Interval="6" Selected="true"></StockChartPeriod>
    <StockChartPeriod Text="1Y" IntervalType="RangeIntervalType.Years" Interval="1"></StockChartPeriod>
</StockChartPeriods>
```

### Issue 3: Range Selector Not Interactive

**Problem**: Cannot drag the range selector thumbs.

**Solution**: Verify `EnableSelector="true"` on the `SfStockChart` component and check for CSS conflicts:

```cshtml
<SfStockChart EnableSelector="true">
    <!-- Series configuration -->
</SfStockChart>
```

### Issue 4: YTD Period Not Calculating Correctly

**Problem**: "YTD" button doesn't show year-to-date data correctly.

**Solution**: YTD is automatically calculated. Ensure your data includes the current year:

```cshtml
<StockChartPeriod Text="YTD"></StockChartPeriod>
```

The component automatically calculates from January 1st of the current year to the present date.

### Issue 5: Intraday Periods Showing No Data

**Problem**: Minute or hour-based periods show empty chart.

**Solution**: Verify your data source includes timestamps with time components, not just dates:

```csharp
new StockChartData 
{ 
    Date = new DateTime(2024, 03, 19, 14, 30, 0), // Include hours and minutes
    Open = 100.0,
    High = 102.0,
    Low = 99.5,
    Close = 101.0
}
```

## Best Practices

1. **Provide Sensible Defaults**
   - Set the most commonly used period with `Selected="true"` on `StockChartPeriod`
   - For financial charts, 6M or YTD are good defaults
   - For intraday trading, 1H or 4H work well

2. **Match Periods to Data Granularity**
   - Don't offer minute-level `IntervalType` periods if data is daily
   - Ensure your data covers the longest period offered

3. **Label Periods Clearly**
   - Use standard abbreviations (1M, 3M, 6M, YTD, 1Y, All) with the `Text` property
   - For custom periods, use clear, short text values

4. **Combine Period and Range Selectors**
   - Enable both `EnablePeriodSelector="true"` and `EnableSelector="true"` unless there's a specific reason not to
   - Provides flexibility for different user workflows

5. **Consider Performance**
   - Large datasets may load slowly with "All" period
   - Consider pagination or data aggregation for very large ranges

6. **Mobile Considerations**
   - Period buttons using `EnablePeriodSelector` are touch-friendly by default
   - Range selector thumbs with `EnableSelector` work well on touch devices
   - Test on mobile to ensure usability

7. **Accessibility**
   - Period selector buttons are keyboard navigable
   - Provide clear visual feedback for selected periods
   - Ensure sufficient color contrast

This comprehensive reference covers both Period and Range Selectors, providing developers with all necessary information to implement flexible time-based navigation in Syncfusion Blazor Stock Charts.
