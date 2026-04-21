# Style and Appearance in Blazor Toolbar Component

## Table of Contents
- [Overview](#overview)
- [Customizing the Toolbar Container](#customizing-the-toolbar-container)
- [Customizing Toolbar Items](#customizing-toolbar-items)
- [Customizing Toolbar Buttons](#customizing-toolbar-buttons)
- [Customizing Item Icons](#customizing-item-icons)
- [Customizing Hover State](#customizing-hover-state)
- [Customizing Selected/Focus State](#customizing-selected-focus-state)
- [Using Built-in Themes](#using-built-in-themes)
- [Right-to-Left (RTL) Support](#right-to-left-rtl-support)
- [Complete Styling Examples](#complete-styling-examples)
- [CSS Class Reference](#css-class-reference)
- [Best Practices](#best-practices)

## Overview

The Toolbar component provides comprehensive styling options through CSS customization. You can modify the toolbar container, individual items, buttons, icons, and interaction states to match your application's design.

## Customizing the Toolbar Container

Target the `.e-toolbar` class to customize the toolbar container itself.

### Border and Background

```css
.e-toolbar {
    border: 5px solid #4caf50;
    background-color: #f5f5f5;
    border-radius: 8px;
}
```

### Shadow and Elevation

```css
.e-toolbar {
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
    background-color: #ffffff;
}
```

### Padding and Spacing

```css
.e-toolbar {
    padding: 8px 16px;
    margin: 16px 0;
}
```

### Complete Container Example

```razor
<SfToolbar CssClass="custom-toolbar">
    <ToolbarItems>
        <ToolbarItem Text="Cut"></ToolbarItem>
        <ToolbarItem Text="Copy"></ToolbarItem>
        <ToolbarItem Text="Paste"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>

<style>
    .custom-toolbar.e-toolbar {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        border: none;
        border-radius: 12px;
        padding: 12px 20px;
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    }
</style>
```

## Customizing Toolbar Items

Target the `.e-toolbar-item` class to style individual item containers.

### Basic Item Styling

```css
.e-toolbar .e-toolbar-item {
    background: #e3f2fd;
    border: 1px solid #2196f3;
    margin: 0 4px;
    border-radius: 4px;
}
```

### Item Spacing

```css
.e-toolbar .e-toolbar-item {
    margin-right: 8px;
    padding: 4px;
}

.e-toolbar .e-toolbar-item:last-child {
    margin-right: 0;
}
```

### Item-Specific Styling with CssClass

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Primary" CssClass="primary-item"></ToolbarItem>
        <ToolbarItem Text="Secondary" CssClass="secondary-item"></ToolbarItem>
        <ToolbarItem Text="Danger" CssClass="danger-item"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>

<style>
    .primary-item .e-tbar-btn {
        background-color: #2196f3;
        color: #ffffff;
    }
    
    .secondary-item .e-tbar-btn {
        background-color: #9e9e9e;
        color: #ffffff;
    }
    
    .danger-item .e-tbar-btn {
        background-color: #f44336;
        color: #ffffff;
    }
</style>
```

## Customizing Toolbar Buttons

Target the `.e-tbar-btn` class to style the buttons within toolbar items.

### Button Background and Text

```css
.e-toolbar .e-tbar-btn {
    background: #2196f3;
    color: #ffffff;
    border-radius: 6px;
    padding: 8px 16px;
    font-weight: 500;
}
```

### Button Text Styling

```css
.e-toolbar .e-tbar-btn .e-tbar-btn-text {
    font-size: 14px;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 0.5px;
}
```

### Button with Icons

```css
.e-toolbar .e-tbar-btn {
    display: flex;
    align-items: center;
    gap: 8px;
}

.e-toolbar .e-tbar-btn .e-icons {
    font-size: 18px;
}
```

## Customizing Item Icons

Target the `.e-icons` class to customize icons within toolbar items.

### Icon Size and Color

```css
.e-toolbar .e-tbar-btn .e-icons {
    font-size: 20px;
    color: #1976d2;
}
```

### Icon Background

```css
.e-toolbar .e-tbar-btn .e-icons {
    background: #e3f2fd;
    color: #1976d2;
    padding: 8px;
    border-radius: 50%;
}
```

### Icon-Only Buttons

```css
/* Style icon-only buttons */
.e-toolbar .e-tbar-btn:not(:has(.e-tbar-btn-text)) {
    padding: 10px;
    min-width: 44px;
    min-height: 44px;
    display: flex;
    align-items: center;
    justify-content: center;
}

.e-toolbar .e-tbar-btn:not(:has(.e-tbar-btn-text)) .e-icons {
    font-size: 22px;
}
```

## Customizing Hover State

Style the hover state to provide visual feedback when users hover over items.

### Basic Hover

```css
.e-toolbar .e-tbar-btn:hover {
    background: #1976d2;
    color: #ffffff;
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    transition: all 0.2s ease;
}
```

### Hover with Icon Change

```css
.e-toolbar .e-tbar-btn {
    transition: background-color 0.3s ease;
}

.e-toolbar .e-tbar-btn:hover {
    background-color: #e3f2fd;
}

.e-toolbar .e-tbar-btn:hover .e-icons {
    color: #2196f3;
}
```

### Smooth Transitions

```css
.e-toolbar .e-tbar-btn {
    transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
}

.e-toolbar .e-tbar-btn:hover {
    background-color: #f5f5f5;
    border-radius: 8px;
}
```

## Customizing Selected/Focus State

Style focused and active items for keyboard navigation and selection feedback.

### Focus State

```css
.e-toolbar .e-tbar-btn:focus {
    background: #e3f2fd;
    border: 2px solid #2196f3;
    outline: none;
}
```

### Focus with Outline

```css
.e-toolbar .e-tbar-btn:focus {
    outline: 3px solid #ffeb3b;
    outline-offset: 2px;
    background: #fff3e0;
}
```

### Active/Pressed State

```css
.e-toolbar .e-tbar-btn:active {
    background: #1565c0;
    transform: scale(0.95);
}
```

### Selected State (for toggle buttons)

```css
.e-toolbar .e-tbar-btn.e-active {
    background: #2196f3;
    color: #ffffff;
    font-weight: 600;
}
```

## Using Built-in Themes

Syncfusion provides multiple built-in themes with consistent styling.

### Available Themes

```html
<!-- Bootstrap 5 theme (recommended) -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Material theme -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- Fabric (Fluent) theme -->
<link href="_content/Syncfusion.Blazor.Themes/fabric.css" rel="stylesheet" />

<!-- Tailwind CSS theme -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />

<!-- Fluent theme -->
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />

<!-- High Contrast theme -->
<link href="_content/Syncfusion.Blazor.Themes/highcontrast.css" rel="stylesheet" />
```

### Theme Variants

Many themes have light and dark variants:

```html
<!-- Bootstrap 5 Dark -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5-dark.css" rel="stylesheet" />

<!-- Material Dark -->
<link href="_content/Syncfusion.Blazor.Themes/material-dark.css" rel="stylesheet" />

<!-- Tailwind Dark -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind-dark.css" rel="stylesheet" />
```

### Customizing Built-in Themes

Override specific styles while keeping the theme base:

```css
/* Override bootstrap5 theme colors */
.e-toolbar .e-tbar-btn {
    background-color: #6f42c1;  /* Purple instead of default blue */
}

.e-toolbar .e-tbar-btn:hover {
    background-color: #5a32a3;
}
```

## Right-to-Left (RTL) Support

Enable RTL support for languages like Arabic, Hebrew, or Urdu using the `EnableRtl` property. This adjusts the toolbar layout and behavior for right-to-left reading direction.

### Implementation

```razor
<SfToolbar EnableRtl="true">
    <ToolbarItems>
        <ToolbarItem Text="قص" TooltipText="قص"></ToolbarItem>
        <ToolbarItem Text="نسخ" TooltipText="نسخ"></ToolbarItem>
        <ToolbarItem Text="لصق" TooltipText="لصق"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Text="عريض" TooltipText="عريض"></ToolbarItem>
        <ToolbarItem Text="مائل" TooltipText="مائل"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

### RTL Behavior

When `EnableRtl="true"`:
- Toolbar items flow right-to-left
- Icons and text positions adjust automatically
- Scroll arrows reverse (left arrow becomes right)
- Popup opens from right side
- Arrow key navigation reverses (← goes forward, → goes back)
- Focus indicators adapt to RTL layout

## Complete Styling Examples

### Example 1: Modern Gradient Toolbar

```razor
<SfToolbar CssClass="gradient-toolbar">
    <ToolbarItems>
        <ToolbarItem Text="Home" PrefixIcon="e-icons e-home"></ToolbarItem>
        <ToolbarItem Text="Profile" PrefixIcon="e-icons e-user"></ToolbarItem>
        <ToolbarItem Text="Settings" PrefixIcon="e-icons e-settings"></ToolbarItem>
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        <ToolbarItem Text="Logout" PrefixIcon="e-icons e-logout"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>

<style>
    .gradient-toolbar.e-toolbar {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        border: none;
        border-radius: 12px;
        padding: 12px 20px;
        box-shadow: 0 8px 16px rgba(102, 126, 234, 0.3);
    }
    
    .gradient-toolbar .e-tbar-btn {
        background: rgba(255, 255, 255, 0.1);
        color: #ffffff;
        border-radius: 8px;
        padding: 10px 16px;
        transition: all 0.3s ease;
        border: 1px solid rgba(255, 255, 255, 0.2);
    }
    
    .gradient-toolbar .e-tbar-btn:hover {
        background: rgba(255, 255, 255, 0.2);
        transform: translateY(-2px);
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }
    
    .gradient-toolbar .e-tbar-btn .e-icons {
        color: #ffffff;
        font-size: 18px;
    }
</style>
```

### Example 2: Minimal Flat Design

```razor
<SfToolbar CssClass="flat-toolbar">
    <ToolbarItems>
        <ToolbarItem Text="Cut" PrefixIcon="e-icons e-cut"></ToolbarItem>
        <ToolbarItem Text="Copy" PrefixIcon="e-icons e-copy"></ToolbarItem>
        <ToolbarItem Text="Paste" PrefixIcon="e-icons e-paste"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Text="Bold" PrefixIcon="e-icons e-bold"></ToolbarItem>
        <ToolbarItem Text="Italic" PrefixIcon="e-icons e-italic"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>

<style>
    .flat-toolbar.e-toolbar {
        background: #ffffff;
        border: none;
        border-bottom: 1px solid #e0e0e0;
        padding: 8px 16px;
    }
    
    .flat-toolbar .e-tbar-btn {
        background: transparent;
        color: #424242;
        border: none;
        border-radius: 4px;
        padding: 8px 12px;
        transition: background-color 0.2s ease;
    }
    
    .flat-toolbar .e-tbar-btn:hover {
        background: #f5f5f5;
    }
    
    .flat-toolbar .e-tbar-btn:focus {
        background: #eeeeee;
        outline: 2px solid #2196f3;
        outline-offset: 2px;
    }
    
    .flat-toolbar .e-tbar-btn .e-icons {
        color: #757575;
        font-size: 16px;
    }
</style>
```

### Example 3: Dark Mode Toolbar

```razor
<SfToolbar CssClass="dark-toolbar">
    <ToolbarItems>
        <ToolbarItem Text="Dashboard" PrefixIcon="e-icons e-dashboard"></ToolbarItem>
        <ToolbarItem Text="Analytics" PrefixIcon="e-icons e-chart"></ToolbarItem>
        <ToolbarItem Text="Reports" PrefixIcon="e-icons e-file"></ToolbarItem>
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-bell" TooltipText="Notifications"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-user" TooltipText="Profile"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>

<style>
    .dark-toolbar.e-toolbar {
        background: #1e1e1e;
        border: 1px solid #333333;
        padding: 12px 20px;
    }
    
    .dark-toolbar .e-tbar-btn {
        background: transparent;
        color: #e0e0e0;
        border-radius: 6px;
        padding: 10px 16px;
        transition: all 0.2s ease;
    }
    
    .dark-toolbar .e-tbar-btn:hover {
        background: #2d2d2d;
        color: #ffffff;
    }
    
    .dark-toolbar .e-tbar-btn:focus {
        background: #3d3d3d;
        outline: 2px solid #64b5f6;
        outline-offset: 2px;
    }
    
    .dark-toolbar .e-tbar-btn .e-icons {
        color: #b0b0b0;
    }
    
    .dark-toolbar .e-tbar-btn:hover .e-icons {
        color: #ffffff;
    }
    
    .dark-toolbar .e-separator {
        background: #424242;
    }
</style>
```

### Example 4: Colorful Icon Toolbar

```razor
<SfToolbar CssClass="icon-toolbar">
    <ToolbarItems>
        <ToolbarItem PrefixIcon="e-icons e-cut" TooltipText="Cut" CssClass="cut-item"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-copy" TooltipText="Copy" CssClass="copy-item"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-paste" TooltipText="Paste" CssClass="paste-item"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-bold" TooltipText="Bold" CssClass="bold-item"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-italic" TooltipText="Italic" CssClass="italic-item"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>

<style>
    .icon-toolbar.e-toolbar {
        background: #fafafa;
        border: 1px solid #e0e0e0;
        border-radius: 8px;
        padding: 12px;
    }
    
    .icon-toolbar .e-tbar-btn {
        width: 44px;
        height: 44px;
        display: flex;
        align-items: center;
        justify-content: center;
        border-radius: 8px;
        transition: all 0.2s ease;
        background: transparent;
    }
    
    .icon-toolbar .e-tbar-btn:hover {
        transform: scale(1.1);
    }
    
    .cut-item .e-tbar-btn { background: #ffebee; }
    .cut-item .e-icons { color: #d32f2f; }
    
    .copy-item .e-tbar-btn { background: #e3f2fd; }
    .copy-item .e-icons { color: #1976d2; }
    
    .paste-item .e-tbar-btn { background: #e8f5e9; }
    .paste-item .e-icons { color: #388e3c; }
    
    .bold-item .e-tbar-btn { background: #fff3e0; }
    .bold-item .e-icons { color: #f57c00; }
    
    .italic-item .e-tbar-btn { background: #f3e5f5; }
    .italic-item .e-icons { color: #7b1fa2; }
</style>
```

## CSS Class Reference

### Primary Classes

| Class | Target | Description |
|-------|--------|-------------|
| `.e-toolbar` | Container | Main toolbar wrapper |
| `.e-toolbar-items` | Items container | Wrapper for all toolbar items |
| `.e-toolbar-item` | Individual item | Single toolbar item container |
| `.e-tbar-btn` | Button | Button element within item |
| `.e-tbar-btn-text` | Button text | Text content of button |
| `.e-icons` | Icon | Icon element |
| `.e-separator` | Separator | Vertical separator line |

### State Classes

| Class | State | Description |
|-------|-------|-------------|
| `.e-tbar-btn:hover` | Hover | Mouse hover state |
| `.e-tbar-btn:focus` | Focus | Keyboard focus state |
| `.e-tbar-btn:active` | Active | Button pressed state |
| `.e-tbar-btn.e-active` | Selected | Toggle button active state |
| `.e-toolbar-item.e-disabled` | Disabled | Disabled item |

### Overflow Classes

| Class | Component | Description |
|-------|-----------|-------------|
| `.e-toolbar-pop` | Popup | Overflow popup container |
| `.e-toolbar-overflow` | Overflow icon | Dropdown icon button |
| `.e-hor-nav` | Navigation | Scroll navigation arrows |

## Best Practices

### Do:

✅ Use CSS custom properties for consistent theming:

```css
:root {
    --toolbar-bg: #ffffff;
    --toolbar-text: #212121;
    --toolbar-hover: #f5f5f5;
    --toolbar-active: #2196f3;
}

.e-toolbar {
    background: var(--toolbar-bg);
    color: var(--toolbar-text);
}

.e-tbar-btn:hover {
    background: var(--toolbar-hover);
}
```

✅ Maintain accessibility with sufficient contrast:

```css
/* Good contrast ratios */
.e-toolbar .e-tbar-btn {
    background: #2196f3;  /* Blue */
    color: #ffffff;        /* White */
    /* Contrast ratio: 4.5:1+ */
}
```

✅ Use transitions for smooth interactions:

```css
.e-tbar-btn {
    transition: background-color 0.2s ease, transform 0.2s ease;
}
```

✅ Test styling with different themes and screen sizes

### Don't:

❌ Remove focus indicators (accessibility issue):

```css
/* Bad: Removes keyboard focus indicator */
.e-tbar-btn:focus {
    outline: none;  /* Don't do this without alternative */
}
```

❌ Use very low contrast colors:

```css
/* Bad: Low contrast */
.e-toolbar {
    background: #f0f0f0;
    color: #e0e0e0;  /* Too similar, hard to read */
}
```

❌ Override all theme styles (breaks consistency):

```css
/* Bad: Overriding everything */
.e-toolbar * {
    all: unset;  /* Breaks component functionality */
}
```

❌ Use inline styles instead of CSS classes:

```razor
<!-- Bad: Inline styles -->
<ToolbarItem Text="Save" HtmlAttributes="@(new Dictionary<string, object> { { "style", "color: red" } })"></ToolbarItem>

<!-- Good: Use CssClass -->
<ToolbarItem Text="Save" CssClass="danger-button"></ToolbarItem>
```

### Performance Tips

- Use CSS classes instead of inline styles
- Minimize use of complex selectors
- Avoid !important unless absolutely necessary
- Use CSS custom properties for easy theme switching
- Test performance with many toolbar items
