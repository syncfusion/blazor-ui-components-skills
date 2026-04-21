# Syncfusion Blazor 3D Chart Events

This document provides comprehensive coverage of all events available in the Syncfusion Blazor 3D Chart component.

## Overview

The `SfChart3D` component provides a rich set of events that allow you to customize behavior, intercept rendering, and respond to user interactions. All events can be bound using the `On[EventName]` pattern in Razor markup.

## Chart Lifecycle Events

### OnCreated

Fired after the 3D chart component is fully created and rendered. The `Created` property will be used to bind this event in the `SfChart3D` component.

**Event Type**: `EventCallback<Chart3DCreatedEventArgs>`

**Event Args Properties**:
- `Chart` - Reference to the chart instance

**Usage**:
```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Chart Created Demo"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="6"
           TiltAngle="8"
           Depth="100"
           Created="@ChartCreated">

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"
                         Title="Month">
    </Chart3DPrimaryXAxis>

    <Chart3DPrimaryYAxis Title="Sales">
    </Chart3DPrimaryYAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData"
                       XName="Month"
                       YName="Sales"
                       Type="Chart3DSeriesType.Column"
                       Fill="#4C9AFF">
        </Chart3DSeries>
    </Chart3DSeriesCollection>

</SfChart3D>

@code {

    // ✅ Basic Model
    public class SalesInfo
    {
        public string Month { get; set; }
        public double Sales { get; set; }
    }

    // ✅ Sample Data Source
    public List<SalesInfo> SalesData = new()
    {
        new SalesInfo { Month = "Jan", Sales = 10000 },
        new SalesInfo { Month = "Feb", Sales = 12000 },
        new SalesInfo { Month = "Mar", Sales = 14000 },
        new SalesInfo { Month = "Apr", Sales = 16000 },
        new SalesInfo { Month = "May", Sales = 18000 }
    };

    // ✅ OnCreated Event: This fires when chart is fully initialized
    private void ChartCreated(Chart3DCreatedEventArgs args)
    {
        Console.WriteLine("Chart created successfully!");
    }
}
```

**Common Use Cases**:
- Initialize chart references
- Perform post-render operations
- Set up external integrations

---

### OnResized

Fired when the chart is resized (browser window resize or container size change). The `OnResized` property will be used to bind this event in the `SfChart3D` component.

**Event Type**: `EventCallback<Chart3DResizedEventArgs>`

**Event Args Properties**:
- `CurrentSize` - Current chart dimensions (width, height)
- `PreviousSize` - Previous chart dimensions

**Usage**:
```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D OnResized="@ChartResized">
    <!-- Chart configuration -->
</SfChart3D>

@code {
    private void ChartResized(Chart3DResizedEventArgs args)
    {
        Console.WriteLine($"Chart resized from {args.PreviousSize} to {args.CurrentSize}");
    }
}
```

**Common Use Cases**:
- Adjust chart layout on resize
- Responsive design handling
- Recalculate dimensions

---

## Rendering Events

### OnSeriesRender

Fired before each series is rendered, allowing customization of series appearance. The `SeriesRendering` property will be used to bind this event in the `SfChart3D` component.

**Event Type**: `EventCallback<Chart3DSeriesRenderEventArgs>`

**Event Args Properties**:
- `Series` - Series being rendered
- `Data` - Series data source
- `Fill` - Series fill color (can be modified)
- `Cancel` - Set to true to cancel rendering

**Usage**:
```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D SeriesRendering ="@SeriesRender">
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>
    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@Data" XName="X" YName="Y1" Name="Series 1"></Chart3DSeries>
        <Chart3DSeries DataSource="@Data" XName="X" YName="Y2" Name="Series 2"></Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

@code {

    public class ChartPoint
    {
        public string X { get; set; }
        public double Y1 { get; set; }
        public double Y2 { get; set; }
    }

    public List<ChartPoint> Data = new List<ChartPoint>
    {
        new ChartPoint { X = "Jan", Y1 = 45, Y2 = 30 },
        new ChartPoint { X = "Feb", Y1 = 50, Y2 = 35 },
        new ChartPoint { X = "Mar", Y1 = 55, Y2 = 40 },
        new ChartPoint { X = "Apr", Y1 = 52, Y2 = 38 },
        new ChartPoint { X = "May", Y1 = 60, Y2 = 45 },
        new ChartPoint { X = "Jun", Y1 = 58, Y2 = 42 }
    };

    private void SeriesRender(Chart3DSeriesRenderEventArgs args)
    {
        // Customize series color dynamically
        if (args.Series.Name == "Series 1")
        {
            args.Fill = "#FF5733";
        }
        else
        {
            args.Fill = "#33C4FF";
        }
    }
}
```

**Common Use Cases**:
- Dynamic color assignment
- Conditional series rendering
- Series-specific styling
- Data validation before rendering

---

### OnPointRender

Fired before each data point is rendered, allowing point-level customization. The `PointRendering` property will be used to bind this event in the `SfChart3D` component.

**Event Type**: `EventCallback<Chart3DPointRenderEventArgs>`

**Event Args Properties**:
- `Point` - Data point being rendered
- `Series` - Parent series
- `Fill` - Point fill color (can be modified)
- `Border` - Point border settings (can be modified)
- `Cancel` - Set to true to cancel rendering

**Usage**:
```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales Performance"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="7"
           TiltAngle="10"
           Depth="100"
           PointRendering="@PointRender">

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"
                         Title="Month">
    </Chart3DPrimaryXAxis>

    <Chart3DPrimaryYAxis Title="Sales">
    </Chart3DPrimaryYAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData"
                       XName="Month"
                       YName="Sales"
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>
    </Chart3DSeriesCollection>

</SfChart3D>

@code {
    public class SalesInfo
    {
        public string Month { get; set; }
        public double Sales { get; set; }
    }

    // Dataset designed to trigger all color conditions
    public List<SalesInfo> SalesData = new List<SalesInfo>
    {
        new SalesInfo { Month = "Jan", Sales = 22 },   // Red
        new SalesInfo { Month = "Feb", Sales = 35 },   // Orange
        new SalesInfo { Month = "Mar", Sales = 48 },   // Orange
        new SalesInfo { Month = "Apr", Sales = 55 },   // Green
        new SalesInfo { Month = "May", Sales = 62 },   // Green
        new SalesInfo { Month = "Jun", Sales = 28 },   // Red
        new SalesInfo { Month = "Jul", Sales = 70 }    // Green
    };

    private void PointRender(Chart3DPointRenderEventArgs args)
    {
        // Color points based on value
        double value = Convert.ToDouble(args.Point.YValue);

        if (value > 50)
        {
            args.Fill = "green";      // High performance
        }
        else if (value > 30)
        {
            args.Fill = "orange";     // Medium performance
        }
        else
        {
            args.Fill = "red";        // Low performance
        }
    }
}
```

**Common Use Cases**:
- Conditional coloring based on values
- Highlighting specific data points
- Custom borders for outliers
- Data-driven styling

---

### OnAxisLabelRender

Fired before each axis label is rendered, allowing label customization. The `AxisLabelRendering` property will be used to bind this event in the `SfChart3D` component.

**Event Type**: `EventCallback<Chart3DAxisLabelRenderEventArgs>`

**Event Args Properties**:
- `Text` - Label text (can be modified)
- `Value` - Numeric value of the label
- `LabelStyle` - Label style settings (can be modified)
- `Axis` - Parent axis
- `Cancel` - Set to true to skip this label

**Usage**:
```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Revenue Chart"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="7"
           TiltAngle="10"
           Depth="100"
           AxisLabelRendering ="@AxisLabelRender">

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"
                         Name="PrimaryXAxis"
                         Title="Month">
    </Chart3DPrimaryXAxis>

    <Chart3DPrimaryYAxis Name="PrimaryYAxis"
                         Title="Revenue">
    </Chart3DPrimaryYAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@Data"
                       XName="X"
                       YName="Y"
                       Type="Chart3DSeriesType.Column"
                       Fill="#4C9AFF">
        </Chart3DSeries>
    </Chart3DSeriesCollection>

</SfChart3D>

@code {
    public class ChartPoint
    {
        public string X { get; set; }
        public double Y { get; set; }
    }

    // Dataset includes thousands and millions to demonstrate K/M formatting
    public List<ChartPoint> Data = new List<ChartPoint>
    {
        new ChartPoint { X = "Jan", Y = 850 },         // $850
        new ChartPoint { X = "Feb", Y = 12500 },       // $12.5K
        new ChartPoint { X = "Mar", Y = 98000 },       // $98K
        new ChartPoint { X = "Apr", Y = 250000 },      // $250K
        new ChartPoint { X = "May", Y = 1200000 },     // $1.2M
        new ChartPoint { X = "Jun", Y = 2750000 },     // $2.75M
        new ChartPoint { X = "Jul", Y = 350000 }       // $350K
    };

    private void AxisLabelRender(Chart3DAxisLabelRenderEventArgs args)
    {
        // Add currency symbol for Y-axis only
        if (args.Axis.Name == "PrimaryYAxis")
        {
            args.Text = "$" + args.Text;
        }

        // Abbreviate numeric labels
        double value = Convert.ToDouble(args.Value);

        if (value >= 1_000_000)
        {
            args.Text = "$" + (value / 1_000_000).ToString("0.#") + "M";
        }
        else if (value >= 1_000)
        {
            args.Text = "$" + (value / 1_000).ToString("0.#") + "K";
        }
    }
}
```

**Common Use Cases**:
- Custom label formatting
- Adding units or prefixes
- Abbreviating large numbers
- Localization of labels
- Conditional label styling

---

### OnLegendRender

Fired before legend items are rendered, allowing legend customization. The `LegendItemRendering` property will be used to bind this event in the `SfChart3D` component.

**Event Type**: `EventCallback<Chart3DLegendRenderEventArgs>`

**Event Args Properties**:
- `Text` - Legend item text (can be modified)
- `Fill` - Legend icon fill color (can be modified)
- `Shape` - Legend icon shape (can be modified)
- `Cancel` - Set to true to skip this legend item

**Usage**:
```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Quarterly Sales Overview"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="7"
           TiltAngle="10"
           Depth="100"
           LegendItemRendering ="@LegendRender">

    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"
                         Title="Month">
    </Chart3DPrimaryXAxis>

    <Chart3DPrimaryYAxis Title="Sales Amount">
    </Chart3DPrimaryYAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@Data"
                       Name="Q1 Sales"
                       XName="X"
                       YName="Y1"
                       Type="Chart3DSeriesType.Column"
                       Fill="#4C9AFF">
        </Chart3DSeries>

        <Chart3DSeries DataSource="@Data"
                       Name="Q2 Sales"
                       XName="X"
                       YName="Y2"
                       Type="Chart3DSeriesType.Column"
                       Fill="#FF8C00">
        </Chart3DSeries>
    </Chart3DSeriesCollection>

</SfChart3D>

@code {
    // Data Model
    public class SalesData
    {
        public string X { get; set; }
        public double Y1 { get; set; }   // Q1 Sales
        public double Y2 { get; set; }   // Q2 Sales
    }

    // Suitable data for legend customization
    public List<SalesData> Data = new List<SalesData>
    {
        new SalesData { X = "Jan", Y1 = 12000, Y2 = 15000 },
        new SalesData { X = "Feb", Y1 = 13500, Y2 = 16000 },
        new SalesData { X = "Mar", Y1 = 14200, Y2 = 17200 },
        new SalesData { X = "Apr", Y1 = 15800, Y2 = 18500 },
        new SalesData { X = "May", Y1 = 16500, Y2 = 19000 },
        new SalesData { X = "Jun", Y1 = 17500, Y2 = 20000 }
    };

    // Legend customization event
    private void LegendRender(Chart3DLegendRenderEventArgs args)
    {
        // Modify legend text
        args.Text = args.Text + " (Data)";

        // Change legend icon
        args.Shape = Syncfusion.Blazor.Chart3D.LegendShape.Circle;
    }
}
```

**Common Use Cases**:
- Custom legend text formatting
- Dynamic legend icon shapes
- Conditional legend items
- Localization

---

### OnTooltipRender

Fired before tooltip is displayed, allowing tooltip customization. The `TooltipRendering` property will be used to bind this event in the `SfChart3D` component.

**Event Type**: `EventCallback<Chart3DTooltipRenderEventArgs>`

**Event Args Properties**:
- `Text` - Tooltip text (can be modified)
- `Data` - Data point information
- `Series` - Series information
- `TextStyle` - Tooltip text style (can be modified)
- `Cancel` - Set to true to prevent tooltip display

**Usage**:
```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales Tooltip Demo"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="7"
           TiltAngle="10"
           Depth="100"
           TooltipRendering ="@TooltipRender">

    <Chart3DTooltipSettings Enable="true"></Chart3DTooltipSettings>

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"
                         Title="Month">
    </Chart3DPrimaryXAxis>

    <Chart3DPrimaryYAxis Title="Sales ($)">
    </Chart3DPrimaryYAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData"
                       Name="Monthly Sales"
                       XName="Month"
                       YName="Sales"
                       Type="Chart3DSeriesType.Column"
                       Fill="#4C9AFF">
        </Chart3DSeries>
    </Chart3DSeriesCollection>

</SfChart3D>

@code {
    public class SalesInfo
    {
        public string Month { get; set; }
        public double Sales { get; set; }
    }

    // Suitable dataset for tooltip customization
    public List<SalesInfo> SalesData = new List<SalesInfo>
    {
        new SalesInfo { Month = "Jan", Sales = 12000 },
        new SalesInfo { Month = "Feb", Sales = 15000 },
        new SalesInfo { Month = "Mar", Sales = 17500 },
        new SalesInfo { Month = "Apr", Sales = 16000 },
        new SalesInfo { Month = "May", Sales = 18500 },
        new SalesInfo { Month = "Jun", Sales = 20000 },
        new SalesInfo { Month = "Jul", Sales = 22000 }
    };

    private void TooltipRender(Chart3DTooltipRenderEventArgs args)
    {
        // Custom tooltip content
        args.Text = $"<b>{args.Data.PointX}</b><br/>" +
                    $"Sales: ${args.Data.PointY}<br/>" +
                    $"Series: {args.Series.Name}";

        // Custom tooltip text style
        args.TextStyle.Color = "white";
        args.TextStyle.Size = "14px";
    }
}
```

**Common Use Cases**:
- Rich tooltip content with HTML
- Custom formatting
- Additional data display
- Conditional tooltip content
- Styling based on data values

---

## User Interaction Events

### OnPointClick

Fired when a data point is clicked. The `PointClick` property will be used to bind this event in the `SfChart3D` component.

**Event Type**: `EventCallback<Chart3DPointClickEventArgs>`

**Event Args Properties**:
- `Point` - Clicked data point
- `Series` - Parent series
- `PointIndex` - Index of clicked point
- `SeriesIndex` - Index of parent series

**Usage**:
```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Click Event Demo"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="7"
           TiltAngle="10"
           Depth="100"
           PointClick ="@PointClick">

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"
                         Title="Month">
    </Chart3DPrimaryXAxis>

    <Chart3DPrimaryYAxis Title="Sales ($)">
    </Chart3DPrimaryYAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@Data"
                       XName="X"
                       YName="Y"
                       Type="Chart3DSeriesType.Column"
                       Fill="#4C9AFF">
        </Chart3DSeries>
    </Chart3DSeriesCollection>

</SfChart3D>

@code {
    public class ChartPoint
    {
        public string X { get; set; }
        public double Y { get; set; }
    }

    // Suitable dataset for point-click interaction
    public List<ChartPoint> Data = new List<ChartPoint>
    {
        new ChartPoint { X = "Jan", Y = 45 },
        new ChartPoint { X = "Feb", Y = 52 },
        new ChartPoint { X = "Mar", Y = 60 },
        new ChartPoint { X = "Apr", Y = 58 },
        new ChartPoint { X = "May", Y = 72 },
        new ChartPoint { X = "Jun", Y = 68 },
        new ChartPoint { X = "Jul", Y = 80 }
    };

    private void PointClick(Chart3DPointClickEventArgs args)
    {
        Console.WriteLine($"Clicked point: Series {args.SeriesIndex}, Point {args.PointIndex}");
        Console.WriteLine($"Value: {args.Point.YValue}");

        // Example navigation logic (uncomment if needed)
        // NavigationManager.NavigateTo($"/details/{args.Point.XValue}");
    }
}
```

**Common Use Cases**:
- Drill-down functionality
- Navigation to detail views
- Displaying additional information
- Triggering actions based on point selection

---

### OnLegendClick

Fired when a legend item is clicked. The `LegendClick` property will be used to bind this event in the `SfChart3D` component.

**Event Type**: `EventCallback<Chart3DLegendClickEventArgs>`

**Event Args Properties**:
- `Series` - Series associated with legend item
- `Point` - Point data (for point-based legends)
- `Cancel` - Set to true to prevent default toggle behavior

**Usage**:
```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Legend Click Demo"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="7"
           TiltAngle="10"
           Depth="100"
           LegendClick ="@LegendClick">

    <Chart3DLegendSettings Visible="true"
                           ToggleVisibility="true">
    </Chart3DLegendSettings>

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"
                         Title="Month">
    </Chart3DPrimaryXAxis>

    <Chart3DPrimaryYAxis Title="Values">
    </Chart3DPrimaryYAxis>

    <Chart3DSeriesCollection>

        <!-- Critical Series (Protected from Hiding) -->
        <Chart3DSeries DataSource="@Data"
                       Name="Critical Series"
                       XName="X"
                       YName="Y1"
                       Type="Chart3DSeriesType.Column"
                       Fill="#FF4C4C">
        </Chart3DSeries>

        <!-- Regular Series -->
        <Chart3DSeries DataSource="@Data"
                       Name="Series 2"
                       XName="X"
                       YName="Y2"
                       Type="Chart3DSeriesType.Column"
                       Fill="#4C9AFF">
        </Chart3DSeries>

    </Chart3DSeriesCollection>

</SfChart3D>

@code {
    public class ChartData
    {
        public string X { get; set; }
        public double Y1 { get; set; }  // Critical series values
        public double Y2 { get; set; }  // Regular series values
    }

    // Suitable dataset for demonstrating toggle visibility + critical protection
    public List<ChartData> Data = new List<ChartData>
    {
        new ChartData { X = "Jan", Y1 = 80, Y2 = 40 },
        new ChartData { X = "Feb", Y1 = 75, Y2 = 55 },
        new ChartData { X = "Mar", Y1 = 90, Y2 = 60 },
        new ChartData { X = "Apr", Y1 = 85, Y2 = 50 },
        new ChartData { X = "May", Y1 = 95, Y2 = 65 },
        new ChartData { X = "Jun", Y1 = 100, Y2 = 70 }
    };

    private void LegendClick(Chart3DLegendClickEventArgs args)
    {
        Console.WriteLine($"Legend clicked: {args.Series.Name}");

        // Prevent hiding the critical series
        if (args.Series.Name == "Critical Series")
        {
            args.Cancel = true;
        }
    }
}
```

**Common Use Cases**:
- Custom series visibility logic
- Preventing certain series from being hidden
- Tracking user interactions
- Triggering related UI updates

---

### OnSelectionChanged

Fired when data point selection changes. The `SelectionChanged` property will be used to bind this event in the `SfChart3D` component.

**Event Type**: `EventCallback<Chart3DSelectionChangedEventArgs>`

**Event Args Properties**:
- `SelectedDataIndexes` - Array of selected data indexes
- `IsSelected` - Whether points were selected or deselected
- `Series` - Affected series
- `PointIndex` - Affected point index
- `SeriesIndex` - Affected series index

**Usage**:
```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="3D Selection Sample"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="5"
           TiltAngle="8"
           Depth="100"
           SelectionMode="Syncfusion.Blazor.Chart3D.SelectionMode.Point"
           SelectionChanged ="@SelectionChanged">

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"
                         Title="Month">
    </Chart3DPrimaryXAxis>

    <Chart3DPrimaryYAxis Title="Sales">
    </Chart3DPrimaryYAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@Data"
                       XName="X"
                       YName="Y"
                       Name="Sales"
                       Type="Chart3DSeriesType.Column"
                       Fill="#4C9AFF">
        </Chart3DSeries>
    </Chart3DSeriesCollection>
</SfChart3D>

<div class="mt-3">
    <h4>Selected Data Values (From SelectedDataValues API):</h4>

    @if (selectedValues.Count == 0)
    {
        <p>No points selected.</p>
    }
    else
    {
        <ul>
            @foreach (var p in selectedValues)
            {
                <li>@($"X = {p.X},  Y = {p.Y}")</li>
            }
        </ul>
    }
</div>

@code {

    public class ChartPoint
    {
        public string X { get; set; }
        public double Y { get; set; }
    }

    // Suitable dataset
    public List<ChartPoint> Data = new List<ChartPoint>
    {
        new ChartPoint { X = "Jan", Y = 45 },
        new ChartPoint { X = "Feb", Y = 52 },
        new ChartPoint { X = "Mar", Y = 60 },
        new ChartPoint { X = "Apr", Y = 58 },
        new ChartPoint { X = "May", Y = 72 },
        new ChartPoint { X = "Jun", Y = 68 },
        new ChartPoint { X = "Jul", Y = 80 }
    };

    // Stores all selected X,Y points (as List<PointXY>)
    private List<Syncfusion.Blazor.Chart3D.PointXY> selectedValues = new();

    private void SelectionChanged(Chart3DSelectionChangedEventArgs args)
    {
        // The ONLY API available:
        selectedValues = args.SelectedDataValues;

        // Log for demonstration
        Console.WriteLine("Selection changed:");
        foreach (var p in selectedValues)
        {
            Console.WriteLine($"Selected -> X: {p.X}, Y: {p.Y}");
        }

        StateHasChanged();
    }
}
```

**Common Use Cases**:
- Multi-select data analysis
- Building comparison views
- Filtering related data
- Synchronized selection across components

---

## Mouse Events

### OnChartMouseMove

Fired when mouse moves over the chart area. The `MouseMove` property will be used to bind this event in the `SfChart3D` component.

**Event Type**: `EventCallback<Chart3DMouseEventArgs>`

**Event Args Properties**:
- `Target` - Element under mouse
- `X` - Mouse X position
- `Y` - Mouse Y position

**Usage**:
```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Mouse Move Demo"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="5"
           TiltAngle="8"
           Depth="100"
           MouseMove="@MouseMove">

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"
                         Title="Month">
    </Chart3DPrimaryXAxis>

    <Chart3DPrimaryYAxis Title="Sales">
    </Chart3DPrimaryYAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@Data"
                       XName="X"
                       YName="Y"
                       Type="Chart3DSeriesType.Column"
                       Fill="#4C9AFF">
        </Chart3DSeries>
    </Chart3DSeriesCollection>

</SfChart3D>

@code {

    public class ChartPoint
    {
        public string X { get; set; }
        public double Y { get; set; }
    }

    // ✅ Basic sample dataset
    public List<ChartPoint> Data = new()
    {
        new ChartPoint { X = "Jan", Y = 45 },
        new ChartPoint { X = "Feb", Y = 52 },
        new ChartPoint { X = "Mar", Y = 60 },
        new ChartPoint { X = "Apr", Y = 58 },
        new ChartPoint { X = "May", Y = 72 }
    };

    private void MouseMove(Chart3DMouseEventArgs args)
    {
        // ✅ Logs the mouse coordinates on the chart
        Console.WriteLine($"Mouse position: ({args.MouseX}, {args.MouseY})");
    }
}
``
```

**Common Use Cases**:
- Custom tooltips
- Crosshair implementation
- Tracking mouse position
- Interactive guides

---

### OnChartMouseClick

Fired when chart area is clicked. The `MouseClick` property will be used to bind this event in the `SfChart3D` component.

**Event Type**: `EventCallback<Chart3DMouseEventArgs>`

**Event Args Properties**:
- `Target` - Clicked element
- `X` - Click X position
- `Y` - Click Y position

**Usage**:
```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Mouse Click Demo"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="5"
           TiltAngle="8"
           Depth="100"
           MouseClick="@ChartClick">

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"
                         Title="Month">
    </Chart3DPrimaryXAxis>

    <Chart3DPrimaryYAxis Title="Sales">
    </Chart3DPrimaryYAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@Data"
                       XName="X"
                       YName="Y"
                       Type="Chart3DSeriesType.Column"
                       Fill="#4C9AFF">
        </Chart3DSeries>
    </Chart3DSeriesCollection>

</SfChart3D>

@code {

    public class ChartPoint
    {
        public string X { get; set; }
        public double Y { get; set; }
    }

    // ✅ Basic dataset for click testing
    public List<ChartPoint> Data = new()
    {
        new ChartPoint { X = "Jan", Y = 45 },
        new ChartPoint { X = "Feb", Y = 52 },
        new ChartPoint { X = "Mar", Y = 60 },
        new ChartPoint { X = "Apr", Y = 58 },
        new ChartPoint { X = "May", Y = 72 }
    };

    private void ChartClick(Chart3DMouseEventArgs args)
    {
        // ✅ Logs mouse click coordinates
        Console.WriteLine($"Chart clicked at: ({args.MouseX}, {args.MouseY})");
    }
}
```

**Common Use Cases**:
- General chart interactions
- Adding annotations
- Context menus
- Custom selection logic

---

### OnChartMouseDown

Fired when mouse button is pressed down on chart. The `MouseDown` property will be used to bind this event in the `SfChart3D` component.

**Event Type**: `EventCallback<Chart3DMouseEventArgs>`

**Usage**:
```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Mouse Down Demo"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="5"
           TiltAngle="8"
           Depth="100"
           MouseDown="@MouseDown">

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"
                         Title="Month">
    </Chart3DPrimaryXAxis>

    <Chart3DPrimaryYAxis Title="Sales">
    </Chart3DPrimaryYAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@Data"
                       XName="X"
                       YName="Y"
                       Type="Chart3DSeriesType.Column"
                       Fill="#4C9AFF">
        </Chart3DSeries>
    </Chart3DSeriesCollection>

</SfChart3D>

@code {

    public class ChartPoint
    {
        public string X { get; set; }
        public double Y { get; set; }
    }

    // ✅ Suitable sample data
    public List<ChartPoint> Data = new()
    {
        new ChartPoint { X = "Jan", Y = 45 },
        new ChartPoint { X = "Feb", Y = 52 },
        new ChartPoint { X = "Mar", Y = 60 },
        new ChartPoint { X = "Apr", Y = 58 },
        new ChartPoint { X = "May", Y = 72 }
    };

    private void MouseDown(Chart3DMouseEventArgs args)
    {
        Console.WriteLine("Mouse button pressed on chart");
        Console.WriteLine($"Mouse position: ({args.MouseX}, {args.MouseY})");
    }
}
```

**Common Use Cases**:
- Drag operations
- Custom selection start
- Context menu triggers

---

### OnChartMouseUp

Fired when mouse button is released on chart. The `MouseUp` property will be used to bind this event in the `SfChart3D` component.

**Event Type**: `EventCallback<Chart3DMouseEventArgs>`

**Usage**:
```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Mouse Up Demo"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="5"
           TiltAngle="8"
           Depth="100"
           MouseUp="@MouseUp">

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"
                         Title="Month">
    </Chart3DPrimaryXAxis>

    <Chart3DPrimaryYAxis Title="Sales">
    </Chart3DPrimaryYAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@Data"
                       XName="X"
                       YName="Y"
                       Type="Chart3DSeriesType.Column"
                       Fill="#4C9AFF">
        </Chart3DSeries>
    </Chart3DSeriesCollection>

</SfChart3D>

@code {

    public class ChartPoint
    {
        public string X { get; set; }
        public double Y { get; set; }
    }

    // ✅ Basic sample dataset
    public List<ChartPoint> Data = new()
    {
        new ChartPoint { X = "Jan", Y = 45 },
        new ChartPoint { X = "Feb", Y = 52 },
        new ChartPoint { X = "Mar", Y = 60 },
        new ChartPoint { X = "Apr", Y = 58 },
        new ChartPoint { X = "May", Y = 72 }
    };

    private void MouseUp(Chart3DMouseEventArgs args)
    {
        Console.WriteLine("Mouse button released on chart");
        Console.WriteLine($"Mouse position: ({args.MouseX}, {args.MouseY})");
    }
}
```

**Common Use Cases**:
- Completing drag operations
- Custom selection end
- Region selection

---

## Export and Data Events

### OnExported

Fired after chart is exported. The `ExportCompleted` property will be used to bind this event in the `SfChart3D` component.

**Event Type**: `EventCallback<Chart3DExportedEventArgs>`

**Event Args Properties**:
- `Type` - Export type (PNG, JPEG, SVG, PDF)
- `Data` - Exported data

**Usage**:
```razor

@using Syncfusion.Blazor.Chart3D

<SfChart3D @ref="chartRef"
           Title="3D Export Example"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="10"
           TiltAngle="8"
           Depth="100"
           ExportCompleted ="@OnExported">

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"
                         Title="Month">
    </Chart3DPrimaryXAxis>

    <Chart3DPrimaryYAxis Title="Sales">
    </Chart3DPrimaryYAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData"
                       XName="Month"
                       YName="Sales"
                       Type="Chart3DSeriesType.Column"
                       Fill="#4C9AFF">
        </Chart3DSeries>
    </Chart3DSeriesCollection>

</SfChart3D>

<button class="btn btn-primary mt-3" @onclick="ExportChart">
    Export Chart (PNG + Base64)
</button>

<div class="mt-3">
    <h4>Export Result:</h4>
    <p>@exportMessage</p>
</div>

@code {
    private SfChart3D chartRef;

    private string exportMessage = "";

    public class SalesInfo
    {
        public string Month { get; set; }
        public double Sales { get; set; }
    }

    public List<SalesInfo> SalesData = new()
    {
        new SalesInfo { Month = "Jan", Sales = 12000 },
        new SalesInfo { Month = "Feb", Sales = 15000 },
        new SalesInfo { Month = "Mar", Sales = 18000 },
        new SalesInfo { Month = "Apr", Sales = 16500 }
    };

    private async Task ExportChart()
    {
        // isBase64 must be TRUE to populate Base64 string
        await chartRef.ExportAsync(
            Syncfusion.Blazor.Chart3D.ExportType.PNG,
            "chart3d",
            orientation: null,
            isBase64: true,
            allowDownload: false
        );
    }

    private void OnExported(Chart3DExportedEventArgs args)
    {
        Console.WriteLine("Export completed!");

        Console.WriteLine("DataUrl:");
        Console.WriteLine(args.DataUrl);

        if (!string.IsNullOrEmpty(args.Base64))
        {
            Console.WriteLine("Base64 length: " + args.Base64.Length);
            exportMessage = "Export complete! Base64 + DataUrl received.";
        }
        else
        {
            exportMessage = "Export complete! DataUrl received.";
        }

        StateHasChanged();
    }
}
```

**Common Use Cases**:
- Export confirmation
- Logging exports
- Post-export actions
- Analytics tracking

---

### OnAxisRangeRendered

Fired after axis range is calculated but before rendering. The `AxisRangeRendered` property will be used to bind this event in the `SfChart3D` component.

**Event Type**: `EventCallback<Chart3DAxisRangeRenderedEventArgs>`

**Event Args Properties**:
- `Axis` - Axis instance
- `Minimum` - Calculated minimum value
- `Maximum` - Calculated maximum value
- `Interval` - Calculated interval

**Usage**:
```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="3D Axis Range Demo"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="8"
           TiltAngle="10"
           Depth="100"
           AxisRangeRendered ="@AxisRangeCalculated">

    <Chart3DPrimaryXAxis 
        ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"
        Title="Month">
    </Chart3DPrimaryXAxis>

    <Chart3DPrimaryYAxis Title="Sales">
    </Chart3DPrimaryYAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@Data"
                       XName="Month"
                       YName="Sales"
                       Type="Chart3DSeriesType.Column"
                       Fill="#4C9AFF">
        </Chart3DSeries>
    </Chart3DSeriesCollection>

</SfChart3D>

<div class="mt-3">
    <h4>Last Axis Range:</h4>
    <p>@axisMessage</p>
</div>

@code {

    private string axisMessage = "Range not calculated yet.";

    public class ChartPoint
    {
        public string Month { get; set; }
        public double Sales { get; set; }
    }

    public List<ChartPoint> Data = new()
    {
        new ChartPoint { Month = "Jan", Sales = 40 },
        new ChartPoint { Month = "Feb", Sales = 55 },
        new ChartPoint { Month = "Mar", Sales = 60 },
        new ChartPoint { Month = "Apr", Sales = 70 },
        new ChartPoint { Month = "May", Sales = 80 }
    };

    private void AxisRangeCalculated(Chart3DAxisRangeRenderedEventArgs args)
    {
        Console.WriteLine($"Axis: {args.AxisName}");
        Console.WriteLine($"Min: {args.Minimum}, Max: {args.Maximum}, Interval: {args.Interval}");

        axisMessage = $"Axis {args.AxisName} → Min={args.Minimum}, Max={args.Maximum}, Interval={args.Interval}";
        StateHasChanged();
    }
}
```

**Common Use Cases**:
- Range validation
- Dynamic UI adjustments
- Analytics and logging
- Synchronizing multiple charts

---

## Complete Example with Multiple Events

```razor
@page "/3d-chart-events"
@using Syncfusion.Blazor.Chart3D

<h3>3D Chart – All Major Events</h3>

<div>
    <p><b>Last Action:</b> @lastAction</p>

    <p><b>Selected Data Values:</b></p>
    @if (selectedValues.Count == 0)
    {
        <p>No selection yet.</p>
    }
    else
    {
        <ul>
            @foreach (var p in selectedValues)
            {
                <li>@($"X = {p.X}, Y = {p.Y}")</li>
            }
        </ul>
    }
</div>

<SfChart3D @ref="chartRef"
           Title="Sales & Target Analysis"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="7"
           TiltAngle="10"
           Depth="100"
           Created="@ChartCreated"
           PointClick="@PointClick"
           PointRendering="@PointRender"
           SeriesRendering="@SeriesRender"
           AxisLabelRendering="@AxisLabelRender"
           TooltipRendering="@TooltipRender"
           LegendClick="@LegendClick"
           SelectionChanged="@SelectionChanged">

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"
                         Title="Month">
    </Chart3DPrimaryXAxis>

    <Chart3DPrimaryYAxis Title="Values">
    </Chart3DPrimaryYAxis>

    <Chart3DLegendSettings Visible="true" ToggleVisibility="true"></Chart3DLegendSettings>
    <Chart3DTooltipSettings Enable="true"></Chart3DTooltipSettings>

    <Chart3DSeriesCollection>

        <Chart3DSeries DataSource="@SalesData"
                       Name="Sales"
                       XName="Month"
                       YName="Sales"
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>

        <Chart3DSeries DataSource="@SalesData"
                       Name="Target"
                       XName="Month"
                       YName="Target"
                       Type="Chart3DSeriesType.Column">
        </Chart3DSeries>

    </Chart3DSeriesCollection>

</SfChart3D>

@code {

    private SfChart3D chartRef;
    private string lastAction = "None";

    private List<Syncfusion.Blazor.Chart3D.PointXY> selectedValues = new();

    public class SalesInfo
    {
        public string Month { get; set; }
        public double Sales { get; set; }
        public double Target { get; set; }
    }

    public List<SalesInfo> SalesData = new()
    {
        new SalesInfo { Month = "Jan", Sales = 35, Target = 40 },
        new SalesInfo { Month = "Feb", Sales = 28, Target = 35 },
        new SalesInfo { Month = "Mar", Sales = 42, Target = 40 },
        new SalesInfo { Month = "Apr", Sales = 32, Target = 38 },
        new SalesInfo { Month = "May", Sales = 45, Target = 42 },
        new SalesInfo { Month = "Jun", Sales = 38, Target = 40 }
    };

    private void ChartCreated(Chart3DCreatedEventArgs args)
    {
        lastAction = "Chart Created";
        StateHasChanged();
    }

    private void PointClick(Chart3DPointClickEventArgs args)
    {
        lastAction = $"Clicked: {args.Series.Name} – {args.Point.XValue}";
        StateHasChanged();
    }

    private void PointRender(Chart3DPointRenderEventArgs args)
    {
        if (args.Series.Name == "Sales")
        {
            double y = Convert.ToDouble(args.Point.YValue);
            var row = SalesData.FirstOrDefault(x => x.Sales == y);

            if (row != null)
            {
                args.Fill = row.Sales >= row.Target ? "green" : "red";
            }
        }
    }

    private void SeriesRender(Chart3DSeriesRenderEventArgs args)
    {
        if (args.Series.Name == "Target")
        {
            args.Fill = "#FFA500"; // Orange
        }
    }

    private void AxisLabelRender(Chart3DAxisLabelRenderEventArgs args)
    {
        if (args.Axis.Name == "PrimaryYAxis")
        {
            args.Text = args.Text + " units";
        }
    }

    private void TooltipRender(Chart3DTooltipRenderEventArgs args)
    {
        args.Text = $"<b>{args.Data.PointX}</b><br/>{args.Series.Name}: {args.Data.PointY}";
        args.TextStyle.Color = "white";
        args.TextStyle.Size = "14px";
    }

    private void LegendClick(Chart3DLegendClickEventArgs args)
    {
        lastAction = $"Legend clicked: {args.Series.Name}";
        StateHasChanged();
    }

    private void SelectionChanged(Chart3DSelectionChangedEventArgs args)
    {
        selectedValues = args.SelectedDataValues;
        lastAction = "Selection changed";
        StateHasChanged();
    }
}
```

## Event Best Practices

1. **Performance Considerations**:
   - Avoid heavy computations in frequently fired events (OnPointRender, OnMouseMove)
   - Use debouncing for mouse events if needed
   - Cancel rendering when necessary to improve performance

2. **State Management**:
   - Call `StateHasChanged()` after updating component state in events
   - Use async event handlers for async operations
   - Avoid circular event triggers

3. **Error Handling**:
   - Wrap event handlers in try-catch blocks
   - Validate event argument data before use
   - Log errors for debugging

4. **Event Ordering**:
   - Rendering events fire before display
   - Interaction events fire after user action
   - Lifecycle events fire at component boundaries

5. **Cancellation**:
   - Set `Cancel = true` in event args to prevent default behavior
   - Use cancellation judiciously to avoid breaking expected functionality

## See Also

- [API Reference](api-reference.md)
- [Getting Started](getting-started.md)
- [Chart Types](chart-types.md)
