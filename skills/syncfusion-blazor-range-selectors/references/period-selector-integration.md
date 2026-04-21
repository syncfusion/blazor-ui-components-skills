# Period Selector Integration

Learn how to implement and customize period selector buttons in Syncfusion Blazor Range Selector for quick date range selection.

## API Reference - Period Selector

**Related Classes & Components:**
- `RangeNavigatorPeriodSelectorSettings` - Period selector configuration
- `RangeNavigatorPeriods` - Container for period buttons
- `RangeNavigatorPeriod` - Individual period button definition

**Period Selector Settings Properties:**
- `Enabled` - bool - Enable/disable period selector (default: true)
- `Height` - double - Height in pixels (default: 32)
- `Position` - PeriodSelectorPosition - Top or Bottom (default: Bottom)

**Period Button Properties:**
- `Text` - string - Button label (e.g., "1M", "3M", "YTD")
- `Interval` - int - Number of intervals (default: 1)
- `IntervalType` - RangeIntervalType - Interval type to apply
- `Selected` - bool - Pre-selected by default

**Enum - PeriodSelectorPosition:**
- `Top` - Period buttons above range selector
- `Bottom` - Period buttons below range selector (default)

**Enum - RangeIntervalType:**
- `Years`, `Months`, `Days`, `Hours`, `Minutes`, `Seconds`

**Standard Period Values:**
- "1M" → Interval=1, IntervalType=Months
- "3M" → Interval=3, IntervalType=Months
- "6M" → Interval=6, IntervalType=Months
- "YTD" → Year-to-Date (handled specially)
- "1Y" → Interval=1, IntervalType=Years
- "All" → Show all data

For complete API details, see [api-reference.md](api-reference.md).

## Table of Contents

- [Overview](#overview)
- [Basic Period Selector](#basic-period-selector)
    - [Enable Period Selector](#enable-period-selector)
- [Predefined Period Buttons](#predefined-period-buttons)
    - [Standard Periods](#standard-periods)
    - [Year-to-Date (YTD) Period](#year-to-date-ytd-period)
- [Custom Period Buttons](#custom-period-buttons)
    - [Define Custom Periods](#define-custom-periods)
- [Position Configuration](#position-configuration)
    - [Top Position](#top-position)
    - [Bottom Position](#bottom-position)
- [Event Handling](#event-handling)
    - [Period Selection Changed Event](#period-selection-changed-event)
- [Styling Period Buttons](#styling-period-buttons)
    - [Custom Button Styles](#custom-button-styles)
- [Best Practices](#best-practices)
    - [Recommended Period Configurations](#recommended-period-configurations)
- [Troubleshooting](#troubleshooting)
    - [Common Issues](#common-issues)

## Overview
Period selectors provide quick date range selection using `RangeNavigatorPeriodSelectorSettings` component with `RangeNavigatorPeriods` containing multiple `RangeNavigatorPeriod` definitions. The Range Selector supports:

- **Predefined Periods**: Built-in buttons for common intervals
- **Custom Periods**: Define your own time ranges
- **Event Handling**: Respond to period selection changes
- **Styling**: Customize button appearance and layout
- **Positioning**: Control placement of period buttons

## Basic Period Selector
Configure period buttons using `RangeNavigatorPeriodSelectorSettings` with `RangeNavigatorPeriods` and `RangeNavigatorPeriod` components.

### Enable Period Selector
Enable period selector with default buttons using `RangeNavigatorPeriodSelectorSettings` component:

```razor
@page "/range-selector/period-basic"
@using Syncfusion.Blazor.Charts

<h3>Range Selector with Period Selector</h3>

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="350">
    
    <!-- Enable Period Selector -->
    <RangeNavigatorPeriodSelectorSettings>
        <RangeNavigatorPeriods>
            <!-- Predefined periods will be shown by default -->
        </RangeNavigatorPeriods>
    </RangeNavigatorPeriodSelectorSettings>
    
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
        // Generate sample stock data
        StockData = GenerateStockData();
    }
    
    private List<StockInfo> GenerateStockData()
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

## Predefined Period Buttons
Define period buttons using `RangeNavigatorPeriod` component with `Interval`, `IntervalType`, `Text`, and `Selected` properties.

### Standard Periods
Configure commonly used period buttons using `RangeNavigatorPeriod` with `Interval`, `IntervalType`, and `Text` properties:

```razor
@page "/range-selector/standard-periods"
@using Syncfusion.Blazor.Charts

<h3>Standard Period Buttons</h3>

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%">
    
    <RangeNavigatorPeriodSelectorSettings Position="PeriodSelectorPosition.Top">
        <RangeNavigatorPeriods>
            <RangeNavigatorPeriod Interval="1" IntervalType="RangeIntervalType.Months" Text="1M"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="2" IntervalType="RangeIntervalType.Months" Text="2M"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="6" IntervalType="RangeIntervalType.Months" Text="6M"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Text="YTD"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="1" IntervalType="RangeIntervalType.Years" Text="1Y"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="2" IntervalType="RangeIntervalType.Years" Text="2Y"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Text="All"></RangeNavigatorPeriod>
        </RangeNavigatorPeriods>
    </RangeNavigatorPeriodSelectorSettings>
    
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close"
                              Type="RangeNavigatorType.Line">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@{
    var dateRange = Range as DateTime[];
}

<div class="info-section">
    @if (dateRange != null && dateRange.Length >= 2)
    {
        <p>
            <strong>Selected Range:</strong>
            @dateRange[0].ToString("MMM dd, yyyy")
            to
            @dateRange[1].ToString("MMM dd, yyyy")
        </p>

        <p>
            <strong>Days Selected:</strong>
            @( (dateRange[1] - dateRange[0]).Days ) days
        </p>
    }
</div>


@code {
    public object Range { get; set; }
    public List<StockInfo> StockData { get; set; }
    
    protected override void OnInitialized()
    {
        StockData = GenerateStockData(new DateTime(2020, 01, 01), 1095);
        
        // Set initial range to last 6 months
        Range = new DateTime[]
        {
            StockData[StockData.Count - 180].Date,
            StockData[StockData.Count - 1].Date
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

### Year-to-Date (YTD) Period
Implement YTD functionality using `RangeNavigatorPeriod` with `Text="YTD"` and `IntervalType="RangeIntervalType.Auto"`:

```razor
@page "/range-selector/ytd-period"
@using Syncfusion.Blazor.Charts

<h3>Year-to-Date Period Selector</h3>

<SfRangeNavigator Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%">
    <RangeNavigatorEvents Changed="OnRangeChanged" />
    <RangeNavigatorPeriodSelectorSettings Position="PeriodSelectorPosition.Top">
        <RangeNavigatorPeriods>
            <RangeNavigatorPeriod Interval="1" IntervalType="RangeIntervalType.Months" Text="1M"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="2" IntervalType="RangeIntervalType.Months" Text="2M"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="6" IntervalType="RangeIntervalType.Months" Text="6M"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Text="YTD" IntervalType="RangeIntervalType.Auto"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="1" IntervalType="RangeIntervalType.Years" Text="1Y"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="2" IntervalType="RangeIntervalType.Years" Text="2Y"></RangeNavigatorPeriod>
        </RangeNavigatorPeriods>
    </RangeNavigatorPeriodSelectorSettings>
    
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public DateTime[] Range { get; set; }
    public List<StockInfo> StockData { get; set; }
    public bool IsYTDSelected { get; set; } = true;
    
    protected override void OnInitialized()
    {
        StockData = GenerateStockData(new DateTime(2022, 01, 01), 730);
        var now = DateTime.Now;
        Range = new DateTime[]
        {
            new DateTime(now.Year, 1, 1),
            now
        };
    }
    
    private void OnRangeChanged(object args)
    {
        if (args is DateTime[] range && range.Length == 2)
        {
            var yearStart = new DateTime(DateTime.Now.Year, 1, 1);

            IsYTDSelected =
                range[0].Date == yearStart &&
                range[1].Date >= DateTime.Today;
        }
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

## Position Configuration
Control period button placement using `Position` property in `RangeNavigatorPeriodSelectorSettings` (Top or Bottom).

### Top Position
Place period selector at the top using `Position="PeriodSelectorPosition.Top"` in `RangeNavigatorPeriodSelectorSettings`:

```razor
@page "/range-selector/period-top"
@using Syncfusion.Blazor.Charts

<h3>Period Selector - Top Position</h3>

<SfRangeNavigator Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="400">
    
    <!-- Position at top -->
    <RangeNavigatorPeriodSelectorSettings Position="PeriodSelectorPosition.Top">
        <RangeNavigatorPeriods>
            <RangeNavigatorPeriod Interval="1" IntervalType="RangeIntervalType.Months" Text="1M"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="2" IntervalType="RangeIntervalType.Months" Text="2M"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="6" IntervalType="RangeIntervalType.Months" Text="6M"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Text="YTD" IntervalType="RangeIntervalType.Auto"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="1" IntervalType="RangeIntervalType.Years" Text="1Y"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="2" IntervalType="RangeIntervalType.Years" Text="2Y"></RangeNavigatorPeriod>
        </RangeNavigatorPeriods>
    </RangeNavigatorPeriodSelectorSettings>
    
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public DateTime[] Range = new DateTime[] 
    { 
        new DateTime(2023, 06, 01), 
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

### Bottom Position
Place period selector at the bottom using `Position="PeriodSelectorPosition.Bottom"` in `RangeNavigatorPeriodSelectorSettings`:

```razor
@page "/range-selector/period-bottom"
@using Syncfusion.Blazor.Charts

<h3>Period Selector - Bottom Position</h3>

<SfRangeNavigator Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="400">
    
    <!-- Position at Bottom -->
    <RangeNavigatorPeriodSelectorSettings Position="PeriodSelectorPosition.Bottom">
        <RangeNavigatorPeriods>
            <RangeNavigatorPeriod Interval="1" IntervalType="RangeIntervalType.Months" Text="1M"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="2" IntervalType="RangeIntervalType.Months" Text="2M"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="6" IntervalType="RangeIntervalType.Months" Text="6M"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Text="YTD" IntervalType="RangeIntervalType.Auto"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="1" IntervalType="RangeIntervalType.Years" Text="1Y"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="2" IntervalType="RangeIntervalType.Years" Text="2Y"></RangeNavigatorPeriod>
        </RangeNavigatorPeriods>
    </RangeNavigatorPeriodSelectorSettings>
    
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public DateTime[] Range = new DateTime[] 
    { 
        new DateTime(2023, 06, 01), 
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

## Event Handling
Respond to period selector interactions using `RangeNavigatorEvents` with `SelectorRender` event handler.

### Period Selection Changed Event
Handle period button click events using `RangeNavigatorEvents` with `SelectorRender` handler and `RangeSelectorRenderEventArgs` parameter:

```razor
@page "/range-selector/period-events"
@using Syncfusion.Blazor.Charts

<h3>Period Selector Event Handling</h3>

<SfRangeNavigator Value="@Value" ValueType="RangeValueType.DateTime" IntervalType="RangeIntervalType.Years">
    <RangeNavigatorPeriodSelectorSettings>
        <RangeNavigatorPeriods>
            <RangeNavigatorPeriod Interval="1" IntervalType="RangeIntervalType.Months" Text="1M"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="2" IntervalType="RangeIntervalType.Months" Text="2M"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="6" IntervalType="RangeIntervalType.Months" Text="6M"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Text="YTD"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="1" IntervalType="RangeIntervalType.Years" Text="1Y"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="2" IntervalType="RangeIntervalType.Years" Text="2Y"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Text="All"></RangeNavigatorPeriod>
        </RangeNavigatorPeriods>
    </RangeNavigatorPeriodSelectorSettings>
    <RangeNavigatorEvents SelectorRender="SelectorCustomization"></RangeNavigatorEvents>
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockInfo" XName="Date" Type="RangeNavigatorType.Line" YName="Close">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public class StockDetails
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }
    public List<StockDetails> StockInfo = new List<StockDetails> {
        new StockDetails { Date = new DateTime(2005, 01, 01), Close = 21 },
        new StockDetails { Date = new DateTime(2006, 01, 01), Close = 24 },
        new StockDetails { Date = new DateTime(2007, 01, 01), Close = 36 },
        new StockDetails { Date = new DateTime(2008, 01, 01), Close = 38 },
        new StockDetails { Date = new DateTime(2009, 01, 01), Close = 54 },
        new StockDetails { Date = new DateTime(2010, 01, 01), Close = 57 },
        new StockDetails { Date = new DateTime(2011, 01, 01), Close = 70 }
    };
    public DateTime[] Value = new DateTime[] {
        new DateTime(2006, 01, 01), new DateTime(2008, 01, 01)
    };
    public void SelectorCustomization(RangeSelectorRenderEventArgs args)
    {
        // Here you can customize your code.
    }
}
```

## Best Practices
Follow configuration guidelines using `RangeNavigatorPeriodSelectorSettings`, `Position`, and logical period ordering with `Interval` and `IntervalType`.

### Recommended Period Configurations
Implement recommended period configurations using `RangeNavigatorPeriodSelectorSettings` with logical button ordering and `Position="PeriodSelectorPosition.Top"`:

```razor
@page "/range-selector/period-best-practices"
@using Syncfusion.Blazor.Charts

<h3>Best Practice: Period Selector Configuration</h3>

<SfRangeNavigator Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="400">
    
    <RangeNavigatorPeriodSelectorSettings Position="PeriodSelectorPosition.Top">
        <RangeNavigatorPeriods>
            <RangeNavigatorPeriod Interval="1" IntervalType="RangeIntervalType.Months" Text="1M"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="2" IntervalType="RangeIntervalType.Months" Text="2M"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="6" IntervalType="RangeIntervalType.Months" Text="6M"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Text="YTD" IntervalType="RangeIntervalType.Auto"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="1" IntervalType="RangeIntervalType.Years" Text="1Y"></RangeNavigatorPeriod>
            <RangeNavigatorPeriod Interval="2" IntervalType="RangeIntervalType.Years" Text="2Y"></RangeNavigatorPeriod>
        </RangeNavigatorPeriods>
    </RangeNavigatorPeriodSelectorSettings>
    
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close"
                              Type="RangeNavigatorType.Area">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

<div class="best-practices">
    <h4>Period Selector Best Practices</h4>
    <ul>
        <li><strong>Order logically:</strong> Arrange buttons from shortest to longest period</li>
        <li><strong>Include YTD:</strong> Year-to-Date is valuable for financial applications</li>
        <li><strong>Limit buttons:</strong> Use 5-7 buttons for optimal user experience</li>
        <li><strong>Label clearly:</strong> Use standard abbreviations (1M, 3M, 6M, 1Y)</li>
        <li><strong>Set default:</strong> Pre-select the most commonly used period</li>
        <li><strong>Include "All":</strong> Always provide option to view complete dataset</li>
    </ul>
</div>

@code {
    public DateTime[] Range { get; set; }
    public List<StockInfo> StockData { get; set; }
    
    protected override void OnInitialized()
    {
        StockData = GenerateStockData(new DateTime(2020, 01, 01), 1460); // 4 years
        
        // Set to YTD
        var now = DateTime.Now;
        Range = new DateTime[]
        {
            new DateTime(now.Year, 1, 1),
            now
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

<style>
    .best-practices {
        margin-top: 20px;
        padding: 15px;
        background-color: #f5f5f5;
        border-left: 4px solid #0078d4;
        border-radius: 4px;
    }
    
    .best-practices h4 {
        margin-top: 0;
        color: #0078d4;
    }
    
    .best-practices ul {
        margin-bottom: 0;
    }
    
    .best-practices li {
        margin-bottom: 8px;
    }
</style>
```

## Troubleshooting
Resolve period selector configuration and functionality issues related to `RangeNavigatorPeriodSelectorSettings` and `RangeNavigatorPeriod` properties.

### Common Issues
Address common period selector problems with proper configuration of settings and button definitions:

**Issue: Period buttons not appearing**
- **Solution**: Ensure `RangeNavigatorPeriodSelectorSettings` is properly configured
- Check that at least one `RangeNavigatorPeriodSelectorButton` is defined
- Verify that the component has sufficient height

**Issue: Wrong date range selected**
- **Solution**: Verify that `Interval` and `IntervalType` match your intentions
- Check that your data source contains dates in the expected range
- Ensure `ValueType` is set to `RangeValueType.DateTime`

**Issue: YTD button not working correctly**
- **Solution**: Ensure your data includes the current year
- Set the initial range correctly in `OnInitialized`
- Handle the `Changed` event to update YTD selection state

**Issue: Custom styling not applied**
- **Solution**: Use correct CSS selectors for period buttons
- Ensure styles are placed in `<style>` tag or external CSS file
- Check for CSS specificity conflicts

