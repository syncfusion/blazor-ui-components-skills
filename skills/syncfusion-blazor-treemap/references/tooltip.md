# Tooltip Configuration in Blazor TreeMap

A concise reference for `TreeMapTooltipSettings`, `TreeMapTooltipBorder`, `TreeMapTooltipTextStyle`, and `TreeMapTooltipArgs`.

Use `Visible` to enable tooltips, `Format` for templated text, `Fill` and `Opacity` for appearance, and `TooltipTemplate` for custom Blazor content.

## Table of Contents

- [Overview](#overview)
- [Basic Tooltip Configuration](#basic-tooltip-configuration)
  - [Enabling Tooltips](#enabling-tooltips)
  - [Default Tooltip Behavior](#default-tooltip-behavior)
- [Tooltip Content Formatting](#tooltip-content-formatting)
  - [Format String Syntax](#format-string-syntax)
  - [Using Multiple Data Properties](#using-multiple-data-properties)
  - [Format Tokens and Placeholders](#format-tokens-and-placeholders)
- [Tooltip Templates](#tooltip-templates)
  - [Custom HTML Content](#custom-html-content)
  - [Data-Bound Templates](#data-bound-templates)
  - [Rich Content with Images](#rich-content-with-images)
- [Tooltip Styling](#tooltip-styling)
  - [Background and Border](#background-and-border)
  - [Text Style Customization](#text-style-customization)
  - [Opacity Settings](#opacity-settings)
- [Tooltip Positioning and Margins](#tooltip-positioning-and-margins)
  - [Automatic Positioning](#automatic-positioning)
  - [Margin Configuration](#margin-configuration)
- [Animation Settings](#animation-settings)
- [Tooltips for Different Data Structures](#tooltips-for-different-data-structures)
  - [Flat Data Tooltips](#flat-data-tooltips)
  - [Hierarchical Data Tooltips](#hierarchical-data-tooltips)
- [Advanced Tooltip Scenarios](#advanced-tooltip-scenarios)
  - [Conditional Tooltip Content](#conditional-tooltip-content)
  - [Dynamic Formatting](#dynamic-formatting)
  - [Multi-Line Tooltips](#multi-line-tooltips)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

Tooltips provide contextual information about TreeMap items when users hover over them, especially useful when space constraints prevent full information display through labels. The Blazor TreeMap component offers comprehensive tooltip customization including content formatting, templates, styling, and positioning options.

**Key Features:**
- Simple visibility toggle with default content
- Format strings with data property interpolation
- Custom templates with Blazor components
- Full styling control (colors, fonts, borders, opacity)
- Automatic intelligent positioning
- Animation duration control
- Support for flat and hierarchical data structures

## Basic Tooltip Configuration

### Enabling Tooltips

Tooltips are hidden by default. Enable them by setting the `Visible` property to `true` in `TreeMapTooltipSettings`.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" TValue="Fruit" DataSource="Fruits">
    <TreeMapLeafItemSettings LabelPath="Name"></TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible=true></TreeMapTooltipSettings>
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

**Result:** Hovering over any item displays a tooltip showing the `WeightValuePath` value (Count).

### Default Tooltip Behavior

Without additional configuration, tooltips display the weight value of each item when `Visible` is enabled.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Sales" TValue="Product" DataSource="Products">
    <TreeMapLeafItemSettings LabelPath="ProductName"></TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible=true></TreeMapTooltipSettings>
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

**Default Display:** Tooltip shows "45000", "32000", "18000" for respective items.

## Tooltip Content Formatting

### Format String Syntax

The `Format` property enables custom tooltip content using data property interpolation with `${PropertyName}` syntax.

**Basic Format Example:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" TValue="Fruit" DataSource="Fruits">
    <TreeMapLeafItemSettings LabelPath="Name"></TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible=true Format="${Name}"></TreeMapTooltipSettings>
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
        new Fruit { Name="Orange", Count=2300 }
    };
}
```

**Result:** Tooltip displays fruit names: "Apple", "Mango", "Orange".

### Using Multiple Data Properties

Combine multiple properties within the `Format` property for comprehensive tooltip information.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" TValue="Fruit" DataSource="Fruits">
    <TreeMapLeafItemSettings LabelPath="Name"></TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible=true Format="Name: ${Name} - TotalCount: ${Count}"></TreeMapTooltipSettings>
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
        new Fruit { Name="Grape", Count=4300 }
    };
}
```

**Result:** "Name: Apple - TotalCount: 5000", "Name: Mango - TotalCount: 3000", etc.

### Format Tokens and Placeholders

Use descriptive text and tokens within the `Format` property to create informative tooltips.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Sales" TValue="Product" DataSource="Products">
    <TreeMapLeafItemSettings LabelPath="ProductName"></TreeMapLeafItemSettings>
    <TreeMapTooltipSettings 
        Visible=true 
        Format="Product: ${ProductName}<br/>Sales: $${Sales}<br/>Category: ${Category}">
    </TreeMapTooltipSettings>
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
        new Product { ProductName="Chair", Category="Furniture", Sales=12000 },
        new Product { ProductName="Phone", Category="Electronics", Sales=38000 }
    };
}
```

**Result:** Multi-line tooltip with labeled data:
```
Product: Laptop
Sales: $45000
Category: Electronics
```

**Note:** Use `<br/>` for line breaks in format strings.

## Tooltip Templates

### Custom HTML Content

Create rich tooltips using the `TooltipTemplate` property with custom HTML and Blazor components.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" TValue="Fruit" DataSource="Fruits">
    <TreeMapLeafItemSettings LabelPath="Name" Fill="lightgray" Gap="2"></TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible=true Opacity="0.8">
        <TooltipTemplate>
            @{
                var data = (context as Fruit);
                <table style="width:100%; background-color: #ffffff; border-spacing: 0px; border-collapse:separate; border: 1px solid grey; border-radius:10px; padding-top: 5px; padding-bottom:5px">
                    <tr>
                        <td style="font-weight:bold; color:black; padding-left: 5px;">Count:</td>
                        <td style="font-weight:bold; color:black; padding-right: 5px;">@data.Count</td>
                    </tr>
                </table>
            }
        </TooltipTemplate>
    </TreeMapTooltipSettings>
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
        new Fruit { Name="Orange", Count=2300 }
    };
}
```

**Result:** Styled table tooltip with rounded borders and custom formatting.

### Data-Bound Templates

Access all item data properties within the `TooltipTemplate` for dynamic content.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Sales" TValue="Product" DataSource="Products">
    <TreeMapLeafItemSettings LabelPath="ProductName"></TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible=true>
        <TooltipTemplate>
            @{
                var product = (context as Product);
                <div style="background-color: #f9f9f9; padding: 10px; border-radius: 5px; border: 2px solid #333;">
                    <h4 style="margin: 0 0 5px 0; color: #0066CC;">@product.ProductName</h4>
                    <p style="margin: 5px 0; color: #666;">Category: <strong>@product.Category</strong></p>
                    <p style="margin: 5px 0; color: #666;">Sales: <strong>$@product.Sales.ToString("N0")</strong></p>
                    <p style="margin: 5px 0; color: @(product.InStock ? "green" : "red");">
                        @(product.InStock ? "✓ In Stock" : "✗ Out of Stock")
                    </p>
                </div>
            }
        </TooltipTemplate>
    </TreeMapTooltipSettings>
</SfTreeMap>

@code {
    public class Product
    {
        public string ProductName { get; set; }
        public string Category { get; set; }
        public int Sales { get; set; }
        public bool InStock { get; set; }
    }
    
    public List<Product> Products = new List<Product> {
        new Product { ProductName="Laptop", Category="Electronics", Sales=45000, InStock=true },
        new Product { ProductName="Chair", Category="Furniture", Sales=12000, InStock=false },
        new Product { ProductName="Phone", Category="Electronics", Sales=38000, InStock=true }
    };
}
```

### Rich Content with Images

Include images, icons, and other media in tooltips using the `TooltipTemplate` property.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Sales" TValue="Product" DataSource="Products">
    <TreeMapLeafItemSettings LabelPath="ProductName"></TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible=true>
        <TooltipTemplate>
            @{
                var product = (context as Product);
                <div style="display: flex; align-items: center; padding: 8px; background: white; border-radius: 8px;">
                    <img src="@product.ImageUrl" alt="@product.ProductName" style="width: 50px; height: 50px; margin-right: 10px; border-radius: 4px;" />
                    <div>
                        <div style="font-weight: bold; color: #333;">@product.ProductName</div>
                        <div style="color: #666; font-size: 12px;">Sales: $@product.Sales.ToString("N0")</div>
                    </div>
                </div>
            }
        </TooltipTemplate>
    </TreeMapTooltipSettings>
</SfTreeMap>

@code {
    public class Product
    {
        public string ProductName { get; set; }
        public int Sales { get; set; }
        public string ImageUrl { get; set; }
    }
    
    public List<Product> Products = new List<Product> {
        new Product { ProductName="Laptop", Sales=45000, ImageUrl="images/laptop.png" },
        new Product { ProductName="Phone", Sales=38000, ImageUrl="images/phone.png" }
    };
}
```

## Tooltip Styling

### Background and Border

Customize tooltip appearance using `Fill` property and `TreeMapTooltipBorder` with `Color` and `Width` properties.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" TValue="Fruit" DataSource="Fruits">
    <TreeMapLeafItemSettings LabelPath="Name"></TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible=true Fill="#FFE5B4" Opacity="0.9">
        <TreeMapTooltipBorder Color="#FF8C00" Width="2"></TreeMapTooltipBorder>
    </TreeMapTooltipSettings>
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
        new Fruit { Name="Orange", Count=2300 }
    };
}
```

**Properties:**
- `Fill`: Background color (hex, rgb, or color name)
- `TreeMapTooltipBorder.Color`: Border color
- `TreeMapTooltipBorder.Width`: Border width in pixels

### Text Style Customization

Configure tooltip text appearance using `TreeMapTooltipTextStyle` with properties like `Size`, `FontFamily`, `FontWeight`, `FontStyle`, `Color`, and `Opacity`.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Count" TValue="Fruit" DataSource="Fruits">
    <TreeMapLeafItemSettings LabelPath="Name" Fill="lightgray" Gap="2"></TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible=true Fill="#2C3E50">
        <TreeMapTooltipTextStyle 
            FontStyle="italic" 
            FontWeight="bold" 
            Size="15px"
            Color="#FFFFFF"
            FontFamily="Arial">
        </TreeMapTooltipTextStyle>
    </TreeMapTooltipSettings>
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
        new Fruit { Name="Orange", Count=2300 }
    };
}
```

**Available Properties:**
- `Size`: Font size (e.g., "12px", "14px")
- `FontFamily`: Font family name
- `FontWeight`: Weight (normal, bold, bolder, lighter, 100-900)
- `FontStyle`: Style (normal, italic, oblique)
- `Color`: Text color
- `Opacity`: Text opacity (0.0 to 1.0)

### Opacity Settings

Control tooltip transparency with the `Opacity` property (0.0 = fully transparent, 1.0 = fully opaque).

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapTooltipSettings Visible=true Fill="#333333" Opacity="0.75">
    <TreeMapTooltipTextStyle Color="#FFFFFF"></TreeMapTooltipTextStyle>
</TreeMapTooltipSettings>
```

**Complete Styling Example:**

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Sales" TValue="Product" DataSource="Products">
    <TreeMapLeafItemSettings LabelPath="ProductName"></TreeMapLeafItemSettings>
    <TreeMapTooltipSettings 
        Visible=true 
        Fill="#E8F5E9" 
        Opacity="0.95"
        Format="${ProductName}: $${Sales}">
        <TreeMapTooltipBorder Color="#4CAF50" Width="2"></TreeMapTooltipBorder>
        <TreeMapTooltipTextStyle 
            Size="14px" 
            FontFamily="Segoe UI" 
            FontWeight="600"
            Color="#2E7D32">
        </TreeMapTooltipTextStyle>
    </TreeMapTooltipSettings>
</SfTreeMap>

@code {
    public class Product
    {
        public string ProductName { get; set; }
        public int Sales { get; set; }
    }
    
    public List<Product> Products = new List<Product> {
        new Product { ProductName="Laptop", Sales=45000 },
        new Product { ProductName="Desktop", Sales=32000 }
    };
}
```

## Tooltip Positioning and Margins

### Automatic Positioning

Tooltips automatically position themselves to remain visible within the viewport, adjusting based on available space around the cursor with default positioning behavior.

**Positioning Behavior:**
- Default: Appears near cursor position
- Automatically flips to opposite side if constrained
- Maintains visibility within component boundaries

### Margin Configuration

Control spacing around tooltip content through CSS styling within templates or global styles. Margin configuration is typically handled via CSS padding and margin properties rather than component properties.

```html
<style>
    .e-treemap-tooltip {
        padding: 8px 12px !important;
    }
</style>
```

## Animation Settings

Tooltip animations are built-in with default fade-in and fade-out transitions. Animation duration is controlled through component-level settings if supported by your version.

**Default Behavior:**
- Smooth fade-in transition
- Quick fade-out on mouse leave
- No configuration required for standard animation

## Tooltips for Different Data Structures

### Flat Data Tooltips

Tooltips for single-level (flat) data structures using `Visible` and `Format` properties.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Population" TValue="Country" DataSource="Countries">
    <TreeMapLeafItemSettings LabelPath="Name"></TreeMapLeafItemSettings>
    <TreeMapTooltipSettings 
        Visible=true 
        Format="${Name}<br/>Population: ${Population} million<br/>GDP: $${GDP} trillion">
    </TreeMapTooltipSettings>
</SfTreeMap>

@code {
    public class Country
    {
        public string Name { get; set; }
        public int Population { get; set; }
        public double GDP { get; set; }
    }
    
    public List<Country> Countries = new List<Country> {
        new Country { Name="USA", Population=331, GDP=21.4 },
        new Country { Name="China", Population=1439, GDP=14.3 },
        new Country { Name="India", Population=1380, GDP=2.9 }
    };
}
```

### Hierarchical Data Tooltips

Tooltips for multi-level hierarchical data using `Visible` and `Format` properties with multi-level group paths.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Sales" TValue="SalesData" DataSource="SalesRecords">
    <TreeMapLeafItemSettings LabelPath="Product"></TreeMapLeafItemSettings>
    <TreeMapLevels>
        <TreeMapLevel GroupPath="Region"></TreeMapLevel>
        <TreeMapLevel GroupPath="Country"></TreeMapLevel>
    </TreeMapLevels>
    <TreeMapTooltipSettings 
        Visible=true 
        Format="Region: ${Region}<br/>Country: ${Country}<br/>Product: ${Product}<br/>Sales: $${Sales}">
    </TreeMapTooltipSettings>
</SfTreeMap>

@code {
    public class SalesData
    {
        public string Region { get; set; }
        public string Country { get; set; }
        public string Product { get; set; }
        public int Sales { get; set; }
    }
    
    public List<SalesData> SalesRecords = new List<SalesData> {
        new SalesData { Region="Americas", Country="USA", Product="Laptop", Sales=45000 },
        new SalesData { Region="Americas", Country="Canada", Product="Phone", Sales=32000 },
        new SalesData { Region="Europe", Country="Germany", Product="Tablet", Sales=28000 }
    };
}
```

## Advanced Tooltip Scenarios

### Conditional Tooltip Content

Show different tooltip content based on data conditions using the `TooltipTemplate` property with conditional logic.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Sales" TValue="Product" DataSource="Products">
    <TreeMapLeafItemSettings LabelPath="ProductName"></TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible=true>
        <TooltipTemplate>
            @{
                var product = (context as Product);
                <div style="padding: 8px; background: white; border-radius: 4px;">
                    <div style="font-weight: bold;">@product.ProductName</div>
                    <div style="color: #666;">Sales: $@product.Sales.ToString("N0")</div>
                    @if (product.Sales > 40000)
                    {
                        <div style="color: green; font-weight: bold; margin-top: 5px;">⭐ Top Performer</div>
                    }
                    else if (product.Sales < 20000)
                    {
                        <div style="color: orange; margin-top: 5px;">⚠ Needs Attention</div>
                    }
                </div>
            }
        </TooltipTemplate>
    </TreeMapTooltipSettings>
</SfTreeMap>

@code {
    public class Product
    {
        public string ProductName { get; set; }
        public int Sales { get; set; }
    }
    
    public List<Product> Products = new List<Product> {
        new Product { ProductName="Laptop", Sales=45000 },
        new Product { ProductName="Mouse", Sales=8000 },
        new Product { ProductName="Phone", Sales=38000 }
    };
}
```

### Dynamic Formatting

Format numeric values dynamically within tooltips using the `TooltipTemplate` property with C# formatting methods.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Value" TValue="DataItem" DataSource="Data">
    <TreeMapLeafItemSettings LabelPath="Name"></TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible=true>
        <TooltipTemplate>
            @{
                var item = (context as DataItem);
                <div style="padding: 10px; background: #f5f5f5; border-radius: 6px;">
                    <div style="font-size: 16px; font-weight: bold; color: #333;">@item.Name</div>
                    <div style="margin-top: 5px; color: #666;">
                        Value: @item.Value.ToString("N0")
                    </div>
                    <div style="margin-top: 3px; color: #666;">
                        Percentage: @item.Percentage.ToString("P2")
                    </div>
                    <div style="margin-top: 3px; color: @(item.ChangePercent >= 0 ? "green" : "red");">
                        Change: @item.ChangePercent.ToString("+0.00%;-0.00%;0.00%")
                    </div>
                </div>
            }
        </TooltipTemplate>
    </TreeMapTooltipSettings>
</SfTreeMap>

@code {
    public class DataItem
    {
        public string Name { get; set; }
        public int Value { get; set; }
        public double Percentage { get; set; }
        public double ChangePercent { get; set; }
    }
    
    public List<DataItem> Data = new List<DataItem> {
        new DataItem { Name="Product A", Value=50000, Percentage=0.35, ChangePercent=12.5 },
        new DataItem { Name="Product B", Value=30000, Percentage=0.21, ChangePercent=-5.3 }
    };
}
```

### Multi-Line Tooltips

Create comprehensive multi-line tooltips using the `Format` property with `<br/>` tags for line breaks.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="Sales" TValue="Product" DataSource="Products">
    <TreeMapLeafItemSettings LabelPath="ProductName"></TreeMapLeafItemSettings>
    <TreeMapTooltipSettings 
        Visible=true 
        Format="<b>${ProductName}</b><br/>Category: ${Category}<br/>Sales: $${Sales}<br/>Units Sold: ${UnitsSold}<br/>Rating: ${Rating} stars">
    </TreeMapTooltipSettings>
</SfTreeMap>

@code {
    public class Product
    {
        public string ProductName { get; set; }
        public string Category { get; set; }
        public int Sales { get; set; }
        public int UnitsSold { get; set; }
        public double Rating { get; set; }
    }
    
    public List<Product> Products = new List<Product> {
        new Product { ProductName="Laptop", Category="Electronics", Sales=45000, UnitsSold=150, Rating=4.5 },
        new Product { ProductName="Chair", Category="Furniture", Sales=12000, UnitsSold=400, Rating=4.2 }
    };
}
```

**Note:** Use `<br/>` for line breaks and `<b>` tags for bold text in format strings.

## Best Practices

### 1. Keep Tooltip Content Concise

Display only essential information to avoid overwhelming users.

```cshtml
@using Syncfusion.Blazor.TreeMap

<!-- Good: Focused information -->
<TreeMapTooltipSettings 
    Visible=true 
    Format="${ProductName}<br/>Sales: $${Sales}">
</TreeMapTooltipSettings>

<!-- Avoid: Too much information -->
<TreeMapTooltipSettings 
    Visible=true 
    Format="${ProductName}<br/>Sales: ${Sales}<br/>Cost: ${Cost}<br/>Profit: ${Profit}<br/>Date: ${Date}<br/>Region: ${Region}<br/>Category: ${Category}">
</TreeMapTooltipSettings>
```

### 2. Ensure Readability

Use sufficient contrast and appropriate font sizes.

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapTooltipSettings Visible=true Fill="#2C3E50">
    <TreeMapTooltipTextStyle Size="13px" Color="#FFFFFF"></TreeMapTooltipTextStyle>
</TreeMapTooltipSettings>
```

### 3. Format Numeric Values

Make numbers readable with appropriate formatting.

```cshtml
@using Syncfusion.Blazor.TreeMap

<!-- Good: Formatted numbers -->
<TooltipTemplate>
    @{
        var item = (context as DataItem);
        <div>Sales: $@item.Sales.ToString("N0")</div>
    }
</TooltipTemplate>

<!-- Avoid: Raw numbers -->
<TreeMapTooltipSettings Format="Sales: ${Sales}"></TreeMapTooltipSettings>
```

### 4. Use Templates for Complex Content

Leverage templates when format strings become too complex.

```cshtml
@using Syncfusion.Blazor.TreeMap

<!-- Use templates instead of complex format strings -->
<TreeMapTooltipSettings Visible=true>
    <TooltipTemplate>
        @{
            var data = (context as Product);
            <!-- Rich HTML content here -->
        }
    </TooltipTemplate>
</TreeMapTooltipSettings>
```

### 5. Test Tooltip Positioning

Verify tooltips remain visible at TreeMap edges.

```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap Width="100%" Height="500px">
    <!-- Test with items near boundaries -->
</SfTreeMap>
```

### 6. Maintain Consistent Styling

Use consistent colors and styles across all tooltips.

```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapTooltipSettings Visible=true Fill="#F5F5F5" Opacity="0.95">
    <TreeMapTooltipBorder Color="#DDDDDD" Width="1"></TreeMapTooltipBorder>
    <TreeMapTooltipTextStyle Size="13px" FontFamily="Segoe UI" Color="#333333"></TreeMapTooltipTextStyle>
</TreeMapTooltipSettings>
```

## Troubleshooting

### Tooltips Not Appearing

**Problem:** Tooltips don't show on item hover.

**Solutions:**

1. **Verify Visible property is true:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapTooltipSettings Visible=true></TreeMapTooltipSettings>
```

2. **Check data source is bound:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="YourData" TValue="YourType">
```

3. **Ensure WeightValuePath is set:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap WeightValuePath="YourProperty">
```

### Format String Not Working

**Problem:** Format string displays literally instead of interpolating values.

**Solutions:**

1. **Check property names are correct (case-sensitive):**
```cshtml
@using Syncfusion.Blazor.TreeMap

<!-- Data property: ProductName -->
<TreeMapTooltipSettings Format="${ProductName}"></TreeMapTooltipSettings>
```

2. **Verify properties exist in data class:**
```csharp
public class Product
{
    public string ProductName { get; set; } // Must exist
}
```

3. **Use correct syntax:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<!-- Correct -->
<TreeMapTooltipSettings Format="${PropertyName}"></TreeMapTooltipSettings>

<!-- Incorrect -->
<TreeMapTooltipSettings Format="{{PropertyName}}"></TreeMapTooltipSettings>
```

### Template Context is Null

**Problem:** Template throws null reference exception.

**Solutions:**

1. **Cast context safely:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TooltipTemplate>
    @{
        var data = context as YourType;
        if (data != null)
        {
            <div>@data.PropertyName</div>
        }
    }
</TooltipTemplate>
```

2. **Verify TValue type matches:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap TValue="Product" DataSource="Products">
```

### Styling Not Applied

**Problem:** Tooltip styles don't appear as configured.

**Solutions:**

1. **Ensure nested elements are correct:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapTooltipSettings Visible=true Fill="#FF0000">
    <TreeMapTooltipTextStyle Color="#FFFFFF"></TreeMapTooltipTextStyle>
</TreeMapTooltipSettings>
```

2. **Check for CSS conflicts:**
```html
<!-- Remove conflicting global styles -->
<style>
    .e-treemap .e-tooltip {
        /* Don't override component styles */
    }
</style>
```

3. **Use valid color values:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<TreeMapTooltipSettings Fill="#FF0000"></TreeMapTooltipSettings> <!-- Valid -->
<TreeMapTooltipSettings Fill="red"></TreeMapTooltipSettings> <!-- Valid -->
```

### Tooltip Position Issues

**Problem:** Tooltip appears cut off or outside viewport.

**Solutions:**

1. **Ensure adequate container size:**
```cshtml
@using Syncfusion.Blazor.TreeMap

<SfTreeMap Width="100%" Height="500px">
```

2. **Check parent element overflow:**
```html
<div style="overflow: visible;">
    <SfTreeMap>...</SfTreeMap>
</div>
```

### Template Images Not Loading

**Problem:** Images in tooltip templates don't display.

**Solutions:**

1. **Verify image paths are correct:**
```cshtml
<img src="@product.ImageUrl" alt="Product" />
```

2. **Use absolute or root-relative paths:**
```csharp
public string ImageUrl => "/images/products/laptop.png";
```

3. **Check image files exist in wwwroot:**
```
wwwroot/
  images/
    products/
      laptop.png
```

---

**Next Steps:**
- Implement drill-down for hierarchical navigation
- Configure selection and highlight for user interaction
- Set up legends for visual data interpretation
- Add events for tooltip customization
- Enable print and export functionality

**Related Topics:**
- Legend Configuration
- Selection and Highlight
- Drill-Down Navigation
- Events and Methods
- Data Binding Strategies