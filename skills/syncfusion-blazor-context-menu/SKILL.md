---
name: syncfusion-blazor-context-menu
description: "Implement Syncfusion Blazor ContextMenu component for building right-click menus, context-sensitive navigation, or popup menu systems. Use this when you need hierarchical menu structures with nesting support, data binding, or custom templates. This skill covers setup, customization, events, and dynamic menu management."
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
  category: "Navigation Components"
---

# Syncfusion Blazor ContextMenu Component

## When to Use This Skill

Use the **Syncfusion Blazor ContextMenu** skill when:
- Building **right-click context menus** for interactive elements
- Creating **popup navigation menus** triggered by user interaction
- Need **hierarchical menu structures** with nesting support
- Implementing **custom menu templates** with shortcuts or icons
- Managing **dynamic menu states** (enable/disable items)
- Requiring **data-driven menus** from custom object collections
- Building **advanced interactions** with dialogs, animations, or scrolling

ContextMenu provides fully customizable, event-driven popup menus perfect for desktop-like interactions in web apps.

---

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet installation (Navigations + Themes packages)
- Namespace imports and Syncfusion service registration
- Stylesheet and script resource setup
- Basic ContextMenu component markup
- First render and click-to-test validation
- Common import patterns for Blazor projects

### Customization and Nesting
📄 **Read:** [references/customization-and-nesting.md](references/customization-and-nesting.md)
- Custom MenuTemplate for rich UI (shortcuts, badges, icons)
- CssClass property for component-level styling
- Multilevel nesting (2-3 nested levels)
- Nested MenuItem hierarchies
- Best practices for menu depth and UX

### Icons and Navigation
📄 **Read:** [references/icons-and-navigation.md](references/icons-and-navigation.md)
- IconCss property and e-icons class mapping
- Custom icon class support
- Icon positioning in menu items
- Url property for hyperlink navigation
- Navigation target behaviors (_blank, _self, etc.)

### Styling and Appearance
📄 **Read:** [references/styling-and-appearance.md](references/styling-and-appearance.md)
- CSS class override reference (.e-contextmenu-container, .e-menu-item, etc.)
- Focus and selected state styling
- Theme customization with Theme Studio
- Common styling patterns and examples

### Events and Interactions
📄 **Read:** [references/events-and-interactions.md](references/events-and-interactions.md)
- ItemSelected event binding and patterns
- MenuEventArgs data access
- Programmatic Open/Close methods with coordinates
- Manual menu control and triggering
- Event handler best practices

### Data Binding
📄 **Read:** [references/data-binding.md](references/data-binding.md)
- Items collection binding
- MenuFieldSettings property mapping
- Custom object data sources
- Hierarchical data binding with nested collections
- Dynamic menu population

### Menu Item Management
📄 **Read:** [references/menu-item-management.md](references/menu-item-management.md)
- Disabled property for item state management
- Enable/disable logic and workflows
- OnOpen event for conditional disabling
- Separator property usage and styling
- Controlling item availability dynamically

### Advanced Features
📄 **Read:** [references/advanced-features.md](references/advanced-features.md)
- MenuAnimationSettings with effects (FadeIn, SlideDown, ZoomIn, None)
- Dialog integration with ItemSelected events
- EnableScrolling for large menu lists
- ScrollHeight configuration
- Large menu handling strategies

---

## Quick Start Example

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open menu</div>

<SfContextMenu Target="#target" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Cut"></MenuItem>
        <MenuItem Text="Copy"></MenuItem>
        <MenuItem Text="Paste"></MenuItem>
    </MenuItems>
</SfContextMenu>

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

**Result:** Right-click the gray area to see the context menu with Cut, Copy, Paste options.

---

## Common Patterns

### Pattern 1: Basic Right-Click Menu
```cshtml
<SfContextMenu Target="#myElement" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Edit"></MenuItem>
        <MenuItem Text="Delete"></MenuItem>
    </MenuItems>
</SfContextMenu>
```
**Use when:** Simple menu with basic actions on target element.

### Pattern 2: Nested Menu (Submenu on Hover)
```cshtml
<SfContextMenu Target="#myElement" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="File">
            <MenuItems>
                <MenuItem Text="Open"></MenuItem>
                <MenuItem Text="Save"></MenuItem>
            </MenuItems>
        </MenuItem>
    </MenuItems>
</SfContextMenu>
```
**Use when:** Hierarchical menu structure with grouped actions.

### Pattern 3: Event-Driven Actions
```cshtml
<SfContextMenu Target="#myElement" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Delete"></MenuItem>
    </MenuItems>
    <MenuEvents TValue="MenuItem" ItemSelected="@OnItemSelected"></MenuEvents>
</SfContextMenu>

@code {
    private void OnItemSelected(MenuEventArgs<MenuItem> e) {
        // Handle: e.Item.Text
    }
}
```
**Use when:** Need to execute custom logic on menu item click.

### Pattern 4: Dynamic Menu from Data
```cshtml
<SfContextMenu Target="#myElement" Items="@menuItems">
    <MenuFieldSettings Text="Title"></MenuFieldSettings>
</SfContextMenu>

@code {
    private List<MenuOption> menuItems = new();
    
    protected override void OnInitialized() {
        menuItems.Add(new MenuOption { Title = "Option 1" });
        menuItems.Add(new MenuOption { Title = "Option 2" });
    }
    
    private class MenuOption { public string Title { get; set; } }
}
```
**Use when:** Menu items come from database or dynamic source.

---

## Key Properties

| Property | Type | Purpose |
|---|---|---|
| `Target` | string | CSS selector for right-click target element |
| `Items` | List | Data collection for data-binding |
| `TValue` | generic | Type parameter (usually `MenuItem`) |
| `CssClass` | string | Custom CSS class for component styling |
| `ShowItemOnClick` | bool | Enable sub-menu on click instead of hover |
| `EnableScrolling` | bool | Enable scroll for large menu lists |

---

## Common Use Cases

**Use Case 1: File Context Menu**
- Right-click file list item → Cut, Copy, Paste, Delete, Rename options
- **Reference:** events-and-interactions.md + menu-item-management.md

**Use Case 2: Rich Text Editor**
- Right-click text → Bold, Italic, Underline, Link options
- Customize template with icons and shortcuts
- **Reference:** customization-and-nesting.md + icons-and-navigation.md

**Use Case 3: Admin Dashboard**
- Right-click user row → View, Edit, Permissions, Delete
- Enable/disable based on user role
- **Reference:** menu-item-management.md + events-and-interactions.md

**Use Case 4: Navigation Menu**
- Click button → Open menu → Navigate to pages
- Nested categories with smooth animation
- **Reference:** icons-and-navigation.md + advanced-features.md

---

## Next Steps

1. **Start:** Read [getting-started.md](references/getting-started.md) to install and setup
2. **Build:** Use [customization-and-nesting.md](references/customization-and-nesting.md) for custom UI
3. **Enhance:** Add icons, events, and interactivity with other reference guides
4. **Explore:** Check [advanced-features.md](references/advanced-features.md) for animations, dialogs, scrolling
