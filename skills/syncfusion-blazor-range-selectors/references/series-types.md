# Series Types

Learn about the different series types available in the Syncfusion Blazor Range Selector and how to customize their appearance.

## API Reference - Series Types

**Related Classes & Enums:**
- `RangeNavigatorSeries` - Series configuration class
- `RangeNavigatorSeriesCollection` - Collection container
- `RangeNavigatorSeriesBorder` - Series border customization
- `RangeNavigatorType` - Enum: Line, Area, StepLine

**Series Properties:**
- `Type` - RangeNavigatorType - Series visualization type
- `DataSource` - object - Data collection
- `XName` - string - X-axis field name
- `YName` - string - Y-axis field name
- `Fill` - string - Series color (hex/RGB/named)
- `Width` - double - Line width in pixels (default: 1)
- `Opacity` - double - Series opacity 0-1 (default: 1)
- `Name` - string - Series identifier

**Border Properties:**
- `RangeNavigatorSeriesBorder.Color` - Border color
- `RangeNavigatorSeriesBorder.Width` - Border width in pixels

For complete API details, see [api-reference.md](api-reference.md).

## Table of Contents

- [Overview](#overview)
- [Line Series](#line-series)
    - [Basic Line Series](#basic-line-series)
    - [Customize Line Appearance](#customize-line-appearance)
    - [When to Use Line Series](#when-to-use-line-series)
- [Area Series](#area-series)
    - [Basic Area Series](#basic-area-series)
    - [Customize Area Appearance](#customize-area-appearance)
    - [Semi-Transparent Area](#semi-transparent-area)
    - [When to Use Area Series](#when-to-use-area-series)
- [StepLine Series](#stepline-series)
    - [Basic StepLine Series](#basic-stepline-series)
    - [Customize StepLine Appearance](#customize-stepline-appearance)
    - [When to Use StepLine Series](#when-to-use-stepline-series)
- [Multiple Series](#multiple-series)
- [Series Comparison](#series-comparison)
    - [Line Series](#line-series-1)
    - [Area Series](#area-series-1)
    - [StepLine Series](#stepline-series-1)
- [Complete Example: Series Type Selector](#complete-example-series-type-selector)
- [Best Practices](#best-practices)
    - [1. Choose the Right Series Type](#1-choose-the-right-series-type)
    - [2. Use Appropriate Colors](#2-use-appropriate-colors)
    - [3. Consider Performance](#3-consider-performance)
    - [4. Multiple Series Clarity](#4-multiple-series-clarity)
- [Practical Examples](#practical-examples)
- [Common Scenarios](#common-scenarios)

## Overview
Configure series visualization using `RangeNavigatorSeries` component with `Type` property (Line, Area, StepLine), `DataSource`, `XName`, `YName`, and styling properties like `Fill`, `Width`, and `RangeNavigatorSeriesBorder`.

The Range Selector supports three series types for visualizing data trends:
1. **Line**: Simple line connecting data points (default)
2. **Area**: Filled area under the line
3. **StepLine**: Step-like lines connecting data points

Each series type provides different visual emphasis and is suited for different data scenarios.

## Line Series
Display data using `RangeNavigatorSeries` with `Type="RangeNavigatorType.Line"`, `DataSource`, `XName`, `YName` for trend visualization.

The Line series type displays data as a continuous line connecting data points. This is the default series type.

### Basic Line Series
Create basic line series using `RangeNavigatorSeries` with `Type="RangeNavigatorType.Line"`:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close"
                              Type="RangeNavigatorType.Line">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public object Range = new DateTime[] 
    { 
        new DateTime(2023, 01, 01), 
        new DateTime(2023, 12, 31) 
    };
    
    public List<StockInfo> StockData = GetStockData();

    private static List<StockInfo> GenerateStockData()
    {
        var data = new List<StockInfo>();
        var startDate = new DateTime(2020, 01, 01);
        var random = new Random();
        double price = 100;
        
        for (int i = 0; i < 1095; i++) // 3 years of data
        {
            price += (random.NextDouble() - 0.5) * 5;
            data.Add(new StockInfo
            {
                Date = startDate.AddDays(i),
                Close = Math.Round(price, 2)
            });
        }
        
        return data;
    }
    
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }
}
```

### Customize Line Appearance
Customize line appearance using `Fill` property for color and `Width` property for thickness:

```razor
<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close"
                              Type="RangeNavigatorType.Line"
                              Fill="#3F51B5"
                              Width="2">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>
```

**Properties:**
- **Fill**: Line color (hex, RGB, or named color)
- **Width**: Line thickness in pixels

### When to Use Line Series
Choose Line series when your use case requires:
- Showing clear trends in data
- Comparing multiple series (coming in multiple series section)
- When minimal visual noise is desired
- Displaying stock prices or financial data
- Temperature or weather data over time

## Area Series
Display data using `RangeNavigatorSeries` with `Type="RangeNavigatorType.Area"`, `Fill` for color, and `RangeNavigatorSeriesBorder` for border styling.

The Area series fills the region below the line, providing visual emphasis on the magnitude of values.

### Basic Area Series
Create basic area series using `RangeNavigatorSeries` with `Type="RangeNavigatorType.Area"`:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@SalesData" 
                              XName="Month" 
                              YName="Revenue"
                              Type="RangeNavigatorType.Area">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public object Range = new DateTime[] 
    { 
        new DateTime(2023, 01, 01), 
        new DateTime(2023, 06, 30) 
    };
    
    public List<SalesData> SalesData = GetSalesData();
}
```

### Customize Area Appearance
Control fill color and border using `Fill` and `RangeNavigatorSeriesBorder` properties:

```razor
<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@RevenueData" 
                              XName="Date" 
                              YName="Amount"
                              Type="RangeNavigatorType.Area"
                              Fill="rgba(0, 123, 255, 0.3)"
                              Width="1">
            <RangeNavigatorSeriesBorder Color="#007bff" Width="2"></RangeNavigatorSeriesBorder>
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>
```

**Properties:**
- **Fill**: Area fill color (supports transparency with rgba)
- **Width**: Border line width
- **RangeNavigatorSeriesBorder**: Border styling (color and width)

### Semi-Transparent Area
Use rgba values in `Fill` property for transparency and visual layering:

```razor
<RangeNavigatorSeries DataSource="@Data" 
                      XName="X" 
                      YName="Y"
                      Type="RangeNavigatorType.Area"
                      Fill="rgba(75, 192, 192, 0.4)">
</RangeNavigatorSeries>
```

### When to Use Area Series
Choose Area series when your use case requires:
- Emphasizing volume or magnitude
- Showing cumulative data
- Revenue or sales visualization
- Traffic or usage patterns
- When you want a bold visual presence
- Comparing against a baseline (zero line)

## StepLine Series
Display data using `RangeNavigatorSeries` with `Type="RangeNavigatorType.StepLine"`, `Fill` and `Width` properties for step visualization.

The StepLine series displays data as horizontal and vertical line segments, creating a step-like appearance. Ideal for data that changes at specific intervals.

### Basic StepLine Series
Create basic step line series using `RangeNavigatorSeries` with `Type="RangeNavigatorType.StepLine"`:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockLevelData" 
                              XName="Date" 
                              YName="Quantity"
                              Type="RangeNavigatorType.StepLine">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public object Range = new DateTime[] 
    { 
        new DateTime(2023, 01, 01), 
        new DateTime(2023, 03, 31) 
    };
    
    public List<InventoryData> StockLevelData = GetInventoryData();
}
```

### Customize StepLine Appearance
Customize step line using `Fill` and `Width` properties:

```razor
<RangeNavigatorSeries DataSource="@StatusData" 
                      XName="Timestamp" 
                      YName="Status"
                      Type="RangeNavigatorType.StepLine"
                      Fill="#FF6384"
                      Width="2">
</RangeNavigatorSeries>
```

### When to Use StepLine Series
Choose StepLine series when your use case requires:
- Inventory levels that change at specific times
- Status changes (on/off, active/inactive)
- Discrete data points
- Showing exactly when values changed
- Financial instruments with price steps
- System states or configurations over time

## Multiple Series
Display multiple series for comparison using multiple `RangeNavigatorSeries` components with distinct `Fill` colors and `RangeNavigatorSeriesBorder` styling:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  LabelFormat="MMM">
    <RangeNavigatorSeriesCollection>
        <!-- Product A sales -->
        <RangeNavigatorSeries DataSource="@ProductASales" 
                              XName="Month" 
                              YName="Sales"
                              Type="RangeNavigatorType.Area"
                              Fill="rgba(255, 99, 132, 0.3)">
            <RangeNavigatorSeriesBorder Color="#FF6384" Width="2"></RangeNavigatorSeriesBorder>
        </RangeNavigatorSeries>
        
        <!-- Product B sales -->
        <RangeNavigatorSeries DataSource="@ProductBSales" 
                              XName="Month" 
                              YName="Sales"
                              Type="RangeNavigatorType.Area"
                              Fill="rgba(54, 162, 235, 0.3)">
            <RangeNavigatorSeriesBorder Color="#36A2EB" Width="2"></RangeNavigatorSeriesBorder>
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

<div class="legend">
    <span style="color: #FF6384;">■</span> Product A
    <span style="color: #36A2EB; margin-left: 20px;">■</span> Product B
</div>

@code {
    public DateTime[] Range = new DateTime[] 
    { 
        new DateTime(2023, 01, 01), 
        new DateTime(2023, 12, 31) 
    };
    
    public List<ProductSales> ProductASales = GetProductASales();
    public List<ProductSales> ProductBSales = GetProductBSales();

    public class ProductSales
    {
        public DateTime Month { get; set; }
        public double Sales { get; set; }
    }

    private static List<ProductSales> GetProductASales()
    {
        return new List<ProductSales>
        {
            new ProductSales { Month = new DateTime(2023, 1, 1), Sales = 120 },
            new ProductSales { Month = new DateTime(2023, 2, 1), Sales = 135 },
            new ProductSales { Month = new DateTime(2023, 3, 1), Sales = 150 },
            new ProductSales { Month = new DateTime(2023, 4, 1), Sales = 170 },
            new ProductSales { Month = new DateTime(2023, 5, 1), Sales = 195 },
            new ProductSales { Month = new DateTime(2023, 6, 1), Sales = 220 },
            new ProductSales { Month = new DateTime(2023, 7, 1), Sales = 245 },
            new ProductSales { Month = new DateTime(2023, 8, 1), Sales = 260 },
            new ProductSales { Month = new DateTime(2023, 9, 1), Sales = 275 },
            new ProductSales { Month = new DateTime(2023, 10, 1), Sales = 300 },
            new ProductSales { Month = new DateTime(2023, 11, 1), Sales = 325 },
            new ProductSales { Month = new DateTime(2023, 12, 1), Sales = 350 }
        };
    }

    private static List<ProductSales> GetProductBSales()
    {
        return new List<ProductSales>
        {
            new ProductSales { Month = new DateTime(2023, 1, 1), Sales = 90 },
            new ProductSales { Month = new DateTime(2023, 2, 1), Sales = 95 },
            new ProductSales { Month = new DateTime(2023, 3, 1), Sales = 100 },
            new ProductSales { Month = new DateTime(2023, 4, 1), Sales = 110 },
            new ProductSales { Month = new DateTime(2023, 5, 1), Sales = 120 },
            new ProductSales { Month = new DateTime(2023, 6, 1), Sales = 125 },
            new ProductSales { Month = new DateTime(2023, 7, 1), Sales = 130 },
            new ProductSales { Month = new DateTime(2023, 8, 1), Sales = 135 },
            new ProductSales { Month = new DateTime(2023, 9, 1), Sales = 140 },
            new ProductSales { Month = new DateTime(2023, 10, 1), Sales = 145 },
            new ProductSales { Month = new DateTime(2023, 11, 1), Sales = 150 },
            new ProductSales { Month = new DateTime(2023, 12, 1), Sales = 155 }
        };
    }
}
```

## Series Comparison
Compare series types with `Type` property to determine the best visualization for your data:

### Line Series
Configure using `Type="RangeNavigatorType.Line"` with `Fill` and `Width`:
- ✅ Clean, minimal appearance
- ✅ Easy to compare multiple series
- ✅ Good for trend analysis
- ❌ Less visual emphasis on magnitude

### Area Series
Configure using `Type="RangeNavigatorType.Area"` with `Fill` and `RangeNavigatorSeriesBorder`:
- ✅ Strong visual presence
- ✅ Emphasizes volume/magnitude
- ✅ Good for single dominant metric
- ❌ Can obscure other series if overlapping

### StepLine Series
Configure using `Type="RangeNavigatorType.StepLine"` with `Fill` and `Width`:
- ✅ Shows exact change points
- ✅ Perfect for discrete data
- ✅ Clear state transitions
- ❌ Can look jagged with many data points

## Complete Example: Series Type Selector
Implement series type switching using `@bind` for `SelectedSeriesType` and `Type` property with dynamic `Fill` and `RangeNavigatorSeriesBorder`:

```razor
@page "/series-types-demo"
@using Syncfusion.Blazor.Charts

<div class="container">
    <h3>Range Selector Series Types</h3>
    
    <div class="series-selector">
        <label>Select Series Type:</label>
        <select @bind="SelectedSeriesType" class="form-select">
            <option value="@RangeNavigatorType.Line">Line</option>
            <option value="@RangeNavigatorType.Area">Area</option>
            <option value="@RangeNavigatorType.StepLine">StepLine</option>
        </select>
    </div>
    
    <SfRangeNavigator Value="@Range" 
                      ValueType="RangeValueType.DateTime"
                      LabelFormat="MMM yy"
                      IntervalType="RangeIntervalType.Months">
        <RangeNavigatorRangeTooltipSettings Enable="true"></RangeNavigatorRangeTooltipSettings>
        <RangeNavigatorSeriesCollection>
            <RangeNavigatorSeries DataSource="@StockData" 
                                  XName="Date" 
                                  YName="Close"
                                  Type="@SelectedSeriesType"
                                  Fill="@GetSeriesColor()"
                                  Width="2">
                @if (SelectedSeriesType == RangeNavigatorType.Area)
                {
                    <RangeNavigatorSeriesBorder Color="#007bff" Width="2"></RangeNavigatorSeriesBorder>
                }
            </RangeNavigatorSeries>
        </RangeNavigatorSeriesCollection>
    </SfRangeNavigator>
    
    <div class="series-info">
        <h4>@SelectedSeriesType Series</h4>
        <p>@GetSeriesDescription()</p>
    </div>
</div>

@code {
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }
    
    public DateTime[] Range = new DateTime[] 
    { 
        new DateTime(2022, 01, 01), 
        new DateTime(2023, 12, 31) 
    };
    
    public RangeNavigatorType SelectedSeriesType = RangeNavigatorType.Line;
    
    public List<StockInfo> StockData = GenerateStockData();
    
    private string GetSeriesColor()
    {
        return SelectedSeriesType switch
        {
            RangeNavigatorType.Area => "rgba(0, 123, 255, 0.3)",
            RangeNavigatorType.StepLine => "#FF6384",
            _ => "#007bff"
        };
    }
    
    private string GetSeriesDescription()
    {
        return SelectedSeriesType switch
        {
            RangeNavigatorType.Line => "Line series shows data as a continuous line, ideal for trend analysis.",
            RangeNavigatorType.Area => "Area series fills the region below the line, emphasizing magnitude.",
            RangeNavigatorType.StepLine => "StepLine series shows data as steps, perfect for discrete values.",
            _ => ""
        };
    }
    
    private static List<StockInfo> GenerateStockData()
    {
        var data = new List<StockInfo>();
        var startDate = new DateTime(2021, 01, 01);
        var random = new Random();
        double price = 100;
        
        for (int i = 0; i < 730; i++) // 2 years of data
        {
            price += random.Next(-5, 6);
            data.Add(new StockInfo
            {
                Date = startDate.AddDays(i),
                Close = Math.Max(50, price)
            });
        }
        
        return data;
    }
}

<style>
    .container {
        padding: 20px;
    }
    
    .series-selector {
        margin-bottom: 20px;
    }
    
    .series-selector select {
        width: 200px;
        margin-left: 10px;
    }
    
    .series-info {
        margin-top: 20px;
        padding: 15px;
        background-color: #f8f9fa;
        border-radius: 5px;
    }
    
    .legend {
        margin-top: 10px;
        font-size: 14px;
    }
</style>
```

## Best Practices
Apply best practices using `Type` for series selection, `Fill` for colors, `Width` for line thickness, and `RangeNavigatorSeriesBorder` for borders.

### 1. Choose the Right Series Type
Match `Type` property to your data visualization requirements:

```razor
<!-- Financial data: Line -->
<RangeNavigatorSeries Type="RangeNavigatorType.Line" ...>

<!-- Volume data: Area -->
<RangeNavigatorSeries Type="RangeNavigatorType.Area" ...>

<!-- Status changes: StepLine -->
<RangeNavigatorSeries Type="RangeNavigatorType.StepLine" ...>
```

### 2. Use Appropriate Colors
Apply `Fill` property with colors that enhance readability:

```razor
<!-- Subtle for background context -->
<RangeNavigatorSeries Fill="rgba(0, 123, 255, 0.2)" ...>

<!-- Bold for primary focus -->
<RangeNavigatorSeries Fill="#007bff" ...>
```

### 3. Consider Performance
Choose `Type="RangeNavigatorType.Line"` for better rendering with large datasets:

```razor
<!-- Better performance with 10,000+ points -->
<RangeNavigatorSeries Type="RangeNavigatorType.Line" ...>
```

### 4. Multiple Series Clarity
Use distinct `Fill` colors for multiple series:

```razor
<RangeNavigatorSeries Fill="rgba(255, 99, 132, 0.3)" ...>  <!-- Red -->
<RangeNavigatorSeries Fill="rgba(54, 162, 235, 0.3)" ...>  <!-- Blue -->
<RangeNavigatorSeries Fill="rgba(75, 192, 192, 0.3)" ...>  <!-- Green -->
```

## Practical Examples
Apply series configurations with specific property combinations for different data scenarios.

### Example 1: Stock Price (Line)
Configure line series using `Type="RangeNavigatorType.Line"` with `Fill` and `Width`:

```razor
<RangeNavigatorSeries DataSource="@StockPrices" 
                      XName="Date" 
                      YName="Close"
                      Type="RangeNavigatorType.Line"
                      Fill="#2196F3"
                      Width="2">
</RangeNavigatorSeries>
```

### Example 2: Sales Volume (Area)
Configure area series using `Type="RangeNavigatorType.Area"` with `Fill` and `RangeNavigatorSeriesBorder`:

```razor
<RangeNavigatorSeries DataSource="@SalesVolume" 
                      XName="Month" 
                      YName="Units"
                      Type="RangeNavigatorType.Area"
                      Fill="rgba(76, 175, 80, 0.3)">
    <RangeNavigatorSeriesBorder Color="#4CAF50" Width="2"></RangeNavigatorSeriesBorder>
</RangeNavigatorSeries>
```

### Example 3: System Status (StepLine)
Configure step line series using `Type="RangeNavigatorType.StepLine"` with `Fill` and `Width`:

```razor
<RangeNavigatorSeries DataSource="@SystemStatus" 
                      XName="Timestamp" 
                      YName="Status"
                      Type="RangeNavigatorType.StepLine"
                      Fill="#FF5722"
                      Width="2">
</RangeNavigatorSeries>
```

## Common Scenarios
Implement domain-specific configurations using appropriate `Type`, `Fill`, and `DataSource` combinations.

### Financial Dashboard
- **Primary Data**: `Type="RangeNavigatorType.Line"` for stock price with `Fill="#2196F3"`
- **Volume**: `Type="RangeNavigatorType.Area"` for trading volume with `Fill="rgba(76, 175, 80, 0.3)"`
- **Color**: Blue for price, green/red for volume

### Sales Analytics
- **Revenue**: `Type="RangeNavigatorType.Area"` to emphasize magnitude with `Fill` for color
- **Orders**: `Type="RangeNavigatorType.Line"` for count trend with appropriate `Width`
- **Color**: Green for revenue, blue for orders

### IoT Monitoring
- **Sensor Values**: `Type="RangeNavigatorType.Line"` for continuous readings with `Width="2"`
- **State Changes**: `Type="RangeNavigatorType.StepLine"` for on/off states with `Fill` property
- **Color**: Theme-based or status-based (red/yellow/green)

