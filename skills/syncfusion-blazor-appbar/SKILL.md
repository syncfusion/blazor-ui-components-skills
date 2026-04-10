---
name: syncfusion-blazor-appbar
description: Guide for implementing Syncfusion Blazor AppBar component (SfAppBar) in Blazor applications. Use this when implementing navigation bars, app bars, or header toolbars with action controls. This skill covers AppBar configuration, size modes (regular, prominent, dense), color modes, positioning (top, bottom, sticky), menu integration, and responsive design patterns.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Syncfusion Blazor AppBar Component

The Syncfusion Blazor AppBar is a navigation component that displays information and actions related to the current application screen. It provides a consistent header area for branding, navigation, and actions across your application with flexible configuration options for appearance and behavior.

## When to Use This Skill

Use this skill when implementing an AppBar in Blazor applications that requires:

- **Navigation headers** with branding, menus, and action buttons
- **Application toolbars** with context-specific actions and controls
- **Responsive headers** that adapt to different screen sizes
- **Fixed or sticky positioning** at the top or bottom of the viewport
- **Integration with other components** like menus, buttons, sidebars
- **Custom styling** to match application themes and branding

## Component Overview

The AppBar component provides:

- **Flexible layout** with spacers and separators for organizing content
- **Multiple size modes** (Regular, Prominent, Dense) for different use cases
- **Multiple color modes** (Light, Dark, Primary, Inherit) for theming
- **Positioning options** (Top, Bottom, Sticky) for layout control
- **Child component support** for buttons, menus, dropdowns, and custom content
- **Responsive design** with media query support

## Documentation and Navigation Guide

### Getting Started and Installation
📄 **Read:** [references/getting-started.md](references/getting-started.md)

Read this reference when you need to:
- Install and configure the Syncfusion Blazor AppBar package
- Add required namespaces and service registrations
- Include stylesheet and script resources
- Create your first basic AppBar implementation
- Add buttons and spacers to the AppBar
- Understand the component's basic structure and usage

### Size and Color Configuration
📄 **Read:** [references/size-and-color.md](references/size-and-color.md)

Read this reference when you need to:
- Configure AppBar height with size modes (Regular, Prominent, Dense)
- Use Regular AppBar for standard height navigation
- Implement Prominent AppBar for larger titles, images, or hero sections
- Apply Dense AppBar for compact, space-efficient headers
- Configure color modes (Light, Dark, Primary, Inherit)
- Apply Light mode for default light backgrounds
- Apply Dark mode for dark-themed applications
- Apply Primary mode using theme's primary colors
- Apply Inherit mode to use parent element styling
- Combine size and color modes for different visual effects

### Positioning Options
📄 **Read:** [references/positioning.md](references/positioning.md)

Read this reference when you need to:
- Control AppBar position with the Position property
- Implement Top AppBar positioning (default)
- Implement Bottom AppBar for bottom-anchored navigation
- Enable Sticky AppBar behavior with the IsSticky property
- Handle scrolling behavior with sticky positioning
- Manage content layout around positioned AppBars
- Understand positioning interactions with page content

### Design UI Elements
📄 **Read:** [references/design-ui.md](references/design-ui.md)

Read this reference when you need to:
- Add spacing with AppBarSpacer component
- Add visual separators with AppBarSeparator component
- Implement responsive design with media queries
- Integrate SfMenu component for navigation menus
- Integrate SfButton and SfDropDownButton for actions
- Integrate SfSidebar with AppBar toggle button
- Control sidebar expand/collapse from AppBar
- Style child components with e-inherit CSS class
- Build complex AppBar layouts with multiple components

### Styling and Customization
📄 **Read:** [references/styling.md](references/styling.md)

Read this reference when you need to:
- Customize AppBar appearance with CSS classes
- Use the CssClass property for custom styling
- Override default CSS for specific elements
- Apply custom background and font colors
- Use HtmlAttributes for additional HTML attributes
- Add custom aria-label attributes
- Integrate with Theme Studio for theming
- Apply CSS variable overrides
- Understand available CSS class selectors

## Quick Start Example

Here's a minimal example to create a basic AppBar with navigation:

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfAppBar ColorMode="AppBarColor.Primary">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
    <span class="app-title">My Application</span>
    <AppBarSpacer></AppBarSpacer>
    <SfButton CssClass="e-inherit" Content="Login"></SfButton>
</SfAppBar>
```

**Prerequisites:**
1. Install `Syncfusion.Blazor.Navigations` package
2. Add `@using Syncfusion.Blazor.Navigations` to _Imports.razor
3. Register service with `builder.Services.AddSyncfusionBlazor()`
4. Include theme CSS and Syncfusion script in index.html

## Common Patterns

### Application Header with Logo and Actions

```cshtml
<SfAppBar ColorMode="AppBarColor.Light">
    <a href="/" class="logo-link">
        <img src="logo.svg" alt="Company Logo" height="30" />
    </a>
    <AppBarSpacer></AppBarSpacer>
    <SfButton CssClass="e-inherit" Content="Features"></SfButton>
    <SfButton CssClass="e-inherit" Content="Pricing"></SfButton>
    <SfButton CssClass="e-inherit" Content="About"></SfButton>
    <AppBarSeparator></AppBarSeparator>
    <SfButton IsPrimary="true" Content="Sign Up"></SfButton>
</SfAppBar>
```

### Responsive AppBar with Menu Toggle

```cshtml
<SfAppBar ColorMode="AppBarColor.Primary">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-menu" @onclick="ToggleMenu"></SfButton>
    <span class="app-title">Dashboard</span>
    <AppBarSpacer></AppBarSpacer>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-settings"></SfButton>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-user"></SfButton>
</SfAppBar>
```

### Sticky AppBar with Navigation Menu

```cshtml
<SfAppBar ColorMode="AppBarColor.Dark" IsSticky="true">
    <span class="brand">MyApp</span>
    <SfMenu CssClass="e-inherit" TValue="MenuItem">
        <MenuItems>
            <MenuItem Text="Home"></MenuItem>
            <MenuItem Text="Products">
                <MenuItems>
                    <MenuItem Text="Product 1"></MenuItem>
                    <MenuItem Text="Product 2"></MenuItem>
                </MenuItems>
            </MenuItem>
            <MenuItem Text="Contact"></MenuItem>
        </MenuItems>
    </SfMenu>
    <AppBarSpacer></AppBarSpacer>
    <SfButton CssClass="e-inherit" Content="Account"></SfButton>
</SfAppBar>
```

### Bottom AppBar for Mobile Actions

```cshtml
<SfAppBar ColorMode="AppBarColor.Primary" Position="AppBarPosition.Bottom">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-home"></SfButton>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-search"></SfButton>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-heart"></SfButton>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-user"></SfButton>
</SfAppBar>
```

## Key Properties Reference

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ColorMode` | AppBarColor | Light | Sets the background color mode (Light, Dark, Primary, Inherit) |
| `Mode` | AppBarMode | Regular | Sets the height mode (Regular, Prominent, Dense) |
| `Position` | AppBarPosition | Top | Sets the position (Top, Bottom) |
| `IsSticky` | bool | false | Enables sticky behavior on scroll |
| `CssClass` | string | - | Custom CSS class for styling |
| `HtmlAttributes` | Dictionary | - | Additional HTML attributes |

### Child Components

| Component | Purpose | Usage |
|-----------|---------|-------|
| `AppBarSpacer` | Adds flexible spacing between elements | Place between elements to push them apart |
| `AppBarSeparator` | Adds a vertical divider line | Place between groups of related items |
| `SfButton` | Action buttons with e-inherit styling | Use CssClass="e-inherit" to match AppBar theme |
| `SfMenu` | Navigation menu integration | Use CssClass="e-inherit" for consistent styling |

## Common Use Cases

### 1. Main Application Navigation
Create a primary navigation header with logo, menu items, and user actions.

**When:** Building the main navigation for web applications
**Pattern:** Logo + Menu Items + Spacer + User Actions
**Reference:** [design-ui.md](references/design-ui.md)

### 2. Dashboard Header
Implement a dashboard toolbar with context-specific actions and controls.

**When:** Building admin dashboards or data-focused applications
**Pattern:** Menu Toggle + Title + Spacer + Action Buttons
**Reference:** [design-ui.md](references/design-ui.md)

### 3. Mobile Bottom Navigation
Create a mobile-optimized bottom navigation bar with icon buttons.

**When:** Building mobile-responsive applications
**Pattern:** Bottom Position + Icon Buttons
**Reference:** [positioning.md](references/positioning.md)

### 4. Hero Section Header
Implement a prominent AppBar for landing pages with large titles and images.

**When:** Creating marketing or landing pages
**Pattern:** Prominent Mode + Custom Background + Large Typography
**Reference:** [size-and-color.md](references/size-and-color.md)

### 5. Compact Toolbar
Create a space-efficient toolbar for applications with limited vertical space.

**When:** Building data-dense applications or tools
**Pattern:** Dense Mode + Icon Buttons + Separators
**Reference:** [size-and-color.md](references/size-and-color.md)

### 6. Sidebar Toggle Navigation
Implement an AppBar that controls sidebar visibility for navigation.

**When:** Building applications with collapsible side navigation
**Pattern:** Menu Button + Title + Spacer + Actions + Sidebar Integration
**Reference:** [design-ui.md](references/design-ui.md)

## Tips for Success

- **Use `e-inherit` CSS class** on child buttons and menus to inherit AppBar's color scheme
- **Combine AppBarSpacer** to create flexible layouts that adapt to content
- **Test responsive behavior** with media queries for different screen sizes
- **Consider sticky positioning** for navigation that stays visible while scrolling
- **Customize with CssClass** instead of overriding default styles directly
- **Match size mode to context**: Regular for standard apps, Prominent for heroes, Dense for tools
