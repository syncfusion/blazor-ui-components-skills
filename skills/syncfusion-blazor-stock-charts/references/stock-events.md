# Stock Events Reference

This reference guide covers stock events in Syncfusion Blazor Stock Chart, enabling visual markers for significant occurrences like earnings releases, dividends, stock splits, and market milestones.

## Table of Contents
- [Overview](#overview)
- [Core Properties](#core-properties)
- [Complete Example](#complete-example)
- [Advanced Customization](#advanced-customization)
- [Event Categories and Use Cases](#event-categories-and-use-cases)
- [Dynamic Event Generation](#dynamic-event-generation)
- [When to Use Stock Events](#when-to-use-stock-events)
- [Common Issues and Troubleshooting](#common-issues-and-troubleshooting)
- [Best Practices](#best-practices)

## Overview

Stock events mark notable occurrences on the chart at specific dates using the `Date`, `Type`, and `Background` properties with customizable shapes, colors, and tooltips. They provide context for price movements and help identify patterns related to corporate actions or market events through the `Text` and `Description` properties.

Key features:
- Multiple event shapes (Flag, Circle, Square, Triangle, Pin, Arrow, Text)
- Customizable colors, borders, and text
- Interactive tooltips on hover
- Support for series-specific or chart-wide events

## Core Properties

### Date

The `Date` property specifies when the event occurred:

```cshtml
<StockChartStockEvent Date="new DateTime(2023, 03, 15)" 
                     Text="Q1" 
                     Description="Q1 Earnings Report">
</StockChartStockEvent>
```

### Text

Short label displayed on the event marker:

```cshtml
<StockChartStockEvent Date="@EventDate" 
                     Text="Split" 
                     Description="2:1 Stock Split">
</StockChartStockEvent>
```

**Best Practices for Text:**
- Keep it short (1-5 characters)
- Use abbreviations (Q1, Q2, DIV, SPLIT)
- Use symbols when appropriate ($, %, ↑, ↓)

### Type

The `Type` property determines the event marker shape:

```cshtml
<StockChartStockEvent Date="@EventDate" 
                     Text="Q1" 
                     Type="FlagType.Flag">
</StockChartStockEvent>
```

Available shapes:
- `Flag`: Banner-style marker (default for quarters/periods)
- `Circle`: Round marker (good for general events)
- `Square`: Box marker (structured events)
- `Triangle`: Upward triangle (bullish events)
- `InvertedTriangle`: Downward triangle (bearish events)
- `ArrowUp`: Upward arrow (price increase, upgrades)
- `ArrowDown`: Downward arrow (price decrease, downgrades)
- `ArrowLeft`: Leftward arrow
- `ArrowRight`: Rightward arrow
- `Text`: Text-only marker (minimal visual impact)
- `Sign`: Plus/minus sign marker
- `Pin`: Map pin marker (location/milestone)

### Background

Sets the event marker background color:

```cshtml
<StockChartStockEvent Date="@EventDate" 
                     Text="DIV" 
                     Background="#4CAF50"
                     Type="FlagType.Circle">
</StockChartStockEvent>
```

### Description

Tooltip text shown on hover:

```cshtml
<StockChartStockEvent Date="@EventDate" 
                     Text="Q2" 
                     Description="Q2 2023 Earnings: $2.50 EPS (Beat est. $2.35)">
</StockChartStockEvent>
```

## Complete Example

### Basic Stock Events

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL Stock Price with Events">
    <StockChartStockEvents>
        <!-- Quarterly Earnings -->
        <StockChartStockEvent Date="new DateTime(2023, 01, 31)" 
                             Text="Q1" 
                             Description="Q1 2023 Earnings Report"
                             Type="FlagType.Flag"
                             Background="#6C6D6D">
            <StockChartStockEventsBorder Color="#6C6D6D"></StockChartStockEventsBorder>
        </StockChartStockEvent>
        
        <!-- Dividend Payment -->
        <StockChartStockEvent Date="new DateTime(2023, 02, 15)" 
                             Text="DIV" 
                             Description="Quarterly Dividend: $0.23/share"
                             Type="FlagType.Circle"
                             Background="#4CAF50">
            <StockChartStockEventsBorder Color="#4CAF50"></StockChartStockEventsBorder>
        </StockChartStockEvent>
        
        <!-- Stock Split -->
        <StockChartStockEvent Date="new DateTime(2023, 06, 01)" 
                             Text="Split" 
                             Description="4:1 Stock Split"
                             Type="FlagType.Square"
                             Background="#2196F3">
            <StockChartStockEventsBorder Color="#2196F3"></StockChartStockEventsBorder>
        </StockChartStockEvent>
        
        <!-- Product Launch -->
        <StockChartStockEvent Date="new DateTime(2023, 09, 12)" 
                             Text="Launch" 
                             Description="iPhone 15 Launch Event"
                             Type="FlagType.ArrowUp"
                             Background="#FF9800">
            <StockChartStockEventsBorder Color="#FF9800"></StockChartStockEventsBorder>
        </StockChartStockEvent>
    </StockChartStockEvents>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Line" 
                         XName="Date" YName="Close" Name="AAPL">
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
        new StockChartData { Date = new DateTime(2023, 01, 01), Close = 130.50 },
        new StockChartData { Date = new DateTime(2023, 02, 01), Close = 145.20 },
        new StockChartData { Date = new DateTime(2023, 03, 01), Close = 142.80 },
        new StockChartData { Date = new DateTime(2023, 04, 01), Close = 155.30 },
        new StockChartData { Date = new DateTime(2023, 05, 01), Close = 160.15 },
        new StockChartData { Date = new DateTime(2023, 06, 01), Close = 165.40 }
    };
}
```

## Advanced Customization

### Custom Text Styling

Customize event text appearance using `StockChartStockEventsTextStyle` with `Color`, `FontSize`, and `FontWeight` properties, while applying border styling through `StockChartStockEventsBorder` and `Color`, `Width` properties:

```cshtml
<StockChartStockEvent Date="@EventDate" 
                     Text="★" 
                     Description="Milestone: $1T Market Cap"
                     Background="#FFD700">
    <StockChartStockEventsBorder Color="#FFA500" Width="2"></StockChartStockEventsBorder>
    <StockChartStockEventsTextStyle Color="#FFFFFF" 
                                   FontSize="14px" 
                                   FontWeight="bold">
    </StockChartStockEventsTextStyle>
</StockChartStockEvent>
```

### Series-Specific Events

Use the `ShowOnSeries` property to control which series displays the event marker. When `ShowOnSeries` is set to `false`, the event appears on the chart but not directly on the series line:

```cshtml
<StockChartStockEvent Date="@EventDate" 
                     Text="Y4" 
                     Description="Year End 2023"
                     Type="FlagType.Pin"
                     ShowOnSeries="false"
                     Background="#6322e0">
</StockChartStockEvent>
```

**When ShowOnSeries is false:** Event appears on the chart but not directly on the series line.

## Event Categories and Use Cases

### Corporate Events

```cshtml
<!-- Earnings Reports -->
<StockChartStockEvent Date="@Q1Date" Text="Q1" Type="FlagType.Flag" Background="#6C6D6D">
</StockChartStockEvent>

<!-- Dividend Payments -->
<StockChartStockEvent Date="@DivDate" Text="DIV" Type="FlagType.Circle" Background="#4CAF50">
</StockChartStockEvent>

<!-- Stock Splits -->
<StockChartStockEvent Date="@SplitDate" Text="Split" Type="FlagType.Square" Background="#2196F3">
</StockChartStockEvent>

<!-- Buyback Announcements -->
<StockChartStockEvent Date="@BuybackDate" Text="BB" Type="FlagType.ArrowUp" Background="#9C27B0">
</StockChartStockEvent>
```

### Market Events

```cshtml
<!-- Market Highs -->
<StockChartStockEvent Date="@HighDate" Text="ATH" Type="FlagType.ArrowUp" Background="#4CAF50">
</StockChartStockEvent>

<!-- Market Lows -->
<StockChartStockEvent Date="@LowDate" Text="Low" Type="FlagType.ArrowDown" Background="#F44336">
</StockChartStockEvent>

<!-- Analyst Upgrades -->
<StockChartStockEvent Date="@UpgradeDate" Text="↑" Type="FlagType.Triangle" Background="#00BCD4">
</StockChartStockEvent>

<!-- Analyst Downgrades -->
<StockChartStockEvent Date="@DowngradeDate" Text="↓" Type="FlagType.InvertedTriangle" Background="#FF5722">
</StockChartStockEvent>
```

### Regulatory Events

```cshtml
<!-- FDA Approvals -->
<StockChartStockEvent Date="@ApprovalDate" Text="FDA" Type="FlagType.Circle" Background="#4CAF50">
</StockChartStockEvent>

<!-- Legal Issues -->
<StockChartStockEvent Date="@LegalDate" Text="Legal" Type="FlagType.Sign" Background="#F44336">
</StockChartStockEvent>
```

## Dynamic Event Generation

### Loading Events from Data Source

Load and bind events dynamically from a data source using properties like `Date`, `Text`, `Description`, `Type`, `Background`, and `ShowOnSeries` in a foreach loop:

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Stock Price with Dynamic Events">
    <StockChartStockEvents>
        @foreach (var evt in StockEvents)
        {
            <StockChartStockEvent Date="@evt.Date" 
                                 Text="@evt.Text" 
                                 Description="@evt.Description"
                                 Type="@evt.Type"
                                 Background="@evt.Background"
                                 ShowOnSeries="@evt.ShowOnSeries">
                <StockChartStockEventsBorder Color="@evt.BorderColor"></StockChartStockEventsBorder>
                <StockChartStockEventsTextStyle Color="@evt.TextColor"></StockChartStockEventsTextStyle>
            </StockChartStockEvent>
        }
    </StockChartStockEvents>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle" 
                         XName="Date" High="High" Low="Low" Open="Open" Close="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {
    public class StockEventDetails
    {
        public DateTime Date { get; set; }
        public string Text { get; set; }
        public string Description { get; set; }
        public FlagType Type { get; set; } = FlagType.Flag;
        public string Background { get; set; }
        public string BorderColor { get; set; }
        public string TextColor { get; set; } = "#FFFFFF";
        public bool ShowOnSeries { get; set; } = true;
    }

    public List<StockEventDetails> StockEvents = new List<StockEventDetails>
    {
        new StockEventDetails 
        { 
            Date = new DateTime(2023, 01, 15), 
            Text = "Q1", 
            Description = "Q1 Earnings Beat",
            Type = FlagType.Flag,
            Background = "#6C6D6D",
            BorderColor = "#6C6D6D"
        },
        new StockEventDetails 
        { 
            Date = new DateTime(2023, 02, 20), 
            Text = "DIV", 
            Description = "Dividend: $0.25",
            Type = FlagType.Circle,
            Background = "#4CAF50",
            BorderColor = "#4CAF50"
        },
        new StockEventDetails 
        { 
            Date = new DateTime(2023, 06, 01), 
            Text = "Split", 
            Description = "2:1 Stock Split",
            Type = FlagType.Square,
            Background = "#2196F3",
            BorderColor = "#2196F3"
        }
    };

    public class StockChartData
    {
        public DateTime Date { get; set; }
        public double Open { get; set; }
        public double High { get; set; }
        public double Low { get; set; }
        public double Close { get; set; }
    }

    public List<StockChartData> StockData = new List<StockChartData>
    {
        new StockChartData { Date = new DateTime(2012, 04, 02), Open = 85.9757, High = 90.6657, Low = 85.7685, Close = 90.5257 },
        new StockChartData { Date = new DateTime(2012, 04, 09), Open = 89.4471, High = 92, Low = 86.2157, Close = 86.4614 },
        new StockChartData { Date = new DateTime(2012, 04, 16), Open = 87.1514, High = 88.6071, Low = 81.4885, Close = 81.8543 },
        new StockChartData { Date = new DateTime(2012, 04, 23), Open = 81.5157, High = 88.2857, Low = 79.2857, Close = 86.1428 },
        new StockChartData { Date = new DateTime(2012, 04, 30), Open = 85.4, High = 85.4857, Low = 80.7385, Close = 80.75 },
        new StockChartData { Date = new DateTime(2012, 05, 07), Open = 80.2143, High = 82.2685, Low = 79.8185, Close = 80.9585 },
        new StockChartData { Date = new DateTime(2012, 05, 14), Open = 80.3671, High = 81.0728, Low = 74.5971, Close = 75.7685 },
        new StockChartData { Date = new DateTime(2012, 05, 21), Open = 76.3571, High = 82.3571, Low = 76.2928, Close = 80.3271 },
        new StockChartData { Date = new DateTime(2012, 05, 28), Open = 81.5571, High = 83.0714, Low = 80.0743, Close = 80.1414 }
    };
}
```

## When to Use Stock Events

### Use Stock Events When:
- Marking quarterly earnings releases
- Highlighting dividend payment dates
- Noting stock splits or reverse splits
- Indicating analyst rating changes
- Marking product launches or announcements
- Showing regulatory approvals/rejections
- Identifying market milestones (ATH, 52-week lows)
- Tracking management changes
- Noting merger/acquisition announcements

### Don't Use Stock Events When:
- Marking every price movement (use annotations instead)
- The events are too frequent (clutters the chart)
- Events aren't related to the stock specifically
- The information is better suited for tooltips or legends

## Common Issues and Troubleshooting

### Issue 1: Events Not Appearing

**Problem**: Stock events are defined but not visible on the chart.

**Solution**: Ensure the `Date` property values fall within the visible date range of your data:

```csharp
// Verify event date is within data range
var minDate = StockData.Min(d => d.Date);
var maxDate = StockData.Max(d => d.Date);
// Event date should be between minDate and maxDate
```

### Issue 2: Event Markers Overlapping

**Problem**: Multiple events on similar dates overlap and are unreadable.

**Solution**: 
1. Use different `Type` property values to vary visual appearance
2. Space events out slightly if exact dates aren't critical
3. Use the `ShowOnSeries` property set to `false` for some events to offset them

### Issue 3: Tooltip Not Showing

**Problem**: The tooltip doesn't appear on hover.

**Solution**: Ensure the `Description` property is set and contains text:

```cshtml
<StockChartStockEvent Date="@EventDate" 
                     Text="Q1" 
                     Description="This text appears in tooltip">
</StockChartStockEvent>
```

### Issue 4: Text Not Visible

**Problem**: Event text is hard to read or invisible.

**Solution**: Ensure proper contrast between the `Color` property in `StockChartStockEventsTextStyle` and the `Background` property of the event:

```cshtml
<StockChartStockEvent Background="#000000">
    <StockChartStockEventsTextStyle Color="#FFFFFF"></StockChartStockEventsTextStyle>
</StockChartStockEvent>
```

## Best Practices

1. **Use Color Coding Consistently**
   - Green (`Background` property): Positive events (dividends, earnings beats, approvals)
   - Red (`Background` property): Negative events (downgrades, legal issues, losses)
   - Blue (`Background` property): Neutral corporate actions (splits, restructuring)
   - Gray (`Background` property): Time markers (quarter ends, year ends)

2. **Choose Appropriate Shapes**
   - Flags (`Type` property = `FlagType.Flag`): Quarterly/periodic events
   - Circles (`Type` property = `FlagType.Circle`): General milestones
   - Arrows (`Type` property = `FlagType.ArrowUp/Down`): Directional events (upgrades/downgrades)
   - Triangles (`Type` property = `FlagType.Triangle`): Significant price movements
   - Pins (`Type` property = `FlagType.Pin`): Long-term milestones

3. **Write Descriptive Tooltips**
   - Include specific numbers in the `Description` property (EPS, dividend amount)
   - Provide context (beat/miss estimates) in the `Description` property
   - Keep `Description` content under 100 characters for readability

4. **Limit Event Density**
   - Don't mark more than 5-10 events per visible period
   - Group related events when possible
   - Use the `Type` property with `FlagType.Text` for less important markers

5. **Test Event Visibility**
   - Verify events are visible across different zoom levels with `Date` property values
   - Test with various themes (light/dark) using `Background` and text `Color` properties
   - Ensure text is readable at default size using `StockChartStockEventsTextStyle` properties

6. **Synchronize with Data**
   - Load events from the same data source as prices using the `Date` property when possible
   - Keep `Date` property values consistent with data granularity
   - Update the `StockChartStockEvents` collection when data changes

This reference provides developers with comprehensive guidance for implementing stock events in Syncfusion Blazor Stock Charts, enabling rich contextual information for financial analysis.
