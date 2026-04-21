# Legend

## Table of Contents
- [Overview](#overview)
- [Legend Types](#legend-types)
- [Positioning and Alignment](#positioning-and-alignment)
- [Smart Legend & Interactivity](#smart-legend--interactivity)
- [Templates and Styling](#templates-and-styling)

## Overview
Explains `TreeMapLegendSettings`, `TreeMapLegendBorder`, `TreeMapLegendLocation`, `TreeMapLegendTitle`, and `TreeMapLegendTextStyle` for positioning, filtering, and styling legends.

## Legend Types
- Continuous range legends for gradient mappings and categorical legends for palette mappings using `LegendMode`, `LegendPosition`, `LegendOrientation`, and `LegendShape`.

## Positioning and Alignment
- Configure legend placement with `Position` and `Alignment` to avoid overlap with tiles.

## Smart Legend & Interactivity
- Use `Mode`, `RemoveDuplicateLegend`, `InvertedPointer`, and `ShowLegendPath` to control legend interactivity.

## Templates and Styling
- Customize `Fill`, `ImageUrl`, `ShapeHeight`, `ShapeWidth`, `ShapePadding`, `Background`, `Width`, and `Height`.
# Legend Configuration in Blazor TreeMap

A comprehensive guide for implementing, customizing, and managing legends in the Syncfusion Blazor TreeMap component to provide visual data interpretation and interactive filtering capabilities.

## Table of Contents

- [Overview](#overview)
- [Legend Types and Modes](#legend-types-and-modes)
  - [Default Legend Mode](#default-legend-mode)
  - [Interactive Legend Mode](#interactive-legend-mode)
- [Legend Positioning and Alignment](#legend-positioning-and-alignment)
  - [Position Options](#position-options)
  - [Alignment Options](#alignment-options)
  - [Auto-Responsive Positioning](#auto-responsive-positioning)
- [Legend Size Configuration](#legend-size-configuration)
  - [Fixed Size](#fixed-size)
  - [Percentage-Based Size](#percentage-based-size)
  - [Legend Paging](#legend-paging)
- [Legend Shapes](#legend-shapes)
  - [Available Shapes](#available-shapes)
  - [Custom Shape Configuration](#custom-shape-configuration)
- [Legend for Color Mappings](#legend-for-color-mappings)
  - [Equal Color Mapping Legend](#equal-color-mapping-legend)
  - [Range Color Mapping Legend](#range-color-mapping-legend)
  - [Desaturation Mapping Legend](#desaturation-mapping-legend)
- [Legend Customization](#legend-customization)
  - [Legend Border](#legend-border)
  - [Legend Title](#legend-title)
  - [Legend Text Style](#legend-text-style)
- [Legend Item Visibility Control](#legend-item-visibility-control)
  - [Hiding Specific Legend Items](#hiding-specific-legend-items)
  - [Data-Driven Visibility](#data-driven-visibility)
  - [Removing Duplicate Legends](#removing-duplicate-legends)
- [Legend Data Binding](#legend-data-binding)
  - [ValuePath Configuration](#valuepath-configuration)
  - [Custom Legend Text](#custom-legend-text)
- [Smart Legend with Filtering](#smart-legend-with-filtering)
- [RTL Support](#rtl-support)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

Legends provide visual interpretation of TreeMap data by displaying symbols and labels that correspond to different categories, ranges, or values. The Syncfusion Blazor TreeMap component offers extensive legend configuration including multiple display modes, positioning options, interactive features, and full customization capabilities.

**Key Features:**
- Default and interactive legend modes
- 6 positioning options with 3 alignment choices
- 10+ shape options for legend symbols
- Automatic paging for large legend sets
- Smart filtering with legend item toggle
- Data-driven visibility control
- Full styling and customization
- RTL support for internationalization

## Legend Types and Modes
Configure legend appearance using `Mode` property to switch between default and interactive modes.

### Default Legend Mode
The default legend mode displays static symbols and labels representing data categories. Uses `Mode="LegendMode.Default"` property.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Cars" TValue="Car" WeightValuePath="Count" EqualColorValuePath="Brand">
    <TreeMapLeafItemSettings LabelPath="Name">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping LeafValue="Ford" Color='new string[]{ "green" }'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Audi" Color='new string[]{ "red" }'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Maruti" Color='new string[]{ "orange" }'></TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Top">
    </TreeMapLegendSettings>
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
        new Car { Name="A3 Cabriolet", Brand="Audi", Count=123}
    };
}
```

**Result:** Static legend displays at top with three entries: Ford (green), Audi (red), Maruti (orange).

### Interactive Legend Mode
Interactive mode displays an arrow indicator on the legend item when hovering over corresponding TreeMap items, providing visual connection between data and legend. Uses `Mode="LegendMode.Interactive"` property.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Cars" TValue="Car" WeightValuePath="Count" EqualColorValuePath="Brand">
    <TreeMapLeafItemSettings LabelPath="Name">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping LeafValue="Ford" Color='new string[]{ "green" }'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Audi" Color='new string[]{ "red" }'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Maruti" Color='new string[]{ "orange" }'></TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Top" Mode="Syncfusion.Blazor.TreeMap.LegendMode.Interactive">
    </TreeMapLegendSettings>
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

**Behavior:** 
- Hover over a TreeMap item → corresponding legend shows arrow indicator
- Hover over legend item → corresponding TreeMap items highlight

## Legend Positioning and Alignment
Configure legend placement and alignment using `Position` and `Alignment` properties to control legend location and item distribution.

### Position Options
The `Position` property controls where the legend appears relative to the TreeMap using values: `Top`, `Bottom`, `Left`, `Right`, `Float`, or `Auto`.

**Available Positions:**

| Position | Description | Legend Item Layout |
|----------|-------------|-------------------|
| `Top` | Above TreeMap | Horizontal rows |
| `Bottom` | Below TreeMap | Horizontal rows |
| `Left` | Left side of TreeMap | Vertical columns |
| `Right` | Right side of TreeMap | Vertical columns |
| `Float` | Absolute positioning | Custom layout |
| `Auto` | Responsive positioning | Based on available space |

**Top Position:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Fruits" TValue="Fruit" WeightValuePath="Count" RangeColorValuePath="Count">
    <TreeMapLeafItemSettings LabelPath="FruitName">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping StartRange="500" EndRange="3000" Color='new string[] { "Orange" }'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping StartRange="3000" EndRange="5000" Color='new string[] { "Green" }'></TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Top"></TreeMapLegendSettings>
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

**Bottom Position:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings Visible="true" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Bottom"></TreeMapLegendSettings>
```

**Left Position:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings Visible="true" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Left"></TreeMapLegendSettings>
```

**Right Position:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings Visible="true" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Right"></TreeMapLegendSettings>
```

### Alignment Options
The `Alignment` property controls legend item alignment within the legend container using values: `Near`, `Center`, or `Far`.

**Available Alignments:**

| Alignment | Description | Works With |
|-----------|-------------|------------|
| `Near` | Start of container | All positions |
| `Center` | Center of container | All positions |
| `Far` | End of container | All positions |

**Far Alignment (Right-aligned):**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Fruits" TValue="Fruit" WeightValuePath="Count" RangeColorValuePath="Count">
    <TreeMapLeafItemSettings LabelPath="FruitName">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping StartRange="500" EndRange="3000" Color='new string[]{"Orange"}'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping StartRange="3000" EndRange="5000" Color='new string[]{"Green"}'></TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true" Alignment="Syncfusion.Blazor.TreeMap.Alignment.Far">
    </TreeMapLegendSettings>
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

**Center Alignment:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings Visible="true" Alignment="Syncfusion.Blazor.TreeMap.Alignment.Center">
</TreeMapLegendSettings>
```

**Near Alignment (Left-aligned):**

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings Visible="true" Alignment="Syncfusion.Blazor.TreeMap.Alignment.Near">
</TreeMapLegendSettings>
```

### Auto-Responsive Positioning
The `Position="Syncfusion.Blazor.TreeMap.LegendPosition.Auto"` property automatically switches between right and bottom positions based on available space.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap 
    DataSource="Fruits" 
    TValue="Fruit" 
    WeightValuePath="Count" 
    Width="700px" 
    Height="500px" 
    Palette='new string[] { "#71B081", "#5A9A77", "#498770", "#39776C", "#266665", "#124F5E" }'>
    <TreeMapLeafItemSettings LabelPath="Name">
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Auto">
    </TreeMapLegendSettings>
</SfTreeMap>

@code {
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

**Behavior:**
- Sufficient width → Legend positioned on right
- Insufficient width → Legend positioned at bottom
- Automatically adjusts on window resize

## Legend Size Configuration
Control legend dimensions using `Height` and `Width` properties with pixel or percentage values.

### Fixed Size
Set explicit dimensions using `Height` and `Width` properties with pixel units (e.g., `"200px"`, `"50px"`).

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Cars" WeightValuePath="Count" EqualColorValuePath="Brand">
    <TreeMapLeafItemSettings LabelPath="Name">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping LeafValue="Ford" Color='new string[] { "green"}'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Audi" Color='new string[] { "red" }'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Maruti" Color='new string[] { "orange"}'></TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true" Height="50px" Width="200px" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Top">
    </TreeMapLegendSettings>
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

### Percentage-Based Size
Use percentage values for `Height` and `Width` properties for responsive legend sizing (e.g., `"10%"`, `"80%"`).

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings 
    Visible="true" 
    Height="10%" 
    Width="80%" 
    Position="Syncfusion.Blazor.TreeMap.LegendPosition.Bottom">
</TreeMapLegendSettings>
```

### Legend Paging
When legend items exceed available space, automatic paging enables navigation through `Height` and `Width` constraints.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Cars" TValue="Car" WeightValuePath="Count" EqualColorValuePath="Brand">
    <TreeMapLeafItemSettings LabelPath="Name">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping LeafValue="Ford" Color='new string[] { "green" }'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Audi" Color='new string[] { "red" }'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Maruti" Color='new string[] { "orange" }'></TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true" Height="50px" Width="100px" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Top">
        <TreeMapLegendBorder Width="1"></TreeMapLegendBorder>
    </TreeMapLegendSettings>
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

**Result:** Navigation arrows appear to browse legend pages.

## Legend Shapes
Control legend symbol appearance using `Shape` property to select from various shape options.

### Available Shapes
The `Shape` property controls the symbol displayed for each legend item using values: `Circle`, `Rectangle`, `Triangle`, `Diamond`, `Cross`, `Star`, `Pentagon`, `InvertedTriangle`, or `Image`.

**Supported Shapes:**

| Shape | Appearance | Common Usage |
|-------|-----------|--------------|
| `Circle` | ● | Default, general purpose |
| `Rectangle` | ▬ | Categories, groups |
| `Triangle` | ▲ | Hierarchical levels |
| `Diamond` | ◆ | Special values |
| `Cross` | ✕ | Negative/excluded data |
| `Star` | ★ | Highlighted items |
| `Pentagon` | ⬟ | Complex data |
| `InvertedTriangle` | ▼ | Descending values |
| `Image` | 🖼 | Custom icons |

### Custom Shape Configuration
Configure individual shape properties using `Shape`, `ShapeHeight`, `ShapeWidth`, and `ShapePadding` properties for legend symbols.

**Circle Shape:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings Visible="true" Shape="Syncfusion.Blazor.TreeMap.LegendShape.Circle">
</TreeMapLegendSettings>
```

**Rectangle Shape:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings Visible="true" Shape="Syncfusion.Blazor.TreeMap.LegendShape.Rectangle">
</TreeMapLegendSettings>
```

**Diamond Shape:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings Visible="true" Shape="Syncfusion.Blazor.TreeMap.LegendShape.Diamond">
</TreeMapLegendSettings>
```

**Star Shape:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings Visible="true" Shape="Syncfusion.Blazor.TreeMap.LegendShape.Star">
</TreeMapLegendSettings>
```

**Complete Shape Example:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Products" TValue="Product" WeightValuePath="Sales" EqualColorValuePath="Category">
    <TreeMapLeafItemSettings LabelPath="Name">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping LeafValue="Electronics" Color='new string[] { "#0066CC" }'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Furniture" Color='new string[] { "#FF6600" }'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Clothing" Color='new string[] { "#339933" }'></TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true" Shape="Syncfusion.Blazor.TreeMap.LegendShape.Diamond" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Top">
    </TreeMapLegendSettings>
</SfTreeMap>

@code {
    public class Product
    {
        public string Name { get; set; }
        public string Category { get; set; }
        public int Sales { get; set; }
    }
    
    public List<Product> Products = new List<Product> {
        new Product { Name="Laptop", Category="Electronics", Sales=5000 },
        new Product { Name="Chair", Category="Furniture", Sales=3000 },
        new Product { Name="Shirt", Category="Clothing", Sales=2000 }
    };
}
```

## Legend for Color Mappings
Configure legends for different color mapping strategies using `EqualColorValuePath` or `RangeColorValuePath` properties.

### Equal Color Mapping Legend
Displays legend items for each unique value in equal color mapping using `EqualColorValuePath` property with `TreeMapLeafColorMapping` color configurations.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Cars" TValue="Car" WeightValuePath="Count" EqualColorValuePath="Brand">
    <TreeMapLeafItemSettings LabelPath="Name">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping LeafValue="Ford" Color='new string[] { "green" }'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Audi" Color='new string[] { "red" }'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Maruti" Color='new string[] { "orange" }'></TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Top">
    </TreeMapLegendSettings>
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
        new Car { Name="A3 Cabriolet", Brand="Audi", Count=123}
    };
}
```

**Result:** Three legend items: "Ford" (green), "Audi" (red), "Maruti" (orange).

### Range Color Mapping Legend
Displays legend items representing value ranges using `RangeColorValuePath` property with `StartRange` and `EndRange` in color mappings.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Fruits" TValue="Fruit" WeightValuePath="Count" RangeColorValuePath="Count">
    <TreeMapLeafItemSettings LabelPath="FruitName">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping StartRange="500" EndRange="3000" Color='new string[] { "Orange" }'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping StartRange="3000" EndRange="5000" Color='new string[] { "Green" }'></TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Top">
    </TreeMapLegendSettings>
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
        new Fruit { FruitName="Grape", Count=4300 }
    };
}
```

**Result:** Two legend items: "500 - 3000" (orange), "3000 - 5000" (green).

### Desaturation Mapping Legend
Legend for desaturation color mappings using `MinOpacity` and `MaxOpacity` properties to show color intensity variations.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Fruits" TValue="Fruit" WeightValuePath="Count" RangeColorValuePath="Count">
    <TreeMapLeafItemSettings LabelPath="FruitName">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping StartRange="500" EndRange="5000" Color='new string[] { "#00FF00" }' MinOpacity="0.3" MaxOpacity="1.0"></TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Top">
    </TreeMapLegendSettings>
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
        new Fruit { FruitName="Orange", Count=2300 }
    };
}
```

**Result:** Gradient legend showing color intensity from light to dark green.

## Legend Customization
Customize legend styling and appearance using `TreeMapLegendBorder`, `TreeMapLegendTitle`, and `TreeMapLegendTextStyle` components.

### Legend Border
Customize legend border appearance using `TreeMapLegendBorder` component with `Color` and `Width` properties.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Cars" TValue="Car" WeightValuePath="Count" EqualColorValuePath="Brand">
    <TreeMapLeafItemSettings LabelPath="Name">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping LeafValue="Ford" Color='new string[] { "green" }'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Audi" Color='new string[] { "red" }'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Maruti" Color='new string[] { "orange" }'></TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Top">
        <TreeMapLegendBorder Color="#333333" Width="2"></TreeMapLegendBorder>
    </TreeMapLegendSettings>
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
        new Car { Name="A3 Cabriolet", Brand="Audi", Count=123}
    };
}
```

### Legend Title
Add descriptive titles to legends using `TreeMapLegendTitle` component with `Text` property and `TreeMapLegendTitleStyle` for styling.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Cars" TValue="Car" WeightValuePath="Count" EqualColorValuePath="Brand">
    <TreeMapLeafItemSettings LabelPath="Name">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping LeafValue="Ford" Color='new string[] { "green" }'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Audi" Color='new string[] { "red" }'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Maruti" Color='new string[] { "orange" }'></TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Top">
        <TreeMapLegendTitle Text="Car Brands">
            <TreeMapLegendTitleStyle Size="16px" FontWeight="bold" Color="#333333"></TreeMapLegendTitleStyle>
        </TreeMapLegendTitle>
    </TreeMapLegendSettings>
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
        new Car { Name="A3 Cabriolet", Brand="Audi", Count=123}
    };
}
```

### Legend Text Style
Customize legend item text appearance using `TreeMapLegendTextStyle` component with `Size`, `FontFamily`, `FontWeight`, and `Color` properties.

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings Visible="true" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Top">
    <TreeMapLegendTextStyle 
        Size="14px" 
        FontFamily="Arial" 
        FontWeight="600" 
        Color="#444444">
    </TreeMapLegendTextStyle>
</TreeMapLegendSettings>
```

## Legend Item Visibility Control
Control which legend items display using `ShowLegend`, `ShowLegendPath`, and `RemoveDuplicateLegend` properties.

### Hiding Specific Legend Items
Control legend visibility per color mapping using `ShowLegend` property in `TreeMapLeafColorMapping` component.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Fruits" TValue="Fruit" WeightValuePath="Count" RangeColorValuePath="Count">
    <TreeMapLeafItemSettings LabelPath="FruitName">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping StartRange="500" EndRange="3000" Color='new string[] { "Orange" }'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping StartRange="3000" EndRange="4000" Color='new string[] { "Green" }' ShowLegend="false"></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping Color='new string[] { "red" }'></TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true">
    </TreeMapLegendSettings>
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

**Result:** Legend excludes the "3000 - 4000" range (green items).

### Data-Driven Visibility
Control legend visibility through data source properties using `ShowLegendPath` property to bind a data field for visibility control.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Fruits" TValue="Fruit" WeightValuePath="Count" ColorValuePath="Color">
    <TreeMapLeafItemSettings LabelPath="Name">
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true" ShowLegendPath="Visibility">
    </TreeMapLegendSettings>
</SfTreeMap>

@code {
    public class Fruit
    {
        public string Name { get; set; }
        public int Count { get; set; }
        public bool Visibility { get; set; }
        public string Color { get; set; }
    }
    
    public List<Fruit> Fruits = new List<Fruit> {
        new Fruit { Name="Apple", Count=5000, Visibility= true , Color="red" },
        new Fruit { Name="Mango", Count=3000, Visibility= false , Color="blue"},
        new Fruit { Name="Orange", Count=2300, Visibility= true , Color="green" },
        new Fruit { Name="Banana", Count=500, Visibility= false , Color = "yellow"},
        new Fruit { Name="Grape", Count=4300, Visibility= true , Color="pink"},
        new Fruit { Name="Papaya", Count=1200, Visibility= false, Color="orange" },
        new Fruit { Name="Melon", Count=4500, Visibility= true , Color="purple"}
    };
}
```

**Result:** Only items with `Visibility=true` appear in legend (Apple, Orange, Grape, Melon).

### Removing Duplicate Legends
Eliminate duplicate legend entries using `RemoveDuplicateLegend` property set to `"true"` in `TreeMapLegendSettings`.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Fruits" TValue="Fruit" WeightValuePath="Count" ColorValuePath="Color">
    <TreeMapLeafItemSettings LabelPath="Name">
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true" ValuePath="Name" RemoveDuplicateLegend="true">
    </TreeMapLegendSettings>
</SfTreeMap>

@code {
    public class Fruit
    {
        public string Name { get; set; }
        public int Count { get; set; }
        public string Color { get; set; }
    }
    
    public List<Fruit> Fruits = new List<Fruit> {
        new Fruit { Name="Apple", Count=5000, Color="red" },
        new Fruit { Name="Apple", Count=2300, Color="yellow" }, // Duplicate
        new Fruit { Name="Mango", Count=3000, Color="blue"},
        new Fruit { Name="Orange", Count=2300, Color="green" },
        new Fruit { Name="Banana", Count=500, Color = "yellow"},
        new Fruit { Name="Grape", Count=4300, Color="pink"}
    };
}
```

**Result:** Only one "Apple" legend entry appears despite multiple Apple data items.

## Legend Data Binding
Configure legend data source binding using `ValuePath` property and custom computed properties.

### ValuePath Configuration
Bind legend text to specific data properties using `ValuePath` property in `TreeMapLegendSettings`.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Fruits" TValue="Fruit" WeightValuePath="Count" ColorValuePath="Color">
    <TreeMapLeafItemSettings LabelPath="Name">
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true" ValuePath="Name">
    </TreeMapLegendSettings>
</SfTreeMap>

@code {
    public class Fruit
    {
        public string Name { get; set; }
        public int Count { get; set; }
        public string Color { get; set; }
    }
    
    public List<Fruit> Fruits = new List<Fruit> {
        new Fruit { Name="Apple", Count=5000, Color="red" },
        new Fruit { Name="Mango", Count=3000, Color="blue"},
        new Fruit { Name="Orange", Count=2300, Color="green" },
        new Fruit { Name="Banana", Count=500, Color = "yellow"},
        new Fruit { Name="Grape", Count=4300, Color="pink"},
        new Fruit { Name="Papaya", Count=1200, Color="orange" },
        new Fruit { Name="Melon", Count=4500, Color="purple"}
    };
}
```

**Result:** Legend displays fruit names from the `Name` property.

### Custom Legend Text
Create custom legend text through computed properties using `ValuePath` to reference properties that combine multiple data fields.

```cshtml
@using Syncfusion.Blazor.TreeMap
<SfTreeMap DataSource="Products" TValue="Product" WeightValuePath="Sales" ColorValuePath="Category">
    <TreeMapLeafItemSettings LabelPath="ProductName"></TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true" ValuePath="LegendText"></TreeMapLegendSettings>
</SfTreeMap>
@code {
    public class Product
    {
        public string ProductName { get; set; }
        public int Sales { get; set; }
        public string Category { get; set; }
        
        // Custom legend text
        public string LegendText => $"{Category} ({ProductName})";
    }
    
    public List<Product> Products = new List<Product> {
        new Product { ProductName="Laptop", Category="Electronics", Sales=5000 },
        new Product { ProductName="Chair", Category="Furniture", Sales=3000 }
    };
}
```

## Smart Legend with Filtering
Smart legend allows users to toggle data visibility by clicking legend items using `Mode="LegendMode.Interactive"` combined with selection features for interactive filtering.

**Note:** While the documentation mentions smart legend, the current TreeMap API primarily supports interactive legend mode. For filtering behavior, use the `Mode="LegendMode.Interactive"` setting combined with selection features.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Cars" TValue="Car" WeightValuePath="Count" EqualColorValuePath="Brand">
    <TreeMapLeafItemSettings LabelPath="Name">
        <TreeMapLeafColorMappings>
            <TreeMapLeafColorMapping LeafValue="Ford" Color='new string[] { "green" }'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Audi" Color='new string[] { "red" }'></TreeMapLeafColorMapping>
            <TreeMapLeafColorMapping LeafValue="Maruti" Color='new string[] { "orange" }'></TreeMapLeafColorMapping>
        </TreeMapLeafColorMappings>
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true" Mode="Syncfusion.Blazor.TreeMap.LegendMode.Interactive" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Top">
    </TreeMapLegendSettings>
    <TreeMapSelectionSettings Enable="true" Fill="lightblue" Opacity="0.8">
    </TreeMapSelectionSettings>
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
        new Car { Name="A3 Cabriolet", Brand="Audi", Count=123}
    };
}
```

## RTL Support
Enable right-to-left layout for legend using `EnableRtl="true"` property in the TreeMap component.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="Cars" WeightValuePath="Count" ColorValuePath="Color" EnableRtl="true">
    <TreeMapLeafItemSettings LabelPath="Name">
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Top">
    </TreeMapLegendSettings>
</SfTreeMap>

@code {
    public class Car
    {
        public string Name { get; set; }
        public string Brand { get; set; }
        public int Count { get; set; }
        public string Color { get; set; }
    }
    
    public List<Car> Cars = new List<Car> {
        new Car { Name="Mustang", Brand="Ford", Count=232, Color= "#71B081" },
        new Car { Name="EcoSport", Brand="Ford", Count=121,  Color= "#5A9A77" },
        new Car { Name="Swift", Brand="Maruti", Count=143, Color= "#498770" },
        new Car { Name="Baleno", Brand="Maruti", Count=454, Color= "#39776C" },
        new Car { Name="Vitara Brezza", Brand="Maruti", Count=545 , Color= "#266665" },
        new Car { Name="A3 Cabriolet", Brand="Audi",Count=123, Color= "#124F5E" }
    };
}
```

**Result:** Legend items render right-to-left with icon on right, text on left.

## Best Practices

### 1. Choose Appropriate Legend Position

Match legend position to visualization layout and user reading patterns.

```cshtml
@using Syncfusion.Blazor.TreeMap

<!-- Wide layouts: Use Top or Bottom -->
<TreeMapLegendSettings Visible="true" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Top"></TreeMapLegendSettings>

<!-- Tall layouts: Use Left or Right -->
<TreeMapLegendSettings Visible="true" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Right"></TreeMapLegendSettings>

<!-- Responsive layouts: Use Auto -->
<TreeMapLegendSettings Visible="true" Position="Syncfusion.Blazor.TreeMap.LegendPosition.Auto"></TreeMapLegendSettings>
```

### 2. Limit Legend Item Count

Too many legend items reduce readability. Consider grouping or filtering data.

```cshtml
@using Syncfusion.Blazor.TreeMap

<!-- Good: 3-8 distinct categories -->
<TreeMapLeafColorMappings>
    <TreeMapLeafColorMapping LeafValue="Category1" Color='new string[] { "red" }'></TreeMapLeafColorMapping>
    <TreeMapLeafColorMapping LeafValue="Category2" Color='new string[] { "blue" }'></TreeMapLeafColorMapping>
    <TreeMapLeafColorMapping LeafValue="Category3" Color='new string[] { "green" }'></TreeMapLeafColorMapping>
</TreeMapLeafColorMappings>

<!-- Avoid: 15+ categories without grouping -->
```

### 3. Use Descriptive Legend Text

Provide clear, concise legend labels.

```cshtml
@using Syncfusion.Blazor.TreeMap

<!-- Good: Clear labels -->
<TreeMapLegendSettings ValuePath="CategoryName"></TreeMapLegendSettings>

<!-- Avoid: Cryptic abbreviations -->
<TreeMapLegendSettings ValuePath="CatCode"></TreeMapLegendSettings>
```

### 4. Match Shape to Data Type

Select legend shapes appropriate for your data context.

```cshtml
@using Syncfusion.Blazor.TreeMap

<!-- Categories: Circle or Rectangle -->
<TreeMapLegendSettings Shape="LegendShape.Circle"></TreeMapLegendSettings>

<!-- Hierarchical: Triangle -->
<TreeMapLegendSettings Shape="LegendShape.Triangle"></TreeMapLegendSettings>

<!-- Special values: Star or Diamond -->
<TreeMapLegendSettings Shape="LegendShape.Star"></TreeMapLegendSettings>
```

### 5. Set Explicit Size for Fixed Layouts

Define legend dimensions for consistent appearance.

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings 
    Visible="true" 
    Height="60px" 
    Width="300px" 
    Position="Syncfusion.Blazor.TreeMap.LegendPosition.Bottom">
</TreeMapLegendSettings>
```

### 6. Enable Paging for Many Items

Allow navigation through numerous legend entries.

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings 
    Visible="true" 
    Height="80px" 
    Width="200px" 
    Position="Syncfusion.Blazor.TreeMap.LegendPosition.Right">
    <TreeMapLegendBorder Width="1" Color="#CCCCCC"></TreeMapLegendBorder>
</TreeMapLegendSettings>
```

## Troubleshooting

### Legend Not Displaying

**Problem:** Legend settings configured but legend doesn't appear.

**Solutions:**

1. **Verify Visible property is true:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings Visible="true"></TreeMapLegendSettings>
```

2. **Ensure color mapping is configured:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafColorMappings>
    <TreeMapLeafColorMapping LeafValue="Value1" Color='new string[] { "red" }'></TreeMapLeafColorMapping>
</TreeMapLeafColorMappings>
```

3. **Check data source has values:**
```csharp
// Ensure data exists
public List<Car> Cars = new List<Car> { /* ... data ... */ };
```

### Legend Items Missing

**Problem:** Some expected legend items don't appear.

**Solutions:**

1. **Check ShowLegend property:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafColorMapping LeafValue="Ford" Color='new string[] { "green" }' ShowLegend="true"></TreeMapLeafColorMapping>
```

2. **Verify ShowLegendPath data:**
```csharp
new Fruit { Name="Apple", Visibility=true } // Must be true
```

3. **Check for duplicate removal:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings RemoveDuplicateLegend="false"></TreeMapLegendSettings>
```

### Legend Positioning Issues

**Problem:** Legend appears in wrong location or overlaps TreeMap.

**Solutions:**

1. **Set explicit position:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings Position="Syncfusion.Blazor.TreeMap.LegendPosition.Bottom"></TreeMapLegendSettings>
```

2. **Configure size appropriately:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings Height="70px" Width="100%"></TreeMapLegendSettings>
```

3. **Use Float position with custom coordinates:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings Position="Syncfusion.Blazor.TreeMap.LegendPosition.Float" X="50" Y="20"></TreeMapLegendSettings>
```

### Interactive Mode Not Working

**Problem:** Interactive legend doesn't show indicators on hover.

**Solutions:**

1. **Verify Mode is set to Interactive:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings Mode="Syncfusion.Blazor.TreeMap.LegendMode.Interactive"></TreeMapLegendSettings>
```

2. **Ensure proper color mapping:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafColorMappings>
    <!-- Must have valid mappings -->
</TreeMapLeafColorMappings>
```

### Legend Paging Not Appearing

**Problem:** Expected paging controls don't show.

**Solutions:**

1. **Reduce legend size to force paging:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings Height="50px" Width="150px"></TreeMapLegendSettings>
```

2. **Add border for visibility:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendBorder Width="1" Color="#000000"></TreeMapLegendBorder>
```

3. **Ensure sufficient legend items exist:**
- Need 4+ items that can't fit in specified dimensions

### Custom Legend Text Not Showing

**Problem:** ValuePath doesn't display expected text.

**Solutions:**

1. **Verify property name is correct:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLegendSettings ValuePath="ProductName"></TreeMapLegendSettings>
<!-- Property must exist in data class -->
```

2. **Check for null values:**
```csharp
new Product { ProductName = "Laptop" } // Not null
```

3. **Use computed property for complex text:**
```csharp
public string LegendLabel => $"{Category} - {Name}";
```

---

**Next Steps:**
- Configure tooltips for detailed item information
- Implement drill-down for hierarchical navigation
- Add selection and highlight for user interaction
- Set up color mapping strategies
- Enable print and export functionality

**Related Topics:**
- Color Mapping Configuration
- Tooltip Setup
- Selection and Highlight
- Data Binding Strategies
- Events and Methods
