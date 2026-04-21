# Tooltip and Legend

This guide explains how to configure and customize tooltips and legends in Stock Charts to enhance data visualization and user interaction.

## Table of Contents

- [Overview](#overview)
- [Default Tooltip](#default-tooltip)
- [Tooltip Formatting](#tooltip-formatting)
- [Shared Tooltips](#shared-tooltips)
- [Tooltip Templates](#tooltip-templates)
- [Legend Configuration](#legend-configuration)
- [Legend Positioning](#legend-positioning)
- [Legend Customization](#legend-customization)
- [Toggle Series with Legend](#toggle-series-with-legend)
- [Legend Click Events](#legend-click-events)
- [When to Use](#when-to-use)
- [Common Issues](#common-issues)

## Overview

Tooltips display detailed information when hovering over data points, while legends identify different series in the chart. The Stock Chart provides extensive customization options for both features.

**Key Features:**
- Auto-formatted tooltips with stock data
- Custom tooltip templates with HTML/Razor content
- Shared tooltips for multiple series
- Flexible legend positioning and styling
- Toggle series visibility via legend clicks

## Default Tooltip

Tooltips are enabled by default and display stock information automatically. The `StockChartTooltipSettings` component with the `Enable` property set to `true` activates the default tooltip functionality:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL Stock Price">
    <StockChartTooltipSettings Enable="true"></StockChartTooltipSettings>
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Candle" 
                          XName="Date" 
                          High="High" 
                          Low="Low" 
                          Open="Open" 
                          Close="Close">
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
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7 },
        new StockInfo { Date = new DateTime(2024, 01, 15), Open = 183.7, High = 190.5, Low = 182.5, Close = 188.2 }
    };
}
```

The default tooltip displays:
- Date/time
- Open, High, Low, Close values
- Volume (if provided)
- Series name

## Tooltip Formatting

Customize tooltip content using the `StockChartTooltipSettings` component with the `Format` property to define custom HTML content for the tooltip display:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Stock Price with Custom Tooltip">
    <StockChartTooltipSettings Enable="true" 
                               Format="<b>${point.x}</b><br/>Open: <b>${point.open}</b><br/>Close: <b>${point.close}</b>">
    </StockChartTooltipSettings>
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Candle" 
                          XName="Date" 
                          High="High" 
                          Low="Low" 
                          Open="Open" 
                          Close="Close">
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
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7 },
        new StockInfo { Date = new DateTime(2024, 01, 15), Open = 183.7, High = 190.5, Low = 182.5, Close = 188.2 }
    };
}
```

**Available Placeholder Variables:**
- `${point.x}` - X-axis value (Date)
- `${point.y}` - Y-axis value
- `${point.open}` - Opening price
- `${point.high}` - Highest price
- `${point.low}` - Lowest price
- `${point.close}` - Closing price
- `${point.volume}` - Trading volume
- `${series.name}` - Series name

### Formatting Numbers and Dates

```razor
<StockChartTooltipSettings Enable="true" 
                           Format="<b>${point.x:MMM dd, yyyy}</b><br/>Price: <b>${point.close:C2}</b>">
</StockChartTooltipSettings>
```

## Shared Tooltips

Display information for all series at a specific point using the `StockChartTooltipSettings` component with the `Shared` property set to `true`:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Multiple Stocks Comparison">
    <StockChartTooltipSettings Enable="true" Shared="true"></StockChartTooltipSettings>
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@AAPLData" 
                          Name="AAPL"
                          Type="ChartSeriesType.Line" 
                          XName="Date" 
                          YName="Close">
        </StockChartSeries>
        <StockChartSeries DataSource="@MSFTData" 
                          Name="MSFT"
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

    public List<StockInfo> AAPLData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Close = 183.7 }
    };

    public List<StockInfo> MSFTData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 370.2 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Close = 375.8 }
    };
}
```

## Tooltip Templates

Create fully customized tooltips using the `StockChartTooltipSettings` component with the `Template` child element to provide custom Razor markup for tooltip rendering:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Stock with Custom Tooltip Template">
    <StockChartTooltipSettings Enable="true">
        <Template>
            @{
                dynamic data = context;
            }
            <div>
                <div class="tooltip-header">@data[0].X</div>
                <table>
                    <tr><td>Open:</td><td><b>$@data[0].Open</b></td></tr>
                    <tr><td>High:</td><td><b>$@data[0].High</b></td></tr>
                    <tr><td>Low:</td><td><b>$@data[0].Low</b></td></tr>
                    <tr><td>Close:</td><td><b>$@data[0].Close</b></td></tr>
                    <tr><td>Change:</td><td><b style="color: @GetChangeColor(data)">@GetChange(data)</b></td></tr>
                </table>
            </div>
        </Template>
    </StockChartTooltipSettings>
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Candle" 
                          XName="Date" 
                          High="High" 
                          Low="Low" 
                          Open="Open" 
                          Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

<style>
    .tooltip-header {
        font-weight: bold;
        font-size: 14px;
        margin-bottom: 8px;
        color: #0066cc;
    }
    .custom-tooltip table { width: 100%; }
    .custom-tooltip td { padding: 2px 8px; }
</style>

@code {
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double High { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7 }
    };

    private string GetChange(dynamic data)
    {
        double open = Convert.ToDouble(data[0].Open);
        double close = Convert.ToDouble(data[0].Close);

        var change = close - open;
        var percent = (change / open) * 100;
        return $"{(change >= 0 ? "+" : "")}{change:F2} ({percent:F2}%)";
    }

    private string GetChangeColor(dynamic data)
    {
        double open = Convert.ToDouble(data[0].Open);
        double close = Convert.ToDouble(data[0].Close);

        return close >= open ? "#00b300" : "#ff0000";
    }
}
```

## Legend Configuration

Enable and configure the legend using the `StockChartLegendSettings` component with the `Visible` property set to `true`:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Stock Comparison with Legend">
    <StockChartLegendSettings Visible="true"></StockChartLegendSettings>
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@AAPLData" 
                          Name="Apple Inc."
                          Type="ChartSeriesType.Line" 
                          XName="Date" 
                          YName="Close">
        </StockChartSeries>
        <StockChartSeries DataSource="@MSFTData" 
                          Name="Microsoft Corp."
                          Type="ChartSeriesType.Line" 
                          XName="Date" 
                          YName="Close">
        </StockChartSeries>
        <StockChartSeries DataSource="@GOOGData" 
                          Name="Alphabet Inc."
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

    public List<StockInfo> AAPLData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 02, 01), Close = 183.7 }
    };

    public List<StockInfo> MSFTData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 370.2 },
        new StockInfo { Date = new DateTime(2024, 02, 01), Close = 375.8 }
    };

    public List<StockInfo> GOOGData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 140.5 },
        new StockInfo { Date = new DateTime(2024, 02, 01), Close = 145.3 }
    };
}
```

## Legend Positioning

Control legend placement using the `StockChartLegendSettings` component with the `Position` property to specify placement location and `Alignment` property to control alignment within the position:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Stock Chart with Top Legend">
    <StockChartLegendSettings Visible="true" 
                              Position="LegendPosition.Top"
                              Alignment="Alignment.Center">
    </StockChartLegendSettings>
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Name="AAPL"
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
        public double Open { get; set; }
        public double High { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7 },
        new StockInfo { Date = new DateTime(2024, 01, 15), Open = 183.7, High = 190.5, Low = 182.5, Close = 188.2 }
    };
}
```

**Available Positions:**
- `LegendPosition.Top` - Above chart
- `LegendPosition.Bottom` - Below chart (default)
- `LegendPosition.Left` - Left side
- `LegendPosition.Right` - Right side
- `LegendPosition.Custom` - Custom X, Y coordinates

**Alignment Options:**
- `Alignment.Near` - Start of container
- `Alignment.Center` - Middle of container
- `Alignment.Far` - End of container

## Legend Customization

Customize legend appearance using the `StockChartLegendSettings` component with properties like `Background`, `Padding`, `ShapeHeight`, `ShapeWidth`, `BorderColor`, and `BorderWidth` along with child elements `StockChartLegendMargin` and `StockChartLegendTextStyle`:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Customized Legend">
    <StockChartLegendSettings Visible="true" 
                              Position="LegendPosition.Right"
                              Background="#f0f0f0"
                              Padding="3"
                              ShapeHeight="15"
                              ShapeWidth="15"
                              BorderColor="#cccccc"
                              BorderWidth="1">
        <StockChartLegendMargin Left="10" Top="10" Right="10" Bottom="10"></StockChartLegendMargin>
        <StockChartLegendTextStyle FontFamily = "Arial" Size = "14px" Color = "#333333"></StockChartLegendTextStyle>
    </StockChartLegendSettings>
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Name="Technology Stocks"
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
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Close = 183.7 }
    };
}
```

## Toggle Series with Legend

Enable series visibility toggling by clicking legend items using the `StockChartLegendSettings` component with the `ToggleVisibility` property set to `true`:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Toggle Series via Legend">
    <StockChartLegendSettings Visible="true" 
                              ToggleVisibility="true">
    </StockChartLegendSettings>
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@AAPLData" 
                          Name="AAPL"
                          Type="ChartSeriesType.Line" 
                          XName="Date" 
                          YName="Close">
        </StockChartSeries>
        <StockChartSeries DataSource="@MSFTData" 
                          Name="MSFT"
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

    public List<StockInfo> AAPLData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 02, 01), Close = 183.7 }
    };

    public List<StockInfo> MSFTData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 370.2 },
        new StockInfo { Date = new DateTime(2024, 02, 01), Close = 375.8 }
    };
}
```

Set `ToggleVisibility="true"` to allow users to show/hide series by clicking legend items.

## When to Use

**Use Tooltips When:**
- Displaying detailed information on hover
- Showing multiple data points simultaneously (shared tooltip)
- Providing contextual information without cluttering the chart
- Users need precise values at specific points

**Use Legends When:**
- Multiple series need identification
- Users need to toggle series visibility
- Color coding requires explanation
- Comparing multiple datasets

**Best Practices:**
- Keep tooltip content concise and relevant
- Use shared tooltips for multi-series comparisons
- Position legends to avoid overlapping chart data
- Enable toggle visibility for better user control
- Use custom templates for complex tooltip layouts

## Common Issues

### Tooltip Not Appearing

**Problem:** Tooltip doesn't show on hover.

**Solution:**
```razor
<!-- Ensure Enable is set to true -->
<StockChartTooltipSettings Enable="true"></StockChartTooltipSettings>
```

### Tooltip Template Context is Null

**Problem:** Template context doesn't contain expected data.

**Solution:**
```razor
<Template>
    @{
        var data = context as StockChartTooltipInfo;
        if (data != null)
        {
            <div>@data.Close</div>
        }
    }
</Template>
```

### Legend Not Visible

**Problem:** Legend doesn't appear even when enabled.

**Solution:**
```razor
<!-- Ensure series have Name property set -->
<StockChartSeries DataSource="@StockData" 
                  Name="AAPL Stock"
                  XName="Date" 
                  YName="Close">
</StockChartSeries>
```

### Legend Overlaps Chart

**Problem:** Legend covers important chart areas.

**Solution:**
```razor
<!-- Adjust position or use custom coordinates -->
<StockChartLegendSettings Visible="true" 
                          Position="LegendPosition.Bottom">
</StockChartLegendSettings>
```

### Shared Tooltip Shows Misaligned Data

**Problem:** Shared tooltip displays data from wrong series.

**Solution:** Ensure all series have synchronized X-axis values (dates) and use the same `XName` property.

