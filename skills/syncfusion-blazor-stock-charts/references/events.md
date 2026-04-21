# Events

This guide explains how to handle Stock Chart events for user interactions, lifecycle management, and dynamic chart updates.

## Table of Contents
- [Overview](#overview)
- [Lifecycle Events](#lifecycle-events)
- [Interaction Events](#interaction-events)
- [Interaction Notes](#interaction-notes)
- [Range Change Event](#range-change-event)
- [Event Handler Patterns](#event-handler-patterns)
- [Common Issues](#common-issues)

## Overview

The Stock Chart component provides comprehensive event handling for chart lifecycle, data selection, zooming, tooltips, exporting, and printing.

**Key Event Categories:**
- **Lifecycle Events** - OnLoaded
- **Interaction Events** - OnPointClick, SharedTooltipRendering, TooltipRendering
- **Axis and Range Events** - AxisLabelRendering, PeriodChanged, RangeChange, OnZooming
- **Output Events** - Exporting, ExportCompleted, OnPrintComplete

## Lifecycle Events

### OnLoaded Event

In the `OnLoaded` event, the `OnLoaded` property will be used to trigger the event callback when the chart completes rendering:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Stock Chart with OnLoaded Event">
    <StockChartEvents OnLoaded="ChartLoadedHandler"></StockChartEvents>
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Line" 
                          XName="Date" 
                          YName="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

<div>Status: @LoadStatus</div>

@code {
    private string LoadStatus = "Loading...";

    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Close = 183.7 },
        new StockInfo { Date = new DateTime(2024, 01, 15), Close = 188.2 }
    };

    private void ChartLoadedHandler(StockChartEventArgs args)
    {
        LoadStatus = $"Chart loaded successfully at {DateTime.Now:HH:mm:ss}";
        Console.WriteLine("Stock Chart rendering complete");
    }
}
```

**Use Cases:**
- Perform actions after chart initialization
- Log analytics when chart renders
- Trigger additional data loading
- Update UI components based on chart state

### Exporting Event

In the `Exporting` event, the `Exporting` property will be used to trigger the event callback when the export process starts:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Stock Chart with Exporting Event">
    <StockChartEvents Exporting="ChartExportingHandler"></StockChartEvents>
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
        new StockInfo { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5 }
    };

    private void ChartExportingHandler(ChartExportEventArgs args)
    {
        Console.WriteLine("Export started");
        // Modify export properties before the file is generated
    }
}
```

## Interaction Events

### OnPointClick Event

In the `OnPointClick` event, the `OnPointClick` property will be used along with `StockChartPointEventArgs` containing the `Point` property to access data when a data point is clicked:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Click on Data Points">
    <StockChartEvents OnPointClick="PointClickHandler"></StockChartEvents>
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

<div style="margin-top: 20px;">
    <h4>Selected Point Details:</h4>
    <p><strong>Date:</strong> @SelectedDate</p>
    <p><strong>Open:</strong> $@SelectedOpen</p>
    <p><strong>Close:</strong> $@SelectedClose</p>
    <p><strong>High:</strong> $@SelectedHigh</p>
    <p><strong>Low:</strong> $@SelectedLow</p>
</div>

@code {
    private string SelectedDate = "None";
    private string SelectedOpen = "0.00";
    private string SelectedClose = "0.00";
    private string SelectedHigh = "0.00";
    private string SelectedLow = "0.00";

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

    private void PointClickHandler(StockChartPointEventArgs args)
    {
        var point = args.Point;
        SelectedDate = point.X.ToString();
        SelectedOpen = point.Open.ToString("F2");
        SelectedClose = point.Close.ToString("F2");
        SelectedHigh = point.High.ToString("F2");
        SelectedLow = point.Low.ToString("F2");
    }
}
```

### SharedTooltipRendering Event

In the `SharedTooltipRendering` event, the `SharedTooltipRendering` property will be used along with `SharedTooltipRenderEventArgs` containing the `Point` property before the shared tooltip is rendered:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Shared Tooltip Demo">
    <StockChartEvents SharedTooltipRendering="SharedTooltipHandler"></StockChartEvents>
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Line" 
                          XName="Date" 
                          YName="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

<div>
    <p>Hovering: @HoverInfo</p>
</div>

@code {
    private string HoverInfo = "Move the pointer over the chart";

    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 01, 08), Close = 183.7 },
        new StockInfo { Date = new DateTime(2024, 01, 15), Close = 188.2 }
    };

    private void SharedTooltipHandler(SharedTooltipRenderEventArgs args)
    {
        HoverInfo = $"Shared tooltip rendered for {args.Point?.X}";
    }
}
```

## Interaction Notes

The validated `StockChartEvents` API exposes point click, tooltip rendering, zooming, period change, range change, export, and print callbacks. Use `OnPointClick`, `SharedTooltipRendering`, `TooltipRendering`, and `OnZooming` for pointer-driven scenarios, and rely on `StockChartMouseEventArgs` only where the component API explicitly provides it.

## Range Change Event

In the `RangeChange` event, the `RangeChange` property will be used along with `StockChartRangeChangeEventArgs` containing the `Start` and `End` properties when the date range changes (via period selector or range selector):

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Range Change Event Demo">
    <StockChartEvents RangeChange="RangeChangeHandler"></StockChartEvents>
    <StockChartPeriods>
        <StockChartPeriod Text="1M" IntervalType="RangeIntervalType.Months" Interval="1"></StockChartPeriod>
        <StockChartPeriod Text="3M" IntervalType="RangeIntervalType.Months" Interval="3"></StockChartPeriod>
        <StockChartPeriod Text="6M" IntervalType="RangeIntervalType.Months" Interval="6"></StockChartPeriod>
    </StockChartPeriods>
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Line" 
                          XName="Date" 
                          YName="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

<div style="margin-top: 20px;">
    <h4>Current Range:</h4>
    <p><strong>Start:</strong> @RangeStart</p>
    <p><strong>End:</strong> @RangeEnd</p>
</div>

@code {
    private string RangeStart = "Not set";
    private string RangeEnd = "Not set";

    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 175.5 },
        new StockInfo { Date = new DateTime(2024, 02, 01), Close = 179.5 },
        new StockInfo { Date = new DateTime(2024, 03, 01), Close = 183.7 },
        new StockInfo { Date = new DateTime(2024, 04, 01), Close = 188.2 },
        new StockInfo { Date = new DateTime(2024, 05, 01), Close = 192.8 },
        new StockInfo { Date = new DateTime(2024, 06, 01), Close = 197.5 }
    };

    private void RangeChangeHandler(StockChartRangeChangeEventArgs args)
    {
        RangeStart = args.Start.ToString("MMM dd, yyyy");
        RangeEnd = args.End.ToString("MMM dd, yyyy");
    }
}
```

## Event Handler Patterns

### Drill-Down Scenario

In the Drill-Down pattern, the `OnPointClick` property will be used to handle navigation when clicking data points:

```razor
@using Syncfusion.Blazor.Charts
@inject NavigationManager NavigationManager

<SfStockChart Title="Click to View Details">
    <StockChartEvents OnPointClick="HandleDrillDown"></StockChartEvents>
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
        new StockInfo { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7 }
    };

    private void HandleDrillDown(StockChartPointEventArgs args)
    {
        var date = args.Point.X.ToString();
        NavigationManager.NavigateTo($"/stock-details/{date}");
    }
}
```

### Dynamic Data Updates

In the Dynamic Data Updates pattern, the `RangeChange` property will be used along with `StockChartRangeChangeEventArgs` containing `Start` and `End` properties to update chart data based on user interaction:

```razor
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Dynamic Data Updates">
    <StockChartEvents RangeChange="LoadDataForRange"></StockChartEvents>
    <StockChartPeriods>
        <StockChartPeriod Text="1W" IntervalType="RangeIntervalType.Days" Interval="7"></StockChartPeriod>
        <StockChartPeriod Text="1M" IntervalType="RangeIntervalType.Months" Interval="1"></StockChartPeriod>
    </StockChartPeriods>
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" 
                          Type="ChartSeriesType.Line" 
                          XName="Date" 
                          YName="Close">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

<div>Loading: @IsLoading</div>

@code {
    private bool IsLoading = false;

    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }

    public List<StockInfo> StockData = new List<StockInfo>
    {
        new StockInfo { Date = new DateTime(2024, 01, 01), Close = 175.5 },
        new StockInfo { Date = new DateTime(2024, 02, 01), Close = 179.5 }
    };

    private async Task LoadDataForRange(StockChartRangeChangeEventArgs args)
    {
        IsLoading = true;
        StateHasChanged();

        // Simulate API call to load data for selected range
        await Task.Delay(500);
        
        // Update StockData based on args.Start and args.End
        Console.WriteLine($"Loading data from {args.Start} to {args.End}");
        
        IsLoading = false;
        StateHasChanged();
    }
}
```

## Common Issues

### Event Handler Not Firing

In Event Handler Not Firing issue, the `StockChartEvents` element and the event property (e.g., `OnPointClick`) must be properly configured.

**Problem:** Event handler method doesn't execute.

**Solution:** Ensure the event is properly wired in `StockChartEvents`:
```razor
<StockChartEvents OnPointClick="PointClickHandler"></StockChartEvents>
```

### Null Reference in Event Args

In Null Reference in Event Args issue, the `StockChartPointEventArgs` parameter and its `Point` property must be null-checked before access.

**Problem:** Accessing properties in event args causes null reference exception.

**Solution:** Check for null before accessing properties:
```csharp
private void PointClickHandler(StockChartPointEventArgs args)
{
    if (args?.Point != null)
    {
        var value = args.Point.Close;
    }
}
```

### UI Not Updating After Event

In UI Not Updating After Event issue, the `StateHasChanged()` method must be called after modifying state variables to reflect changes in the component.

**Problem:** UI doesn't reflect changes made in event handler.

**Solution:** Call `StateHasChanged()` to trigger re-render:
```csharp
private void ChartClickHandler(StockChartPointEventArgs args)
{
    ClickPosition = $"Point clicked: {args.Point?.X}";
    StateHasChanged();
}
```

### Performance Issues with Mouse Move

In Performance Issues with Mouse Move issue, the event callback and timing logic using `DateTime.Now` property must be implemented to throttle frequent event calls.

**Problem:** Excessive hover processing causes performance degradation.

**Solution:** Throttle event handling or avoid heavy operations:
```csharp
private DateTime lastUpdate = DateTime.MinValue;

private void MouseMoveHandler(StockChartPointEventArgs args)
{
    if ((DateTime.Now - lastUpdate).TotalMilliseconds < 100)
        return;
    
    lastUpdate = DateTime.Now;
    // Handle event
}
```

