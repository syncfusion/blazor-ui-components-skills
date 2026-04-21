````markdown
# API Reference - Range Navigator

Comprehensive API documentation for Syncfusion Blazor Range Navigator (SfRangeNavigator) component. This reference provides detailed information about classes, properties, methods, and events.

## Table of Contents

- [Main Component](#main-component)
    - [SfRangeNavigator](#sfrangenavigator)
- [Series Configuration](#series-configuration)
    - [RangeNavigatorSeriesCollection](#rangenavigatorseriescollection)
    - [RangeNavigatorSeries](#rangenavigatorseries)
    - [RangeNavigatorSeriesBorder](#rangenavigatorseriesborder)
- [Tooltip Configuration](#tooltip-configuration)
    - [RangeNavigatorRangeTooltipSettings](#rangenavigatorrangetooltipsettings)
    - [RangeNavigatorTooltipBorder](#rangenavigatortooltipborder)
    - [RangeNavigatorTooltipTextStyle](#rangenavigatortooltiptextstyle)
- [Period Selector Configuration](#period-selector-configuration)
    - [RangeNavigatorPeriodSelectorSettings](#rangenavigatorperiodselectorsettings)
    - [RangeNavigatorPeriods](#rangenavigatorperiods)
    - [RangeNavigatorPeriod](#rangenavigatorperiod)
- [Axis Customization](#axis-customization)
    - [RangeNavigatorMargin](#rangenavigatormargin)
    - [RangeNavigatorBorder](#rangenavigatorborder)
    - [RangeNavigatorStyleSettings](#rangenavigatorstylesettings)
    - [RangeNavigatorMajorGridLines](#rangenavigatormajorgridlines)
    - [RangeNavigatorMajorTickLines](#rangenavigatormajorticklines)
    - [RangeNavigatorLabelStyle](#rangenavigatorlabelstyle)
- [Thumb Configuration](#thumb-configuration)
    - [RangeNavigatorThumbSettings](#rangenavigatorthumbsettings)
    - [RangeNavigatorThumbBorder](#rangenavigatorthumbborder)
- [Animation Settings](#animation-settings)
    - [RangeNavigatorAnimation](#rangenavigatoranimation)
- [Event Arguments](#event-arguments)
    - [ChangedEventArgs](#changedeventargs)
    - [RangeResizeEventArgs](#rangeresizeeventargs)
    - [RangeLoadedEventArgs](#rangeloadedeventargs)
    - [RangeSelectorRenderEventArgs](#rangeselectorrendereventargs)
    - [RangeTooltipRenderEventArgs](#rangetooltiprendereventargs)
    - [RangeLabelRenderEventArgs](#rangelabelrendereventargs)
- [Enums](#enums)
- [Data Models](#data-models)
    - [VisibleRangeModel](#visiblerangemodel)
- [Best Practices](#best-practices)
- [Related Classes](#related-classes)

## Main Component

### SfRangeNavigator

The primary Range Navigator component for data range selection and navigation.

**Namespace:** `Syncfusion.Blazor.Charts`

**Key Properties:**

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `Value` | `DateTime[]` or `double[]` | Selected range values [start, end] | null |
| `ValueType` | `RangeValueType` | Data type: DateTime, Double, or Logarithmic | Double |
| `DataSource` | `object` | Data collection for the series | null |
| `Width` | `string` | Component width | "100%" |
| `Height` | `string` | Component height | "80px" |
| `Theme` | `Theme` | Visual theme (Material, Bootstrap5, Fluent, Tailwind, Fabric, HighContrast) | Material |
| `Interval` | `int` | Axis interval value | 1 |
| `IntervalType` | `RangeIntervalType` | Interval type (Auto, Years, Months, Days, Hours, Minutes, Seconds) | Auto |
| `LabelFormat` | `string` | Format string for labels (e.g., "MMM-yy", "dd/MM/yyyy") | null |
| `LogBase` | `double` | Base for logarithmic axis | 10 |
| `Orientation` | `Orientation` | Horizontal or Vertical | Horizontal |
| `AllowSnapping` | `bool` | Snap thumbs to data points | false |
| `EnableGrouping` | `bool` | Enable data grouping | false |
| `EnableRtl` | `bool` | Right-to-Left support | false |
| `Margin` | `RangeNavigatorMargin` | Margin settings | - |
| `Border` | `RangeNavigatorBorder` | Border customization | - |
| `TabIndex` | `int` | Tab index for keyboard navigation | 0 |

**Methods:**

| Method | Return Type | Description |
|--------|------------|-------------|
| `ExportAsync(ExportType, string)` | `Task` | Export chart as PNG, JPEG, SVG, or PDF |
| `Print()` | `Task` | Print the Range Navigator |
| `Refresh()` | `Task` | Refresh/redraw the component |
| `GetVisibleRangeModel()` | `VisibleRangeModel` | Get current visible range model |

**Events:**

| Event | Event Args | Description |
|-------|-----------|-------------|
| `Changed` | `ChangedEventArgs` | Fires when range selection changes |
| `Loaded` | `RangeLoadedEventArgs` | Fires after component loads |
| `Resizing` | `RangeResizeEventArgs` | Fires during thumb resize |
| `Resized` | `RangeResizeEventArgs` | Fires after thumb resize completes |
| `Rendering` | `RangeSelectorRenderEventArgs` | Fires before rendering |
| `TooltipRender` | `RangeTooltipRenderEventArgs` | Fires before tooltip renders |
| `LabelRender` | `RangeLabelRenderEventArgs` | Fires when labels render |
| `OnCrosshairMove` | `CrosshairMoveEventArgs` | Fires on crosshair movement |

---

## Series Configuration

### RangeNavigatorSeriesCollection

Collection container for Range Navigator series.

**Namespace:** `Syncfusion.Blazor.Charts`

**Usage:**
```razor
<RangeNavigatorSeriesCollection>
    <RangeNavigatorSeries DataSource="@Data" XName="Date" YName="Value" />
</RangeNavigatorSeriesCollection>
```

---

### RangeNavigatorSeries

Defines a single series in the Range Navigator.

**Key Properties:**

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `DataSource` | `object` | Data collection | null |
| `XName` | `string` | X-axis data field | null |
| `YName` | `string` | Y-axis data field | null |
| `Type` | `RangeNavigatorType` | Series type: Line, Area, or StepLine | Line |
| `Fill` | `string` | Series color | null |
| `Width` | `double` | Line width in pixels | 1 |
| `Query` | `DataManagerRequest` | Data manager query | null |
| `Name` | `string` | Series name | null |
| `Opacity` | `double` | Series opacity (0-1) | 1 |

**Child Components:**
- `RangeNavigatorSeriesBorder` - Series border settings

---

### RangeNavigatorSeriesBorder

Customizes the border of Range Navigator series.

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `Color` | `string` | Border color |
| `Width` | `double` | Border width in pixels |

---

## Tooltip Configuration

### RangeNavigatorRangeTooltipSettings

Configures the tooltip displayed during range selection.

**Properties:**

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `Enable` | `bool` | Enable/disable tooltip | true |
| `DisplayMode` | `TooltipDisplayMode` | Display mode: OnDemand or Always | OnDemand |
| `Fill` | `string` | Tooltip background color | null |
| `Opacity` | `double` | Tooltip opacity (0-1) | 1 |
| `RoundedCornerRadius` | `double` | Corner radius | 5 |
| `Format` | `string` | Tooltip content format template | null |

**Child Components:**
- `RangeNavigatorTooltipBorder` - Tooltip border settings
- `RangeNavigatorTooltipTextStyle` - Tooltip text styling

---

### RangeNavigatorTooltipBorder

Customizes tooltip border appearance.

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `Color` | `string` | Border color |
| `Width` | `double` | Border width in pixels |

---

### RangeNavigatorTooltipTextStyle

Customizes tooltip text appearance.

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `Color` | `string` | Text color |
| `FontFamily` | `string` | Font family name |
| `FontSize` | `string` | Font size (e.g., "14px") |
| `FontStyle` | `string` | Font style (normal, italic, oblique) |
| `FontWeight` | `string` | Font weight (normal, bold, 100-900) |
| `Opacity` | `double` | Text opacity (0-1) |
| `TextAlignment` | `Alignment` | Text alignment |

---

## Period Selector Configuration

### RangeNavigatorPeriodSelectorSettings

Enables and configures period selector buttons.

**Properties:**

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `Enabled` | `bool` | Enable period selector | true |
| `Height` | `double` | Height in pixels | 32 |
| `Position` | `PeriodSelectorPosition` | Position: Top or Bottom | Bottom |
| `Intervals` | `IntervalType[]` | Interval types to display | Months, Years |

**Child Components:**
- `RangeNavigatorPeriods` - Period button collection

---

### RangeNavigatorPeriods

Container for period selector buttons.

**Usage:**
```razor
<RangeNavigatorPeriods>
    <RangeNavigatorPeriod Text="1M" Interval="1" IntervalType="RangeIntervalType.Months" />
    <RangeNavigatorPeriod Text="3M" Interval="3" IntervalType="RangeIntervalType.Months" />
    <RangeNavigatorPeriod Text="All" />
</RangeNavigatorPeriods>
```

---

### RangeNavigatorPeriod

Defines a single period selector button.

**Properties:**

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `Text` | `string` | Button label text | null |
| `Interval` | `int` | Interval value | 1 |
| `IntervalType` | `RangeIntervalType` | Interval type (Years, Months, Days, Hours, Minutes, Seconds) | Months |
| `Selected` | `bool` | Whether button is selected by default | false |

**Predefined Values:**
- "1M" - 1 Month
- "3M" - 3 Months
- "6M" - 6 Months
- "YTD" - Year-to-Date
- "1Y" - 1 Year
- "All" - All data

---

## Axis Customization

### RangeNavigatorMargin

Customizes component margins.

**Properties:**

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `Left` | `double` | Left margin in pixels | 10 |
| `Right` | `double` | Right margin in pixels | 10 |
| `Top` | `double` | Top margin in pixels | 10 |
| `Bottom` | `double` | Bottom margin in pixels | 10 |

---

### RangeNavigatorBorder

Customizes component border.

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `Color` | `string` | Border color |
| `Width` | `double` | Border width in pixels |
| `DashArray` | `string` | Dash pattern (e.g., "5,5") |

---

### RangeNavigatorStyleSettings

Style configuration for Range Navigator.

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `SelectedRegionColor` | `string` | Color of selected range area |
| `UnselectedRegionColor` | `string` | Color of unselected range area |

---

### RangeNavigatorMajorGridLines

Customizes major gridlines.

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `Width` | `double` | Line width in pixels |
| `Color` | `string` | Line color |
| `DashArray` | `string` | Dash pattern |

---

### RangeNavigatorMajorTickLines

Customizes major tick lines.

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `Width` | `double` | Tick line width |
| `Color` | `string` | Tick line color |
| `Height` | `double` | Tick line height |

---

### RangeNavigatorLabelStyle

Customizes axis label appearance.

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `Color` | `string` | Label color |
| `FontFamily` | `string` | Font family |
| `Size` | `string` | Font size |
| `FontWeight` | `string` | Font weight |
| `Opacity` | `double` | Label opacity |

---

## Thumb Configuration

### RangeNavigatorThumbSettings

Customizes the range selection thumbs.

**Properties:**

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `Type` | `ThumbType` | Thumb shape: Circle or Rectangle | ThumbType.Circle |
| `Fill` | `string` | Thumb fill color | null |
| `Width` | `double` | width of thumb| double.NaN |
| `Height` | `double` | height of thumb| double.NaN |

**Child Components:**
- `RangeNavigatorThumbBorder` - Thumb border settings

---

### RangeNavigatorThumbBorder

Customizes thumb border.

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `Color` | `string` | Border color |
| `Width` | `double` | Border width |

---

## Animation Settings

### RangeNavigatorAnimation

Configures thumb animation during selection.

**Properties:**

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `Enable` | `bool` | Enable animation | false |
| `Duration` | `double` | Animation duration in milliseconds | 500 |
| `Delay` | `double` | Animation delay in milliseconds | 0 |

---

## Event Arguments

### ChangedEventArgs

Fired when range selection changes.

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `Start` | `DateTime` or `double` | Start value of selected range |
| `End` | `DateTime` or `double` | End value of selected range |
| `Value` | `Array` | Selected range as array |
| `SelectedData` | `IEnumerable` | Selected data from series |

**Example:**
```csharp
private void OnRangeChanged(ChangedEventArgs args)
{
    var startDate = args.Start;
    var endDate = args.End;
    StateHasChanged();
}
```

---

### RangeResizeEventArgs

Fired during or after thumb resize.

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `Start` | `DateTime` or `double` | New start value |
| `End` | `DateTime` or `double` | New end value |
| `IsMoved` | `bool` | Whether thumb was moved |
| `SelectedData` | `IEnumerable` | Selected data |

---

### RangeLoadedEventArgs

Fired after Range Navigator loads.

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `Name` | `string` | Event name |
| `Cancel` | `bool` | Cancel event |
| `AvailableSize` | `Size` | Available size |

---

### RangeSelectorRenderEventArgs

Fired before selector rendering.

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `Name` | `string` | Event name |
| `Period` | `string` | Selected period |
| `Cancel` | `bool` | Cancel rendering |

---

### RangeTooltipRenderEventArgs

Fired before tooltip rendering.

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `Name` | `string` | Event name |
| `Text` | `string` | Tooltip text |
| `Content` | `string` | Tooltip content |
| `Location` | `Point` | Tooltip position |
| `Cancel` | `bool` | Cancel tooltip |

---

### RangeLabelRenderEventArgs

Fired when labels render.

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `Name` | `string` | Event name |
| `Value` | `object` | Label value |
| `Text` | `string` | Label text |
| `LabelStyle` | `RangeNavigatorLabelStyle` | Label style |
| `Cancel` | `bool` | Cancel rendering |

---

## Enums

### RangeValueType

```csharp
public enum RangeValueType
{
    Double,        // Numeric values
    DateTime,      // Date/time values
    Logarithmic    // Logarithmic scale
}
```

---

### RangeNavigatorType

```csharp
public enum RangeNavigatorType
{
    Line,          // Line series (default)
    Area,          // Filled area
    StepLine       // Step line series
}
```

---

### RangeIntervalType

```csharp
public enum RangeIntervalType
{
    Auto,
    Years,
    Months,
    Days,
    Hours,
    Minutes,
    Seconds
}
```

---

### PeriodSelectorPosition

```csharp
public enum PeriodSelectorPosition
{
    Top,           // Period selector at top
    Bottom         // Period selector at bottom (default)
}
```

---

### ThumbType

```csharp
public enum ThumbType
{
    Circle,        // Circular thumb (default)
    Rectangle      // Rectangular thumb
}
```

---

### TooltipDisplayMode

```csharp
public enum TooltipDisplayMode
{
    OnDemand,      // Show on hover only
    Always         // Always visible
}
```

---

### Orientation

```csharp
public enum Orientation
{
    Horizontal,    // Horizontal layout (default)
    Vertical       // Vertical layout
}
```

---

## Data Models

### VisibleRangeModel

Represents the visible range of data.

**Properties:**

| Property | Type | Description |
|----------|------|-------------|
| `Start` | `DateTime` or `double` | Range start value |
| `End` | `DateTime` or `double` | Range end value |
| `IsUpdated` | `bool` | Whether range was updated |

---

## Best Practices

### Property Binding
- Use `@bind-Value` for two-way binding with range changes
- Use `Value="@variable"` for one-way binding

### Data Binding
- Set `DataSource` to your data collection
- Use `XName` and `YName` to map data fields
- Ensure data is in chronological order for DateTime values

### Performance
- Use `EnableGrouping="true"` for large datasets
- Set appropriate `IntervalType` to reduce label clutter
- Consider `AllowSnapping="true"` to improve interaction

### Events
- Handle `Changed` event to sync with other components
- Use `Loaded` event for post-initialization logic
- Use `TooltipRender` to customize tooltip content dynamically

---

## Related Classes

- `SfChart` - Main chart component (integrates with Range Navigator)
- `ChartPrimaryXAxis` - X-axis configuration
- `ChartPrimaryYAxis` - Y-axis configuration
- `RangeNavigatorEvents` - Event handler definitions
- `RangeNavigatorAnimation` - Animation settings

````