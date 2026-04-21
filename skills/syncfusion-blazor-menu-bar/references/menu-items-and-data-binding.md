# Menu Items and Data Binding

## Table of Contents
- [MenuItem Component](#menuitem-component)
- [Basic Menu Structure](#basic-menu-structure)
- [Hardcoded vs Data-Bound Menus](#hardcoded-vs-data-bound-menus)
- [Self-Referential Data Binding](#self-referential-data-binding)
- [MenuFieldSettings Configuration](#menufieldsettings-configuration)
- [Custom Data Models](#custom-data-models)
- [Multilevel Nesting](#multilevel-nesting)

## MenuItem Component

The `MenuItem` component represents a single menu item. Key properties include:

| Property | Type | Description |
|----------|------|-------------|
| `Text` | string | Display text for the menu item |
| `Url` | string | Navigation URL for the menu item |
| `IconCss` | string | CSS class for icon display |
| `Separator` | bool | Display a separator line below item |
| `Disabled` | bool | Disable the menu item |
| `Items` | IEnumerable<MenuItem> | Child menu items (submenus) |

### Basic MenuItem Example

```razor
<MenuItem Text="File" IconCss="e-icons e-file">
    <MenuItems>
        <MenuItem Text="Open"></MenuItem>
        <MenuItem Text="Save"></MenuItem>
    </MenuItems>
</MenuItem>
```

## Basic Menu Structure

### Creating a Hardcoded Menu

For simple, static menus, define MenuItem components directly:

```razor
<SfMenu TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Home"></MenuItem>
        <MenuItem Text="About">
            <MenuItems>
                <MenuItem Text="Company"></MenuItem>
                <MenuItem Text="Team"></MenuItem>
                <MenuItem Text="Careers"></MenuItem>
            </MenuItems>
        </MenuItem>
        <MenuItem Text="Services">
            <MenuItems>
                <MenuItem Text="Consulting"></MenuItem>
                <MenuItem Text="Development"></MenuItem>
                <MenuItem Text="Support"></MenuItem>
            </MenuItems>
        </MenuItem>
        <MenuItem Text="Contact"></MenuItem>
    </MenuItems>
</SfMenu>
```

### MenuItem Hierarchy

Menus support unlimited nesting levels. Each `MenuItem` can contain its own `MenuItems` collection:

```razor
<MenuItem Text="Products">
    <MenuItems>
        <MenuItem Text="Category A">
            <MenuItems>
                <MenuItem Text="Product A1"></MenuItem>
                <MenuItem Text="Product A2">
                    <MenuItems>
                        <MenuItem Text="Variant A2-1"></MenuItem>
                        <MenuItem Text="Variant A2-2"></MenuItem>
                    </MenuItems>
                </MenuItem>
            </MenuItems>
        </MenuItem>
        <MenuItem Text="Category B"></MenuItem>
    </MenuItems>
</MenuItem>
```

## Hardcoded vs Data-Bound Menus

### When to Use Hardcoded Menus

**Advantages:**
- Simple implementation for small menus
- No database queries or external data needed
- Directly visible in component markup
- Suitable for navigation menus that rarely change

**Example:**
```razor
<SfMenu TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="File">
            <MenuItems>
                <MenuItem Text="New"></MenuItem>
                <MenuItem Text="Open"></MenuItem>
            </MenuItems>
        </MenuItem>
    </MenuItems>
</SfMenu>
```

### When to Use Data-Bound Menus

**Advantages:**
- Dynamic menu generation from lists, databases, or APIs
- Data-driven updates without code changes
- Suitable for large menu structures
- Supports filtering and transformations

**Example:**
```razor
<SfMenu Items="@MenuList">
    <MenuFieldSettings Text="@nameof(MenuModel.Label)" Children="@nameof(MenuModel.SubItems)"></MenuFieldSettings>
</SfMenu>

@code {
    public List<MenuModel> MenuList = new List<MenuModel>
    {
        new MenuModel { Label = "Home" },
        new MenuModel { Label = "Products", SubItems = new List<MenuModel> { ... } }
    };
}
```

## Self-Referential Data Binding

Self-referential data structures use a flat list with ParentId relationships to build hierarchies.

### Data Model

```csharp
public class MenuItem
{
    public string Id { get; set; }
    public string Text { get; set; }
    public string ParentId { get; set; }  // Links to parent's Id
}
```

### Self-Referential Binding Example

```razor
@using Syncfusion.Blazor.Navigations

<SfMenu Items="@MenuItems">
    <MenuFieldSettings ItemId="Id" Text="Text" ParentId="ParentId"></MenuFieldSettings>
</SfMenu>

@code {
    public List<MenuItem> MenuItems = new List<MenuItem>
    {
        new MenuItem { Id = "1", Text = "Events", ParentId = null },
        new MenuItem { Id = "2", Text = "Movies", ParentId = null },
        new MenuItem { Id = "3", Text = "Directory", ParentId = null },
        
        // Submenus under Events (ParentId = "1")
        new MenuItem { Id = "4", Text = "Conferences", ParentId = "1" },
        new MenuItem { Id = "5", Text = "Music", ParentId = "1" },
        new MenuItem { Id = "6", Text = "Workshops", ParentId = "1" },
        
        // Submenus under Movies (ParentId = "2")
        new MenuItem { Id = "7", Text = "Now Showing", ParentId = "2" },
        new MenuItem { Id = "8", Text = "Coming Soon", ParentId = "2" },
        
        // Sub-submenus under Music (ParentId = "5")
        new MenuItem { Id = "9", Text = "Pop", ParentId = "5" },
        new MenuItem { Id = "10", Text = "Folk", ParentId = "5" },
        new MenuItem { Id = "11", Text = "Classical", ParentId = "5" }
    };

    public class MenuItem
    {
        public string Id { get; set; }
        public string Text { get; set; }
        public string ParentId { get; set; }
    }
}
```

### Advantages of Self-Referential Data

- Stores hierarchical data in a single flat list
- Easy to save to databases (single table)
- Flexible parent-child relationships
- Supports unlimited nesting levels

## MenuFieldSettings Configuration

The `MenuFieldSettings` component maps data source properties to Menu Bar fields:

```razor
<MenuFieldSettings 
    ItemId="Id"
    Text="Label"
    ParentId="ParentId"
    Children="SubMenu"
    Url="Link"
    IconCss="Icon"
    Separator="IsSeparator">
</MenuFieldSettings>
```

### Common Property Mappings

| Setting | Purpose | Data Property |
|---------|---------|---------------|
| `ItemId` | Unique identifier | Id, ItemId |
| `Text` | Display label | Text, Label, Name |
| `ParentId` | Parent relationship | ParentId, Parent, PId |
| `Children` | Nested items collection | Items, SubMenu, SubItems |
| `Url` | Navigation link | Url, Link, NavigationUrl |
| `IconCss` | Icon CSS class | IconCss, Icon, CssClass |
| `Separator` | Separator indicator | Separator, IsSeparator |

### Example: Mapping Custom Properties

```csharp
public class CustomMenuData
{
    public int ItemKey { get; set; }      // Maps to ItemId
    public string Label { get; set; }     // Maps to Text
    public int? ParentKey { get; set; }   // Maps to ParentId
    public List<CustomMenuData> ChildItems { get; set; }  // Maps to Children
    public string NavigationPath { get; set; }  // Maps to Url
}
```

```razor
<MenuFieldSettings 
    ItemId="ItemKey"
    Text="Label"
    ParentId="ParentKey"
    Children="ChildItems"
    Url="NavigationPath">
</MenuFieldSettings>
```

## Custom Data Models

Create custom models that match your data structure:

```csharp
public class CategoryMenu
{
    public string CategoryId { get; set; }
    public string CategoryName { get; set; }
    public string CategoryIcon { get; set; }
    public List<ProductMenu> Products { get; set; }
}

public class ProductMenu
{
    public string ProductId { get; set; }
    public string ProductName { get; set; }
    public string ProductUrl { get; set; }
}
```

### Using Custom Models with Hierarchical Data

```razor
<SfMenu Items="@Categories">
    <MenuFieldSettings 
        Text="CategoryName" 
        IconCss="CategoryIcon"
        Children="Products">
    </MenuFieldSettings>
</SfMenu>

@code {
    public List<CategoryMenu> Categories = new List<CategoryMenu>
    {
        new CategoryMenu
        {
            CategoryId = "1",
            CategoryName = "Electronics",
            CategoryIcon = "e-icons e-gadget",
            Products = new List<ProductMenu>
            {
                new ProductMenu { ProductId = "1", ProductName = "Laptop", ProductUrl = "/products/1" },
                new ProductMenu { ProductId = "2", ProductName = "Phone", ProductUrl = "/products/2" }
            }
        }
    };
}
```

## Multilevel Nesting

The Menu Bar supports unlimited nesting levels:

```razor
<SfMenu Items="@MenuData">
    <MenuFieldSettings Text="Name" Children="SubMenus"></MenuFieldSettings>
</SfMenu>

@code {
    public class MenuNode
    {
        public string Name { get; set; }
        public List<MenuNode> SubMenus { get; set; }
    }

    public List<MenuNode> MenuData = new List<MenuNode>
    {
        new MenuNode
        {
            Name = "Fashion",
            SubMenus = new List<MenuNode>
            {
                new MenuNode
                {
                    Name = "Men",
                    SubMenus = new List<MenuNode>
                    {
                        new MenuNode { Name = "Shirts" },
                        new MenuNode { Name = "Pants" }
                    }
                },
                new MenuNode
                {
                    Name = "Women",
                    SubMenus = new List<MenuNode>
                    {
                        new MenuNode { Name = "Dresses" },
                        new MenuNode { Name = "Skirts" }
                    }
                }
            }
        }
    };
}
```

## Best Practices

1. **Use consistent data structure**: Keep all items at same hierarchy level using same properties
2. **Unique identifiers**: Use unique ItemId values for ParentId mapping
3. **Null handling**: Set ParentId to `null` for root-level items
4. **Property naming**: Use meaningful names in MenuFieldSettings mappings
5. **Lazy loading**: For large menus, consider loading data on demand
