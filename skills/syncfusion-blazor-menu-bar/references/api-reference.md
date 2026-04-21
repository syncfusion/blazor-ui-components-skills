# Syncfusion Blazor Menu Bar API Reference

## Overview

The Syncfusion Blazor Menu Bar (`SfMenu`) component provides a flexible, hierarchical navigation menu system with support for data binding, events, animations, and extensive customization options. It enables developers to create horizontal and vertical menus with nested submenus, icons, and dynamic item management.

---

## SfMenu Component

### Properties

| Property | Type | Default | Accepted Values | Description | Reference |
|----------|------|---------|-----------------|-------------|-----------|
| `TValue` | Generic Type | - | Any | Generic type parameter for menu items | [View](#) |
| `Items` | `IEnumerable<TValue>` | null | List of items | Data source for the menu items | [View](#) |
| `Orientation` | `Orientation` | Horizontal | Horizontal, Vertical | Menu layout direction | [Orientation Enum](#orientation-enum) |
| `ShowItemOnClick` | `bool` | false | true, false | Open submenus on click instead of hover | [View](#) |
| `EnableRtl` | `bool` | false | true, false | Enable right-to-left (RTL) layout for Arabic/Hebrew languages | [View](#) |
| `MenuAnimationSettings` | `MenuAnimationSettings` | - | - | Animation configuration for submenu display | [MenuAnimationSettings](#menuanimationsettings-component) |
| `CssClass` | `string` | null | CSS class names | Custom CSS classes to apply to the menu | [View](#) |
| `HtmlAttributes` | `Dictionary<string, object>` | null | - | Additional HTML attributes for the component | [View](#) |

### Methods

| Method | Parameters | Return Type | Description | Reference |
|--------|------------|-------------|-------------|-----------|
| `Add` | `TValue item` | `void` | Add a new menu item to the menu at runtime | See [Dynamic Item Management](#dynamic-item-management) |
| `Remove` | `TValue item` | `void` | Remove a menu item from the menu | See [Dynamic Item Management](#dynamic-item-management) |
| `RefreshAsync` | `(none)` | `Task` | Refresh the menu component and re-render all items | [View](#) |

**Example - Dynamic Item Management:**
```csharp
<SfMenu Items="@MenuItems" @ref="MenuRef">
    <MenuFieldSettings Text="Text" Children="SubItems"></MenuFieldSettings>
</SfMenu>

@code {
    SfMenu<MenuItem> MenuRef;
    
    // Add item
    private void AddItem()
    {
        MenuRef.Items.Add(new MenuItem { Text = "New Item" });
    }
    
    // Remove item
    private void RemoveItem(MenuItem item)
    {
        MenuRef.Items.Remove(item);
    }
}
```

---

## MenuItem Component

### Properties

| Property | Type | Default | Accepted Values | Description | Reference |
|----------|------|---------|-----------------|-------------|-----------|
| `Text` | `string` | null | Any string | Display text for the menu item | [View](#) |
| `IconCss` | `string` | null | CSS class names | Icon CSS class to display before/after the text | [Icons Reference](icons-and-styling.md) |
| `Url` | `string` | null | Valid URL path | Navigation URL for the menu item | [View](#) |
| `Separator` | `bool` | false | true, false | Display a horizontal separator line | [View](#) |
| `Disabled` | `bool` | false | true, false | Disable the menu item from interaction | [View](#) |
| `Items` | `IEnumerable<MenuItem>` | null | List of MenuItem | Child menu items (submenus) | [View](#) |

**Example - MenuItem Usage:**
```csharp
<SfMenu TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="File" IconCss="e-icons e-file">
            <MenuItems>
                <MenuItem Text="Open" IconCss="e-icons e-open"></MenuItem>
                <MenuItem Text="Save" IconCss="e-icons e-save"></MenuItem>
                <MenuItem Separator="true"></MenuItem>
                <MenuItem Text="Exit"></MenuItem>
            </MenuItems>
        </MenuItem>
    </MenuItems>
</SfMenu>
```

---

## MenuFieldSettings Component

### Properties

| Property | Type | Default | Accepted Values | Description | Reference |
|----------|------|---------|-----------------|-------------|-----------|
| `ItemId` | `string` | "Id" | Property name | Maps the unique identifier field of data source | [Data Binding Reference](menu-items-and-data-binding.md) |
| `Text` | `string` | "Text" | Property name | Maps the text field of data source for display | [View](#) |
| `ParentId` | `string` | "ParentId" | Property name | Maps the parent ID field for hierarchical data | [View](#) |
| `Children` | `string` | "Children" | Property name | Maps the child collection property name | [View](#) |
| `Url` | `string` | "Url" | Property name | Maps the URL navigation property | [View](#) |
| `IconCss` | `string` | "IconCss" | Property name | Maps the icon CSS class property | [View](#) |
| `Disabled` | `string` | "Disabled" | Property name | Maps the disabled state property | [View](#) |
| `Separator` | `string` | "Separator" | Property name | Maps the separator indicator property | [View](#) |

**Example - Self-Referential Data Binding:**
```razor
<SfMenu Items="@MenuItems">
    <MenuFieldSettings ItemId="Id" Text="Text" ParentId="ParentId"></MenuFieldSettings>
</SfMenu>

@code {
    public class MenuItem
    {
        public string Id { get; set; }
        public string Text { get; set; }
        public string ParentId { get; set; }
    }
    
    public List<MenuItem> MenuItems = new List<MenuItem>
    {
        new MenuItem { Id = "1", Text = "File", ParentId = null },
        new MenuItem { Id = "2", Text = "Open", ParentId = "1" },
        new MenuItem { Id = "3", Text = "Save", ParentId = "1" }
    };
}
```

---

## MenuAnimationSettings Component

### Properties

| Property | Type | Default | Accepted Values | Description | Reference |
|----------|------|---------|-----------------|-------------|-----------|
| `Effect` | `MenuEffect` | SlideDown | None, SlideDown, ZoomIn, FadeIn | Animation effect when opening/closing submenus | [MenuEffect Enum](#menueefect-enum) |
| `Duration` | `double` | 400 | 0-∞ | Animation duration in milliseconds | [View](#) |

**Example - Animation Configuration:**
```razor
<SfMenu TValue="MenuItem">
    <MenuAnimationSettings Effect="MenuEffect.ZoomIn" Duration="600"></MenuAnimationSettings>
    <MenuItems>
        <!-- Menu items -->
    </MenuItems>
</SfMenu>
```

---

## MenuEffect Enum

### Fields

| Field | Description | Value |
|-------|-------------|-------|
| `None` | No animation effect; menu appears instantly | 0 |
| `SlideDown` | Menu slides down when opening | 1 |
| `ZoomIn` | Menu zooms in from center | 2 |
| `FadeIn` | Menu fades in gradually | 3 |

---

## Orientation Enum

### Fields

| Field | Description | Value |
|-------|-------------|-------|
| `Horizontal` | Menu items display horizontally (left to right) | 0 |
| `Vertical` | Menu items stack vertically (top to bottom) | 1 |

**Example - Orientation Usage:**
```razor
<!-- Horizontal (default) -->
<SfMenu TValue="MenuItem" Orientation="Orientation.Horizontal">
    <!-- Menu items -->
</SfMenu>

<!-- Vertical -->
<SfMenu TValue="MenuItem" Orientation="Orientation.Vertical">
    <!-- Menu items -->
</SfMenu>
```

---

## MenuEvents Component

### Properties

| Property | Type | Description | Reference |
|----------|------|-------------|-----------|
| `Created` | `EventCallback` | Fires after the Menu Bar is successfully created and initialized | [View](#) |
| `OnItemRender` | `EventCallback<MenuEventArgs<TValue>>` | Fires while rendering each menu item; can be used to customize items | [View](#) |
| `OnOpen` | `EventCallback<MenuEventArgs<TValue>>` | Fires before opening a submenu; can be cancelled | [View](#) |
| `OnClose` | `EventCallback<MenuEventArgs<TValue>>` | Fires before closing a submenu; can be cancelled | [View](#) |
| `Opened` | `EventCallback<MenuEventArgs<TValue>>` | Fires after a submenu has been successfully opened | [View](#) |
| `Closed` | `EventCallback<MenuEventArgs<TValue>>` | Fires after a submenu has been successfully closed | [View](#) |
| `ItemSelected` | `EventCallback<MenuEventArgs<TValue>>` | Fires when a menu item is selected/clicked by the user | [View](#) |

**Example - Event Handling:**
```razor
<SfMenu TValue="MenuItem">
    <MenuItems>
        <!-- Menu items -->
    </MenuItems>
    <MenuEvents TValue="MenuItem" 
        Created="OnCreated"
        ItemSelected="OnItemSelected"
        OnOpen="OnOpen"
        Opened="OnOpened"
        OnClose="OnClose"
        Closed="OnClosed">
    </MenuEvents>
</SfMenu>

@code {
    private void OnCreated()
    {
        Console.WriteLine("Menu created");
    }
    
    private void OnItemSelected(MenuEventArgs<MenuItem> args)
    {
        Console.WriteLine($"Selected: {args.Item.Text}");
    }
    
    private void OnOpen(MenuEventArgs<MenuItem> args)
    {
        // args.Cancel = true; // Prevent opening if needed
        Console.WriteLine($"Opening: {args.Item.Text}");
    }
    
    private void OnOpened(MenuEventArgs<MenuItem> args)
    {
        Console.WriteLine($"Opened: {args.Item.Text}");
    }
    
    private void OnClose(MenuEventArgs<MenuItem> args)
    {
        Console.WriteLine($"Closing: {args.Item.Text}");
    }
    
    private void OnClosed(MenuEventArgs<MenuItem> args)
    {
        Console.WriteLine($"Closed: {args.Item.Text}");
    }
}
```

---

## MenuEventArgs Component

### Properties

| Property | Type | Description | Reference |
|----------|------|-------------|-----------|
| `Item` | `TValue` | The menu item that triggered the event | [View](#) |
| `Cancel` | `bool` | Set to `true` to cancel the operation (valid for OnOpen and OnClose events) | [View](#) |

**Example - Using MenuEventArgs:**
```csharp
private void OnOpen(MenuEventArgs<MenuItem> args)
{
    var selectedItem = args.Item;
    var itemText = selectedItem.Text;
    
    // Prevent opening if item is locked
    if (selectedItem.Disabled)
    {
        args.Cancel = true;
    }
}
```

---

## MenuTemplates Component

### Properties

| Property | Type | Description | Reference |
|----------|------|-------------|-----------|
| `Template` | `RenderFragment<TValue>` | Custom template for rendering menu items | [View](#) |

**Example - Custom Menu Item Template:**
```razor
<SfMenu Items="@MenuItems">
    <MenuFieldSettings Text="Text" Children="SubItems"></MenuFieldSettings>
    <MenuTemplates TValue="MenuItem">
        <Template>
            @{
                var item = context as MenuItem;
                <div style="display: flex; justify-content: space-between; align-items: center;">
                    <span>@item.Text</span>
                    @if (item.HasBadge)
                    {
                        <span style="background: red; color: white; padding: 2px 6px; border-radius: 3px;">
                            @item.BadgeCount
                        </span>
                    }
                </div>
            }
        </Template>
    </MenuTemplates>
</SfMenu>
```

---

## Complete Integration Example

```razor
@page "/menu-example"
@using Syncfusion.Blazor.Navigations

<SfMenu TValue="MenuItemModel" 
    Items="@MenuItems"
    Orientation="Orientation.Horizontal"
    ShowItemOnClick="false"
    EnableRtl="false"
    @ref="MenuRef">
    
    <MenuAnimationSettings Effect="MenuEffect.SlideDown" Duration="400"></MenuAnimationSettings>
    
    <MenuFieldSettings 
        ItemId="Id"
        Text="Text"
        IconCss="IconClass"
        ParentId="ParentId"
        Children="SubItems">
    </MenuFieldSettings>
    
    <MenuEvents TValue="MenuItemModel"
        Created="OnCreated"
        OnItemRender="OnItemRender"
        OnOpen="OnOpen"
        OnClose="OnClose"
        Opened="OnOpened"
        Closed="OnClosed"
        ItemSelected="OnItemSelected">
    </MenuEvents>
</SfMenu>

@code {
    SfMenu<MenuItemModel> MenuRef;
    
    public class MenuItemModel
    {
        public string Id { get; set; }
        public string Text { get; set; }
        public string IconClass { get; set; }
        public string ParentId { get; set; }
        public List<MenuItemModel> SubItems { get; set; }
    }
    
    public List<MenuItemModel> MenuItems = new List<MenuItemModel>
    {
        new MenuItemModel 
        { 
            Id = "1", 
            Text = "File", 
            IconClass = "e-icons e-file",
            ParentId = null,
            SubItems = new List<MenuItemModel>
            {
                new MenuItemModel { Id = "2", Text = "Open", ParentId = "1" },
                new MenuItemModel { Id = "3", Text = "Save", ParentId = "1" }
            }
        }
    };
    
    private void OnCreated()
    {
        Console.WriteLine("Menu initialized");
    }
    
    private void OnItemRender(MenuEventArgs<MenuItemModel> args)
    {
        Console.WriteLine($"Rendering: {args.Item.Text}");
    }
    
    private void OnOpen(MenuEventArgs<MenuItemModel> args)
    {
        Console.WriteLine($"Opening: {args.Item.Text}");
    }
    
    private void OnClose(MenuEventArgs<MenuItemModel> args)
    {
        Console.WriteLine($"Closing: {args.Item.Text}");
    }
    
    private void OnOpened(MenuEventArgs<MenuItemModel> args)
    {
        Console.WriteLine($"Opened: {args.Item.Text}");
    }
    
    private void OnClosed(MenuEventArgs<MenuItemModel> args)
    {
        Console.WriteLine($"Closed: {args.Item.Text}");
    }
    
    private void OnItemSelected(MenuEventArgs<MenuItemModel> args)
    {
        Console.WriteLine($"Selected: {args.Item.Text}");
    }
}
```

---

## Additional Resources

- [Getting Started Reference](getting-started.md)
- [Menu Items and Data Binding](menu-items-and-data-binding.md)
- [Events and Interactions](events-and-interactions.md)
- [Icons and Styling](icons-and-styling.md)
- [Animation and Orientation](animation-and-orientation.md)
- [Advanced Scenarios](advanced-scenarios.md)

---

## Notes

- **TValue Required**: The `TValue` parameter must be specified for all event handlers in `MenuEvents`
- **MenuFieldSettings**: Use custom property mappings when your data model doesn't follow the default naming conventions
- **Performance**: For large menu structures, consider lazy-loading submenus with the `OnOpen` event
- **Accessibility**: Ensure keyboard navigation works by properly structuring menu hierarchy
- **RTL Support**: The `EnableRtl` property automatically mirrors the menu layout for RTL languages
