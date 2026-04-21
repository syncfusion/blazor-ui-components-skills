# Axis Customization

Learn how to customize axis properties in Syncfusion Blazor Range Selector including grid lines, ticks, labels, intervals, and logarithmic scaling.

## API Reference - Axis Customization

**Related Classes & Components:**
- `RangeNavigatorMargin` - Margin settings (Left, Right, Top, Bottom)
- `RangeNavigatorBorder` - Border customization (Color, Width, DashArray)
- `RangeNavigatorMajorGridLines` - Major gridlines (Width, Color, DashArray)
- `RangeNavigatorMajorTickLines` - Major ticks (Width, Color, Height)
- `RangeNavigatorLabelStyle` - Label styling (Color, FontFamily, Size, FontWeight, Opacity)
- `RangeNavigatorStyleSettings` - Overall style configuration

**Enum References:**
- `RangeIntervalType` - Auto, Years, Months, Days, Hours, Minutes, Seconds
- `RangeValueType` - DateTime, Double, Logarithmic
- `RangeLabelIntersectAction` - Label placement handling

**Key Axis Properties:**
- `Interval` - int - Axis interval value
- `IntervalType` - RangeIntervalType - Type of interval
- `LabelFormat` - string - Label format string
- `LogBase` - double - Base for logarithmic axis
- `EnableRtl` - bool - Right-to-left support

For complete API details, see [api-reference.md](api-reference.md).

## Table of Contents

- [Overview](#overview)
- [Axis Configuration](#axis-configuration)
  - [Basic Axis Setup](#basic-axis-setup)
  - [Value Types](#value-types)
- [Grid Lines](#grid-lines)
  - [Major Grid Lines](#major-grid-lines)
  - [Minor Grid Lines](#minor-grid-lines)
  - [Grid Styling](#grid-styling)
- [Tick Configuration](#tick-configuration)
  - [Major Ticks](#major-ticks)
  - [Minor Ticks](#minor-ticks)
  - [Tick Positioning](#tick-positioning)
- [Label Formatting](#label-formatting)
  - [DateTime Labels](#datetime-labels)
  - [Numeric Labels](#numeric-labels)
  - [Custom Label Format](#custom-label-format)
  - [Label Rotation](#label-rotation)
- [Interval Configuration](#interval-configuration)
  - [Auto Intervals](#auto-intervals)
  - [Manual Intervals](#manual-intervals)
  - [Interval Types](#interval-types)
- [Logarithmic Axis](#logarithmic-axis)
  - [Log Base Configuration](#log-base-configuration)
  - [Log Intervals](#log-intervals)
- [RTL Support](#rtl-support)
- [Advanced Scenarios](#advanced-scenarios)
- [Best Practices](#best-practices)

## Overview

The Range Selector axis controls how data is displayed along the X-axis. Customization options include:

- **Grid Lines**: Visual guides for data alignment
- **Ticks**: Marks indicating data points or intervals
- **Labels**: Text representation of axis values
- **Intervals**: Spacing between axis elements
- **Value Types**: DateTime, Numeric, Logarithmic
- **RTL Support**: Right-to-left rendering

## Axis Configuration
Configure axis properties using `ValueType`, `Interval`, and `IntervalType` properties.

### Basic Axis Setup
Configure basic axis properties using `SfRangeNavigator` with `ValueType` property:

```razor
@page "/range-selector/axis-basic"
@using Syncfusion.Blazor.Charts

<h3>Basic Axis Configuration</h3>

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="350">
    
    <!-- Axis Configuration -->
    <RangeNavigatorRangeTooltipSettings Enable="true" 
                                        DisplayMode="TooltipDisplayMode.Always">
    </RangeNavigatorRangeTooltipSettings>
    
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close"
                              Type="RangeNavigatorType.Area">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public object Range = new DateTime[] 
    { 
        new DateTime(2023, 01, 01), 
        new DateTime(2023, 12, 31) 
    };
    
    public List<StockInfo> StockData { get; set; }
    
    protected override void OnInitialized()
    {
        StockData = GenerateStockData(new DateTime(2023, 01, 01), 365);
    }
    
    private List<StockInfo> GenerateStockData(DateTime start, int days)
    {
        var data = new List<StockInfo>();
        var random = new Random();
        double price = 100;
        
        for (int i = 0; i < days; i++)
        {
            price += (random.NextDouble() - 0.5) * 5;
            data.Add(new StockInfo
            {
                Date = start.AddDays(i),
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

### Value Types
Configure different axis value types using the `ValueType` property (DateTime, Double, Logarithmic):

```razor
@page "/range-selector/axis-value-types"
@using Syncfusion.Blazor.Charts

<h3>DateTime Axis</h3>

<SfRangeNavigator @bind-Value="@DateRange" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="300">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@DateData" 
                              XName="Date" 
                              YName="Value">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

<h3>Numeric Axis</h3>

<SfRangeNavigator @bind-Value="@NumericRange" 
                  ValueType="RangeValueType.Double"
                  Width="100%"
                  Height="300">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@NumericData" 
                              XName="X" 
                              YName="Y">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public object DateRange { get; set; }
    public object NumericRange = new double[] { 0, 100 };
    
    public List<DataPoint> DateData { get; set; }
    public List<NumericPoint> NumericData { get; set; }
    
    protected override void OnInitialized()
    {
        // DateTime data
        DateData = new List<DataPoint>();
        var startDate = new DateTime(2023, 01, 01);
        for (int i = 0; i < 365; i++)
        {
            DateData.Add(new DataPoint
            {
                Date = startDate.AddDays(i),
                Value = 100 + Math.Sin(i * 0.1) * 20
            });
        }
        DateRange = new DateTime[] { startDate, startDate.AddMonths(6) };
        
        // Numeric data
        NumericData = new List<NumericPoint>();
        for (int i = 0; i <= 100; i++)
        {
            NumericData.Add(new NumericPoint
            {
                X = i,
                Y = 50 + Math.Sin(i * 0.5) * 30
            });
        }
    }
    
    public class DataPoint
    {
        public DateTime Date { get; set; }
        public double Value { get; set; }
    }
    
    public class NumericPoint
    {
        public double X { get; set; }
        public double Y { get; set; }
    }
}
```

## Grid Lines
Customize grid lines using `RangeNavigatorMajorGridLines` with properties like `Width`, `Color`, and `DashArray`.

### Major Grid Lines
Configure major grid lines using `RangeNavigatorMajorGridLines` component with `Width`, `Color`, and `DashArray` properties:

```razor
@page "/range-selector/major-grid"
@using Syncfusion.Blazor.Charts

<h3>Major Grid Lines</h3>

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="350">
    
    <!-- Major Grid Line Settings -->
    <RangeNavigatorMajorGridLines Width="2" 
                                  Color="#e0e0e0" 
                                  DashArray="5,5">
    </RangeNavigatorMajorGridLines>
    
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
    
    public List<StockInfo> StockData { get; set; }
    
    protected override void OnInitialized()
    {
        StockData = GenerateStockData(new DateTime(2023, 01, 01), 365);
    }
    
    private List<StockInfo> GenerateStockData(DateTime start, int days)
    {
        var data = new List<StockInfo>();
        var random = new Random();
        double price = 100;
        
        for (int i = 0; i < days; i++)
        {
            price += (random.NextDouble() - 0.5) * 5;
            data.Add(new StockInfo
            {
                Date = start.AddDays(i),
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

### Grid Styling
Apply advanced grid line styling using `RangeNavigatorMajorGridLines` with `Width`, `Color`, and `DashArray` properties for custom effects:

```razor
@page "/range-selector/grid-styling"
@using Syncfusion.Blazor.Charts

<h3>Custom Grid Styling</h3>

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="350">
    
    <!-- Styled Major Grid Lines -->
    <RangeNavigatorMajorGridLines Width="2" 
                                  Color="#4CAF50" 
                                  DashArray="10,5">
    </RangeNavigatorMajorGridLines>
    
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close"
                              Type="RangeNavigatorType.StepLine"
                              Fill="#4CAF50"
                              Width="3">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public object Range = new DateTime[] 
    { 
        new DateTime(2023, 03, 01), 
        new DateTime(2023, 09, 30) 
    };
    
    public List<StockInfo> StockData { get; set; }
    
    protected override void OnInitialized()
    {
        StockData = GenerateStockData(new DateTime(2023, 01, 01), 365);
    }
    
    private List<StockInfo> GenerateStockData(DateTime start, int days)
    {
        var data = new List<StockInfo>();
        var random = new Random();
        double price = 100;
        
        for (int i = 0; i < days; i++)
        {
            price += (random.NextDouble() - 0.5) * 5;
            data.Add(new StockInfo
            {
                Date = start.AddDays(i),
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

## Tick Configuration
Customize tick marks using `RangeNavigatorMajorTickLines` component with properties like `Width`, `Height`, and `Color`.

### Major Ticks
Configure major tick marks using `RangeNavigatorMajorTickLines` component with `Width`, `Height`, and `Color` properties:

```razor
@page "/range-selector/major-ticks"
@using Syncfusion.Blazor.Charts

<h3>Major Tick Configuration</h3>

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="350">
    
    <!-- Major Tick Settings -->
    <RangeNavigatorMajorTickLines Width="3" 
                                  Height="10" 
                                  Color="#333333">
    </RangeNavigatorMajorTickLines>
    
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public object Range = new DateTime[] 
    { 
        new DateTime(2023, 01, 01), 
        new DateTime(2023, 12, 31) 
    };
    
    public List<StockInfo> StockData { get; set; }
    
    protected override void OnInitialized()
    {
        StockData = GenerateStockData(new DateTime(2023, 01, 01), 365);
    }
    
    private List<StockInfo> GenerateStockData(DateTime start, int days)
    {
        var data = new List<StockInfo>();
        var random = new Random();
        double price = 100;
        
        for (int i = 0; i < days; i++)
        {
            price += (random.NextDouble() - 0.5) * 5;
            data.Add(new StockInfo
            {
                Date = start.AddDays(i),
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

### Tick Positioning
Control tick placement using the `TickPosition` property (Outside, Inside) with `RangeNavigatorMajorTickLines`:

```razor
@page "/range-selector/tick-positioning"
@using Syncfusion.Blazor.Charts

<h3>Inside Tick Position</h3>

<SfRangeNavigator @bind-Value="@Range1" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="300"
                  TickPosition="AxisPosition.Inside">
    
    <RangeNavigatorMajorTickLines Width="2" 
                                  Height="10" 
                                  Color="#FF5722">
    </RangeNavigatorMajorTickLines>
    
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

<h3>Styled Ticks</h3>

<SfRangeNavigator @bind-Value="@Range2" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="300" TickPosition="AxisPosition.Inside">
    
    <RangeNavigatorMajorTickLines Width="3" 
                                  Height="15" 
                                  Color="#4CAF50">
    </RangeNavigatorMajorTickLines>
    
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public object Range1 = new DateTime[] 
    { 
        new DateTime(2023, 03, 01), 
        new DateTime(2023, 09, 30) 
    };
    
    public object Range2 = new DateTime[] 
    { 
        new DateTime(2023, 01, 01), 
        new DateTime(2023, 06, 30) 
    };
    
    public List<StockInfo> StockData { get; set; }
    
    protected override void OnInitialized()
    {
        StockData = GenerateStockData(new DateTime(2023, 01, 01), 365);
    }
    
    private List<StockInfo> GenerateStockData(DateTime start, int days)
    {
        var data = new List<StockInfo>();
        var random = new Random();
        double price = 100;
        
        for (int i = 0; i < days; i++)
        {
            price += (random.NextDouble() - 0.5) * 5;
            data.Add(new StockInfo
            {
                Date = start.AddDays(i),
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

## Label Formatting
Format axis labels using `LabelFormat`, `RangeNavigatorLabelStyle` component with properties like `Color`, `FontFamily`, `Size`, and `FontWeight`.

### DateTime Labels
Format date and time labels using `LabelFormat` property with `RangeNavigatorLabelStyle` component for styling:

```razor
@page "/range-selector/datetime-labels"
@using Syncfusion.Blazor.Charts

<h3>DateTime Label Formatting</h3>

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="350"
                  LabelFormat="MMM yyyy"
                  Interval="1"
                  IntervalType="RangeIntervalType.Months">
    
    <!-- Label Style Settings -->
    <RangeNavigatorLabelStyle Color="#333333" 
                              FontFamily="Arial" 
                              Size="12px" 
                              FontWeight="500">
    </RangeNavigatorLabelStyle>
    
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

<div class="format-examples">
    <h4>Common DateTime Formats</h4>
    <ul>
        <li><code>dd/MM/yyyy</code> - 31/12/2023</li>
        <li><code>MMM dd</code> - Dec 31</li>
        <li><code>MMM yyyy</code> - Dec 2023</li>
        <li><code>yyyy</code> - 2023</li>
        <li><code>MMMM dd, yyyy</code> - December 31, 2023</li>
    </ul>
</div>

@code {
    public object Range = new DateTime[] 
    { 
        new DateTime(2023, 01, 01), 
        new DateTime(2023, 12, 31) 
    };
    
    public List<StockInfo> StockData { get; set; }
    
    protected override void OnInitialized()
    {
        StockData = GenerateStockData(new DateTime(2023, 01, 01), 365);
    }
    
    private List<StockInfo> GenerateStockData(DateTime start, int days)
    {
        var data = new List<StockInfo>();
        var random = new Random();
        double price = 100;
        
        for (int i = 0; i < days; i++)
        {
            price += (random.NextDouble() - 0.5) * 5;
            data.Add(new StockInfo
            {
                Date = start.AddDays(i),
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

<style>
    .format-examples {
        margin-top: 20px;
        padding: 15px;
        background-color: #f5f5f5;
        border-radius: 4px;
    }
    
    .format-examples code {
        background-color: #e0e0e0;
        padding: 2px 6px;
        border-radius: 3px;
        font-family: 'Courier New', monospace;
    }
</style>
```

### Numeric Labels
Format numeric axis labels using `LabelFormat` property with `RangeNavigatorLabelStyle` for numeric value styling:

```razor
@page "/range-selector/numeric-labels"
@using Syncfusion.Blazor.Charts

<h3>Numeric Label Formatting</h3>

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.Double"
                  Width="100%"
                  Height="350"
                  LabelFormat="n2">
    
    <RangeNavigatorLabelStyle Color="#0078d4" 
                              Size="13px" 
                              FontWeight="bold">
    </RangeNavigatorLabelStyle>
    
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@NumericData" 
                              XName="X" 
                              YName="Y">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public object Range = new double[] { 0, 100 };
    public List<NumericPoint> NumericData { get; set; }
    
    protected override void OnInitialized()
    {
        NumericData = new List<NumericPoint>();
        for (int i = 0; i <= 100; i++)
        {
            NumericData.Add(new NumericPoint
            {
                X = i,
                Y = 50 + Math.Sin(i * 0.2) * 30
            });
        }
    }
    
    public class NumericPoint
    {
        public double X { get; set; }
        public double Y { get; set; }
    }
}
```

### Custom Label Format
Implement custom label formatting using `RangeNavigatorEvents` with `LabelRender` event handler to modify label text dynamically:

```razor
@page "/range-selector/custom-labels"
@using Syncfusion.Blazor.Charts

<h3>Custom Label Formatting</h3>

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="350"
                  Interval="1"
                  IntervalType="RangeIntervalType.Quarter">
    
    <RangeNavigatorEvents LabelRender="OnLabelRender"></RangeNavigatorEvents>
    
    <RangeNavigatorLabelStyle Color="#333333" 
                              Size="12px">
    </RangeNavigatorLabelStyle>
    
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public object Range = new DateTime[] 
    { 
        new DateTime(2023, 01, 01), 
        new DateTime(2023, 12, 31) 
    };
    
    public List<StockInfo> StockData { get; set; }
    
    protected override void OnInitialized()
    {
        StockData = GenerateStockData(new DateTime(2023, 01, 01), 365);
    }
    
    private void OnLabelRender(RangeLabelRenderEventArgs args)
    {
        args.Text = $"{args.Text} : {args.Value}";
    }
    
    private List<StockInfo> GenerateStockData(DateTime start, int days)
    {
        var data = new List<StockInfo>();
        var random = new Random();
        double price = 100;
        
        for (int i = 0; i < days; i++)
        {
            price += (random.NextDouble() - 0.5) * 5;
            data.Add(new StockInfo
            {
                Date = start.AddDays(i),
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

## Interval Configuration
Control axis intervals using `Interval` and `IntervalType` properties to define label spacing and frequency.

### Auto Intervals
Let the component automatically calculate intervals when `Interval` and `IntervalType` properties are not explicitly set:

```razor
@page "/range-selector/auto-intervals"
@using Syncfusion.Blazor.Charts

<h3>Automatic Interval Calculation</h3>

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="350">
    
    <!-- No interval specified - automatically calculated -->
    
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public object Range = new DateTime[] 
    { 
        new DateTime(2023, 01, 01), 
        new DateTime(2023, 12, 31) 
    };
    
    public List<StockInfo> StockData { get; set; }
    
    protected override void OnInitialized()
    {
        StockData = GenerateStockData(new DateTime(2023, 01, 01), 365);
    }
    
    private List<StockInfo> GenerateStockData(DateTime start, int days)
    {
        var data = new List<StockInfo>();
        var random = new Random();
        double price = 100;
        
        for (int i = 0; i < days; i++)
        {
            price += (random.NextDouble() - 0.5) * 5;
            data.Add(new StockInfo
            {
                Date = start.AddDays(i),
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

### Manual Intervals
Set specific intervals using `Interval` (numeric value) and `IntervalType` (RangeIntervalType enum) properties:

```razor
@page "/range-selector/manual-intervals"
@using Syncfusion.Blazor.Charts

<h3>Manual Interval Configuration</h3>

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="350"
                  Interval="2"
                  IntervalType="RangeIntervalType.Months">
    
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

<div class="info-panel">
    <p><strong>Interval:</strong> 2 Months</p>
    <p><strong>Effect:</strong> Labels appear every 2 months</p>
</div>

@code {
    public object Range = new DateTime[] 
    { 
        new DateTime(2023, 01, 01), 
        new DateTime(2023, 12, 31) 
    };
    
    public List<StockInfo> StockData { get; set; }
    
    protected override void OnInitialized()
    {
        StockData = GenerateStockData(new DateTime(2023, 01, 01), 365);
    }
    
    private List<StockInfo> GenerateStockData(DateTime start, int days)
    {
        var data = new List<StockInfo>();
        var random = new Random();
        double price = 100;
        
        for (int i = 0; i < days; i++)
        {
            price += (random.NextDouble() - 0.5) * 5;
            data.Add(new StockInfo
            {
                Date = start.AddDays(i),
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

### Interval Types
Use different interval types with `IntervalType` property (RangeIntervalType.Years, RangeIntervalType.Months, RangeIntervalType.Days, RangeIntervalType.Hours, RangeIntervalType.Minutes, RangeIntervalType.Seconds) combined with `Interval` value:

```razor
@page "/range-selector/interval-types"
@using Syncfusion.Blazor.Charts

<h3>Years Interval</h3>

<SfRangeNavigator @bind-Value="@YearRange" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="250"
                  Interval="1"
                  IntervalType="RangeIntervalType.Years"
                  LabelFormat="yyyy">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@LongTermData" 
                              XName="Date" 
                              YName="Close">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

<h3>Months Interval</h3>

<SfRangeNavigator @bind-Value="@MonthRange" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="250"
                  Interval="1"
                  IntervalType="RangeIntervalType.Months"
                  LabelFormat="MMM yyyy">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@MediumTermData" 
                              XName="Date" 
                              YName="Close">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

<h3>Days Interval</h3>

<SfRangeNavigator @bind-Value="@DayRange" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="250"
                  Interval="7"
                  IntervalType="RangeIntervalType.Days"
                  LabelFormat="MMM dd">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@ShortTermData" 
                              XName="Date" 
                              YName="Close">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public object YearRange { get; set; }
    public object MonthRange { get; set; }
    public object DayRange { get; set; }
    
    public List<StockInfo> LongTermData { get; set; }
    public List<StockInfo> MediumTermData { get; set; }
    public List<StockInfo> ShortTermData { get; set; }
    
    protected override void OnInitialized()
    {
        // 5 years of data
        LongTermData = GenerateStockData(new DateTime(2019, 01, 01), 1825);
        YearRange = new DateTime[] 
        { 
            new DateTime(2020, 01, 01), 
            new DateTime(2023, 12, 31) 
        };
        
        // 1 year of data
        MediumTermData = GenerateStockData(new DateTime(2023, 01, 01), 365);
        MonthRange = new DateTime[] 
        { 
            new DateTime(2023, 01, 01), 
            new DateTime(2023, 12, 31) 
        };
        
        // 3 months of data
        ShortTermData = GenerateStockData(new DateTime(2023, 10, 01), 90);
        DayRange = new DateTime[] 
        { 
            new DateTime(2023, 10, 01), 
            new DateTime(2023, 12, 31) 
        };
    }
    
    private List<StockInfo> GenerateStockData(DateTime start, int days)
    {
        var data = new List<StockInfo>();
        var random = new Random();
        double price = 100;
        
        for (int i = 0; i < days; i++)
        {
            price += (random.NextDouble() - 0.5) * 5;
            data.Add(new StockInfo
            {
                Date = start.AddDays(i),
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

## Logarithmic Axis
Implement logarithmic scaling using `ValueType="RangeValueType.Logarithmic"` with `LogBase` property for custom base values.

### Log Base Configuration
Implement logarithmic axis scaling using `ValueType="RangeValueType.Logarithmic"` and `LogBase` property:

```razor
@page "/range-selector/logarithmic-axis"
@using Syncfusion.Blazor.Charts

<h3>Logarithmic Axis</h3>

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.Logarithmic"
                  Width="100%"
                  Height="350"
                  LogBase="10">
    
    <RangeNavigatorLabelStyle Color="#333333">
    </RangeNavigatorLabelStyle>
    
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@ExponentialData" 
                              XName="X" 
                              YName="Y">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

<div class="info-panel">
    <p><strong>Use Case:</strong> Logarithmic axis is ideal for data with exponential growth or wide value ranges</p>
    <p><strong>Log Base:</strong> 10 (each interval is 10x the previous)</p>
</div>

@code {
    public object Range = new double[] { 1, 10000 };
    public List<DataPoint> ExponentialData { get; set; }
    
    protected override void OnInitialized()
    {
        ExponentialData = new List<DataPoint>();
        
        // Generate exponential data
        for (double x = 1; x <= 10000; x *= 1.1)
        {
            ExponentialData.Add(new DataPoint
            {
                X = x,
                Y = Math.Log10(x) * 50 + new Random().NextDouble() * 10
            });
        }
    }
    
    public class DataPoint
    {
        public double X { get; set; }
        public double Y { get; set; }
    }
}
```

### Log Intervals
Configure logarithmic intervals using `LogBase` and `Interval` properties together with `ValueType="RangeValueType.Logarithmic"`:

```razor
@page "/range-selector/log-intervals"
@using Syncfusion.Blazor.Charts

<h3>Logarithmic Intervals - Base 10</h3>

<SfRangeNavigator @bind-Value="@Range1" 
                  ValueType="RangeValueType.Logarithmic"
                  Width="100%"
                  Height="300"
                  LogBase="10"
                  Interval="1">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@LogData10" 
                              XName="X" 
                              YName="Y">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

<h3>Logarithmic Intervals - Base 2</h3>

<SfRangeNavigator @bind-Value="@Range2" 
                  ValueType="RangeValueType.Logarithmic"
                  Width="100%"
                  Height="300"
                  LogBase="2"
                  Interval="1">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@LogData2" 
                              XName="X" 
                              YName="Y">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public object Range1 = new double[] { 1, 1000 };
    public object Range2 = new double[] { 1, 128 };
    
    public List<DataPoint> LogData10 { get; set; }
    public List<DataPoint> LogData2 { get; set; }
    
    protected override void OnInitialized()
    {
        var random = new Random();
        
        // Base 10 data
        LogData10 = new List<DataPoint>();
        for (double x = 1; x <= 10000; x *= 1.2)
        {
            LogData10.Add(new DataPoint
            {
                X = x,
                Y = Math.Log10(x) * 25 + random.NextDouble() * 10
            });
        }
        
        // Base 2 data
        LogData2 = new List<DataPoint>();
        for (double x = 1; x <= 256; x *= 1.1)
        {
            LogData2.Add(new DataPoint
            {
                X = x,
                Y = Math.Log(x, 2) * 15 + random.NextDouble() * 5
            });
        }
    }
    
    public class DataPoint
    {
        public double X { get; set; }
        public double Y { get; set; }
    }
}
```

## RTL Support
Enable right-to-left rendering using the `EnableRtl` property on `SfRangeNavigator` component.

### Right-to-Left Rendering
Enable RTL (Right-to-Left) support using `EnableRtl="true"` property on `SfRangeNavigator`:

```razor
@page "/range-selector/rtl-support"
@using Syncfusion.Blazor.Charts

<h3>RTL (Right-to-Left) Support</h3>

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="350"
                  EnableRtl="true"
                  LabelFormat="MMM yyyy">
    
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

<div class="rtl-info" dir="rtl">
    <h4>معلومات</h4>
    <p>يتم عرض المحور من اليمين إلى اليسار</p>
</div>

@code {
    public object Range = new DateTime[] 
    { 
        new DateTime(2023, 01, 01), 
        new DateTime(2023, 12, 31) 
    };
    
    public List<StockInfo> StockData { get; set; }
    
    protected override void OnInitialized()
    {
        StockData = GenerateStockData(new DateTime(2023, 01, 01), 365);
    }
    
    private List<StockInfo> GenerateStockData(DateTime start, int days)
    {
        var data = new List<StockInfo>();
        var random = new Random();
        double price = 100;
        
        for (int i = 0; i < days; i++)
        {
            price += (random.NextDouble() - 0.5) * 5;
            data.Add(new StockInfo
            {
                Date = start.AddDays(i),
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

<style>
    .rtl-info {
        margin-top: 20px;
        padding: 15px;
        background-color: #fff3cd;
        border: 1px solid #ffc107;
        border-radius: 4px;
    }
</style>
```

## Advanced Scenarios
Implement complex axis configurations combining `Interval`, `IntervalType`, `LabelFormat`, and event handlers for specialized use cases.

### Multi-Level Axis Labels
Create hierarchical axis labels using `Interval`, `IntervalType`, and `LabelFormat` properties with custom styling:

```razor
@page "/range-selector/multi-level-labels"
@using Syncfusion.Blazor.Charts

<h3>Multi-Level Axis Configuration</h3>

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="400"
                  Interval="1"
                  IntervalType="RangeIntervalType.Months"
                  LabelFormat="MMM">
    
    <RangeNavigatorLabelStyle Color="#666666" 
                              Size="11px">
    </RangeNavigatorLabelStyle>
    
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public object Range = new DateTime[] 
    { 
        new DateTime(2023, 01, 01), 
        new DateTime(2023, 12, 31) 
    };
    
    public List<StockInfo> StockData { get; set; }
    
    protected override void OnInitialized()
    {
        StockData = GenerateStockData(new DateTime(2023, 01, 01), 365);
    }
    
    private List<StockInfo> GenerateStockData(DateTime start, int days)
    {
        var data = new List<StockInfo>();
        var random = new Random();
        double price = 100;
        
        for (int i = 0; i < days; i++)
        {
            price += (random.NextDouble() - 0.5) * 5;
            data.Add(new StockInfo
            {
                Date = start.AddDays(i),
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

## Best Practices
Follow recommendations for axis styling using `RangeNavigatorLabelStyle`, `RangeNavigatorMajorGridLines`, and `RangeNavigatorMajorTickLines` for optimal visual presentation.

### Axis Configuration Guidelines
Apply best practices using `LabelFormat`, style components, and property configurations for clear and accessible axis display:

```razor
@page "/range-selector/axis-best-practices"
@using Syncfusion.Blazor.Charts

<h3>Axis Best Practices Example</h3>

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="400"
                  Interval="1"
                  IntervalType="RangeIntervalType.Months"
                  LabelFormat="MMM yy">
    
    <!-- Readable label styling -->
    <RangeNavigatorLabelStyle Color="#333333" 
                              Size="12px" 
                              FontFamily="Arial, sans-serif"
                              FontWeight="500">
    </RangeNavigatorLabelStyle>
    
    <!-- Subtle grid lines -->
    <RangeNavigatorMajorGridLines Width="1" 
                                  Color="rgba(0,0,0,0.1)">
    </RangeNavigatorMajorGridLines>
    
    <!-- Appropriate tick marks -->
    <RangeNavigatorMajorTickLines Width="2" 
                                  Height="8" 
                                  Color="#666666">
    </RangeNavigatorMajorTickLines>
    
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close"
                              Type="RangeNavigatorType.Area"
                              Fill="rgba(0, 120, 212, 0.3)">
            <RangeNavigatorSeriesBorder Color="#0078d4" Width="2" />
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

<div class="best-practices-panel">
    <h4>Axis Configuration Best Practices</h4>
    <ul>
        <li><strong>Label Clarity:</strong> Use readable fonts (12-14px) and appropriate contrast</li>
        <li><strong>Grid Lines:</strong> Keep subtle (rgba with low opacity) to avoid visual clutter</li>
        <li><strong>Intervals:</strong> Choose intervals that match your data granularity</li>
        <li><strong>Tick Marks:</strong> Use consistent sizing (7-10px height) for visual hierarchy</li>
        <li><strong>Format Strings:</strong> Match format to your audience (MMM yyyy vs MM/yyyy)</li>
        <li><strong>Performance:</strong> Avoid excessive grid lines/ticks for large datasets</li>
        <li><strong>Accessibility:</strong> Ensure sufficient color contrast for labels</li>
    </ul>
</div>

@code {
    public object Range = new DateTime[] 
    { 
        new DateTime(2023, 01, 01), 
        new DateTime(2023, 12, 31) 
    };
    
    public List<StockInfo> StockData { get; set; }
    
    protected override void OnInitialized()
    {
        StockData = GenerateStockData(new DateTime(2023, 01, 01), 365);
    }
    
    private List<StockInfo> GenerateStockData(DateTime start, int days)
    {
        var data = new List<StockInfo>();
        var random = new Random();
        double price = 100;
        
        for (int i = 0; i < days; i++)
        {
            price += (random.NextDouble() - 0.5) * 5;
            data.Add(new StockInfo
            {
                Date = start.AddDays(i),
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

<style>
    .best-practices-panel {
        margin-top: 25px;
        padding: 20px;
        background: linear-gradient(135deg, #f5f7fa 0%, #c3cfe2 100%);
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
    }
    
    .best-practices-panel h4 {
        margin-top: 0;
        color: #0078d4;
        border-bottom: 2px solid #0078d4;
        padding-bottom: 10px;
    }
    
    .best-practices-panel li {
        margin-bottom: 10px;
        line-height: 1.6;
    }
</style>
```

## Troubleshooting

### Common Issues and Solutions

**Issue: Labels overlapping**
- **Solution**: Increase interval value or reduce label font size
- Consider label rotation for dense data
- Use abbreviated formats (MMM vs MMMM)

**Issue: Grid lines not visible**
- **Solution**: Increase grid line width or adjust color
- Check that grid line color has sufficient contrast
- Ensure Width property is set > 0

**Issue: Logarithmic axis shows incorrect values**
- **Solution**: Verify LogBase is appropriate for your data
- Ensure data contains only positive values
- Check that ValueType is set to `Logarithmic`

**Issue: Custom label formatting not applied**
- **Solution**: Verify LabelRender event is properly bound
- Check that event handler returns modified text
- Ensure args.Text is being correctly parsed

**Issue: RTL not working**
- **Solution**: Set EnableRtl="true" on SfRangeNavigator
- Verify parent elements don't override direction
- Check browser RTL support

