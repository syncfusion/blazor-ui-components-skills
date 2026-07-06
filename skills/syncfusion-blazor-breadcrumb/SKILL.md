---
name: syncfusion-blazor-breadcrumb
description: Implement Syncfusion Blazor Breadcrumb (SfBreadcrumb) control for hierarchical navigation. Use this when building breadcrumb trails, auto-generating items from URLs, or managing dynamic navigation sequences. This skill covers overflow modes, item templates, icon customization, and responsive layout configurations.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
  category: "Navigation Components"
---

# Syncfusion Blazor Breadcrumb Component

## When to Use This Skill

Use this skill when you need to:
- **Build navigation breadcrumbs** for single-page applications (SPAs) to show the user's current position in a hierarchy
- **Auto-generate breadcrumb trails** based on the current URL path
- **Create custom navigation sequences** with dynamic item management (add/remove items at runtime)
- **Handle overflow scenarios** with many breadcrumb items (collapsed, menu, wrap, scroll modes)
- **Customize visual appearance** with icons, images, separators, or templates
- **Implement templated breadcrumb layouts** using Chip components or custom renderers
- **Enable/disable navigation** based on business logic or user roles

The Syncfusion Blazor Breadcrumb component provides built-in URL parsing, responsive overflow handling, and templating capabilities to simplify hierarchical navigation UIs.

---

## Component Overview

The [Blazor Breadcrumb component](https://www.syncfusion.com/blazor-components/blazor-breadcrumb) is a navigation control that displays a hierarchy of pages or sections visited within an application. 

**Key Features:**
- **Auto-generated items** from current URL path
- **Manual item declaration** using `BreadcrumbItem` tag directive
- **Multiple overflow modes** (Collapsed, Menu, Wrap, Scroll, Hidden, None)
- **Icon and image support** via `IconCss` property
- **Template-based customization** for items and separators
- **Dynamic item management** (add/remove at runtime)
- **URL navigation** (relative or absolute)
- **Responsive design** with CSS customization

---

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet package installation (`Syncfusion.Blazor.Navigations`)
- Namespace imports and service registration
- Theme stylesheet and script configuration
- Basic `<SfBreadcrumb>` component initialization
- First render with URL-based navigation

### Item Configuration
📄 **Read:** [references/breadcrumb-items-configuration.md](references/breadcrumb-items-configuration.md)
- `BreadcrumbItem` properties (`Text`, `Url`, `IconCss`)
- Declaring items using `<BreadcrumbItem>` tag directive
- Auto-generating items from the current page URL
- Generating items from an absolute URL using the `Url` property
- Dynamically adding and removing items at runtime using the `Items` property

### Icons and Visual Elements
📄 **Read:** [references/icons-and-visual-elements.md](references/icons-and-visual-elements.md)
- Adding font icons with `e-icons` CSS class
- Image customization via `IconCss` property with custom CSS classes
- SVG image support for breadcrumb items
- Icon-only breadcrumb items (no text)
- Displaying icons selectively (e.g., first item only)

### Navigation and Routing
📄 **Read:** [references/navigation-and-routing.md](references/navigation-and-routing.md)
- Relative URL navigation in breadcrumb items
- Absolute URL navigation for external links
- Disabling navigation with `EnableNavigation="false"`
- Enabling navigation for the active (last) item with `EnableActiveItemNavigation="true"`

### Overflow Modes
📄 **Read:** [references/overflow-modes.md](references/overflow-modes.md)
- Limiting displayed items with `MaxItems` property
- `Collapsed` mode: Hide middle items, show first/last with expand button
- `Menu` mode: Remaining items in a dropdown submenu
- `Wrap` mode: Items wrap to multiple lines
- `Scroll` mode: Horizontal scroll bar for overflow
- `Hidden` mode: Hidden items revealed on parent click
- `None` mode: All items on a single line

### Templates and Customization
📄 **Read:** [references/templates-and-customization.md](references/templates-and-customization.md)
- `ItemTemplate` for rendering custom item UI
- `SeparatorTemplate` for customizing item separators
- Template `context` parameter for accessing current item data
- Integrating Chip components with `ItemTemplate`
- Specific item customization using child content

### Styling and Appearance
📄 **Read:** [references/styling-and-appearance.md](references/styling-and-appearance.md)
- CSS classes for customization (`.e-breadcrumb-item`, `.e-breadcrumb-text`, `.e-breadcrumb-icon`, `.e-breadcrumb-separator`)
- Theme Studio integration for custom themes
- Overriding default Breadcrumb styling with CSS
- Customizing background, text color, icon color, and separator color

---

## Quick Start Example

**Basic breadcrumb with explicit items:**

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb>
    <BreadcrumbItems>
        <BreadcrumbItem IconCss="e-icons e-home" Url="https://example.com/home"></BreadcrumbItem>
        <BreadcrumbItem Text="Products" Url="https://example.com/products"></BreadcrumbItem>
        <BreadcrumbItem Text="Electronics" Url="https://example.com/products/electronics"></BreadcrumbItem>
        <BreadcrumbItem Text="Laptops"></BreadcrumbItem>
    </BreadcrumbItems>
</SfBreadcrumb>
```

**Auto-generated breadcrumb from current URL:**

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb></SfBreadcrumb>
```

---

## Common Patterns

### Pattern 1: Icon + Text Items
Combine icons with text for visual hierarchy and quick scanning:

```cshtml
<SfBreadcrumb>
    <BreadcrumbItems>
        <BreadcrumbItem IconCss="e-icons e-home" Url="/"></BreadcrumbItem>
        <BreadcrumbItem IconCss="e-icons e-folder-open" Text="Documents" Url="/docs"></BreadcrumbItem>
        <BreadcrumbItem Text="Projects"></BreadcrumbItem>
    </BreadcrumbItems>
</SfBreadcrumb>
```

### Pattern 2: Handling Long Breadcrumb Trails
Use `MaxItems` with `OverflowMode` to manage deep hierarchies:

```cshtml
<SfBreadcrumb MaxItems="4" OverflowMode="BreadcrumbOverflowMode.Menu">
    <BreadcrumbItems>
        <BreadcrumbItem Text="Home" Url="/"></BreadcrumbItem>
        <BreadcrumbItem Text="Category 1" Url="/cat1"></BreadcrumbItem>
        <BreadcrumbItem Text="Category 2" Url="/cat1/cat2"></BreadcrumbItem>
        <BreadcrumbItem Text="Category 3" Url="/cat1/cat2/cat3"></BreadcrumbItem>
        <BreadcrumbItem Text="Current Page"></BreadcrumbItem>
    </BreadcrumbItems>
</SfBreadcrumb>
```

### Pattern 3: Dynamic Item Management
Add/remove items at runtime based on user actions:

```csharp
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb Items="@breadcrumbItems"></SfBreadcrumb>

<button @onclick="AddItem">Add Item</button>
<button @onclick="RemoveItem">Remove Item</button>

@code {
    List<BreadcrumbItem> breadcrumbItems = new List<BreadcrumbItem>
    {
        new BreadcrumbItem { IconCss = "e-icons e-home" },
        new BreadcrumbItem { Text = "Products" }
    };

    private void AddItem()
    {
        breadcrumbItems.Add(new BreadcrumbItem { Text = "New Item" });
    }

    private void RemoveItem()
    {
        if (breadcrumbItems.Count > 0)
            breadcrumbItems.RemoveAt(breadcrumbItems.Count - 1);
    }
}
```

### Pattern 4: Custom Separator with Template
Use `SeparatorTemplate` for custom visual separators:

```cshtml
<SfBreadcrumb>
    <BreadcrumbItems>
        <BreadcrumbItem Text="Home" Url="/"></BreadcrumbItem>
        <BreadcrumbItem Text="Products" Url="/products"></BreadcrumbItem>
        <BreadcrumbItem Text="Details"></BreadcrumbItem>
    </BreadcrumbItems>
    <BreadcrumbTemplates>
        <SeparatorTemplate>
            <span class="e-icons e-arrow"></span>
        </SeparatorTemplate>
    </BreadcrumbTemplates>
</SfBreadcrumb>
```

---

## Key Props

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `Items` | `List<BreadcrumbItem>` | null | Collection of breadcrumb items to display |
| `Url` | `string` | null | Generate items from a specific URL path |
| `MaxItems` | `int?` | null | Maximum items to display before overflow handling |
| `OverflowMode` | `BreadcrumbOverflowMode` | `Default` | How to handle items exceeding `MaxItems` |
| `EnableNavigation` | `bool` | `true` | Enable/disable URL navigation on item click |
| `EnableActiveItemNavigation` | `bool` | `false` | Enable navigation for the last (active) item |
| `ActiveItem` | `string` | null | Set the active (currently selected) item |
| `Class` | `string` | null | Custom CSS class for styling |

---

## Common Use Cases

1. **E-commerce product breadcrumbs:** Show navigation path through categories (Home > Electronics > Laptops > Gaming Laptops)
2. **Admin dashboard navigation:** Display user position in multi-level menu hierarchy
3. **File explorer:** Visualize folder navigation path with icons
4. **Documentation sites:** Show section hierarchy (Documentation > Guides > Getting Started)
5. **Search result filters:** Display active filter path (Search > Results > Filtered by Price)
6. **Step-by-step workflows:** Indicate current step in multi-step processes (Step 1 > Step 2 > Current)

---

**Next Steps:** Read the appropriate reference file based on your use case from the "Documentation and Navigation Guide" section above.
