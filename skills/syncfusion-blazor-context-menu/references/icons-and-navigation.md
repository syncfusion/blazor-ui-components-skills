# Icons and Navigation

## Table of Contents
- [Adding Icons with IconCss](#adding-icons-with-iconcss)
- [Using Syncfusion e-icons](#using-syncfusion-e-icons)
- [Custom Icon Classes](#custom-icon-classes)
- [Navigation with Url Property](#navigation-with-url-property)
- [Icon Positioning](#icon-positioning)

## Adding Icons with IconCss

Use the `IconCss` property to add icons to menu items using CSS class names.

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Cut" IconCss="e-icons e-cut"></MenuItem>
        <MenuItem Text="Copy" IconCss="e-icons e-copy"></MenuItem>
        <MenuItem Text="Paste" IconCss="e-icons e-paste"></MenuItem>
    </MenuItems>
</SfContextMenu>

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
    
    /* Define custom icon glyphs */
    .e-cut::before {
        content: '\e279';
    }
    .e-copy::before {
        content: '\e280';
    }
    .e-paste::before {
        content: '\e601';
    }
</style>
```

**Key Points:**
- `IconCss="e-icons e-cut"` - Combines e-icons base class with specific icon class
- `e-icons` - Base class that activates icon rendering
- Icon-specific class (e.g., `e-cut`) - Maps to Unicode glyph via `::before` content

---

## Using Syncfusion e-icons

Syncfusion provides a built-in icon library. Common icons for ContextMenu:

| Icon Class | Glyph | Use Case |
|---|---|---|
| `e-cut` | ✂️ | Cut action |
| `e-copy` | 📋 | Copy action |
| `e-paste` | 📄 | Paste action |
| `e-edit` | ✏️ | Edit/modify |
| `e-delete` | 🗑️ | Delete action |
| `e-save` | 💾 | Save action |
| `e-refresh` | 🔄 | Refresh/reload |
| `e-search` | 🔍 | Search |
| `e-download` | ⬇️ | Download |
| `e-upload` | ⬆️ | Upload |

### Full Icons Example

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Edit" IconCss="e-icons e-edit"></MenuItem>
        <MenuItem Text="Delete" IconCss="e-icons e-delete"></MenuItem>
        <MenuItem Text="Save" IconCss="e-icons e-save"></MenuItem>
        <MenuItem Text="Refresh" IconCss="e-icons e-refresh"></MenuItem>
    </MenuItems>
</SfContextMenu>

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
    
    /* Custom colors for icons */
    .e-contextmenu-container .e-menu-item .e-icons {
        color: #555;
        margin-right: 8px;
    }
</style>
```

---

## Custom Icon Classes

Use third-party icon libraries (FontAwesome, Material Icons, etc.) with custom CSS classes.

### FontAwesome Icons Example

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Edit" IconCss="fa fa-edit"></MenuItem>
        <MenuItem Text="Delete" IconCss="fa fa-trash"></MenuItem>
        <MenuItem Text="Download" IconCss="fa fa-download"></MenuItem>
    </MenuItems>
</SfContextMenu>

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
    
    /* Assuming FontAwesome CSS is loaded via CDN or npm */
</style>
```

**Steps to use custom icons:**
1. Include icon library CSS in `index.html` (e.g., FontAwesome CDN)
2. Reference icon class in `IconCss` (e.g., `fa fa-edit`)
3. Syncfusion applies the class to the icon element
4. Browser renders icon from library

---

## Navigation with Url Property

Use the `Url` property to navigate to other pages when menu item is clicked.

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Syncfusion" Url="https://www.syncfusion.com"></MenuItem>
        <MenuItem Text="GitHub" Url="https://www.github.com"></MenuItem>
        <MenuItem Text="Stack Overflow" Url="https://stackoverflow.com"></MenuItem>
    </MenuItems>
</SfContextMenu>

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

**Behavior:**
- Clicking menu item with `Url` navigates to specified URL
- Default target is `_self` (opens in current tab)
- Useful for external links or page navigation

### Navigation with Icons

```cshtml
<SfContextMenu Target="#target" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Home" Url="/home" IconCss="e-icons e-home"></MenuItem>
        <MenuItem Text="About" Url="/about" IconCss="e-icons e-info"></MenuItem>
        <MenuItem Text="Contact" Url="/contact" IconCss="e-icons e-mail"></MenuItem>
    </MenuItems>
</SfContextMenu>
```

---

## Icon Positioning

By default, icons appear **left of text**. Customize position with CSS.

### Left-Aligned Icons (Default)

```cshtml
<MenuItem Text="Edit" IconCss="e-icons e-edit"></MenuItem>
```

Result: `[Icon] Edit`

### Right-Aligned Icons

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem" CssClass="right-icons">
    <MenuItems>
        <MenuItem Text="Edit" IconCss="e-icons e-edit"></MenuItem>
        <MenuItem Text="Delete" IconCss="e-icons e-delete"></MenuItem>
    </MenuItems>
</SfContextMenu>

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
    
    .right-icons.e-contextmenu-container .e-menu-item {
        flex-direction: row-reverse;
    }
    
    .right-icons.e-contextmenu-container .e-menu-item .e-icons {
        margin-left: 8px;
        margin-right: 0;
    }
</style>
```

Result: `Edit [Icon]`

### Custom Icon Colors

```cshtml
<style>
    .e-contextmenu-container .e-menu-item .e-icons {
        color: #2196F3; /* Blue */
    }
    
    .e-contextmenu-container .e-menu-item:hover .e-icons {
        color: #FF5722; /* Orange on hover */
    }
</style>
```

---

## Common Patterns

**Pattern: File Context Menu with Icons**
```cshtml
<MenuItems>
    <MenuItem Text="Cut" IconCss="e-icons e-cut"></MenuItem>
    <MenuItem Text="Copy" IconCss="e-icons e-copy"></MenuItem>
    <MenuItem Text="Delete" IconCss="e-icons e-delete"></MenuItem>
    <MenuItem Separator="true"></MenuItem>
    <MenuItem Text="Properties" IconCss="e-icons e-settings"></MenuItem>
</MenuItems>
```

**Pattern: Navigation Menu with Icons**
```cshtml
<MenuItems>
    <MenuItem Text="Dashboard" Url="/dashboard" IconCss="e-icons e-home"></MenuItem>
    <MenuItem Text="Reports" Url="/reports" IconCss="e-icons e-chart"></MenuItem>
    <MenuItem Text="Settings" Url="/settings" IconCss="e-icons e-settings"></MenuItem>
</MenuItems>
```

**Pattern: Mixed Icons and Text**
```cshtml
<MenuItems>
    <MenuItem Text="Edit Profile" IconCss="e-icons e-edit"></MenuItem>
    <MenuItem Text="Preferences"></MenuItem>
    <MenuItem Text="Logout" IconCss="e-icons e-exit"></MenuItem>
</MenuItems>
```

