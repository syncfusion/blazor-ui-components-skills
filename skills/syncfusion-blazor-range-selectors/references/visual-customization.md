# Visual Customization

Learn how to customize the appearance of Syncfusion Blazor Range Selector including dimensions, themes, tooltips, and responsive design.

## API Reference - Visual Customization

**Related Classes & Components:**
- `RangeNavigatorStyleSettings` - Style configuration
- `RangeNavigatorThumbSettings` - Thumb appearance (Type, Fill, Size)
- `RangeNavigatorThumbBorder` - Thumb border (Color, Width)
- `RangeNavigatorRangeTooltipSettings` - Tooltip configuration
- `RangeNavigatorTooltipBorder` - Tooltip border styling
- `RangeNavigatorTooltipTextStyle` - Tooltip text styling

**Enum References:**
- `Theme` - Material, Bootstrap5, Fluent, Tailwind, Fabric, HighContrast
- `ThumbType` - Circle (default), Rectangle
- `TooltipDisplayMode` - OnDemand (default), Always

**Visual Properties:**
- `Width` - string - Component width (default: "100%")
- `Height` - string - Component height (default: "80px")
- `Theme` - Theme - Visual theme
- `Margin` - RangeNavigatorMargin - Spacing
- `Border` - RangeNavigatorBorder - Border customization

**Thumb Properties:**
- `RangeNavigatorThumbSettings.Type` - ThumbType (Circle or Rectangle)
- `RangeNavigatorThumbSettings.Fill` - Thumb color (default: "#f3f3f3")
- `RangeNavigatorThumbSettings.Size` - Thumb size in pixels (default: 15)

For complete API details, see [api-reference.md](api-reference.md).

## Table of Contents

- [Overview](#overview)
- [Chart Dimensions](#chart-dimensions)
- [Themes](#themes)
- [Tooltips](#tooltips)
- [Colors and Styling](#colors-and-styling)
- [Responsive Design](#responsive-design)
- [Performance Optimization](#performance-optimization)

## Overview
Customize visual appearance using `Width`, `Height`, `Theme`, `RangeNavigatorStyleSettings`, `RangeNavigatorRangeTooltipSettings`, and responsive properties for complete styling control.

The Range Selector provides extensive customization options for visual appearance, including size, themes, colors, tooltips, and responsive behavior.

## Chart Dimensions
Control component size using `Width` and `Height` properties with pixel or percentage values, and adjust spacing with `RangeNavigatorMargin`.

### Width and Height
Control the size of your range selector using `Width` and `Height` properties:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  Width="100%"
                  Height="150px">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@Data" XName="Date" YName="Value">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public DateTime[] Range;
    public List<DataPoint> Data = GetData();
}
```

### Fixed Width
Set fixed `Width` and `Height` values in pixels:

```razor
<SfRangeNavigator Width="800px" Height="120px" ...>
```

### Percentage Width
Use percentage `Width` for responsive sizing:

```razor
<SfRangeNavigator Width="100%" Height="150px" ...>
```

### Margin
Add spacing around the range selector using `RangeNavigatorMargin` component with Top, Bottom, Left, Right properties:

```razor
<SfRangeNavigator @bind-Value="@Range" ValueType="RangeValueType.DateTime">
    <RangeNavigatorMargin Top="10" Bottom="10" Left="20" Right="20"></RangeNavigatorMargin>
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@Data" XName="Date" YName="Value">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>
```

## Themes
Apply visual themes using the `Theme` property with predefined Theme enum values (Material, Bootstrap5, Fluent, Tailwind, Fabric, HighContrast).

### Available Themes
Syncfusion provides 6 built-in themes. Change themes by updating the stylesheet reference and setting `Theme` property:

#### Material Theme
Apply Material theme using `Theme="Theme.Material"` property:

```html
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />
```

```razor
<SfRangeNavigator Theme="Theme.Material" ...>
```

#### Bootstrap 5 Theme
Apply Bootstrap5 theme using `Theme="Theme.Bootstrap5"` property:

```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```

```razor
<SfRangeNavigator Theme="Theme.Bootstrap5" ...>
```

#### Fluent Theme
Apply Fluent theme using `Theme="Theme.Fluent"` property:

```html
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />
```

```razor
<SfRangeNavigator Theme="Theme.Fluent" ...>
```

#### Tailwind Theme
Apply Tailwind theme using `Theme="Theme.Tailwind"` property:

```html
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />
```

```razor
<SfRangeNavigator Theme="Theme.Tailwind" ...>
```

#### Fabric Theme
Apply Fabric theme using `Theme="Theme.Fabric"` property:

```html
<link href="_content/Syncfusion.Blazor.Themes/fabric.css" rel="stylesheet" />
```

```razor
<SfRangeNavigator Theme="Theme.Fabric" ...>
```

#### High Contrast Theme
Apply HighContrast theme using `Theme="Theme.HighContrast"` property:

```html
<link href="_content/Syncfusion.Blazor.Themes/highcontrast.css" rel="stylesheet" />
```

```razor
<SfRangeNavigator Theme="Theme.HighContrast" ...>
```

### Theme Comparison Example
Switch themes dynamically using `@bind` with `Theme` property:

```razor
@page "/theme-demo"
@using Syncfusion.Blazor.Charts

<div class="theme-selector">
    <label>Select Theme:</label>
    <select @bind="SelectedTheme" class="form-select">
        <option value="@Theme.Material">Material</option>
        <option value="@Theme.Bootstrap5">Bootstrap 5</option>
        <option value="@Theme.Fluent">Fluent</option>
        <option value="@Theme.Tailwind">Tailwind</option>
        <option value="@Theme.Fabric">Fabric</option>
        <option value="@Theme.HighContrast">High Contrast</option>
    </select>
</div>

<SfRangeNavigator Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  Theme="@SelectedTheme"
                  LabelFormat="MMM-yy">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@StockData" 
                              XName="Date" 
                              YName="Close"
                              Type="RangeNavigatorType.Area">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public DateTime[] Range = new DateTime[] { DateTime.Now.AddMonths(-6), DateTime.Now };
    public Theme SelectedTheme = Theme.Material;
    public List<StockInfo> StockData = GenerateData();
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }
    private static List<StockInfo> GenerateData()
    {
        var data = new List<StockInfo>();
        var random = new Random();

        DateTime startDate = DateTime.Now.AddYears(-1);
        double price = 120;

        for (int i = 0; i < 365; i++)
        {
            // Simulate daily price movement
            price += random.NextDouble() * 4 - 2;
            price = Math.Max(price, 50); // prevent negative prices

            data.Add(new StockInfo
            {
                Date = startDate.AddDays(i),
                Close = Math.Round(price, 2)
            });
        }

        return data;
    }
}
```

## Tooltips
Configure tooltip behavior using `RangeNavigatorRangeTooltipSettings` with `Enable`, `DisplayMode`, `Format`, and styling components.

### Enable Tooltips
Enable tooltips using `RangeNavigatorRangeTooltipSettings` component with `Enable="true"`:

```razor
<SfRangeNavigator @bind-Value="@Range" ValueType="RangeValueType.DateTime">
    <RangeNavigatorRangeTooltipSettings Enable="true"></RangeNavigatorRangeTooltipSettings>
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@Data" XName="Date" YName="Value">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>
```

### Always Show Tooltips
Set `DisplayMode="TooltipDisplayMode.Always"` to show tooltips permanently:

```razor
<RangeNavigatorRangeTooltipSettings Enable="true" 
                                    DisplayMode="TooltipDisplayMode.Always">
</RangeNavigatorRangeTooltipSettings>
```

### Show on Demand
Set `DisplayMode="TooltipDisplayMode.OnDemand"` to show tooltips only on interaction:

```razor
<RangeNavigatorRangeTooltipSettings Enable="true" 
                                    DisplayMode="TooltipDisplayMode.OnDemand">
</RangeNavigatorRangeTooltipSettings>
```

### Custom Tooltip Format
Customize tooltip date format using `Format` property:

```razor
<RangeNavigatorRangeTooltipSettings Enable="true" 
                                    Format="MMM dd, yyyy">
</RangeNavigatorRangeTooltipSettings>
```

### Tooltip Styling
Style tooltips using `RangeNavigatorTooltipTextStyle` and `RangeNavigatorRangeTooltipBorder` components:

```razor
<RangeNavigatorRangeTooltipSettings Enable="true">
    <RangeNavigatorTooltipTextStyle Color="#fff" 
                                         FontFamily="Arial" 
                                         Size="12px"
                                         FontWeight="500">
    </RangeNavigatorTooltipTextStyle>
    <RangeNavigatorRangeTooltipBorder Color="#2196F3" Width="2"></RangeNavigatorRangeTooltipBorder>
</RangeNavigatorRangeTooltipSettings>
```

## Colors and Styling
Customize colors using `Fill`, `RangeNavigatorSeriesBorder`, `RangeNavigatorStyleSettings`, and theme-based styling properties.

### Series Colors
Set series appearance using `Fill` and `RangeNavigatorSeriesBorder` properties:

```razor
<RangeNavigatorSeries DataSource="@Data" 
                      XName="X" 
                      YName="Y"
                      Type="RangeNavigatorType.Area"
                      Fill="rgba(33, 150, 243, 0.3)">
    <RangeNavigatorSeriesBorder Color="#2196F3" Width="2"></RangeNavigatorSeriesBorder>
</RangeNavigatorSeries>
```

### Background Color
Set component background using `Background` property:

```razor
<SfRangeNavigator Background="#f5f5f5" ...>
```

### Selected Region Color
Customize region colors using `RangeNavigatorStyleSettings` with `SelectedRegionColor` and `UnselectedRegionColor`:

```razor
<SfRangeNavigator @bind-Value="@Range" ValueType="RangeValueType.DateTime">
    <RangeNavigatorStyleSettings SelectedRegionColor="rgba(33, 150, 243, 0.1)" 
                                 UnselectedRegionColor="rgba(0, 0, 0, 0.05)">
    </RangeNavigatorStyleSettings>
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@Data" XName="Date" YName="Value">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>
```

### Thumb Styling
Customize thumb appearance using `RangeNavigatorNavigatorStyleSettings` with `ThumbBackground` and `ThumbBorder` properties:

```razor
<SfRangeNavigator @bind-Value="@Range" ValueType="RangeValueType.DateTime">
    <RangeNavigatorNavigatorStyleSettings ThumbBackground="#2196F3" 
                                          ThumbBorder="#1976D2">
    </RangeNavigatorNavigatorStyleSettings>
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@Data" XName="Date" YName="Value">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>
```

## Responsive Design
Implement responsive layouts using `Width`, `Height`, `LabelFormat`, `EnableGrouping`, and device detection logic.

### Percentage-Based Width
Use percentage-based `Width` for responsive containers:

```razor
<div style="width: 100%; max-width: 1200px; margin: 0 auto;">
    <SfRangeNavigator Width="100%" Height="150px" ...>
    </SfRangeNavigator>
</div>
```

### Adaptive Labels
Adapt labels with `LabelFormat` and `EnableGrouping="true"` for responsive display:

```razor
<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  LabelFormat="MMM-yy"
                  EnableGrouping="true">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@Data" XName="Date" YName="Value">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>
```

### Mobile-Friendly Configuration
Adjust `Height` and `LabelFormat` properties based on device type:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator Height="@GetHeight()" 
                  LabelFormat="@GetLabelFormat()" ...>
</SfRangeNavigator>
@code {
    private bool IsMobile => /* Your mobile detection logic */;
    
    private string GetHeight() => IsMobile ? "100px" : "150px";
    private string GetLabelFormat() => IsMobile ? "MMM" : "MMM-yy";
}

```

## Performance Optimization
Optimize rendering performance using `EnableGrouping` and `GroupBy` properties for large datasets.

### Lightweight Mode
Enable lightweight rendering using `EnableGrouping="true"` and `GroupBy` property for better performance:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator @bind-Value="@Range" 
                  ValueType="RangeValueType.DateTime"
                  EnableGrouping="true"
                  GroupBy="RangeIntervalType.Months">
    <RangeNavigatorSeriesCollection>
        <RangeNavigatorSeries DataSource="@LargeDataset" 
                              XName="Date" 
                              YName="Value"
                              Type="RangeNavigatorType.Line">
        </RangeNavigatorSeries>
    </RangeNavigatorSeriesCollection>
</SfRangeNavigator>

@code {
    public List<DataPoint> LargeDataset = GenerateLargeDataset(10000);
}
```

### Data Grouping
Group data points using `EnableGrouping="true"` and `GroupBy` property for improved rendering:

```razor
@using Syncfusion.Blazor.Charts

<SfRangeNavigator EnableGrouping="true" 
                  GroupBy="RangeIntervalType.Months" ...>

@code {}
```

## Complete Example
Implement comprehensive visual customization using `Theme`, `Width`, `Height`, `RangeNavigatorRangeTooltipSettings`, `RangeNavigatorStyleSettings`, and dynamic property bindings:

```razor
@page "/visual-customization-demo"
@using Syncfusion.Blazor.Charts

<div class="demo-container">
    <h3>Range Selector Visual Customization</h3>
    
    <div class="controls-panel">
        <div class="control-group">
            <label>Theme:</label>
            <select @bind="SelectedTheme">
                <option value="@Theme.Material">Material</option>
                <option value="@Theme.Bootstrap5">Bootstrap 5</option>
                <option value="@Theme.Fluent">Fluent</option>
                <option value="@Theme.Tailwind">Tailwind</option>
                <option value="@Theme.Fabric">Fabric</option>
                <option value="@Theme.HighContrast">High Contrast</option>
            </select>
        </div>
        
        <div class="control-group">
            <label>Series Type:</label>
            <select @bind="SeriesType">
                <option value="@RangeNavigatorType.Line">Line</option>
                <option value="@RangeNavigatorType.Area">Area</option>
                <option value="@RangeNavigatorType.StepLine">StepLine</option>
            </select>
        </div>
        
        <div class="control-group">
            <label>Show Tooltips:</label>
            <input type="checkbox" @bind="ShowTooltips" />
        </div>
        
        <div class="control-group">
            <label>Height:</label>
            <input type="range" min="80" max="300" @bind="Height" />
            <span>@Height px</span>
        </div>
    </div>
    
    <div class="range-selector-container">
        <SfRangeNavigator Value="@Range" 
                          ValueType="RangeValueType.DateTime"
                          Theme="@SelectedTheme"
                          Width="100%"
                          Height="@(Height + "px")"
                          LabelFormat="MMM-yy"
                          IntervalType="RangeIntervalType.Months">
            <RangeNavigatorMargin Top="10" Bottom="10" Left="10" Right="10"></RangeNavigatorMargin>
            <RangeNavigatorRangeTooltipSettings Enable="@ShowTooltips" 
                                                DisplayMode="TooltipDisplayMode.Always"
                                                Format="MMM dd, yyyy">
                <RangeNavigatorTooltipTextStyle Size="12px"></RangeNavigatorTooltipTextStyle>
            </RangeNavigatorRangeTooltipSettings>
            <RangeNavigatorStyleSettings SelectedRegionColor="rgba(33, 150, 243, 0.1)" 
                                         UnselectedRegionColor="rgba(0, 0, 0, 0.04)">
            </RangeNavigatorStyleSettings>
            <RangeNavigatorSeriesCollection>
                <RangeNavigatorSeries DataSource="@StockData" 
                                      XName="Date" 
                                      YName="Close"
                                      Type="@SeriesType"
                                      Fill="@GetSeriesFill()"
                                      Width="2">
                    @if (SeriesType == RangeNavigatorType.Area)
                    {
                        <RangeNavigatorSeriesBorder Color="#2196F3" Width="2"></RangeNavigatorSeriesBorder>
                    }
                </RangeNavigatorSeries>
            </RangeNavigatorSeriesCollection>
        </SfRangeNavigator>
    </div>
    
    <div class="info-panel">
        <h4>Current Settings</h4>
        <p><strong>Theme:</strong> @SelectedTheme</p>
        <p><strong>Series Type:</strong> @SeriesType</p>
        <p><strong>Height:</strong> @Height px</p>
        <p><strong>Tooltips:</strong> @(ShowTooltips ? "Enabled" : "Disabled")</p>
        <p><strong>Selected Range:</strong> @Range[0].ToShortDateString() to @Range[1].ToShortDateString()</p>
    </div>
</div>

@code {
    public class StockInfo
    {
        public DateTime Date { get; set; }
        public double Close { get; set; }
    }
    
    public DateTime[] Range = new DateTime[] 
    { 
        DateTime.Now.AddMonths(-6), 
        DateTime.Now 
    };
    
    public Theme SelectedTheme = Theme.Material;
    public RangeNavigatorType SeriesType = RangeNavigatorType.Area;
    public bool ShowTooltips = true;
    public int Height = 150;
    
    public List<StockInfo> StockData = GenerateStockData();
    
    private string GetSeriesFill()
    {
        return SeriesType == RangeNavigatorType.Area 
            ? "rgba(33, 150, 243, 0.3)" 
            : "#2196F3";
    }
    
    private static List<StockInfo> GenerateStockData()
    {
        var data = new List<StockInfo>();
        var startDate = DateTime.Now.AddYears(-2);
        var random = new Random();
        double price = 150;
        
        for (int i = 0; i < 730; i++)
        {
            price += random.Next(-5, 6);
            price = Math.Max(100, Math.Min(200, price));
            
            data.Add(new StockInfo
            {
                Date = startDate.AddDays(i),
                Close = price
            });
        }
        
        return data;
    }
}

<style>
    .demo-container {
        padding: 20px;
        max-width: 1200px;
        margin: 0 auto;
    }
    
    .controls-panel {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
        gap: 15px;
        margin-bottom: 30px;
        padding: 20px;
        background: #f8f9fa;
        border-radius: 8px;
    }
    
    .control-group {
        display: flex;
        flex-direction: column;
        gap: 8px;
    }
    
    .control-group label {
        font-weight: 600;
        font-size: 14px;
        color: #495057;
    }
    
    .control-group select,
    .control-group input[type="range"] {
        padding: 8px;
        border: 1px solid #ced4da;
        border-radius: 4px;
    }
    
    .range-selector-container {
        margin-bottom: 30px;
        padding: 20px;
        background: white;
        border-radius: 8px;
        box-shadow: 0 2px 8px rgba(0,0,0,0.1);
    }
    
    .info-panel {
        padding: 20px;
        background: #e7f3ff;
        border-radius: 8px;
        border-left: 4px solid #2196F3;
    }
    
    .info-panel h4 {
        margin-top: 0;
        color: #1976D2;
    }
    
    .info-panel p {
        margin: 8px 0;
    }
</style>
```

## Best Practices
Apply best practices using `Width`, `Theme`, `RangeNavigatorRangeTooltipSettings`, and performance optimization properties.

### 1. Use Percentage Width
For responsive designs, use percentage-based `Width`:

```razor
<SfRangeNavigator Width="100%" ...>
```

### 2. Match Theme to Application
Choose a `Theme` that matches your overall application design:

```html
<!-- If using Bootstrap -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```

### 3. Enable Tooltips
Always enable tooltips with `Enable="true"` for better user experience:

```razor
<RangeNavigatorRangeTooltipSettings Enable="true">
```

### 4. Optimize for Mobile
Use adaptive `Height` and `LabelFormat` settings for mobile devices:

```csharp
private string GetHeight() => IsMobile ? "100px" : "150px";
```

### 5. Use Lightweight Mode for Large Data
Enable grouping with `EnableGrouping="true"` and `GroupBy` for datasets with thousands of points:

```razor
<SfRangeNavigator EnableGrouping="true" GroupBy="RangeIntervalType.Months">
```

## Troubleshooting
Resolve common visual customization issues using appropriate property configurations.

### Issue: Theme not applied

**Solution**: Ensure the correct stylesheet is referenced and matches the `Theme` property:

```html
<!-- In _Layout.cshtml or index.html -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />
```

```razor
<!-- In component -->
<SfRangeNavigator Theme="Theme.Material" ...>
```

### Issue: Range selector too small on mobile

**Solution**: Use percentage `Width` and appropriate `Height`:

```razor
<SfRangeNavigator Width="100%" Height="120px" ...>
```

### Issue: Tooltips not showing

**Solution**: Ensure tooltips are enabled with `Enable="true"`:

```razor
<RangeNavigatorRangeTooltipSettings Enable="true">
```

### Issue: Poor performance with large dataset

**Solution**: Enable grouping with `EnableGrouping="true"` and `GroupBy`:

```razor
<SfRangeNavigator EnableGrouping="true" GroupBy="RangeIntervalType.Months">
```
