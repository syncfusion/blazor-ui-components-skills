# Syncfusion Blazor 3D Chart API Reference

## Table of Contents

- [Main Component](#main-component)
    - [SfChart3D](#sfchart3d)
- [Series Components](#series-components)
    - [Chart3DSeries](#chart3dseries)
    - [Chart3DSeriesCollection](#chart3dseriescollection)
    - [Chart3DSeriesBorder](#chart3dseriesborder)
- [Axis Components](#axis-components)
    - [Chart3DPrimaryXAxis](#chart3dprimaryxaxis)
    - [Chart3DPrimaryYAxis](#chart3dprimaryyaxis)
    - [Chart3DAxis](#chart3daxis)
    - [Chart3DAxes](#chart3daxes)
- [Data Label Components](#data-label-components)
    - [Chart3DDataLabel](#chart3ddatalabel)
- [Legend Components](#legend-components)
- [Tooltip Components](#tooltip-components)
- [Title and Subtitle Components](#title-and-subtitle-components)
- [Layout Components](#layout-components)
- [Selection Components](#selection-components)
- [Animation Components](#animation-components)
- [Empty Point Components](#empty-point-components)
- [Border and Margin Components](#border-and-margin-components)
- [Event Argument Classes](#event-argument-classes)
- [Data Classes](#data-classes)
- [Base Classes](#base-classes)
- [Enumerations](#enumerations)
- [Interface](#interface)
- [Usage Examples](#usage-examples)
- [API Reference Links](#api-reference-links)
- [See Also](#see-also)

This document provides a comprehensive reference for all classes, interfaces, and enums available in the `Syncfusion.Blazor.Chart3D` namespace.

## Main Component

### SfChart3D

The primary component for rendering 3D charts in Blazor applications.

**Namespace**: `Syncfusion.Blazor.Chart3D`

**Key Properties**:
- `Title` - Chart title text
- `SubTitle` - Chart subtitle text
- `Width` - Chart width (string with units)
- `Height` - Chart height (string with units)
- `WallColor` - Background color of 3D walls
- `EnableRotation` - Enable/disable mouse rotation
- `RotationAngle` - Initial rotation angle (0-360)
- `TiltAngle` - Tilt angle for 3D perspective
- `Depth` - Depth of 3D rendering
- `Theme` - Chart theme (Material, Bootstrap, Fluent, Tailwind, etc.)
- `BackgroundImage` - Background image for the chart
- `Background` - Background color for the chart

**Child Components**:
- `Chart3DPrimaryXAxis` - Primary X-axis configuration
- `Chart3DPrimaryYAxis` - Primary Y-axis configuration
- `Chart3DSeriesCollection` - Collection of chart series
- `Chart3DLegendSettings` - Legend configuration
- `Chart3DTooltipSettings` - Tooltip configuration
- `Chart3DRows` - Row-based layout configuration
- `Chart3DColumns` - Column-based layout configuration
- `Chart3DAxes` - Additional axes collection

**Methods**:
- `ExportAsync()` - Export chart to image or PDF
- `PrintAsync()` - Print the chart
- `Refresh()` - Refresh the chart rendering

**Events**:
- `OnCreated` - Fired after chart is created
- `OnAxisLabelRender` - Fired before axis labels render
- `OnSeriesRender` - Fired before series renders
- `OnPointRender` - Fired before point renders
- `OnTooltipRender` - Fired before tooltip renders
- `OnLegendRender` - Fired before legend renders
- `OnLegendClick` - Fired when legend is clicked
- `OnPointClick` - Fired when point is clicked
- `OnSelectionChanged` - Fired when selection changes
- `OnResized` - Fired when chart is resized
- `OnExported` - Fired after chart is exported

## Series Components

### Chart3DSeries

Represents a single data series in the 3D chart.

**Key Properties**:
- `DataSource` - Data collection for the series
- `XName` - Property name for X-axis values
- `YName` - Property name for Y-axis values
- `Name` - Series name for legend
- `Type` - Chart type (Column, Bar, StackingColumn, StackingBar)
- `Fill` - Series color
- `Opacity` - Series opacity (0-1)
- `ColumnWidth` - Width of columns (0-1)
- `ColumnSpacing` - Space between columns (0-1)
- `GroupName` - Group name for grouped columns
- `Visible` - Series visibility

**Child Components**:
- `Chart3DDataLabel` - Data label configuration
- `Chart3DEmptyPointSettings` - Empty point handling
- `Chart3DSeriesBorder` - Series border settings

### Chart3DSeriesCollection

Container for multiple Chart3DSeries components.

### Chart3DSeriesBorder

Border customization for series.

**Properties**:
- `Color` - Border color
- `Width` - Border width

## Axis Components

### Chart3DPrimaryXAxis

Primary X-axis configuration component.

**Key Properties**:
- `ValueType` - Axis value type (Category, Numeric, DateTime, Logarithmic)
- `Title` - Axis title text
- `Minimum` - Minimum axis value
- `Maximum` - Maximum axis value
- `Interval` - Interval between axis values
- `LabelFormat` - Label format string
- `LabelRotationAngle` - Label rotation angle
- `EdgeLabelPlacement` - Edge label placement
- `LabelPlacement` - Label placement (OnTicks, BetweenTicks)
- `RangePadding` - Range padding type
- `IsInversed` - Invert axis direction
- `OpposedPosition` - Position axis on opposite side
- `Visible` - Axis visibility

**Child Components**:
- `Chart3DAxisLabelStyle` - Label font style
- `Chart3DAxisTitleStyle` - Title font style
- `Chart3DMajorGridLines` - Major grid lines
- `Chart3DMinorGridLines` - Minor grid lines
- `Chart3DMajorTickLines` - Major tick lines
- `Chart3DMinorTickLines` - Minor tick lines

### Chart3DPrimaryYAxis

Primary Y-axis configuration component (same properties as Chart3DPrimaryXAxis).

### Chart3DAxis

Generic axis component for additional axes.

### Chart3DAxes

Collection of additional axes.

### Chart3DAxisLabelStyle

Font style customization for axis labels.

**Properties**:
- `Size` - Font size
- `FontFamily` - Font family
- `FontStyle` - Font style (Normal, Italic, Oblique)
- `FontWeight` - Font weight
- `Color` - Font color

### Chart3DAxisTitleStyle

Font style customization for axis titles (same properties as Chart3DAxisLabelStyle).

### Chart3DMajorGridLines

Major grid line customization.

**Properties**:
- `Width` - Line width
- `Color` - Line color
- `DashArray` - Dash pattern

### Chart3DMinorGridLines

Minor grid line customization (same properties as Chart3DMajorGridLines).

### Chart3DMajorTickLines

Major tick line customization.

**Properties**:
- `Width` - Line width
- `Height` - Line height
- `Color` - Line color

### Chart3DMinorTickLines

Minor tick line customization (same properties as Chart3DMajorTickLines).

## Data Label Components

### Chart3DDataLabel

Data label configuration for series.

**Key Properties**:
- `Visible` - Label visibility
- `Position` - Label position (Top, Middle, Bottom, Outer)
- `Fill` - Label background color
- `Opacity` - Label opacity
- `Angle` - Label rotation angle
- `EnableRotation` - Enable label rotation
- `Template` - Custom label template

**Child Components**:
- `Chart3DDataLabelFont` - Label font style
- `Chart3DDataLabelBorder` - Label border
- `Chart3DDataLabelMargin` - Label margin

### Chart3DDataLabelFont

Font style for data labels.

**Properties**:
- `Size` - Font size
- `FontFamily` - Font family
- `FontStyle` - Font style
- `FontWeight` - Font weight
- `Color` - Font color

### Chart3DDataLabelBorder

Border customization for data labels.

**Properties**:
- `Color` - Border color
- `Width` - Border width

### Chart3DDataLabelMargin

Margin customization for data labels.

**Properties**:
- `Left` - Left margin
- `Right` - Right margin
- `Top` - Top margin
- `Bottom` - Bottom margin

## Legend Components

### Chart3DLegendSettings

Legend configuration for the chart.

**Key Properties**:
- `Visible` - Legend visibility
- `Position` - Legend position (Top, Bottom, Left, Right, Custom)
- `Alignment` - Legend alignment (Near, Center, Far)
- `Height` - Legend height
- `Width` - Legend width
- `Location` - Custom location (X, Y)
- `Padding` - Legend padding
- `ShapeHeight` - Legend icon height
- `ShapeWidth` - Legend icon width
- `ShapePadding` - Padding around legend icon
- `ToggleVisibility` - Enable series toggle on click
- `Mode` - Legend mode (Series, Point)

**Child Components**:
- `Chart3DLegendTextStyle` - Legend text font style
- `Chart3DLegendBorder` - Legend border
- `Chart3DLegendMargin` - Legend margin
- `Chart3DLegendContainerPadding` - Container padding
- `Chart3DLegendTitleStyle` - Title font style

### Chart3DLegendTextStyle

Font style for legend text.

### Chart3DLegendBorder

Border customization for legend.

### Chart3DLegendMargin

Margin customization for legend.

### Chart3DLegendContainerPadding

Container padding for legend.

### Chart3DLegendTitleStyle

Font style for legend title.

## Tooltip Components

### Chart3DTooltipSettings

Tooltip configuration for the chart.

**Key Properties**:
- `Enable` - Enable tooltip
- `Fill` - Tooltip background color
- `Opacity` - Tooltip opacity
- `Format` - Tooltip format string
- `Shared` - Shared tooltip across series
- `EnableMarker` - Enable marker in tooltip
- `FadeOutDuration` - Fade out duration
- `FadeOutMode` - Fade out mode (Click, Move)
- `Template` - Custom tooltip template

**Child Components**:
- `Chart3DTooltipTextStyle` - Tooltip text font style
- `Chart3DTooltipBorder` - Tooltip border
- `Chart3DTooltipLocation` - Tooltip location

### Chart3DTooltipTextStyle

Font style for tooltip text.

### Chart3DTooltipBorder

Border customization for tooltip.

### Chart3DTooltipLocation

Location specification for tooltip.

**Properties**:
- `X` - X coordinate
- `Y` - Y coordinate

## Title and Subtitle Components

### Chart3DTitleStyle

Font style for chart title.

**Properties**:
- `Size` - Font size
- `FontFamily` - Font family
- `FontStyle` - Font style
- `FontWeight` - Font weight
- `Color` - Font color
- `TextAlignment` - Text alignment (Near, Center, Far)
- `TextOverflow` - Text overflow handling (None, Wrap, Trim)

**Child Component**:
- `Chart3DTitleBorder` - Title border

### Chart3DTitleBorder

Border customization for chart title.

### Chart3DSubTitleStyle

Font style for chart subtitle (same properties as Chart3DTitleStyle).

**Child Component**:
- `Chart3DSubTitleBorder` - Subtitle border

### Chart3DSubTitleBorder

Border customization for chart subtitle.

## Layout Components

### Chart3DRows

Container for row-based chart layout.

### Chart3DRow

Single row configuration in chart layout.

**Properties**:
- `Height` - Row height
- `Border` - Row border

### Chart3DColumns

Container for column-based chart layout.

### Chart3DColumn

Single column configuration in chart layout.

**Properties**:
- `Width` - Column width
- `Border` - Column border

## Selection Components

### Chart3DSelectedDataIndex

Represents a selected data point.

**Properties**:
- `SeriesIndex` - Series index
- `PointIndex` - Point index

### Chart3DSelectedDataIndexes

Collection of selected data indexes.

## Animation Components

### Chart3DAnimation

Animation configuration for series.

**Properties**:
- `Enable` - Enable animation
- `Duration` - Animation duration in milliseconds
- `Delay` - Animation delay in milliseconds

## Empty Point Components

### Chart3DEmptyPointSettings

Configuration for handling empty points in series.

**Properties**:
- `Mode` - Empty point mode (Zero, Average, Drop, Gap)
- `Fill` - Empty point fill color
- `Border` - Empty point border

## Border and Margin Components

### Chart3DBorder

Border customization for chart.

**Properties**:
- `Color` - Border color
- `Width` - Border width

### Chart3DMargin

Margin customization for chart.

**Properties**:
- `Left` - Left margin
- `Right` - Right margin
- `Top` - Top margin
- `Bottom` - Bottom margin

### Chart3DLocation

Location specification.

**Properties**:
- `X` - X coordinate
- `Y` - Y coordinate

## Event Argument Classes

### Chart3DCreatedEventArgs

Event arguments for chart creation event.

**Properties**:
- `Chart` - Chart instance

### Chart3DAxisLabelRenderEventArgs

Event arguments for axis label render event.

**Properties**:
- `Text` - Label text
- `Value` - Label value
- `LabelStyle` - Label style
- `Axis` - Axis instance
- `Cancel` - Cancel rendering flag

### Chart3DSeriesRenderEventArgs

Event arguments for series render event.

**Properties**:
- `Series` - Series instance
- `Data` - Series data
- `Fill` - Series fill color
- `Cancel` - Cancel rendering flag

### Chart3DPointRenderEventArgs

Event arguments for point render event.

**Properties**:
- `Point` - Point data
- `Series` - Series instance
- `Fill` - Point fill color
- `Border` - Point border
- `Cancel` - Cancel rendering flag

### Chart3DTooltipRenderEventArgs

Event arguments for tooltip render event.

**Properties**:
- `Text` - Tooltip text
- `Data` - Point data
- `Series` - Series instance
- `Cancel` - Cancel rendering flag
- `TextStyle` - Text style

### Chart3DLegendRenderEventArgs

Event arguments for legend render event.

**Properties**:
- `Text` - Legend text
- `Fill` - Legend fill color
- `Shape` - Legend shape
- `Cancel` - Cancel rendering flag

### Chart3DLegendClickEventArgs

Event arguments for legend click event.

**Properties**:
- `Series` - Series instance
- `Point` - Point data
- `Cancel` - Cancel action flag

### Chart3DPointClickEventArgs

Event arguments for point click event.

**Properties**:
- `Point` - Point data
- `Series` - Series instance
- `PointIndex` - Point index
- `SeriesIndex` - Series index

### Chart3DSelectionChangedEventArgs

Event arguments for selection changed event.

**Properties**:
- `SelectedDataIndexes` - Selected data indexes
- `IsSelected` - Selection state
- `Series` - Series instance
- `PointIndex` - Point index
- `SeriesIndex` - Series index

### Chart3DMouseEventArgs

Event arguments for mouse events.

**Properties**:
- `Target` - Event target
- `X` - Mouse X position
- `Y` - Mouse Y position

### Chart3DResizedEventArgs

Event arguments for resize event.

**Properties**:
- `CurrentSize` - Current chart size
- `PreviousSize` - Previous chart size

### Chart3DExportedEventArgs

Event arguments for export event.

**Properties**:
- `Type` - Export type
- `Data` - Export data

### Chart3DTextRenderEventArgs

Event arguments for text render event.

**Properties**:
- `Text` - Text content
- `Location` - Text location
- `Cancel` - Cancel rendering flag

### Chart3DAxisRangeRenderedEventArgs

Event arguments for axis range calculated event.

**Properties**:
- `Axis` - Axis instance
- `Minimum` - Calculated minimum
- `Maximum` - Calculated maximum
- `Interval` - Calculated interval

## Data Classes

### Chart3DDataPointInfo

Information about a chart data point.

**Properties**:
- `X` - X value
- `Y` - Y value
- `Text` - Point text
- `Tooltip` - Tooltip text

### Chart3DTooltipInfo

Tooltip information for a point.

**Properties**:
- `PointX` - Point X value
- `PointY` - Point Y value
- `SeriesName` - Series name
- `PointIndex` - Point index
- `SeriesIndex` - Series index

### PointXY

Represents point coordinates.

**Properties**:
- `X` - X value
- `Y` - Y value

## Base Classes

### Chart3DSubComponent

Abstract base class for 3D chart subcomponents.

### Chart3DDataBoundComponent

Base class for data-bound 3D chart components.

### Chart3DEventArgs

Base class for chart event arguments.

### Chart3DDefaultBorder

Default border settings.

### Chart3DDefaultFont

Default font settings.

### Chart3DDefaultLocation

Default location settings.

### Chart3DDefaultMargin

Default margin settings.

### Chart3DDefaultSelectedData

Default selected data settings.

### Chart3DEventBorder

Border settings for events.

### Chart3DEventFont

Font settings for events.

### Chart3DEventLocation

Location settings for events.

## Enumerations

### Chart3DSeriesType

Chart series types.

**Values**:
- `Column` - 3D column chart
- `Bar` - 3D bar chart
- `StackingColumn` - Stacked 3D column chart
- `StackingBar` - Stacked 3D bar chart

### ValueType

Axis value types.

**Values**:
- `Double` - Numeric axis
- `DateTime` - DateTime axis
- `Category` - Category axis
- `DateTimeCategory` - DateTime category axis
- `Logarithmic` - Logarithmic axis

### Chart3DDataLabelPosition

Data label positions.

**Values**:
- `Top` - Label at top of point
- `Middle` - Label at middle of point
- `Bottom` - Label at bottom of point
- `Outer` - Label outside point

### SelectionMode

Selection modes.

**Values**:
- `None` - No selection
- `Point` - Select individual points
- `Series` - Select entire series
- `Cluster` - Select cluster of points

### SelectionPattern

Selection patterns.

**Values**:
- `None` - No pattern
- `Dots` - Dot pattern
- `DiagonalForward` - Forward diagonal pattern
- `DiagonalBackward` - Backward diagonal pattern
- `Grid` - Grid pattern
- `Chessboard` - Chessboard pattern
- `HorizontalStripe` - Horizontal stripe pattern
- `VerticalStripe` - Vertical stripe pattern
- `Bubble` - Bubble pattern

### HighlightMode

Highlight modes.

**Values**:
- `None` - No highlighting
- `Point` - Highlight point
- `Series` - Highlight series
- `Cluster` - Highlight cluster

### LegendPosition

Legend positions.

**Values**:
- `Top` - Legend at top
- `Bottom` - Legend at bottom
- `Left` - Legend at left
- `Right` - Legend at right
- `Custom` - Custom position

### LegendShape

Legend icon shapes.

**Values**:
- `Circle` - Circle shape
- `Rectangle` - Rectangle shape
- `Triangle` - Triangle shape
- `Diamond` - Diamond shape
- `Cross` - Cross shape
- `HorizontalLine` - Horizontal line
- `VerticalLine` - Vertical line
- `Pentagon` - Pentagon shape
- `InvertedTriangle` - Inverted triangle
- `SeriesType` - Use series type shape

### Alignment

Alignment options.

**Values**:
- `Near` - Near alignment
- `Center` - Center alignment
- `Far` - Far alignment

### Chart3DTitlePosition

Title positions.

**Values**:
- `Top` - Title at top
- `Bottom` - Title at bottom
- `Left` - Title at left
- `Right` - Title at right
- `Custom` - Custom position

### Chart3DLegendTitlePosition

Legend title positions.

**Values**:
- `Top` - Title at top
- `Bottom` - Title at bottom
- `Left` - Title at left
- `Right` - Title at right

### Chart3DLegendMode

Legend rendering modes.

**Values**:
- `Series` - Legend per series
- `Point` - Legend per point
- `Range` - Legend for ranges
- `Gradient` - Gradient legend

### Chart3DFadeOutMode

Tooltip fade out modes.

**Values**:
- `Click` - Fade out on click
- `Move` - Fade out on move

### EmptyPointMode

Empty point handling modes.

**Values**:
- `Gap` - Leave gap for empty points
- `Zero` - Treat empty points as zero
- `Average` - Use average of neighbors
- `Drop` - Drop empty points

### EdgeLabelPlacement

Edge label placement options.

**Values**:
- `None` - No special placement
- `Shift` - Shift edge labels inside
- `Hide` - Hide edge labels

### LabelPlacement

Label placement options.

**Values**:
- `OnTicks` - Labels on tick marks
- `BetweenTicks` - Labels between tick marks

### LabelIntersectAction

Label intersection actions.

**Values**:
- `None` - No action
- `Hide` - Hide intersecting labels
- `Trim` - Trim intersecting labels
- `Wrap` - Wrap intersecting labels
- `Rotate45` - Rotate labels 45 degrees
- `Rotate90` - Rotate labels 90 degrees

### ChartRangePadding

Range padding options.

**Values**:
- `Auto` - Automatic padding
- `None` - No padding
- `Normal` - Normal padding
- `Additional` - Additional padding
- `Round` - Round to nice numbers

### IntervalType

DateTime interval types.

**Values**:
- `Auto` - Automatic interval
- `Years` - Year intervals
- `Months` - Month intervals
- `Days` - Day intervals
- `Hours` - Hour intervals
- `Minutes` - Minute intervals
- `Seconds` - Second intervals

### RangeIntervalType

Range interval types (same as IntervalType).

### AnimationType

Animation types.

**Values**:
- `Linear` - Linear animation
- `EaseIn` - Ease in animation
- `EaseOut` - Ease out animation
- `EaseInOut` - Ease in-out animation

### ExportType

Export file types.

**Values**:
- `PNG` - Export as PNG image
- `JPEG` - Export as JPEG image
- `SVG` - Export as SVG
- `PDF` - Export as PDF

### TextOverflow

Text overflow handling.

**Values**:
- `None` - No handling
- `Wrap` - Wrap text
- `Trim` - Trim text

### Chart3DShapeType

3D shape types for columns and bars.

**Values**:
- `Cylinder` - Cylindrical shape
- `Rectangle` - Rectangular shape

## Interface

### IChart3DLegendMethods

Interface for legend methods.

**Methods**:
- `GetMaxItemWidth()` - Get maximum item width
- `GetMaxItemHeight()` - Get maximum item height
- `GetTotalItemsCount()` - Get total items count

## Usage Examples

### Basic 3D Column Chart

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales Analysis" 
           WallColor="transparent" 
           EnableRotation="true" 
           RotationAngle="7" 
           TiltAngle="10" 
           Depth="100">
    
    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category" Title="Month">
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

    public List<SalesInfo> SalesData = new List<SalesInfo>
    {
        new SalesInfo { Month = "Jan", Sales = 12000 },
        new SalesInfo { Month = "Feb", Sales = 13500 },
        new SalesInfo { Month = "Mar", Sales = 14200 },
        new SalesInfo { Month = "Apr", Sales = 15800 },
        new SalesInfo { Month = "May", Sales = 16500 },
        new SalesInfo { Month = "Jun", Sales = 17200 },
        new SalesInfo { Month = "Jul", Sales = 18000 },
        new SalesInfo { Month = "Aug", Sales = 19000 },
        new SalesInfo { Month = "Sep", Sales = 17500 },
        new SalesInfo { Month = "Oct", Sales = 18500 },
        new SalesInfo { Month = "Nov", Sales = 19500 },
        new SalesInfo { Month = "Dec", Sales = 21000 }
    };
}
```

### Using Events

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Interactive Sales Chart"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="7"
           TiltAngle="10"
           Depth="100"
           PointClick="@OnPointClick"
           LegendClick="@OnLegendClick">

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"
                         Title="Month">
    </Chart3DPrimaryXAxis>

    <Chart3DPrimaryYAxis Title="Sales">
    </Chart3DPrimaryYAxis>

    <Chart3DLegendSettings Visible="true"></Chart3DLegendSettings>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@SalesData"
                       Name="Sales"
                       XName="Month"
                       YName="Sales"
                       Fill="#4C9AFF"
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

    public List<SalesInfo> SalesData = new List<SalesInfo>
    {
        new SalesInfo { Month = "Jan", Sales = 12000 },
        new SalesInfo { Month = "Feb", Sales = 13500 },
        new SalesInfo { Month = "Mar", Sales = 14200 },
        new SalesInfo { Month = "Apr", Sales = 15800 },
        new SalesInfo { Month = "May", Sales = 16500 },
        new SalesInfo { Month = "Jun", Sales = 17200 },
        new SalesInfo { Month = "Jul", Sales = 18000 }
    };

    private void OnPointClick(Chart3DPointClickEventArgs args)
    {
        Console.WriteLine($"Point clicked: Index = {args.PointIndex}, Series = {args.Series.Name}");
    }

    private void OnLegendClick(Chart3DLegendClickEventArgs args)
    {
        Console.WriteLine($"Legend clicked: {args.Series.Name}");
    }
}
```

### Custom Data Labels

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales with Custom Data Labels"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="7"
           TiltAngle="10"
           Depth="100">

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category">
    </Chart3DPrimaryXAxis>

    <Chart3DSeriesCollection>
        <Chart3DSeries DataSource="@Data"
                       XName="X"
                       YName="Y"
                       Type="Chart3DSeriesType.Column">

            <Chart3DDataLabel Visible="true"
                              Position="Syncfusion.Blazor.Chart3D.Chart3DDataLabelPosition.Top">
                <Chart3DDataLabelFont FontSize="12px"
                                      FontWeight="Bold"
                                      Color="Blue">
                </Chart3DDataLabelFont>

                <Chart3DDataLabelBorder Color="Red"
                                        Width="2">
                </Chart3DDataLabelBorder>
            </Chart3DDataLabel>

        </Chart3DSeries>
    </Chart3DSeriesCollection>

</SfChart3D>


@code {
    public class ChartPoint
    {
        public string X { get; set; }
        public double Y { get; set; }
    }

    public List<ChartPoint> Data = new List<ChartPoint>
    {
        new ChartPoint { X = "Jan", Y = 45 },
        new ChartPoint { X = "Feb", Y = 52 },
        new ChartPoint { X = "Mar", Y = 60 },
        new ChartPoint { X = "Apr", Y = 58 },
        new ChartPoint { X = "May", Y = 65 },
        new ChartPoint { X = "Jun", Y = 70 },
        new ChartPoint { X = "Jul", Y = 75 }
    };
}
```

### Multiple Axes

```razor
@using Syncfusion.Blazor.Chart3D

<SfChart3D Title="Sales & Profit Analysis"
           WallColor="transparent"
           EnableRotation="true"
           RotationAngle="7"
           TiltAngle="10"
           Depth="100">

    <Chart3DPrimaryXAxis ValueType="Syncfusion.Blazor.Chart3D.ValueType.Category"
                         Title="Month">
    </Chart3DPrimaryXAxis>

    <Chart3DPrimaryYAxis Title="Sales (in USD)">
    </Chart3DPrimaryYAxis>

    <Chart3DAxes>
        <Chart3DAxis Name="SecondaryYAxis"
                     Title="Profit (in USD)"
                     OpposedPosition="true">
        </Chart3DAxis>
    </Chart3DAxes>

    <Chart3DSeriesCollection>

        <!-- Sales Series (Primary Y Axis) -->
        <Chart3DSeries DataSource="@Data"
                       XName="X"
                       YName="Y1"
                       Name="Sales"
                       Type="Chart3DSeriesType.Column"
                       Fill="#4C9AFF">
        </Chart3DSeries>

        <!-- Profit Series (Secondary Y Axis) -->
        <Chart3DSeries DataSource="@Data"
                       XName="X"
                       YName="Y2"
                       YAxisName="SecondaryYAxis"
                       Name="Profit"
                       Type="Chart3DSeriesType.Column"
                       Fill="#FF8C00">
        </Chart3DSeries>

    </Chart3DSeriesCollection>
</SfChart3D>

@code {
    public class ChartData
    {
        public string X { get; set; }
        public double Y1 { get; set; }   // Sales
        public double Y2 { get; set; }   // Profit
    }

    public List<ChartData> Data = new List<ChartData>
    {
        new ChartData { X = "Jan", Y1 = 12000, Y2 = 3000 },
        new ChartData { X = "Feb", Y1 = 13500, Y2 = 3500 },
        new ChartData { X = "Mar", Y1 = 15000, Y2 = 4200 },
        new ChartData { X = "Apr", Y1 = 15800, Y2 = 4500 },
        new ChartData { X = "May", Y1 = 17000, Y2 = 4800 },
        new ChartData { X = "Jun", Y1 = 16500, Y2 = 4600 },
        new ChartData { X = "Jul", Y1 = 18000, Y2 = 5000 }
    };
}
```

## API Reference Links

For detailed API documentation, visit:
- [Syncfusion.Blazor.Chart3D Namespace](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Chart3D.html)
- [SfChart3D Class](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Chart3D.SfChart3D.html)
- [Chart3DSeries Class](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Chart3D.Chart3DSeries.html)
