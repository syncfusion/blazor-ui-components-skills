# Color Mapping

## Table of Contents
- [Overview](#overview)
- [Range Color Mapping](#range-color-mapping)
- [Equal Color Mapping](#equal-color-mapping)
- [Desaturation Mapping](#desaturation-mapping)
- [Palette Mapping](#palette-mapping)
- [Binding Colors from Data](#binding-colors-from-data)

## Overview
Describes strategies to map colors to TreeMap items based on numeric ranges or explicit color values in data.

## Range Color Mapping
- Define ranges with start/end and corresponding colors to visualize value gradients.

## Equal Color Mapping
- Map discrete value buckets to fixed colors.

## Desaturation Mapping
- Use desaturation techniques for subtle value gradations across a palette.

## Palette Mapping
- Provide an explicit palette array for categorical coloring.

## Binding Colors from Data
- Bind a color field from your data when each item already contains a color value.
# Color Mapping in Blazor TreeMap Component

## Table of Contents

- [Overview](#overview)
- [Color Mapping Types](#color-mapping-types)
- [Range Color Mapping](#range-color-mapping)
  - [Basic Range Mapping](#basic-range-mapping)
  - [Multiple Ranges](#multiple-ranges)
  - [Edge Cases and Overlaps](#edge-cases-and-overlaps)
- [Equal Color Mapping](#equal-color-mapping)
  - [Categorical Data Coloring](#categorical-data-coloring)
  - [Multiple Categories](#multiple-categories)
- [Desaturation Color Mapping](#desaturation-color-mapping)
  - [Single Color Desaturation](#single-color-desaturation)
  - [Opacity Range Configuration](#opacity-range-configuration)
- [Desaturation with Multiple Colors](#desaturation-with-multiple-colors)
  - [Gradient Effects](#gradient-effects)
  - [Multi-Color Ranges](#multi-color-ranges)
- [Palette Color Mapping](#palette-color-mapping)
  - [Using Palette Arrays](#using-palette-arrays)
  - [Built-in Palettes](#built-in-palettes)
- [Binding Colors from Data](#binding-colors-from-data)
- [Handling Excluded Items](#handling-excluded-items)
- [Combining Color Mapping Techniques](#combining-color-mapping-techniques)
- [Troubleshooting](#troubleshooting)
- [Best Practices](#best-practices)

## Overview

Color mapping is a powerful feature that allows you to customize the color of TreeMap items based on data values or categories. It provides visual differentiation, highlights important data points, and creates intuitive data visualizations. The TreeMap component supports multiple color mapping strategies, each suited for different data types and visualization goals.

**Why Use Color Mapping?**
- Visual data segmentation
- Highlighting ranges or thresholds
- Categorical differentiation
- Creating heat maps
- Improving data readability
- Emphasizing outliers

## Color Mapping Types

The Blazor TreeMap supports five primary color mapping approaches:

| Type | Use Case | Data Type | Color Source |
|------|----------|-----------|--------------|
| **Range** | Numeric ranges and thresholds | Numeric | Configuration |
| **Equal** | Categorical grouping | String/Enum | Configuration |
| **Desaturation** | Gradient within ranges | Numeric | Opacity variation |
| **Palette** | Automatic distribution | Any | Predefined array |
| **ColorValuePath** | Direct data binding | Any | Data property |

## Range Color Mapping

Range color mapping applies colors based on numeric value ranges using the `RangeColorValuePath` property to evaluate data values. Define mapping ranges with `StartRange` and `EndRange` properties, and specify colors using the `Color` property. Perfect for creating heat maps and highlighting thresholds.

### Basic Range Mapping

Define color ranges using the `StartRange` and `EndRange` properties within `TreeMapLeafColorMapping` components. Set the `RangeColorValuePath` property at the SfTreeMap level to specify which data field will be evaluated against these ranges. Use the `Color` property to assign a color array for each range.

**Example: Two-Range Color Mapping**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" TValue="Fruit" 
           DataSource="Fruits" RangeColorValuePath="Count">
    <TreeMapLeafItemSettings LabelPath="FruitName">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping StartRange="500" EndRange="3000" 
                                    Color='new string[] { "Orange" }'>
            </TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping StartRange="3000" EndRange="5000" 
                                    Color='new string[] { "Green" }'>
            </TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
</SfTreeMap>

@code {
    public class Fruit
    {
        public string FruitName { get; set; }
        public double Count { get; set; }
    }
    
    public List<Fruit> Fruits = new List<Fruit> {
        new Fruit { FruitName="Apple", Count=5000 },
        new Fruit { FruitName="Mango", Count=3000 },
        new Fruit { FruitName="Orange", Count=2300 },
        new Fruit { FruitName="Banana", Count=500 },
        new Fruit { FruitName="Grape", Count=4300 },
        new Fruit { FruitName="Papaya", Count=1200 },
        new Fruit { FruitName="Melon", Count=4500 }
    };
}
```

**Key Properties:**
- `RangeColorValuePath`: Specifies which data field contains values for range evaluation
- `StartRange`: Minimum value (inclusive)
- `EndRange`: Maximum value (inclusive)
- `Color`: Array of color strings

### Multiple Ranges

Create sophisticated visualizations with multiple color ranges by defining multiple `TreeMapLeafColorMapping` components with different `StartRange` and `EndRange` values. Each mapping applies its `Color` property when the data value falls within that range. Define mappings in order, as the first matching range takes precedence.

**Example: Five-Tier TreeMap**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Sales" TValue="Product" 
           DataSource="Products" RangeColorValuePath="Sales">
    <TreeMapLeafItemSettings LabelPath="Name">
        <TreeMapLeafColorMappings>
            <!-- Critical Low -->
            <TreeMapLeafColorMapping StartRange="0" EndRange="1000" 
                                    Color='new string[] { "#D32F2F" }'>
            </TreeMapLeafColorMapping>
            <!-- Low -->
            <TreeMapLeafColorMapping StartRange="1000" EndRange="2500" 
                                    Color='new string[] { "#FF9800" }'>
            </TreeMapLeafColorMapping>
            <!-- Medium -->
            <TreeMapLeafColorMapping StartRange="2500" EndRange="4000" 
                                    Color='new string[] { "#FDD835" }'>
            </TreeMapLeafColorMapping>
            <!-- Good -->
            <TreeMapLeafColorMapping StartRange="4000" EndRange="5500" 
                                    Color='new string[] { "#7CB342" }'>
            </TreeMapLeafColorMapping>
            <!-- Excellent -->
            <TreeMapLeafColorMapping StartRange="5500" EndRange="8000" 
                                    Color='new string[] { "#388E3C" }'>
            </TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible="true" Format="${Name}: ${Sales}">
    </TreeMapTooltipSettings>
</SfTreeMap>

@code {
    public class Product
    {
        public string Name { get; set; }
        public double Sales { get; set; }
    }
    
    public List<Product> Products = new List<Product> {
        new Product { Name="Product A", Sales=7500 },
        new Product { Name="Product B", Sales=3200 },
        new Product { Name="Product C", Sales=1200 },
        new Product { Name="Product D", Sales=5800 },
        new Product { Name="Product E", Sales=2100 }
    };
}
```

### Edge Cases and Overlaps

**Important Range Rules:**
1. Ranges are **inclusive** on both ends
2. Value at exact boundary belongs to first matching range
3. Overlapping ranges: first match wins
4. Missing ranges use default fill color

**Example: Handling Boundary Values**

```cshtml
@using Syncfusion.Blazor.TreeMap

<!-- Value = 3000 will match first range due to inclusive EndRange -->
<TreeMapLeafColorMapping StartRange="500" EndRange="3000" Color='new string[] { "Orange" }'></TreeMapLeafColorMapping>
<TreeMapLeafColorMapping StartRange="3000" EndRange="5000" Color='new string[] { "Green" }'></TreeMapLeafColorMapping>

<!-- Better: Use non-overlapping ranges -->
<TreeMapLeafColorMapping StartRange="500" EndRange="2999" Color='new string[] { "Orange" }'></TreeMapLeafColorMapping>
<TreeMapLeafColorMapping StartRange="3000" EndRange="5000" Color='new string[] { "Green" }'></TreeMapLeafColorMapping>
```

## Equal Color Mapping

Equal color mapping assigns colors based on exact value matches using the `EqualColorValuePath` property to specify which data field contains categorical values. Use the `LeafValue` property to match specific values and the `Color` property to assign corresponding colors. Ideal for categorical data visualization.

### Categorical Data Coloring

Map specific values to colors using the `LeafValue` property to match categorical values, and the `Color` property to assign the corresponding color. Set the `EqualColorValuePath` property at the SfTreeMap level to specify which data field contains the categorical values to be matched.

**Example: Brand-Based Coloring**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" TValue="Car" 
           DataSource="Cars" EqualColorValuePath="Brand">
    <TreeMapLeafItemSettings LabelPath="Name">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping LeafValue="Ford" 
                                    Color='new string[] { "green" }'>
            </TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Audi" 
                                    Color='new string[] { "red" }'>
            </TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Maruti" 
                                    Color='new string[]{ "orange" }'>
            </TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
</SfTreeMap>

@code {
    public class Car
    {
        public string Name { get; set; }
        public int Count { get; set; }
        public string Brand { get; set; }
    }
    
    public List<Car> Cars = new List<Car> {
        new Car { Name="Mustang", Brand="Ford", Count=232 },
        new Car { Name="EcoSport", Brand="Ford", Count=121 },
        new Car { Name="Swift", Brand="Maruti", Count=143 },
        new Car { Name="Baleno", Brand="Maruti", Count=454 },
        new Car { Name="Vitara Brezza", Brand="Maruti", Count=545 },
        new Car { Name="A3 Cabriolet", Brand="Audi", Count=123 },
        new Car { Name="RS7 Sportback", Brand="Audi", Count=523 }
    };
}
```

### Multiple Categories

Handle complex categorical data with many distinct values by creating multiple `TreeMapLeafColorMapping` components. Use the `LeafValue` property for each category and the `Color` property to assign a specific color to that category. Set `EqualColorValuePath` to specify the data field containing these category values.

**Example: Status-Based Coloring**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Amount" TValue="Transaction" 
           DataSource="Transactions" EqualColorValuePath="Status">
    <TreeMapLeafItemSettings LabelPath="TransactionID">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping LeafValue="Completed" 
                                    Color='new string[] { "#4CAF50" }'>
            </TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Pending" 
                                    Color='new string[] { "#FFC107" }'>
            </TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Failed" 
                                    Color='new string[] { "#F44336" }'>
            </TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Cancelled" 
                                    Color='new string[] { "#9E9E9E" }'>
            </TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Processing" 
                                    Color='new string[] { "#2196F3" }'>
            </TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible="true" 
                           Format="${TransactionID}<br>Status: ${Status}<br>Amount: $${Amount}">
    </TreeMapTooltipSettings>
</SfTreeMap>

@code {
    public class Transaction
    {
        public string TransactionID { get; set; }
        public double Amount { get; set; }
        public string Status { get; set; }
    }
    
    public List<Transaction> Transactions = new List<Transaction> {
        new Transaction { TransactionID="TXN001", Amount=5000, Status="Completed" },
        new Transaction { TransactionID="TXN002", Amount=3200, Status="Pending" },
        new Transaction { TransactionID="TXN003", Amount=1500, Status="Failed" },
        new Transaction { TransactionID="TXN004", Amount=7800, Status="Completed" },
        new Transaction { TransactionID="TXN005", Amount=2100, Status="Processing" },
        new Transaction { TransactionID="TXN006", Amount=900, Status="Cancelled" }
    };
}
```

**Equal Color Mapping Tips:**
- Values must match exactly (case-sensitive)
- Undefined values use default fill color
- Works with strings, numbers, and enums
- Consider using enums for type safety

## Desaturation Color Mapping

Desaturation creates gradient effects by varying opacity within color ranges using the `MinOpacity` and `MaxOpacity` properties. Combined with `StartRange` and `EndRange` properties, opacity values are applied linearly across the value range to create subtle color gradations.

### Single Color Desaturation

Apply a single color with varying opacity based on value by specifying the `Color` property and using the `MinOpacity` and `MaxOpacity` properties within `TreeMapLeafColorMapping`. Define the range using `StartRange` and `EndRange`, and the opacity will vary linearly across that range.

**Example: Opacity Gradient**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" TValue="Fruit" 
           DataSource="Fruits" RangeColorValuePath="Count">
    <TreeMapLeafItemSettings LabelPath="FruitName">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping StartRange="500" EndRange="3000" 
                                    MinOpacity="0.2" MaxOpacity="0.5" 
                                    Color='new string[] { "Orange" }'>
            </TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping StartRange="3000" EndRange="5000" 
                                    MinOpacity="0.5" MaxOpacity="0.8" 
                                    Color='new string[] { "Green" }'>
            </TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible="true" Format="${FruitName}: ${Count}">
    </TreeMapTooltipSettings>
</SfTreeMap>
```

### Opacity Range Configuration

**Opacity Properties:**
- `MinOpacity`: Starting opacity value (0.0 to 1.0) applied at the `StartRange` value
- `MaxOpacity`: Ending opacity value (0.0 to 1.0) applied at the `EndRange` value
- Opacity is applied linearly across the value range defined by `StartRange` and `EndRange`
- Works with any base color specified in the `Color` property
- Used in conjunction with `StartRange` and `EndRange` for range-based gradient effects

**Calculating Opacity:**

For a value `V` in range `[StartRange, EndRange]`:
```
Opacity = MinOpacity + ((V - StartRange) / (EndRange - StartRange)) * (MaxOpacity - MinOpacity)
```

**Example: Precise Opacity Control**

```cshtml
@using Syncfusion.Blazor.TreeMap

<!-- Very Light to Medium -->
<TreeMapLeafColorMapping StartRange="0" EndRange="1000" 
                        MinOpacity="0.1" MaxOpacity="0.4" 
                        Color='new string[] { "#2196F3" }'>
</TreeMapLeafColorMapping>

<!-- Medium to Dark -->
<TreeMapLeafColorMapping StartRange="1000" EndRange="3000" 
                        MinOpacity="0.4" MaxOpacity="0.7" 
                        Color='new string[] { "#2196F3" }'>
</TreeMapLeafColorMapping>

<!-- Dark to Very Dark -->
<TreeMapLeafColorMapping StartRange="3000" EndRange="5000" 
                        MinOpacity="0.7" MaxOpacity="1.0" 
                        Color='new string[] { "#2196F3" }'>
</TreeMapLeafColorMapping>
```

## Desaturation with Multiple Colors

Create sophisticated gradient effects using multiple colors within a range.

### Gradient Effects

When multiple colors are specified in the `Color` property as an array, the TreeMap creates smooth transitions between those colors. Combine with `MinOpacity` and `MaxOpacity` properties to create sophisticated gradient effects with opacity variations across the range defined by `StartRange` and `EndRange`.

**Example: Two-Color Gradient**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" TValue="Fruit" 
           DataSource="Fruits" RangeColorValuePath="Count">
    <TreeMapLeafItemSettings LabelPath="FruitName">
        <TreeMapLeafColorMappings>
            <!-- Orange to Pink gradient -->
            <TreeMapLeafColorMapping StartRange="500" EndRange="3000" 
                                    MinOpacity="0.2" MaxOpacity="0.5"
                                    Color='new string[]{ "orange", "pink" }'>
            </TreeMapLeafColorMapping>
            <!-- Three-color gradient: Green to Red to Blue -->
            <TreeMapLeafColorMapping StartRange="3000" EndRange="5000" 
                                    MinOpacity="0.2" MaxOpacity="0.5"
                                    Color='new string[]{ "green", "red", "blue" }'>
            </TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

### Multi-Color Ranges

Use three or more colors in the `Color` property array for complex gradients. Define separate `TreeMapLeafColorMapping` components with different `StartRange` and `EndRange` values, each containing multiple colors in the `Color` property. Optionally use `MinOpacity` and `MaxOpacity` for additional opacity-based gradation.

**Example: Temperature Heat Map**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Temperature" TValue="Location" 
           DataSource="WeatherData" RangeColorValuePath="Temperature">
    <TreeMapLeafItemSettings LabelPath="City">
        <TreeMapLeafColorMappings>
            <!-- Cold: Blue to Cyan -->
            <TreeMapLeafColorMapping StartRange="-20" EndRange="0"
                                    MinOpacity="0.2" MaxOpacity="0.5"
                                    Color='new string[]{ "#0D47A1", "#29B6F6" }'>
            </TreeMapLeafColorMapping>
            <!-- Cool to Mild: Cyan to Yellow -->
            <TreeMapLeafColorMapping StartRange="0" EndRange="20" 
                                    MinOpacity="0.2" MaxOpacity="0.5"
                                    Color='new string[]{ "#00BCD4", "#FFEB3B" }'>
            </TreeMapLeafColorMapping>
            <!-- Warm: Yellow to Orange -->
            <TreeMapLeafColorMapping StartRange="20" EndRange="35" 
                                    MinOpacity="0.2" MaxOpacity="0.5"
                                    Color='new string[]{ "#FFC107", "#FF5722" }'>
            </TreeMapLeafColorMapping>
            <!-- Hot: Orange to Red to Dark Red -->
            <TreeMapLeafColorMapping StartRange="35" EndRange="50" 
                                    MinOpacity="0.2" MaxOpacity="0.5"
                                    Color='new string[]{ "#FF5722", "#F44336", "#B71C1C" }'>
            </TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible="true" 
                           Format="${City}: ${Temperature}°C">
    </TreeMapTooltipSettings>
</SfTreeMap>

@code {
    public class Location
    {
        public string City { get; set; }
        public double Temperature { get; set; }
    }
    
    public List<Location> WeatherData = new List<Location> {
        new Location { City="Moscow", Temperature=-15 },
        new Location { City="New York", Temperature=8 },
        new Location { City="London", Temperature=12 },
        new Location { City="Dubai", Temperature=42 },
        new Location { City="Singapore", Temperature=30 }
    };
}
```

## Palette Color Mapping

Palette mapping automatically distributes colors from a predefined array across all items using the `Palette` property. Define a color palette array at the TreeMap component level for automatic sequential color distribution to TreeMap elements.

### Using Palette Arrays

Define a palette using the `Palette` property at the SfTreeMap level with an array of color values for automatic color distribution. The TreeMap will sequentially assign colors from the palette to items in the order they appear in the data source.

**Example: Custom Palette**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" TValue="Car" DataSource="Cars" 
           Palette='new string[] { "#FFB3B3", "#2196F3", "#E67E22", "#9B59B6" }'>
    <TreeMapLeafItemSettings LabelPath="Name">
    </TreeMapLeafItemSettings>
</SfTreeMap>

@code {
    public class Car
    {
        public string Name { get; set; }
        public string Brand { get; set; }
        public int Count { get; set; }
    }
    
    public List<Car> Cars = new List<Car> {
        new Car { Name="Mustang", Brand="Ford", Count=232},
        new Car { Name="EcoSport", Brand="Ford", Count=121},
        new Car { Name="Swift", Brand="Maruti", Count=143},
        new Car { Name="Baleno", Brand="Maruti", Count=454},
        new Car { Name="Vitara Brezza", Brand="Maruti", Count=545},
        new Car { Name="A3 Cabriolet", Brand="Audi", Count=123},
        new Car { Name="RS7 Sportback", Brand="Audi", Count=523 }
    };
}
```

### Built-in Palettes

**Popular Palette Schemes:**

```cshtml
@code {
    // Material Design
    string[] MaterialPalette = new string[] { 
        "#F44336", "#E91E63", "#9C27B0", "#673AB7", 
        "#3F51B5", "#2196F3", "#03A9F4", "#00BCD4" 
    };
    
    // Flat UI
    string[] FlatPalette = new string[] { 
        "#1ABC9C", "#2ECC71", "#3498DB", "#9B59B6", 
        "#34495E", "#F1C40F", "#E67E22", "#E74C3C" 
    };
    
    // Gradient
    string[] GradientPalette = new string[] { 
        "#C33764", "#AB3566", "#993367", "#853169", 
        "#742F6A", "#632D6C", "#532C6D", "#412A6F", 
        "#312870", "#1D2671" 
    };
    
    // Monochrome Blue
    string[] MonochromePalette = new string[] { 
        "#0D47A1", "#1565C0", "#1976D2", "#1E88E5", 
        "#2196F3", "#42A5F5", "#64B5F6", "#90CAF9" 
    };
}
```

## Binding Colors from Data

Directly bind colors from your data source by setting the `ColorValuePath` property to specify a data field containing color values. Each TreeMap item will use the color specified in its corresponding data record.

**Example: Data-Driven Colors**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" TValue="Fruit" 
           DataSource="Fruits" ColorValuePath="Color">
    <TreeMapLeafItemSettings LabelPath="Name">
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true">
    </TreeMapLegendSettings>
</SfTreeMap>

@code {
    public class Fruit
    {
        public string Name { get; set; }
        public double Count { get; set; }
        public string Color { get; set; }
    }
    
    public List<Fruit> Fruits = new List<Fruit> {
        new Fruit { Name="Apple", Count=5000, Color = "red" },
        new Fruit { Name="Mango", Count=3000, Color="blue" },
        new Fruit { Name="Orange", Count=2300, Color="green" },
        new Fruit { Name="Banana", Count=500 , Color="yellow"},
        new Fruit { Name="Grape", Count=4300 , Color="orange"},
        new Fruit { Name="Papaya",Count=1200 , Color="pink"},
        new Fruit { Name="Melon", Count=4500, Color="violet" }
    };
}
```

**When to Use ColorValuePath:**
- Dynamic color assignment
- User-customizable colors
- Colors from external sources
- Complex business logic for colors
- Database-driven color schemes

## Handling Excluded Items

Items outside defined ranges can be assigned a specific color by creating a `TreeMapLeafColorMapping` without `StartRange` and `EndRange` properties. This catch-all mapping will apply its `Color` property to any values that don't match the defined range conditions.

**Example: Catch-All Color for Unmapped Values**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" TValue="Fruit" 
           DataSource="Fruits" RangeColorValuePath="Count">
    <TreeMapLeafItemSettings LabelPath="Name">
        <TreeMapLeafColorMappings>
            <!-- Low Range -->
            <TreeMapLeafColorMapping StartRange="500" EndRange="3000" 
                                    Color='new string[] { "Orange" }'>
            </TreeMapLeafColorMapping>
            <!-- Medium Range -->
            <TreeMapLeafColorMapping StartRange="3000" EndRange="4000" 
                                    Color='new string[]{ "Green" }'>
            </TreeMapLeafColorMapping>
            <!-- Catch-all for values outside ranges (e.g., >4000) -->
            <TreeMapLeafColorMapping Color='new string[]{ "purple" }'>
            </TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true">
    </TreeMapLegendSettings>
</SfTreeMap>
```

**Rules for Excluded Items:**
- Mapping without StartRange/EndRange catches all unmatched values
- Should be defined last in the mappings
- Useful for outliers or unexpected values
- Appears in legend if legend is enabled

## Combining Color Mapping Techniques

Mix different color mapping strategies for complex visualizations by using multiple `TreeMapLeafColorMapping` components with different property combinations. Combine `RangeColorValuePath` with `StartRange`/`EndRange` mappings for specific ranges, and add a catch-all mapping using `Color` property without range properties. You can also use the `Palette` property alongside color mappings as a fallback.

**Example: Range Mapping with Palette Fallback**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Sales" TValue="Product" 
           DataSource="Products" 
           RangeColorValuePath="Sales"
           Palette='new string[] { "#95A5A6", "#7F8C8D" }'>
    <TreeMapLeafItemSettings LabelPath="Name">
        <TreeMapLeafColorMappings>
            <!-- High performers -->
            <TreeMapLeafColorMapping StartRange="5000" EndRange="10000" 
                                    Color='new string[] { "#27AE60" }'>
            </TreeMapLeafColorMapping>
            <!-- Medium performers -->
            <TreeMapLeafColorMapping StartRange="2000" EndRange="5000" 
                                    Color='new string[] { "#F39C12" }'>
            </TreeMapLeafColorMapping>
            <!-- Low performers (will use palette colors) -->
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

## Troubleshooting

### Colors Not Applied

**Problem:** TreeMap shows default colors instead of mapped colors.

**Solutions:**
1. Verify `RangeColorValuePath` or `EqualColorValuePath` is set correctly
2. Check that color mapping is inside `TreeMapLeafItemSettings`
3. Ensure data values fall within defined ranges
4. Validate color values are valid CSS colors
5. Check for typos in property names

```cshtml
@code {
    // Debug color mapping
    protected override void OnAfterRender(bool firstRender)
    {
        if (firstRender)
        {
            foreach (var item in Fruits)
            {
                Console.WriteLine($"{item.Name}: Count={item.Count}");
            }
        }
    }
}
```

### Equal Color Mapping Not Working

**Problem:** Categorical colors not displaying correctly.

**Solutions:**
1. Ensure `EqualColorValuePath` matches data property exactly
2. Check `LeafValue` matches data values (case-sensitive)
3. Verify data contains expected categorical values
4. Test with simple string values first

### Desaturation Not Visible

**Problem:** Opacity gradient not apparent.

**Solutions:**
1. Increase difference between `MinOpacity` and `MaxOpacity`
2. Use darker base colors for better visibility
3. Check background color contrast
4. Ensure values span the full range
5. Verify opacity values are between 0.0 and 1.0

### Legend Not Matching Colors

**Problem:** Legend shows different colors than TreeMap.

**Solutions:**
1. Ensure legend is enabled: `<TreeMapLegendSettings Visible="true">`
2. Check if `ShowLegend` is set for color mappings
3. Verify color mapping configuration
4. Test with simpler color schemes

## Best Practices

### Range Color Mapping
1. ✅ Use non-overlapping ranges
2. ✅ Cover full expected data range
3. ✅ Provide catch-all for outliers
4. ✅ Use intuitive color progressions (light to dark)
5. ✅ Test with edge case values
6. ✅ Document range meanings

### Equal Color Mapping
1. ✅ Use distinct, contrasting colors
2. ✅ Limit categories for clarity (< 10 recommended)
3. ✅ Consider colorblind-friendly palettes
4. ✅ Provide legend for category identification
5. ✅ Handle missing categories gracefully
6. ✅ Use semantic colors (red=danger, green=success)

### Desaturation
1. ✅ Use opacity range of at least 0.3-0.4 difference
2. ✅ Start with MinOpacity ≥ 0.2 for visibility
3. ✅ Keep MaxOpacity ≤ 0.9 for label readability
4. ✅ Use darker colors for better gradient visibility
5. ✅ Test on different backgrounds

### Palette Selection
1. ✅ Choose colors with sufficient contrast
2. ✅ Use 5-10 colors for optimal variety
3. ✅ Consider brand colors
4. ✅ Test accessibility (WCAG AA compliance)
5. ✅ Avoid overly bright or neon colors
6. ✅ Use established color schemes

### Performance
1. ✅ Minimize number of color mappings
2. ✅ Avoid complex gradient calculations
3. ✅ Use simple color values (hex preferred)
4. ✅ Cache palette arrays
5. ✅ Test with maximum expected data size

### Accessibility
1. ✅ Ensure 4.5:1 contrast ratio for text
2. ✅ Don't rely solely on color for information
3. ✅ Provide legends for color meanings
4. ✅ Use patterns in addition to colors when possible
5. ✅ Test with colorblind simulation tools
6. ✅ Support high contrast modes

### Maintenance
1. ✅ Document color scheme meanings
2. ✅ Use constants for color values
3. ✅ Centralize palette definitions
4. ✅ Version control color schemes
5. ✅ Provide theme switching capability
6. ✅ Keep color logic separate from data logic
