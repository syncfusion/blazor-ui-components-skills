# Customization Options for Blazor Sankey Diagram

This guide covers extensive customization options for the Syncfusion Blazor Sankey Diagram, including background styling, dimensions, layout orientation, and right-to-left (RTL) support.

## Table of Contents

- [Background Customization](#background-customization)
- [Dimensions (Width and Height)](#dimensions-width-and-height)
- [Orientation Control](#orientation-control)
- [Right-to-Left (RTL) Support](#right-to-left-rtl-support)
- [Theme Integration](#theme-integration)
- [Complete Customization Examples](#complete-customization-examples)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

The Blazor Sankey Diagram provides extensive customization options to match your application's visual language and support different layout requirements. These options allow you to control appearance, dimensions, flow direction, and cultural presentation.

**When to customize:**
- Match application branding and theme
- Support different screen sizes and responsive layouts
- Adapt to right-to-left languages (Arabic, Hebrew)
- Optimize for specific use cases (vertical hierarchy, horizontal flow)

## Background Customization

Customize the diagram background using `BackgroundColor` and `BackgroundImage` properties on the `SfSankey` component.

**Required namespace:**
```csharp
@using Syncfusion.Blazor.Sankey
```

### Background Color

Use `BackgroundColor` to set a solid color background. Accepts any valid CSS color value.

**Supported color formats:**
- **Hex**: `"#f8f9fa"`, `"#0b1320"`
- **Named colors**: `"white"`, `"lightgray"`, `"transparent"`
- **RGB**: `"rgb(248, 249, 250)"`
- **RGBA**: `"rgba(248, 249, 250, 0.95)"` (with transparency)

**Example with background color:**
```razor
@using Syncfusion.Blazor.Sankey

<SfSankey BackgroundColor="#f8f9fa" Nodes="@Nodes" Links="@Links">
</SfSankey>

@code {
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
    public List<SankeyDataLink> Links = new List<SankeyDataLink>();

    protected override void OnInitialized()
    {
        Nodes = new List<SankeyDataNode>()
        {
            new SankeyDataNode() { Id = "Solar", Label = new SankeyDataLabel() { Text = "Solar" } },
            new SankeyDataNode() { Id = "Wind", Label = new SankeyDataLabel() { Text = "Wind" } },
            new SankeyDataNode() { Id = "Electricity", Label = new SankeyDataLabel() { Text = "Electricity" } },
            new SankeyDataNode() { Id = "Residential", Label = new SankeyDataLabel() { Text = "Residential" } }
        };

        Links = new List<SankeyDataLink>()
        {
            new SankeyDataLink() { SourceId = "Solar", TargetId = "Electricity", Value = 100 },
            new SankeyDataLink() { SourceId = "Wind", TargetId = "Electricity", Value = 120 },
            new SankeyDataLink() { SourceId = "Electricity", TargetId = "Residential", Value = 220 }
        };
    }
}
```

**Dark theme example:**
```razor
<SfSankey BackgroundColor="#0b1320" Nodes="@Nodes" Links="@Links">
    <SankeyNodeSettings Color="#1c3f60"></SankeyNodeSettings>
    <SankeyLinkSettings Color="#afc1d0"></SankeyLinkSettings>
    <SankeyLabelSettings Color="#FFFFFF" FontWeight="400"></SankeyLabelSettings>
</SfSankey>

@code {
    // Nodes and links initialization
}
```

### Background Image

Use `BackgroundImage` to apply an image background. Provide a URL or file path to the image.

**Example with background image:**
```razor
<SfSankey BackgroundImage="assets/sankey-background.png" Nodes="@Nodes" Links="@Links">
</SfSankey>

@code {
    // Nodes and links
}
```

**Image considerations:**
- Use lightweight images to avoid slow loading
- Ensure sufficient contrast between image and diagram elements
- Consider using semi-transparent overlays for better readability
- Test across different screen sizes for responsive layouts

### Combined Background Styling

Combine color and image for layered effects:

```razor
<SfSankey 
    BackgroundColor="rgba(0, 0, 0, 0.3)" 
    BackgroundImage="assets/texture.jpg" 
    Nodes="@Nodes" 
    Links="@Links">
</SfSankey>

@code {
    // Nodes and links
}
```

## Dimensions (Width and Height)

Control diagram size using `Width` and `Height` properties. Support both absolute (pixels) and relative (percentage) values.

### Fixed Dimensions

Use pixel values for fixed-size diagrams:

```razor
<SfSankey Width="800px" Height="600px" Nodes="@Nodes" Links="@Links">
</SfSankey>

@code {
    // Nodes and links
}
```

### Responsive Dimensions

Use percentage values for responsive layouts:

```razor
<SfSankey Width="100%" Height="500px" Nodes="@Nodes" Links="@Links">
</SfSankey>

@code {
    // Nodes and links
}
```

**Responsive considerations:**
- Use `Width="100%"` to fill parent container width
- Combine percentage width with fixed height for controlled aspect ratio
- Consider viewport units (`vh`, `vw`) for full-screen diagrams
- Test across different screen sizes and orientations

### Dynamic Dimensions

Bind dimensions to variables for runtime control:

```razor
<SfSankey Width="@DiagramWidth" Height="@DiagramHeight" Nodes="@Nodes" Links="@Links">
</SfSankey>

<div>
    <button @onclick="() => SetSize(800, 600)">Medium</button>
    <button @onclick="() => SetSize(1200, 800)">Large</button>
    <button @onclick="() => SetSize(600, 400)">Small</button>
</div>

@code {
    private string DiagramWidth = "800px";
    private string DiagramHeight = "600px";

    private void SetSize(int width, int height)
    {
        DiagramWidth = $"{width}px";
        DiagramHeight = $"{height}px";
        StateHasChanged();
    }

    // Nodes and links initialization
}
```

## Orientation Control

Control flow direction using the `Orientation` property. Choose between horizontal and vertical layouts.

**Available orientations:**
- **`SankeyOrientation.Horizontal`** (default) - Left-to-right flow
- **`SankeyOrientation.Vertical`** - Top-to-bottom flow
- **`SankeyOrientation.Auto`** - Component decides based on available space

### Horizontal Orientation (Default)

Default horizontal flow from left to right:

```razor
<SfSankey 
    Orientation="SankeyOrientation.Horizontal" 
    Nodes="@Nodes" 
    Links="@Links">
</SfSankey>

@code {
    // Nodes and links
}
```

**When to use horizontal:**
- Standard process flows and workflows
- Time-series data (chronological progression)
- Wide displays with landscape orientation
- Traditional left-to-right reading patterns

### Vertical Orientation

Top-to-bottom flow for hierarchical or vertical layouts:

```razor
<SfSankey 
    Orientation="SankeyOrientation.Vertical" 
    Width="700px" 
    Height="800px" 
    Nodes="@Nodes" 
    Links="@Links">
</SfSankey>

@code {
    // Nodes and links initialization
}
```

**Effects of vertical orientation:**
1. Nodes arranged from top to bottom
2. Links flow downward between nodes
3. Labels align with vertical flow (typically to the right of nodes)
4. Space used more efficiently for many nodes or longer labels
5. Top-down arrangement implies hierarchical or sequential relationships
6. More responsive on narrow/mobile screens
7. Large diagrams scroll vertically instead of horizontally

**When to use vertical:**
- Organizational hierarchies
- Dependency chains or decision trees
- Mobile or narrow viewport layouts
- Emphasize top-down relationships
- Long node labels that need more horizontal space

### Dynamic Orientation

Toggle orientation at runtime:

```razor
<SfSankey 
    Orientation="@CurrentOrientation" 
    Width="700px" 
    Height="700px" 
    Nodes="@Nodes" 
    Links="@Links">
</SfSankey>

<div>
    <button @onclick="() => CurrentOrientation = SankeyOrientation.Horizontal">
        Horizontal
    </button>
    <button @onclick="() => CurrentOrientation = SankeyOrientation.Vertical">
        Vertical
    </button>
</div>

@code {
    private SankeyOrientation CurrentOrientation = SankeyOrientation.Horizontal;

    // Nodes and links initialization
}
```

## Right-to-Left (RTL) Support

Enable right-to-left layout for languages like Arabic, Hebrew, Persian, and Urdu using the `EnableRTL` property.

**Basic RTL configuration:**
```razor
<SfSankey EnableRTL="true" Nodes="@Nodes" Links="@Links">
</SfSankey>

@code {
    // Nodes and links
}
```

### Effects of Enabling RTL

When `EnableRTL="true"`:

1. **Node Order**: First node appears on the right side
2. **Link Direction**: Flows from right to left
3. **Label Alignment**: Labels align for right-to-left reading
4. **Tooltip Position**: Tooltips position appropriately for RTL
5. **Legend Layout**: Legend order and positioning mirrored
6. **Text Direction**: All text respects RTL reading direction

**Complete RTL example:**
```razor
@using Syncfusion.Blazor.Sankey

<SfSankey EnableRTL="true" Width="100%" Height="600px" Nodes="@Nodes" Links="@Links">
    <SankeyNodeSettings Width="30" Padding="15"></SankeyNodeSettings>
    <SankeyLabelSettings Visible="true" FontSize="14px"></SankeyLabelSettings>
    <SankeyLegendSettings Visible="true" Position="SankeyLegendPosition.Bottom"></SankeyLegendSettings>
</SfSankey>

@code {
    public List<SankeyDataNode> Nodes = new List<SankeyDataNode>();
    public List<SankeyDataLink> Links = new List<SankeyDataLink>();

    protected override void OnInitialized()
    {
        Nodes = new List<SankeyDataNode>()
        {
            new SankeyDataNode() { Id = "مصدر1", Label = new SankeyDataLabel() { Text = "مصدر الطاقة 1" } },
            new SankeyDataNode() { Id = "مصدر2", Label = new SankeyDataLabel() { Text = "مصدر الطاقة 2" } },
            new SankeyDataNode() { Id = "كهرباء", Label = new SankeyDataLabel() { Text = "كهرباء" } },
            new SankeyDataNode() { Id = "سكني", Label = new SankeyDataLabel() { Text = "سكني" } }
        };

        Links = new List<SankeyDataLink>()
        {
            new SankeyDataLink() { SourceId = "مصدر1", TargetId = "كهرباء", Value = 100 },
            new SankeyDataLink() { SourceId = "مصدر2", TargetId = "كهرباء", Value = 120 },
            new SankeyDataLink() { SourceId = "كهرباء", TargetId = "سكني", Value = 220 }
        };
    }
}
```

**Best practices for RTL:**
- Test with actual RTL content (Arabic, Hebrew text)
- Ensure all labels use appropriate fonts supporting RTL languages
- Verify tooltip and legend behavior in RTL mode
- Consider cultural conventions for your target audience
- Combine with appropriate CSS direction properties on parent containers

## Theme Integration

Integrate with Syncfusion themes and custom application themes.

### Using Syncfusion Themes

Syncfusion Blazor components support multiple built-in themes:

**Available themes:**
- Bootstrap 5 (`bootstrap5`, `bootstrap5-dark`)
- Material (`material`, `material-dark`, `material3`, `material3-dark`)
- Tailwind CSS (`tailwind`, `tailwind-dark`)
- Fluent (`fluent`, `fluent-dark`)
- Fabric (`fabric`, `fabric-dark`)
- High Contrast (`highcontrast`)

**Theme reference in _Host.cshtml or App.razor:**
```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```

### Custom Theme Colors

Coordinate Sankey colors with your application theme:

```razor
<SfSankey BackgroundColor="@ThemeColors.Background" Nodes="@Nodes" Links="@Links">
    <SankeyNodeSettings Color="@ThemeColors.Primary"></SankeyNodeSettings>
    <SankeyLinkSettings Color="@ThemeColors.Secondary" Opacity="0.5"></SankeyLinkSettings>
    <SankeyLabelSettings Color="@ThemeColors.Text" FontFamily="@ThemeColors.Font"></SankeyLabelSettings>
</SfSankey>

@code {
    public static class ThemeColors
    {
        public const string Background = "#ffffff";
        public const string Primary = "#0d6efd";
        public const string Secondary = "#6c757d";
        public const string Text = "#212529";
        public const string Font = "Segoe UI, system-ui, sans-serif";
    }

    // Nodes and links
}
```

## Complete Customization Examples

### Example 1: Professional Light Theme

```razor
@using Syncfusion.Blazor.Sankey

<SfSankey 
    Width="100%" 
    Height="600px"
    BackgroundColor="#f8f9fa"
    Orientation="SankeyOrientation.Horizontal"
    EnableRTL="false"
    Nodes="@Nodes" 
    Links="@Links">
    <SankeyNodeSettings 
        Width="25" 
        Padding="12" 
        Color="#0d6efd">
    </SankeyNodeSettings>
    <SankeyLinkSettings 
        Color="#6c757d" 
        Opacity="0.4">
    </SankeyLinkSettings>
    <SankeyLabelSettings 
        Visible="true" 
        Color="#212529" 
        FontSize="13px"
        FontFamily="Segoe UI">
    </SankeyLabelSettings>
    <SankeyLegendSettings 
        Visible="true" 
        Position="SankeyLegendPosition.Bottom"
        Background="#ffffff">
    </SankeyLegendSettings>
</SfSankey>

@code {
    // Nodes and links initialization
}
```

### Example 2: Dark Theme with Vertical Layout

```razor
@using Syncfusion.Blazor.Sankey

<SfSankey 
    Width="800px" 
    Height="900px"
    BackgroundColor="#0b1320"
    Orientation="SankeyOrientation.Vertical"
    EnableRTL="false"
    Nodes="@Nodes" 
    Links="@Links">
    <SankeyNodeSettings 
        Width="30" 
        Padding="15" 
        Color="#1c3f60">
    </SankeyNodeSettings>
    <SankeyLinkSettings 
        Color="#afc1d0" 
        Opacity="0.3">
    </SankeyLinkSettings>
    <SankeyLabelSettings 
        Visible="true" 
        Color="#ffffff" 
        FontSize="14px"
        FontWeight="400"
        FontFamily="Arial">
    </SankeyLabelSettings>
    <SankeyLegendSettings 
        Visible="false">
    </SankeyLegendSettings>
    <SankeyTooltipSettings 
        Enable="true"
        Fill="#34495e"
        Opacity="0.9">
        <SankeyTooltipTextStyle Color="#ecf0f1"></SankeyTooltipTextStyle>
    </SankeyTooltipSettings>
</SfSankey>

@code {
    // Nodes and links initialization
}
```

### Example 3: High-Contrast Accessible Theme

```razor
@using Syncfusion.Blazor.Sankey

<SfSankey 
    Width="100%" 
    Height="650px"
    BackgroundColor="#000000"
    Orientation="SankeyOrientation.Horizontal"
    Nodes="@Nodes" 
    Links="@Links">
    <SankeyNodeSettings 
        Width="35" 
        Padding="18" 
        Color="#ffff00">
    </SankeyNodeSettings>
    <SankeyLinkSettings 
        Color="#ffffff" 
        Opacity="0.7">
    </SankeyLinkSettings>
    <SankeyLabelSettings 
        Visible="true" 
        Color="#ffff00" 
        FontSize="16px"
        FontWeight="600"
        FontFamily="Arial">
    </SankeyLabelSettings>
    <SankeyLegendSettings 
        Visible="true" 
        Position="SankeyLegendPosition.Right"
        Background="#000000">
        <SankeyLegendTextStyle Color="#ffff00" FontSize="14px" FontWeight="600"></SankeyLegendTextStyle>
    </SankeyLegendSettings>
</SfSankey>

@code {
    // Nodes and links initialization
}
```

## Best Practices

1. **Background Selection**: Choose backgrounds with good contrast to diagram elements
2. **Responsive Design**: Use percentage widths and consider mobile viewports
3. **Orientation Choice**: Match orientation to data structure and screen layout
4. **RTL Support**: Test thoroughly with actual RTL content and fonts
5. **Theme Consistency**: Coordinate colors with overall application theme
6. **Performance**: Avoid heavy background images that slow loading
7. **Accessibility**: Ensure sufficient color contrast for WCAG compliance
8. **Testing**: Verify appearance across browsers and screen sizes

## Troubleshooting

### Background Not Showing

**Problem**: Background color or image doesn't appear

**Solutions**:
- Verify the property name (`BackgroundColor`, not `Background`)
- Check CSS color format is valid (hex, rgb, rgba, named)
- For images, verify file path is correct and accessible
- Ensure no parent styles are overriding the background
- Check browser DevTools to see computed styles

### Diagram Size Issues

**Problem**: Diagram appears too small or doesn't fill container

**Solutions**:
- Use `Width="100%"` to fill parent container width
- Ensure parent container has defined dimensions
- Check for CSS constraints on parent elements
- Verify no `max-width` or `max-height` restrictions
- Use browser DevTools to inspect element dimensions

### RTL Not Working Properly

**Problem**: RTL layout doesn't display correctly

**Solutions**:
- Verify `EnableRTL="true"` (case-sensitive)
- Ensure fonts support RTL characters
- Add `dir="rtl"` to parent HTML elements if needed
- Check that node labels use actual RTL text
- Test with real RTL content (Arabic, Hebrew)

### Orientation Change Not Reflecting

**Problem**: Changing orientation doesn't update diagram

**Solutions**:
- Call `StateHasChanged()` after updating orientation property
- Verify property binding is correct (`@CurrentOrientation`)
- Check that enum value is correct (`SankeyOrientation.Vertical`)
- Ensure component re-renders after property change
- Try explicit height and width values for vertical orientation

### Theme Colors Not Matching

**Problem**: Colors don't match application theme

**Solutions**:
- Verify theme CSS file is loaded in project
- Check color values in node/link/label settings
- Use CSS custom properties for dynamic theming
- Ensure `BackgroundColor` doesn't conflict with theme
- Test with different Syncfusion themes to verify compatibility

## Key Considerations

- **Background**: Choose colors and images with good contrast
- **Dimensions**: Use percentage values for responsive layouts
- **RTL Support**: Test with actual RTL content and appropriate fonts
- **Orientation**: Match to data structure and display requirements
- **Themes**: Coordinate with overall application visual design
- **Performance**: Optimize background images and avoid heavy assets
- **Accessibility**: Ensure WCAG-compliant color contrast ratios

## Summary

The Blazor Sankey Diagram provides extensive customization options to match your application's requirements. Use `BackgroundColor` and `BackgroundImage` for visual styling, `Width` and `Height` for sizing, `Orientation` for flow direction, and `EnableRTL` for right-to-left language support. Combine these options with node, link, and label settings to create cohesive, accessible, and visually appealing diagrams that integrate seamlessly with your application's design system.
