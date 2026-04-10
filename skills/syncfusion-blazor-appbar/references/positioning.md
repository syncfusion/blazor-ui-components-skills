# Positioning in Blazor AppBar Component

## Table of Contents
- [Overview](#overview)
- [Top AppBar](#top-appbar)
- [Bottom AppBar](#bottom-appbar)
- [Sticky AppBar](#sticky-appbar)
- [Content Layout Considerations](#content-layout-considerations)

## Overview

The position of the AppBar can be controlled using the `Position` and `IsSticky` properties. These properties allow you to anchor the AppBar to the top or bottom of the viewport and control its behavior when scrolling.

**Available positioning options:**
- **Top AppBar** - Positions the AppBar at the top of the content (default)
- **Bottom AppBar** - Positions the AppBar at the bottom of the content
- **Sticky AppBar** - Makes the AppBar remain visible while scrolling

## Top AppBar

Top AppBar is the default positioning mode where the AppBar is placed at the top of the content. This is the most common placement for navigation headers.

**When to use:**
- Standard application headers
- Primary navigation bars
- Branding and logo placement
- Main application toolbar

**Default behavior:**
By default, the AppBar uses top positioning without needing to explicitly set the `Position` property.

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfAppBar ColorMode="AppBarColor.Primary">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
    <AppBarSpacer></AppBarSpacer>
    <SfButton CssClass="e-inherit" Content="FREE TRIAL"></SfButton>
</SfAppBar>

<div class="appbar-content" style="font-size: 12px">
    <p>
        The AppBar also known as action bar or nav bar displays information and actions related to the current application screen. It is used to show branding, screen titles, navigation, and actions. The control supports height mode, color mode, positioning, and more.
    </p>
    <p>
        The AppBar control provides flexible ways to configure the look and feel of the bar to match your requirement.
    </p>
    <p>
        Developers can control the appearance and behaviors of the AppBar using a rich set of APIs.
    </p>
    <p>
        The AppBar component supports built-in themes such as Material, Bootstrap, Fabric (Office 365), Tailwind CSS, and high contrast. Users can customize these built-in themes or create new themes to achieve their desired look and feel by either simply overriding SASS variables or using our Theme Studio application.
    </p>
</div>

<style>
    .e-btn.e-inherit {
        margin: 0 3px;
    }
</style>
```

## Bottom AppBar

Bottom AppBar positions the AppBar at the bottom of the content. This is useful for mobile-style navigation or action bars that should be easily accessible at the bottom of the screen.

**When to use:**
- Mobile applications with bottom navigation
- Quick action bars
- Contextual toolbars for selected content
- Alternative navigation patterns

**Set the position:**

```cshtml
Position="AppBarPosition.Bottom"
```

**Example:**

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<div class="appbar-content" style="font-size: 12px">
    <p>
        The AppBar also known as action bar or nav bar displays information and actions related to the current application screen. It is used to show branding, screen titles, navigation, and actions. The control supports height mode, color mode, positioning, and more.
    </p>
    <p>
        The AppBar control provides flexible ways to configure the look and feel of the bar to match your requirement.
    </p>
    <p>
        Developers can control the appearance and behaviors of the AppBar using a rich set of APIs.
    </p>
    <p>
        The AppBar component supports built-in themes such as Material, Bootstrap, Fabric (Office 365), Tailwind CSS, and high contrast. Users can customize these built-in themes or create new themes to achieve their desired look and feel by either simply overriding SASS variables or using our Theme Studio application.
    </p>
</div>

<SfAppBar ColorMode="AppBarColor.Primary" Position="AppBarPosition.Bottom">
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

**Mobile navigation pattern:**

```cshtml
<SfAppBar ColorMode="AppBarColor.Primary" Position="AppBarPosition.Bottom">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-home"></SfButton>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-search"></SfButton>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-heart"></SfButton>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-user"></SfButton>
</SfAppBar>
```

## Sticky AppBar

Sticky AppBar remains fixed in its position while the content scrolls beneath it. Enable this behavior by setting the `IsSticky` property to `true`.

**When to use:**
- Keeping navigation always accessible
- Persistent access to actions while scrolling
- Long-form content with important header controls
- Applications requiring consistent navigation visibility

**Set sticky behavior:**

```cshtml
IsSticky="true"
```

**Example with top positioning (default):**

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<div class="control-container">
    <SfAppBar ColorMode="AppBarColor.Primary" IsSticky="true">
        <SfButton CssClass="e-inherit" IconCss="e-icons e-menu"></SfButton>
        <AppBarSpacer></AppBarSpacer>
        <SfButton CssClass="e-inherit" Content="FREE TRIAL"></SfButton>
    </SfAppBar>
    
    <div class="appbar-content" style="font-size: 12px">
        <p>
            The AppBar also known as action bar or nav bar displays information and actions related to the current application screen. It is used to show branding, screen titles, navigation, and actions. The control supports height mode, color mode, positioning, and more.
        </p>
        <p>
            The AppBar control provides flexible ways to configure the look and feel of the bar to match your requirement.
        </p>
        <p>
            Developers can control the appearance and behaviors of the AppBar using a rich set of APIs.
        </p>
        <p>
            The AppBar component supports built-in themes such as Material, Bootstrap, Fabric (Office 365), Tailwind CSS, and high contrast. Users can customize these built-in themes or create new themes to achieve their desired look and feel by either simply overriding SASS variables or using our Theme Studio application.
        </p>
    </div>
</div>

<style>
    .control-container {
        height: 220px;
        margin: 0 auto;
        width: 500px;
        overflow-y: scroll;
    }
    .e-btn.e-inherit {
        margin: 0 3px;
    }
</style>
```

**Sticky behavior details:**
- The AppBar remains fixed while scrolling the content
- Works with both top and bottom positioning
- Useful for maintaining navigation visibility
- Can be combined with any size or color mode

**Combining sticky with bottom position:**

```cshtml
<SfAppBar ColorMode="AppBarColor.Primary" Position="AppBarPosition.Bottom" IsSticky="true">
    <SfButton CssClass="e-inherit" IconCss="e-icons e-home"></SfButton>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-search"></SfButton>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-notifications"></SfButton>
    <SfButton CssClass="e-inherit" IconCss="e-icons e-user"></SfButton>
</SfAppBar>
```

## Content Layout Considerations

When using positioned AppBars, consider how they affect your page layout:

### Top AppBar Layout

**Without sticky:**
- AppBar flows with page content
- Content appears below the AppBar
- AppBar scrolls away with content

**With sticky:**
- AppBar stays fixed at top
- Content scrolls beneath AppBar
- May need padding-top on content to prevent overlap

```cshtml
<style>
    .page-content {
        padding-top: 70px; /* Adjust based on AppBar height */
    }
</style>
```

### Bottom AppBar Layout

**Without sticky:**
- AppBar flows at the end of content
- Only visible when scrolling to bottom
- Moves with content

**With sticky:**
- AppBar stays fixed at bottom
- Content scrolls above AppBar
- May need padding-bottom on content to prevent overlap

```cshtml
<style>
    .page-content {
        padding-bottom: 70px; /* Adjust based on AppBar height */
    }
</style>
```

### Responsive Considerations

For responsive applications, adjust padding based on screen size:

```cshtml
<style>
    /* Desktop */
    .page-content {
        padding-top: 70px;
    }
    
    /* Mobile */
    @media screen and (max-width: 768px) {
        .page-content {
            padding-top: 60px; /* Smaller AppBar on mobile */
        }
    }
</style>
```

### Z-Index Considerations

The AppBar has a default z-index to stay above content. If you have overlapping elements, you may need to adjust z-index values:

```cshtml
<style>
    .e-appbar {
        z-index: 1000; /* Ensure AppBar appears above other content */
    }
</style>
```

## Best Practices

**Positioning:**
- Use **Top** positioning for primary navigation (most common)
- Use **Bottom** positioning for mobile-style navigation or quick actions
- Use **Sticky** when navigation should always be accessible

**Layout:**
- Add appropriate padding to page content to prevent overlap with sticky AppBars
- Test scrolling behavior with various content lengths
- Consider mobile vs. desktop layouts

**Performance:**
- Sticky positioning uses CSS position: sticky, which is performant
- Avoid complex animations on sticky AppBars for better scroll performance

**User Experience:**
- Sticky AppBars improve navigation accessibility on long pages
- Bottom AppBars on mobile improve thumb reach for navigation
- Consistent positioning across the application improves predictability
