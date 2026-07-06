---
name: syncfusion-blazor-toolbar
description: Implement Syncfusion Blazor Toolbar (SfToolbar) component for creating interactive command bars and toolbars. Use this when working with toolbars, command bars, or action bars with buttons and icons. This skill covers responsive toolbars with overflow handling, item configuration and alignment, keyboard navigation, accessibility features, and dynamic item management.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
---

# Syncfusion Blazor Toolbar Component

The Syncfusion Blazor Toolbar is a versatile component for creating interactive command bars with buttons, separators, and custom controls. It provides built-in responsive behavior and flexible item configuration.

## When to Use This Skill

Use this skill when you need to:

- Create toolbars with buttons, icons, and custom controls
- Build text editors with formatting toolbars (bold, italic, alignment, etc.)
- Implement command bars or action bars in applications
- Add responsive toolbars that adapt to different screen sizes
- Configure item overflow behavior (scrollable, popup, multirow)
- Create navigation toolbars with alignment and spacing
- Add toggle buttons or stateful toolbar items
- Implement dynamic toolbars with add/remove item functionality
- Customize toolbar appearance and styling
- Enable RTL support for right-to-left languages

## Component Overview

The Blazor Toolbar component features:

- **Item Types**: Buttons, separators, spacers, and custom templates
- **Responsive Modes**: Scrollable, popup, multirow, and extended overflow handling
- **Alignment**: Left, center, right alignment with spacer support
- **Customization**: Icons, tooltips, styling, templates, and RTL support
- **Dynamic Management**: Add, remove, enable, disable, show, hide items programmatically

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)

When the user needs to:
- Install and configure the Toolbar component
- Add NuGet packages and register services
- Create a basic toolbar with items
- Understand the fundamental ToolbarItems structure
- Set up CSS themes and script references

### Item Configuration and Properties
📄 **Read:** [references/item-configuration.md](references/item-configuration.md)

When the user needs to:
- Configure item properties (Text, PrefixIcon, SuffixIcon, Width, etc.)
- Set item alignment (Left, Center, Right)
- Define item types (Button, Separator, Input, Spacer)
- Control item visibility and enabled state
- Add tooltips to toolbar items
- Use custom CSS classes or HTML attributes
- Configure overflow behavior for individual items
- Integrate other Syncfusion components (DropDownList, NumericTextBox, etc.)
- Understand all 18 item properties in detail

### Practical How-To Guides
📄 **Read:** [references/how-to-guides.md](references/how-to-guides.md)

When the user needs to:
- Add or remove toolbar items dynamically
- Enable or disable toolbar items programmatically
- Show or hide toolbar items based on conditions
- Create toggle buttons with state management
- Customize items with HtmlAttributes and CssClass
- Set item-wise custom templates
- Add tooltips using the SfTooltip component
- Enable tab key navigation with TabIndex
- Customize the scrolling distance

### Responsive Behavior and Overflow Modes
📄 **Read:** [references/responsive-mode.md](references/responsive-mode.md)

When the user needs to:
- Configure toolbar overflow behavior (Scrollable, Popup, MultiRow, Extended)
- Set item priority for overflow handling
- Control text display in toolbar vs popup (ShowTextOn)
- Implement scrollable toolbars with navigation arrows
- Create popup toolbars with dropdown overflow
- Configure multirow or extended overflow layouts
- Handle touch gestures for scrolling

### Alignment and Spacing with Spacer
📄 **Read:** [references/alignment-spacer.md](references/alignment-spacer.md)

When the user needs to:
- Use Spacer to align items left, center, and right
- Create left and right alignment patterns
- Implement right-only alignment
- Understand when to use Spacer vs Align property
- Apply flexible spacing between toolbar items

### Styling and Customization
📄 **Read:** [references/styling.md](references/styling.md)

When the user needs to:
- Customize toolbar container styling
- Style toolbar items and buttons
- Modify icon appearance
- Customize hover and focus states
- Apply custom themes
- Override default CSS classes
- Enable RTL (right-to-left) support for Arabic, Hebrew, or Urdu languages

## Quick Start Example

Basic toolbar with common formatting buttons:

```razor
@using Syncfusion.Blazor.Navigations

<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Cut" PrefixIcon="e-icons e-cut"></ToolbarItem>
        <ToolbarItem Text="Copy" PrefixIcon="e-icons e-copy"></ToolbarItem>
        <ToolbarItem Text="Paste" PrefixIcon="e-icons e-paste"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Text="Bold" PrefixIcon="e-icons e-bold"></ToolbarItem>
        <ToolbarItem Text="Italic" PrefixIcon="e-icons e-italic"></ToolbarItem>
        <ToolbarItem Text="Underline" PrefixIcon="e-icons e-underline"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

## Common Patterns

### Pattern 1: Text Editor Toolbar

Formatting toolbar with alignment and spacing:

```razor
<SfToolbar Width="600">
    <ToolbarItems>
        <ToolbarItem Text="Bold" PrefixIcon="e-icons e-bold"></ToolbarItem>
        <ToolbarItem Text="Italic" PrefixIcon="e-icons e-italic"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        <ToolbarItem Text="Left" PrefixIcon="e-icons e-align-left"></ToolbarItem>
        <ToolbarItem Text="Center" PrefixIcon="e-icons e-align-center"></ToolbarItem>
        <ToolbarItem Text="Right" PrefixIcon="e-icons e-align-right"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

### Pattern 2: Responsive Toolbar with Popup

Toolbar that moves overflow items to a popup:

```razor
<SfToolbar Width="400" OverflowMode="OverflowMode.Popup">
    <ToolbarItems>
        <ToolbarItem Text="Cut" PrefixIcon="e-icons e-cut" Overflow="OverflowOption.Show"></ToolbarItem>
        <ToolbarItem Text="Copy" PrefixIcon="e-icons e-copy" Overflow="OverflowOption.Show"></ToolbarItem>
        <ToolbarItem Text="Paste" PrefixIcon="e-icons e-paste"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Text="Bold" PrefixIcon="e-icons e-bold"></ToolbarItem>
        <ToolbarItem Text="Italic" PrefixIcon="e-icons e-italic"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

### Pattern 3: Dynamic Toolbar with Add/Remove

Toolbar with programmatic item management:

```razor
<SfToolbar>
    <ToolbarItems>
        @foreach (var item in ToolbarItems)
        {
            <ToolbarItem Text="@item.Text" PrefixIcon="@item.Icon"></ToolbarItem>
        }
    </ToolbarItems>
</SfToolbar>

@code {
    private List<ToolbarItemData> ToolbarItems = new()
    {
        new() { Text = "Cut", Icon = "e-icons e-cut" },
        new() { Text = "Copy", Icon = "e-icons e-copy" }
    };
    
    private void AddItem()
    {
        ToolbarItems.Add(new() { Text = "Paste", Icon = "e-icons e-paste" });
    }
    
    private void RemoveItem()
    {
        if (ToolbarItems.Count > 0)
            ToolbarItems.RemoveAt(0);
    }
    
    public class ToolbarItemData
    {
        public string Text { get; set; }
        public string Icon { get; set; }
    }
}
```

### Pattern 4: Toolbar with Custom Alignment

Left-aligned, center-aligned, and right-aligned items using Spacer:

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="File"></ToolbarItem>
        <ToolbarItem Text="Edit"></ToolbarItem>
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        <ToolbarItem Text="Help"></ToolbarItem>
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        <ToolbarItem Text="Settings" PrefixIcon="e-icons e-settings"></ToolbarItem>
        <ToolbarItem Text="Profile" PrefixIcon="e-icons e-user"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

## Key Properties Reference

### SfToolbar Component Properties

| Property | Type | Description |
|----------|------|-------------|
| `Width` | string | Sets the toolbar width (e.g., "600px", "100%") |
| `Height` | string | Sets the toolbar height |
| `OverflowMode` | OverflowMode | Defines overflow behavior: Scrollable, Popup, MultiRow, Extended |
| `ScrollStep` | int | Scrolling distance in pixels when using navigation arrows |
| `EnableRtl` | bool | Enables right-to-left layout |
| `CssClass` | string | Custom CSS class for the toolbar container |

### ToolbarItem Properties

| Property | Type | Description |
|----------|------|-------------|
| `Text` | string | Button text content |
| `PrefixIcon` | string | Icon CSS class displayed before text |
| `SuffixIcon` | string | Icon CSS class displayed after text |
| `TooltipText` | string | Tooltip displayed on hover |
| `Type` | ItemType | Button, Separator, Input, or Spacer |
| `Align` | ItemAlign | Left, Center, or Right alignment |
| `Disabled` | bool | Disables the item when true |
| `Visible` | bool | Controls item visibility |
| `Width` | string | Sets item width |
| `CssClass` | string | Custom CSS classes for the item |
| `HtmlAttributes` | Dictionary | Custom HTML attributes (id, style, role, etc.) |
| `Overflow` | OverflowOption | Show (priority), Hide (secondary), or None |
| `ShowAlwaysInPopup` | bool | Always displays item in popup (Popup mode) |
| `ShowTextOn` | DisplayMode | Controls text display: Toolbar, Overflow, or Both |
| `TabIndex` | int | Enables tab key navigation |
| `Template` | RenderFragment | Custom content template |
| `Id` | string | Unique identifier for the item |

## Implementation Guidelines

### When Configuring Items

1. **Use appropriate item types**: Button for actions, Separator for visual dividers, Spacer for alignment, Input for custom controls
2. **Add icons for better UX**: Use PrefixIcon for icon-only or icon-with-text buttons
3. **Provide tooltips**: Set TooltipText for icon-only buttons to improve accessibility
4. **Consider responsive behavior**: Choose appropriate OverflowMode based on target devices
5. **Set overflow priorities**: Use Overflow property to control which items move to popup first

### When Handling Overflow

1. **Scrollable mode** (default): Best for desktop applications with horizontal space
2. **Popup mode**: Ideal for mobile or limited-width containers
3. **MultiRow mode**: Good for toolbars with many related items
4. **Extended mode**: Similar to MultiRow but with different layout behavior

### When Customizing Appearance

1. **Use built-in icons**: Leverage Syncfusion icon library with `e-icons` class
2. **Apply themes consistently**: Use CssClass for custom styling
3. **Consider accessibility**: Maintain sufficient color contrast and focus indicators
4. **Test responsive behavior**: Verify toolbar appearance at different widths

### When Implementing Dynamic Behavior

1. **Use data binding**: Bind ToolbarItems to a collection for dynamic updates
2. **Handle events**: Use OnClick for button actions
3. **Update state conditionally**: Toggle Disabled or Visible properties based on application state
4. **Manage item lifecycle**: Add or remove items from the bound collection to update the toolbar

## Common Use Cases

1. **Rich Text Editors**: Formatting toolbars with bold, italic, alignment, lists
2. **File Managers**: Action bars with upload, download, delete, rename buttons
3. **Data Grids**: Command toolbars for add, edit, delete, export operations
4. **Image Editors**: Tool palettes with drawing, cropping, filter tools
5. **Navigation Menus**: Top-level navigation with menu items and search
6. **Dashboard Controls**: Action buttons for refresh, filter, settings
7. **Form Builders**: Component palettes with draggable controls

## Tips and Best Practices

- **Icon-only buttons**: Always provide TooltipText for accessibility
- **Responsive design**: Test toolbar at various widths to verify overflow behavior
- **Keyboard navigation**: Enable TabIndex for better keyboard accessibility
- **Visual grouping**: Use Separator to create logical groups of related items
- **Performance**: When using many items, consider Popup mode to reduce initial render size
- **Custom templates**: Use Template property for complex controls beyond standard buttons
- **Alignment flexibility**: Prefer Spacer over Align property for more dynamic layouts

## Next Steps

After implementing the basic toolbar:

1. Explore responsive modes to handle different screen sizes
2. Add keyboard navigation with TabIndex for accessibility
3. Customize styling to match your application theme
4. Implement dynamic item management for interactive toolbars
5. Add toggle buttons or custom controls using templates
6. Integrate with other Syncfusion components (DropDownList, ColorPicker, etc.)
