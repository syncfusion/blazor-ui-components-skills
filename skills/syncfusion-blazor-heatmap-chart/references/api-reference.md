# Syncfusion Blazor HeatMap - API Reference

This document provides a comprehensive reference for the Syncfusion.Blazor.HeatMap namespace, covering all major components, classes, interfaces, and enumerations.

## Table of Contents

1. [Main Component](#main-component)
   - [SfHeatMap<TValue>](#sfheatmaptvalue)
2. [Core Settings Classes](#core-settings-classes)
   - [HeatMapCellSettings](#heatmapcellsettings)
   - [HeatMapTitleSettings](#heatmaptitlesettings)
   - [HeatMapPaletteSettings](#heatmappalettesettings)
   - [HeatMapLegendSettings](#heatmaplegendsettings)
   - [HeatMapTooltipSettings](#heatmaptooltipsettings)
   - [HeatMapDataSourceSettings](#heatmapdatasourcesettings)
3. [Axis Configuration](#axis-configuration)
   - [HeatMapXAxis](#heatmapxaxis)
   - [HeatMapYAxis](#heatmapyaxis)
   - [HeatMapAxisLabelSettings](#heatmapaxislabelsettings)
   - [HeatMapMultiLevelLabels](#heatmapmultilevellabels)
   - [HeatMapMultiLevelCategories](#heatmapmultilevelcategories)
4. [Bubble Configuration](#bubble-configuration)
   - [HeatMapBubbleDataMapping](#heatmapbubbledatamapping)
   - [HeatMapBubbleSize](#heatmapbubblesize)
5. [Styling Classes](#styling-classes)
   - [HeatMapCellBorder](#heatmapcellborder)
   - [HeatMapTitleTextStyle](#heatmaptitletextstyle)
   - [HeatMapAxisTextStyle](#heatmapaxistextstyle)
   - [HeatMapLegendTextStyle](#heatmaplegendtextstyle)
   - [HeatMapMargin](#heatmapmargin)
   - [HeatMapFont](#heatmapfont)
6. [Event Arguments](#event-arguments)
   - [CellClickEventArgs](#cellclickeventargs)
   - [CellDoubleClickEventArgs](#celldoubleclickeventargs)
   - [HeatMapCellRenderEventArgs](#heatmapcellrendereventargs)
   - [TooltipEventArgs](#tooltipeventargs)
   - [LegendRenderEventArgs](#legendrendereventargs)
7. [Enumerations](#enumerations)
   - [CellType](#celltype)
   - [PaletteType](#palettetype)
   - [AdaptorType](#adaptortype)
   - [LegendPosition](#legendposition)
   - [Alignment](#alignment)
   - [IntervalType](#intervaltype)
   - [BubbleType](#bubbletype)
   - [ValueType](#valuetype)
   - [BorderType](#bordertype)
8. [Interfaces](#interfaces)
   - [IHeatMap](#iheatmap)

---

## Main Component

### SfHeatMap<TValue>

The main component for rendering heat map visualizations in Blazor applications.

**Purpose**: Provides a complete heat map control with support for data binding, customization, and interactive features.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| DataSource | IEnumerable<TValue> | Specifies the data source for the heat map |
| Width | string | Defines the width of the heat map |
| Height | string | Defines the height of the heat map |
| CellSettings | HeatMapCellSettings | Configures cell appearance and behavior |
| TitleSettings | HeatMapTitleSettings | Configures the heat map title |
| PaletteSettings | HeatMapPaletteSettings | Configures color palette for cells |
| LegendSettings | HeatMapLegendSettings | Configures legend appearance and behavior |
| TooltipSettings | HeatMapTooltipSettings | Configures tooltip display |
| XAxis | HeatMapXAxis | Configures the X-axis |
| YAxis | HeatMapYAxis | Configures the Y-axis |
| Theme | Theme | Specifies the theme for the heat map |
| AllowSelection | bool | Enables cell selection functionality |

**Events**:

| Event | Type | Description |
|-------|------|-------------|
| CellClick | EventCallback<CellClickEventArgs> | Triggered when a cell is clicked |
| CellDoubleClicked | EventCallback<CellDoubleClickEventArgs> | Triggered when a cell is double-clicked |
| CellRendering | EventCallback<HeatMapCellRenderEventArgs> | Triggered before a cell is rendered |
| TooltipRendering | EventCallback<TooltipEventArgs> | Triggered before tooltip is rendered |
| LegendRendering | EventCallback<LegendRenderEventArgs> | Triggered before legend is rendered |
| Created | EventCallback | Triggered after the heat map is created |
| Loaded | EventCallback | Triggered after the heat map is loaded |
| CellSelected | EventCallback<SelectedEventArgs> | Triggers when multiple cells get selected in the HeatMap component. |
| Resized | EventCallback<ResizeEventArgs> | Triggers after resizing the Heatmap component. |

---

## Core Settings Classes

### HeatMapCellSettings

Configures the appearance and behavior of heat map cells.

**Purpose**: Provides fine-grained control over cell rendering, borders, labels, and data mapping.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| ShowLabel | bool | Enables or disables cell labels |
| Format | string | Specifies the format for cell values |
| EnableCellHighlighting | bool | Enables cell highlighting on hover |
| Border | HeatMapCellBorder | Configures cell border appearance |
| TextStyle | HeatMapFont | Configures cell label text style |
| TileType | CellType | Specifies the cell rendering type (Rect, Bubble) |
| BubbleType | BubbleType | Specifies bubble type when using bubble cells |

### HeatMapTitleSettings

Configures the heat map title.

**Purpose**: Controls the appearance and positioning of the main heat map title.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| Text | string | Specifies the title text |
| TextStyle | HeatMapTitleTextStyle | Configures title text appearance |
| TextAlignment | Alignment | Specifies title horizontal alignment |

### HeatMapPaletteSettings

Configures the color palette for heat map cells.

**Purpose**: Defines color mapping for different data values in the heat map.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| Palette | List<HeatMapPalette> | Collection of color palette definitions |
| Type | PaletteType | Specifies palette type (Fixed, Gradient) |

### HeatMapLegendSettings

Configures the heat map legend.

**Purpose**: Controls legend appearance, position, and behavior.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| Visible | bool | Shows or hides the legend |
| Position | LegendPosition | Specifies legend position |
| Width | string | Defines legend width |
| Height | string | Defines legend height |
| Alignment | Alignment | Specifies legend alignment |
| ShowLabel | bool | Shows or hides legend labels |
| ShowGradientPointer | bool | Shows gradient pointer in legend |
| LabelFormat | string | Specifies format for legend labels |
| TextStyle | HeatMapLegendTextStyle | Configures legend text style |
| Text | HeatMapLegendTitle | Configures legend title |
| EnableSmartLegend | bool | Enables smart legend mode |
| LabelDisplayType | LabelDisplayType | Specifies how labels are displayed |
| ToggleVisibility | bool | Enables legend click to toggle visibility |

### HeatMapTooltipSettings

Configures tooltip display for heat map cells.

**Purpose**: Controls tooltip appearance and content when hovering over cells.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| Fill | string | Specifies tooltip background color |
| Border | HeatMapTooltipBorder | Configures tooltip border |
| TextStyle | HeatMapFont | Configures tooltip text style |
| Template | RenderFragment | Specifies custom tooltip template |

### HeatMapDataSourceSettings

Configures data source mapping for the heat map.

**Purpose**: Defines how data from the source is mapped to heat map cells.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| IsJsonData | bool | Indicates if data source is JSON |
| AdaptorType | AdaptorType | Specifies the data adaptor type |
| XDataMapping | string | Maps X-axis data field |
| YDataMapping | string | Maps Y-axis data field |
| ValueMapping | string | Maps cell value field |

---

## Axis Configuration

### HeatMapXAxis

Configures the X-axis of the heat map.

**Purpose**: Controls X-axis appearance, labels, and behavior.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| AxisLabels | List<string> | Collection of X-axis labels |
| Title | HeatMapAxisTitle | Configures axis title |
| LabelRotation | int | Specifies label rotation angle |
| LabelIntersectAction | LabelIntersectAction | Defines action for intersecting labels |
| TextStyle | HeatMapAxisTextStyle | Configures axis label text style |
| ValueType | ValueType | Specifies axis value type |
| Minimum | object | Specifies minimum axis value |
| Maximum | object | Specifies maximum axis value |
| Interval | double | Specifies axis interval |
| IntervalType | IntervalType | Specifies interval type for date-time axes |
| LabelFormat | string | Specifies format for axis labels |
| ShowLabelOn | LabelDisplayType | Specifies when to show labels |
| IsInversed | bool | Inverts the axis direction |
| MultiLevelLabels | List<HeatMapMultiLevelLabel> | Collection of multi-level labels |

### HeatMapYAxis

Configures the Y-axis of the heat map.

**Purpose**: Controls Y-axis appearance, labels, and behavior.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| AxisLabels | List<string> | Collection of Y-axis labels |
| Title | HeatMapAxisTitle | Configures axis title |
| LabelRotation | int | Specifies label rotation angle |
| LabelIntersectAction | LabelIntersectAction | Defines action for intersecting labels |
| TextStyle | HeatMapAxisTextStyle | Configures axis label text style |
| ValueType | ValueType | Specifies axis value type |
| Minimum | object | Specifies minimum axis value |
| Maximum | object | Specifies maximum axis value |
| Interval | double | Specifies axis interval |
| IntervalType | IntervalType | Specifies interval type for date-time axes |
| LabelFormat | string | Specifies format for axis labels |
| ShowLabelOn | LabelDisplayType | Specifies when to show labels |
| IsInversed | bool | Inverts the axis direction |
| MultiLevelLabels | List<HeatMapMultiLevelLabel> | Collection of multi-level labels |

### HeatMapAxisLabelSettings

Configures axis label appearance and behavior.

**Purpose**: Provides detailed control over axis label rendering.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| TextStyle | HeatMapAxisTextStyle | Configures label text style |
| LabelRotation | int | Specifies label rotation angle |
| LabelIntersectAction | LabelIntersectAction | Defines action for intersecting labels |

### HeatMapMultiLevelLabels

Configures multi-level labels for axes.

**Purpose**: Enables grouping of axis labels with hierarchical categories.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| Categories | List<HeatMapMultiLevelCategories> | Collection of category definitions |
| Border | HeatMapBorder | Configures multi-level label border |
| TextStyle | HeatMapAxisTextStyle | Configures multi-level label text style |
| Overflow | Overflow | Specifies overflow behavior |
| Alignment | Alignment | Specifies label alignment |

### HeatMapAxisMultiLevelCategories

Defines individual categories for multi-level labels.

**Purpose**: Specifies start, end, and text for each category in multi-level labels.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| Start | int | Specifies category start position |
| End | int | Specifies category end position |
| Text | string | Specifies category text |
| MaximumTextWidth | int | Specifies maximum text width |

---

## Bubble Configuration

### HeatMapBubbleDataMapping

Configures data mapping for bubble heat maps.

**Purpose**: Maps data fields to bubble size and color properties.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| Size | string | Maps data field to bubble size |
| Color | string | Maps data field to bubble color |

### HeatMapBubbleSize

Configures bubble size properties.

**Purpose**: Controls minimum and maximum bubble sizes in bubble heat maps.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| Minimum | string | Specifies minimum bubble size (percentage) |
| Maximum | string | Specifies maximum bubble size (percentage) |

---

## Styling Classes

### HeatMapCellBorder

Configures cell border appearance.

**Purpose**: Controls border width, color, and radius for heat map cells.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| Width | int | Specifies border width |
| Color | string | Specifies border color |
| Radius | int | Specifies border radius |

### HeatMapTitleTextStyle

Configures title text styling.

**Purpose**: Controls font, size, color, and other text properties for the title.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| Size | string | Specifies font size |
| Color | string | Specifies text color |
| FontFamily | string | Specifies font family |
| FontWeight | string | Specifies font weight |
| FontStyle | string | Specifies font style |
| TextAlignment | Alignment | Specifies text alignment |
| TextOverflow | TextOverflow | Specifies text overflow behavior |

### HeatMapAxisTextStyle

Configures axis label text styling.

**Purpose**: Controls font, size, color, and other text properties for axis labels.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| Size | string | Specifies font size |
| Color | string | Specifies text color |
| FontFamily | string | Specifies font family |
| FontWeight | string | Specifies font weight |
| FontStyle | string | Specifies font style |
| TextAlignment | Alignment | Specifies text alignment |
| TextOverflow | TextOverflow | Specifies text overflow behavior |

### HeatMapLegendTextStyle

Configures legend text styling.

**Purpose**: Controls font, size, color, and other text properties for legend labels.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| Size | string | Specifies font size |
| Color | string | Specifies text color |
| FontFamily | string | Specifies font family |
| FontWeight | string | Specifies font weight |
| FontStyle | string | Specifies font style |

### HeatMapMargin

Configures margin spacing.

**Purpose**: Defines spacing around heat map elements.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| Left | int | Specifies left margin |
| Right | int | Specifies right margin |
| Top | int | Specifies top margin |
| Bottom | int | Specifies bottom margin |

### HeatMapFont

Configures general font properties.

**Purpose**: Provides basic font styling for various heat map elements.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| Size | string | Specifies font size |
| Color | string | Specifies text color |
| FontFamily | string | Specifies font family |
| FontWeight | string | Specifies font weight |
| FontStyle | string | Specifies font style |

---

## Event Arguments

### CellClickEventArgs

Event arguments for cell click events.

**Purpose**: Provides information about the clicked cell.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| XLabel | string | X-axis label of the clicked cell |
| YLabel | string | Y-axis label of the clicked cell |
| XValue | object | X-axis value of the clicked cell |
| YValue | object | Y-axis value of the clicked cell |
| Value | double | Data value of the clicked cell |
| Cancel | bool | Allows canceling the event |

### CellDoubleClickEventArgs

Event arguments for cell double-click events.

**Purpose**: Provides information about the double-clicked cell.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| XLabel | string | X-axis label of the double-clicked cell |
| YLabel | string | Y-axis label of the double-clicked cell |
| XValue | object | X-axis value of the double-clicked cell |
| YValue | object | Y-axis value of the double-clicked cell |
| Value | double | Data value of the double-clicked cell |
| Cancel | bool | Allows canceling the event |

### HeatMapCellRenderEventArgs

Event arguments for cell rendering events.

**Purpose**: Allows customization of cells before they are rendered.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| XLabel | string | X-axis label of the cell being rendered |
| YLabel | string | Y-axis label of the cell being rendered |
| XValue | object | X-axis value of the cell being rendered |
| YValue | object | Y-axis value of the cell being rendered |
| Value | double | Data value of the cell being rendered |
| CellColor | string | Color of the cell (can be modified) |
| DisplayText | string | Display text for the cell (can be modified) |
| Cancel | bool | Allows canceling the rendering |

### TooltipEventArgs

Event arguments for tooltip rendering events.

**Purpose**: Allows customization of tooltip content before display.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| XLabel | string | X-axis label of the cell |
| YLabel | string | Y-axis label of the cell |
| XValue | object | X-axis value of the cell |
| YValue | object | Y-axis value of the cell |
| Value | double | Data value of the cell |
| Content | string | Tooltip content (can be modified) |
| Cancel | bool | Allows canceling the tooltip display |

### LegendRenderEventArgs

Event arguments for legend rendering events.

**Purpose**: Allows customization of legend items before they are rendered.

**Key Properties**:

| Property | Type | Description |
|----------|------|-------------|
| Text | string | Legend item text (can be modified) |
| Cancel | bool | Allows canceling the rendering |

---

## Enumerations

### CellType

Specifies the type of cell rendering.

**Purpose**: Determines how cells are visually represented in the heat map.

**Values**:

| Value | Description |
|-------|-------------|
| Rect | Renders cells as rectangles (default) |
| Bubble | Renders cells as bubbles with varying sizes |

### PaletteType

Specifies the palette type for color mapping.

**Purpose**: Determines how colors are distributed across cell values.

**Values**:

| Value | Description |
|-------|-------------|
| Fixed | Uses fixed color ranges for specific value ranges |
| Gradient | Uses smooth color gradients between value ranges |

### AdaptorType

Specifies the data adaptor type for data binding.

**Purpose**: Determines how data is processed from the data source.

**Values**:

| Value | Description |
|-------|-------------|
| Cell | Data is organized in cell format (X, Y, Value) |
| Table | Data is organized in table format |
| None | Data is organized in datasource format |

### LegendPosition

Specifies the position of the legend.

**Purpose**: Determines where the legend is displayed relative to the heat map.

**Values**:

| Value | Description |
|-------|-------------|
| Left | Legend positioned on the left side |
| Right | Legend positioned on the right side |
| Top | Legend positioned at the top |
| Bottom | Legend positioned at the bottom |

### Alignment

Specifies horizontal alignment.

**Purpose**: Controls alignment of text and elements.

**Values**:

| Value | Description |
|-------|-------------|
| Near | Aligns to the start (left or top) |
| Center | Aligns to the center |
| Far | Aligns to the end (right or bottom) |

### IntervalType

Specifies interval type for date-time axes.

**Purpose**: Determines the unit of intervals for date-time data.

**Values**:

| Value | Description |
|-------|-------------|
| Years | Intervals in years |
| Months | Intervals in months |
| Days | Intervals in days |
| Hours | Intervals in hours |
| Minutes | Intervals in minutes |

### BubbleType

Specifies bubble rendering type.

**Purpose**: Determines how bubbles are sized and rendered.

**Values**:

| Value | Description |
|-------|-------------|
| Size | Bubble size based on value |
| Color | Bubble color based on value |
| Sector | Bubble rendered as sector/pie segment |
| SizeAndColor | Both size and color based on values |

### ValueType

Specifies the value type for axis data.

**Purpose**: Determines how axis values are interpreted and formatted.

**Values**:

| Value | Description |
|-------|-------------|
| Numeric | Numeric values |
| DateTime | Date and time values |
| Category | Categorical values |

### BorderType

Specifies border rendering type.

**Purpose**: Determines which borders are rendered for cells.

**Values**:

| Value | Description |
|-------|-------------|
| Rectangle | All four borders (default) |
| WithoutTopBorder | Only WithoutTopBorder type |
| WithoutBottomBorder | Only WithoutBottomBorder type |
| WithoutBorder | Only WithoutBorder type. |
| WithoutTopandBottomBorder | Only WithoutTopandBottomBorder type. |
| Brace | Only Brace type border |

---

## Interfaces

### IHeatMap

Interface for heat map component interaction.

**Purpose**: Provides programmatic access to heat map methods and properties.

**Key Methods**:

| Method | Return Type | Description |
|--------|-------------|-------------|
| ExportAsync(ExportType, string) | Task | Exports heat map to specified format |
| PrintAsync() | Task | Prints the heat map |
| ClearSelectionAsync() | Task | Clears all cell selections |

---

## Usage Notes

### Component Initialization

```razor
<SfHeatMap DataSource="@HeatMapData"
           XAxis="@XAxisSettings"
           YAxis="@YAxisSettings">
    <HeatMapTitleSettings Text="Sales Analysis"></HeatMapTitleSettings>
    <HeatMapCellSettings ShowLabel="true"></HeatMapCellSettings>
</SfHeatMap>
```

### Event Handling

All event callbacks follow Blazor's EventCallback pattern and can be used to customize behavior:

```razor
<SfHeatMap>
   <HeatMapEvents CellClicked="OnCellClicked" CellRendering="@OnCellRendering"></HeatMapEvents>
</SfHeatMap>

@code {
    private void OnCellClicked(CellClickEventArgs args)
    {
        // Handle cell click
    }
    
    private void OnCellRendering(HeatMapCellRenderEventArgs args)
    {
        // Customize cell appearance
    }
}
```

### Data Binding

The heat map supports two main data adaptor types:

1. **Cell Adaptor**: Data in (X, Y, Value) format
2. **Table Adaptor**: Data in table/matrix format

Configure using `HeatMapDataSourceSettings`:

```razor
<HeatMapDataSourceSettings AdaptorType="AdaptorType.Cell"
                          XDataMapping="XValue"
                          YDataMapping="YValue"
                          ValueMapping="Value">
</HeatMapDataSourceSettings>
```

### Styling and Theming

All styling classes support cascading and can be nested within their parent settings:

```razor
<HeatMapTitleSettings Text="Title">
    <HeatMapTitleTextStyle Size="18px" Color="blue" FontWeight="bold">
    </HeatMapTitleTextStyle>
</HeatMapTitleSettings>
```

---

## Additional Resources

For complete implementation examples and detailed guides, refer to the other reference files in this documentation:
- Getting Started Guide
- Data Binding Examples
- Customization Techniques
- Performance Optimization
