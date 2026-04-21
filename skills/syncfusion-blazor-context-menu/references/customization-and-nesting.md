# Customization and Nesting

## Table of Contents
- [Custom MenuTemplate](#custom-menutemplate)
- [CssClass Styling](#cssclass-styling)
- [Multilevel Nesting](#multilevel-nesting)
- [ShowItemOnClick](#showitemoncClick)
- [Best Practices](#best-practices)

## Custom MenuTemplate

Use `MenuTemplate` to customize menu item appearance with shortcuts, icons, badges, or other rich content.

### Template with Keyboard Shortcuts

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem">
    <MenuTemplates TValue="MenuItem">
        <Template>
            <span>@context.Text</span>
            @if (context.Text == "Save As...") {
                <span class="shortcut">Ctrl + S</span>
            } else if (context.Text == "Inspect") {
                <span class="shortcut">Ctrl + Shift + I</span>
            }
        </Template>
    </MenuTemplates>
    <MenuItems>
        <MenuItem Text="Save As..."></MenuItem>
        <MenuItem Text="Inspect"></MenuItem>
    </MenuItems>
</SfContextMenu>

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
    .shortcut {
        float: right; font-size: 11px; opacity: 0.6; 
        margin-left: 10px; color: #666;
    }
</style>
```

**Features:**
- `@context.Text` - Access menu item text
- `@context` - Full menu item object
- Conditional rendering for shortcuts
- Right-aligned shortcut display

### Template with Icons and Labels

```cshtml
<MenuTemplates TValue="MenuItem">
    <Template>
        <span class="e-icons @context.IconCss"></span>
        <span>@context.Text</span>
    </Template>
</MenuTemplates>
```

---

## CssClass Styling

Style entire ContextMenu component with custom CSS class.

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem" CssClass="custom-menu">
    <MenuItems>
        <MenuItem Text="Edit"></MenuItem>
        <MenuItem Text="View"></MenuItem>
        <MenuItem Text="Delete"></MenuItem>
    </MenuItems>
</SfContextMenu>

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
    
    /* Custom menu styling */
    .custom-menu.e-contextmenu-container .e-menu-item {
        padding: 8px 15px;
        font-size: 12px;
        font-style: italic;
    }
    
    .custom-menu.e-contextmenu-container .e-menu-item:hover {
        background-color: #e3f2fd;
        border-radius: 4px;
    }
    
    .custom-menu.e-contextmenu-container .e-menu-item.e-focused {
        background-color: #bbdefb;
    }
</style>
```

**CSS Classes Available:**
- `.e-contextmenu-container` - Main container
- `.e-menu-item` - Individual item
- `.e-menu-item:hover` - On hover state
- `.e-menu-item.e-focused` - Focused state
- `.e-menu-item.e-selected` - Selected state

---

## Multilevel Nesting

Create hierarchical menus with nested `MenuItems` components (up to 3+ levels).

### Two-Level Nesting

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="File">
            <MenuItems>
                <MenuItem Text="New"></MenuItem>
                <MenuItem Text="Open"></MenuItem>
                <MenuItem Text="Save"></MenuItem>
            </MenuItems>
        </MenuItem>
        <MenuItem Text="Edit">
            <MenuItems>
                <MenuItem Text="Cut"></MenuItem>
                <MenuItem Text="Copy"></MenuItem>
                <MenuItem Text="Paste"></MenuItem>
            </MenuItems>
        </MenuItem>
        <MenuItem Text="View"></MenuItem>
    </MenuItems>
</SfContextMenu>

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

### Three-Level Nesting

```cshtml
<SfContextMenu Target="#target" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Bookmarks">
            <MenuItems>
                <MenuItem Text="Most Visited">
                    <MenuItems>
                        <MenuItem Text="Google"></MenuItem>
                        <MenuItem Text="Gmail"></MenuItem>
                    </MenuItems>
                </MenuItem>
                <MenuItem Text="Recently Added"></MenuItem>
            </MenuItems>
        </MenuItem>
        <MenuItem Text="History"></MenuItem>
    </MenuItems>
</SfContextMenu>
```

**Behavior:**
- Hover over parent item → Sub-menu slides in
- Hover off → Sub-menu closes
- Works with unlimited nesting (though 2-3 levels recommended for UX)

---

## ShowItemOnClick

By default, sub-menus open on **hover**. Use `ShowItemOnClick="true"` to open on **click** instead.

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem" ShowItemOnClick="true">
    <MenuItems>
        <MenuItem Text="File">
            <MenuItems>
                <MenuItem Text="New"></MenuItem>
                <MenuItem Text="Open"></MenuItem>
                <MenuItem Text="Save"></MenuItem>
            </MenuItems>
        </MenuItem>
        <MenuItem Text="Edit">
            <MenuItems>
                <MenuItem Text="Cut"></MenuItem>
                <MenuItem Text="Copy"></MenuItem>
                <MenuItem Text="Paste"></MenuItem>
            </MenuItems>
        </MenuItem>
    </MenuItems>
</SfContextMenu>

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

**When to use:**
- Touch/mobile interfaces (hover doesn't work well)
- Complex nested menus (reduce accidental opens)
- Desktop apps expecting click behavior

---

## Best Practices

### Practice 1: Nesting Depth
- **Limit to 2-3 levels** for usability
- Deeper nesting makes items hard to reach
- Consider flat menu with categories for large item counts

### Practice 2: Separator Items
```cshtml
<MenuItem Separator="true"></MenuItem>
```
Use separators to group related items visually.

### Practice 3: Template Consistency
- Keep custom template styling consistent across all items
- Use `@context` properties uniformly
- Avoid overly complex templates (performance)

### Practice 4: Touch Support
```cshtml
<SfContextMenu Target="#target" ShowItemOnClick="true">
```
Enable `ShowItemOnClick` if supporting touch devices.

### Practice 5: Item Count
- Keep menu to **5-8 items** at root level
- Use nesting or scrolling for longer menus (see advanced-features.md)
- Group related actions under common parent

---

## Common Patterns

**Pattern: Settings Menu with Groups**
```cshtml
<MenuItems>
    <MenuItem Text="Display">
        <MenuItems>
            <MenuItem Text="Zoom"></MenuItem>
            <MenuItem Text="Theme"></MenuItem>
        </MenuItems>
    </MenuItem>
    <MenuItem Separator="true"></MenuItem>
    <MenuItem Text="Advanced"></MenuItem>
</MenuItems>
```

**Pattern: Rich Template with Badges**
```cshtml
<MenuTemplates TValue="MenuItem">
    <Template>
        @context.Text
        <span class="badge">NEW</span>
    </Template>
</MenuTemplates>
```

