# User Interaction and Events

## Table of Contents
- [Overview](#overview)
- [Tooltips](#tooltips)
  - [Basic Tooltip Configuration](#basic-tooltip-configuration)
  - [Tooltip Customization](#tooltip-customization)
  - [Tooltip Formatting](#tooltip-formatting)
  - [Tooltip Templates](#tooltip-templates)
- [Track Lines](#track-lines)
- [Component Events](#component-events)
    - [OnLoaded Event](#onloaded-event)
  - [OnPointRendering](#onpointrendering)
  - [OnSeriesRendering](#onseriesrendering)
  - [OnMarkerRendering](#onmarkerrendering)
  - [OnDataLabelRendering](#ondatalabelrendering)
  - [OnPointRegionMouseClick](#onpointregionmouseclick)
  - [OnResizing](#onresizing)
- [Component Methods](#component-methods)
  - [RefreshAsync](#refreshasync)
- [User Interaction Patterns](#user-interaction-patterns)
- [Best Practices](#best-practices)

## Overview

User interaction features enhance the Sparkline experience by providing detailed information on hover, responding to user actions, and enabling dynamic chart updates. Syncfusion Blazor Sparkline supports tooltips, track lines, various events, and methods for programmatic control.

**Key Interactive Features:**
- Tooltips for displaying data point details
- Track lines for visual point highlighting
- Comprehensive event system
- Methods for refresh and updates

## Tooltips

Tooltips display detailed information about data points when users hover over them. Use the `SparklineTooltipSettings` component with the `Visible` property to enable tooltips, and customize them using properties like `Fill`, `Format`, and child components `SparklineTooltipTextStyle`, `SparklineTooltipBorder`, and `SparklineTrackLineSettings`.

### Basic Tooltip Configuration

Enable tooltips by setting the `Visible` property to `true` in the `SparklineTooltipSettings` component. The `TValue` property must match the data source type.

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline Width="500" 
              Height="200" 
              TValue="WorkLog" 
              DataSource="@WorkLogs" 
              XName="Day" 
              YName="Hour" 
              Fill="blue" 
              ValueType="SparklineValueType.Category">
    <SparklineAxisSettings MinX="-1" MaxX="7" MaxY="8" MinY="-1">
    </SparklineAxisSettings>
    <SparklineTooltipSettings TValue="WorkLog" Visible="true">
    </SparklineTooltipSettings>
</SfSparkline>

@code {
    public class WorkLog
    {
        public string Day { get; set; }
        public double Hour { get; set; }
    }

    public List<WorkLog> WorkLogs = new List<WorkLog> 
    {
        new WorkLog { Day = "Mon", Hour = 3 },
        new WorkLog { Day = "Tue", Hour = 5 },
        new WorkLog { Day = "Wed", Hour = 2 },
        new WorkLog { Day = "Thu", Hour = 4 },
        new WorkLog { Day = "Fri", Hour = 6 }
    };
}
```

### Tooltip Customization

Customize tooltip appearance using properties such as `Fill` (background color), `Width`, `Height`, and child components `SparklineTooltipTextStyle` (for text formatting with `Color`, `Size`, `FontWeight`, `FontFamily`) and `SparklineTooltipBorder` (for border styling with `Color` and `Width`).

#### Fill and Text Style

```razor
<SfSparkline Width="500" 
              Height="200" 
              TValue="WorkLog" 
              DataSource="@WorkLogs" 
              XName="Day" 
              YName="Hour" 
              Fill="blue" 
              ValueType="SparklineValueType.Category">
    <SparklineAxisSettings MinX="-1" MaxX="7" MaxY="8" MinY="-1">
    </SparklineAxisSettings>
    <SparklineTooltipSettings TValue="WorkLog" 
                              Visible="true" 
                              Fill="lightgray">
        <SparklineTooltipTextStyle Color="darkblue">
        </SparklineTooltipTextStyle>
        <SparklineTooltipBorder Color="red" Width="1">
        </SparklineTooltipBorder>
    </SparklineTooltipSettings>
</SfSparkline>
```

#### Complete Styling Example

```razor
<SfSparkline DataSource="@SalesData" 
              TValue="SalesInfo"
              XName="Month"
              YName="Amount"
              ValueType="SparklineValueType.Category"
              Type="SparklineType.Column" 
              Height="180px" 
              Width="500px"
              Fill="#4CAF50">
    <SparklineTooltipSettings TValue="SalesInfo" 
                              Visible="true" 
                              Fill="#FFFFFF">
        <SparklineTooltipTextStyle Color="#333333" 
                                    Size="13px" 
                                    FontWeight="600" 
                                    FontFamily="Arial">
        </SparklineTooltipTextStyle>
        <SparklineTooltipBorder Color="#4CAF50" Width="2">
        </SparklineTooltipBorder>
    </SparklineTooltipSettings>
</SfSparkline>

@code {
    public class SalesInfo
    {
        public string Month { get; set; }
        public double Amount { get; set; }
    }

    public List<SalesInfo> SalesData = new List<SalesInfo>
    {
        new SalesInfo { Month = "Jan", Amount = 45000 },
        new SalesInfo { Month = "Feb", Amount = 52000 },
        new SalesInfo { Month = "Mar", Amount = 48000 },
        new SalesInfo { Month = "Apr", Amount = 58000 }
    };
}
```

### Tooltip Formatting

Use the `Format` property in `SparklineTooltipSettings` to customize tooltip content with placeholders (e.g., `${PropertyName}`) that reference data source fields. The format string supports multiple data properties and custom text combinations.

#### Basic Format String

```razor
<SfSparkline Width="500" 
              Height="200" 
              TValue="WorkLog" 
              DataSource="@WorkLogs" 
              XName="Day" 
              YName="Hour" 
              Fill="blue" 
              ValueType="SparklineValueType.Category">
    <SparklineAxisSettings MinX="-1" MaxX="7" MaxY="8" MinY="-1">
    </SparklineAxisSettings>
    <SparklineTooltipSettings TValue="WorkLog" 
                              Visible="true" 
                              Format="${Day} : ${Hour} hours">
    </SparklineTooltipSettings>
</SfSparkline>
```

#### Currency Formatting

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@RevenueData" 
              TValue="RevenueInfo"
              XName="Quarter"
              YName="Amount"
              ValueType="SparklineValueType.Category"
              Type="SparklineType.Area" 
              Height="150px" 
              Width="450px"
              Fill="#E8F5E9">
    <SparklineTooltipSettings TValue="RevenueInfo" 
                              Visible="true" 
                              Format="${Quarter} Revenue: ${Amount}">
    </SparklineTooltipSettings>
    <SparklineBorder Color="#4CAF50" Width="2">
    </SparklineBorder>
</SfSparkline>

@code {
    public class RevenueInfo
    {
        public string Quarter { get; set; }
        public double  Amount { get; set; }
    }

    public List<RevenueInfo> RevenueData = new()
    {
        new RevenueInfo { Quarter = "Q1", Amount = 250000 },
        new RevenueInfo { Quarter = "Q2", Amount = 320000 },
        new RevenueInfo { Quarter = "Q3", Amount = 290000 },
        new RevenueInfo { Quarter = "Q4", Amount = 380000 }
    };
}
```

#### Multiple Data Points

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@TemperatureData" 
              TValue="TempReading"
              XName="Hour"
              YName="Celsius"
              ValueType="SparklineValueType.Category"
              Type="SparklineType.Line" 
              Height="160px" 
              Width="500px"
              LineWidth="2"
              Fill="#FF5722">
    <SparklineTooltipSettings TValue="TempReading" 
                              Visible="true" 
                              Format="Time: ${Hour}:00<br/>Temperature: ${Celsius}°C<br/>Humidity: ${Humidity}%">
    </SparklineTooltipSettings>
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }"
                             Size="5">
    </SparklineMarkerSettings>
</SfSparkline>

@code {
    public class TempReading
    {
        public int Hour { get; set; }
        public double Celsius { get; set; }
        public int Humidity { get; set; }
    }

    public List<TempReading> TemperatureData = new List<TempReading>
    {
        new TempReading { Hour = 0, Celsius = 18.5, Humidity = 65 },
        new TempReading { Hour = 4, Celsius = 16.2, Humidity = 72 },
        new TempReading { Hour = 8, Celsius = 20.8, Humidity = 58 },
        new TempReading { Hour = 12, Celsius = 28.5, Humidity = 42 },
        new TempReading { Hour = 16, Celsius = 31.3, Humidity = 38 },
        new TempReading { Hour = 20, Celsius = 23.7, Humidity = 55 }
    };
}
```

### Tooltip Templates

Create custom tooltip layouts using the `Template` property within `SparklineTooltipSettings`. The template provides access to the data context through the `@context` directive, allowing full HTML and Razor markup customization for rich, interactive tooltip displays.

#### Custom HTML Template

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline Width="500" 
              Height="200" 
              TValue="WorkLog" 
              DataSource="@WorkLogs" 
              XName="Day" 
              YName="Hour" 
              Fill="blue" 
              ValueType="SparklineValueType.Category">
    <SparklineAxisSettings MinX="-1" MaxX="7" MaxY="8" MinY="-1">
    </SparklineAxisSettings>
    <SparklineTooltipSettings TValue="WorkLog" 
                              Visible="true" 
                              Fill="lightgray">
        <Template>
            @{
                <table style="width:100%; background-color: #ffffff; border-spacing: 0px; border-collapse:separate; border: 1px solid grey; border-radius:10px; padding-top: 5px; padding-bottom:5px">
                    <tr>
                        <td style="font-weight:bold; color:black; padding-left: 5px;padding-top: 2px;padding-bottom: 2px;">Worklog</td>
                    </tr>
                    <tr>
                        <td style="padding-left: 5px; color:black; padding-right: 5px; padding-bottom: 2px;">Day : @context.Day  </td>
                    </tr>
                    <tr>
                        <td style="padding-left: 5px; color:black; padding-right: 5px">Hour : @context.Hour hrs </td>
                    </tr>
                </table>
            }
        </Template>
    </SparklineTooltipSettings>
</SfSparkline>
```

#### Rich Content Template

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@PerformanceData" 
              TValue="PerformanceMetric"
              XName="Category"
              YName="Score"
              ValueType="SparklineValueType.Category"
              Type="SparklineType.Column" 
              Height="180px" 
              Width="500px"
              Fill="#2196F3">
    <SparklineTooltipSettings TValue="PerformanceMetric" Visible="true">
        <Template>
            @{
                var data = context as PerformanceMetric;
                var statusColor = data.Score >= 80 ? "#4CAF50" : data.Score >= 60 ? "#FFC107" : "#F44336";
                var statusText = data.Score >= 80 ? "Excellent" : data.Score >= 60 ? "Good" : "Needs Improvement";
                
                <div style="background: white; padding: 12px; border-radius: 8px; box-shadow: 0 2px 8px rgba(0,0,0,0.15); min-width: 180px;">
                    <div style="font-weight: bold; color: #333; margin-bottom: 8px; font-size: 14px;">
                        @data.Category
                    </div>
                    <div style="display: flex; justify-content: space-between; align-items: center; margin-bottom: 4px;">
                        <span style="color: #666; font-size: 12px;">Score:</span>
                        <span style="font-weight: bold; color: @statusColor; font-size: 16px;">@data.Score</span>
                    </div>
                    <div style="display: flex; justify-content: space-between; align-items: center;">
                        <span style="color: #666; font-size: 12px;">Status:</span>
                        <span style="color: @statusColor; font-weight: 600; font-size: 12px;">@statusText</span>
                    </div>
                </div>
            }
        </Template>
    </SparklineTooltipSettings>
</SfSparkline>

@code {
    public class PerformanceMetric
    {
        public string Category { get; set; }
        public int Score { get; set; }
    }

    public List<PerformanceMetric> PerformanceData = new List<PerformanceMetric>
    {
        new PerformanceMetric { Category = "Quality", Score = 92 },
        new PerformanceMetric { Category = "Speed", Score = 78 },
        new PerformanceMetric { Category = "Reliability", Score = 88 },
        new PerformanceMetric { Category = "Efficiency", Score = 85 }
    };
}
```

## Track Lines

Track lines provide visual feedback by highlighting the data point closest to the cursor. Use the `SparklineTrackLineSettings` component (nested within `SparklineTooltipSettings`) with properties like `Visible` (boolean), `Color` (string), and `Width` (numeric) to customize the track line appearance.

### Basic Track Line

Enable track lines by setting the `Visible` property of `SparklineTrackLineSettings` to `true`. The `Color` and `Width` properties control the visual styling of the track line indicator.

```razor
<SfSparkline Width="500px" 
              Height="200px"
              DataSource="new int[]{ 5, 3, 4, 6, 8, 7, 9, 1, 3, 5, 3, 4, 6, 8, 7, 9, 1, 3, 5, 2, 4, 6, 7, 9, 5, 8, 3, 6, 1, 7, 4, 2, 5, 2, 4, 6, 7, 9, 5, 8, 3, 6, 1, 7, 4, 2 }">
    <SparklineAxisSettings MinX="-1" MaxX="46" MaxY="10" MinY="-1">
    </SparklineAxisSettings>
    <SparklineTooltipSettings TValue="int" Visible="true">
        <SparklineTrackLineSettings Visible="true">
        </SparklineTrackLineSettings>
    </SparklineTooltipSettings>
</SfSparkline>
```

### Customized Track Line

Customize track lines using the `Color` and `Width` properties in `SparklineTrackLineSettings`, and combine with `SparklineMarkerSettings` (with `Visible` and `Size` properties) to enhance visual feedback when hovering over data points.

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@StockPrices" 
              Type="SparklineType.Area" 
              Height="180px" 
              Width="500px"
              Fill="#E3F2FD"
              LineWidth="2">
    <SparklineBorder Color="#2196F3" Width="2">
    </SparklineBorder>
    <SparklineTooltipSettings TValue="double" 
                              Visible="true" 
                              >
        <SparklineTrackLineSettings Visible="true" 
                                     Color="#1976D2" 
                                     Width="2">
        </SparklineTrackLineSettings>
    </SparklineTooltipSettings>
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }"
                             Size="4">
    </SparklineMarkerSettings>
</SfSparkline>

@code {
    public double[] StockPrices = new double[] 
    { 
        172.50, 174.20, 173.80, 175.90, 174.50, 176.30, 
        177.10, 175.80, 178.45, 177.20, 178.10, 179.30 
    };
}
```

### Track Line with Enhanced Tooltip

Combine `SparklineTrackLineSettings` (with `Visible`, `Color`, `Width` properties) with `SparklineTooltipSettings` (with `Format`, `Fill`, and text styling) to create an integrated visual experience that highlights and provides information about hovered data points.

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@SensorData" 
              TValue="SensorReading"
              XName="Time"
              YName="Value"
              ValueType="SparklineValueType.Category"
              Type="SparklineType.Line" 
              Height="160px" 
              Width="550px"
              LineWidth="2"
              Fill="#FF5722">
    <SparklineTooltipSettings TValue="SensorReading" 
                              Visible="true" 
                              Format="Time: ${Time} - Value: ${Value} "
                              Fill="#FFFFFF">
        <SparklineTrackLineSettings Visible="true" 
                                     Color="#D32F2F" 
                                     Width="1">
        </SparklineTrackLineSettings>
        <SparklineTooltipTextStyle Color="#333" 
                                    Size="12px" 
                                    FontWeight="500">
        </SparklineTooltipTextStyle>
        <SparklineTooltipBorder Color="#FF5722" Width="2">
        </SparklineTooltipBorder>
    </SparklineTooltipSettings>
</SfSparkline>

@code {
    public class SensorReading
    {
        public string Time { get; set; }
        public int Value { get; set; }
        public string Status { get; set; }
    }

    public List<SensorReading> SensorData = new List<SensorReading>
    {
        new SensorReading { Time = "00:00", Value = 45, Status = "Normal" },
        new SensorReading { Time = "04:00", Value = 52, Status = "Normal" },
        new SensorReading { Time = "08:00", Value = 48, Status = "Normal" },
        new SensorReading { Time = "12:00", Value = 89, Status = "Alert" },
        new SensorReading { Time = "16:00", Value = 55, Status = "Normal" },
        new SensorReading { Time = "20:00", Value = 62, Status = "Normal" }
    };
}
```

## Component Events

Component events enable programmatic responses to user interactions and lifecycle events. Events are registered using the `SparklineEvents` component with event handlers like `OnLoaded`, `OnPointRendering`, `OnSeriesRendering`, `OnMarkerRendering`, `OnDataLabelRendering`, `OnPointRegionMouseClick`, and `OnResizing`.

### OnLoaded Event

The `OnLoaded` event triggers after the Sparkline component has been fully loaded and rendered. Register the event handler using the `OnLoaded` property in the `SparklineEvents` component.

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="new int[]{ 0, 6, 4, 1, 3, 2, 5 }" 
              Type="SparklineType.Line" 
              Height="200px" 
              Width="450px">
    <SparklineEvents OnLoaded="@LoadedHandler">
    </SparklineEvents>
</SfSparkline>

@code {
    public void LoadedHandler(System.EventArgs args)
    {
        // Chart is loaded - perform initialization tasks
        Console.WriteLine("Sparkline loaded successfully");
    }
}
```

### OnPointRendering

The `OnPointRendering` event triggers before each data point is rendered, allowing point-level customization. Use the `OnPointRendering` property in `SparklineEvents` to access `SparklinePointEventArgs` which provides `PointIndex`, `Fill`, and other point properties for customization.

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="new int[]{ 6, 4, 1, 3, 2, 5 }" 
              Type="SparklineType.Column" 
              Height="200px" 
              Width="450px">
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }">
    </SparklineMarkerSettings>
    <SparklineEvents OnPointRendering="@PointRenderHandler">
    </SparklineEvents>
</SfSparkline>

@code {
    public void PointRenderHandler(SparklinePointEventArgs args)
    {
        // Customize point appearance based on value
        if (args.PointIndex == 2)
        {
            args.Fill = "#F44336"; // Highlight specific point
        }
    }
}
```

#### Dynamic Point Coloring

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@ScoreData" 
              Type="SparklineType.Column" 
              Height="180px" 
              Width="500px">
    <SparklineEvents OnPointRendering="@ColorByValue">
    </SparklineEvents>
</SfSparkline>

@code {
    public int[] ScoreData = new int[] { 45, 68, 52, 88, 75, 92, 65, 58, 78, 85 };

    public void ColorByValue(SparklinePointEventArgs args)
    {
        var dataValue = ScoreData[args.PointIndex];
        
        if (dataValue >= 80)
        {
            args.Fill = "#4CAF50"; // Green for high scores
        }
        else if (dataValue >= 60)
        {
            args.Fill = "#FFC107"; // Yellow for medium scores
        }
        else
        {
            args.Fill = "#F44336"; // Red for low scores
        }
    }
}
```

### OnSeriesRendering

The `OnSeriesRendering` event triggers before the series is rendered, enabling series-level customization. Register using the `OnSeriesRendering` property in `SparklineEvents` to access `SeriesRenderingEventArgs` with properties like `Fill` and `LineWidth` for styling the entire series.

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="new int[]{ 0, 6, 4, 1, 3, 2, 5 }" 
              Type="SparklineType.Line" 
              Height="200px" 
              Width="450px">
    <SparklineEvents OnSeriesRendering="@SeriesRenderingHandler">
    </SparklineEvents>
</SfSparkline>

@code {
    public void SeriesRenderingHandler(SeriesRenderingEventArgs args)
    {
        // Customize series appearance
        args.Fill = "#2196F3";
        args.LineWidth = 3;
    }
}
```

### OnMarkerRendering

The `OnMarkerRendering` event triggers before each marker is rendered on the chart. Use the `OnMarkerRendering` property in `SparklineEvents` to access `MarkerRenderingEventArgs` with properties like `PointIndex`, `Fill`, and `Size` to customize individual markers based on their position or value.

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="new int[]{ 6, 4, 1, 3, 2, 5 }" 
              Type="SparklineType.Line" 
              Height="200px" 
              Width="450px">
    <SparklineMarkerSettings Visible="new List<VisibleType> { VisibleType.All }">
    </SparklineMarkerSettings>
    <SparklineEvents OnMarkerRendering="@MarkerRenderingHandler">
    </SparklineEvents>
</SfSparkline>

@code {
    public void MarkerRenderingHandler(MarkerRenderingEventArgs args)
    {
        // Customize marker appearance
        if (args.PointIndex % 2 == 0)
        {
            args.Fill = "#4CAF50";
            args.Size = 8;
        }
        else
        {
            args.Fill = "#F44336";
            args.Size = 6;
        }
    }
}
```

### OnDataLabelRendering

The `OnDataLabelRendering` event triggers before each data label is rendered on the chart. Register using the `OnDataLabelRendering` property in `SparklineEvents` to access `DataLabelRenderingEventArgs` with properties like `PointIndex`, `Text`, and `Color` for customizing label content and appearance.

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="new int[]{ 6, 4, 1, 3, 2, 5 }" 
              Type="SparklineType.Line" 
              Height="200px" 
              Width="450px">
     <SparklinePadding Left="60" Right="20" Top="20" Bottom="20"></SparklinePadding>
    <SparklineDataLabelSettings Visible="new List<VisibleType> { VisibleType.All }">
    </SparklineDataLabelSettings>
    <SparklineEvents OnDataLabelRendering="@DataLabelRenderingHandler">
    </SparklineEvents>
    <SparklineAxisSettings MinX="-1" MaxX="7" MaxY="7" MinY="-1">
    </SparklineAxisSettings>
</SfSparkline>

@code {
    public void DataLabelRenderingHandler(DataLabelRenderingEventArgs args)
    {
        // Customize data label
        if (args.PointIndex == 0)
        {
            args.Text = "Start: " + args.Text;
            args.Color = "#4CAF50";
        }
    }
}
```

### OnPointRegionMouseClick

The `OnPointRegionMouseClick` event triggers when a user clicks on a data point region. Register using the `OnPointRegionMouseClick` property in `SparklineEvents` to access `PointRegionEventArgs` with properties like `PointIndex` to handle click interactions and implement drill-down or selection functionality.

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline DataSource="@ClickableData" 
              Type="SparklineType.Column" 
              Height="200px" 
              Width="450px">
    <SparklineEvents OnPointRegionMouseClick="@PointClickHandler">
    </SparklineEvents>
</SfSparkline>

<div style="margin-top: 20px;">
    <strong>Selected Point:</strong> @SelectedInfo
</div>

@code {
    public int[] ClickableData = new int[] { 45, 68, 52, 88, 75, 92, 65 };
    public string SelectedInfo = "None";

    public void PointClickHandler(PointRegionEventArgs args)
    {
        var value = ClickableData[args.PointIndex];
        SelectedInfo = $"Index: {args.PointIndex}, Value: {value}";
    }
}
```

#### Interactive Selection

```razor
@using Syncfusion.Blazor.Charts

<SfSparkline @ref="@SparklineRef"
              DataSource="@InteractiveData" 
              TValue="DataPoint"
              XName="Label"
              YName="Value"
              ValueType="SparklineValueType.Category"
              Type="SparklineType.Column" 
              Height="180px" 
              Width="500px">
    <SparklineEvents OnPointRegionMouseClick="@HandlePointClick"
                     OnPointRendering="@HighlightSelected">
    </SparklineEvents>
</SfSparkline>

<div style="margin-top: 15px; padding: 15px; background: #f5f5f5; border-radius: 8px;">
    <h5>Selected: @SelectedLabel</h5>
    <p>Value: @SelectedValue</p>
</div>

@code {
    public SfSparkline<DataPoint> SparklineRef;
    public int SelectedIndex = -1;
    public string SelectedLabel = "None";
    public int SelectedValue = 0;

    public class DataPoint
    {
        public string Label { get; set; }
        public int Value { get; set; }
    }

    public List<DataPoint> InteractiveData = new List<DataPoint>
    {
        new DataPoint { Label = "A", Value = 45 },
        new DataPoint { Label = "B", Value = 68 },
        new DataPoint { Label = "C", Value = 52 },
        new DataPoint { Label = "D", Value = 88 },
        new DataPoint { Label = "E", Value = 75 }
    };

    public async Task HandlePointClick(PointRegionEventArgs args)
    {
        SelectedIndex = args.PointIndex;
        SelectedLabel = InteractiveData[args.PointIndex].Label;
        SelectedValue = InteractiveData[args.PointIndex].Value;
        
        // Refresh to apply highlighting
        await SparklineRef.RefreshAsync();
    }

    public void HighlightSelected(SparklinePointEventArgs args)
    {
        if (args.PointIndex == SelectedIndex)
        {
            args.Fill = "#F44336";
        }
        else
        {
            args.Fill = "#2196F3";
        }
    }
}
```

### OnResizing

The `OnResizing` event triggers when the browser window or container is resized. Register using the `OnResizing` property in `SparklineEvents` to access `SparklineResizeEventArgs` with properties like `PreviousSize` and `CurrentSize` to respond to layout changes and adjust the chart accordingly.

```razor
@using Syncfusion.Blazor.Charts
@using System.Drawing

<SfSparkline DataSource="new int[]{ 0, 6, 4, 1, 3, 2, 5 }" 
              Type="SparklineType.Line" 
              Height="200px" 
              Width="100%">
    <SparklineEvents OnResizing="@ResizeHandler">
    </SparklineEvents>
</SfSparkline>

@code {
    public struct PointF
    {
        public float X { get; set; }
        public float Y { get; set; }
    }
    public void ResizeHandler(SparklineResizeEventArgs args)
    {
        Console.WriteLine($"Resized from {args.PreviousSize.X}x{args.PreviousSize.Y} " +
                         $"to {args.CurrentSize.X}x{args.CurrentSize.Y}");
    }
}
```

## Component Methods

Component methods enable programmatic control of the Sparkline component. Methods are called on the component reference (obtained using `@ref`) to perform actions like refreshing and updating the chart display.

### RefreshAsync

The `RefreshAsync` method re-renders the Sparkline component programmatically. Call this method on the Sparkline component reference (e.g., `SparklineRef.RefreshAsync()`) after updating data or properties to reflect changes in the chart display. This is an asynchronous method that returns a `Task`.

```razor
@using Syncfusion.Blazor.Charts

<button @onclick="RefreshChart" class="btn btn-primary">Refresh Chart</button>

<SfSparkline @ref="@SparklineRef" 
              DataSource="@DynamicData" 
              Type="SparklineType.Line" 
              Height="180px" 
              Width="450px"
              LineWidth="2">
</SfSparkline>

@code {
    public SfSparkline<int> SparklineRef;
    public int[] DynamicData = new int[] { 3, 6, 4, 8, 5, 9, 7 };

    public async Task RefreshChart()
    {
        // Update data
        var random = new Random();
        DynamicData = Enumerable.Range(0, 10)
                                .Select(_ => random.Next(1, 100))
                                .ToArray();
        
        // Refresh the chart
        await SparklineRef.RefreshAsync();
    }
}
```

#### Auto-Refresh Example

```razor
@using Syncfusion.Blazor.Charts

<div class="live-data-panel">
    <h4>Live Server Metrics</h4>
    <div class="control-panel">
        <button @onclick="ToggleAutoRefresh" class="btn btn-sm">
            @(IsAutoRefreshing ? "Stop" : "Start") Auto-Refresh
        </button>
        <span class="status">Last Update: @LastUpdate.ToString("HH:mm:ss")</span>
    </div>
    
    <SfSparkline @ref="@LiveSparkline" 
                  DataSource="@LiveData" 
                  Type="SparklineType.Area" 
                  Height="150px" 
                  Width="500px"
                  Fill="#E8F5E9"
                  LineWidth="2">
        <SparklineBorder Color="#4CAF50" Width="2">
        </SparklineBorder>
        <SparklineTooltipSettings TValue="int" 
                                  Visible="true" 
                                  Format="Value: ${y}">
        </SparklineTooltipSettings>
    </SfSparkline>
</div>

@code {
    public SfSparkline<int> LiveSparkline;
    public List<int> LiveData = new List<int> { 45, 52, 48, 55, 51, 58, 54 };
    public bool IsAutoRefreshing = false;
    public DateTime LastUpdate = DateTime.Now;
    private System.Threading.Timer RefreshTimer;

    public void ToggleAutoRefresh()
    {
        IsAutoRefreshing = !IsAutoRefreshing;
        
        if (IsAutoRefreshing)
        {
            RefreshTimer = new System.Threading.Timer(async _ =>
            {
                await InvokeAsync(async () =>
                {
                    // Simulate new data
                    var random = new Random();
                    LiveData.RemoveAt(0);
                    LiveData.Add(random.Next(40, 70));
                    LastUpdate = DateTime.Now;
                    
                    await LiveSparkline.RefreshAsync();
                    StateHasChanged();
                });
            }, null, TimeSpan.Zero, TimeSpan.FromSeconds(2));
        }
        else
        {
            RefreshTimer?.Dispose();
        }
    }

    public void Dispose()
    {
        RefreshTimer?.Dispose();
    }
}
```

## User Interaction Patterns

User interaction patterns demonstrate how to combine multiple interactive features like tooltips, events, and templates to create engaging user experiences. These patterns integrate `SparklineTooltipSettings`, `SparklineTrackLineSettings`, `SparklineEvents`, and component references for comprehensive interactivity.

### Dashboard with Interactive Tooltips

```razor
@using Syncfusion.Blazor.Charts

<div class="dashboard-metrics">
    <div class="metric-card">
        <h5>Daily Visitors</h5>
        <div class="metric-value">12,458</div>
        <SfSparkline DataSource="@VisitorData" 
                      Type="SparklineType.Area" 
                      Height="80px" 
                      Width="100%"
                      Fill="#E3F2FD">
            <SparklineBorder Color="#2196F3" Width="2">
            </SparklineBorder>
            <SparklineTooltipSettings TValue="int" 
                                      Visible="true" 
                                      >
                <SparklineTrackLineSettings Visible="true" 
                                             Color="#1976D2" 
                                             Width="1">
                </SparklineTrackLineSettings>
            </SparklineTooltipSettings>
        </SfSparkline>
    </div>
</div>

@code {
    public int[] VisitorData = new int[] 
    { 
        11200, 11800, 12100, 11950, 12300, 12150, 12458 
    };
}
```

### Drill-Down Interaction

Implement drill-down interactions by combining `OnPointRegionMouseClick` event with component state management. Use properties like `PointIndex` from `PointRegionEventArgs` and component references to dynamically update the `DataSource` and refresh the chart using `RefreshAsync()` to display hierarchical data views.

```razor
@using Syncfusion.Blazor.Charts

<div class="drill-down-chart">
    <h4>@CurrentView Revenue</h4>
    <button @onclick="GoBack" disabled="@(CurrentLevel == 0)" class="btn btn-sm">
        ← Back
    </button>
    
    <SfSparkline DataSource="@CurrentData" 
                  TValue="RevenuePoint"
                  XName="Label"
                  YName="Amount"
                  ValueType="SparklineValueType.Category"
                  Type="SparklineType.Column" 
                  Height="180px" 
                  Width="500px"
                  Fill="#4CAF50">
        <SparklineEvents OnPointRegionMouseClick="@DrillDown">
        </SparklineEvents>
        <SparklineTooltipSettings TValue="RevenuePoint" 
                                  Visible="true" 
                                  Format="${Label}: ${Amount}">
        </SparklineTooltipSettings>
    </SfSparkline>
</div>

@code {
    public int CurrentLevel = 0;
    public string CurrentView = "Quarterly";
    
    public class RevenuePoint
    {
        public string Label { get; set; }
        public double Amount { get; set; }
    }

    public List<RevenuePoint> QuarterlyData = new List<RevenuePoint>
    {
        new RevenuePoint { Label = "Q1", Amount = 250000 },
        new RevenuePoint { Label = "Q2", Amount = 320000 },
        new RevenuePoint { Label = "Q3", Amount = 290000 },
        new RevenuePoint { Label = "Q4", Amount = 380000 }
    };

    public List<RevenuePoint> MonthlyData = new List<RevenuePoint>
    {
        new RevenuePoint { Label = "Jan", Amount = 85000 },
        new RevenuePoint { Label = "Feb", Amount = 78000 },
        new RevenuePoint { Label = "Mar", Amount = 87000 }
    };

    public List<RevenuePoint> CurrentData => CurrentLevel == 0 ? QuarterlyData : MonthlyData;

    public void DrillDown(PointRegionEventArgs args)
    {
        if (CurrentLevel == 0)
        {
            CurrentLevel = 1;
            CurrentView = "Monthly (Q1)";
        }
    }

    public void GoBack()
    {
        CurrentLevel = 0;
        CurrentView = "Quarterly";
    }
}
```

## Best Practices

Best practices guide the effective use of interactive features with properties like `Visible`, `Format`, `Fill`, event handlers, and component references to ensure optimal user experience, performance, and accessibility.

### Tooltip Design

**Do:**
- Keep tooltip content concise and relevant
- Use consistent formatting across charts
- Include units (%, $, etc.)
- Enable track lines for better precision

**Don't:**
- Overload tooltips with too much information
- Use small font sizes (< 11px)
- Forget to enable tooltips on interactive charts

### Event Handling

Event handlers registered in `SparklineEvents` should be optimized for performance. Use the event handler properties (`OnPointRegionMouseClick`, `OnPointRendering`, etc.) with efficient, non-blocking operations to ensure responsive user interactions.

**Performance:**
```razor
@using Syncfusion.Blazor.Charts

<!-- Good: Efficient event handling -->
<SparklineEvents OnPointRegionMouseClick="@HandleClick">
</SparklineEvents>

@code {
    // Quick, non-blocking operation
    public void HandleClick(PointRegionEventArgs args)
    {
        SelectedIndex = args.PointIndex;
    }
}

<!-- Avoid: Heavy operations in event handlers -->
@code {
    public async Task HandleClick(PointRegionEventArgs args)
    {
        // Avoid: Heavy database calls or API requests
        // await LongRunningOperation();
    }
}
```

### Accessibility

- Provide alternative text descriptions for charts
- Ensure tooltips are keyboard-accessible
- Use ARIA labels where appropriate
- Test with screen readers

### Mobile Considerations

Optimize interactive features for mobile devices by adjusting `Height`, `Width` properties, and tooltip `FontSize` in `SparklineTooltipTextStyle`. Ensure touch-friendly sizing and consider touch event limitations when designing `OnPointRegionMouseClick` handlers.

```razor
@using Syncfusion.Blazor.Charts

<!-- Touch-friendly sizing -->
<SfSparkline DataSource="@Data" 
              Height="120px"  
              Width="100%">
    <SparklineTooltipSettings TValue="int" Visible="true">
        <SparklineTooltipTextStyle FontSize="14px">
        </SparklineTooltipTextStyle>
    </SparklineTooltipSettings>
</SfSparkline>
@code {}
```

**User interaction features transform sparklines from static visualizations into dynamic, informative tools that engage users and provide actionable insights through tooltips, events, and programmatic control.**
