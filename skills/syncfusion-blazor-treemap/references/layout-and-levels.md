# Layout and Levels

## Table of Contents
- [Overview](#overview)
- [Layout Types](#layout-types)
- [Level Configuration](#level-configuration)
- [Headers and Grouping](#headers-and-grouping)
- [Sizing, Gaps and Borders](#sizing-gaps-and-borders)

## Overview
Covers `LayoutMode`, `RenderingMode`, `TreeMapLevels`, and `TreeMapLevel` configuration for multi-level TreeMaps.

## Layout Types
- `Squarified`, `SliceAndDiceVertical`, `SliceAndDiceHorizontal`, and `SliceAndDiceAuto` — choose based on readability and aspect ratio.

## Level API Reference
- Use `TreeMapLevels` to group one or more `TreeMapLevel` items.
- `TreeMapLevel` supports `GroupPath`, `HeaderTemplate`, `HeaderFormat`, `HeaderAlignment`, `HeaderHeight`, `ShowHeader`, `GroupGap`, `GroupPadding`, `Fill`, `Opacity`, `AutoFill`, and `TemplatePosition`.
- Use `TreeMapLevelBorder` to set level border color and width.

## Level Configuration
- Configure `TreeMapLevel` entries with `GroupPath`, `HeaderTemplate`, and level-specific styling.

## Headers and Grouping
- Use headers to label groups per level and control visibility with `ShowHeader` or templates.

## Sizing, Gaps and Borders
- Adjust `GroupGap`, `GroupPadding`, `TreeMapLevelBorder`, and padding to improve legibility.
# Layout and Levels in Blazor TreeMap Component

## Table of Contents

- [Overview](#overview)
- [Layout Algorithms](#layout-algorithms)
  - [Squarified Layout](#squarified-layout)
  - [SliceAndDiceVertical Layout](#sliceanddicevertical-layout)
  - [SliceAndDiceHorizontal Layout](#sliceanddice-horizontal-layout)
  - [SliceAndDiceAuto Layout](#sliceanddiceauto-layout)
  - [Choosing the Right Layout](#choosing-the-right-layout)
- [Rendering Direction](#rendering-direction)
  - [TopLeftBottomRight](#topleftbottomright)
  - [TopRightBottomLeft](#toprightbottomleft)
  - [BottomRightTopLeft](#bottomrighttopleft)
  - [BottomLeftTopRight](#bottomlefttopright)
- [Multi-Level Configuration](#multi-level-configuration)
  - [Understanding TreeMapLevels](#understanding-treemaplevels)
  - [GroupPath Property](#grouppath-property)
  - [Configuring Multiple Levels](#configuring-multiple-levels)
- [Level Customization](#level-customization)
  - [Group Gap](#group-gap)
  - [Header Configuration](#header-configuration)
  - [Header Alignment](#header-alignment)
  - [Border Styling](#border-styling)
- [Advanced Level Features](#advanced-level-features)
  - [Header Templates](#header-templates)
  - [Conditional Level Display](#conditional-level-display)
  - [Dynamic Level Generation](#dynamic-level-generation)
- [Troubleshooting](#troubleshooting)
- [Best Practices](#best-practices)

## Overview

Layout and levels are fundamental features of the TreeMap component that control how data is visually organized and displayed. The layout algorithm determines the arrangement pattern of rectangles, while levels define the hierarchical structure for multi-level data visualization. Together, they enable powerful and flexible data representation.

## Layout Algorithms

The `LayoutType` property is used to control the layout algorithm. The TreeMap component provides four layout algorithms through the `LayoutType` property. Each algorithm offers different visual characteristics and is suitable for specific use cases.

### Squarified Layout

In the Squarified layout, the `LayoutType` property is set to `LayoutMode.Squarified`. The **Squarified** layout is the default algorithm that creates rectangles with aspect ratios as close to 1:1 as possible, making items appear more square-like. This approach optimizes readability by avoiding extremely elongated rectangles.

**When to use:**
- Default choice for most scenarios
- When readability is the priority
- When items need to be easily comparable

**Example:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="GrowthReports" TValue="GDPReport" 
           WeightValuePath="GDP" LayoutType="Syncfusion.Blazor.TreeMap.LayoutMode.Squarified">
    <TreeMapLeafItemSettings LabelPath="CountryName">
    </TreeMapLeafItemSettings>
</SfTreeMap>

@code {
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

**Characteristics:**
- Maintains balanced aspect ratios
- Organizes items in a grid-like pattern
- Excellent for comparing similarly-sized items
- Default layout if not specified

### SliceAndDiceVertical Layout

In the SliceAndDiceVertical layout, the `LayoutType` property is set to `LayoutMode.SliceAndDiceVertical`. The **SliceAndDiceVertical** layout arranges rectangles in vertical strips with high aspect ratios, creating a columnar appearance.

**When to use:**
- Emphasizing vertical hierarchy
- Dashboard-style layouts
- When horizontal space is limited
- Comparing items by height

**Example:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="GrowthReports" TValue="GDPReport" 
           WeightValuePath="GDP" LayoutType="Syncfusion.Blazor.TreeMap.LayoutMode.SliceAndDiceVertical">
    <TreeMapLeafItemSettings LabelPath="CountryName">
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

**Characteristics:**
- Items arranged in vertical columns
- High aspect ratio rectangles
- Clear top-to-bottom reading order
- Space-efficient for narrow containers

### SliceAndDiceHorizontal Layout

In the SliceAndDiceHorizontal layout, the `LayoutType` property is set to `LayoutMode.SliceAndDiceHorizontal`. The **SliceAndDiceHorizontal** layout arranges rectangles in horizontal rows with high aspect ratios, creating a row-based appearance.

**When to use:**
- Emphasizing horizontal hierarchy
- Timeline-style visualizations
- When vertical space is limited
- Comparing items by width

**Example:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="GrowthReports" TValue="GDPReport" 
           WeightValuePath="GDP" LayoutType="Syncfusion.Blazor.TreeMap.LayoutMode.SliceAndDiceHorizontal">
    <TreeMapLeafItemSettings LabelPath="CountryName">
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

**Characteristics:**
- Items arranged in horizontal rows
- High aspect ratio rectangles
- Clear left-to-right reading order
- Space-efficient for wide containers

### SliceAndDiceAuto Layout

In the SliceAndDiceAuto layout, the `LayoutType` property is set to `LayoutMode.SliceAndDiceAuto`. The **SliceAndDiceAuto** layout intelligently switches between horizontal and vertical arrangements based on available space and item count.

**When to use:**
- Responsive designs
- Unknown container dimensions
- Adaptive layouts
- Mixed data sizes

**Example:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="GrowthReports" TValue="GDPReport" 
           WeightValuePath="GDP" LayoutType="Syncfusion.Blazor.TreeMap.LayoutMode.SliceAndDiceAuto">
    <TreeMapLeafItemSettings LabelPath="CountryName">
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

**Characteristics:**
- Automatically determines optimal direction
- Adapts to container size
- Balances space utilization
- Ideal for responsive applications

### Choosing the Right Layout

| Scenario | Recommended Layout | Reason |
|----------|-------------------|---------|
| General purpose | Squarified | Best readability |
| Tall containers | SliceAndDiceVertical | Maximizes vertical space |
| Wide containers | SliceAndDiceHorizontal | Maximizes horizontal space |
| Responsive design | SliceAndDiceAuto | Adapts to container |
| Data analysis | Squarified | Easy comparison |
| Hierarchical data | Squarified | Clear grouping |
| Timeline data | SliceAndDiceHorizontal | Natural flow |

## Rendering Direction

The `RenderDirection` property is used to control the starting position and flow direction of TreeMap items. This is particularly useful for right-to-left (RTL) languages and specific design requirements.

### TopLeftBottomRight

In the TopLeftBottomRight rendering direction, the `RenderDirection` property is set to `RenderingMode.TopLeftBottomRight`. The default rendering direction starts from the top-left corner and flows to the bottom-right.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Fruits" TValue="Fruit" WeightValuePath="Count" 
           Palette='new string[]{"#71B081","#5A9A77", "#498770", "#39776C", "#266665","#124F5E"}' 
           RenderDirection="Syncfusion.Blazor.TreeMap.RenderingMode.TopLeftBottomRight">
    <TreeMapLeafItemSettings LabelPath="Name">
    </TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible=true Format="${Count} : ${Name}">
    </TreeMapTooltipSettings>
</SfTreeMap>

@code{
    public class Fruit
    {
        public string Name { get; set; }
        public int Count { get; set; }
    }
    
    public List<Fruit> Fruits = new List<Fruit> {
        new Fruit { Name="Apple", Count=5000 },
        new Fruit { Name="Mango", Count=3000 },
        new Fruit { Name="Orange", Count=2300 },
        new Fruit { Name="Banana", Count=500 },
        new Fruit { Name="Grape", Count=4300 },
        new Fruit { Name="Papaya", Count=1200 },
        new Fruit { Name="Melon", Count=4500 }
    };
}
```

### TopRightBottomLeft

In the TopRightBottomLeft rendering direction, the `RenderDirection` property is set to `RenderingMode.TopRightBottomLeft`. Renders from top-right to bottom-left, suitable for RTL languages.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Fruits" TValue="Fruit" WeightValuePath="Count" 
           RenderDirection="Syncfusion.Blazor.TreeMap.RenderingMode.TopRightBottomLeft">
    <TreeMapLeafItemSettings LabelPath="Name">
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

### BottomRightTopLeft

In the BottomRightTopLeft rendering direction, the `RenderDirection` property is set to `RenderingMode.BottomRightTopLeft`. Renders from bottom-right to top-left, creating an inverted flow.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Fruits" TValue="Fruit" WeightValuePath="Count" 
           RenderDirection="Syncfusion.Blazor.TreeMap.RenderingMode.BottomRightTopLeft">
    <TreeMapLeafItemSettings LabelPath="Name">
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

### BottomLeftTopRight

In the BottomLeftTopRight rendering direction, the `RenderDirection` property is set to `RenderingMode.BottomLeftTopRight`. Renders from bottom-left to top-right, useful for specific design patterns.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Fruits" TValue="Fruit" WeightValuePath="Count" 
           RenderDirection="Syncfusion.Blazor.TreeMap.RenderingMode.BottomLeftTopRight">
    <TreeMapLeafItemSettings LabelPath="Name">
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

## Multi-Level Configuration

### Understanding TreeMapLevels

The `TreeMapLevels` component is used to define multiple levels of grouping for hierarchical data visualization. Each level represents a layer in the data hierarchy, with its own visual properties and configuration.

**Key Concepts:**
- Levels are defined in order from outermost to innermost
- Each level requires a `GroupPath` to specify the grouping property
- Visual properties can be customized per level
- Headers display the group name for each level

### GroupPath Property

The `GroupPath` property is used to specify which data property should be used for grouping at each level. It is the most critical property for multi-level configuration as it specifies which property from the data source should be used to create groups at that level.

**Example: Employee Organization**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="EmployeeCount" TValue="Employee" 
           DataSource="Employees" 
           Palette='new string[] {"#f44336", "#29b6f6", "#ab47bc", "#ffc107", "#5c6bc0", "#009688"}'>
    <TreeMapLevels>
        <TreeMapLevel GroupPath="Country">
            <TreeMapLevelBorder Color="black" Width="0.5">
            </TreeMapLevelBorder>
        </TreeMapLevel>
        <TreeMapLevel GroupPath="JobDescription">
            <TreeMapLevelBorder Color="black" Width="0.5">
            </TreeMapLevelBorder>
        </TreeMapLevel>
        <TreeMapLevel GroupPath="JobGroup">
            <TreeMapLevelBorder Color="black" Width="0.5">
            </TreeMapLevelBorder>
        </TreeMapLevel>
    </TreeMapLevels>
</SfTreeMap>

@code{
    public class Employee
    {
        public string Country { get; set; }
        public string JobDescription { get; set; }
        public string JobGroup { get; set; }
        public int EmployeeCount { get; set; }
    }
    
    public List<Employee> Employees = new List<Employee> {
        new Employee { Country= "USA", JobDescription= "Sales", JobGroup= "Executive", EmployeeCount= 20 },
        new Employee { Country= "USA", JobDescription= "Sales", JobGroup= "Analyst", EmployeeCount= 30 },
        new Employee { Country= "USA", JobDescription= "Marketing", EmployeeCount= 40 },
        new Employee { Country= "USA", JobDescription= "Management", EmployeeCount= 80 },
        new Employee { Country= "India", JobDescription= "Technical", JobGroup= "Testers", EmployeeCount= 100 },
        new Employee { Country= "India", JobDescription= "HR Executives", EmployeeCount= 30 },
        new Employee { Country= "India", JobDescription= "Accounts", EmployeeCount= 40 },
        new Employee { Country= "UK", JobDescription= "Technical", JobGroup= "Testers", EmployeeCount= 30 },
        new Employee { Country= "UK", JobDescription= "HR Executives", EmployeeCount= 50 },
        new Employee { Country= "UK", JobDescription= "Accounts", EmployeeCount= 60 }
    };
}
```

**Hierarchy Breakdown:**
1. **Level 1 (Country)**: Groups by country - creates USA, India, UK groups
2. **Level 2 (JobDescription)**: Within each country, groups by job - Sales, Technical, etc.
3. **Level 3 (JobGroup)**: Within job descriptions, groups by specific roles - Executive, Analyst, etc.

### Configuring Multiple Levels

When configuring multiple levels with `TreeMapLevel` components and `GroupPath` properties, consider:

1. **Order Matters**: Levels are applied in the order they are defined
2. **Property Availability**: Ensure GroupPath properties exist in data
3. **Visual Hierarchy**: Use `Fill`, `Opacity`, and other styling properties per level
4. **Header Information**: Each level can display a header using `HeaderTemplate` or `HeaderFormat`

**Example: Three-Level Hierarchy with Customization**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="EmployeeCount" TValue="Employee" 
           DataSource="Employees">
    <TreeMapLevels>
        <!-- Level 1: Country -->
        <TreeMapLevel GroupPath="Country" Fill="#c5e2f7" HeaderHeight="30">
            <TreeMapLevelBorder Color="black" Width="1">
            </TreeMapLevelBorder>
            <TreeMapHeaderStyle Size="16px" FontWeight="bold">
            </TreeMapHeaderStyle>
        </TreeMapLevel>
        
        <!-- Level 2: Job Description -->
        <TreeMapLevel GroupPath="JobDescription" Fill="#a4d1f2" HeaderHeight="25">
            <TreeMapLevelBorder Color="black" Width="0.8">
            </TreeMapLevelBorder>
            <TreeMapHeaderStyle Size="14px">
            </TreeMapHeaderStyle>
        </TreeMapLevel>
        
        <!-- Level 3: Job Group -->
        <TreeMapLevel GroupPath="JobGroup" Fill="#8ebfe2" HeaderHeight="20">
            <TreeMapLevelBorder Color="black" Width="0.5">
            </TreeMapLevelBorder>
            <TreeMapHeaderStyle Size="12px">
            </TreeMapHeaderStyle>
        </TreeMapLevel>
    </TreeMapLevels>
    <TreeMapLeafItemSettings LabelPath="JobDescription" Fill="#6699cc">
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

## Level Customization

### Group Gap

The `GroupGap` property is used to create visual separation between groups at each level. This improves readability and distinguishes between different groups.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="EmployeeCount" TValue="Employee" 
           DataSource="Employees" 
           Palette='new string[] {"#f44336", "#29b6f6", "#ab47bc", "#ffc107", "#5c6bc0", "#009688"}'>
    <TreeMapLevels>
        <TreeMapLevel GroupPath="Country" GroupGap="10">
            <TreeMapLevelBorder Color="black" Width="0.5">
            </TreeMapLevelBorder>
        </TreeMapLevel>
        <TreeMapLevel GroupPath="JobDescription" GroupGap="10">
            <TreeMapLevelBorder Color="black" Width="0.5">
            </TreeMapLevelBorder>
        </TreeMapLevel>
        <TreeMapLevel GroupPath="JobGroup" GroupGap="10">
            <TreeMapLevelBorder Color="black" Width="0.5">
            </TreeMapLevelBorder>
        </TreeMapLevel>
    </TreeMapLevels>
</SfTreeMap>
```

**Best Practices for GroupGap:**
- Use 5-15 pixels for standard layouts
- Increase for better visual separation
- Keep consistent across levels for uniformity
- Consider container size when setting values

### Header Configuration

The `HeaderHeight`, `TreeMapHeaderStyle`, and `ShowHeader` properties are used to configure headers that display the group name at each level. Customize header appearance using these properties.

**Example: Custom Header Styling**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="EmployeeCount" TValue="Employee" 
           DataSource="Employees" 
           Palette='new string[] {"#f44336", "#29b6f6", "#ab47bc", "#ffc107", "#5c6bc0", "#009688"}'>
    <TreeMapLevels>
        <TreeMapLevel GroupPath="Country" HeaderHeight="35">
            <TreeMapHeaderStyle Size="15px" FontWeight="bold" Color="#ffffff">
            </TreeMapHeaderStyle>
            <TreeMapLevelBorder Color="black" Width="0.5">
            </TreeMapLevelBorder>
        </TreeMapLevel>
        <TreeMapLevel GroupPath="JobDescription" HeaderHeight="45">
            <TreeMapHeaderStyle Size="15px" Color="#333333">
            </TreeMapHeaderStyle>
            <TreeMapLevelBorder Color="black" Width="0.5">
            </TreeMapLevelBorder>
        </TreeMapLevel>
        <TreeMapLevel GroupPath="JobGroup" HeaderHeight="40">
            <TreeMapHeaderStyle Size="15px" FontStyle="italic">
            </TreeMapHeaderStyle>
            <TreeMapLevelBorder Color="black" Width="0.5">
            </TreeMapLevelBorder>
        </TreeMapLevel>
    </TreeMapLevels>
</SfTreeMap>
```

**Header Format Customization:**

Use `HeaderFormat` to customize the displayed text with data placeholders:

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLevel GroupPath="Country" HeaderFormat="${Country}-${EmployeeCount}">
</TreeMapLevel>
```

**Show/Hide Headers:**

Control header visibility with the `ShowHeader` property:

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLevel GroupPath="Country" ShowHeader="false">
</TreeMapLevel>
```

### Header Alignment

The `HeaderAlignment` property is used to position headers within the level area.

**Available Alignments:**
- `Near` - Align to start (left for LTR)
- `Center` - Center alignment
- `Far` - Align to end (right for LTR)

**Example:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="EmployeeCount" TValue="Employee" 
           DataSource="Employees" 
           Palette='new string[] {"#f44336", "#29b6f6", "#ab47bc", "#ffc107", "#5c6bc0", "#009688"}'>
    <TreeMapLevels>
        <TreeMapLevel GroupPath="Country" 
                     HeaderFormat="${Country}-${EmployeeCount}" 
                     HeaderAlignment="Syncfusion.Blazor.TreeMap.Alignment.Center">
            <TreeMapLevelBorder Color="black" Width="0.5">
            </TreeMapLevelBorder>
        </TreeMapLevel>
        <TreeMapLevel GroupPath="JobDescription" 
                     HeaderFormat="${JobDescription}-${EmployeeCount}" 
                     HeaderAlignment="Syncfusion.Blazor.TreeMap.Alignment.Far">
            <TreeMapLevelBorder Color="black" Width="0.5">
            </TreeMapLevelBorder>
        </TreeMapLevel>
        <TreeMapLevel GroupPath="JobGroup" 
                     HeaderFormat="${JobGroup}-${EmployeeCount}" 
                     HeaderAlignment="Syncfusion.Blazor.TreeMap.Alignment.Near">
            <TreeMapLevelBorder Color="black" Width="0.5">
            </TreeMapLevelBorder>
        </TreeMapLevel>
    </TreeMapLevels>
</SfTreeMap>
```

### Border Styling

The `TreeMapLevelBorder` component with `Color` and `Width` properties is used to customize borders for each level:

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLevel GroupPath="Country">
    <TreeMapLevelBorder Color="red" Width="2">
    </TreeMapLevelBorder>
</TreeMapLevel>
```

**Properties:**
- `Color` - Border color (hex, rgb, or color name)
- `Width` - Border thickness in pixels

## Advanced Level Features

### Header Templates

The `HeaderTemplate` property is used to create custom header content with complex layouts or dynamic data.

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLevel GroupPath="Country" TemplatePosition="LabelPosition.TopCenter">
    <HeaderTemplate>
        @{
            var data = context as Employee;
            <div style="background-color: #4CAF50; padding: 5px; color: white;">
                <strong>@data.Country</strong> - @data.EmployeeCount employees
            </div>
        }
    </HeaderTemplate>
</TreeMapLevel>
```

### Conditional Level Display

Use conditional logic to dynamically control level visibility with `TreeMapLevel` components based on data or user interaction:

```cshtml
@using Syncfusion.Blazor.TreeMap

<button @onclick="ToggleLevels">Toggle Detailed View</button>

<SfTreeMap WeightValuePath="EmployeeCount" TValue="Employee" DataSource="Employees">
    <TreeMapLevels>
        <TreeMapLevel GroupPath="Country"></TreeMapLevel>
        @if (ShowDetailedLevels)
        {
            <TreeMapLevel GroupPath="JobDescription"></TreeMapLevel>
            <TreeMapLevel GroupPath="JobGroup"></TreeMapLevel>
        }
    </TreeMapLevels>
</SfTreeMap>

@code {
    private bool ShowDetailedLevels { get; set; } = false;
    
    private void ToggleLevels()
    {
        ShowDetailedLevels = !ShowDetailedLevels;
    }
}

```

### Dynamic Level Generation

Use `TreeMapLevel` with `GroupPath` in loops to generate levels programmatically based on data structure:

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="EmployeeCount" TValue="Employee" DataSource="Employees">
    <TreeMapLevels>
        @foreach (var groupPath in GetGroupPaths())
        {
            <TreeMapLevel GroupPath="@groupPath">
                <TreeMapLevelBorder Color="black" Width="0.5">
                </TreeMapLevelBorder>
            </TreeMapLevel>
        }
    </TreeMapLevels>
</SfTreeMap>
@code {
    private List<string> GetGroupPaths()
    {
        return new List<string> { "Country", "JobDescription", "JobGroup" };
    }
}

```

## Troubleshooting

### Levels Not Displaying

**Problem:** Multi-level hierarchy not appearing.

**Solutions:**
1. Verify `GroupPath` property names match data properties exactly
2. Check that data contains the specified properties
3. Ensure data has multiple unique values for grouping
4. Verify `TValue` type matches data structure

```cshtml
@code {
    // Debug: Check if grouping properties exist
    protected override void OnAfterRender(bool firstRender)
    {
        if (firstRender)
        {
            foreach (var item in Employees)
            {
                Console.WriteLine($"Country: {item.Country}, Job: {item.JobDescription}");
            }
        }
    }
}
```

### Headers Not Showing

**Problem:** Level headers are missing or not visible.

**Solutions:**
1. Check `ShowHeader` property (default is true)
2. Verify `HeaderHeight` is set appropriately
3. Ensure header text color contrasts with background
4. Check if headers are outside visible area

### Incorrect Level Colors

**Problem:** Levels don't display expected colors.

**Solutions:**
1. Set `Fill` property for each level
2. Use `Palette` for automatic coloring
3. Check CSS conflicts
4. Verify color values are valid

### Group Gap Too Large/Small

**Problem:** Spacing between groups is not appropriate.

**Solutions:**
1. Adjust `GroupGap` value (typical range: 0-20)
2. Consider container size when setting gap
3. Test with different data sizes
4. Keep gaps consistent across levels

## Best Practices

### Layout Selection
1. ✅ Use **Squarified** as default for balanced readability
2. ✅ Choose **SliceAndDiceAuto** for responsive designs
3. ✅ Match layout to data characteristics and container shape
4. ✅ Test layouts with various data sizes

### Level Configuration
1. ✅ Limit to 3-4 levels for optimal readability
2. ✅ Use clear, descriptive `GroupPath` names
3. ✅ Apply consistent styling across levels
4. ✅ Provide visual hierarchy through colors and sizes

### Header Design
1. ✅ Keep headers concise and informative
2. ✅ Use appropriate font sizes for hierarchy
3. ✅ Ensure headers don't overlap content
4. ✅ Consider header height in overall layout

### Visual Hierarchy
1. ✅ Use darker/bolder colors for higher levels
2. ✅ Decrease font size from outer to inner levels
3. ✅ Apply consistent border styling
4. ✅ Use gap spacing to improve readability

### Performance
1. ✅ Limit level depth for large datasets
2. ✅ Consider data aggregation for deep hierarchies
3. ✅ Test rendering with maximum expected data
4. ✅ Optimize header templates for performance

### Responsive Design
1. ✅ Use `SliceAndDiceAuto` for adaptive layouts
2. ✅ Test on various screen sizes
3. ✅ Adjust `GroupGap` based on container size
4. ✅ Consider mobile-friendly header heights

### Accessibility
1. ✅ Ensure sufficient color contrast for headers
2. ✅ Provide meaningful `GroupPath` values
3. ✅ Test with screen readers
4. ✅ Use descriptive header formats

### Data Preparation
1. ✅ Validate all grouping properties exist
2. ✅ Handle missing or null values gracefully
3. ✅ Ensure consistent data types
4. ✅ Test with edge cases (empty groups, single items)
