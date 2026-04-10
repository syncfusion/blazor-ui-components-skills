# Size and Color with Blazor AppBar Component

## Table of Contents
- [Overview](#overview)
- [Size Modes](#size-modes)
  - [Regular AppBar](#regular-appbar)
  - [Prominent AppBar](#prominent-appbar)
  - [Dense AppBar](#dense-appbar)
- [Color Modes](#color-modes)
  - [Light AppBar](#light-appbar)
  - [Dark AppBar](#dark-appbar)
  - [Primary AppBar](#primary-appbar)
  - [Inherit AppBar](#inherit-appbar)
- [Combining Size and Color Modes](#combining-size-and-color-modes)

## Overview

The Blazor AppBar provides flexible configuration for both size and color to match different design requirements. Size modes control the height of the AppBar, while color modes control the background and text colors.

## Size Modes

The size of the AppBar can be set using the `Mode` property. The available size modes are:

- **Regular** - Default height for standard applications
- **Prominent** - Longer height for larger titles, images, or hero sections
- **Dense** - Shorter height for compact, space-efficient headers

### Regular AppBar

Regular mode is the default size in which the AppBar is displayed with the standard height.

**When to use:**
- Standard application headers
- Typical navigation bars
- General-purpose toolbars

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfAppBar ColorMode="AppBarColor.Primary">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
    <span class="regular">Regular AppBar</span>
    <AppBarSpacer></AppBarSpacer>
    <SfButton CssClass="e-inherit" Content="FREE TRIAL"></SfButton>
</SfAppBar>

<style>
    .e-btn.e-inherit {
        margin: 0 3px;
    }
</style>
```

### Prominent AppBar

Prominent mode displays the AppBar with a longer height, ideal for larger titles, images, or text. This mode is useful for hero sections or landing pages where you want to make a visual impact.

**When to use:**
- Landing pages with hero sections
- Pages with large branding or imagery
- Marketing or promotional headers
- Applications requiring prominent titles

**Set the mode:**

```cshtml
Mode="AppBarMode.Prominent"
```

**Example with custom styling:**

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfAppBar ColorMode="AppBarColor.Primary" Mode="AppBarMode.Prominent" CssClass="prominent-appbar">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
    <span class="prominent">AppBar Component with Prominent mode</span>
    <AppBarSpacer></AppBarSpacer>
    <SfButton CssClass="e-inherit" Content="FREE TRIAL"></SfButton>
</SfAppBar>

<style>
    .e-appbar .prominent {
        align-self: center;
        white-space: break-spaces;
        text-align: inherit;
        font-size: 35px;
        line-height: 50px;
    }
    
    .e-appbar.prominent-appbar {
        background-image: url("https://blazor.syncfusion.com/demos/_content/blazor_server_common_net8/images/appbar/prominent.png");
        background-size: 100% 400px;
        color: #ffffff;
        background-repeat: no-repeat;
        height: 400px;
    }
    
    .prominent-appbar .e-inherit.e-btn {
        background: transparent;
    }
    
    .prominent-appbar .e-inherit.e-btn:hover,
    .prominent-appbar .e-inherit.e-btn:focus,
    .prominent-appbar .e-inherit.e-btn:active,
    .prominent-appbar .e-inherit.e-btn.e-active {
        background: rgba(255, 255, 255, .08);
    }
</style>
```

**Customization tips:**
- You can adjust the prominent AppBar height if using larger titles, images, or text
- Use `align-self: center` to vertically center prominent content
- Use `white-space: break-spaces` for multi-line text
- Consider background images for visual impact
- Adjust font size and line height for better readability

### Dense AppBar

Dense mode displays the AppBar with a shorter height, ideal for applications where vertical space is limited or when you want a more compact appearance.

**When to use:**
- Data-dense applications or tools
- Applications with limited vertical space
- Secondary toolbars or action bars
- Mobile applications requiring more content space

**Set the mode:**

```cshtml
Mode="AppBarMode.Dense"
```

**Example:**

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfAppBar ColorMode="AppBarColor.Primary" Mode="AppBarMode.Dense">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
    <span class="dense">Dense AppBar</span>
    <AppBarSpacer></AppBarSpacer>
    <SfButton CssClass="e-inherit" Content="FREE TRIAL"></SfButton>
</SfAppBar>

<style>
    .e-btn.e-inherit {
        margin: 0 3px;
    }
</style>
```

## Color Modes

The background and font colors can be set using the `ColorMode` property. The available color modes are:

- **Light** - Light background with corresponding font color (default)
- **Dark** - Dark background with corresponding font color
- **Primary** - Primary theme colors
- **Inherit** - Inherits colors from parent element

### Light AppBar

Light mode is the default color scheme with a light background and dark text. This mode is ideal for applications with a light theme.

**When to use:**
- Applications with light themes
- Professional or corporate applications
- Default styling for most use cases

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfAppBar>
    <a href="https://www.syncfusion.com/blazor-components" target="_blank" rel="noopener" role="link" aria-label="Syncfusion blazor components">
        <div class="syncfusion-logo"></div>
    </a>
    <AppBarSpacer></AppBarSpacer>
    <SfButton IsPrimary="true" Content="FREE TRIAL"></SfButton>
</SfAppBar>

<style>
    .syncfusion-logo {
        background: url(https://cdn.syncfusion.com/blazor/images/demos/syncfusion-logo.svg);
        background-size: contain;
        background-repeat: no-repeat;
        height: 30px;
        width: 150px;
    }
</style>
```

> **Note:** When using Light mode, use `IsPrimary="true"` on buttons instead of `CssClass="e-inherit"` for better contrast.

### Dark AppBar

Dark mode displays the AppBar with a dark background and light text. Set by using `AppBarColor.Dark` for the `ColorMode` property.

**When to use:**
- Applications with dark themes
- Modern or creative applications
- Reducing eye strain in low-light environments
- Gaming or entertainment applications

**Set the color mode:**

```cshtml
ColorMode="AppBarColor.Dark"
```

**Example:**

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfAppBar ColorMode="AppBarColor.Dark">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
    <AppBarSpacer></AppBarSpacer>
    <SfButton CssClass="e-inherit" Content="FREE TRIAL"></SfButton>
</SfAppBar>

<style>
    .e-btn.e-inherit {
        margin: 0 3px;
    }
</style>
```

### Primary AppBar

Primary mode displays the AppBar using the theme's primary colors. Set by using `AppBarColor.Primary` for the `ColorMode` property.

**When to use:**
- Emphasizing branding colors
- Creating visual hierarchy
- Matching application's color scheme
- Drawing attention to navigation

**Set the color mode:**

```cshtml
ColorMode="AppBarColor.Primary"
```

**Example:**

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfAppBar ColorMode="AppBarColor.Primary">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
    <AppBarSpacer></AppBarSpacer>
    <SfButton CssClass="e-inherit" Content="FREE TRIAL"></SfButton>
</SfAppBar>

<style>
    .e-btn.e-inherit {
        margin: 0 3px;
    }
</style>
```

> **Note:** The primary color depends on the theme you're using (Bootstrap, Material, Fabric, etc.). You can customize primary colors using Theme Studio.

### Inherit AppBar

Inherit mode makes the AppBar inherit the background and font colors from its parent element. Set by using `AppBarColor.Inherit` for the `ColorMode` property.

**When to use:**
- Integrating AppBar seamlessly with existing layouts
- Custom color schemes defined by parent containers
- Maintaining consistent styling across nested components
- Advanced theming scenarios

**Set the color mode:**

```cshtml
ColorMode="AppBarColor.Inherit"
```

**Example:**

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfAppBar ColorMode="AppBarColor.Inherit">
    <a href="https://www.syncfusion.com/blazor-components" target="_blank" rel="noopener" role="link" aria-label="Syncfusion blazor components">
        <div class="syncfusion-logo"></div>
    </a>
    <AppBarSpacer></AppBarSpacer>
    <SfButton IsPrimary="true" Content="FREE TRIAL"></SfButton>
</SfAppBar>

<style>
    .syncfusion-logo {
        background: url(https://cdn.syncfusion.com/blazor/images/demos/syncfusion-logo.svg);
        background-size: contain;
        background-repeat: no-repeat;
        height: 30px;
        width: 150px;
    }
</style>
```

## Combining Size and Color Modes

Size and color modes can be combined to achieve the desired appearance:

**Dense Primary AppBar:**

```cshtml
<SfAppBar ColorMode="AppBarColor.Primary" Mode="AppBarMode.Dense">
    <!-- content -->
</SfAppBar>
```

**Prominent Dark AppBar:**

```cshtml
<SfAppBar ColorMode="AppBarColor.Dark" Mode="AppBarMode.Prominent">
    <!-- content -->
</SfAppBar>
```

**Regular Light AppBar (default):**

```cshtml
<SfAppBar>
    <!-- content -->
</SfAppBar>
```

### Best Practices

**Size Mode Selection:**
- Use **Regular** for most standard applications (default choice)
- Use **Prominent** when you need visual impact or larger branding
- Use **Dense** when vertical space is limited or for secondary toolbars

**Color Mode Selection:**
- Use **Light** for applications with light themes (default choice)
- Use **Dark** for modern applications or reducing eye strain
- Use **Primary** to emphasize branding and create visual hierarchy
- Use **Inherit** for advanced theming or seamless integration with custom layouts

**Button Styling:**
- Use `CssClass="e-inherit"` on buttons within Dark and Primary AppBars for consistent styling
- Use `IsPrimary="true"` on buttons within Light AppBars for better contrast
