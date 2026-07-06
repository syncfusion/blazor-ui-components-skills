---
name: syncfusion-blazor-treemap
description: Implement and configure the Syncfusion Blazor TreeMap component (SfTreeMap) for hierarchical data visualization. Use this when working with treemaps, area-proportional visualizations, or heat-map-like block displays. This skill covers TreeMap setup, layout configuration, data binding, color mapping, drill-down navigation, and accessibility features.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
  category: "data-visualization"
---

# Implementing TreeMap

**NuGet:** `Syncfusion.Blazor.TreeMap` + `Syncfusion.Blazor.Themes`  
**Namespace:** `Syncfusion.Blazor.TreeMap`

This skill collects all guidance for implementing the Syncfusion Blazor `TreeMap` component. Use the navigation guide below to open the specific reference topic you need; each reference file is self-contained and includes examples, edge cases, and troubleshooting.

## Official API Surface
- Component: `SfTreeMap<TValue>`
- Interface: `ITreeMap`
- Core child settings: `TreeMapLevels`, `TreeMapLeafItemSettings`, `TreeMapLegendSettings`, `TreeMapTooltipSettings`, `TreeMapSelectionSettings`, `TreeMapHighlightSettings`, `TreeMapEvents`
- Common enums: `LayoutMode`, `RenderingMode`, `LabelPosition`, `LabelPlacement`, `LabelIntersectAction`, `LegendMode`, `LegendPosition`, `LegendOrientation`, `LegendShape`, `SelectionMode`, `HighLightMode`
- Common event args: `LoadEventArgs`, `LoadedEventArgs`, `ItemRenderingEventArgs`, `ItemClickEventArgs`, `ItemSelectedEventArgs`, `ItemMoveEventArgs`, `LegendRenderingEventArgs`, `LegendItemRenderingEventArgs`, `TreeMapTooltipArgs`

## When to Use This Skill
- When integrating a hierarchical, area-proportional visualization in a Blazor app.
- When you need guidance on layouts, color-mapping, labels, legends, or drill-down.
- When configuring data-binding for hierarchical or flat datasets.
- When implementing accessibility, localization, print/export, or performance optimizations.

## ⚠️ Security Warning

**DO NOT bind TreeMap to untrusted public APIs or third-party data sources without proper validation.** Remote data can be manipulated to inject malicious code, corrupt visualizations, or cause denial-of-service attacks. Always:
- ✅ Use only internal, authenticated APIs you own and control
- ✅ Validate and sanitize ALL remote data before binding
- ✅ Implement server-side authorization and rate limiting
- ✅ Use HTTPS and verify SSL/TLS certificates
- ✅ HTML-encode string properties to prevent XSS

**For detailed security guidance, see:** [Security Considerations in data-binding.md](references/data-binding.md)

## Documentation and Navigation Guide

### API Reference
📄 **Read:** [references/api-reference.md](references/api-reference.md)
- Quick lookup for the main TreeMap component, child settings, events, methods, and enums.

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation, NuGet package, project setup, minimal example.

### Data Binding and Sources
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Flat vs hierarchical data, local/remote sources, data adaptors.
- Remote data binding: Only to internal, authenticated APIs you control — do NOT bind directly to untrusted third-party endpoints. See [references/data-binding.md](references/data-binding.md) for the mandatory security checklist and safe patterns (server-side proxying, validation, sanitization, rate limits).

### Layout and Levels
📄 **Read:** [references/layout-and-levels.md](references/layout-and-levels.md)
- Layout algorithms, level grouping, headers and gaps.

### Leaf Items
📄 **Read:** [references/leaf-items.md](references/leaf-items.md)
- Leaf node styling, templates and label positioning.

### Color Mapping
📄 **Read:** [references/color-mapping.md](references/color-mapping.md)
- Range, equal, desaturation, palette mappings and strategies.

### Labels
📄 **Read:** [references/labels.md](references/labels.md)
- Data label templates, formatting and overflow handling.

### Legend
📄 **Read:** [references/legend.md](references/legend.md)
- Legend modes, positioning and smart legend behaviors.

### Tooltips
📄 **Read:** [references/tooltip.md](references/tooltip.md)
- Default and templated tooltips, styling and dynamic content.

### Drill-Down
📄 **Read:** [references/drill-down.md](references/drill-down.md)
- Enabling drill-down, breadcrumbs, and custom navigation.

### Selection & Highlight
📄 **Read:** [references/selection-and-highlight.md](references/selection-and-highlight.md)
- Selection modes, highlight, and programmatic selection.

### Events & Methods
📄 **Read:** [references/events-and-methods.md](references/events-and-methods.md)
- Lifecycle events, render events, print/export methods.

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- Accessibility, localization, performance, placing the TreeMap inside other components.

### Troubleshooting
📄 **Read:** [references/troubleshooting.md](references/troubleshooting.md)
- Common issues, rendering and data-binding fixes.

## Quick Start (Minimal example)

```csharp
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="@Data"> 
  <TreeMapLevels>
    <TreeMapLevel GroupPath="Category" />
  </TreeMapLevels>
</SfTreeMap>

@code {
  public object[] Data = new[] { new { Category = "A", Value = 10 } };
}
```

## Common Patterns
- Quick selection + drill-down for hierarchical exploration.
- Use `RangeColorMapping` for value-driven coloring and `Palette` when mapping explicit colors.
- Prefer server-side paging/aggregation for very large datasets and use client-side rendering for moderate datasets.

## References
- All reference files are in `references/` and are self-contained with TOCs.

---

# Implementing Syncfusion Blazor TreeMap

A comprehensive guide for implementing hierarchical data visualization using Syncfusion Blazor TreeMap component. TreeMaps display hierarchical data as nested rectangles where the size and color of each rectangle represents different data dimensions.

## When to Use This Skill

Use this skill when you need to:

- **Visualize hierarchical data structures** - Display organizational charts, file systems, or nested data with parent-child relationships
- **Show proportional data** - Represent data where rectangle size reflects quantitative values (sales, population, disk space)
- **Enable drill-down navigation** - Allow users to interactively explore multi-level hierarchical data
- **Compare data distributions** - Use color mapping to show additional dimensions (growth, performance, categories)
- **Display large datasets** - Efficiently visualize hundreds or thousands of data points in limited space
- **Create interactive dashboards** - Combine with selection, highlighting, tooltips, and legends for rich user experience

**Common Use Cases:**
- Portfolio analysis and asset allocation
- Market share and competitive analysis
- Budget and resource distribution
- File/folder size visualization
- Organizational structure displays
- Product category sales analysis
- Website analytics (page hierarchy and traffic)

## Component Overview

The TreeMap component organizes data into nested rectangles using various layout algorithms (Squarified, SliceDiceAuto, Horizontal, Vertical). Each rectangle's size is proportional to a specified data value, and colors can represent additional dimensions through sophisticated color mapping strategies.

**Key Capabilities:**
- Multiple layout algorithms for optimal space utilization
- Hierarchical data binding (flat or nested structures)
- Multi-level drill-down with breadcrumb navigation
- Advanced color mapping (range, equal, desaturation, palette)
- Interactive legends with filtering
- Rich tooltips with templates
- Selection and highlight modes
- Print and export (PDF, PNG, JPEG, SVG)
- Full accessibility support (ARIA, keyboard navigation)

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

**When to read:** Setting up TreeMap for the first time, installation, basic implementation

**What's covered:**
- Installation via Visual Studio, VS Code, or .NET CLI
- NuGet package setup (Syncfusion.Blazor.TreeMap)
- Service registration and namespace imports
- CSS theme configuration
- Basic TreeMap implementation with sample data
- License registration for production
- Blazor WebAssembly vs Web App differences

### Data Binding and Sources
📄 **Read:** [references/data-binding.md](references/data-binding.md)

**When to read:** Connecting TreeMap to data sources, handling different data structures

**What's covered:**
- Flat data structure vs hierarchical data structure
- Local data binding (collections, JSON files)
- Remote data binding with authentication and authorization
- **Security considerations** - Only bind to trusted internal APIs with proper authentication
- WeightValuePath and RangeColorValuePath configuration
- Handling nested objects and complex data
- Entity Framework integration
- Data transformation techniques
- Best practices for large datasets

### Layout and Hierarchy
📄 **Read:** [references/layout-and-levels.md](references/layout-and-levels.md)

**When to read:** Configuring TreeMap appearance, multi-level structures

**What's covered:**
- Layout algorithms (Squarified, SliceDiceAuto, Horizontal, Vertical)
- When to use each layout type
- Multi-level TreeMap configuration
- Level-specific customization (headers, borders, colors)
- GroupPath for hierarchical data organization
- Gap and border configuration
- Header templates and positioning

### Leaf Items
📄 **Read:** [references/leaf-items.md](references/leaf-items.md)

**When to read:** Customizing individual TreeMap rectangles (leaf nodes)

**What's covered:**
- Leaf item styling and appearance
- Label formatting and positioning
- Template-based customization
- Gap, border, and color configuration
- Show/hide labels
- Overflow handling
- Interactive leaf patterns

### Color Mapping
📄 **Read:** [references/color-mapping.md](references/color-mapping.md)

**When to read:** Applying colors to represent data dimensions, creating heat maps

**What's covered:**
- Range color mapping (gradients based on value ranges)
- Equal color mapping (distinct colors per category)
- Desaturation color mapping (intensity variations)
- Desaturation with multiple colors
- Palette color mapping
- Binding colors directly from data source
- Handling items excluded from color mapping
- Custom color strategies

### Labels
📄 **Read:** [references/labels.md](references/labels.md)

**When to read:** Adding text labels to TreeMap items

**What's covered:**
- Data label configuration and formatting
- Label positioning options (Center, TopLeft, TopCenter, etc.)
- Font customization
- Label templates with custom content
- Smart label positioning (trim, hide, wrap)
- Format strings and data binding
- Responsive label behavior

### Legend
📄 **Read:** [references/legend.md](references/legend.md)

**When to read:** Adding legends for color mapping interpretation

**What's covered:**
- Legend modes (Default, Interactive, Smart)
- Legend positioning and alignment
- Legend customization (colors, shapes, text)
- Smart legend with toggle functionality
- Legend templates for custom rendering
- Visibility control
- Legend integration with color mappings

### Tooltips
📄 **Read:** [references/tooltip.md](references/tooltip.md)

**When to read:** Adding hover information to TreeMap items

**What's covered:**
- Default tooltip configuration
- Tooltip templates with custom HTML/Blazor components
- Styling and formatting
- Dynamic tooltip content based on data
- Tooltip positioning and animation
- Format strings for values
- Tooltips for different data structures

### Drill-Down Navigation
📄 **Read:** [references/drill-down.md](references/drill-down.md)

**When to read:** Enabling interactive hierarchical navigation

**What's covered:**
- Enabling drill-down on item click
- Breadcrumb navigation configuration
- Header customization for drill-down
- Programmatic drill-down control
- Drill-down events (ItemClick, DrillStart, DrillEnd)
- Resetting to initial view
- Multi-level navigation patterns

### Selection and Highlight
📄 **Read:** [references/selection-and-highlight.md](references/selection-and-highlight.md)

**When to read:** Adding user interaction for selecting/highlighting items

**What's covered:**
- Selection modes (Item, Child, Parent, All)
- Single vs multiple selection
- Highlight on hover configuration
- Customizing selection appearance (colors, borders, opacity)
- Programmatic selection
- Selection events (ItemSelected)
- Combining selection with drill-down

### Events and Methods
📄 **Read:** [references/events-and-methods.md](references/events-and-methods.md)

**When to read:** Handling user interactions, lifecycle events, or invoking component methods

**What's covered:**
- Component lifecycle events (Loaded, BeforeRender)
- User interaction events (ItemClick, ItemSelected, ItemHighlight)
- Rendering events (ItemRendering, TooltipRender, LegendRender)
- Drill-down events (DrillStart, DrillEnd)
- Print() and Export() methods (PDF, PNG, JPEG, SVG)
- Refresh() and other utility methods
- Event handling patterns and best practices

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)

**When to read:** Implementing print/export, localization, accessibility, or performance optimization

**What's covered:**
- Print and export functionality (PDF, PNG, JPEG, SVG)
- Internationalization and localization
- RTL (right-to-left) support
- Accessibility features (ARIA, keyboard navigation, screen readers)
- Performance optimization techniques
- Responsive design patterns
- Embedding TreeMap in other components
- CSP (Content Security Policy) compliance

## Quick Start Example

Here's a minimal working example to get started:

```razor
@page "/treemap-demo"
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="@GrowthReports"
            WeightValuePath="GDP"
            TValue="Country">
    <TreeMapLeafItemSettings LabelPath="Name">
    </TreeMapLeafItemSettings>
</SfTreeMap>

@code {
    public class Country
    {
        public string Name { get; set; }
        public double GDP { get; set; }
    }

    private List<Country> GrowthReports = new List<Country>
    {
        new Country { Name = "United States", GDP = 17946 },
        new Country { Name = "China", GDP = 10866 },
        new Country { Name = "Japan", GDP = 4123 },
        new Country { Name = "Germany", GDP = 3355 },
        new Country { Name = "United Kingdom", GDP = 2848 }
    };
}
```

**Prerequisites:**
- Install `Syncfusion.Blazor.TreeMap` NuGet package
- Register services in `Program.cs`: `builder.Services.AddSyncfusionBlazor();`
- Add theme CSS reference in layout file
- Register Syncfusion license for production use

## Common Patterns

### Pattern 1: Hierarchical Data with Color Mapping

```razor
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="@SalesData"
            WeightValuePath="Sales"
            RangeColorValuePath="Growth"
            TValue="Region">
    <TreeMapLeafItemSettings LabelPath="Name">
        <TreeMapLeafLabelStyle Color="#FFFFFF"></TreeMapLeafLabelStyle>
    </TreeMapLeafItemSettings>
    <TreeMapLegendSettings Visible="true" Position="@using Syncfusion.Blazor.TreeMap.LegendPosition.Top">
    </TreeMapLegendSettings>
    <TreeMapLevels>
        <TreeMapLevel GroupPath="Continent" HeaderFormat="${Continent} - ${Sales}">
            <TreeMapLevelBorder Color="#FFFFFF" Width="1"></TreeMapLevelBorder>
        </TreeMapLevel>
    </TreeMapLevels>
    <TreeMapRangeColorMappings>
        <TreeMapRangeColorMapping From="0" To="5" Color="@("#70AD47")" />
        <TreeMapRangeColorMapping From="5" To="10" Color="@("#FFC000")" />
        <TreeMapRangeColorMapping From="10" To="20" Color="@("#FF5722")" />
    </TreeMapRangeColorMappings>
</SfTreeMap>
```

**Use when:** Displaying hierarchical business data with performance indicators

### Pattern 2: Drill-Down with Breadcrumb Navigation

```razor
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="@OrganizationData"
            WeightValuePath="EmployeeCount"
            EnableDrillDown="true"
            TValue="Department">
    <TreeMapLeafItemSettings LabelPath="Name">
    </TreeMapLeafItemSettings>
    <TreeMapBreadcrumbSettings Visible="true">
    </TreeMapBreadcrumbSettings>
    <TreeMapLevels>
        <TreeMapLevel GroupPath="Division" HeaderFormat="${Division}">
        </TreeMapLevel>
        <TreeMapLevel GroupPath="Team" HeaderFormat="${Team}">
        </TreeMapLevel>
    </TreeMapLevels>
</SfTreeMap>
```

**Use when:** Users need to navigate through multi-level organizational or hierarchical data

### Pattern 3: Interactive TreeMap with Selection and Tooltips

```razor
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="@PortfolioData"
            WeightValuePath="Value"
            TValue="Investment">
    <TreeMapLeafItemSettings LabelPath="Name">
    </TreeMapLeafItemSettings>
    <TreeMapTooltipSettings Visible="true">
        <Template>
            @{
                var data = context as Investment;
                <div style="padding:5px;background:#fff;border:1px solid #ccc">
                    <b>@data.Name</b><br/>
                    Value: $@data.Value.ToString("N0")<br/>
                    Return: @data.Return%
                </div>
            }
        </Template>
    </TreeMapTooltipSettings>
    <TreeMapSelectionSettings Enable="true" Fill="#58a0d3" Opacity="0.8">
    </TreeMapSelectionSettings>
    <TreeMapHighlightSettings Enable="true" Fill="#e5e5e5" Opacity="0.6">
    </TreeMapHighlightSettings>
</SfTreeMap>
```

**Use when:** Building interactive dashboards requiring user engagement

### Pattern 4: Squarified Layout with Custom Palette

```razor
@using Syncfusion.Blazor.TreeMap

<SfTreeMap DataSource="@Categories"
            WeightValuePath="Sales"
            LayoutType="LayoutMode.Squarified"
            Palette="@(new string[] {"#9999ff", "#CBCBCB", "#E8DAFF", "#E4E4E4"})"
            TValue="Category">
    <TreeMapLeafItemSettings LabelPath="Name" Fill="#8ebfe2">
        <TreeMapLeafBorder Color="#FFFFFF" Width="2"></TreeMapLeafBorder>
    </TreeMapLeafItemSettings>
    <TreeMapTitleSettings Text="Product Category Sales">
    </TreeMapTitleSettings>
</SfTreeMap>
```

**Use when:** Creating visually distinct category comparisons

## Key Props and Configuration

**Essential Properties:**

| Property | Type | Purpose |
|----------|------|---------|
| `DataSource` | IEnumerable<TValue> | Data collection to bind |
| `WeightValuePath` | string | Property determining rectangle size |
| `RangeColorValuePath` | string | Property for color mapping values |
| `TValue` | Type | Generic type of data items |
| `LayoutType` | LayoutMode | Layout algorithm (Squarified, SliceDiceAuto, etc.) |
| `EnableDrillDown` | bool | Enable hierarchical navigation |
| `Palette` | string[] | Color palette for items |

**Child Components:**
- `TreeMapLeafItemSettings` - Leaf node configuration
- `TreeMapLevels` - Multi-level hierarchy
- `TreeMapRangeColorMappings` / `TreeMapEqualColorMappings` - Color strategies
- `TreeMapLegendSettings` - Legend configuration
- `TreeMapTooltipSettings` - Tooltip configuration
- `TreeMapSelectionSettings` - Selection behavior
- `TreeMapHighlightSettings` - Hover effects

## Related Skills

- [HeatMap Chart](../syncfusion-blazor-heatmap-chart/) - For matrix-based heat map visualization
- For other data visualization components, explore the Data Visualization category

## Troubleshooting Quick Tips

- **TreeMap not rendering:** Ensure NuGet package is installed, services are registered, and CSS theme is referenced
- **Data not displaying:** Verify `WeightValuePath` matches a numeric property in your data model
- **Colors not applying:** Check that `RangeColorValuePath` is set and color mappings are properly configured
- **Drill-down not working:** Set `EnableDrillDown="true"` and configure `TreeMapLevels` with `GroupPath`
- **Performance issues:** For large datasets (>1000 items), consider pagination or hierarchical loading with drill-down

For comprehensive details on any topic, refer to the appropriate reference file listed in the navigation guide above.