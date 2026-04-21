# Leaf Items

## Table of Contents
- [Overview](#overview)
- [Styling Leaf Nodes](#styling-leaf-nodes)
- [Label Positioning](#label-positioning)
- [Templates](#templates)
- [Interactivity](#interactivity)

## Overview
Guidance on `TreeMapLeafItemSettings`, `TreeMapLeafLabelStyle`, `TreeMapLeafBorder`, and `TreeMapBorderSettings` for styling, templating, and interactive behaviors of leaf nodes.

## Styling Leaf Nodes
- Customize leaf items using the `TreeMapLeafItemSettings` component with properties like `Fill`, `Opacity`, `Gap`, and `Padding`. Use `TreeMapLeafBorder` to configure border appearance with `Color` and `Width` properties. Apply data-driven fills using `TreeMapLeafColorMappings` and `TreeMapLeafColorMapping`.

## Label Positioning
- Use the `TreeMapLeafItemSettings` component to set `LabelPath` (which field to display), `LabelPosition` (position within item), and `LabelFormat` (template syntax) to control label placement. Use `InterSectAction` property to handle label overflow, and `ShowLabels` property to toggle visibility.

## Templates
- Use the `LabelTemplate` property within `TreeMapLeafItemSettings` to define custom HTML content, and the `TemplatePosition` property to control template placement within leaf items.

## Interactivity
- Support click, hover, and selection behaviors using `TreeMapEvents` for event handling and `TreeMapSelectionSettings` component to configure selection behavior for leaf items.
# Leaf Items in Blazor TreeMap Component

## Table of Contents

- [Overview](#overview)
- [Understanding Leaf Items](#understanding-leaf-items)
- [Basic Configuration](#basic-configuration)
  - [LabelPath Property](#labelpath-property)
  - [Fill Color](#fill-color)
  - [Opacity Control](#opacity-control)
- [Label Customization](#label-customization)
  - [Label Positioning](#label-positioning)
  - [Label Formatting](#label-formatting)
  - [Label Styling](#label-styling)
  - [Show/Hide Labels](#showhide-labels)
- [Border Configuration](#border-configuration)
- [Padding and Spacing](#padding-and-spacing)
  - [Item Padding](#item-padding)
  - [Gap Between Items](#gap-between-items)
- [Label Templates](#label-templates)
  - [Basic Templates](#basic-templates)
  - [Template Positioning](#template-positioning)
  - [Dynamic Content in Templates](#dynamic-content-in-templates)
- [Intersect Actions](#intersect-actions)
- [Advanced Styling](#advanced-styling)
- [Troubleshooting](#troubleshooting)
- [Best Practices](#best-practices)

## Overview

Leaf items represent the innermost, visualized data elements in a TreeMap. They are the actual rectangles that display your data values and do not contain child nodes. Proper configuration of leaf items is essential for creating readable and visually appealing TreeMaps. This guide covers all aspects of leaf item customization, from basic labeling to advanced styling and templates.

## Understanding Leaf Items

**What is a Leaf Item?**
- The final level of the TreeMap hierarchy
- Represents individual data points
- Contains no child nodes
- Size determined by `WeightValuePath`
- May have parent nodes if levels are configured

**Leaf Item vs Level:**
- **Leaf Items**: Individual data elements
- **Levels**: Grouping containers for leaf items
- Leaf items inherit properties from their parent levels
- Leaf-specific settings override level settings

## Basic Configuration

### LabelPath Property

The `LabelPath` property, configured within `TreeMapLeafItemSettings`, specifies which field from your data source should be displayed as the label on each leaf item.

**Example: Basic Label Display**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="GrowthReports" TValue="GDPReport" WeightValuePath="GDP">
    <TreeMapLeafItemSettings LabelPath="CountryName">
        <TreeMapLeafLabelStyle Color="#000000"></TreeMapLeafLabelStyle>
        <TreeMapLeafBorder Color="#000000" Width="0.5"></TreeMapLeafBorder>
    </TreeMapLeafItemSettings>
</SfTreeMap>

@code{
    public class GDPReport
    {
        public string CountryName { get; set; }
        public double GDP { get; set; }
        public double Percentage { get; set; }
        public int Rank { get; set; }
    }
    
    public List<GDPReport> GrowthReports = new List<GDPReport> {
        new GDPReport {CountryName="United States", GDP=17946, Percentage=11.08, Rank=1},
        new GDPReport {CountryName="China", GDP=10866, Percentage= 28.42, Rank=2},
        new GDPReport {CountryName="Japan", GDP=4123, Percentage=-30.78, Rank=3},
        new GDPReport {CountryName="Germany", GDP=3355, Percentage=-5.19, Rank=4},
        new GDPReport {CountryName="United Kingdom", GDP=2848, Percentage=8.28, Rank=5},
        new GDPReport {CountryName="France", GDP=2421, Percentage=-9.69, Rank=6},
        new GDPReport {CountryName="India", GDP=2073, Percentage=13.65, Rank=7},
        new GDPReport {CountryName="Italy", GDP=1814, Percentage=-12.45, Rank=8},
        new GDPReport {CountryName="Brazil", GDP=1774, Percentage=-27.88, Rank=9},
        new GDPReport {CountryName="Canada", GDP=1550, Percentage=-15.02, Rank=10}
    };
}
```

**Important Notes:**
- `LabelPath` must reference a valid property in your data class
- Property names are case-sensitive
- Works with string properties (converts other types to string)
- Required for label display

### Fill Color

The `Fill` property, defined in `TreeMapLeafItemSettings`, sets the background color for all leaf items. This can be overridden by color mapping if configured.

**Example: Solid Fill Color**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="GrowthReports" TValue="GDPReport" WeightValuePath="GDP">
    <TreeMapLeafItemSettings LabelPath="CountryName" Fill="#4CAF50">
        <TreeMapLeafBorder Color="#ffffff" Width="1"></TreeMapLeafBorder>
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

**Color Formats Supported:**
- Hex: `#4CAF50`
- RGB: `rgb(76, 175, 80)`
- RGBA: `rgba(76, 175, 80, 0.8)`
- Color names: `green`, `blue`, etc.

**Priority Order:**
1. `ColorValuePath` (from data)
2. Color mapping (if configured)
3. `Fill` property (default)
4. Palette colors

### Opacity Control

The `Opacity` property, configured in `TreeMapLeafItemSettings`, controls the transparency of leaf items.

**Example: Semi-Transparent Leaf Items**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="GrowthReports" TValue="GDPReport" WeightValuePath="GDP">
    <TreeMapLeafItemSettings LabelPath="CountryName" 
                            Fill="#4CAF50" 
                            Opacity="0.7">
        <TreeMapLeafBorder Color="#ffffff" Width="1"></TreeMapLeafBorder>
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

**Opacity Values:**
- Range: 0.0 (fully transparent) to 1.0 (fully opaque)
- Default: 1.0
- Useful for overlapping items or layered effects
- Affects fill color but not borders or labels

## Label Customization

### Label Positioning

The `LabelPosition` property, defined in `TreeMapLeafItemSettings`, determines where labels appear within leaf items. Choose from nine predefined positions:

**Available Positions:**

| Position | Description |
|----------|-------------|
| `TopLeft` | Upper left corner |
| `TopCenter` | Top center |
| `TopRight` | Upper right corner |
| `CenterLeft` | Middle left |
| `Center` | Dead center (default) |
| `CenterRight` | Middle right |
| `BottomLeft` | Lower left corner |
| `BottomCenter` | Bottom center |
| `BottomRight` | Lower right corner |

**Example: Top Center Label Positioning**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="GrowthReports" TValue="GDPReport" WeightValuePath="GDP">
    <TreeMapLeafItemSettings LabelPath="CountryName" 
                            LabelPosition="Syncfusion.Blazor.TreeMap.LabelPosition.TopCenter"
                            LabelFormat="${CountryName}<br>$${GDP} Trillion<br>(${Percentage} %)">
        <TreeMapLeafLabelStyle Color="#000000"></TreeMapLeafLabelStyle>
        <TreeMapLeafBorder Color="#000000" Width="0.5"></TreeMapLeafBorder>
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

**When to Use Each Position:**
- **TopLeft/TopRight**: Headers or identifiers
- **TopCenter**: Primary labels
- **Center**: Balanced, default choice
- **BottomCenter**: Captions or metadata
- **CenterLeft/CenterRight**: Side labels

### Label Formatting

The `LabelFormat` property, available in `TreeMapLeafItemSettings`, enables complex label content using template syntax with data field placeholders.

**Format Syntax:**
- Use `${}` to reference data properties
- Supports HTML for line breaks (`<br>`)
- Can combine multiple properties
- Applies string formatting

**Example: Multi-Line Formatted Labels**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="GrowthReports" TValue="GDPReport" WeightValuePath="GDP">
    <TreeMapLeafItemSettings LabelPath="CountryName" 
                            LabelPosition="Syncfusion.Blazor.TreeMap.LabelPosition.Center"
                            LabelFormat="${CountryName}<br>GDP: $${GDP}B<br>Rank: #${Rank}">
        <TreeMapLeafLabelStyle Color="#ffffff" Size="14px"></TreeMapLeafLabelStyle>
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

**Format Examples:**

```cshtml
<!-- Simple format -->
LabelFormat="${CountryName}"

<!-- With currency -->
LabelFormat="${CountryName} - $${GDP} Trillion"

<!-- Multi-line -->
LabelFormat="${CountryName}<br>${GDP} Trillion<br>${Percentage}%"

<!-- With calculations (done in code) -->
LabelFormat="${CountryName}<br>Score: ${CalculatedScore}"
```

### Label Styling

The `TreeMapLeafLabelStyle` component, used within `TreeMapLeafItemSettings`, provides comprehensive control over label appearance through the following properties:

**Available Properties:**
- `Color` - Text color property for label text
- `Size` - Font size property (e.g., "14px")
- `FontFamily` - Font family property for typeface selection
- `FontWeight` - Font weight property ("normal", "bold", "600")
- `FontStyle` - Font style property ("normal", "italic")
- `Opacity` - Opacity property for text transparency (0.0 to 1.0)

**Example: Comprehensive Label Styling**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="GrowthReports" TValue="GDPReport" WeightValuePath="GDP">
    <TreeMapLeafItemSettings LabelPath="CountryName" Fill="#2196F3">
        <TreeMapLeafLabelStyle Color="#ffffff" 
                              Size="16px" 
                              FontFamily="Arial, sans-serif"
                              FontWeight="bold"
                              FontStyle="normal"
                              Opacity="1.0">
        </TreeMapLeafLabelStyle>
        <TreeMapLeafBorder Color="#1976D2" Width="2"></TreeMapLeafBorder>
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

**Styling Best Practices:**
1. Ensure high contrast between label and background
2. Use readable font sizes (minimum 10-12px)
3. Avoid overly decorative fonts
4. Test with different item sizes
5. Consider accessibility guidelines

### Show/Hide Labels

The `ShowLabels` property, configured in `TreeMapLeafItemSettings`, controls label visibility globally for all leaf items.

**Example: Hiding Labels**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="GrowthReports" TValue="GDPReport" WeightValuePath="GDP">
    <TreeMapLeafItemSettings LabelPath="CountryName" 
                            ShowLabels="false"
                            Fill="#FF5722">
        <TreeMapLeafBorder Color="#ffffff" Width="1"></TreeMapLeafBorder>
    </TreeMapLeafItemSettings>
    <!-- Use tooltips when labels are hidden -->
    <TreeMapTooltipSettings Visible="true" Format="${CountryName}: ${GDP}B">
    </TreeMapTooltipSettings>
</SfTreeMap>
```

**When to Hide Labels:**
- Very small leaf items
- When using tooltips for information
- Minimalist designs
- Space-constrained layouts
- When labels would overlap significantly

## Border Configuration

The `TreeMapLeafBorder` component, configured within `TreeMapLeafItemSettings`, defines the border appearance for leaf items using properties like `Color` and `Width`.

**Example: Custom Borders**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="GrowthReports" TValue="GDPReport" WeightValuePath="GDP">
    <TreeMapLeafItemSettings LabelPath="CountryName" Fill="#E3F2FD">
        <TreeMapLeafLabelStyle Color="#1565C0"></TreeMapLeafLabelStyle>
        <TreeMapLeafBorder Color="#1976D2" Width="2"></TreeMapLeafBorder>
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

**Border Properties:**
- `Color` - Border color property (any valid CSS color)
- `Width` - Border width property in pixels

**Border Styling Tips:**
1. Use contrasting colors for clarity
2. Typical width: 0.5-2 pixels
3. White/light borders separate adjacent items
4. Dark borders emphasize boundaries
5. Consider border width with gap spacing

**Example: No Border**

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafBorder Color="transparent" Width="0"></TreeMapLeafBorder>
```

## Padding and Spacing

### Item Padding

The `Padding` property, defined in `TreeMapLeafItemSettings`, adds internal spacing between the border and label content.

**Example: Padded Leaf Items**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="GrowthReports" TValue="GDPReport" WeightValuePath="GDP">
    <TreeMapLeafItemSettings LabelPath="CountryName" 
                            Padding="10"
                            Fill="#FFF3E0">
        <TreeMapLeafLabelStyle Color="#E65100"></TreeMapLeafLabelStyle>
        <TreeMapLeafBorder Color="#FF9800" Width="1"></TreeMapLeafBorder>
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

**Padding Values:**
- Single number applies to all sides
- Measured in pixels
- Default: varies by implementation
- Affects label positioning
- Reduces available label space

### Gap Between Items

The `Gap` property, configured in `TreeMapLeafItemSettings`, creates space between adjacent leaf items, improving visual separation.

**Example: Gapped Layout**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="GrowthReports" TValue="GDPReport" WeightValuePath="GDP">
    <TreeMapLeafItemSettings LabelPath="CountryName" Gap="20">
        <TreeMapLeafLabelStyle Color="#ffffff" Size="14px">
        </TreeMapLeafLabelStyle>
        <TreeMapLeafBorder Color="#ffffff" Width="1"></TreeMapLeafBorder>
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

**Gap Recommendations:**
- **0-5px**: Minimal separation
- **5-10px**: Standard separation
- **10-20px**: Clear separation
- **20+px**: Emphasized separation

**Visual Impact:**
- Larger gaps improve item distinction
- Excessive gaps waste space
- Balance with container size
- Consider data density

## Label Templates

### Basic Templates

The `LabelTemplate` property, available within `TreeMapLeafItemSettings`, enables custom HTML content for labels, providing complete control over label appearance and structure.

**Example: Simple HTML Template**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="GrowthReports" TValue="GDPReport" WeightValuePath="GDP">
    <TreeMapLeafItemSettings LabelPath="CountryName" Fill="#E8F5E9">
        <LabelTemplate>
            @{
                var data = context as GDPReport;
                <div style="padding: 5px;">
                    <strong style="color: #2E7D32;">@data.CountryName</strong>
                    <br />
                    <span style="color: #66BB6A; font-size: 12px;">
                        GDP: $@data.GDP Billion
                    </span>
                </div>
            }
        </LabelTemplate>
        <TreeMapLeafBorder Color="#4CAF50" Width="1"></TreeMapLeafBorder>
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

### Template Positioning

The `TemplatePosition` property, defined in `TreeMapLeafItemSettings`, controls where custom templates are placed within leaf items.

**Example: Positioned Template**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="GrowthReports" TValue="GDPReport" WeightValuePath="GDP">
    <TreeMapLeafItemSettings LabelPath="CountryName" 
                            Fill="#F3E5F5"
                            TemplatePosition="Syncfusion.Blazor.TreeMap.LabelPosition.TopLeft">
        <LabelTemplate>
            @{
                var data = context as GDPReport;
                <div style="background: rgba(255,255,255,0.9); padding: 5px; border-radius: 3px;">
                    <div style="font-weight: bold; color: #6A1B9A;">@data.CountryName</div>
                    <div style="font-size: 11px; color: #8E24AA;">Rank: #@data.Rank</div>
                </div>
            }
        </LabelTemplate>
        <TreeMapLeafBorder Color="#9C27B0" Width="1"></TreeMapLeafBorder>
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

### Dynamic Content in Templates

The `LabelTemplate` property within `TreeMapLeafItemSettings` can include conditional logic, calculations, and dynamic styling through Razor syntax.

**Example: Conditional Template Styling**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="GrowthReports" TValue="GDPReport" WeightValuePath="GDP">
    <TreeMapLeafItemSettings LabelPath="CountryName" Fill="#ECEFF1">
        <LabelTemplate>
            @{
                var data = context as GDPReport;
                var isPositive = data.Percentage > 0;
                var bgColor = isPositive ? "#C8E6C9" : "#FFCDD2";
                var textColor = isPositive ? "#2E7D32" : "#C62828";
                var arrow = isPositive ? "▲" : "▼";
                
                <div style="background: @bgColor; padding: 8px; border-radius: 4px;">
                    <div style="font-weight: bold; color: #263238; font-size: 14px;">
                        @data.CountryName
                    </div>
                    <div style="color: @textColor; font-size: 12px; margin-top: 4px;">
                        @arrow @Math.Abs(data.Percentage)%
                    </div>
                    <div style="color: #546E7A; font-size: 11px; margin-top: 2px;">
                        GDP: $@data.GDP B
                    </div>
                </div>
            }
        </LabelTemplate>
        <ChildContent>
            <TreeMapLeafBorder Color="#607D8B" Width="1"></TreeMapLeafBorder>
        </ChildContent>
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

**Template Capabilities:**
- HTML markup
- Inline CSS styles
- Conditional rendering
- Data calculations
- Icons and images
- Multiple data fields
- Complex layouts

**Template Best Practices:**
1. Keep templates performant
2. Avoid complex calculations in templates
3. Use inline styles sparingly
4. Test with various data
5. Consider item sizes
6. Maintain readability
7. Handle null/missing data

## Intersect Actions

When labels exceed the available space in leaf items, the `InterSectAction` property, configured in `TreeMapLeafItemSettings`, determines how to handle the overflow.

**Available Actions:**

| Action | Behavior |
|--------|----------|
| `None` | Label may overflow item bounds |
| `Hide` | Hide labels that don't fit |
| `Trim` | Truncate labels with ellipsis |
| `WrapByWord` | Wrap text at word boundaries |
| `Wrap` | Wrap text at any character |

**Example: Word Wrapping**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" TValue="Car" DataSource="Cars">
    <TreeMapLeafItemSettings LabelPath="Name" 
                            LabelFormat="${Name}-${Brand}" 
                            InterSectAction="Syncfusion.Blazor.TreeMap.LabelAlignment.WrapByWord"
                            Padding="5">
        <TreeMapLeafLabelStyle Size="12px" Color="#263238">
        </TreeMapLeafLabelStyle>
        <TreeMapLeafBorder Color="#90A4AE" Width="1"></TreeMapLeafBorder>
    </TreeMapLeafItemSettings>
</SfTreeMap>

@code{
    public class Car
    {
        public string Name { get; set; }
        public string Brand { get; set; }
        public int Count { get; set; }
    }
    
    public List<Car> Cars = new List<Car> {
        new Car { Name="Mustang", Brand="Ford Motor Company", Count=232 },
        new Car { Name="Lincoln Continental Mark V", Brand="Ford Motor Company", Count=50},
        new Car { Name="EcoSport", Brand="Ford Motor Company", Count=121 },
        new Car { Name="Swift", Brand="Maruti", Count=143 },
        new Car { Name="Baleno", Brand="Maruti", Count=454 },
        new Car { Name="Vitara Brezza", Brand="Maruti", Count=545 },
        new Car { Name="A3 Cabriolet", Brand="Audi", Count=123 },
        new Car { Name="RS7 Sportback", Brand="Audi", Count=523 }
    };
}
```

**Choosing Intersect Actions:**

| Scenario | Recommended Action |
|----------|-------------------|
| Short labels | `None` or `Trim` |
| Long descriptions | `WrapByWord` |
| Technical terms | `Wrap` |
| Space-constrained | `Hide` or `Trim` |
| Multi-word labels | `WrapByWord` |

## Advanced Styling

### Combining Multiple Style Features

The following example demonstrates how to combine multiple `TreeMapLeafItemSettings` properties along with `TreeMapLeafLabelStyle` and `TreeMapLeafBorder` components to create comprehensive leaf item styling:

**Example: Complete Leaf Item Styling**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="GrowthReports" 
           TValue="GDPReport" 
           WeightValuePath="GDP"
           Palette='new string[] { "#FF6B6B", "#4ECDC4", "#45B7D1", "#FFA07A", "#98D8C8", "#F7DC6F" }'>
    
    <TreeMapLeafItemSettings LabelPath="CountryName"
                            LabelPosition="LabelPosition.Center"
                            LabelFormat="${CountryName}<br>${GDP}B"
                            InterSectAction="LabelAlignment.WrapByWord"
                            Gap="8"
                            Padding="10"
                            Opacity="0.9">
        
        <TreeMapLeafLabelStyle Color="#FFFFFF"
                              Size="14px"
                              FontFamily="'Segoe UI', Tahoma, Geneva, Verdana, sans-serif"
                              FontWeight="600"
                              Opacity="1.0">
        </TreeMapLeafLabelStyle>
        
        <TreeMapLeafBorder Color="#FFFFFF" Width="2">
        </TreeMapLeafBorder>
    </TreeMapLeafItemSettings>
    
    <TreeMapTooltipSettings Visible="true" 
                           Format="${CountryName}<br>GDP: $${GDP}B<br>Growth: ${Percentage}%">
        <TreeMapTooltipTextStyle Color="#263238" Size="13px">
        </TreeMapTooltipTextStyle>
    </TreeMapTooltipSettings>
</SfTreeMap>
```

### Responsive Leaf Item Styling

Use dynamic property values for `TreeMapLeafItemSettings` properties like `Gap` and `TreeMapLeafLabelStyle` properties like `Size` to adjust styling based on container size or data:

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="GrowthReports" TValue="GDPReport" WeightValuePath="GDP">
    <TreeMapLeafItemSettings LabelPath="CountryName" Gap="@GetItemGap()">
        <TreeMapLeafLabelStyle Size="@GetLabelSize()">
        </TreeMapLeafLabelStyle>
    </TreeMapLeafItemSettings>
</SfTreeMap>
@code {
    private string GetLabelSize()
    {
        // Adjust based on container or data
        return DataCount > 50 ? "10px" : "14px";
    }
    
    private double GetItemGap()
    {
        return DataCount > 100 ? 2 : 10;
    }
}

```

## Troubleshooting

### Labels Not Displaying

**Problem:** Labels are missing or invisible.

**Solutions:**
1. Verify the `LabelPath` property in `TreeMapLeafItemSettings` matches data property exactly
2. Check `ShowLabels` property is not set to false
3. Ensure label color (via `TreeMapLeafLabelStyle.Color`) contrasts with `Fill` property
4. Verify data contains non-empty label values
5. Check if `InterSectAction` property is set to `Hide`

```cshtml
@code {
    // Debug label display
    protected override void OnAfterRender(bool firstRender)
    {
        if (firstRender)
        {
            foreach (var item in GrowthReports)
            {
                var labelValue = item.GetType()
                    .GetProperty("CountryName")
                    ?.GetValue(item);
                Console.WriteLine($"Label: {labelValue}");
            }
        }
    }
}
```

### Labels Overlapping or Truncated

**Problem:** Labels overlap each other or are cut off.

**Solutions:**
1. Use appropriate `InterSectAction` property in `TreeMapLeafItemSettings` (e.g., `WrapByWord`)
2. Reduce `LabelFormat` complexity
3. Decrease `Size` property in `TreeMapLeafLabelStyle`
4. Increase `Gap` property in `TreeMapLeafItemSettings`
5. Use shorter label text
6. Adjust `LabelPosition` property in `TreeMapLeafItemSettings`

### Borders Not Visible

**Problem:** Borders don't appear or are too faint.

**Solutions:**
1. Increase `Width` property in `TreeMapLeafBorder` (try 1-2 pixels)
2. Use contrasting `Color` property in `TreeMapLeafBorder`
3. Check if `TreeMapLeafBorder.Color` matches `TreeMapLeafItemSettings.Fill`
4. Ensure `TreeMapLeafBorder.Width` is greater than 0
5. Verify CSS isn't overriding borders

### Template Not Rendering

**Problem:** Custom `LabelTemplate` property doesn't display.

**Solutions:**
1. Verify `@context` is properly cast to the correct data type
2. Check for null data values in template binding
3. Ensure HTML markup is valid
4. Test with simpler template in `LabelTemplate` first
5. Check browser console for errors

### Performance Issues with Many Items

**Problem:** TreeMap renders slowly with many leaf items.

**Solutions:**
1. Simplify the `LabelTemplate` property content
2. Reduce `LabelFormat` complexity
3. Minimize calculations in `TreeMapLeafLabelStyle` properties
4. Consider data aggregation before binding to DataSource
5. Use simpler `InterSectAction` property values (e.g., `None` or `Hide`)
6. Disable tooltips using `TreeMapTooltipSettings.Visible` if not needed

## Best Practices

### Label Design
1. ✅ Use clear, concise labels
2. ✅ Ensure high contrast ratios (WCAG AA: 4.5:1 minimum)
3. ✅ Test with various data lengths
4. ✅ Use appropriate font sizes (12-16px typical)
5. ✅ Consider internationalization

### Color and Style
1. ✅ Maintain consistent color scheme
2. ✅ Use palette for automatic coloring
3. ✅ Provide sufficient visual separation
4. ✅ Test with colorblind-friendly palettes
5. ✅ Balance aesthetics with readability

### Spacing and Layout
1. ✅ Use gap spacing appropriately (5-15px typical)
2. ✅ Balance padding with available space
3. ✅ Keep borders subtle (0.5-2px)
4. ✅ Test with minimum and maximum data sizes
5. ✅ Consider responsive adjustments

### Templates
1. ✅ Keep templates simple and performant
2. ✅ Handle null/missing data gracefully
3. ✅ Test with edge cases
4. ✅ Use semantic HTML
5. ✅ Avoid complex calculations in templates
6. ✅ Cache computed values when possible

### Accessibility
1. ✅ Ensure text is readable at minimum size
2. ✅ Provide tooltips for additional context
3. ✅ Use semantic color meanings
4. ✅ Test with screen readers
5. ✅ Support keyboard navigation
6. ✅ Provide alternative text when needed

### Performance
1. ✅ Limit template complexity for large datasets
2. ✅ Use simple intersect actions
3. ✅ Minimize DOM operations
4. ✅ Consider lazy loading for very large datasets
5. ✅ Profile rendering performance
6. ✅ Optimize re-renders

### Data Handling
1. ✅ Validate data before binding
2. ✅ Handle missing properties gracefully
3. ✅ Provide default values for optional fields
4. ✅ Test with various data shapes
5. ✅ Document data requirements
6. ✅ Implement error boundaries
