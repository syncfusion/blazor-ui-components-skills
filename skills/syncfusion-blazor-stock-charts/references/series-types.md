# Series Types

## Table of Contents
- [Overview](#overview)
- [Line Series](#line-series)
- [Spline Series](#spline-series)
- [Candle Series](#candle-series)
- [Hollow Candle Rendering](#hollow-candle-rendering)
- [HiloOpenClose Series](#hiloopenclose-series)
- [Hilo Series](#hilo-series)
- [Series Selector](#series-selector)
- [Choosing the Right Series Type](#choosing-the-right-series-type)

## Overview

The Stock Chart supports six series types optimized for financial data visualization. Each series type presents data differently to suit various analysis needs. Users can switch between series types at runtime using the built-in series selector.

**Supported Series Types:**
- **Line** - Simple trend visualization with connected points
- **Spline** - Smoothed curve for trend analysis
- **Candle** - Traditional candlestick chart (OHLC)
- **HiloOpenClose** - Bar chart showing OHLC with ticks
- **Hilo** - Simplified high-low range visualization

> Hollow candle rendering is available through candle configuration such as `EnableSolidCandles="false"`.

## Line Series

Line series connect data points with straight lines, ideal for visualizing price trends over time. Use `Type="ChartSeriesType.Line"` property with `XName` for dates and `YName` for single value data to create line series.

### When to Use

- Comparing multiple stocks on the same chart
- Showing overall price trends without OHLC details
- Displaying single value metrics (close price, volume)
- Creating performance comparison charts

### Implementation

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Stock Price Trend">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Line" 
                          XName="Date" 
                          YName="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 175.5 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Close = 179.3 },
        new StockInfo { Date = new DateTime(2024, 01, 15), Close = 183.7 },
        new StockInfo { Date = new DateTime(2024, 01, 22), Close = 188.2 },
        new StockInfo { Date = new DateTime(2024, 01, 29), Close = 191.5 }
    };
}
```

### Data Requirements

| Property | Required | Purpose |
|----------|----------|---------|
| `XName` | Yes | Date/time field |
| `YName` | Yes | Single value (typically Close price) |

### Customization

```razor
<StockChartSeries DataSource="@StockData" 
                  Type="ChartSeriesType.Line" 
                  XName="Date" 
                  YName="Close"
                  Fill="#FF5733"
                  Width="2"
                  DashArray="5,5">
</StockChartSeries>
```

## Spline Series

Spline series create smooth curves through data points, providing a polished visual representation of trends. Use `Type="ChartSeriesType.Spline"` property with `XName` for dates and `YName` for single value data to create spline series.

### When to Use

- Presenting data in reports or presentations
- Emphasizing smooth trends over exact values
- Creating visually appealing dashboards
- Showing momentum or moving average overlays

### Implementation

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Stock Price - Smoothed Trend">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Spline" 
                          XName="Date" 
                          YName="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 175.5 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Close = 179.3 },
        new StockInfo { Date = new DateTime(2024, 01, 15), Close = 183.7 },
        new StockInfo { Date = new DateTime(2024, 01, 22), Close = 188.2 }
    };
}
```

### Data Requirements

Same as Line series - requires `XName` (Date) and `YName` (single value).

## Candle Series

Candlestick charts are the most popular format for stock price visualization, showing open, high, low, and close (OHLC) values for each period. Use `Type="ChartSeriesType.Candle"` property with `XName`, `Open`, `High`, `Low`, and `Close` properties to display candlestick data. The `BullFillColor` and `BearFillColor` properties control the visualization colors for bullish and bearish candles.

### When to Use

- Traditional stock chart visualization
- Technical analysis and pattern recognition
- Showing price action and volatility
- Identifying support/resistance levels

### Visual Representation

- **Green/Bullish Candle**: Close > Open (price increased)
- **Red/Bearish Candle**: Close < Open (price decreased)
- **Body**: Shows range between Open and Close
- **Wicks/Shadows**: Show High and Low extremes

### Implementation

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL - Candlestick Chart">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Candle" 
                          XName="Date" 
                          High="High" 
                          Low="Low" 
                          Open="Open" 
                          Close="Close"
                          BearFillColor="#FF4136"
                          BullFillColor="#2ECC40">
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

### Data Requirements

| Property | Required | Purpose |
|----------|----------|---------|
| `XName` | Yes | Date/time field |
| `Open` | Yes | Opening price |
| `High` | Yes | Highest price |
| `Low` | Yes | Lowest price |
| `Close` | Yes | Closing price |
| `Volume` | No | Trading volume (optional) |

### Customization Options

```razor
<StockChartSeries DataSource="@StockData" 
                  Type="ChartSeriesType.Candle" 
                  XName="Date" 
                  High="High" Low="Low" Open="Open" Close="Close"
                  BearFillColor="#E74C3C"
                  BullFillColor="#27AE60"
                  EnableSolidCandles="true">
</StockChartSeries>
```

## Hollow Candle Rendering

Hollow candles provide additional visual distinction based on price movement relative to the previous period, while still using the candle series type. Set the `EnableSolidCandles="false"` property to enable hollow candle rendering for advanced technical analysis visualization.

### When to Use

- Advanced technical analysis
- Identifying trend continuation vs reversal
- Enhanced visual clarity for price movements
- Professional trading applications

### Visual Representation

- **Hollow (Unfilled)**: Close > previous Close (continued uptrend)
- **Filled Green**: Close > Open but Close < previous Close (potential reversal)
- **Filled Red**: Close < Open (downtrend)

### Implementation

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Stock Price - Hollow Candles">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Candle" 
                          XName="Date" 
                          High="High" 
                          Low="Low" 
                          Open="Open" 
                          Close="Close"
                          EnableSolidCandles="false">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    // Use same data structure as Candle series
}
```

**Key Difference:** Set `EnableSolidCandles="false"` to enable hollow candle rendering.

## HiloOpenClose Series

HiloOpenClose (OHLC) series display price data as vertical bars with horizontal ticks indicating open and close prices. Use `Type="ChartSeriesType.HiloOpenClose"` property with `XName`, `High`, `Low`, `Open`, and `Close` properties to create OHLC bar representations of stock data.

### When to Use

- Compact visualization with less visual weight than candles
- Financial reports and publications
- Comparing multiple securities with less clutter
- When screen space is limited

### Visual Representation

- **Vertical Line**: Connects High and Low prices
- **Left Tick**: Opening price
- **Right Tick**: Closing price
- **Color**: Green if Close > Open, Red if Close < Open

### Implementation

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Stock Price - OHLC Bars">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.HiloOpenClose" 
                          XName="Date" 
                          High="High" 
                          Low="Low" 
                          Open="Open" 
                          Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    // Use same OHLC data structure as Candle series
}
```

### Data Requirements

Same as Candle series - requires Open, High, Low, Close values.

## Hilo Series

Hilo series provide simplified visualization showing only the high-low price range without open/close indicators. Use `Type="ChartSeriesType.Hilo"` property with `XName`, `High`, and `Low` properties to create simplified price range visualizations.

### When to Use

- Showing price volatility or range
- Comparing daily/weekly ranges
- When open/close details aren't critical
- Creating range charts or volatility indicators

### Implementation

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Stock Price Range">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Hilo" 
                          XName="Date" 
                          High="High" 
                          Low="Low" 
                          Open="Open" 
                          Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public class StockChartData
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
        public double High { get; set; }
        public double Volume { get; set; }
    }

    public List<StockChartData> StockData = new List<StockChartData>
    {
        new StockChartData { Date = new DateTime(2012, 04, 02), Open = 85.9757, High = 90.6657, Low = 85.7685, Close = 90.5257, Volume = 660187068 },
        new StockChartData { Date = new DateTime(2012, 04, 09), Open = 89.4471, High = 92, Low = 86.2157, Close = 86.4614, Volume = 912634864 },
        new StockChartData { Date = new DateTime(2012, 04, 16), Open = 87.1514, High = 88.6071, Low = 81.4885, Close = 81.8543, Volume = 1221746066 },
        new StockChartData { Date = new DateTime(2012, 04, 23), Open = 81.5157, High = 88.2857, Low = 79.2857, Close = 86.1428, Volume = 965935749 },
        new StockChartData { Date = new DateTime(2012, 04, 30), Open = 85.4, High = 85.4857, Low = 80.7385, Close = 80.75, Volume = 615249365 },
        new StockChartData { Date = new DateTime(2012, 05, 07), Open = 80.2143, High = 82.2685, Low = 79.8185, Close = 80.9585, Volume = 541742692 },
        new StockChartData { Date = new DateTime(2012, 05, 14), Open = 80.3671, High = 81.0728, Low = 74.5971, Close = 75.7685, Volume = 708126233 },
        new StockChartData { Date = new DateTime(2012, 05, 21), Open = 76.3571, High = 82.3571, Low = 76.2928, Close = 80.3271, Volume = 682076215 },
        new StockChartData { Date = new DateTime(2012, 05, 28), Open = 81.5571, High = 83.0714, Low = 80.0743, Close = 80.1414, Volume = 480059584 }
    };
}
```

### Data Requirements

| Property | Required | Purpose |
|----------|----------|---------|
| `XName` | Yes | Date/time field |
| `High` | Yes | Highest price |
| `Low` | Yes | Lowest price |

**Note:** Open and Close properties are not required for Hilo series.

## Series Selector

The series selector allows users to switch between series types at runtime without reloading data. Use the `SeriesType` property to define available series types and set `EnableSelector="true"` to enable the series selector dropdown for users.

### Enabling Series Selector

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Stock Price" 
              SeriesType="@SeriesTypes" 
              EnableSelector="true">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Candle" 
                          XName="Date" 
                          High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public List<ChartSeriesType> SeriesTypes = new List<ChartSeriesType>
    {
        ChartSeriesType.Line,
        ChartSeriesType.Spline,
        ChartSeriesType.Candle,
        ChartSeriesType.HiloOpenClose,
        ChartSeriesType.Hilo
    };

    public class StockChartData
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
        public double High { get; set; }
        public double Volume { get; set; }
    }

    public List<StockChartData> StockData = new List<StockChartData>
    {
        new StockChartData { Date = new DateTime(2012, 04, 02), Open = 85.9757, High = 90.6657, Low = 85.7685, Close = 90.5257, Volume = 660187068 },
        new StockChartData { Date = new DateTime(2012, 04, 09), Open = 89.4471, High = 92, Low = 86.2157, Close = 86.4614, Volume = 912634864 },
        new StockChartData { Date = new DateTime(2012, 04, 16), Open = 87.1514, High = 88.6071, Low = 81.4885, Close = 81.8543, Volume = 1221746066 },
        new StockChartData { Date = new DateTime(2012, 04, 23), Open = 81.5157, High = 88.2857, Low = 79.2857, Close = 86.1428, Volume = 965935749 },
        new StockChartData { Date = new DateTime(2012, 04, 30), Open = 85.4, High = 85.4857, Low = 80.7385, Close = 80.75, Volume = 615249365 },
        new StockChartData { Date = new DateTime(2012, 05, 07), Open = 80.2143, High = 82.2685, Low = 79.8185, Close = 80.9585, Volume = 541742692 },
        new StockChartData { Date = new DateTime(2012, 05, 14), Open = 80.3671, High = 81.0728, Low = 74.5971, Close = 75.7685, Volume = 708126233 },
        new StockChartData { Date = new DateTime(2012, 05, 21), Open = 76.3571, High = 82.3571, Low = 76.2928, Close = 80.3271, Volume = 682076215 },
        new StockChartData { Date = new DateTime(2012, 05, 28), Open = 81.5571, High = 83.0714, Low = 80.0743, Close = 80.1414, Volume = 480059584 }
    };
}
```

### Limiting Available Series Types

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Stock Price" 
              SeriesType="@SeriesTypes" 
              EnableSelector="true">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="SeriesTypes[0]" 
                          XName="Date" 
                          High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
        <StockChartSeries DataSource="@StockData1" 
                          Type="SeriesTypes[1]" 
                          XName="Date" 
                          High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>
@code {
    // Only show Candle and Line options
    public List<ChartSeriesType> SeriesTypes = new List<ChartSeriesType>
    {
        ChartSeriesType.Candle,
        ChartSeriesType.Line
    };
}
```

### Disabling Series Selector

```razor
<SfStockChart Title="Stock Price" EnableSelector="false">
    <!-- Chart will use only the specified series type -->
</SfStockChart>
```

## Choosing the Right Series Type

Select the appropriate series type based on your data availability and visualization requirements using the decision matrix below.

### Decision Matrix

| Use Case | Recommended Series | Reason |
|----------|-------------------|--------|
| Technical analysis | Candle / Hollow Candle | Shows OHLC pattern recognition |
| Trend comparison | Line | Simple, clear trends |
| Professional trading | Hollow Candle | Advanced price action insights |
| Reports/presentations | Spline | Visually polished |
| Volatility analysis | Hilo | Focuses on price range |
| Space-constrained UI | HiloOpenClose | Compact representation |

### Data Completeness Considerations

**If you have OHLC data:**
- Primary: Candle (most informative)
- Alternative: HiloOpenClose (more compact)
- Simplified: Hilo (range only)
- Trend: Line using Close price

**If you only have single values (e.g., Close price):**
- Use: Line or Spline
- Cannot use: Candle, HiloOpenClose, or Hilo (require multiple values)

## Common Issues and Solutions

Troubleshoot common series type configuration and rendering issues using the solutions provided below.

### Issue: Candle series not displaying

**Symptoms:** Empty chart or errors

**Solutions:**
1. Verify all required properties are set: `XName`, `Open`, `High`, `Low`, `Close`
2. Check property names match your data class exactly (case-sensitive)
3. Ensure data has valid numeric values (not null)
4. Confirm Date property is valid DateTime

### Issue: Series selector not showing all types

**Symptoms:** Some series types missing from dropdown

**Solutions:**
1. Check `SeriesType` list includes desired types
2. Ensure `EnableSelector="true"` is set
3. Verify your data supports the series types (e.g., Line doesn't need OHLC)

### Issue: Hollow candles look the same as solid

**Symptoms:** All candles appear filled

**Solutions:**
1. Set `EnableSolidCandles="false"` explicitly
2. Ensure sufficient data points to see variation
3. Check that Close prices vary relative to previous candles

