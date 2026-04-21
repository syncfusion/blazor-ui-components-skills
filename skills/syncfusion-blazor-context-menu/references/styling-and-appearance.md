# Styling and Appearance

## Table of Contents
- [CSS Class Reference](#css-class-reference)
- [Override Default Styles](#override-default-styles)
- [Focus and Selected States](#focus-and-selected-states)
- [Theme Customization](#theme-customization)
- [Common Styling Examples](#common-styling-examples)

## CSS Class Reference

The ContextMenu component uses these CSS classes for styling:

| CSS Class | Target | Purpose |
|---|---|---|
| `.e-contextmenu-container` | Container | Main wrapper for entire menu |
| `.e-contextmenu-container .e-menu-parent` | Parent menu | Top-level menu container |
| `.e-contextmenu-container ul .e-menu-item` | Menu items | Individual menu item styling |
| `.e-contextmenu-container ul .e-menu-item.e-focused` | Focused item | Item on keyboard focus or hover |
| `.e-contextmenu-container ul .e-menu-item.e-selected` | Selected item | Currently selected item |
| `.e-contextmenu-container ul .e-menu-item.e-disabled` | Disabled item | Disabled menu items (grayed out) |
| `.e-contextmenu-container ul .e-menu-item .e-menu-icon` | Icon area | Container for item icons |
| `.e-contextmenu-container ul .e-menu-item.e-selected .e-caret::before` | Caret icon | Submenu indicator icon |
| `.e-contextmenu-container .e-menu-parent.e-ul` | Submenu | Nested submenu list |

---

## Override Default Styles

Customize menu appearance by overriding default CSS classes.

### Change Background and Text Color

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem" CssClass="custom-dark">
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
    
    /* Dark theme override */
    .custom-dark.e-contextmenu-container {
        background-color: #2c3e50;
        border: 1px solid #34495e;
        border-radius: 4px;
    }
    
    .custom-dark.e-contextmenu-container .e-menu-item {
        color: #ecf0f1;
        padding: 8px 12px;
    }
    
    .custom-dark.e-contextmenu-container .e-menu-item:hover {
        background-color: #34495e;
    }
</style>
```

### Change Border and Padding

```cshtml
<style>
    .custom-menu.e-contextmenu-container {
        border: 2px solid #3498db;
        border-radius: 6px;
        box-shadow: 0 2px 8px rgba(0,0,0,0.15);
    }
    
    .custom-menu.e-contextmenu-container .e-menu-item {
        padding: 10px 16px;
        font-weight: 500;
    }
</style>
```

---

## Focus and Selected States

Customize how menu items appear when focused or selected.

### Highlight on Hover/Focus

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem" CssClass="highlight-focus">
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
    
    /* Focused item with left border accent */
    .highlight-focus.e-contextmenu-container .e-menu-item.e-focused {
        background-color: #fff3cd;
        border-left: 4px solid #ffc107;
        padding-left: 12px;
    }
    
    .highlight-focus.e-contextmenu-container .e-menu-item:hover {
        background-color: #f5f5f5;
    }
</style>
```

### Custom Selected Indicator

```cshtml
<style>
    .custom-selected.e-contextmenu-container .e-menu-item.e-selected {
        background-color: #2196F3;
        color: white;
        border-radius: 3px;
    }
    
    .custom-selected.e-contextmenu-container .e-menu-item.e-selected .e-caret::before {
        color: white;
    }
</style>
```

---

## Theme Customization

Syncfusion supports multiple built-in themes. Switch themes by changing the CSS file in `index.html`.

### Available Themes

**Bootstrap 5 (Default)**
```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```

**Material Design**
```html
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />
```

**Microsoft Fluent**
```html
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />
```

**Tailwind CSS**
```html
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />
```

### Theme-Specific Overrides

```cshtml
<style>
    /* Override for Bootstrap 5 theme */
    .e-contextmenu-container {
        /* Customization */
    }
    
    /* For Material theme, use matching color palette */
    .material .e-contextmenu-container .e-menu-item:hover {
        background-color: rgba(0, 0, 0, 0.04);
    }
</style>
```

---

## Common Styling Examples

### Example 1: Modern Minimal Style

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem" CssClass="minimal">
    <MenuItems>
        <MenuItem Text="Edit"></MenuItem>
        <MenuItem Text="Share"></MenuItem>
        <MenuItem Text="Delete"></MenuItem>
    </MenuItems>
</SfContextMenu>

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
    
    .minimal.e-contextmenu-container {
        background: white;
        border: 1px solid #e0e0e0;
        border-radius: 8px;
        box-shadow: 0 2px 8px rgba(0,0,0,0.1);
        padding: 4px 0;
    }
    
    .minimal.e-contextmenu-container .e-menu-item {
        padding: 8px 16px;
        font-size: 14px;
        color: #333;
        transition: background-color 0.2s ease;
    }
    
    .minimal.e-contextmenu-container .e-menu-item.e-focused {
        background-color: #f5f5f5;
        border-left: 3px solid #2196F3;
        padding-left: 13px;
    }
</style>
```

### Example 2: Colored Categories

```cshtml
<style>
    .category-colors.e-contextmenu-container .e-menu-item[data-category="edit"] {
        border-left: 3px solid #4CAF50;
    }
    
    .category-colors.e-contextmenu-container .e-menu-item[data-category="danger"] {
        border-left: 3px solid #f44336;
    }
    
    .category-colors.e-contextmenu-container .e-menu-item[data-category="action"] {
        border-left: 3px solid #2196F3;
    }
    
    .category-colors.e-contextmenu-container .e-menu-item.e-focused {
        background-color: #fafafa;
    }
</style>
```

### Example 3: Compact Menu (Mobile-friendly)

```cshtml
<style>
    .compact.e-contextmenu-container {
        min-width: 140px;
    }
    
    .compact.e-contextmenu-container .e-menu-item {
        padding: 6px 12px;
        font-size: 13px;
        min-height: 30px;
    }
    
    .compact.e-contextmenu-container .e-menu-item .e-icons {
        font-size: 12px;
        margin-right: 6px;
    }
</style>
```

### Example 4: High Contrast (Accessibility)

```cshtml
<style>
    .high-contrast.e-contextmenu-container {
        background-color: #000;
        border: 2px solid #fff;
    }
    
    .high-contrast.e-contextmenu-container .e-menu-item {
        color: #fff;
        padding: 10px 14px;
    }
    
    .high-contrast.e-contextmenu-container .e-menu-item.e-focused {
        background-color: #fff;
        color: #000;
    }
    
    .high-contrast.e-contextmenu-container .e-menu-item.e-disabled {
        color: #888;
    }
</style>
```

---

## CSS Variables (Advanced)

Some themes support CSS custom properties for dynamic theming:

```css
:root {
    --primary-color: #2196F3;
    --hover-color: #1976D2;
    --text-color: #333;
    --border-color: #e0e0e0;
}

.e-contextmenu-container {
    border-color: var(--border-color);
}

.e-contextmenu-container .e-menu-item.e-focused {
    background-color: var(--hover-color);
    color: white;
}
```

---

## Tips for Styling

1. **Specificity:** Use `.custom-class.e-contextmenu-container` to ensure override
2. **Hover states:** Include `:hover` and `.e-focused` for interactive feedback
3. **Icons:** Style `.e-icons` color and size separately
4. **Animations:** Add `transition` properties for smooth effects
5. **Accessibility:** Maintain sufficient color contrast (WCAG AA standard)

