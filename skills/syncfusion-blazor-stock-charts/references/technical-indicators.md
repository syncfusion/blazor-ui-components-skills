# Technical Indicators Reference

This reference guide provides comprehensive coverage of technical indicators in Syncfusion Blazor Stock Chart, including configuration, customization, and practical examples for financial analysis.

## Table of Contents

- [Overview](#overview)
- [Supported Indicators](#supported-indicators)
- [Simple Moving Average (SMA)](#simple-moving-average-sma)
- [Exponential Moving Average (EMA)](#exponential-moving-average-ema)
- [Triangular Moving Average (TMA)](#triangular-moving-average-tma)
- [Moving Average Convergence Divergence (MACD)](#moving-average-convergence-divergence-macd)
- [Relative Strength Index (RSI)](#relative-strength-index-rsi)
- [Bollinger Bands](#bollinger-bands)
- [Average True Range (ATR)](#average-true-range-atr)
- [Accumulation Distribution](#accumulation-distribution)
- [Stochastic Oscillator](#stochastic-oscillator)
- [Momentum](#momentum)
- [Overlaying Multiple Indicators](#overlaying-multiple-indicators)
- [Customization Options](#customization-options)
- [When to Use Each Indicator](#when-to-use-each-indicator)
- [Complete Examples](#complete-examples)
- [Common Issues and Troubleshooting](#common-issues-and-troubleshooting)
- [Best Practices](#best-practices)

## Overview

Technical indicators are mathematical calculations based on historical price, volume, or open interest data that help forecast financial market direction. The Stock Chart supports 10 types of technical indicators that can be added dynamically using the indicator dropdown button.

**Properties Used**: `Type`, `Field`, `SeriesName`, `Period` and additional configuration properties based on indicator type.

Key features:
- Multiple indicators can be overlaid simultaneously
- Each indicator is fully customizable (colors, periods, thresholds)
- Indicators update automatically when data changes
- Compatible with all series types

## Supported Indicators

| Indicator | Type | Description | Key Parameters |
|-----------|------|-------------|----------------|
| SMA | Trend | Simple Moving Average | Period |
| EMA | Trend | Exponential Moving Average | Period |
| TMA | Trend | Triangular Moving Average | Period |
| MACD | Momentum | Moving Average Convergence Divergence | Fast/Slow/Signal Periods |
| RSI | Momentum | Relative Strength Index | Period, Overbought, Oversold |
| Bollinger Bands | Volatility | Price bands based on standard deviation | Period, Standard Deviation |
| ATR | Volatility | Average True Range | Period |
| Accumulation Distribution | Volume | Money flow indicator | Requires Volume data |
| Stochastic | Momentum | Compares closing price to price range | Period, K Period, D Period |
| Momentum | Momentum | Rate of price change | Period, Upper Band |

## Simple Moving Average (SMA)

The SMA calculates the average of prices over a specified period, smoothing price data to identify trends. Uses properties: `Type` (set to `TechnicalIndicators.Sma`), `Field`, `SeriesName`, `Period`, `Fill`, and `Width` for customization.

### Basic Implementation

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL with SMA Indicator">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle" 
                         XName="Date" High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
    
    <StockChartIndicators>
        <StockChartIndicator Type="TechnicalIndicators.Sma" 
                            Field="FinancialDataFields.Close" 
                            SeriesName="Series1" 
                            Period="14">
        </StockChartIndicator>
    </StockChartIndicators>
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
        // Add more data points
    };
}
```

### Customized SMA

```cshtml
<StockChartIndicator Type="TechnicalIndicators.Sma" 
                    Field="FinancialDataFields.Close" 
                    SeriesName="Series1" 
                    Period="20"
                    Fill="#FF6B6B"
                    Width="2">
</StockChartIndicator>
```

## Exponential Moving Average (EMA)

EMA gives more weight to recent prices, responding faster to price changes than SMA. Uses properties: `Type` (set to `TechnicalIndicators.Ema`), `Field`, `SeriesName`, `Period`, `Fill`, and `Width` for configuration and styling.

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL with EMA (12 & 26)">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Name="AAPL" Type="ChartSeriesType.Line" 
                         XName="Date" YName="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
    
    <StockChartIndicators>
        <!-- Short-term EMA -->
        <StockChartIndicator Type="TechnicalIndicators.Ema" 
                            Field="FinancialDataFields.Close" 
                            SeriesName="AAPL" 
                            Period="12"
                            Fill="#4CAF50"
                            Width="2">
        </StockChartIndicator>
        
        <!-- Long-term EMA -->
        <StockChartIndicator Type="TechnicalIndicators.Ema" 
                            Field="FinancialDataFields.Close" 
                            SeriesName="AAPL" 
                            Period="26"
                            Fill="#FF9800"
                            Width="2">
        </StockChartIndicator>
    </StockChartIndicators>
</SfStockChart>
@code {}
```

**When to Use EMA:**
- Tracking short-term trends
- Identifying potential entry/exit points
- Faster response to price changes than SMA

## Triangular Moving Average (TMA)

TMA is a double-smoothed moving average that reduces lag and filters noise effectively. Uses properties: `Type` (set to `TechnicalIndicators.Tma`), `Field`, `SeriesName`, `Period`, and `Fill` for rendering.

```cshtml
<StockChartIndicators>
    <StockChartIndicator Type="TechnicalIndicators.Tma" 
                        Field="FinancialDataFields.Close" 
                        SeriesName="Series1" 
                        Period="14"
                        Fill="#9C27B0">
    </StockChartIndicator>
</StockChartIndicators>
```

## Moving Average Convergence Divergence (MACD)

MACD shows the relationship between two EMAs and is used to identify trend changes and momentum. Uses properties: `Type` (set to `TechnicalIndicators.Macd`), `Field`, `SeriesName`, `Period`, `FastPeriod`, `SlowPeriod`, and `MacdType` to configure the MACD behavior.

### Configuration

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL with MACD Indicator">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Name="AAPL" Type="ChartSeriesType.Candle" 
                         XName="Date" High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
    
    <StockChartIndicators>
        <StockChartIndicator Type="TechnicalIndicators.Macd" 
                            Field="FinancialDataFields.Close" 
                            SeriesName="AAPL"
                            Period="12"
                            SlowPeriod="26"
                            FastPeriod="12"
                            MacdType="MacdType.Both">
        </StockChartIndicator>
    </StockChartIndicators>
</SfStockChart>
@code {}
```

### MACD Components

- **MACD Line**: Difference between 12-period EMA and 26-period EMA
- **Signal Line**: 9-period EMA of MACD line
- **Histogram**: Difference between MACD line and signal line

**When to Use MACD:**
- Identifying trend reversals
- Detecting momentum changes
- Confirming breakouts
- Spotting divergences (price vs. indicator)

## Relative Strength Index (RSI)

RSI measures the magnitude of recent price changes to evaluate overbought or oversold conditions. Uses properties: `Type` (set to `TechnicalIndicators.Rsi`), `Field`, `SeriesName`, `Period`, `OverBought`, `OverSold`, `Fill`, and `StockChartIndicatorUpperLine`/`StockChartIndicatorLowerLine` for customization.

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL with RSI Indicator">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Name="AAPL" Type="ChartSeriesType.Line" 
                         XName="Date" YName="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
    
    <StockChartIndicators>
        <StockChartIndicator Type="TechnicalIndicators.Rsi" 
                            Field="FinancialDataFields.Close" 
                            SeriesName="AAPL"
                            Period="14"
                            OverBought="70"
                            OverSold="30"
                            Fill="#2196F3">
            <StockChartIndicatorUpperLine Color="#FF5252"></StockChartIndicatorUpperLine>
            <StockChartIndicatorLowerLine Color="#4CAF50"></StockChartIndicatorLowerLine>
        </StockChartIndicator>
    </StockChartIndicators>
</SfStockChart>
@code {}
```

### RSI Interpretation

- **Above 70**: Overbought condition (potential sell signal)
- **Below 30**: Oversold condition (potential buy signal)
- **50**: Neutral zone
- **Divergence**: Price makes new high/low but RSI doesn't = potential reversal

**When to Use RSI:**
- Identifying overbought/oversold conditions
- Spotting momentum divergences
- Confirming trend strength
- Range-bound markets

## Bollinger Bands

Bollinger Bands consist of a middle band (SMA) and upper/lower bands (standard deviations from SMA). Uses properties: `Type` (set to `TechnicalIndicators.BollingerBand`), `Field`, `SeriesName`, `Period`, `StandardDeviation`, `Fill`, and `StockChartIndicatorUpperLine`/`StockChartIndicatorLowerLine` for band styling.

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL with Bollinger Bands">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Name="AAPL" Type="ChartSeriesType.Candle" 
                         XName="Date" High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
    
    <StockChartIndicators>
        <StockChartIndicator Type="TechnicalIndicators.BollingerBand" 
                            Field="FinancialDataFields.Close" 
                            SeriesName="AAPL"
                            Period="20"
                            StandardDeviation="2"
                            Fill="#E3F2FD">
            <StockChartIndicatorUpperLine Color="#2196F3" Width="1"></StockChartIndicatorUpperLine>
            <StockChartIndicatorLowerLine Color="#2196F3" Width="1"></StockChartIndicatorLowerLine>
        </StockChartIndicator>
    </StockChartIndicators>
</SfStockChart>
@code {}
```

### Bollinger Band Interpretation

- **Price at Upper Band**: Potentially overbought
- **Price at Lower Band**: Potentially oversold
- **Band Squeeze**: Low volatility, potential breakout coming
- **Band Expansion**: High volatility, trend continuation

**When to Use Bollinger Bands:**
- Measuring volatility
- Identifying overbought/oversold levels
- Spotting potential breakouts
- Mean reversion strategies

## Average True Range (ATR)

ATR measures market volatility by calculating the average of true ranges over a specified period. Uses properties: `Type` (set to `TechnicalIndicators.Atr`), `Field`, `SeriesName`, `Period`, and `Fill` for configuration.

```cshtml
<StockChartIndicators>
    <StockChartIndicator Type="TechnicalIndicators.Atr" 
                        Field="FinancialDataFields.Close" 
                        SeriesName="AAPL"
                        Period="14"
                        Fill="#FF9800">
    </StockChartIndicator>
</StockChartIndicators>
```

**When to Use ATR:**
- Measuring market volatility
- Setting stop-loss levels
- Position sizing
- Identifying potential breakouts

## Accumulation Distribution

Shows the cumulative flow of money into and out of a security. Uses properties: `Type` (set to `TechnicalIndicators.AccumulationDistribution`), `Field`, `SeriesName`, `Volume`, `XName`, `High`, `Low`, `Open`, `Close`, and `Fill` for data mapping and rendering.

```cshtml
<StockChartIndicators>
    <StockChartIndicator Type="TechnicalIndicators.AccumulationDistribution" 
                        Field="FinancialDataFields.Close" 
                        SeriesName="AAPL"
                        Volume="Volume"
                        XName="Date"
                        High="High"
                        Low="Low"
                        Open="Open"
                        Close="Close"
                        Fill="#9C27B0">
    </StockChartIndicator>
</StockChartIndicators>
```

**Note**: Requires volume data in your data source.

**When to Use Accumulation Distribution:**
- Confirming trend strength
- Identifying divergences
- Detecting distribution (selling) or accumulation (buying)

## Stochastic Oscillator

Compares a security's closing price to its price range over a given period. Uses properties: `Type` (set to `TechnicalIndicators.Stochastic`), `Field`, `SeriesName`, `Period`, `KPeriod`, `DPeriod`, `OverBought`, `OverSold`, and `StockChartIndicatorUpperLine`/`StockChartIndicatorLowerLine`/`StockChartIndicatorPeriodLine` for customization.

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL with Stochastic Oscillator">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Name="AAPL" Type="ChartSeriesType.Candle" 
                         XName="Date" High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
    
    <StockChartIndicators>
        <StockChartIndicator Type="TechnicalIndicators.Stochastic" 
                            Field="FinancialDataFields.Close" 
                            SeriesName="AAPL"
                            Period="14"
                            KPeriod="3"
                            DPeriod="3"
                            OverBought="80"
                            OverSold="20">
            <StockChartIndicatorUpperLine Color="#FF5252"></StockChartIndicatorUpperLine>
            <StockChartIndicatorLowerLine Color="#4CAF50"></StockChartIndicatorLowerLine>
            <StockChartIndicatorPeriodLine Color="#2196F3" Width="2"></StockChartIndicatorPeriodLine>
        </StockChartIndicator>
    </StockChartIndicators>
</SfStockChart>
@code {}
```

### Stochastic Components

- **%K Line**: Fast stochastic
- **%D Line**: Slow stochastic (moving average of %K)
- **Upper Band**: Overbought threshold (usually 80)
- **Lower Band**: Oversold threshold (usually 20)

**When to Use Stochastic:**
- Identifying overbought/oversold conditions
- Spotting divergences
- Range-bound markets
- Confirming reversals

## Momentum

Measures the rate of change in price over a specified period. Uses properties: `Type` (set to `TechnicalIndicators.Momentum`), `Field`, `SeriesName`, `Period`, `UpperBand`, `Fill`, and `StockChartIndicatorUpperLine` for customization.

```cshtml
<StockChartIndicators>
    <StockChartIndicator Type="TechnicalIndicators.Momentum" 
                        Field="FinancialDataFields.Close" 
                        SeriesName="AAPL"
                        Period="14"
                        UpperBand="100"
                        Fill="#FF5722">
        <StockChartIndicatorUpperLine Color="#E91E63" Width="1"></StockChartIndicatorUpperLine>
    </StockChartIndicator>
</StockChartIndicators>
```

**When to Use Momentum:**
- Identifying trend strength
- Spotting potential reversals
- Confirming breakouts

## Overlaying Multiple Indicators

Combine multiple indicators for comprehensive analysis. Each indicator uses the same property structure (`Type`, `Field`, `SeriesName`, `Period`, and type-specific properties) within the `StockChartIndicators` collection:

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Multi-Indicator Technical Analysis">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Name="AAPL" Type="ChartSeriesType.Candle" 
                         XName="Date" High="High" Low="Low" Open="Open" Close="Close" Volume="Volume">
        </StockChartSeries>
    </StockChartSeriesCollection>
    
    <StockChartIndicators>
        <!-- SMA for trend identification -->
        <StockChartIndicator Type="TechnicalIndicators.Sma" 
                            Field="FinancialDataFields.Close" 
                            SeriesName="AAPL"
                            Period="50"
                            Fill="#4CAF50"
                            Width="2">
        </StockChartIndicator>
        
        <!-- Bollinger Bands for volatility -->
        <StockChartIndicator Type="TechnicalIndicators.BollingerBand" 
                            Field="FinancialDataFields.Close" 
                            SeriesName="AAPL"
                            Period="20"
                            StandardDeviation="2"
                            Fill="#E3F2FD">
            <StockChartIndicatorUpperLine Color="#2196F3"></StockChartIndicatorUpperLine>
            <StockChartIndicatorLowerLine Color="#2196F3"></StockChartIndicatorLowerLine>
        </StockChartIndicator>
        
        <!-- RSI for momentum -->
        <StockChartIndicator Type="TechnicalIndicators.Rsi" 
                            Field="FinancialDataFields.Close" 
                            SeriesName="AAPL"
                            Period="14"
                            OverBought="70"
                            OverSold="30">
        </StockChartIndicator>
        
        <!-- MACD for trend changes -->
        <StockChartIndicator Type="TechnicalIndicators.Macd" 
                            Field="FinancialDataFields.Close" 
                            SeriesName="AAPL"
                            FastPeriod="12"
                            SlowPeriod="26"
                            Period="9">
        </StockChartIndicator>
    </StockChartIndicators>
    
    <StockChartTooltipSettings Enable="true"></StockChartTooltipSettings>
    <StockChartCrosshairSettings Enable="true"></StockChartCrosshairSettings>
</SfStockChart>
@code {}
```

## Customization Options

Each indicator provides customization through properties like `Fill`, `Width`, and specialized components like `StockChartIndicatorUpperLine`, `StockChartIndicatorLowerLine`, and `StockChartIndicatorPeriodLine`.

### Color Customization

Uses property `Fill` to set indicator line color and line-specific components with `Color` property:

```cshtml
<StockChartIndicator Type="TechnicalIndicators.Sma" 
                    Fill="#FF6B6B"
                    Width="3">
</StockChartIndicator>
```

### Line Style Customization

Uses properties: `Color`, `Width`, and `DashArray` within `StockChartIndicatorUpperLine`, `StockChartIndicatorLowerLine`, and `StockChartIndicatorPeriodLine` components:

```cshtml
<StockChartIndicator Type="TechnicalIndicators.Rsi">
    <StockChartIndicatorUpperLine Color="#FF5252" Width="2" DashArray="5"></StockChartIndicatorUpperLine>
    <StockChartIndicatorLowerLine Color="#4CAF50" Width="2" DashArray="5"></StockChartIndicatorLowerLine>
    <StockChartIndicatorPeriodLine Color="#2196F3" Width="2"></StockChartIndicatorPeriodLine>
</StockChartIndicator>
```

### Animation

Uses `StockChartIndicatorAnimation` component with properties `Enable` and `Duration` to control indicator animation behavior:

```cshtml
<StockChartIndicator Type="TechnicalIndicators.Ema">
    <StockChartIndicatorAnimation Enable="true" Duration="1000"></StockChartIndicatorAnimation>
</StockChartIndicator>
```

## When to Use Each Indicator

### Trend Indicators
- **SMA/EMA/TMA**: Use when identifying trend direction and strength
- **Best for**: Trending markets, long-term analysis

### Momentum Indicators
- **RSI/Stochastic**: Use when identifying overbought/oversold conditions
- **MACD/Momentum**: Use when confirming trend changes
- **Best for**: Swing trading, identifying reversals

### Volatility Indicators
- **Bollinger Bands**: Use when measuring market volatility
- **ATR**: Use when setting stop-loss levels or position sizing
- **Best for**: Breakout strategies, risk management

### Volume Indicators
- **Accumulation Distribution**: Use when confirming trend strength with volume
- **Best for**: Validating price movements, spotting divergences

## Complete Examples

### Day Trading Setup

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Day Trading Analysis - AAPL" EnableSelector="true">
    <StockChartPeriods>
        <StockChartPeriod Text="5m" IntervalType="RangeIntervalType.Minutes" Interval="5"></StockChartPeriod>
        <StockChartPeriod Text="15m" IntervalType="RangeIntervalType.Minutes" Interval="15"></StockChartPeriod>
        <StockChartPeriod Text="1H" IntervalType="RangeIntervalType.Hours" Interval="1" Selected="true"></StockChartPeriod>
    </StockChartPeriods>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@IntradayData" Name="AAPL" Type="ChartSeriesType.HiloOpenClose" 
                         XName="Time" High="High" Low="Low" Open="Open" Close="Close" Volume="Volume">
        </StockChartSeries>
    </StockChartSeriesCollection>
    
    <StockChartIndicators>
        <!-- Fast EMA for quick signals -->
        <StockChartIndicator Type="TechnicalIndicators.Ema" 
                            Field="FinancialDataFields.Close" 
                            SeriesName="AAPL"
                            Period="9"
                            Fill="#4CAF50"
                            Width="2">
        </StockChartIndicator>
        
        <!-- RSI for momentum -->
        <StockChartIndicator Type="TechnicalIndicators.Rsi" 
                            Field="FinancialDataFields.Close" 
                            SeriesName="AAPL"
                            Period="14"
                            OverBought="70"
                            OverSold="30">
        </StockChartIndicator>
        
        <!-- Bollinger Bands for volatility breakouts -->
        <StockChartIndicator Type="TechnicalIndicators.BollingerBand" 
                            Field="FinancialDataFields.Close" 
                            SeriesName="AAPL"
                            Period="20"
                            StandardDeviation="2">
        </StockChartIndicator>
    </StockChartIndicators>
    
    <StockChartCrosshairSettings Enable="true"></StockChartCrosshairSettings>
    <StockChartTooltipSettings Enable="true"></StockChartTooltipSettings>
</SfStockChart>
@code {}
```

### Swing Trading Setup

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Swing Trading Analysis - AAPL">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@DailyData" Name="AAPL" Type="ChartSeriesType.Candle" 
                         XName="Date" High="High" Low="Low" Open="Open" Close="Close" Volume="Volume">
        </StockChartSeries>
    </StockChartSeriesCollection>
    
    <StockChartIndicators>
        <!-- Long-term trend -->
        <StockChartIndicator Type="TechnicalIndicators.Sma" 
                            Field="FinancialDataFields.Close" 
                            SeriesName="AAPL"
                            Period="200"
                            Fill="#FF9800"
                            Width="3">
        </StockChartIndicator>
        
        <!-- Medium-term trend -->
        <StockChartIndicator Type="TechnicalIndicators.Sma" 
                            Field="FinancialDataFields.Close" 
                            SeriesName="AAPL"
                            Period="50"
                            Fill="#4CAF50"
                            Width="2">
        </StockChartIndicator>
        
        <!-- MACD for trend confirmation -->
        <StockChartIndicator Type="TechnicalIndicators.Macd" 
                            Field="FinancialDataFields.Close" 
                            SeriesName="AAPL"
                            FastPeriod="12"
                            SlowPeriod="26"
                            Period="9">
        </StockChartIndicator>
        
        <!-- ATR for volatility/stop-loss -->
        <StockChartIndicator Type="TechnicalIndicators.Atr" 
                            Field="FinancialDataFields.Close" 
                            SeriesName="AAPL"
                            Period="14">
        </StockChartIndicator>
    </StockChartIndicators>
</SfStockChart>
@code {}
```

## Common Issues and Troubleshooting

### Issue 1: Indicator Not Appearing

**Problem**: Indicator is configured but doesn't appear on the chart.

**Solution**: Ensure the `SeriesName` matches exactly with the series `Name` property:

```cshtml
<StockChartSeries DataSource="@StockData" Name="AAPL" ...>
</StockChartSeries>

<StockChartIndicator SeriesName="AAPL" ...>
</StockChartIndicator>
```

### Issue 2: Accumulation Distribution Indicator Error

**Problem**: Accumulation Distribution indicator throws error or shows no data.

**Solution**: This indicator requires volume data. Ensure your data source includes the `Volume` field and it's properly mapped:

```csharp
public class StockChartData
{
    public DateTime Date { get; set; }
    public double Open { get; set; }
    public double High { get; set; }
    public double Low { get; set; }
    public double Close { get; set; }
    public double Volume { get; set; } // Required!
}
```

### Issue 3: Indicators Overlapping and Hard to Read

**Problem**: Multiple indicators are overlapping, making the chart cluttered.

**Solution**: Use different colors and consider showing fewer indicators simultaneously:

```cshtml
<!-- Use distinct colors -->
<StockChartIndicator Type="TechnicalIndicators.Sma" Fill="#4CAF50">
</StockChartIndicator>
<StockChartIndicator Type="TechnicalIndicators.Ema" Fill="#2196F3">
</StockChartIndicator>
```

### Issue 4: Wrong Period Value

**Problem**: Indicator calculation seems incorrect.

**Solution**: Verify the period value is appropriate for your data:
- Daily data: 14-50 period
- Hourly data: 20-100 period
- Minute data: 50-200 period

### Issue 5: Indicator Line Not Smooth

**Problem**: Indicator line appears jagged or choppy.

**Solution**: Ensure sufficient data points. Indicators need data points equal to or greater than the period value to calculate properly.

## Best Practices

1. **Don't Overload the Chart**
   - Use 2-4 indicators maximum
   - Combine different types (trend + momentum + volume)
   - Consider separate panels for oscillators

2. **Match Indicators to Trading Style**
   - Day trading: Fast EMAs, RSI, Bollinger Bands
   - Swing trading: SMAs, MACD, Stochastic
   - Long-term investing: Long-period SMAs, ATR

3. **Customize Colors Meaningfully**
   - Use green for bullish indicators
   - Use red for bearish indicators
   - Maintain consistency across charts

4. **Set Appropriate Periods**
   - Shorter periods: More signals, more noise
   - Longer periods: Fewer signals, more reliable
   - Standard: 14 for RSI, 20 for Bollinger Bands, 12/26/9 for MACD

5. **Confirm with Multiple Indicators**
   - Don't rely on a single indicator
   - Look for confluence (multiple indicators agreeing)
   - Use indicators to confirm price action, not replace it

6. **Consider Data Timeframe**
   - Match indicator periods to your trading timeframe
   - Test indicators with historical data
   - Adjust based on market conditions (trending vs. ranging)

7. **Performance Optimization**
   - Limit concurrent indicators to 4-5
   - Use simpler indicators for large datasets
   - Consider lazy loading for historical data

This comprehensive reference provides developers with everything needed to implement and customize technical indicators in Syncfusion Blazor Stock Charts for sophisticated financial analysis applications.
