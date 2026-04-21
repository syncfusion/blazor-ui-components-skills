# Labels

## Table of Contents
- [Overview](#overview)
- [Data Label Configuration](#data-label-configuration)
- [Templates and Formatting](#templates-and-formatting)
- [Overflow and Truncation](#overflow-and-truncation)

## Overview
Guidance on configuring labels shown inside TreeMap items and headers for levels using the `TreeMapLeafItemSettings` component.

## Data Label Configuration
- Set font, size, alignment and color using `TreeMapLeafLabelStyle`. Use multiple label fields when needed with `LabelPath` and `LabelFormat` properties.

## Templates and Formatting
- Use `LabelFormat` property to include formatted values, percentages, or additional HTML with `${PropertyName}` syntax.

## Overflow and Truncation
- Handle long labels by truncation, wrapping, or hiding depending on available tile size using `InterSectAction` property with `LabelAlignment` values.
# Data Labels in Blazor TreeMap

A comprehensive guide for configuring, formatting, and customizing data labels in the Syncfusion Blazor TreeMap component to display meaningful item identification and information.

## Table of Contents

- [Overview](#overview)
- [Basic Label Configuration](#basic-label-configuration)
  - [Enabling Labels](#enabling-labels)
  - [Label Path Mapping](#label-path-mapping)
- [Label Formatting](#label-formatting)
  - [Format String Syntax](#format-string-syntax)
  - [Using Multiple Data Properties](#using-multiple-data-properties)
  - [Custom Format Patterns](#custom-format-patterns)
- [Label Positioning](#label-positioning)
  - [Available Position Options](#available-position-options)
  - [Center Position](#center-position)
  - [Corner Positions](#corner-positions)
  - [Edge Positions](#edge-positions)
- [Font Customization](#font-customization)
  - [Font Size and Family](#font-size-and-family)
  - [Font Weight and Style](#font-weight-and-style)
  - [Color and Opacity](#color-and-opacity)
- [Label Intersect Actions](#label-intersect-actions)
  - [Trim Label](#trim-label)
  - [Hide Label](#hide-label)
  - [Wrap Label](#wrap-label)
  - [WrapByWord Option](#wrapbyword-option)
- [Advanced Label Configuration](#advanced-label-configuration)
  - [Label Gap and Spacing](#label-gap-and-spacing)
  - [Template-Based Labels](#template-based-labels)
  - [Conditional Label Visibility](#conditional-label-visibility)
- [Responsive Label Behavior](#responsive-label-behavior)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

Data labels are essential visual elements in TreeMap components that identify items or groups by displaying text information extracted from the data source. The Blazor TreeMap component provides extensive label customization options including positioning, formatting, font styling, and intelligent overflow handling.

**Key Features:**
- Dynamic label binding from data source properties
- 9 positioning options for flexible label placement
- Format strings with data property interpolation
- Smart intersect actions (trim, hide, wrap)
- Full font and style customization
- Gap spacing control between labels and item boundaries

## Basic Label Configuration

### Enabling Labels

Labels display by specifying the data source property to use via the `LabelPath` property of `TreeMapLeafItemSettings`. The `LabelPath` property binds a data source field to display as the label text on each TreeMap item.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" TValue="Fruit" DataSource="Fruits">
    <TreeMapLeafItemSettings LabelPath="FruitName"></TreeMapLeafItemSettings>
</SfTreeMap>

@code {
    public class Fruit
    {
        public string FruitName { get; set; }
        public int Count { get; set; }
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

**Result:** Each TreeMap item displays its corresponding fruit name.

### Label Path Mapping

The `LabelPath` property establishes the connection between data source properties and displayed labels. It maps a specific data source field to be rendered as the identifying text within each TreeMap item.

**Example: Mapping Different Properties**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Sales" TValue="Product" DataSource="Products">
    <TreeMapLeafItemSettings LabelPath="ProductName"></TreeMapLeafItemSettings>
</SfTreeMap>

@code {
    public class Product
    {
        public string ProductName { get; set; }
        public string Category { get; set; }
        public int Sales { get; set; }
    }
    
    public List<Product> Products = new List<Product> {
        new Product { ProductName="Laptop", Category="Electronics", Sales=45000 },
        new Product { ProductName="Phone", Category="Electronics", Sales=38000 },
        new Product { ProductName="Tablet", Category="Electronics", Sales=22000 }
    };
}
```

## Label Formatting

### Format String Syntax

The `LabelFormat` property enables custom label templates using data property interpolation with `${PropertyName}` syntax. This property allows dynamic formatting of labels by combining multiple data source properties into a single display string.

**Basic Format Example:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" TValue="Car" DataSource="Cars" RangeColorValuePath="Count">
    <TreeMapLeafItemSettings LabelPath="Name" LabelFormat="${Name}"></TreeMapLeafItemSettings>
</SfTreeMap>

@code {
    public class Car
    {
        public string Name { get; set; }
        public string Brand { get; set; }
        public int Count { get; set; }
    }
    
    public List<Car> Cars = new List<Car> {
        new Car { Name="Mustang", Brand="Ford", Count=232 },
        new Car { Name="EcoSport", Brand="Ford", Count=121 },
        new Car { Name="Swift", Brand="Maruti", Count=143 }
    };
}
```

### Using Multiple Data Properties

Use the `LabelFormat` property to combine multiple properties in format strings for richer label information. The property supports multiple `${PropertyName}` placeholders separated by custom text.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" TValue="Car" DataSource="Cars" RangeColorValuePath="Count">
    <TreeMapLeafItemSettings LabelPath="Name" LabelFormat="${Name}-${Brand}"></TreeMapLeafItemSettings>
</SfTreeMap>

@code {
    public class Car
    {
        public string Name { get; set; }
        public string Brand { get; set; }
        public int Count { get; set; }
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

**Output:** Labels display as "Mustang-Ford", "EcoSport-Ford", etc.

### Custom Format Patterns

Create complex label formats with text, symbols, and data properties using the `LabelFormat` property. Combine literal text with `${PropertyName}` placeholders to create customized display patterns.

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafItemSettings 
    LabelPath="Name" 
    LabelFormat="${Brand}: ${Name} (${Count} units)">
</TreeMapLeafItemSettings>
```

**Output:** "Ford: Mustang (232 units)"

## Label Positioning

### Available Position Options

The `LabelPosition` property supports 9 positioning values from the `LabelPosition` enumeration. This property determines where labels appear relative to each TreeMap item.

| Position | Description | Common Use Case |
|----------|-------------|-----------------|
| `TopLeft` | Top-left corner | Header-style labels |
| `TopCenter` | Top edge center | Title positioning |
| `TopRight` | Top-right corner | Status indicators |
| `Center` | Center of item (default) | Standard positioning |
| `CenterLeft` | Left edge center | Left-aligned labels |
| `CenterRight` | Right edge center | Right-aligned labels |
| `BottomLeft` | Bottom-left corner | Footer information |
| `BottomCenter` | Bottom edge center | Bottom-aligned labels |
| `BottomRight` | Bottom-right corner | Right-bottom indicators |

### Center Position

The `LabelPosition` property set to `Center` uses default positioning to center labels within item boundaries. This is the default value when positioning is not explicitly specified.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" TValue="Car" DataSource="Cars" RangeColorValuePath="Count">
    <TreeMapLeafItemSettings 
        LabelPath="Name" 
        LabelFormat="${Name}-${Brand}" 
        LabelPosition="Syncfusion.Blazor.TreeMap.LabelPosition.Center" 
        Gap="2">
    </TreeMapLeafItemSettings>
</SfTreeMap>
```

### Corner Positions

Use the `LabelPosition` property with values like `TopLeft`, `TopRight`, `BottomLeft`, and `BottomRight` to position labels at specific corners for specialized layouts.

**Top-Left Corner:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafItemSettings 
    LabelPath="Name" 
    LabelPosition="Syncfusion.Blazor.TreeMap.LabelPosition.TopLeft" 
    Gap="5">
</TreeMapLeafItemSettings>
```

**Bottom-Right Corner:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafItemSettings 
    LabelPath="Name" 
    LabelPosition="Syncfusion.Blazor.TreeMap.LabelPosition.BottomRight" 
    Gap="3">
</TreeMapLeafItemSettings>
```

### Edge Positions

Use the `LabelPosition` property with values like `TopCenter`, `CenterLeft`, and `CenterRight` to center labels along edges for balanced appearance.

**Top Center:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafItemSettings 
    LabelPath="ProductName" 
    LabelPosition="Syncfusion.Blazor.TreeMap.LabelPosition.TopCenter">
</TreeMapLeafItemSettings>
```

**Center Left:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafItemSettings 
    LabelPath="ProductName" 
    LabelPosition="Syncfusion.Blazor.TreeMap.LabelPosition.CenterLeft">
</TreeMapLeafItemSettings>
```

## Font Customization

### Font Size and Family

Customize font appearance using `TreeMapLeafLabelStyle` with `Size` and `FontFamily` properties to control the font dimensions and type used for label rendering.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Sales" TValue="Product" DataSource="Products">
    <TreeMapLeafItemSettings LabelPath="ProductName">
        <TreeMapLeafLabelStyle 
            Size="14px" 
            FontFamily="Segoe UI">
        </TreeMapLeafLabelStyle>
    </TreeMapLeafItemSettings>
</SfTreeMap>

@code {
    public class Product
    {
        public string ProductName { get; set; }
        public int Sales { get; set; }
    }
    
    public List<Product> Products = new List<Product> {
        new Product { ProductName="Laptop", Sales=45000 },
        new Product { ProductName="Desktop", Sales=32000 },
        new Product { ProductName="Monitor", Sales=18000 }
    };
}
```

### Font Weight and Style

Use `TreeMapLeafLabelStyle` properties `FontWeight` and `FontStyle` to control font weight and style for emphasis on labels.

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafItemSettings LabelPath="Name">
    <TreeMapLeafLabelStyle 
        Size="16px" 
        FontWeight="bold" 
        FontStyle="italic" 
        FontFamily="Arial">
    </TreeMapLeafLabelStyle>
</TreeMapLeafItemSettings>
```

**Available Font Weights:**
- `normal` (default)
- `bold`
- `bolder`
- `lighter`
- Numeric values: `100`, `200`, `300`, `400`, `500`, `600`, `700`, `800`, `900`

**Available Font Styles:**
- `normal` (default)
- `italic`
- `oblique`

### Color and Opacity

Use `TreeMapLeafLabelStyle` properties `Color` and `Opacity` to set label color and transparency for visual customization.

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafItemSettings LabelPath="FruitName">
    <TreeMapLeafLabelStyle 
        Color="#FFFFFF" 
        Opacity="0.9" 
        Size="12px">
    </TreeMapLeafLabelStyle>
</TreeMapLeafItemSettings>
```

**Complete Font Customization Example:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" TValue="Fruit" DataSource="Fruits">
    <TreeMapLeafItemSettings LabelPath="FruitName" LabelPosition="Syncfusion.Blazor.TreeMap.LabelPosition.Center">
        <TreeMapLeafLabelStyle 
            Size="15px" 
            FontFamily="Verdana" 
            FontWeight="600" 
            Color="#2C3E50" 
            Opacity="1">
        </TreeMapLeafLabelStyle>
    </TreeMapLeafItemSettings>
</SfTreeMap>

@code {
    public class Fruit
    {
        public string FruitName { get; set; }
        public int Count { get; set; }
    }
    
    public List<Fruit> Fruits = new List<Fruit> {
        new Fruit { FruitName="Apple", Count=5000 },
        new Fruit { FruitName="Mango", Count=3000 },
        new Fruit { FruitName="Orange", Count=2300 }
    };
}
```

## Label Intersect Actions

When labels exceed item dimensions, use `InterSectAction` to control overflow behavior.

### Trim Label

Use the `InterSectAction` property set to `LabelAlignment.Trim` to truncate overflowing labels with ellipsis (...).

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" TValue="Car" DataSource="Cars">
    <TreeMapLeafItemSettings 
        LabelPath="Name" 
        LabelFormat="${Name}-${Brand}" 
        InterSectAction="Syncfusion.Blazor.TreeMap.LabelAlignment.Trim">
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
        new Car { Name="Mustang", Brand="Ford Motor Company", Count=232 },
        new Car { Name="Lincoln Continental Mark V", Brand="Ford Motor Company", Count=50},
        new Car { Name="EcoSport", Brand="Ford Motor Company", Count=121 }
    };
}
```

**Result:** "Lincoln Continental Mark V-For..." (truncated)

### Hide Label

Use the `InterSectAction` property set to `LabelAlignment.Hide` to hide labels completely when they don't fit within item boundaries.

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafItemSettings 
    LabelPath="Name" 
    InterSectAction="Syncfusion.Blazor.TreeMap.LabelAlignment.Hide">
</TreeMapLeafItemSettings>
```

**Use Case:** Cleaner visualization when many small items exist.

### Wrap Label

Use the `InterSectAction` property set to `LabelAlignment.Wrap` to wrap labels to multiple lines when they exceed the available space.

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafItemSettings 
    LabelPath="Name" 
    LabelFormat="${Name}-${Brand}" 
    InterSectAction="Syncfusion.Blazor.TreeMap.LabelAlignment.Wrap">
</TreeMapLeafItemSettings>
```

### WrapByWord Option

Use the `InterSectAction` property set to `LabelAlignment.WrapByWord` to wrap labels at word boundaries for better readability without breaking words.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" TValue="Car" DataSource="Cars">
    <TreeMapLeafItemSettings 
        LabelPath="Name" 
        LabelFormat="${Name}-${Brand}" 
        InterSectAction="Syncfusion.Blazor.TreeMap.LabelAlignment.WrapByWord">
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

**Result:** Labels wrap at word boundaries preserving word integrity.

## Advanced Label Configuration

### Label Gap and Spacing

The `Gap` property of `TreeMapLeafItemSettings` controls spacing between labels and item boundaries (in pixels).

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafItemSettings 
    LabelPath="Name" 
    Gap="10" 
    LabelPosition="LabelPosition.TopLeft">
</TreeMapLeafItemSettings>
```

**Gap Value Guidelines:**
- `0`: No spacing (label touches boundary)
- `2-5`: Subtle spacing (default: 2)
- `10-15`: Prominent spacing
- `20+`: Large spacing for special layouts

**Example with Multiple Gap Values:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Sales" TValue="Product" DataSource="Products">
    <TreeMapLeafItemSettings 
        LabelPath="ProductName" 
        Gap="8" 
        LabelPosition="Syncfusion.Blazor.TreeMap.LabelPosition.Center">
        <TreeMapLeafLabelStyle Size="14px" Color="#333333"></TreeMapLeafLabelStyle>
    </TreeMapLeafItemSettings>
</SfTreeMap>

@code {
    public class Product
    {
        public string ProductName { get; set; }
        public int Sales { get; set; }
    }
    
    public List<Product> Products = new List<Product> {
        new Product { ProductName="Laptop", Sales=45000 },
        new Product { ProductName="Phone", Sales=38000 }
    };
}
```

### Template-Based Labels

For complex label rendering beyond format strings, use the `LabelFormat` property with custom templates and data-bound properties.

**Note:** While direct label templates aren't supported in `TreeMapLeafItemSettings`, you can achieve custom rendering through:

1. **Format strings** with complex expressions
2. **Tooltip templates** for detailed information display
3. **Custom data preparation** with computed properties

**Example: Computed Label Property**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Sales" TValue="Product" DataSource="Products">
    <TreeMapLeafItemSettings LabelPath="DisplayLabel"></TreeMapLeafItemSettings>
</SfTreeMap>

@code {
    public class Product
    {
        public string ProductName { get; set; }
        public int Sales { get; set; }
        public string Category { get; set; }
        
        // Computed property for complex label
        public string DisplayLabel => $"{ProductName} ({Category}): ${Sales:N0}";
    }
    
    public List<Product> Products = new List<Product> {
        new Product { ProductName="Laptop", Category="Electronics", Sales=45000 },
        new Product { ProductName="Phone", Category="Mobile", Sales=38000 }
    };
}
```

### Conditional Label Visibility

Use the `LabelPath` property to control label visibility based on data conditions by preparing your data source with conditional properties.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Sales" TValue="Product" DataSource="Products">
    <TreeMapLeafItemSettings LabelPath="LabelText"></TreeMapLeafItemSettings>
</SfTreeMap>

@code {
    public class Product
    {
        public string ProductName { get; set; }
        public int Sales { get; set; }
        
        // Only show label for high-value items
        public string LabelText => Sales > 30000 ? ProductName : "";
    }
    
    public List<Product> Products = new List<Product> {
        new Product { ProductName="Laptop", Sales=45000 }, // Label shown
        new Product { ProductName="Mouse", Sales=2000 },   // Label hidden
        new Product { ProductName="Phone", Sales=38000 }   // Label shown
    };
}

```

## Responsive Label Behavior

Labels automatically adjust to container size based on the TreeMap's `Width` and `Height` properties. Combine with `InterSectAction` property for optimal responsiveness.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap 
    WeightValuePath="Count" 
    TValue="Fruit" 
    DataSource="Fruits" 
    Width="100%" 
    Height="400px">
    <TreeMapLeafItemSettings 
        LabelPath="FruitName" 
        InterSectAction="Syncfusion.Blazor.TreeMap.LabelAlignment.WrapByWord" 
        Gap="5">
        <TreeMapLeafLabelStyle Size="12px" FontFamily="Arial"></TreeMapLeafLabelStyle>
    </TreeMapLeafItemSettings>
</SfTreeMap>

@code {
    public class Fruit
    {
        public string FruitName { get; set; }
        public int Count { get; set; }
    }
    
    public List<Fruit> Fruits = new List<Fruit> {
        new Fruit { FruitName="Apple", Count=5000 },
        new Fruit { FruitName="Mango", Count=3000 },
        new Fruit { FruitName="Orange", Count=2300 },
        new Fruit { FruitName="Banana", Count=500 }
    };
}
```

**Responsive Design Tips:**
1. Use percentage-based width/height for TreeMap
2. Set `InterSectAction` to handle varying sizes
3. Use relative font sizes (em, rem) when possible
4. Test across different viewport sizes

## Best Practices

### 1. Choose Appropriate Label Sources

Select meaningful data properties that identify items clearly.

```cshtml
@using Syncfusion.Blazor.TreeMap

<!-- Good: Descriptive property -->
<TreeMapLeafItemSettings LabelPath="ProductName"></TreeMapLeafItemSettings>

<!-- Avoid: Non-descriptive property -->
<TreeMapLeafItemSettings LabelPath="ID"></TreeMapLeafItemSettings>
```

### 2. Format Labels for Readability

Use format strings to provide context without overwhelming users.

```cshtml
@using Syncfusion.Blazor.TreeMap

<!-- Good: Concise format -->
<TreeMapLeafItemSettings 
    LabelPath="Name" 
    LabelFormat="${Name} (${Count})">
</TreeMapLeafItemSettings>

<!-- Avoid: Too much information -->
<TreeMapLeafItemSettings 
    LabelPath="Name" 
    LabelFormat="${Name}-${Brand}-${Category}-${Count}-${Date}">
</TreeMapLeafItemSettings>
```

### 3. Handle Label Overflow Gracefully

Always set `InterSectAction` for datasets with varying item sizes.

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafItemSettings 
    LabelPath="Name" 
    InterSectAction="Syncfusion.Blazor.TreeMap.LabelAlignment.WrapByWord">
</TreeMapLeafItemSettings>
```

### 4. Maintain Sufficient Contrast

Ensure labels are readable against item background colors.

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafItemSettings LabelPath="Name" Fill="darkblue">
    <TreeMapLeafLabelStyle Color="#FFFFFF"></TreeMapLeafLabelStyle>
</TreeMapLeafItemSettings>
```

### 5. Use Consistent Font Styling

Maintain visual consistency across all labels.

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafItemSettings LabelPath="Name">
    <TreeMapLeafLabelStyle 
        Size="13px" 
        FontFamily="Segoe UI" 
        FontWeight="normal">
    </TreeMapLeafLabelStyle>
</TreeMapLeafItemSettings>
```

### 6. Consider Hierarchical Data

For multi-level TreeMaps, differentiate labels at different levels.

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLevels>
    <TreeMapLevel GroupPath="Country">
        <TreeMapHeaderStyle Size="16px" FontWeight="bold"></TreeMapHeaderStyle>
    </TreeMapLevel>
</TreeMapLevels>
<TreeMapLeafItemSettings LabelPath="Product">
    <TreeMapLeafLabelStyle Size="12px"></TreeMapLeafLabelStyle>
</TreeMapLeafItemSettings>
```

## Troubleshooting

### Labels Not Appearing

**Problem:** Labels are not visible on TreeMap items.

**Solutions:**

1. **Verify LabelPath is set:**
```cshtml
<TreeMapLeafItemSettings LabelPath="YourPropertyName"></TreeMapLeafItemSettings>
```

2. **Check data source property exists:**
```csharp
public class DataItem
{
    public string YourPropertyName { get; set; } // Must exist
    public int Value { get; set; }
}
```

3. **Ensure property has values:**
```csharp
new DataItem { YourPropertyName = "Label Text", Value = 100 } // Not null/empty
```

### Labels Truncated Unexpectedly

**Problem:** Labels are cut off even when space appears sufficient.

**Solutions:**

1. **Adjust Gap property:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafItemSettings LabelPath="Name" Gap="5"></TreeMapLeafItemSettings>
```

2. **Use WrapByWord intersect action:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafItemSettings 
    LabelPath="Name" 
    InterSectAction="Syncfusion.Blazor.TreeMap.LabelAlignment.WrapByWord">
</TreeMapLeafItemSettings>
```

3. **Reduce font size:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafLabelStyle Size="11px"></TreeMapLeafLabelStyle>
```

### Label Format Not Working

**Problem:** Format string like `"${Property}"` displays literally.

**Solutions:**

1. **Check property name matches exactly (case-sensitive):**
```cshtml
@using Syncfusion.Blazor.TreeMap

<!-- Data property: ProductName -->
<TreeMapLeafItemSettings LabelFormat="${ProductName}"></TreeMapLeafItemSettings>
```

2. **Verify TValue type matches data source:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap TValue="Product" DataSource="Products">
```

### Labels Overlapping

**Problem:** Multiple labels overlap each other.

**Solutions:**

1. **Use Hide intersect action for small items:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafItemSettings 
    LabelPath="Name" 
    InterSectAction="Syncfusion.Blazor.TreeMap.LabelAlignment.Hide">
</TreeMapLeafItemSettings>
```

2. **Reduce font size:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafLabelStyle Size="10px"></TreeMapLeafLabelStyle>
```

3. **Increase TreeMap size:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap Width="1000px" Height="600px">
```

### Font Styling Not Applied

**Problem:** Font properties like size or color don't appear.

**Solutions:**

1. **Ensure TreeMapLeafLabelStyle is nested correctly:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafItemSettings LabelPath="Name">
    <TreeMapLeafLabelStyle Size="14px" Color="#FF0000"></TreeMapLeafLabelStyle>
</TreeMapLeafItemSettings>
```

2. **Check for CSS conflicts:**
```html
<!-- Remove global styles that may override -->
<style>
    .e-treemap .e-treemap-label {
        /* Don't set font properties here */
    }
</style>
```

3. **Use valid CSS color values:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafLabelStyle Color="#FF0000"></TreeMapLeafLabelStyle> <!-- Valid -->
<TreeMapLeafLabelStyle Color="red"></TreeMapLeafLabelStyle> <!-- Valid -->
```

### Position Not Changing

**Problem:** LabelPosition property has no effect.

**Solutions:**

1. **Use correct enumeration value:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafItemSettings 
    LabelPath="Name" 
    LabelPosition="Syncfusion.Blazor.TreeMap.LabelPosition.TopLeft">
</TreeMapLeafItemSettings>
```

2. **Ensure adequate Gap for corner positions:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapLeafItemSettings 
    LabelPath="Name" 
    LabelPosition="Syncfusion.Blazor.TreeMap.LabelPosition.TopLeft" 
    Gap="5">
</TreeMapLeafItemSettings>
```

3. **Verify item size is sufficient:**
- Very small items may not show positional differences
- Test with larger data values

---

**Next Steps:**
- Explore legend configuration for visual data interpretation
- Implement tooltips for detailed information display
- Configure color mappings for data categorization
- Add drill-down functionality for hierarchical navigation

**Related Topics:**
- Tooltip Configuration
- Legend Setup
- Color Mapping
- Hierarchical Data Display