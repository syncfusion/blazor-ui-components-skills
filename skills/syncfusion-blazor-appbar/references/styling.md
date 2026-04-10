# Styles and Appearances in Blazor AppBar Component

## Table of Contents
- [Overview](#overview)
- [CSS Classes Reference](#css-classes-reference)
- [CssClass Property](#cssc lass-property)
- [HtmlAttributes](#htmlattributes)
- [Theme Studio Integration](#theme-studio-integration)
- [Custom Styling Examples](#custom-styling-examples)

## Overview

The AppBar component provides multiple ways to customize its appearance to match your application's design. You can modify the AppBar using CSS classes, the `CssClass` property, HTML attributes, or by creating custom themes with Theme Studio.

## CSS Classes Reference

The following CSS classes are available for customizing specific parts of the AppBar:

| CSS Class | Purpose |
|-----------|---------|
| `.e-appbar` | Customizes the AppBar container |
| `.e-appbar.e-prominent` | Customizes the prominent AppBar |
| `.e-appbar.e-dense` | Customizes the dense AppBar |
| `.e-appbar.e-light` | Customizes the light AppBar |
| `.e-appbar.e-dark` | Customizes the dark AppBar |
| `.e-appbar.e-primary` | Customizes the primary AppBar |
| `.e-appbar.e-inherit` | Customizes the inherit AppBar |

**Note:** You can change the prominent AppBar height if larger titles, images, or texts are used.

### Using CSS Classes

Target specific AppBar modes with CSS:

```cshtml
<style>
    /* Customize all AppBars */
    .e-appbar {
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    }
    
    /* Customize prominent AppBar */
    .e-appbar.e-prominent {
        min-height: 300px;
    }
    
    /* Customize dense AppBar */
    .e-appbar.e-dense {
        height: 48px;
    }
    
    /* Customize primary AppBar */
    .e-appbar.e-primary {
        border-bottom: 3px solid #0078d4;
    }
</style>
```

## CssClass Property

The `CssClass` property allows you to apply custom CSS classes to the AppBar for specific styling needs.

**When to use:**
- Applying custom background colors
- Adding custom shadows or borders
- Creating variant styles for different pages
- Overriding default theme colors

**Basic example:**

```cshtml
<SfAppBar ColorMode="AppBarColor.Primary" CssClass="custom-appbar">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-home"></SfButton>
</SfAppBar>

<style>
    .custom-appbar {
        background: #ff0000;
        color: #ffffff;
    }
</style>
```

### Custom Brand Colors

Apply your brand colors to the AppBar:

```cshtml
<SfAppBar ColorMode="AppBarColor.Primary" CssClass="brand-appbar">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
    <span class="brand-name">Company Name</span>
    <AppBarSpacer></AppBarSpacer>
    <SfButton CssClass="e-inherit" Content="Contact"></SfButton>
</SfAppBar>

<style>
    .brand-appbar {
        background: linear-gradient(90deg, #667eea 0%, #764ba2 100%);
        color: #ffffff;
    }
    
    .brand-appbar .brand-name {
        font-weight: 600;
        font-size: 18px;
        margin-left: 12px;
    }
    
    .brand-appbar .e-btn.e-inherit:hover {
        background: rgba(255, 255, 255, 0.15);
    }
</style>
```

### Elevated Shadow Style

Add elevation with box-shadow:

```cshtml
<SfAppBar ColorMode="AppBarColor.Light" CssClass="elevated-appbar">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
    <span>Elevated AppBar</span>
    <AppBarSpacer></AppBarSpacer>
    <SfButton IsPrimary="true" Content="Sign In"></SfButton>
</SfAppBar>

<style>
    .elevated-appbar {
        box-shadow: 0 4px 6px -1px rgba(0, 0, 0, 0.1), 
                    0 2px 4px -1px rgba(0, 0, 0, 0.06);
    }
</style>
```

### Bordered Style

Add borders for definition:

```cshtml
<SfAppBar ColorMode="AppBarColor.Light" CssClass="bordered-appbar">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
    <span>Bordered AppBar</span>
    <AppBarSpacer></AppBarSpacer>
    <SfButton IsPrimary="true" Content="Login"></SfButton>
</SfAppBar>

<style>
    .bordered-appbar {
        border-bottom: 2px solid #e5e7eb;
    }
</style>
```

## HtmlAttributes

The `HtmlAttributes` parameter allows you to add additional HTML attributes to the AppBar element, useful for accessibility, SEO, or custom data attributes.

**When to use:**
- Adding ARIA attributes for accessibility
- Adding custom data attributes
- Setting specific HTML properties
- Adding tracking attributes

**Adding aria-label:**

```cshtml
<SfAppBar ColorMode="AppBarColor.Primary" aria-label="Main application navigation">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-home"></SfButton>
</SfAppBar>
```

**Using @attributes directive:**

```cshtml
@code {
    Dictionary<string, object> AppBarAttributes = new Dictionary<string, object>
    {
        { "role", "navigation" },
        { "aria-label", "Primary navigation bar" },
        { "data-testid", "main-appbar" }
    };
}

<SfAppBar ColorMode="AppBarColor.Primary" @attributes="AppBarAttributes">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
</SfAppBar>
```

## Theme Studio Integration

Theme Studio is Syncfusion's theme customization tool that allows you to create custom themes for all Syncfusion components, including the AppBar.

**When to use:**
- Creating consistent themes across all Syncfusion components
- Customizing primary, secondary, and accent colors
- Adjusting typography and spacing
- Exporting theme as CSS for production use

**Using Theme Studio:**

1. Visit the [Syncfusion Blazor Theme Studio](https://blazor.syncfusion.com/themestudio/?theme=material)
2. Select a base theme (Material, Bootstrap, Fabric, etc.)
3. Customize colors, typography, and component styles
4. Preview changes across different components
5. Download the generated CSS file
6. Reference the custom theme in your application

**Applying custom theme:**

```html
<!-- Replace default theme with custom theme -->
<link href="path/to/custom-theme.css" rel="stylesheet" />
```

### CSS Variables

Modern Syncfusion themes use CSS custom properties (variables) that you can override:

```cshtml
<SfAppBar ColorMode="AppBarColor.Primary" CssClass="custom-vars">
    <SfButton CssClass="e-inherit" Content="Home"></SfButton>
</SfAppBar>

<style>
    .custom-vars {
        --primary-color: #00796b;
        --text-color: #ffffff;
        background: var(--primary-color);
        color: var(--text-color);
    }
</style>
```

## Custom Styling Examples

### Glassmorphism Style

Create a modern glassmorphism effect:

```cshtml
<SfAppBar ColorMode="AppBarColor.Light" CssClass="glass-appbar">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
    <span class="title">Glass AppBar</span>
    <AppBarSpacer></AppBarSpacer>
    <SfButton CssClass="e-inherit" Content="Login"></SfButton>
</SfAppBar>

<style>
    .glass-appbar {
        background: rgba(255, 255, 255, 0.7);
        backdrop-filter: blur(10px);
        border-bottom: 1px solid rgba(255, 255, 255, 0.3);
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }
    
    .glass-appbar .title {
        font-weight: 600;
        color: #1f2937;
    }
</style>
```

### Animated Gradient

Add an animated gradient background:

```cshtml
<SfAppBar ColorMode="AppBarColor.Primary" CssClass="gradient-appbar">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
    <span class="title">Animated Gradient</span>
    <AppBarSpacer></AppBarSpacer>
    <SfButton CssClass="e-inherit" Content="Explore"></SfButton>
</SfAppBar>

<style>
    .gradient-appbar {
        background: linear-gradient(270deg, #667eea, #764ba2, #f093fb, #4facfe);
        background-size: 800% 800%;
        animation: gradientShift 15s ease infinite;
    }
    
    @keyframes gradientShift {
        0% { background-position: 0% 50%; }
        50% { background-position: 100% 50%; }
        100% { background-position: 0% 50%; }
    }
    
    .gradient-appbar .title {
        font-weight: 600;
        color: #ffffff;
    }
</style>
```

### Neumorphism Style

Create a soft neumorphism effect:

```cshtml
<SfAppBar ColorMode="AppBarColor.Light" CssClass="neomorph-appbar">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
    <span class="title">Neumorphic AppBar</span>
    <AppBarSpacer></AppBarSpacer>
    <SfButton CssClass="e-inherit" Content="Settings"></SfButton>
</SfAppBar>

<style>
    .neomorph-appbar {
        background: #e0e5ec;
        box-shadow: 9px 9px 16px rgba(163, 177, 198, 0.6),
                   -9px -9px 16px rgba(255, 255, 255, 0.5);
    }
    
    .neomorph-appbar .e-btn.e-inherit {
        background: #e0e5ec;
        box-shadow: 5px 5px 10px rgba(163, 177, 198, 0.6),
                   -5px -5px 10px rgba(255, 255, 255, 0.5);
        border: none;
    }
    
    .neomorph-appbar .e-btn.e-inherit:active {
        box-shadow: inset 5px 5px 10px rgba(163, 177, 198, 0.6),
                   inset -5px -5px 10px rgba(255, 255, 255, 0.5);
    }
</style>
```

### Custom Height for Prominent

Adjust prominent AppBar height for custom content:

```cshtml
<SfAppBar ColorMode="AppBarColor.Primary" 
          Mode="AppBarMode.Prominent" 
          CssClass="custom-prominent">
    <div class="prominent-content">
        <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
        <h1 class="hero-title">Welcome to Our Application</h1>
        <p class="hero-subtitle">Build amazing experiences</p>
    </div>
</SfAppBar>

<style>
    .custom-prominent {
        height: 250px;
        display: flex;
        align-items: center;
    }
    
    .prominent-content {
        display: flex;
        flex-direction: column;
        align-items: flex-start;
        width: 100%;
        padding: 0 20px;
    }
    
    .hero-title {
        font-size: 42px;
        font-weight: 700;
        margin: 20px 0 10px 0;
        color: #ffffff;
    }
    
    .hero-subtitle {
        font-size: 18px;
        color: rgba(255, 255, 255, 0.9);
        margin: 0;
    }
</style>
```

## Best Practices

**Styling approach:**
- Use `CssClass` property for component-specific customization
- Use global CSS classes for consistent styling across multiple AppBars
- Leverage Theme Studio for comprehensive theme customization
- Test custom styles with different AppBar modes (Regular, Prominent, Dense)

**Performance:**
- Avoid complex animations on sticky AppBars for smooth scrolling
- Use CSS transforms instead of positioning for better performance
- Minimize the use of heavy box-shadows on large AppBars

**Maintainability:**
- Keep custom styles in separate stylesheet files
- Use CSS variables for easy theme switching
- Document custom CSS classes for team collaboration
- Test customizations across different browsers
