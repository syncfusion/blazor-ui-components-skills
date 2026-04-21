# Labels in Blazor Sankey Diagram

This guide covers label configuration and customization in the Syncfusion Blazor Sankey Diagram. Labels provide textual context for nodes, improving readability and data comprehension.

## Table of Contents

- [Overview](#overview)
- [Label Text Configuration](#label-text-configuration)
- [Global Label Styling with SankeyLabelSettings](#global-label-styling-with-sankeylabelsettings)
- [Label Visibility](#label-visibility)
- [Label Color](#label-color)
- [Font Sizing](#font-sizing)
- [Font Family](#font-family)
- [Font Weight](#font-weight)
- [Font Style](#font-style)
- [Label Padding](#label-padding)
- [Complete Styling Examples](#complete-styling-examples)
- [Label Best Practices](#label-best-practices)
- [Dynamic Label Updates](#dynamic-label-updates)
- [Troubleshooting](#troubleshooting)
- [Complete Working Example](#complete-working-example)
- [Summary](#summary)

## Overview

Labels in a Sankey diagram serve to:
- **Identify nodes**: Display names or descriptions for categories/stages
- **Improve readability**: Make the diagram self-explanatory
- **Provide context**: Help users understand what each node represents

Labels are configured at two levels:
1. **Node level**: Define label text via `SankeyDataLabel` in each node
2. **Global level**: Style all labels using `SankeyLabelSettings`

## Label Text Configuration

### Node Labels

Labels are defined within `SankeyDataNode` using the `Label` property:

```csharp
Nodes = new List<SankeyDataNode>()
{
    new SankeyDataNode() 
    { 
        Id = "solar_energy",
        Label = new SankeyDataLabel() { Text = "Solar Energy" }
    },
    new SankeyDataNode() 
    { 
        Id = "electricity_grid",
        Label = new SankeyDataLabel() { Text = "Electricity Grid" }
    }
};
```

### Labels vs. IDs

If you don't provide a `Label`, the node `Id` is used as the label text:

```csharp
// Without Label - displays "Solar"
new SankeyDataNode() { Id = "Solar" }

// With Label - displays "Solar Energy Production"
new SankeyDataNode() 
{ 
    Id = "Solar",
    Label = new SankeyDataLabel() { Text = "Solar Energy Production" }
}
```

**When to use labels:**
- Display text differs from technical ID
- Multi-word labels with proper formatting
- User-friendly names vs. database keys

---

## Global Label Styling with SankeyLabelSettings

The `SankeyLabelSettings` component configures the appearance of all labels globally.

### Available Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `Visible` | bool | true | Show or hide all labels |
| `Color` | string | null | Text color (hex, rgb, named) |
| `FontSize` | string | "12px" | Font size with units (px, pt, em) |
| `FontFamily` | string | "Segoe UI" | Font family name |
| `FontWeight` | string | "normal" | Font weight (normal, bold, 400-900) |
| `FontStyle` | string | "normal" | Font style (normal, italic, oblique) |
| `Padding` | double | 8 | Spacing around label text |

### Basic Label Configuration

```razor
@page "/labels-demo"
@using Syncfusion.Blazor.Sankey

<h3>Label Configuration Example</h3>

<SfSankey Nodes="@Nodes" Links="@Links" Width="100%" Height="600px">
    <SankeyNodeSettings Width="25" Padding="20" Color="#2e7d32"></SankeyNodeSettings>
    <SankeyLabelSettings 
        Visible="true"
        Color="#ffffff"
        FontSize="14px"
        FontWeight="600">
    </SankeyLabelSettings>
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
            new SankeyDataNode() { Id = "Residential", Label = new SankeyDataLabel() { Text = "Residential" } },
            new SankeyDataNode() { Id = "Commercial", Label = new SankeyDataLabel() { Text = "Commercial" } }
        };

        Links = new List<SankeyDataLink>()
        {
            new SankeyDataLink() { SourceId = "Solar", TargetId = "Electricity", Value = 40 },
            new SankeyDataLink() { SourceId = "Wind", TargetId = "Electricity", Value = 60 },
            new SankeyDataLink() { SourceId = "Electricity", TargetId = "Residential", Value = 45 },
            new SankeyDataLink() { SourceId = "Electricity", TargetId = "Commercial", Value = 55 }
        };
    }
}
```

---

## Label Visibility

Control whether labels are shown or hidden:

```razor
<!-- Show labels (default) -->
<SankeyLabelSettings Visible="true"></SankeyLabelSettings>

<!-- Hide labels -->
<SankeyLabelSettings Visible="false"></SankeyLabelSettings>
```

**Use cases:**
- Hide labels for minimalist visualizations
- Show labels for self-explanatory diagrams
- Toggle visibility based on diagram complexity

---

## Label Color

Set text color for visibility and theming:

```razor
<!-- Hex color -->
<SankeyLabelSettings Color="#000000"></SankeyLabelSettings>

<!-- RGB color -->
<SankeyLabelSettings Color="rgb(0, 0, 0)"></SankeyLabelSettings>

<!-- Named color -->
<SankeyLabelSettings Color="white"></SankeyLabelSettings>

<!-- RGBA with transparency -->
<SankeyLabelSettings Color="rgba(255, 255, 255, 0.9)"></SankeyLabelSettings>
```

**Color Guidelines:**
- **White (#ffffff)**: For dark node colors
- **Black (#000000)**: For light node colors
- **High contrast**: Ensure readability against node background
- **Theme colors**: Match application color scheme

---

## Font Sizing

Control label text size:

```razor
<!-- Small text -->
<SankeyLabelSettings FontSize="10px"></SankeyLabelSettings>

<!-- Standard text (default) -->
<SankeyLabelSettings FontSize="12px"></SankeyLabelSettings>

<!-- Medium text -->
<SankeyLabelSettings FontSize="14px"></SankeyLabelSettings>

<!-- Large text -->
<SankeyLabelSettings FontSize="16px"></SankeyLabelSettings>

<!-- Extra large text -->
<SankeyLabelSettings FontSize="18px"></SankeyLabelSettings>
```

**Guidelines:**
- **10-12px**: Many nodes, compact display
- **12-14px**: Standard, balanced readability
- **14-16px**: Emphasis, presentations
- **16px+**: Large displays, accessibility

**Units supported:** px, pt, em, rem

```razor
<!-- Pixels -->
<SankeyLabelSettings FontSize="14px"></SankeyLabelSettings>

<!-- Points -->
<SankeyLabelSettings FontSize="11pt"></SankeyLabelSettings>

<!-- Em units (relative to parent) -->
<SankeyLabelSettings FontSize="0.875em"></SankeyLabelSettings>
```

---

## Font Family

Specify the typeface for labels:

```razor
<!-- System fonts -->
<SankeyLabelSettings FontFamily="Arial"></SankeyLabelSettings>
<SankeyLabelSettings FontFamily="Segoe UI"></SankeyLabelSettings>
<SankeyLabelSettings FontFamily="Verdana"></SankeyLabelSettings>

<!-- Web-safe fonts -->
<SankeyLabelSettings FontFamily="Georgia"></SankeyLabelSettings>
<SankeyLabelSettings FontFamily="Times New Roman"></SankeyLabelSettings>

<!-- Font stack (fallbacks) -->
<SankeyLabelSettings FontFamily="'Segoe UI', Tahoma, Geneva, Verdana, sans-serif"></SankeyLabelSettings>

<!-- Custom web fonts -->
<SankeyLabelSettings FontFamily="'Roboto', sans-serif"></SankeyLabelSettings>
```

**Font Recommendations:**
- **Sans-serif**: Modern, clean (Arial, Segoe UI, Roboto)
- **Serif**: Traditional, formal (Georgia, Times New Roman)
- **Monospace**: Technical data (Consolas, Courier New)

---

## Font Weight

Control text thickness/boldness:

```razor
<!-- Normal weight (default) -->
<SankeyLabelSettings FontWeight="normal"></SankeyLabelSettings>
<SankeyLabelSettings FontWeight="400"></SankeyLabelSettings>

<!-- Bold weight -->
<SankeyLabelSettings FontWeight="bold"></SankeyLabelSettings>
<SankeyLabelSettings FontWeight="700"></SankeyLabelSettings>

<!-- Light weight -->
<SankeyLabelSettings FontWeight="300"></SankeyLabelSettings>

<!-- Extra bold -->
<SankeyLabelSettings FontWeight="900"></SankeyLabelSettings>
```

**Common Values:**
- **300**: Light
- **400 / normal**: Regular
- **500**: Medium
- **600**: Semi-bold
- **700 / bold**: Bold
- **900**: Black/Extra bold

**Use cases:**
- **Light (300)**: Subtle, secondary information
- **Normal (400)**: Standard readability
- **Semi-bold (600)**: Moderate emphasis
- **Bold (700)**: Strong emphasis, headers

---

## Font Style

Apply italic or oblique styling:

```razor
<!-- Normal style (default) -->
<SankeyLabelSettings FontStyle="normal"></SankeyLabelSettings>

<!-- Italic style -->
<SankeyLabelSettings FontStyle="italic"></SankeyLabelSettings>

<!-- Oblique style (slanted) -->
<SankeyLabelSettings FontStyle="oblique"></SankeyLabelSettings>
```

**Use cases:**
- **Italic**: Emphasis, quotes, foreign terms
- **Normal**: Standard presentation
- **Oblique**: Similar to italic, browser-rendered slant

---

## Label Padding

Control spacing around label text:

```razor
<!-- Tight spacing -->
<SankeyLabelSettings Padding="4"></SankeyLabelSettings>

<!-- Standard spacing (default) -->
<SankeyLabelSettings Padding="8"></SankeyLabelSettings>

<!-- Loose spacing -->
<SankeyLabelSettings Padding="12"></SankeyLabelSettings>

<!-- Extra spacing -->
<SankeyLabelSettings Padding="16"></SankeyLabelSettings>
```

**Guidelines:**
- **4-6**: Compact diagrams, minimal spacing
- **8-10**: Standard, balanced appearance
- **12-16**: Generous spacing, clarity

---

## Complete Styling Examples

### Example 1: Professional Theme

```razor
<SfSankey Nodes="@Nodes" Links="@Links" BackgroundColor="#f5f5f5">
    <SankeyNodeSettings Width="25" Padding="20" Color="#1565c0"></SankeyNodeSettings>
    <SankeyLabelSettings 
        Visible="true"
        Color="#ffffff"
        FontSize="13px"
        FontFamily="'Segoe UI', sans-serif"
        FontWeight="600"
        FontStyle="normal"
        Padding="10">
    </SankeyLabelSettings>
</SfSankey>
```

### Example 2: High Contrast

```razor
<SfSankey Nodes="@Nodes" Links="@Links" BackgroundColor="#000000">
    <SankeyNodeSettings Width="30" Padding="25" Color="#ffffff"></SankeyNodeSettings>
    <SankeyLabelSettings 
        Visible="true"
        Color="#000000"
        FontSize="14px"
        FontFamily="Arial"
        FontWeight="bold"
        Padding="12">
    </SankeyLabelSettings>
</SfSankey>
```

### Example 3: Minimalist

```razor
<SfSankey Nodes="@Nodes" Links="@Links" BackgroundColor="#ffffff">
    <SankeyNodeSettings Width="20" Padding="15" Color="#757575"></SankeyNodeSettings>
    <SankeyLabelSettings 
        Visible="true"
        Color="#212121"
        FontSize="12px"
        FontFamily="'Roboto', sans-serif"
        FontWeight="400"
        FontStyle="normal"
        Padding="8">
    </SankeyLabelSettings>
</SfSankey>
```

### Example 4: Bold Emphasis

```razor
<SfSankey Nodes="@Nodes" Links="@Links">
    <SankeyNodeSettings Width="35" Padding="30" Color="#d32f2f"></SankeyNodeSettings>
    <SankeyLabelSettings 
        Visible="true"
        Color="#ffffff"
        FontSize="16px"
        FontFamily="'Arial Black', sans-serif"
        FontWeight="900"
        Padding="14">
    </SankeyLabelSettings>
</SfSankey>
```

---

## Label Best Practices

### 1. Ensure Readability

Choose colors with sufficient contrast:

```razor
<!-- ✅ Good - high contrast -->
<SankeyNodeSettings Color="#1a237e"></SankeyNodeSettings>
<SankeyLabelSettings Color="#ffffff"></SankeyLabelSettings>

<!-- ❌ Bad - low contrast -->
<SankeyNodeSettings Color="#e3f2fd"></SankeyNodeSettings>
<SankeyLabelSettings Color="#ffffff"></SankeyLabelSettings>
```

### 2. Keep Text Concise

Use short, descriptive labels:

```csharp
// ✅ Good - concise
new SankeyDataLabel() { Text = "Solar" }
new SankeyDataLabel() { Text = "Residential" }

// ❌ Too verbose
new SankeyDataLabel() { Text = "Solar Energy Production Facilities" }
new SankeyDataLabel() { Text = "Residential Consumption and Usage" }
```

### 3. Consistent Formatting

Use consistent label formatting across all nodes:

```csharp
// ✅ Good - consistent style
Nodes = new List<SankeyDataNode>()
{
    new SankeyDataNode() { Id = "A", Label = new SankeyDataLabel() { Text = "Source A" } },
    new SankeyDataNode() { Id = "B", Label = new SankeyDataLabel() { Text = "Source B" } },
    new SankeyDataNode() { Id = "C", Label = new SankeyDataLabel() { Text = "Source C" } }
};

// ❌ Bad - inconsistent
Nodes = new List<SankeyDataNode>()
{
    new SankeyDataNode() { Id = "A", Label = new SankeyDataLabel() { Text = "SOURCE A" } },
    new SankeyDataNode() { Id = "B", Label = new SankeyDataLabel() { Text = "source b" } },
    new SankeyDataNode() { Id = "C", Label = new SankeyDataLabel() { Text = "Source-C" } }
};
```

### 4. Font Size Hierarchy

Maintain readable font sizes relative to diagram size:

```razor
<!-- For large diagrams (>800px height) -->
<SankeyLabelSettings FontSize="14px"></SankeyLabelSettings>

<!-- For medium diagrams (400-800px height) -->
<SankeyLabelSettings FontSize="12px"></SankeyLabelSettings>

<!-- For small diagrams (<400px height) -->
<SankeyLabelSettings FontSize="10px"></SankeyLabelSettings>
```

### 5. Accessibility

Ensure labels are accessible:

- **Color contrast**: WCAG AA minimum ratio 4.5:1
- **Font size**: Minimum 12px for readability
- **Font weight**: Avoid extremely thin weights (<300)

---

## Dynamic Label Updates

Update labels dynamically:

```csharp
private void UpdateLabel(string nodeId, string newText)
{
    var node = Nodes.FirstOrDefault(n => n.Id == nodeId);
    if (node != null)
    {
        if (node.Label == null)
            node.Label = new SankeyDataLabel();
        
        node.Label.Text = newText;
        StateHasChanged();
    }
}

private void ToggleLabelVisibility()
{
    labelVisible = !labelVisible;
    StateHasChanged();
}
```

```razor
<button @onclick="() => UpdateLabel(\"Solar\", \"Solar Energy\")">Update Label</button>
<button @onclick="ToggleLabelVisibility">Toggle Labels</button>

<SfSankey Nodes="@Nodes" Links="@Links">
    <SankeyLabelSettings Visible="@labelVisible"></SankeyLabelSettings>
</SfSankey>

@code {
    private bool labelVisible = true;
}
```

---

## Troubleshooting

### Labels Not Showing

**Possible causes:**
1. `Visible` is set to false
2. Label text is null or empty
3. Label color matches node/background color
4. Font size is too small

**Solutions:**
```razor
<!-- Ensure visibility -->
<SankeyLabelSettings Visible="true"></SankeyLabelSettings>

<!-- Set contrasting color -->
<SankeyLabelSettings Color="#ffffff"></SankeyLabelSettings>

<!-- Use readable size -->
<SankeyLabelSettings FontSize="12px"></SankeyLabelSettings>
```

### Labels Overlapping

**Possible causes:**
1. Insufficient node padding
2. Labels too long
3. Font size too large
4. Too many nodes in small space

**Solutions:**
```razor
<!-- Increase node padding -->
<SankeyNodeSettings Padding="25"></SankeyNodeSettings>

<!-- Reduce font size -->
<SankeyLabelSettings FontSize="10px"></SankeyLabelSettings>

<!-- Use shorter labels -->
new SankeyDataLabel() { Text = "Solar" }  // Instead of "Solar Energy Production"
```

### Labels Hard to Read

**Possible causes:**
1. Poor color contrast
2. Font size too small
3. Thin font weight

**Solutions:**
```razor
<!-- High contrast colors -->
<SankeyNodeSettings Color="#000000"></SankeyNodeSettings>
<SankeyLabelSettings Color="#ffffff"></SankeyLabelSettings>

<!-- Larger font -->
<SankeyLabelSettings FontSize="14px"></SankeyLabelSettings>

<!-- Bolder weight -->
<SankeyLabelSettings FontWeight="600"></SankeyLabelSettings>
```

---

## Complete Working Example

```razor
@page "/labels-complete"
@using Syncfusion.Blazor.Sankey

<h3>Energy Flow with Styled Labels</h3>

<SfSankey Nodes="@Nodes" Links="@Links" Width="100%" Height="600px" BackgroundColor="#eceff1">
    <SankeyNodeSettings Width="30" Padding="25" Color="#37474f"></SankeyNodeSettings>
    <SankeyLinkSettings Color="#78909c" Opacity="0.4"></SankeyLinkSettings>
    <SankeyLabelSettings 
        Visible="true"
        Color="#ffffff"
        FontSize="14px"
        FontFamily="'Segoe UI', Arial, sans-serif"
        FontWeight="600"
        FontStyle="normal"
        Padding="10">
    </SankeyLabelSettings>
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
            new SankeyDataNode() { Id = "Hydro", Label = new SankeyDataLabel() { Text = "Hydro" } },
            new SankeyDataNode() { Id = "Grid", Label = new SankeyDataLabel() { Text = "Power Grid" } },
            new SankeyDataNode() { Id = "Residential", Label = new SankeyDataLabel() { Text = "Residential" } },
            new SankeyDataNode() { Id = "Commercial", Label = new SankeyDataLabel() { Text = "Commercial" } },
            new SankeyDataNode() { Id = "Industrial", Label = new SankeyDataLabel() { Text = "Industrial" } }
        };

        Links = new List<SankeyDataLink>()
        {
            new SankeyDataLink() { SourceId = "Solar", TargetId = "Grid", Value = 150 },
            new SankeyDataLink() { SourceId = "Wind", TargetId = "Grid", Value = 200 },
            new SankeyDataLink() { SourceId = "Hydro", TargetId = "Grid", Value = 100 },
            new SankeyDataLink() { SourceId = "Grid", TargetId = "Residential", Value = 160 },
            new SankeyDataLink() { SourceId = "Grid", TargetId = "Commercial", Value = 140 },
            new SankeyDataLink() { SourceId = "Grid", TargetId = "Industrial", Value = 150 }
        };
    }
}
```

---

## Summary

Labels enhance Sankey diagrams by:
- Providing textual identification for nodes
- Improving readability and comprehension
- Supporting custom typography and styling
- Enabling theme consistency

**Key Points:**
- Define label text via `SankeyDataLabel` in nodes
- Style globally with `SankeyLabelSettings`
- Ensure high contrast for readability
- Keep labels concise and consistent
- Use appropriate font sizes for diagram scale
- Toggle visibility for different presentation needs

Effective label configuration creates clear, professional Sankey diagrams that communicate flows and relationships with precision.
