# Axes Configuration

## Table of Contents
- [Overview](#overview)
- [Axis Types](#axis-types)
  - [DateTime Axis](#datetime-axis)
  - [DateTimeCategory Axis](#datetimecategory-axis)
  - [Logarithmic Axis](#logarithmic-axis)
- [Axis Customization](#axis-customization)
  - [Axis Titles](#axis-titles)
  - [Tick Lines](#tick-lines)
  - [Grid Lines](#grid-lines)
  - [Multiple Axes](#multiple-axes)
  - [Inversed Axis](#inversed-axis)
  - [Opposed Position](#opposed-position)
- [Common Scenarios](#common-scenarios)

## Overview

Stock Charts support multiple axis types and extensive customization options. Proper axis configuration is critical for accurate financial data visualization. The X-axis typically represents time, while the Y-axis shows price or volume.

**Key Concepts:**
- **Primary Axes**: Default X and Y axes configured via `StockChartPrimaryXAxis` and `StockChartPrimaryYAxis`
- **Secondary Axes**: Additional axes for overlaying different metrics (e.g., volume with price)
- **Axis Types**: DateTime (default), DateTimeCategory (business days), Logarithmic
- **Customization**: Titles, labels, tick lines, grid lines, ranges, formatting

## Axis Types

### DateTime Axis

The standard time-based axis for continuous date/time data. This is the default and most common axis type for stock charts. Set the `ValueType` property to `Syncfusion.Blazor.Charts.ValueType.DateTime` on the `StockChartPrimaryXAxis` component to use DateTime axis. Use `LabelFormat` property to customize the date/time display format.

**When to Use:**
- Continuous time series data
- Intraday, daily, weekly, monthly, yearly charts
- When weekends/holidays should appear in the timeline

**Implementation:**

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Stock Price - Continuous Timeline">
    <StockChartPrimaryXAxis ValueType="Syncfusion.Blazor.Charts.ValueType.DateTime">
    </StockChartPrimaryXAxis>
    
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
        new StockInfo { Date = new DateTime(2024, 01, 02), Close = 176.8 },
        new StockInfo { Date = new DateTime(2024, 01, 03), Close = 178.2 },
        new StockInfo { Date = new DateTime(2024, 01, 04), Close = 177.5 },
        new StockInfo { Date = new DateTime(2024, 01, 05), Close = 179.1 }
    };
}
```

**Date Formatting:**

```razor
<StockChartPrimaryXAxis ValueType="Syncfusion.Blazor.Charts.ValueType.DateTime"
                        LabelFormat="MMM dd, yyyy">
</StockChartPrimaryXAxis>
```

Common formats:
- `"MMM dd"` → Jan 15
- `"MM/dd/yyyy"` → 01/15/2024
- `"yyyy-MM-dd"` → 2024-01-15
- `"MMM yyyy"` → Jan 2024

### DateTimeCategory Axis

Displays only business days, automatically skipping weekends and gaps in data. Perfect for stock market data where trading doesn't occur on weekends/holidays. Set the `ValueType` property to `Syncfusion.Blazor.Charts.ValueType.DateTimeCategory` on the `StockChartPrimaryXAxis` component to enable business day axis. The axis automatically handles weekend and holiday gaps without displaying empty spaces.

**When to Use:**
- Stock market data (trading days only)
- Business day analysis
- When gaps in data should not appear as empty space
- Comparing daily trading patterns

**Implementation:**

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Trading Days Only">
    <StockChartPrimaryXAxis ValueType="Syncfusion.Blazor.Charts.ValueType.DateTimeCategory">
        <StockChartAxisCrosshairTooltip Enable="true"></StockChartAxisCrosshairTooltip>
    </StockChartPrimaryXAxis>
    
    <StockChartCrosshairSettings Enable="true"></StockChartCrosshairSettings>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@TradingDayData" 
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
    public List<StockInfo> TradingDayData = new List<StockInfo>
    {
        // Note: Weekends automatically skipped
        new StockInfo { Date = new DateTime(2024, 01, 08), Close = 175.5 }, // Monday
        new StockInfo { Date = new DateTime(2024, 01, 09), Close = 176.8 }, // Tuesday
        new StockInfo { Date = new DateTime(2024, 01, 10), Close = 178.2 }, // Wednesday
        // Thursday/Friday data...
        new StockInfo { Date = new DateTime(2024, 01, 15), Close = 181.3 }, // Next Monday - no weekend gap shown
    };
}
```

**Benefits:**
- Cleaner visualization without non-trading day gaps
- Equal spacing between trading days
- Better for pattern recognition
- More accurate for business day calculations

### Logarithmic Axis

Uses logarithmic scale for Y-axis, useful when data spans multiple orders of magnitude or when showing percentage changes. Set the `ValueType` property to `Syncfusion.Blazor.Charts.ValueType.Logarithmic` on the `StockChartPrimaryYAxis` component. Use the `LogBase` property to specify the logarithm base (default is 10). All data values must be positive since logarithms of zero or negative numbers are undefined.

**When to Use:**
- Long-term charts with significant price changes
- Comparing stocks with vastly different prices
- Highlighting percentage changes over absolute changes
- Volatility analysis

**Implementation:**

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart>
    <StockChartPrimaryXAxis ValueType="@Syncfusion.Blazor.Charts.ValueType.DateTime">
    </StockChartPrimaryXAxis>
    <StockChartPrimaryYAxis ValueType="@Syncfusion.Blazor.Charts.ValueType.Logarithmic">
    </StockChartPrimaryYAxis>
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockDetails" Type="ChartSeriesType.Area" XName="Date" YName="Y">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public class ChartData
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }
    public List<ChartData> StockDetails = new List<ChartData>
    {
        new ChartData { Date = new DateTime(2012, 04, 02), Close = 100 },
        new ChartData { Date = new DateTime(2012, 04, 09), Close = 10 },
        new ChartData { Date = new DateTime(2012, 04, 16), Close = 500 },
        new ChartData { Date = new DateTime(2012, 04, 23), Close = 80 },
        new ChartData { Date = new DateTime(2012, 04, 30), Close = 200 },
        new ChartData { Date = new DateTime(2012, 05, 07), Close = 600 },
        new ChartData { Date = new DateTime(2012, 05, 14), Close = 50 },
        new ChartData { Date = new DateTime(2012, 05, 21), Close = 700 },
        new ChartData { Date = new DateTime(2012, 05, 28), Close = 90 }
   };
}
```

**Log Base Options:**
- `LogBase="10"` - Base-10 logarithm (default, most common)
- `LogBase="2"` - Base-2 logarithm
- `LogBase="Math.E"` - Natural logarithm (ln)

## Axis Customization

### Tick Lines

Customize the small lines marking values on axes using the `StockChartAxisMajorTickLines` and `StockChartAxisMinorTickLines` components. Use `StockChartAxisMajorTickLines` for primary tick marks and `StockChartAxisMinorTickLines` for secondary tick marks on both `StockChartPrimaryXAxis` and `StockChartPrimaryYAxis`.

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Custom Tick Lines">
    <StockChartPrimaryXAxis>
        <StockChartAxisMajorTickLines Width="3" Color="#3498db"></StockChartAxisMajorTickLines>
        <StockChartAxisMinorTickLines Width="1" Color="#95a5a6"></StockChartAxisMinorTickLines>
    </StockChartPrimaryXAxis>
    
    <StockChartPrimaryYAxis>
        <StockChartAxisMajorTickLines Width="3" Color="#e74c3c"></StockChartAxisMajorTickLines>
        <StockChartAxisMinorTickLines Width="0"></StockChartAxisMinorTickLines> <!-- Hide minor ticks -->
    </StockChartPrimaryYAxis>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Line"
                          XName="Date" YName="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public class ChartData
    {
        public DateTime Date { get; set; }
        public Double Close { get; set; }
    }
    public List<ChartData> StockData = new List<ChartData>
    {
        new ChartData { Date = new DateTime(2012, 04, 02), Close = 100 },
        new ChartData { Date = new DateTime(2012, 04, 09), Close = 10 },
        new ChartData { Date = new DateTime(2012, 04, 16), Close = 500 },
        new ChartData { Date = new DateTime(2012, 04, 23), Close = 80 },
        new ChartData { Date = new DateTime(2012, 04, 30), Close = 200 },
        new ChartData { Date = new DateTime(2012, 05, 07), Close = 600 },
        new ChartData { Date = new DateTime(2012, 05, 14), Close = 50 },
        new ChartData { Date = new DateTime(2012, 05, 21), Close = 700 },
        new ChartData { Date = new DateTime(2012, 05, 28), Close = 90 }
   };
}
```

**Properties:**
- `Width` - Thickness of tick line (set to 0 to hide)
- `Color` - Tick line color (hex or named colors)
- `Height` - Length of tick line

### Grid Lines

Customize the lines that extend from tick marks across the chart using the `StockChartAxisMajorGridLines` and `StockChartAxisMinorGridLines` components. Apply these to both `StockChartPrimaryXAxis` and `StockChartPrimaryYAxis`. The `DashArray` property creates dashed patterns (e.g., "5,5" for dashed lines, "0" for solid).

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Custom Grid Lines">
    <StockChartPrimaryXAxis>
        <StockChartAxisMajorGridLines Width="2" Color="#ecf0f1" DashArray="5,5">
        </StockChartAxisMajorGridLines>
        <StockChartAxisMinorGridLines Width="0"></StockChartAxisMinorGridLines>
    </StockChartPrimaryXAxis>
    
    <StockChartPrimaryYAxis>
        <StockChartAxisMajorGridLines Width="2" Color="#bdc3c7">
        </StockChartAxisMajorGridLines>
        <StockChartAxisMinorGridLines Width="0"></StockChartAxisMinorGridLines>
    </StockChartPrimaryYAxis>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockDetails" Type="ChartSeriesType.Candle"
                          XName="Date" High="High" Low="Low" Open="Open" Close="Close">
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

    public List<StockChartData> StockDetails = new List<StockChartData>
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

**Properties:**
- `Width` - Line thickness (set to 0 to hide the grid lines)
- `Color` - Grid line color (hex or named colors)
- `DashArray` - Pattern for dashed lines (e.g., "5,5" for dashed, "0" for solid)

### Multiple Axes

Add secondary axes for displaying related but differently-scaled metrics (e.g., price + volume). Use the `StockChartRows` component to divide the chart vertically, then create additional axes using the `StockChartAxis` component within `StockChartAxes`. Set the `Name` property to identify the axis, use `RowIndex` to specify its position, and link series to the axis using the `YAxisName` property. The `OpposedPosition` property moves the axis to the opposite side.

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Price and Volume">
    <StockChartRows>
        <StockChartRow Height="70%"></StockChartRow>
        <StockChartRow Height="30%"></StockChartRow>
    </StockChartRows>
    
    <StockChartAxes>
        <StockChartAxis Name="VolumeAxis" OpposedPosition="true" RowIndex="1">
            <StockChartAxisMajorGridLines Width="0"></StockChartAxisMajorGridLines>
        </StockChartAxis>
    </StockChartAxes>
    
    <StockChartSeriesCollection>
        <!-- Price series on primary Y-axis -->
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Candle" 
                          XName="Date" High="High" Low="Low" Open="Open" Close="Close"
                          Name="AAPL Price">
        </StockChartSeries>
        
        <!-- Volume series on secondary Y-axis -->
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Column" 
                          XName="Date" YName="Volume"
                          YAxisName="VolumeAxis"
                          Name="Volume">
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
        public long Volume { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5, Volume = 12000000 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7, Volume = 15000000 }
    };
}
```

**Key Points:**
- Use `StockChartRows` with `Height` property to divide chart vertically
- Use `StockChartAxis` with `Name` property to create secondary axes
- Assign `RowIndex` property to position axis in specific row
- Link series to axis using `YAxisName` property on `StockChartSeries`
- Use `OpposedPosition` property to move axis to opposite side
- Each axis can have independent scaling and formatting

### Inversed Axis

Flip axis direction (useful for certain analysis patterns). Set the `IsInversed` property to `true` on either `StockChartPrimaryXAxis` or `StockChartPrimaryYAxis` to reverse the axis direction.

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Inversed Time Axis">
    <StockChartPrimaryXAxis IsInversed="true">
    </StockChartPrimaryXAxis>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Line"
                          XName="Date" YName="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public class ChartData
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }
    public List<ChartData> StockData = new List<ChartData>
    {
        new ChartData { Date = new DateTime(2012, 04, 02), Close = 100 },
        new ChartData { Date = new DateTime(2012, 04, 09), Close = 10 },
        new ChartData { Date = new DateTime(2012, 04, 16), Close = 500 },
        new ChartData { Date = new DateTime(2012, 04, 23), Close = 80 },
        new ChartData { Date = new DateTime(2012, 04, 30), Close = 200 },
        new ChartData { Date = new DateTime(2012, 05, 07), Close = 600 },
        new ChartData { Date = new DateTime(2012, 05, 14), Close = 50 },
        new ChartData { Date = new DateTime(2012, 05, 21), Close = 700 },
        new ChartData { Date = new DateTime(2012, 05, 28), Close = 90 }
    };
}
```

**When to Use:**
- Reading right-to-left layouts
- Specific analysis patterns
- Custom visualization requirements

**Property:**
- `IsInversed` - Set to `true` to reverse axis direction (boolean)

### Opposed Position

Move axis to opposite side of chart. Set the `OpposedPosition` property to `true` on `StockChartPrimaryXAxis` or `StockChartPrimaryYAxis` to move the axis to the opposite side. This is commonly used to place the Y-axis on the right side or X-axis on the top.

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Y-Axis on Right Side">
    <StockChartPrimaryYAxis OpposedPosition="true">
    </StockChartPrimaryYAxis>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle"
                          XName="Date" High="High" Low="Low" Open="Open" Close="Close">
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
        public long Volume { get; set; }
    }
    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5, Volume = 12000000 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7, Volume = 15000000 }
    };
}
```

**Typical Usage:**
- Y-axis on right (common in financial applications)
- X-axis on top
- Better alignment with other UI elements

**Property:**
- `OpposedPosition` - Set to `true` to move axis to opposite side (boolean)

## Common Scenarios

### Scenario 1: Basic Price Chart

This scenario demonstrates a basic stock price chart using the `DateTimeCategory` axis type for trading days only. The `StockChartPrimaryXAxis` is configured with the `ValueType` property set to show only business days.

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL - Daily Prices">
    <StockChartPrimaryXAxis ValueType="Syncfusion.Blazor.Charts.ValueType.DateTimeCategory">
    </StockChartPrimaryXAxis>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle"
                          XName="Date" High="High" Low="Low" Open="Open" Close="Close">
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
        public long Volume { get; set; }
    }
    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5, Volume = 12000000 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7, Volume = 15000000 }
    };
}
```

### Scenario 2: Price and Volume Split View

This scenario creates a split view displaying price and volume separately. The `StockChartRows` with `Height` properties divides the chart into two sections. A secondary axis named "VolumeAxis" is created using `StockChartAxis` with `RowIndex="1"` to position it in the lower section. The volume series links to this secondary axis using the `YAxisName` property.

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL - Price and Volume">
    <StockChartRows>
        <StockChartRow Height="75%"></StockChartRow>
        <StockChartRow Height="25%"></StockChartRow>
    </StockChartRows>
    
    <StockChartAxes>
        <StockChartAxis Name="VolumeAxis" RowIndex="1" OpposedPosition="true">
        </StockChartAxis>
    </StockChartAxes>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle"
                          XName="Date" High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Column"
                          XName="Date" YName="Volume" YAxisName="VolumeAxis">
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
        public long Volume { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5, Volume = 12000000 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7, Volume = 15000000 }
    };
}
```

### Scenario 3: Comparing Multiple Stocks

This scenario compares multiple stocks on the same chart. The `StockChartPrimaryXAxis` uses `DateTime` axis type for continuous time series data. Multiple series are defined for different stocks (AAPL, MSFT, GOOGL) using the `StockChartSeriesCollection` with distinct `Name` and `Fill` properties for identification.

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Tech Stocks Comparison">
    <StockChartPrimaryXAxis ValueType="Syncfusion.Blazor.Charts.ValueType.DateTime">
    </StockChartPrimaryXAxis>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@AaplData" Type="ChartSeriesType.Line" Name="AAPL"
                          XName="Date" YName="Close" Fill="#3498db">
        </StockChartSeries>
        <StockChartSeries DataSource="@MsftData" Type="ChartSeriesType.Line" Name="MSFT"
                          XName="Date" YName="Close" Fill="#e74c3c">
        </StockChartSeries>
        <StockChartSeries DataSource="@GooglData" Type="ChartSeriesType.Line" Name="GOOGL"
                          XName="Date" YName="Close" Fill="#2ecc71">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public class StockData
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }

    public List<StockData> AaplData = new()
    {
        new StockData { Date = new DateTime(2024, 01, 01), Close = 180 },
        new StockData { Date = new DateTime(2024, 01, 02), Close = 182 },
        new StockData { Date = new DateTime(2024, 01, 03), Close = 185 },
        new StockData { Date = new DateTime(2024, 01, 04), Close = 183 },
        new StockData { Date = new DateTime(2024, 01, 05), Close = 187 }
    };

    public List<StockData> MsftData = new()
    {
        new StockData { Date = new DateTime(2024, 01, 01), Close = 368 },
        new StockData { Date = new DateTime(2024, 01, 02), Close = 371 },
        new StockData { Date = new DateTime(2024, 01, 03), Close = 374 },
        new StockData { Date = new DateTime(2024, 01, 04), Close = 372 },
        new StockData { Date = new DateTime(2024, 01, 05), Close = 376 }
    };

    public List<StockData> GooglData = new()
    {
        new StockData { Date = new DateTime(2024, 01, 01), Close = 140 },
        new StockData { Date = new DateTime(2024, 01, 02), Close = 141 },
        new StockData { Date = new DateTime(2024, 01, 03), Close = 143 },
        new StockData { Date = new DateTime(2024, 01, 04), Close = 142 },
        new StockData { Date = new DateTime(2024, 01, 05), Close = 145 }
    };
}
```

## Common Issues

### Issue: Axis labels overlapping

**Solutions:**
1. Adjust `LabelFormat` property to shorter format (e.g., "MMM dd" instead of "MMM dd, yyyy")
2. Use `StockChartAxisLabelStyle` with `Angle` property to rotate labels: `<StockChartAxisLabelStyle Angle="-45"></StockChartAxisLabelStyle>`
3. Increase chart width using `Width` property on `SfStockChart`
4. Use `Interval` property on axis to skip labels

### Issue: DateTimeCategory not skipping weekends

**Solutions:**
1. Verify `ValueType="Syncfusion.Blazor.Charts.ValueType.DateTimeCategory"` on `StockChartPrimaryXAxis`
2. Ensure data only contains trading days
3. Check DateTime values are correctly formatted

### Issue: Logarithmic axis showing incorrect scale

**Solutions:**
1. Ensure all Y values are positive (log of negative/zero is undefined)
2. Check `LogBase` property setting on `StockChartPrimaryYAxis`
3. Verify data range isn't too narrow for logarithmic display

