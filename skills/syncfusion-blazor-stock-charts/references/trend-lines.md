# Trendlines Reference

This reference guide covers trendlines in Syncfusion Blazor Stock Chart, providing mathematical models to visualize price trends, forecast future values, and identify patterns in stock data.

## Table of Contents
- [Overview](#overview)
- [Core Concepts](#core-concepts)
- [Linear Trendline](#linear-trendline)
- [Exponential Trendline](#exponential-trendline)
- [Logarithmic Trendline](#logarithmic-trendline)
- [Polynomial Trendline](#polynomial-trendline)
- [Power Trendline](#power-trendline)
- [Moving Average Trendline](#moving-average-trendline)
- [Advanced Customization](#advanced-customization)
- [Complete Examples](#complete-examples)
- [Trendline Selection Guide](#trendline-selection-guide)
- [When to Use](#when-to-use)
- [Common Issues and Troubleshooting](#common-issues-and-troubleshooting)
- [Best Practices](#best-practices)

## Overview

Trendlines are mathematical calculations overlaid on the chart that show the general direction and speed of price movement. They help identify trend strength, potential reversals, and forecast future price levels.

The Stock Chart supports six types of trendlines:
- Linear
- Exponential
- Logarithmic
- Polynomial
- Power
- Moving Average

## Core Concepts

### Trendline vs. Technical Indicator

| Feature | Trendline | Technical Indicator |
|---------|-----------|---------------------|
| Purpose | Mathematical fit to price data | Derived calculation from price/volume |
| Configuration | Simple (type + period) | Complex (multiple parameters) |
| Use Case | Trend direction and forecasting | Momentum, volatility, overbought/oversold |
| Visual | Single line fitting data | Can have multiple components |

### Common Properties

All trendlines share these properties:

```cshtml
<StockChartTrendline Type="TrendlineTypes.Linear"
                     Period="14"
                     Fill="#FF6B6B"
                     Width="2"
                     EnableTooltip="true">
</StockChartTrendline>
```

- **Type**: Algorithm used for calculation
- **Period**: Number of data points used (for Moving Average)
- **Fill**: Line color
- **Width**: Line thickness
- **EnableTooltip**: Show trendline value on hover

## Linear Trendline

A best-fit straight line used with datasets that increase or decrease at a steady rate. The `Type` property will be set to `TrendlineTypes.Linear` for linear trendline calculation.

### Basic Implementation

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL with Linear Trendline">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle" 
                         XName="Date" High="High" Low="Low" Open="Open" Close="Close" Name="AAPL">
            <StockChartTrendlines>
                <StockChartTrendline Type="TrendlineTypes.Linear" 
                                     Fill="#4CAF50" 
                                     Width="2"
                                     EnableTooltip="true">
                </StockChartTrendline>
            </StockChartTrendlines>
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
        new StockChartData { Date = new DateTime(2012, 04, 02), Open = 85.97, High = 90.66, Low = 85.76, Close = 90.52, Volume = 660187068 },
        new StockChartData { Date = new DateTime(2012, 04, 09), Open = 89.44, High = 92.00, Low = 86.21, Close = 86.46, Volume = 912634864 },
        new StockChartData { Date = new DateTime(2012, 04, 16), Open = 87.15, High = 88.60, Low = 81.48, Close = 81.85, Volume = 1221746066 },
        new StockChartData { Date = new DateTime(2012, 04, 23), Open = 81.51, High = 88.28, Low = 79.28, Close = 86.14, Volume = 965935749 }
    };
}
```

### When to Use Linear

- Data shows steady increase or decrease
- Identifying long-term trend direction
- Simple trending patterns
- Quick visual assessment of trend strength

**Mathematical Formula:** y = mx + b (slope-intercept form)

## Exponential Trendline

A curved line best suited for data that rises or falls at increasingly higher rates. Cannot be used with zero or negative values. The `Type` property will be set to `TrendlineTypes.Exponential` for exponential growth pattern analysis.

### Implementation

```cshtml
<StockChartTrendlines>
    <StockChartTrendline Type="TrendlineTypes.Exponential" 
                         Fill="#FF9800" 
                         Width="2"
                         EnableTooltip="true">
    </StockChartTrendline>
</StockChartTrendlines>
```

### When to Use Exponential

- Growth stocks with accelerating gains
- Compound growth patterns
- Viral adoption curves
- Exponentially increasing volatility

**Mathematical Formula:** y = ab^x

**Limitations:** Cannot be used if data contains zero or negative values.

## Logarithmic Trendline

A curved line useful when data increases or decreases quickly and then levels out. Supports negative and positive values. The `Type` property will be set to `TrendlineTypes.Logarithmic` for analyzing slowing growth patterns.

### Implementation

```cshtml
<StockChartTrendlines>
    <StockChartTrendline Type="TrendlineTypes.Logarithmic" 
                         Fill="#9C27B0" 
                         Width="2"
                         EnableTooltip="true">
    </StockChartTrendline>
</StockChartTrendlines>
```

### When to Use Logarithmic

- Market saturation scenarios
- Mature company growth slowing
- Recovery patterns that level off
- Diminishing returns situations

**Mathematical Formula:** y = a ln(x) + b

## Polynomial Trendline

A curved line used when data fluctuates. The degree of polynomial determines the number of curves. The `Type` property will be set to `TrendlineTypes.Polynomial` and the `PolynomialOrder` property will define the degree of the polynomial curve.

### Basic Implementation

```cshtml
<StockChartTrendlines>
    <StockChartTrendline Type="TrendlineTypes.Polynomial" 
                         PolynomialOrder="3"
                         Fill="#2196F3" 
                         Width="2"
                         EnableTooltip="true">
    </StockChartTrendline>
</StockChartTrendlines>
```

### Polynomial Orders

```cshtml
<!-- Order 2: Quadratic (one curve) -->
<StockChartTrendline Type="TrendlineTypes.Polynomial" 
                     PolynomialOrder="2">
</StockChartTrendline>

<!-- Order 3: Cubic (two curves) -->
<StockChartTrendline Type="TrendlineTypes.Polynomial" 
                     PolynomialOrder="3">
</StockChartTrendline>

<!-- Order 4: Quartic (three curves) -->
<StockChartTrendline Type="TrendlineTypes.Polynomial" 
                     PolynomialOrder="4">
</StockChartTrendline>
```

### When to Use Polynomial

- Complex fluctuating patterns
- Cyclical market behavior
- Multiple trend reversals
- Detailed curve fitting needed

**Mathematical Formula:** y = a₀ + a₁x + a₂x² + a₃x³ + ... + aₙxⁿ

**Note:** Higher orders (4+) can lead to overfitting. Use 2-3 for most cases.

## Power Trendline

A curved line for datasets comparing measurements that increase at a specific rate. The `Type` property will be set to `TrendlineTypes.Power` for analyzing proportional growth relationships.

### Implementation

```cshtml
<StockChartTrendlines>
    <StockChartTrendline Type="TrendlineTypes.Power" 
                         Fill="#E91E63" 
                         Width="2"
                         EnableTooltip="true">
    </StockChartTrendline>
</StockChartTrendlines>
```

### When to Use Power

- Acceleration/deceleration patterns
- Performance metrics
- Market efficiency studies

**Mathematical Formula:** y = ax^b

**Limitations:** Cannot be used with zero or negative values.

## Moving Average Trendline

Smooths out fluctuations by averaging prices over a specified period. The `Type` property will be set to `TrendlineTypes.MovingAverage` and the `Period` property will determine the number of data points used for averaging calculation.

### Configuration

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL with Moving Average Trendline">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle" 
                         XName="Date" High="High" Low="Low" Open="Open" Close="Close" Name="AAPL">
            <StockChartTrendlines>
                <StockChartTrendline Type="TrendlineTypes.MovingAverage" 
                                     Period="20"
                                     Fill="#4CAF50" 
                                     Width="2"
                                     EnableTooltip="true">
                </StockChartTrendline>
            </StockChartTrendlines>
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
        new StockChartData { Date = new DateTime(2012, 04, 02), Open = 85.97, High = 90.66, Low = 85.76, Close = 90.52, Volume = 660187068 },
        new StockChartData { Date = new DateTime(2012, 04, 09), Open = 89.44, High = 92.00, Low = 86.21, Close = 86.46, Volume = 912634864 },
        new StockChartData { Date = new DateTime(2012, 04, 16), Open = 87.15, High = 88.60, Low = 81.48, Close = 81.85, Volume = 1221746066 },
        new StockChartData { Date = new DateTime(2012, 04, 23), Open = 81.51, High = 88.28, Low = 79.28, Close = 86.14, Volume = 965935749 }
    };
}
```

### Common Period Values

```cshtml
<!-- Short-term: 10-20 days -->
<StockChartTrendline Type="TrendlineTypes.MovingAverage" Period="10">
</StockChartTrendline>

<!-- Medium-term: 50 days -->
<StockChartTrendline Type="TrendlineTypes.MovingAverage" Period="50">
</StockChartTrendline>

<!-- Long-term: 200 days -->
<StockChartTrendline Type="TrendlineTypes.MovingAverage" Period="200">
</StockChartTrendline>
```

### When to Use Moving Average

- Smoothing noisy data
- Identifying support/resistance levels
- Trend confirmation
- Generating trading signals (crossovers)

## Advanced Customization

### Multiple Trendlines

Overlay multiple trendline types for comprehensive analysis. The `StockChartTrendlines` collection property will contain multiple `StockChartTrendline` items with different Type configurations:

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Multi-Trendline Analysis">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Line" 
                         XName="Date" YName="Close" Name="AAPL">
            <StockChartTrendlines>
                <!-- Linear for overall trend -->
                <StockChartTrendline Type="TrendlineTypes.Linear" 
                                     Fill="#4CAF50" 
                                     Width="2">
                </StockChartTrendline>
                
                <!-- Moving Average for smoothing -->
                <StockChartTrendline Type="TrendlineTypes.MovingAverage" 
                                     Period="20"
                                     Fill="#2196F3" 
                                     Width="2">
                </StockChartTrendline>
                
                <!-- Polynomial for detailed curves -->
                <StockChartTrendline Type="TrendlineTypes.Polynomial" 
                                     PolynomialOrder="3"
                                     Fill="#FF9800" 
                                     Width="1">
                </StockChartTrendline>
            </StockChartTrendlines>
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
        new StockChartData { Date = new DateTime(2012, 04, 02), Open = 85.97, High = 90.66, Low = 85.76, Close = 90.52, Volume = 660187068 },
        new StockChartData { Date = new DateTime(2012, 04, 09), Open = 89.44, High = 92.00, Low = 86.21, Close = 86.46, Volume = 912634864 },
        new StockChartData { Date = new DateTime(2012, 04, 16), Open = 87.15, High = 88.60, Low = 81.48, Close = 81.85, Volume = 1221746066 },
        new StockChartData { Date = new DateTime(2012, 04, 23), Open = 81.51, High = 88.28, Low = 79.28, Close = 86.14, Volume = 965935749 }
    };
}
```

### Styling Options

```cshtml
<StockChartTrendline Type="TrendlineTypes.Linear" 
                     Fill="#FF6B6B"
                     Width="3"
                     DashArray="5,5"
                     Opacity="0.7"
                     EnableTooltip="true">
</StockChartTrendline>
```

The `Fill` property will set the line color for the trendline. The `Width` property will define the line thickness. The `DashArray` property will create dash patterns for visual distinction. The `Opacity` property will control the transparency level. The `EnableTooltip` property will display values on hover:
- **Fill**: Line color (hex, rgb, color name)
- **Width**: Line thickness (1-10 typical range)
- **DashArray**: Dash pattern (e.g., "5,5" for dashed line)
- **Opacity**: Transparency (0-1)
- **EnableTooltip**: Show values on hover

### Forward and Backward Forecasting

Extend trendlines beyond the data range. The `ForwardForecast` property will extend the trendline into future periods and the `BackwardForecast` property will extend it into past periods:

```cshtml
<StockChartTrendline Type="TrendlineTypes.Linear" 
                     ForwardForecast="10"
                     BackwardForecast="5">
</StockChartTrendline>
```

- **ForwardForecast**: Extends trendline into the future
- **BackwardForecast**: Extends trendline into the past

## Complete Examples

### Trend Analysis Dashboard

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Comprehensive Trend Analysis - AAPL">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle" 
                         XName="Date" High="High" Low="Low" Open="Open" Close="Close" Name="AAPL">
            <StockChartTrendlines>
                <!-- Long-term linear trend -->
                <StockChartTrendline Type="TrendlineTypes.Linear" 
                                     Fill="#4CAF50" 
                                     Width="2"
                                     ForwardForecast="30">
                </StockChartTrendline>
                
                <!-- Short-term moving average -->
                <StockChartTrendline Type="TrendlineTypes.MovingAverage" 
                                     Period="20"
                                     Fill="#2196F3" 
                                     Width="2">
                </StockChartTrendline>
                
                <!-- Polynomial for market cycles -->
                <StockChartTrendline Type="TrendlineTypes.Polynomial" 
                                     PolynomialOrder="3"
                                     Fill="#FF9800" 
                                     Width="1">
                </StockChartTrendline>
            </StockChartTrendlines>
        </StockChartSeries>
    </StockChartSeriesCollection>
    
    <StockChartCrosshairSettings Enable="true"></StockChartCrosshairSettings>
    <StockChartTooltipSettings Enable="true"></StockChartTooltipSettings>
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
        new StockChartData { Date = new DateTime(2012, 04, 02), Open = 85.97, High = 90.66, Low = 85.76, Close = 90.52, Volume = 660187068 },
        new StockChartData { Date = new DateTime(2012, 04, 09), Open = 89.44, High = 92.00, Low = 86.21, Close = 86.46, Volume = 912634864 },
        new StockChartData { Date = new DateTime(2012, 04, 16), Open = 87.15, High = 88.60, Low = 81.48, Close = 81.85, Volume = 1221746066 },
        new StockChartData { Date = new DateTime(2012, 04, 23), Open = 81.51, High = 88.28, Low = 79.28, Close = 86.14, Volume = 965935749 }
    };
}
```

### Growth Stock Analysis

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Growth Stock Trendline - TSLA">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Area" 
                         XName="Date" YName="Close" Name="TSLA" Opacity="0.3">
            <StockChartTrendlines>
                <!-- Exponential for growth trajectory -->
                <StockChartTrendline Type="TrendlineTypes.Exponential" 
                                     Fill="#E91E63" 
                                     Width="3"
                                     ForwardForecast="60">
                </StockChartTrendline>
            </StockChartTrendlines>
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
        new StockChartData { Date = new DateTime(2012, 04, 02), Open = 85.97, High = 90.66, Low = 85.76, Close = 90.52, Volume = 660187068 },
        new StockChartData { Date = new DateTime(2012, 04, 09), Open = 89.44, High = 92.00, Low = 86.21, Close = 86.46, Volume = 912634864 },
        new StockChartData { Date = new DateTime(2012, 04, 16), Open = 87.15, High = 88.60, Low = 81.48, Close = 81.85, Volume = 1221746066 },
        new StockChartData { Date = new DateTime(2012, 04, 23), Open = 81.51, High = 88.28, Low = 79.28, Close = 86.14, Volume = 965935749 }
    };
}
```

## Trendline Selection Guide

Use the `Type` property to select appropriate trendline for different market conditions:

| Market Condition | Recommended Trendline | Why |
|-----------------|---------------------|-----|
| Steady uptrend | Linear (TrendlineTypes.Linear) | Simple, clear direction |
| Accelerating growth | Exponential (TrendlineTypes.Exponential) | Captures acceleration |
| Maturing growth | Logarithmic (TrendlineTypes.Logarithmic) | Shows leveling off |
| Volatile/cyclical | Polynomial (TrendlineTypes.Polynomial) with PolynomialOrder 2-3 | Captures curves |
| High noise | Moving Average (TrendlineTypes.MovingAverage) with Period property | Smooths fluctuations |
| Multiple phases | Multiple trendlines with different Types | Comprehensive view |

## When to Use

### Use Trendlines When:
- Identifying overall trend direction
- Forecasting potential future prices
- Confirming support/resistance levels
- Comparing multiple securities' trends
- Explaining historical price patterns
- Smoothing volatile data for analysis

### Don't Use Trendlines When:
- Sideways/choppy markets (no clear trend)
- Very short time periods (insufficient data)
- Extremely high volatility (trendline won't fit well)
- You need precise entry/exit signals (use indicators instead)

## Common Issues and Troubleshooting

### Issue 1: Trendline Not Visible

**Problem**: Trendline is configured but doesn't appear.

**Solution**: Ensure sufficient data points and the trendline is within the visible range. Verify that `DataSource` is properly assigned to the series and the trendline is added to the `StockChartTrendlines` collection:

```cshtml
<!-- Verify series has data -->
<StockChartSeries DataSource="@StockData" Name="AAPL">
    <StockChartTrendlines>
        <!-- Name matches series -->
        <StockChartTrendline Type="TrendlineTypes.Linear">
        </StockChartTrendline>
    </StockChartTrendlines>
</StockChartSeries>
```

### Issue 2: Exponential/Power Trendline Error

**Problem**: Error when using Exponential or Power trendlines with `Type` set to `TrendlineTypes.Exponential` or `TrendlineTypes.Power`.

**Solution**: These types cannot handle zero or negative values. Filter your data:

```csharp
StockData = StockData.Where(d => d.Close > 0).ToList();
```

### Issue 3: Polynomial Trendline Looks Wrong

**Problem**: Polynomial trendline has too many curves or looks unnatural when `Type` is set to `TrendlineTypes.Polynomial`.

**Solution**: Reduce the `PolynomialOrder` property value. Start with 2 or 3:

```cshtml
<!-- Try lower order first -->
<StockChartTrendline Type="TrendlineTypes.Polynomial" 
                     PolynomialOrder="2">
</StockChartTrendline>
```

### Issue 4: Moving Average Period Too Large

**Problem**: Moving average trendline is very short or missing when `Type` is set to `TrendlineTypes.MovingAverage`.

**Solution**: Ensure the `Period` property value is less than your total data points:

```csharp
int maxPeriod = StockData.Count - 1;
// Use period less than maxPeriod
```

### Issue 5: Forecast Not Showing

**Problem**: Forward/backward forecast doesn't extend the trendline when `ForwardForecast` or `BackwardForecast` properties are set.

**Solution**: Ensure you have space in the chart for the forecast:

```cshtml
<StockChartTrendline Type="TrendlineTypes.Linear" 
                     ForwardForecast="30">
</StockChartTrendline>

<!-- Chart must have extra space to show forecast -->
```

## Best Practices

1. **Match Trendline to Data Pattern**
   - Use `Type="TrendlineTypes.Linear"` for steady trends
   - Use `Type="TrendlineTypes.Exponential"` for accelerating growth
   - Use `Type="TrendlineTypes.Logarithmic"` for slowing growth
   - Use `Type="TrendlineTypes.Polynomial"` with appropriate `PolynomialOrder` for complex patterns
   - Use `Type="TrendlineTypes.MovingAverage"` with `Period` property for noisy data

2. **Limit Concurrent Trendlines**
   - Use 1-3 trendlines maximum in the `StockChartTrendlines` collection
   - More than 3 clutters the chart
   - Each should serve a distinct purpose

3. **Choose Appropriate Colors**
   - Use contrasting `Fill` colors from series
   - Green for bullish trends
   - Red for bearish trends
   - Blue/gray for neutral analysis

4. **Set Meaningful Periods**
   - Short-term: 10-20 periods for `Period` property
   - Medium-term: 50 periods
   - Long-term: 100-200 periods
   - Match to your trading timeframe

5. **Use Forecasting Judiciously**
   - `ForwardForecast`: 10-30 periods
   - Don't over-forecast (unreliable beyond 30)
   - Always show historical data alongside

6. **Combine with Indicators**
   - Trendlines show direction with `Type` property
   - Indicators show momentum/strength
   - Use both for complete analysis

7. **Update Regularly**
   - Recalculate as new data arrives in `DataSource`
   - Old trendlines may become irrelevant
   - Trends change over time with `ForwardForecast` adjustments

This comprehensive reference provides developers with all necessary information to implement and customize trendlines in Syncfusion Blazor Stock Charts for effective trend analysis and forecasting.
