# Chart Types

## Table of Contents
- [Overview](#overview)
- [Line Sparkline](#line-sparkline)
- [Area Sparkline](#area-sparkline)
- [Column Sparkline](#column-sparkline)
- [WinLoss Sparkline](#winloss-sparkline)
- [Pie Sparkline](#pie-sparkline)
- [Type Selection Guide](#type-selection-guide)

## Overview

Syncfusion Blazor Sparkline supports five chart types, each optimized for different data visualization scenarios. Change the type using the `Type` property set to `SparklineType.Line`, `SparklineType.Area`, `SparklineType.Column`, `SparklineType.WinLoss`, or `SparklineType.Pie`.

**Available Types:**
- **Line** - Default, shows continuous trends with connected points
- **Area** - Filled line chart, emphasizes magnitude
- **Column** - Vertical bars, ideal for comparing discrete values
- **WinLoss** - Binary outcomes (win/loss/tie)
- **Pie** - Proportional segments showing part-to-whole relationships

## Line Sparkline

The default chart type that connects data points with a line, perfect for showing trends over time. Set `Type="SparklineType.Line"` and use properties `XName` and `YName` for data mapping, with optional `LineWidth` for customization.

### Basic Line Sparkline

Create a line sparkline using `Type="SparklineType.Line"` with `TValue`, `XName`, and `YName` properties for data binding:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@PopulationData" 
              TValue="PopulationReport" 
              XName="Year" 
              YName="Population" 
              Type="SparklineType.Line"
              Width="200px" 
              Height="60px">
</SfSparkline>

@code {
    public class PopulationReport
    {
        public int Year { get; set; }
        public int Population { get; set; }
    }

    private List<PopulationReport> PopulationData = new List<PopulationReport> 
    {
        new PopulationReport { Year = 2005, Population = 20090440 },
        new PopulationReport { Year = 2006, Population = 20264080 },
        new PopulationReport { Year = 2007, Population = 20434180 },
        new PopulationReport { Year = 2008, Population = 21007310 },
        new PopulationReport { Year = 2009, Population = 21262640 },
        new PopulationReport { Year = 2010, Population = 21515750 },
        new PopulationReport { Year = 2011, Population = 21766710 },
        new PopulationReport { Year = 2012, Population = 22015580 }
    };
}
```

### Styled Line Sparkline

Customize line sparkline appearance using properties `Fill`, `LineWidth`, `Opacity`, along with `SparklineAxisSettings` for range control and `SparklineMarkerSettings` for marker visibility:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@StockPrices" 
              Type="SparklineType.Line"
              Height="50px" 
              Width="250px"
              Fill="#2196F3"
              LineWidth="2">
    <SparklineAxisSettings MinY="90" MaxY="110"></SparklineAxisSettings>
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }" 
                             Size="4">
    </SparklineMarkerSettings>
</SfSparkline>

@code {
    public double[] StockPrices = new double[] { 95.5, 98.2, 97.8, 102.3, 100.5, 104.7, 103.2 };
}
```

**When to use Line:**
- Continuous data over time
- Stock prices, temperatures, metrics
- Showing trends and patterns
- Data with many points (10+)

## Area Sparkline

A filled line chart that emphasizes the magnitude of values by filling the area beneath the line. Set `Type="SparklineType.Area"` with customization properties `Fill` and `Opacity` for appearance control.

### Basic Area Sparkline

Create an area sparkline using `Type="SparklineType.Area"` with `Fill` property to set the fill color and `Opacity` for transparency control:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@SalesData" 
              Type="SparklineType.Area"
              Height="60px" 
              Width="200px"
              Fill="#4CAF50"
              Opacity="0.6">
</SfSparkline>

@code {
    public double[] SalesData = new double[] { 3000, 4200, 3800, 5100, 4700, 5500, 5200 };
}
```

### Area with Gradient Effect

Enhance area sparkline with data binding using `TValue`, `XName`, `YName` properties and `SparklineTooltipSettings` for interactive tooltips:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@RevenueData" 
              TValue="RevenueInfo"
              XName="Quarter"
              YName="Amount"
              Type="SparklineType.Area"
              Height="80px" 
              Width="300px"
              Fill="4CAF50">
    <SparklineTooltipSettings TValue="RevenueInfo" 
                              Visible="true" 
                              Format="${Quarter}: ${Amount}">
    </SparklineTooltipSettings>
</SfSparkline>

@code {
    public class RevenueInfo
    {
        public string Quarter { get; set; }
        public double Amount { get; set; }
    }

    public List<RevenueInfo> RevenueData = new List<RevenueInfo>
    {
        new RevenueInfo { Quarter = "Q1", Amount = 250000 },
        new RevenueInfo { Quarter = "Q2", Amount = 320000 },
        new RevenueInfo { Quarter = "Q3", Amount = 290000 },
        new RevenueInfo { Quarter = "Q4", Amount = 380000 }
    };
}
```

**When to use Area:**
- Cumulative or total values over time
- Revenue, sales totals, accumulated metrics
- Emphasizing volume or magnitude
- Comparing filled areas visually

## Column Sparkline

Displays data as vertical bars, ideal for comparing discrete values. Set `Type="SparklineType.Column"` with properties `Fill` for color and `NegativePointColor` for negative values.

### Basic Column Sparkline

Create a column sparkline using `Type="SparklineType.Column"` with `Fill` property for bar color:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@MonthlySales" 
              Type="SparklineType.Column"
              Height="60px" 
              Width="250px"
              Fill="#FF9800">
</SfSparkline>

@code {
    public int[] MonthlySales = new int[] { 45, 67, 52, 78, 65, 82, 70, 88 };
}
```

### Column with Negative Values

Display positive and negative values using `NegativePointColor` property and `SparklineAxisSettings` with `Value="0"` for axis baseline, along with `SparklineAxisLineSettings` for customization:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@ProfitLoss" 
              Type="SparklineType.Column"
              Height="80px" 
              Width="300px"
              Fill="#4CAF50" NegativePointColor ="#F44336">
    <SparklineAxisSettings Value="0" >
    
        <SparklineAxisLineSettings Visible="true"
                                   Color="#000"
                                   Width="1">
        </SparklineAxisLineSettings>

    </SparklineAxisSettings>
</SfSparkline>

@code {
    public int[] ProfitLoss = new int[] { 12, -5, 8, -3, 15, -8, 10, 6, -4, 11 };
}
```

### Column with Custom Colors

Highlight specific data points using `HighPointColor` and `LowPointColor` properties along with `SparklineMarkerSettings` for marker visibility control:

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@CategoryScores" 
              TValue="ScoreInfo"
              XName="Category"
              YName="Score"
              ValueType="SparklineValueType.Category"
              Type="SparklineType.Column"
              Height="80px" 
              Width="300px"
              HighPointColor="#FFD700"
              LowPointColor="#FF6B6B">
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.High, VisibleType.Low }">
    </SparklineMarkerSettings>
</SfSparkline>

@code {
    public class ScoreInfo
    {
        public string Category { get; set; }
        public int Score { get; set; }
    }

    public List<ScoreInfo> CategoryScores = new List<ScoreInfo>
    {
        new ScoreInfo { Category = "A", Score = 85 },
        new ScoreInfo { Category = "B", Score = 92 },
        new ScoreInfo { Category = "C", Score = 78 },
        new ScoreInfo { Category = "D", Score = 95 },
        new ScoreInfo { Category = "E", Score = 88 }
    };
}
```

**When to use Column:**
- Discrete, categorical data
- Monthly counts, category comparisons
- Showing individual values clearly
- Emphasizing specific data points
- Negative values (profit/loss scenarios)

## WinLoss Sparkline

Specialized chart for binary outcomes: wins, losses, and ties. Set `Type="SparklineType.WinLoss"` with properties `Fill` for wins, `NegativePointColor` for losses, and `TiePointColor` for ties.

### Basic WinLoss Sparkline

Create a WinLoss sparkline using `Type="SparklineType.WinLoss"` with color properties `Fill`, `NegativePointColor`, and `TiePointColor`:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@GameResults" 
              Type="SparklineType.WinLoss"
              Height="60px" 
              Width="300px"
              Fill="#4CAF50"
              TiePointColor="#FFC107" NegativePointColor="#F44336">
</SfSparkline>

@code {
    // 1 = Win, -1 = Loss, 0 = Tie
    public int[] GameResults = new int[] { 1, 1, -1, 1, 0, 1, -1, -1, 1, 1, 0, 1 };
}
```

### WinLoss with Axis Line

Add axis baseline using `SparklineAxisSettings` with `MinY`, `MaxY`, and `Value` properties, along with `SparklineAxisLineSettings` for line customization:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@TradingOutcomes" 
              Type="SparklineType.WinLoss"
              Height="80px" 
              Width="400px"
              Fill="#28a745"
              TiePointColor="#ffc107" NegativePointColor="#dc3545">
    <SparklineAxisSettings MinY="-1" 
                           MaxY="1" 
                           Value="0">

        <SparklineAxisLineSettings Visible="true"
                                   Color="#333"
                                   Width="2">
        </SparklineAxisLineSettings>
    </SparklineAxisSettings>
    <SparklineTooltipSettings TValue="int" 
                              Visible="true" 
                              >
    </SparklineTooltipSettings>
</SfSparkline>

@code {
    public int[] TradingOutcomes = new int[] { 1, 1, -1, 0, 1, 1, 1, -1, 0, -1, 1, 1, -1, 1 };
}
```

### Sports Team Performance

Create a team record visualization with `Type="SparklineType.WinLoss"` using `Fill`, `NegativePointColor`, and `TiePointColor` properties for visual distinction:

```razor
@using Syncfusion.Blazor.Charts
<div class="team-record">
    <h4>Season Record: 8-4-2</h4>
    <SfSparkline DataSource="@SeasonResults" 
                  Type="SparklineType.WinLoss"
                  Height="50px" 
                  Width="350px"
                  Fill="#0066cc"
                  TiePointColor="#999999" NegativePointColor="#cc0000">
    </SfSparkline>
</div>

@code {
    public int[] SeasonResults = new int[] 
    { 
        1, 1, -1, 1, 0, -1, 1, 1, 1, 0, -1, -1, 1, 1 
    };
}
```

**When to use WinLoss:**
- Binary outcomes (success/failure)
- Game results, trading outcomes
- Pass/fail scenarios
- Up/down trends
- Quality control (pass/fail tests)

**Data Values:**
- `1` = Win/Success/Positive (uses Fill color)
- `-1` = Loss/Failure/Negative (uses NegativePointColor)
- `0` = Tie/Neutral (uses TiePointColor)

## Pie Sparkline

Displays data as proportional segments in a circular chart, showing part-to-whole relationships. Set `Type="SparklineType.Pie"` with optional `SparklineDataLabelSettings` for label display.

### Basic Pie Sparkline

Create a pie sparkline using `Type="SparklineType.Pie"` with `Height` and `Width` properties to define chart dimensions:

```razor
<SfSparkline DataSource="@MarketShare" 
              Type="SparklineType.Pie"
              Height="100px" 
              Width="100px">
</SfSparkline>

@code {
    public double[] MarketShare = new double[] { 35, 28, 20, 17 };
}
```

### Pie with Labels

Add data labels using `SparklineDataLabelSettings` with `Visible` property to display segment information alongside `XName` and `YName` for data mapping:

```razor
@using Syncfusion.Blazor.Charts
<SfSparkline DataSource="@BudgetAllocation" 
              TValue="BudgetItem"
              XName="Category"
              YName="Amount"
              Type="SparklineType.Pie"
              Height="120px" 
              Width="120px">
    <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.All }">
    </SparklineDataLabelSettings>
</SfSparkline>

@code {
    public class BudgetItem
    {
        public string Category { get; set; }
        public double Amount { get; set; }
    }

    public List<BudgetItem> BudgetAllocation = new List<BudgetItem>
    {
        new BudgetItem { Category = "Marketing", Amount = 45000 },
        new BudgetItem { Category = "Development", Amount = 65000 },
        new BudgetItem { Category = "Sales", Amount = 38000 },
        new BudgetItem { Category = "Support", Amount = 22000 }
    };
}
```

**When to use Pie:**
- Proportional data (percentages, shares)
- Budget allocation, market share
- Distribution visualization
- Limited categories (3-6 segments ideal)
- Part-to-whole relationships

**Note:** Pie sparklines are less common than other types due to their larger footprint and reduced effectiveness at small sizes.

## Type Selection Guide

Select the appropriate `Type` property value (`SparklineType.Line`, `.Area`, `.Column`, `.WinLoss`, or `.Pie`) based on your data characteristics and visualization needs.

### Decision Matrix

| Scenario | Recommended Type | Reason |
|----------|-----------------|--------|
| Stock prices over time | Line | Continuous trend visualization |
| Monthly sales counts | Column | Discrete values, easy comparison |
| Revenue accumulation | Area | Emphasizes cumulative magnitude |
| Game win/loss record | WinLoss | Binary outcome visualization |
| Budget distribution | Pie | Part-to-whole proportions |
| Temperature trends | Line | Continuous variable over time |
| Quarterly performance | Column | Distinct time periods |
| Trading outcomes | WinLoss | Success/failure tracking |
| Negative/positive values | Column | Clear visual distinction with colors |

### By Data Characteristics

Use `Type` property value based on your data pattern distribution.

**Continuous Data (many connected points):**
- Use **Line** or **Area**
- Examples: Time series, temperatures, prices

**Discrete Data (separate, countable items):**
- Use **Column**
- Examples: Monthly totals, category counts

**Binary Data (two or three outcomes):**
- Use **WinLoss**
- Examples: Pass/fail, win/loss/tie

**Proportional Data (parts of a whole):**
- Use **Pie**
- Examples: Percentages, market share

### Performance Considerations

Optimize rendering performance by considering data point count based on `Type` property selection.

**Data Point Count:**
- Line/Area: Efficient with 100+ points
- Column: Best with <50 points (visual clarity)
- WinLoss: Any count (equal-width bars)
- Pie: Best with 3-8 segments

**Visual Clarity:**
- Small spaces: Line or WinLoss
- Medium spaces: Column or Area
- Larger spaces: Any type, Pie requires most space

## Mixing Types in a Dashboard

Display multiple sparklines with different `Type` property values (e.g., `SparklineType.Area`, `.Column`, `.WinLoss`) in a single dashboard for comprehensive insights:

```razor
@using Syncfusion.Blazor.Charts
<div class="metrics-dashboard">
    <div class="metric-card">
        <h5>Revenue Trend</h5>
        <SfSparkline DataSource="@Revenue" Type="SparklineType.Area" Height="50px" Width="150px">
        </SfSparkline>
    </div>
    
    <div class="metric-card">
        <h5>Monthly Orders</h5>
        <SfSparkline DataSource="@Orders" Type="SparklineType.Column" Height="50px" Width="150px">
        </SfSparkline>
    </div>
    
    <div class="metric-card">
        <h5>Conversion Success</h5>
        <SfSparkline DataSource="@Conversions" Type="SparklineType.WinLoss" Height="50px" Width="150px">
        </SfSparkline>
    </div>
</div>

@code {
    public double[] Revenue = new double[] { 25000, 28000, 26000, 31000, 29000, 33000 };
    public int[] Orders = new int[] { 450, 520, 480, 610, 550, 680 };
    public int[] Conversions = new int[] { 1, 1, -1, 1, 0, 1, -1, 1, 1, 0 };
}
```

**Choose the type that best communicates your data story to users.**
