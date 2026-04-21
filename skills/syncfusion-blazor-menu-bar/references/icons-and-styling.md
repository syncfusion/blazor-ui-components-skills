# Icons and Styling

## Table of Contents
- [Adding Icons to Menu Items](#adding-icons-to-menu-items)
- [Icon Positioning](#icon-positioning)
- [Custom CSS Icons](#custom-css-icons)
- [Separators and Disabled Items](#separators-and-disabled-items)
- [Navigation Links](#navigation-links)
- [CSS Customization](#css-customization)

## Adding Icons to Menu Items

### Using IconCss Property

The `IconCss` property adds an icon to a menu item by applying CSS classes:

```razor
<SfMenu TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="File" IconCss="e-icons e-file">
            <MenuItems>
                <MenuItem Text="Open" IconCss="e-icons e-open"></MenuItem>
                <MenuItem Text="Save" IconCss="e-icons e-save"></MenuItem>
            </MenuItems>
        </MenuItem>
        <MenuItem Text="Edit" IconCss="e-icons e-edit">
            <MenuItems>
                <MenuItem Text="Cut" IconCss="e-icons e-cut"></MenuItem>
                <MenuItem Text="Copy" IconCss="e-icons e-copy"></MenuItem>
                <MenuItem Text="Paste" IconCss="e-icons e-paste"></MenuItem>
            </MenuItems>
        </MenuItem>
    </MenuItems>
</SfMenu>
```

### Syncfusion Icon Library

Syncfusion provides a built-in icon library. Use `e-icons` class combined with specific icon classes:

**Common Icons:**
- `e-file` - File icon
- `e-open` - Open/folder icon
- `e-save` - Save/disk icon
- `e-cut` - Cut icon
- `e-copy` - Copy icon
- `e-paste` - Paste icon
- `e-edit` - Edit/pencil icon
- `e-delete` - Delete/trash icon
- `e-home` - Home icon
- `e-search` - Search/magnifying glass icon
- `e-settings` - Settings/gear icon
- `e-star` - Star icon
- `e-user` - User/person icon
- `e-print` - Print icon
- `e-download` - Download icon
- `e-upload` - Upload icon

### Icon Size and Styling

Control icon appearance with CSS classes:

```razor
<style>
    /* Make icons larger */
    .e-icons::before {
        font-size: 18px;
    }
    
    /* Custom icon color */
    .e-menu .e-menu-item.icon-red .e-icons::before {
        color: red;
    }
</style>

<SfMenu TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Critical" IconCss="e-icons e-delete icon-red"></MenuItem>
    </MenuItems>
</SfMenu>
```

## Icon Positioning

### Default Position (Left)

By default, icons appear to the left of text:

```razor
<MenuItem Text="File" IconCss="e-icons e-file"></MenuItem>
<!-- Output: [Icon] File -->
```

### Custom Icon Positioning with CSS

Position icons to the right or apply custom styling:

```razor
<style>
    /* Position icons to the right */
    .e-menu .e-menu-item.right-icon .e-icons {
        order: 2;
        margin-left: 10px;
        margin-right: 0;
    }
    
    .e-menu .e-menu-item.right-icon {
        flex-direction: row-reverse;
    }
</style>

<SfMenu TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Settings" IconCss="e-icons e-settings right-icon"></MenuItem>
    </MenuItems>
</SfMenu>
```

## Custom CSS Icons

### Creating Custom Icon Classes

Define custom icons using CSS content property:

```razor
<style>
    .custom-icon::before {
        content: '\e7cb';  /* Unicode character for icon */
        font-family: 'e_icons';
        font-size: 16px;
    }
    
    .e-home::before {
        content: '\e700';
    }
    
    .e-file::before {
        content: '\e7cb';
    }
    
    .e-edit::before {
        content: '\e78f';
    }
    
    .e-open::before {
        content: '\e70f';
    }
    
    .e-save::before {
        content: '\e74d';
    }
    
    .e-cut::before {
        content: '\e73f';
    }
    
    .e-copy::before {
        content: '\e77b';
    }
    
    .e-paste::before {
        content: '\e739';
    }
    
    .e-delete::before {
        content: '\e71d';
    }
</style>

<SfMenu TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="File" IconCss="e-file"></MenuItem>
        <MenuItem Text="Edit" IconCss="e-edit"></MenuItem>
        <MenuItem Text="Delete" IconCss="e-delete"></MenuItem>
    </MenuItems>
</SfMenu>
```

### Using Font Awesome Icons

Integrate Font Awesome for extended icon choices:

```html
<!-- Include Font Awesome in index.html -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
```

```razor
<SfMenu TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Home" IconCss="fas fa-home"></MenuItem>
        <MenuItem Text="Settings" IconCss="fas fa-cog"></MenuItem>
        <MenuItem Text="Users" IconCss="fas fa-users"></MenuItem>
        <MenuItem Text="Logout" IconCss="fas fa-sign-out-alt"></MenuItem>
    </MenuItems>
</SfMenu>
```

## Separators and Disabled Items

### Adding Separators

Separators are visual dividers between menu items:

```razor
<SfMenu TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="New"></MenuItem>
        <MenuItem Text="Open"></MenuItem>
        <MenuItem Text="Save"></MenuItem>
        <MenuItem Separator="true"></MenuItem>
        <MenuItem Text="Exit"></MenuItem>
    </MenuItems>
</SfMenu>
```

### Disabling Menu Items

Disable items that should not be interactive:

```razor
<SfMenu TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Undo"></MenuItem>
        <MenuItem Text="Redo" Disabled="true"></MenuItem>
        <MenuItem Separator="true"></MenuItem>
        <MenuItem Text="Cut" Disabled="@(!isTextSelected)"></MenuItem>
        <MenuItem Text="Copy"></MenuItem>
        <MenuItem Text="Paste"></MenuItem>
    </MenuItems>
</SfMenu>

@code {
    private bool isTextSelected = false;
}
```

### Conditional Disabling

```razor
<SfMenu Items="@MenuList">
    <MenuFieldSettings Text="Text" Children="SubItems"></MenuFieldSettings>
</SfMenu>

@code {
    public class MenuModel
    {
        public string Text { get; set; }
        public bool Disabled { get; set; }
        public List<MenuModel> SubItems { get; set; }
    }

    public List<MenuModel> MenuList = new List<MenuModel>
    {
        new MenuModel { Text = "File", SubItems = new List<MenuModel>
        {
            new MenuModel { Text = "Open", Disabled = false },
            new MenuModel { Text = "Recent", Disabled = true }  // Disabled
        }}
    };
}
```

## Navigation Links

### Adding URLs to Menu Items

Use the `Url` property to make menu items navigate to different pages:

```razor
<SfMenu TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Products" Url="/products"></MenuItem>
        <MenuItem Text="Services" Url="/services"></MenuItem>
        <MenuItem Text="About">
            <MenuItems>
                <MenuItem Text="Company" Url="/about/company"></MenuItem>
                <MenuItem Text="Team" Url="/about/team"></MenuItem>
            </MenuItems>
        </MenuItem>
    </MenuItems>
</SfMenu>
```

### External Links

```razor
<SfMenu TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Documentation" Url="https://docs.example.com" Target="_blank"></MenuItem>
        <MenuItem Text="GitHub" Url="https://github.com/example" Target="_blank"></MenuItem>
    </MenuItems>
</SfMenu>
```

### Programmatic Navigation

Combine Url with event handling for custom navigation:

```razor
<SfMenu TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Dashboard"></MenuItem>
        <MenuItem Text="Reports"></MenuItem>
    </MenuItems>
    <MenuEvents TValue="MenuItem" ItemSelected="OnSelectItem"></MenuEvents>
</SfMenu>

@inject NavigationManager NavigationManager

@code {
    private void OnSelectItem(MenuEventArgs<MenuItem> args)
    {
        if (!string.IsNullOrEmpty(args.Item.Url))
        {
            NavigationManager.NavigateTo(args.Item.Url);
        }
    }
}
```

## CSS Customization

### Custom Menu Styling

```razor
<style>
    /* Custom menu background */
    .e-menu {
        background-color: #f0f0f0;
    }
    
    /* Custom menu item styling */
    .e-menu .e-menu-item {
        padding: 12px 20px;
        border-radius: 4px;
    }
    
    /* Hover effect */
    .e-menu .e-menu-item:hover {
        background-color: #e0e0e0;
        transition: background-color 0.3s ease;
    }
    
    /* Active/selected item */
    .e-menu .e-menu-item.e-selected {
        background-color: #007bff;
        color: white;
    }
    
    /* Icon spacing */
    .e-menu .e-menu-item .e-icons {
        margin-right: 8px;
    }
    
    /* Submenu styling */
    .e-menu.e-contextmenu,
    .e-menu .e-menu {
        background-color: white;
        border: 1px solid #ddd;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
    }
</style>

<SfMenu TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="File" IconCss="e-file"></MenuItem>
        <MenuItem Text="Edit" IconCss="e-edit"></MenuItem>
    </MenuItems>
</SfMenu>
```

### Theme Variables

Customize colors using CSS variables:

```razor
<style>
    :root {
        --menu-bg: #f5f5f5;
        --menu-hover-bg: #e8e8e8;
        --menu-text: #333;
        --menu-icon-size: 16px;
    }
    
    .e-menu {
        background-color: var(--menu-bg);
        color: var(--menu-text);
    }
    
    .e-menu .e-menu-item:hover {
        background-color: var(--menu-hover-bg);
    }
    
    .e-menu .e-icons::before {
        font-size: var(--menu-icon-size);
    }
</style>
```

### Dark Mode Support

```razor
<style>
    @media (prefers-color-scheme: dark) {
        .e-menu {
            background-color: #2d2d2d;
            color: #f0f0f0;
        }
        
        .e-menu .e-menu-item:hover {
            background-color: #404040;
        }
        
        .e-menu.e-contextmenu,
        .e-menu .e-menu {
            background-color: #1e1e1e;
            border-color: #444;
        }
    }
</style>
```

## Best Practices

1. **Consistent Icon Usage**: Use icons consistently across your menu
2. **Icon Clarity**: Choose clear, recognizable icons
3. **Color Contrast**: Ensure adequate contrast between text and background
4. **Accessibility**: Provide text labels alongside icons
5. **Performance**: Use icon fonts rather than individual SVG/image files
6. **Responsive Design**: Test icon display on different screen sizes
7. **Separation**: Use separators to group related menu items logically
