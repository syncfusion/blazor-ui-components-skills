# Zooming, Panning, and Crosshair Reference

This comprehensive reference guide covers zooming, panning, and crosshair features in Syncfusion Blazor Stock Chart, enabling interactive data exploration and precise value identification.

## Table of Contents

- [Overview](#overview)
- [Zooming](#zooming)
  - [Zoom Modes](#zoom-modes)
  - [Zoom Configuration](#zoom-configuration)
  - [Zoom Toolbar](#zoom-toolbar)
  - [Scrollbar](#scrollbar)
  - [Auto Interval on Zooming](#auto-interval-on-zooming)
- [Panning](#panning)
  - [Enabling Panning](#enabling-panning)
  - [Pan Configuration](#pan-configuration)
- [Crosshair](#crosshair)
  - [Basic Crosshair](#basic-crosshair)
  - [Snap to Data](#snap-to-data)
  - [Axis Tooltips](#axis-tooltips)
  - [Trackball Mode](#trackball-mode)
  - [Customization](#customization)
- [Combined Examples](#combined-examples)
- [When to Use](#when-to-use)
- [Common Issues and Troubleshooting](#common-issues-and-troubleshooting)
- [Best Practices](#best-practices)

## Overview

The Stock Chart provides three complementary features for data exploration:

- **Zooming**: Magnify specific regions of the chart for detailed analysis
- **Panning**: Navigate through zoomed data without changing zoom level
- **Crosshair**: Display vertical and horizontal lines showing exact axis values at cursor position

These features work together to provide a powerful, interactive charting experience.

## Zooming

### Zoom Modes

The Stock Chart supports three zooming interactions:

#### Selection Zooming

Click and drag to select a rectangular area to zoom. The `EnableSelectionZooming` property enables this interactive selection-based zoom feature:

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL Stock Price">
    <StockChartZoomSettings EnableSelectionZooming="true"></StockChartZoomSettings>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle" 
                         XName="Date" High="High" Low="Low" Open="Open" Close="Close">
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
        new StockChartData { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5, Volume = 12000000 },
        new StockChartData { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7, Volume = 15000000 },
        new StockChartData { Date = new DateTime(2024, 01, 15), Open = 183.7, High = 190.5, Low = 182.5, Close = 188.2, Volume = 18000000 }
    };
}
```

#### Mouse Wheel Zooming

Zoom in/out using the mouse scroll wheel. The `EnableMouseWheelZooming` property enables scroll-based zoom functionality:

```cshtml
<StockChartZoomSettings EnableMouseWheelZooming="true"></StockChartZoomSettings>
```

**Usage:**
- Scroll up: Zoom in
- Scroll down: Zoom out
- Zooms toward cursor position

#### Pinch Zooming

Pinch gesture zooming on touch-enabled devices. The `EnablePinchZooming` property enables multi-touch pinch zoom support:

```cshtml
<StockChartZoomSettings EnablePinchZooming="true"></StockChartZoomSettings>
```

**Note:** Only supported on browsers with multi-touch gesture support.

### Zoom Configuration

#### Enable All Zoom Types

```cshtml
<StockChartZoomSettings EnableSelectionZooming="true"
                        EnableMouseWheelZooming="true"
                        EnablePinchZooming="true">
</StockChartZoomSettings>
```

#### Zoom Mode (Axis Direction)

Control which axes are zoomed using the `Mode` property. The `ZoomMode` enum provides options to zoom specific axes:

```cshtml
<!-- Zoom both axes (default) -->
<StockChartZoomSettings EnableSelectionZooming="true" 
                        Mode="ZoomMode.XY">
</StockChartZoomSettings>

<!-- Zoom X-axis only (horizontal) -->
<StockChartZoomSettings EnableSelectionZooming="true" 
                        Mode="ZoomMode.X">
</StockChartZoomSettings>

<!-- Zoom Y-axis only (vertical) -->
<StockChartZoomSettings EnableSelectionZooming="true" 
                        Mode="ZoomMode.Y">
</StockChartZoomSettings>
```

| Mode | Description | Best For |
|------|-------------|----------|
| `XY` | Zoom both axes | General data exploration |
| `X` | Zoom time axis only | Focusing on specific time periods |
| `Y` | Zoom price axis only | Analyzing price ranges |

### Zoom Toolbar

After zooming, a toolbar appears with zoom controls. The `ToolbarItems` property defines which zoom control buttons are displayed in the toolbar:

```cshtml
@using Syncfusion.Blazor.Charts

<StockChartZoomSettings EnableSelectionZooming="true"
                        ToolbarItems="@ZoomTools">
</StockChartZoomSettings>

@code {
    public List<ToolbarItems> ZoomTools = new List<ToolbarItems>() 
    { 
        ToolbarItems.Zoom, 
        ToolbarItems.ZoomIn, 
        ToolbarItems.ZoomOut, 
        ToolbarItems.Pan, 
        ToolbarItems.Reset 
    };
}
```

Available toolbar items:
- **Zoom**: Enables selection zooming
- **ZoomIn**: Zoom in by fixed increment
- **ZoomOut**: Zoom out by fixed increment
- **Pan**: Enable pan mode
- **Reset**: Reset to original view

#### Custom Toolbar Selection

```cshtml
@using Syncfusion.Blazor.Charts

@code {
    // Show only essential tools
    public List<ToolbarItems> EssentialTools = new List<ToolbarItems>() 
    { 
        ToolbarItems.Reset, 
        ToolbarItems.Pan 
    };
}

<StockChartZoomSettings EnableSelectionZooming="true"
                        ToolbarItems="@EssentialTools">
</StockChartZoomSettings>
```

### Scrollbar

Add a scrollbar for zoomed navigation. The `EnableScrollbar` property displays a scrollbar control for navigating zoomed content:

```cshtml
@using Syncfusion.Blazor.Charts

<StockChartZoomSettings EnableMouseWheelZooming="true"
                        EnableScrollbar="true"
                        EnablePinchZooming="true"
                        EnableSelectionZooming="true">
</StockChartZoomSettings>
```

**Features:**
- Appears automatically when zoomed
- Drag scrollbar to navigate
- Resize scrollbar ends to adjust zoom level
- Provides overview of entire dataset

### Auto Interval on Zooming

Automatically adjust axis intervals based on the zoomed range. The `EnableAutoIntervalOnZooming` property on the primary X-axis automatically optimizes axis label density during zoom operations:

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL Stock Price">
    <StockChartZoomSettings EnableMouseWheelZooming="true"
                           EnablePinchZooming="true"
                           EnableSelectionZooming="true">
    </StockChartZoomSettings>
    
    <StockChartPrimaryXAxis EnableAutoIntervalOnZooming="true">
        <StockChartAxisMajorGridLines Width="0"></StockChartAxisMajorGridLines>
    </StockChartPrimaryXAxis>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle" 
                         XName="Date" High="High" Low="Low" Open="Open" Close="Close">
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
        new StockChartData { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5, Volume = 12000000 },
        new StockChartData { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7, Volume = 15000000 },
        new StockChartData { Date = new DateTime(2024, 01, 15), Open = 183.7, High = 190.5, Low = 182.5, Close = 188.2, Volume = 18000000 }
    };
}
```

**Benefits:**
- Prevents label overlap when zoomed in
- Optimizes label density
- Improves readability

## Panning

### Enabling Panning

Panning is enabled by default but can be controlled using the `EnablePan` property in the `StockChartZoomSettings`:

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL Stock Price">
    <StockChartZoomSettings EnableSelectionZooming="true"
                           EnablePan="true">
    </StockChartZoomSettings>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle" 
                         XName="Date" High="High" Low="Low" Open="Open" Close="Close">
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
        new StockChartData { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5, Volume = 12000000 },
        new StockChartData { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7, Volume = 15000000 },
        new StockChartData { Date = new DateTime(2024, 01, 15), Open = 183.7, High = 190.5, Low = 182.5, Close = 188.2, Volume = 18000000 }
    };
}
```

**How to Pan:**
1. Zoom into the chart (using any zoom method)
2. Click and drag to pan
3. Or use the Pan button in the zoom toolbar

### Pan Configuration

#### Pan Without Toolbar

Enable panning immediately without requiring toolbar activation. The `ZoomFactor` and `ZoomPosition` properties on the primary X-axis configure initial zoom state for immediate panning:

```cshtml
@using Syncfusion.Blazor.Charts

<StockChartPrimaryXAxis ZoomFactor="0.5" ZoomPosition="0.3">
    <StockChartAxisMajorGridLines Width="0"></StockChartAxisMajorGridLines>
</StockChartPrimaryXAxis>

<StockChartZoomSettings EnableSelectionZooming="true" 
                        EnablePan="true">
</StockChartZoomSettings>
```

**Initial Zoom Properties:**
- **ZoomFactor**: Zoom level (0-1, where 1 is no zoom)
- **ZoomPosition**: Initial position (0-1, where 0 is start)

## Crosshair

### Basic Crosshair

Display vertical and horizontal lines at cursor position. The `Enable` property in `StockChartCrosshairSettings` activates the crosshair feature:

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL Stock Price">
    <StockChartCrosshairSettings Enable="true"></StockChartCrosshairSettings>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle" 
                         XName="Date" High="High" Low="Low" Open="Open" Close="Close">
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
        new StockChartData { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5, Volume = 12000000 },
        new StockChartData { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7, Volume = 15000000 },
        new StockChartData { Date = new DateTime(2024, 01, 15), Open = 183.7, High = 190.5, Low = 182.5, Close = 188.2, Volume = 18000000 }
    };
}
```

### Snap to Data

Align crosshair to nearest data point using the `SnapToData` property. This boolean property determines whether the crosshair snaps to discrete data points or allows free cursor movement:

```cshtml
@using Syncfusion.Blazor.Charts

<StockChartCrosshairSettings Enable="true" 
                            SnapToData="true">
</StockChartCrosshairSettings>
```

**When SnapToData is true:**
- Crosshair snaps to closest point
- More precise data point identification
- Better for discrete data

**When SnapToData is false:**
- Crosshair follows cursor exactly
- Shows interpolated values
- Better for continuous analysis

### Axis Tooltips

Show axis values in tooltips where crosshair intersects. The `Enable` property in `StockChartAxisCrosshairTooltip` displays axis values, while the `Fill` property controls the tooltip background color:

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL Stock Price">
    <StockChartCrosshairSettings Enable="true"></StockChartCrosshairSettings>
    
    <StockChartPrimaryXAxis>
        <StockChartAxisCrosshairTooltip Enable="true"></StockChartAxisCrosshairTooltip>
    </StockChartPrimaryXAxis>
    
    <StockChartPrimaryYAxis>
        <StockChartAxisCrosshairTooltip Enable="true" 
                                       Fill="#4CAF50">
        </StockChartAxisCrosshairTooltip>
    </StockChartPrimaryYAxis>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle" 
                         XName="Date" High="High" Low="Low" Open="Open" Close="Close">
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
        new StockChartData { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5, Volume = 12000000 },
        new StockChartData { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7, Volume = 15000000 },
        new StockChartData { Date = new DateTime(2024, 01, 15), Open = 183.7, High = 190.5, Low = 182.5, Close = 188.2, Volume = 18000000 }
    };
}
```

### Trackball Mode

Show multiple series values at crosshair position. The `LineType` property set to `Vertical` creates a vertical trackball line, while the `Shared` property in `StockChartTooltipSettings` combines multiple series values in a single tooltip:

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="AAPL Stock Price">
    <StockChartCrosshairSettings Enable="true" 
                                LineType="LineType.Vertical">
    </StockChartCrosshairSettings>
    
    <StockChartTooltipSettings Enable="true" 
                              Shared="true">
    </StockChartTooltipSettings>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Line" 
                         XName="Date" YName="Close" Name="Close">
        </StockChartSeries>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Line" 
                         XName="Date" YName="High" Name="High">
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
        new StockChartData { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5, Volume = 12000000 },
        new StockChartData { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7, Volume = 15000000 },
        new StockChartData { Date = new DateTime(2024, 01, 15), Open = 183.7, High = 190.5, Low = 182.5, Close = 188.2, Volume = 18000000 }
    };
}
```

**Trackball Features:**
- Single vertical line
- Shows all series values at that x-position
- Shared tooltip with multiple values

### Customization

#### Line Styling

Customize crosshair appearance using the `StockChartCrosshairLine` component with the `Width`, `Color`, and `DashArray` properties:

```cshtml
@using Syncfusion.Blazor.Charts

<StockChartCrosshairSettings Enable="true">
    <StockChartCrosshairLine Width="2" 
                            Color="#FF6B6B" 
                            DashArray="5,5">
    </StockChartCrosshairLine>
</StockChartCrosshairSettings>
```

Properties:
- **Width**: Line thickness
- **Color**: Line color
- **DashArray**: Dash pattern (e.g., "5,5" for dashed)

#### Tooltip Styling

Style crosshair tooltips using `StockChartAxisCrosshairTooltip` with properties like `Fill` for background color and `StockChartAxisCrosshairTooltipTextStyle` for text styling:

```cshtml
@using Syncfusion.Blazor.Charts

<StockChartPrimaryYAxis>
    <StockChartAxisCrosshairTooltip Enable="true" 
                                   Fill="#2196F3">
        <StockChartAxisCrosshairTooltipTextStyle Color="#FFFFFF" 
                                                 Size="12px" 
                                                 FontWeight="bold">
        </StockChartAxisCrosshairTooltipTextStyle>
    </StockChartAxisCrosshairTooltip>
</StockChartPrimaryYAxis>
```

## Combined Examples

### Complete Interactive Chart

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Interactive Stock Analysis - AAPL" 
              EnableSelector="true"
              EnablePeriodSelector="true">
    
    <!-- Zoom Configuration -->
    <StockChartZoomSettings EnableSelectionZooming="true"
                           EnableMouseWheelZooming="true"
                           EnablePinchZooming="true"
                           EnableScrollbar="true"
                           EnablePan="true"
                           ToolbarItems="@ZoomTools">
    </StockChartZoomSettings>
    
    <!-- Crosshair Configuration -->
    <StockChartCrosshairSettings Enable="true" SnapToData="true">
        <StockChartCrosshairLine Width="1" Color="#666666">
        </StockChartCrosshairLine>
    </StockChartCrosshairSettings>
    
    <!-- Axes with Crosshair Tooltips -->
    <StockChartPrimaryXAxis EnableAutoIntervalOnZooming="true">
        <StockChartAxisMajorGridLines Width="0"></StockChartAxisMajorGridLines>
        <StockChartAxisCrosshairTooltip Enable="true" Fill="#2196F3">
        </StockChartAxisCrosshairTooltip>
    </StockChartPrimaryXAxis>
    
    <StockChartPrimaryYAxis>
        <StockChartAxisLineStyle Width="0"></StockChartAxisLineStyle>
        <StockChartAxisMajorTickLines Width="0"></StockChartAxisMajorTickLines>
        <StockChartAxisCrosshairTooltip Enable="true" Fill="#4CAF50">
        </StockChartAxisCrosshairTooltip>
    </StockChartPrimaryYAxis>
    
    <!-- Series -->
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@StockData" Type="ChartSeriesType.Candle" 
                         XName="Date" High="High" Low="Low" Open="Open" Close="Close" Volume="Volume">
        </StockChartSeries>
    </StockChartSeriesCollection>
    
    <!-- Tooltip -->
    <StockChartTooltipSettings Enable="true" Header="" EnableMarker="false">
    </StockChartTooltipSettings>
</SfStockChart>

@code {
    public List<ToolbarItems> ZoomTools = new List<ToolbarItems>() 
    { 
        ToolbarItems.Zoom, 
        ToolbarItems.ZoomIn, 
        ToolbarItems.ZoomOut, 
        ToolbarItems.Pan, 
        ToolbarItems.Reset 
    };

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
        new StockChartData { Date = new DateTime(2024, 01, 01), Open = 175.5, High = 180.2, Low = 174.8, Close = 179.5, Volume = 12000000 },
        new StockChartData { Date = new DateTime(2024, 01, 08), Open = 179.5, High = 185.3, Low = 178.1, Close = 183.7, Volume = 15000000 },
        new StockChartData { Date = new DateTime(2024, 01, 15), Open = 183.7, High = 190.5, Low = 182.5, Close = 188.2, Volume = 18000000 }
    };
}
```

### Multi-Series Trackball

```cshtml
@using Syncfusion.Blazor.Charts

<SfStockChart Title="Multi-Asset Comparison">
    <StockChartCrosshairSettings Enable="true" LineType="LineType.Vertical">
        <StockChartCrosshairLine Width="1" Color="#999999">
        </StockChartCrosshairLine>
    </StockChartCrosshairSettings>
    
    <StockChartZoomSettings EnableMouseWheelZooming="true"
                           Mode="ZoomMode.X">
    </StockChartZoomSettings>
    
    <StockChartTooltipSettings Enable="true" Shared="true">
    </StockChartTooltipSettings>
    
    <StockChartSeriesCollection>
        <StockChartSeries DataSource="@AppleData" Type="ChartSeriesType.Line" 
                         XName="Date" YName="Price" Name="AAPL" Fill="#4CAF50">
        </StockChartSeries>
        <StockChartSeries DataSource="@GoogleData" Type="ChartSeriesType.Line" 
                         XName="Date" YName="Price" Name="GOOGL" Fill="#2196F3">
        </StockChartSeries>
        <StockChartSeries DataSource="@MicrosoftData" Type="ChartSeriesType.Line" 
                         XName="Date" YName="Price" Name="MSFT" Fill="#FF9800">
        </StockChartSeries>
    </StockChartSeriesCollection>
</SfStockChart>

@code {

    public class StockPrice
    {
        public DateTime Date { get; set; }
        public double Price { get; set; }
    }

    public List<StockPrice> AppleData = new()
    {
        new() { Date = new DateTime(2024, 01, 01), Price = 175.5 },
        new() { Date = new DateTime(2024, 01, 08), Price = 179.8 },
        new() { Date = new DateTime(2024, 01, 15), Price = 182.3 },
        new() { Date = new DateTime(2024, 01, 22), Price = 180.1 },
        new() { Date = new DateTime(2024, 01, 29), Price = 185.4 }
    };

    public List<StockPrice> GoogleData = new()
    {
        new() { Date = new DateTime(2024, 01, 01), Price = 138.2 },
        new() { Date = new DateTime(2024, 01, 08), Price = 140.9 },
        new() { Date = new DateTime(2024, 01, 15), Price = 142.6 },
        new() { Date = new DateTime(2024, 01, 22), Price = 141.3 },
        new() { Date = new DateTime(2024, 01, 29), Price = 144.8 }
    };

    public List<StockPrice> MicrosoftData = new()
    {
        new() { Date = new DateTime(2024, 01, 01), Price = 365.1 },
        new() { Date = new DateTime(2024, 01, 08), Price = 368.4 },
        new() { Date = new DateTime(2024, 01, 15), Price = 372.7 },
        new() { Date = new DateTime(2024, 01, 22), Price = 370.2 },
        new() { Date = new DateTime(2024, 01, 29), Price = 375.9 }
    };
}
```

## When to Use

### Use Zooming When:
- Analyzing specific time periods in detail
- Identifying exact entry/exit points
- Examining price patterns closely
- Working with large datasets
- Users need to focus on particular regions

### Use Panning When:
- Navigating through zoomed data
- Comparing different time periods at same zoom level
- Exploring large datasets systematically
- Users need to maintain context while moving

### Use Crosshair When:
- Identifying exact values at specific points
- Comparing values across multiple series
- Precise price/time identification needed
- Users need detailed cursor feedback
- Analyzing correlation between series

### Use All Three Together When:
- Building comprehensive analysis tools
- Supporting both overview and detail work
- Users need maximum interactivity
- Professional trading/analysis applications

## Common Issues and Troubleshooting

### Issue 1: Zoom Not Working

**Problem**: Zoom gestures have no effect.

**Solution**: Ensure zoom settings are properly enabled:

```cshtml
@using Syncfusion.Blazor.Charts

<StockChartZoomSettings EnableSelectionZooming="true"
                        EnableMouseWheelZooming="true"
                        EnablePinchZooming="true">
</StockChartZoomSettings>
```

### Issue 2: Pan Mode Not Activating

**Problem**: Cannot pan after zooming.

**Solution**: Check `EnablePan` is true:

```cshtml
<StockChartZoomSettings EnablePan="true">
</StockChartZoomSettings>
```

### Issue 3: Crosshair Not Visible

**Problem**: Crosshair doesn't appear on hover.

**Solution**: Verify crosshair is enabled and styled:

```cshtml
@using Syncfusion.Blazor.Charts

<StockChartCrosshairSettings Enable="true">
    <StockChartCrosshairLine Width="1" Color="#666666">
    </StockChartCrosshairLine>
</StockChartCrosshairSettings>
```

### Issue 4: Zoom Toolbar Missing

**Problem**: Expected toolbar doesn't appear after zooming.

**Solution**: Ensure you've performed a zoom action (toolbar only appears post-zoom):

```cshtml
@using Syncfusion.Blazor.Charts

<StockChartZoomSettings EnableSelectionZooming="true"
                        ToolbarItems="@ZoomTools">
</StockChartZoomSettings>
```

### Issue 5: Scrollbar Not Showing

**Problem**: Scrollbar doesn't appear when zoomed.

**Solution**: Verify `EnableScrollbar` is true and chart is actually zoomed:

```cshtml
@using Syncfusion.Blazor.Charts

<StockChartZoomSettings EnableScrollbar="true"
                        EnableMouseWheelZooming="true">
</StockChartZoomSettings>
```

### Issue 6: Crosshair Snapping Issues

**Problem**: Crosshair doesn't snap to data points correctly.

**Solution**: Adjust `SnapToData` based on your needs:

```cshtml
@using Syncfusion.Blazor.Charts

<!-- For discrete data points -->
<StockChartCrosshairSettings Enable="true" SnapToData="true">
</StockChartCrosshairSettings>

<!-- For continuous interpolation -->
<StockChartCrosshairSettings Enable="true" SnapToData="false">
</StockChartCrosshairSettings>
```

## Best Practices

1. **Enable Multiple Zoom Methods**
   - Support mouse wheel, selection, and pinch
   - Users have different preferences and devices
   - Increases accessibility

2. **Provide Zoom Reset**
   - Always include Reset in toolbar
   - Users need easy way back to full view
   - Prevents "lost in data" feeling

3. **Use Auto Interval on Zooming**
   - Prevents label overlap
   - Improves readability when zoomed
   - Essential for time-based axes

4. **Combine Crosshair with Tooltip**
   - Show precise values
   - Enhance data exploration
   - Improve user understanding

5. **Choose Appropriate Zoom Mode**
   - `X` for time series (most common)
   - `Y` for price range analysis
   - `XY` for general exploration

6. **Style Crosshair for Visibility**
   - Use contrasting colors
   - Consider dashed lines to reduce visual weight
   - Ensure width is visible but not obtrusive

7. **Enable Scrollbar for Large Datasets**
   - Provides overview while zoomed
   - Easier navigation
   - Shows current position context

8. **Test on Touch Devices**
   - Ensure pinch zoom works smoothly
   - Test pan gestures
   - Verify touch-friendly sizes for toolbar buttons

9. **Consider Performance**
   - Large datasets may slow zooming
   - Consider data virtualization
   - Test with realistic data volumes

10. **Provide Visual Feedback**
    - Clear indication of zoom level
    - Visible toolbar in accessible location
    - Smooth transitions

This comprehensive reference provides developers with complete guidance for implementing zooming, panning, and crosshair features in Syncfusion Blazor Stock Charts, enabling rich interactive data exploration experiences.
