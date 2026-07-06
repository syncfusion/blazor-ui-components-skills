---
name: syncfusion-blazor-stock-charts
description: Implement Syncfusion Blazor Stock Chart (SfStockChart) for financial data visualization. Use this when working with stock charts, candlestick displays, OHLC data, or technical indicators like SMA, EMA, MACD, and Bollinger Bands. This skill covers period selectors, range navigation, and financial time-series data visualization in Blazor applications.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
  category: "Data Visualization"
---

# Implementing Stock Charts

**NuGet:** `Syncfusion.Blazor.Charts` + `Syncfusion.Blazor.Themes` (or `Syncfusion.Blazor.StockChart` for individual package)
**Namespace:** `Syncfusion.Blazor.Charts`

A comprehensive skill for implementing Syncfusion Blazor Stock Chart components for financial data visualization. The Stock Chart is specifically designed for visualizing stock market data with built-in features for technical analysis, time-based navigation, and financial indicators.

## When to Use This Skill

Use this skill immediately when you need to:
- Display stock market price data with OHLC (Open, High, Low, Close) values
- Create candlestick, line, or OHLC charts for financial data
- Add technical indicators (SMA, EMA, MACD, RSI, Bollinger Bands, ATR, Stochastic)
- Implement period selectors (1M, 3M, 6M, YTD, 1Y, All) for time-based navigation
- Add range selectors for custom date range selection
- Display stock events (earnings, dividends, stock splits)
- Add trend lines (linear, exponential, polynomial) to financial data
- Enable zooming, panning, and crosshair for data exploration
- Export stock charts to PNG, JPEG, SVG, or PDF formats
- Build investment portfolio dashboards or trading applications
- Visualize cryptocurrency price data or any time-series financial data
- Create responsive financial data visualizations for Blazor Server, WebAssembly, or Web App

## Component Overview

The **Syncfusion Blazor Stock Chart** (`SfStockChart`) is a feature-rich financial charting component that extends standard charting capabilities with:

- **5 Core Series Types**: Line, Spline, Candle, HiloOpenClose, Hilo
- **Hollow Candle Mode**: Enable solid or hollow candle rendering through candle configuration
- **9+ Technical Indicators**: SMA, EMA, MACD, RSI, Bollinger Bands, ATR, and more
- **Time Navigation**: Period selector with preset ranges, Range selector for custom dates
- **Stock Events**: Mark important dates (earnings, dividends, splits)
- **Trend Analysis**: Multiple trend line types with forecasting
- **Interactive Features**: Zooming, panning, crosshair, trackball tooltips
- **Export Options**: PNG, JPEG, SVG, PDF export and print
- **Accessibility**: WCAG compliant with keyboard navigation

## Documentation and Navigation Guide

### Getting Started

📄 **Read:** [references/getting-started.md](references/getting-started.md)

Start here for installation, setup, and your first stock chart. Covers:
- Installing Syncfusion.Blazor.Charts NuGet package
- Blazor Server, WebAssembly, and Web App setup
- Service registration and theme configuration
- Basic SfStockChart implementation with sample data
- Project structure and script references

### Core Chart Features

#### Series Types and Rendering

📄 **Read:** [references/series-types.md](references/series-types.md)

Learn about the 5 core series types for stock data visualization:
- Line and Spline series for trend visualization
- Candle series for traditional stock charts, with hollow candle rendering available through candle configuration
- HiloOpenClose and Hilo for price range display
- Series selector for runtime switching between types
- Choosing the right series type for your data

#### Data Binding and Structure

📄 **Read:** [references/working-with-data.md](references/working-with-data.md)

Configure data sources for stock charts:
- OHLC data structure (Open, High, Low, Close, Volume)
- DateTime-based data binding
- Remote data binding with APIs
- Data adapters for different sources
- Handling large datasets efficiently

#### Axis Configuration

📄 **Read:** [references/axes.md](references/axes.md)

Customize X and Y axes for financial data:
- DateTime, DateTimeCategory, and Logarithmic axis types
- Axis titles, labels, and formatting
- Tick lines and grid line customization
- Multiple axes for volume or indicator overlays
- Inversed axis and opposed positioning

### Financial Analysis Features

#### Period and Range Selectors

📄 **Read:** [references/period-range-selectors.md](references/period-range-selectors.md)

Implement time-based navigation:
- Period selector with preset buttons (1M, 3M, 6M, YTD, 1Y, All)
- Custom period button configuration
- Range selector for dragging date ranges
- Styling and positioning selectors
- Handling period change events

#### Technical Indicators

📄 **Read:** [references/technical-indicators.md](references/technical-indicators.md)

Add financial analysis indicators:
- Simple Moving Average (SMA) and Exponential Moving Average (EMA)
- MACD (Moving Average Convergence Divergence)
- RSI (Relative Strength Index) for momentum
- Bollinger Bands for volatility analysis
- ATR, Accumulation Distribution, Stochastic
- Overlaying multiple indicators
- Customizing indicator appearance

#### Stock Events

📄 **Read:** [references/stock-events.md](references/stock-events.md)

Mark important dates on the chart:
- Adding earnings announcements, dividends, stock splits
- Custom event shapes and icons
- Event positioning and styling
- Tooltip content for events
- Multiple events on the same chart

#### Trend Lines

📄 **Read:** [references/trend-lines.md](references/trend-lines.md)

Add trend analysis to series:
- Linear, Exponential, Logarithmic, Polynomial trends
- Moving average trend lines
- Forecasting with forward/backward periods
- Customizing trend line appearance
- Multiple trend lines per series

### Interactive Features

#### Zooming, Panning, and Crosshair

📄 **Read:** [references/zooming-panning-crosshair.md](references/zooming-panning-crosshair.md)

Enable user interaction for data exploration:
- Selection zoom, pinch zoom, mouse wheel zoom
- Zoom toolbar with reset functionality
- Pan mode for navigating zoomed data
- Crosshair for precise value reading
- Trackball mode for comparing multiple points
- Customizing zoom and crosshair appearance

#### Tooltips and Legend

📄 **Read:** [references/tooltip-legend.md](references/tooltip-legend.md)

Display data details on hover:
- Tooltip configuration and formatting
- Custom tooltip templates
- Shared tooltips for multiple series
- Legend positioning and customization
- Toggle series visibility from legend
- Legend click events

### Customization and Styling

#### Appearance Customization

📄 **Read:** [references/appearance-customization.md](references/appearance-customization.md)

Customize the visual design:
- Stock chart title and subtitle
- Built-in themes (Material, Bootstrap, Fluent, Tailwind, etc.)
- Chart dimensions (width, height, responsive)
- Gradient fills for series
- Last data label display
- Border, background, and margin settings
- Custom CSS styling

#### Export and Print

📄 **Read:** [references/export-print.md](references/export-print.md)

Export charts for reports and sharing:
- Export to PNG, JPEG, SVG, PDF formats
- Export configuration and customization
- Print functionality with print-specific styling
- Exporting programmatically or via UI button

### Advanced Topics

#### Events

📄 **Read:** [references/events.md](references/events.md)

Handle stock chart events exposed by `StockChartEvents`:
- `OnLoaded` for post-render initialization
- `OnPointClick` for point selection scenarios
- `AxisLabelRendering` for axis label customization
- `PeriodChanged` and `RangeChange` for time navigation updates
- `OnZooming` for zoom interaction handling
- `TooltipRendering` and `SharedTooltipRendering` for tooltip customization
- `Exporting`, `ExportCompleted`, and `OnPrintComplete` for output workflows

#### Accessibility

📄 **Read:** [references/accessibility.md](references/accessibility.md)

Ensure accessible financial charts:
- WCAG 2.0 compliance features
- WAI-ARIA attributes
- Keyboard navigation support
- Screen reader compatibility
- High contrast theme support
- Focus management best practices

## Quick Start Example

Here's a minimal stock chart with candlestick series and period selector:

```cshtml
@page "/stock-chart"
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL Stock Price" Height="450px">
    <StockChartPrimaryXAxis ValueType="Syncfusion.Blazor.Charts.ValueType.DateTime">
    </StockChartPrimaryXAxis>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle" 
                          XName="Date" High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
    
    <StockChartPeriods>
        <StockChartPeriod Text="1M" Interval="1" IntervalType="RangeIntervalType.Months"></StockChartPeriod>
        <StockChartPeriod Text="3M" Interval="3" IntervalType="RangeIntervalType.Months"></StockChartPeriod>
        <StockChartPeriod Text="6M" Interval="6" IntervalType="RangeIntervalType.Months"></StockChartPeriod>
        <StockChartPeriod Text="1Y" Interval="1" IntervalType="RangeIntervalType.Years"></StockChartPeriod>
        <StockChartPeriod Text="All"></StockChartPeriod>
    </StockChartPeriods>
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
        new StockInfo { Date = new DateTime(2024, 02, 01), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7, Volume = 15000000 },
        new StockInfo { Date = new DateTime(2024, 03, 01), Open = 183.7, High = 190.5, Low = 182.5, Close = 188.2, Volume = 18000000 },
        // Add more data points...
    };
}
```

## Common Use Cases

### 1. Stock Price Dashboard with Technical Indicators

```cshtml
<SfStockChart Title="Stock Analysis with SMA and RSI">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle"
                          XName="Date" High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
    
    <StockChartIndicators>
        <StockChartIndicator Type="TechnicalIndicators.Sma" Period="14" Field="FinancialDataFields.Close">
        </StockChartIndicator>
        <StockChartIndicator Type="TechnicalIndicators.Rsi" Period="14" ShowZones="true">
        </StockChartIndicator>
    </StockChartIndicators>
</SfStockChart>
```

**When to use:** Investment analysis dashboards, trading platforms, portfolio monitoring

### 2. Multiple Series Comparison (e.g., Compare Multiple Stocks)

```cshtml
<SfStockChart Title="Tech Stocks Comparison">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@AppleData" Type="ChartSeriesType.Line" Name="AAPL"
                          XName="Date" YName="Close">
        </StockChartSeries>
        <StockChartSeries DataSource="@MicrosoftData" Type="ChartSeriesType.Line" Name="MSFT"
                          XName="Date" YName="Close">
        </StockChartSeries>
        <StockChartSeries DataSource="@GoogleData" Type="ChartSeriesType.Line" Name="GOOGL"
                          XName="Date" YName="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>
```

**When to use:** Portfolio comparison, sector analysis, benchmarking against indices

### 3. Stock Events with Earnings and Dividends

```cshtml
<SfStockChart Title="AAPL with Corporate Events">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle"
                          XName="Date" High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
    
    <StockChartEvents>
        <StockChartEvent Date="new DateTime(2024, 02, 01)" Text="Q1 Earnings" Type="@SplineType.Circle">
        </StockChartEvent>
        <StockChartEvent Date="new DateTime(2024, 03, 15)" Text="Dividend $0.24" Type="@SplineType.Square">
        </StockChartEvent>
    </StockChartEvents>
</SfStockChart>
```

**When to use:** Fundamental analysis, event-driven trading, corporate calendar visualization

### 4. Cryptocurrency Price Chart with Volume

```cshtml
<SfStockChart Title="Bitcoin Price and Volume">
    <StockChartRows>
        <StockChartRow Height="70%"></StockChartRow>
        <StockChartRow Height="30%"></StockChartRow>
    </StockChartRows>
    
    <StockChartAxes>
        <StockChartAxis Name="VolumeAxis" OpposedPosition="true" RowIndex="1">
        </StockChartAxis>
    </StockChartAxes>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@BitcoinData" Type="ChartSeriesType.Candle"
                          XName="Date" High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
        <StockChartSeries DataSource="@BitcoinData" Type="ChartSeriesType.Column" Name="Volume"
                          XName="Date" YName="Volume" YAxisName="VolumeAxis">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>
```

**When to use:** Cryptocurrency tracking, 24/7 market visualization, digital asset portfolios

### 5. Export Stock Chart to PDF Report

```cshtml
@using Syncfusion.Blazor.Charts

<button class="btn btn-primary" @onclick="ExportChart">Export to PDF</button>

<SfStockChart @ref="StockChartRef" Title="Annual Stock Performance">
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle"
                          XName="Date" High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    SfStockChart StockChartRef;
    
    private async Task ExportChart()
    {
        await StockChartRef.ExportAsync(ExportType.PDF, "StockReport");
    }
}
```

**When to use:** Financial reports, investor presentations, automated reporting systems

## Key Configuration Properties

### Essential Props for Stock Charts

| Property | Type | Purpose |
|----------|------|---------|
| `Title` | string | Main chart title |
| `Height` / `Width` | string | Chart dimensions (e.g., "450px", "100%") |
| `Theme` | Theme | Visual theme (Material, Bootstrap, Fluent, etc.) |
| `EnablePeriodSelector` | bool | Show/hide period selector (default: true) |
| `EnableSelector` | bool | Show/hide range selector (default: true) |
| `EnableCustomRange` | bool | Allow custom date range input |

### Official API Reference

📄 **Read:** [references/api-reference.md](references/api-reference.md)

This page summarizes the validated `SfStockChart` API surface, including:
- Core properties such as `Background`, `DataSource`, `SeriesType`, `IndicatorType`, `TrendlineType`, `Theme`, `Title`, `Height`, and `Width`
- Methods such as `ExportAsync()`, `PrintAsync()`, `Refresh()`, and `UpdateStockChart()`
- Child components such as `StockChartSeriesCollection`, `StockChartIndicators`, `StockChartTrendlines`, `StockChartPeriods`, `StockChartEvents`, and `StockChartAxes`

### Series Configuration

| Property | Type | Purpose |
|----------|------|---------|
| `Type` | ChartSeriesType | Series type (Candle, Line, Hilo, etc.) |
| `DataSource` | object | Data collection |
| `XName` | string | Property name for X-axis (Date) |
| `High` / `Low` / `Open` / `Close` | string | OHLC property names |
| `YName` | string | Property for Line/Spline series |
| `Fill` | string | Series color |

### Period Selector Props

| Property | Type | Purpose |
|----------|------|---------|
| `Text` | string | Button label (e.g., "1M", "6M", "1Y") |
| `Interval` | int | Number of intervals |
| `IntervalType` | RangeIntervalType | Days, Months, Years |
| `Selected` | bool | Default selected period |

## Next Steps

1. **Start with [Getting Started](references/getting-started.md)** for installation and basic setup
2. **Choose your series type** from [Series Types](references/series-types.md)
3. **Add technical indicators** from [Technical Indicators](references/technical-indicators.md)
4. **Implement time navigation** with [Period and Range Selectors](references/period-range-selectors.md)
5. **Customize appearance** using [Appearance Customization](references/appearance-customization.md)

For questions or issues, refer to [Accessibility](references/accessibility.md) for compliance requirements or [Events](references/events.md) for handling user interactions.
