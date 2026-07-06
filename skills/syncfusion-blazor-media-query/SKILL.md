---
name: syncfusion-blazor-media-query
description: Create responsive Blazor applications using Media Query component. Trigger when user needs responsive layouts, breakpoint-based rendering, screen size adaptation, multi-device support, or needs to conditionally render components based on device type. Essential for building adaptive UIs that work across mobile, tablet, and desktop devices.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
---

# Syncfusion Blazor Media Query

The Syncfusion Blazor Media Query component detects the current screen size and triggers layout changes based on defined breakpoints. This enables you to build responsive, adaptive applications that provide optimal user experiences across all device sizes.

## When to Use This Skill

Use this skill when you need to:
- **Build responsive layouts** that adapt to different screen sizes (mobile, tablet, desktop)
- **Conditionally render components** based on the current device breakpoint
- **Apply dynamic styling** based on screen width
- **Integrate Media Query** with other Syncfusion components (Data Grid, Charts, etc.)
- **Create reusable responsive patterns** across multiple pages using cascading values

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet package installation (Visual Studio, VSCode, .NET CLI)
- Namespace imports and service registration
- Theme stylesheet configuration
- First component implementation

### Breakpoints and Media Queries
📄 **Read:** [references/breakpoints-and-media-queries.md](references/breakpoints-and-media-queries.md)
- Understanding built-in breakpoints (Small, Medium, Large)
- ActiveBreakpoint property binding
- Modifying built-in breakpoints
- Creating custom media breakpoints

### Responsive Layout Patterns
📄 **Read:** [references/responsive-layout-patterns.md](references/responsive-layout-patterns.md)
- Binding to ActiveBreakpoint property
- Conditional rendering based on screen size
- Dynamic property adjustment patterns
- Layout adaptation strategies

### Component Integration
📄 **Read:** [references/component-integration.md](references/component-integration.md)
- Global component reuse with MainLayout.razor
- Cascading values and parameters
- Integration with Data Grid for responsive tables
- Best practices for multi-component layouts

## Quick Start Example

```razor
@using Syncfusion.Blazor

<SfMediaQuery @bind-ActiveBreakPoint="currentBreakpoint"></SfMediaQuery>

<h3>Current Breakpoint: @currentBreakpoint</h3>

@if (currentBreakpoint == "Small")
{
    <p>You are viewing on a mobile device</p>
}
else if (currentBreakpoint == "Medium")
{
    <p>You are viewing on a tablet</p>
}
else
{
    <p>You are viewing on a desktop</p>
}

@code {
    private string currentBreakpoint { get; set; }
}
```

## Common Use Cases

### 1. Responsive Data Grid
Hide or show columns based on screen size, adjust row rendering mode:
```
Small → Vertical row layout, hide non-essential columns
Medium → Adaptive mode enabled
Large → Horizontal layout, all columns visible
```

### 2. Responsive Navigation
Show full navigation on desktop, hamburger menu on mobile:
```
Small → Collapse navigation to hamburger menu
Large → Show full navigation bar
```

### 3. Multi-Column Layouts
Adjust grid layout based on available space:
```
Small → 1-column layout
Medium → 2-column layout
Large → 3-column layout
```

### 4. Global App Responsiveness
Wrap entire application in MainLayout.razor with cascading Media Query:
```
MainLayout → Provides activeBreakpoint to all pages
Child Pages → Use CascadingParameter to access breakpoint
Result → Entire app responds to screen size changes
```

## Key Properties

| Property | Type | Description |
|----------|------|-------------|
| `ActiveBreakPoint` | string | The currently matching breakpoint name (Small, Medium, Large, or custom) |
| `MediaBreakpoints` | List<MediaBreakpoint> | Custom breakpoints with name and media query string |

## Common Patterns

### Pattern 1: Simple Conditional Rendering
```razor
<SfMediaQuery @bind-ActiveBreakPoint="bp"></SfMediaQuery>

@if (bp == "Small")
{
    <MobileLayout />
}
else if (bp == "Medium")
{
    <TabletLayout />
}
else
{
    <DesktopLayout />
}
```

### Pattern 2: Dynamic Component Properties
```razor
<SfMediaQuery @bind-ActiveBreakPoint="bp"></SfMediaQuery>

@{
    var pageSize = bp == "Small" ? 10 : 25;
    var allowPaging = bp != "Small";
}

<SfGrid PageSettings="@new GridPageSettings { PageSize = pageSize }">
    ...
</SfGrid>
```

### Pattern 3: Responsive Navigation
```razor
<SfMediaQuery @bind-ActiveBreakPoint="bp"></SfMediaQuery>

@if (bp == "Small")
{
    <button @onclick="ToggleMenu">☰ Menu</button>
}
else
{
    <nav>Full Navigation Bar</nav>
}
```
