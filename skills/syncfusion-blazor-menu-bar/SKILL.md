---
name: syncfusion-blazor-menu-bar
description: Implement navigation menus using Syncfusion Blazor Menu Bar component. Use this skill to create horizontal or vertical menus with submenus, icons, events, animations, and data binding. Essential for building professional menu bars in Blazor applications.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
  category: "Navigation Components"
---

# Implementing Syncfusion Blazor Menu Bar

## When to Use This Skill

Use this skill when you need to:
- Create horizontal or vertical navigation menus in Blazor applications
- Add dropdown submenus with multiple nesting levels
- Display icons in menu items
- Handle user interactions with menu events
- Bind menu items to data sources
- Apply animations and custom styling to menus
- Build professional menu bars for web applications

## Component Overview

The Syncfusion Blazor Menu Bar component provides a flexible, feature-rich menu system for navigation. It supports:
- **Hierarchical structure**: Multiple levels of nested menu items
- **Data binding**: Self-referential data source mapping
- **Events**: Comprehensive event handling (Created, OnOpen, OnClose, Opened, Closed, ItemSelected, OnItemRender)
- **Styling**: Icons, custom CSS, separators, and disabled states
- **Animation**: Multiple animation effects (SlideDown, ZoomIn, FadeIn, None)
- **Orientation**: Horizontal (default) or Vertical layout
- **Customization**: Rich API for dynamic menu management

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and package setup (NuGet packages)
- Prerequisites and system requirements
- Blazor WebAssembly App creation
- Basic menu bar implementation
- Imports and service registration
- Theme stylesheet and script resources

### Menu Items and Data Binding
📄 **Read:** [references/menu-items-and-data-binding.md](references/menu-items-and-data-binding.md)
- MenuItem structure and properties (Text, Url, Separator, Disabled)
- Creating hardcoded menu structures
- Self-referential data source binding
- MenuFieldSettings configuration (ItemId, ParentId, Text, Children)
- Custom MenuItem classes and data models
- Hierarchical menu creation patterns

### Events and Interactions
📄 **Read:** [references/events-and-interactions.md](references/events-and-interactions.md)
- Available events (Created, OnItemRender, OnOpen, OnClose, Opened, Closed, ItemSelected)
- MenuEvents component setup
- Event handler implementation
- TValue specification requirements
- Capturing selected items and item data
- User interaction patterns

### Icons and Styling
📄 **Read:** [references/icons-and-styling.md](references/icons-and-styling.md)
- Adding icons with IconCss property
- Icon positioning and placement
- CSS custom icons (content, unicode)
- Separators in menu items
- Custom CSS classes
- Navigation links with Url property
- Disabled menu items

### Animation and Orientation
📄 **Read:** [references/animation-and-orientation.md](references/animation-and-orientation.md)
- MenuAnimationSettings configuration
- Animation effects (SlideDown, ZoomIn, FadeIn, None)
- Duration customization
- Orientation property (Horizontal, Vertical)
- RTL support
- Responsive menu behavior

### Advanced Scenarios and Customization
📄 **Read:** [references/advanced-scenarios.md](references/advanced-scenarios.md)
- ShowItemOnClick property for click-based opening
- Add/Remove menu items dynamically
- Enable/Disable menu items
- Menu templates and custom rendering
- Performance considerations
- Common patterns and best practices

### API Reference
📄 **Read:** [references/api-reference.md](references/api-reference.md)
- Complete SfMenu component properties and methods
- MenuItem component properties and structure
- MenuAnimationSettings and MenuEffect enums
- Orientation enum (Horizontal, Vertical)
- MenuEvents and MenuEventArgs documentation
- MenuFieldSettings for data binding
- MenuTemplates for custom rendering
- Complete integration example

## Quick Start Example

```razor
@using Syncfusion.Blazor.Navigations

<SfMenu TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="File">
            <MenuItems>
                <MenuItem Text="Open"></MenuItem>
                <MenuItem Text="Save"></MenuItem>
                <MenuItem Text="Exit"></MenuItem>
            </MenuItems>
        </MenuItem>
        <MenuItem Text="Edit">
            <MenuItems>
                <MenuItem Text="Cut"></MenuItem>
                <MenuItem Text="Copy"></MenuItem>
                <MenuItem Text="Paste"></MenuItem>
            </MenuItems>
        </MenuItem>
        <MenuItem Text="Help"></MenuItem>
    </MenuItems>
</SfMenu>
```

## Common Patterns

### Pattern 1: Simple Hardcoded Menu
Use direct MenuItem components for small, static menus. This is the simplest approach and requires no data binding setup.

### Pattern 2: Data-Bound Menu from List
Use MenuFieldSettings to bind a List<T> to your menu. Perfect for dynamic menus sourced from databases or APIs.

### Pattern 3: Self-Referential Data
Map ParentId relationships in flat data to create hierarchical menus. Ideal for category/subcategory structures.

### Pattern 4: Event-Driven Menus
Use MenuEvents to respond to user actions (Created, ItemSelected, OnOpen, OnClose). Essential for tracking user navigation.

### Pattern 5: Styled and Animated Menus
Combine IconCss, MenuAnimationSettings, and Orientation for visually appealing, professional menus.

## Key Properties

| Property | Purpose | Common Values |
|----------|---------|----------------|
| `TValue` | Generic type for menu items | `MenuItem`, custom classes |
| `Orientation` | Menu layout direction | `Horizontal` (default), `Vertical` |
| `ShowItemOnClick` | Open submenus on click instead of hover | `true`, `false` (default) |
| `MenuAnimationSettings` | Animation configuration | SlideDown, ZoomIn, FadeIn |
| `MenuFieldSettings` | Data binding field mappings | ItemId, Text, ParentId, Children |
| `MenuEvents` | Event handlers | Created, OnOpen, ItemSelected, etc. |

## Next Steps

1. **Setup**: Start with the Getting Started reference for installation
2. **Create**: Choose a menu structure (hardcoded, data-bound, or hierarchical)
3. **Enhance**: Add events, styling, and animations as needed
4. **Test**: Verify menu navigation and interactions work correctly
