# Advanced Scenarios and Customization

## Table of Contents
- [Click-Based Menu Opening](#click-based-menu-opening)
- [Dynamic Menu Item Management](#dynamic-menu-item-management)
- [Custom Templates](#custom-templates)
- [Menu Item Selection Patterns](#menu-item-selection-patterns)
- [Performance Optimization](#performance-optimization)
- [Common Patterns and Best Practices](#common-patterns-and-best-practices)

## Click-Based Menu Opening

By default, submenus open on hover. Use `ShowItemOnClick` to require click-based opening:

### Basic Click Opening

```razor
<SfMenu TValue="MenuItem" ShowItemOnClick="true">
    <MenuItems>
        <MenuItem Text="File">
            <MenuItems>
                <MenuItem Text="New"></MenuItem>
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
    </MenuItems>
</SfMenu>
```

**Behavior**: Click "File" to open submenu, click again to close

### Use Cases for Click Opening

- **Touch devices**: Better UX for mobile/tablet
- **Accessibility**: Easier for keyboard navigation
- **Desktop apps**: Familiar behavior for Windows/Office-style menus

### Conditional Click Opening

```razor
<SfMenu TValue="MenuItem" ShowItemOnClick="@isClickMode">
    <!-- Menu items -->
</SfMenu>

<button @onclick="@(() => isClickMode = !isClickMode)">
    Toggle: @(isClickMode ? "Click Mode" : "Hover Mode")
</button>

@code {
    private bool isClickMode = false;
}
```

## Dynamic Menu Item Management

### Adding Menu Items

Add items after menu creation using component reference:

```razor
@using Syncfusion.Blazor.Navigations

<button @onclick="AddMenuItem">Add New Item</button>
<button @onclick="RemoveMenuItem">Remove Last Item</button>

<SfMenu Items="@MenuItems" @ref="MenuRef">
    <MenuFieldSettings Text="@nameof(MenuItem.Text)" Children="@nameof(MenuItem.SubItems)"></MenuFieldSettings>
</SfMenu>

@code {
    private SfMenu<MenuItem> MenuRef;
    
    public class MenuItem
    {
        public string Text { get; set; }
        public List<MenuItem> SubItems { get; set; }
    }
    
    public List<MenuItem> MenuItems = new List<MenuItem>
    {
        new MenuItem { Text = "File", SubItems = new List<MenuItem>
        {
            new MenuItem { Text = "Open" },
            new MenuItem { Text = "Save" }
        }},
        new MenuItem { Text = "Edit" }
    };
    
    private void AddMenuItem()
    {
        MenuItems.Add(new MenuItem 
        { 
            Text = $"Item {MenuItems.Count + 1}", 
            SubItems = new List<MenuItem>() 
        });
        StateHasChanged();
    }
    
    private void RemoveMenuItem()
    {
        if (MenuItems.Count > 0)
        {
            MenuItems.RemoveAt(MenuItems.Count - 1);
            StateHasChanged();
        }
    }
}
```

### Removing Menu Items

```csharp
private void RemoveItem(MenuItem item)
{
    MenuItems.Remove(item);
    StateHasChanged();
}
```

### Enabling/Disabling Items

```razor
<SfMenu Items="@MenuItems">
    <MenuFieldSettings Text="Text" Disabled="IsDisabled" Children="SubItems"></MenuFieldSettings>
</SfMenu>

@code {
    public class MenuItem
    {
        public string Text { get; set; }
        public bool IsDisabled { get; set; }
        public List<MenuItem> SubItems { get; set; }
    }
    
    public List<MenuItem> MenuItems = new List<MenuItem>
    {
        new MenuItem { Text = "File", IsDisabled = false },
        new MenuItem { Text = "Edit", IsDisabled = true },
        new MenuItem { Text = "Undo", IsDisabled = @(!canUndo) }
    };
    
    private bool canUndo = false;
    
    private void UpdateUndoState()
    {
        canUndo = true;
        StateHasChanged();
    }
}
```

### Add/Remove on Event

```csharp
private void OnItemSelected(MenuEventArgs<MenuItem> args)
{
    if (args.Item.Text == "Add Report")
    {
        var newReport = new MenuItem 
        { 
            Text = $"Report {DateTime.Now:HH:mm:ss}" 
        };
        MenuItems.Add(newReport);
        StateHasChanged();
    }
}
```

## Custom Templates

### Menu Item Template

Customize menu item rendering:

```razor
@using Syncfusion.Blazor.Navigations

<SfMenu Items="@MenuItems">
    <MenuFieldSettings Text="Text" Children="SubItems"></MenuFieldSettings>
    <MenuTemplates TValue="MenuItem">
        <Template>
            @{
                var item = context as MenuItem;
                <div style="display: flex; justify-content: space-between; align-items: center;">
                    <span>@item.Text</span>
                    @if (item.HasNew)
                    {
                        <span style="background: red; color: white; padding: 2px 6px; border-radius: 3px; font-size: 12px;">NEW</span>
                    }
                </div>
            }
        </Template>
    </MenuTemplates>
</SfMenu>

@code {
    public class MenuItem
    {
        public string Text { get; set; }
        public bool HasNew { get; set; }
        public List<MenuItem> SubItems { get; set; }
    }
    
    public List<MenuItem> MenuItems = new List<MenuItem>
    {
        new MenuItem { Text = "Features", HasNew = true, SubItems = new List<MenuItem>() },
        new MenuItem { Text = "Documentation", HasNew = false, SubItems = new List<MenuItem>() }
    };
}
```

### Complex Template with Icons and Badge

```razor
<MenuTemplates TValue="MenuItem">
    <Template>
        @{
            var item = context as MenuItem;
            <div class="custom-menu-item">
                <i class="@item.IconClass"></i>
                <span class="item-text">@item.Text</span>
                @if (item.BadgeCount > 0)
                {
                    <span class="badge">@item.BadgeCount</span>
                }
            </div>
        }
    </Template>
</MenuTemplates>

<style>
    .custom-menu-item {
        display: flex;
        align-items: center;
        gap: 10px;
    }
    
    .item-text {
        flex: 1;
    }
    
    .badge {
        background: #ff5722;
        color: white;
        padding: 2px 8px;
        border-radius: 12px;
        font-size: 12px;
        font-weight: bold;
    }
</style>
```

## Menu Item Selection Patterns

### Get Selected Item

```csharp
private MenuItem selectedItem;

private void OnItemSelected(MenuEventArgs<MenuItem> args)
{
    selectedItem = args.Item;
    Console.WriteLine($"Selected: {selectedItem.Text}");
}
```

### Track Selection History

```razor
@page "/menu-history"
@using Syncfusion.Blazor.Navigations

<h3>Selection History</h3>
<ul>
    @foreach (var item in selectionHistory.Take(5))
    {
        <li>@item</li>
    }
</ul>

<SfMenu TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Item 1"></MenuItem>
        <MenuItem Text="Item 2"></MenuItem>
        <MenuItem Text="Item 3"></MenuItem>
    </MenuItems>
    <MenuEvents TValue="MenuItem" ItemSelected="OnSelectItem"></MenuEvents>
</SfMenu>

@code {
    private Stack<string> selectionHistory = new Stack<string>();
    
    private void OnSelectItem(MenuEventArgs<MenuItem> args)
    {
        selectionHistory.Push(args.Item.Text);
        if (selectionHistory.Count > 10)
        {
            selectionHistory.Pop();  // Keep last 10 items
        }
    }
}
```

### Multi-Selection (Custom Logic)

```razor
@page "/menu-multiselect"

<h3>Selected Items (@selectedItems.Count)</h3>
<ul>
    @foreach (var item in selectedItems)
    {
        <li>@item</li>
    }
</ul>

<SfMenu TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Item 1"></MenuItem>
        <MenuItem Text="Item 2"></MenuItem>
        <MenuItem Text="Item 3"></MenuItem>
    </MenuItems>
    <MenuEvents TValue="MenuItem" ItemSelected="OnSelectMultiple"></MenuEvents>
</SfMenu>

@code {
    private HashSet<string> selectedItems = new HashSet<string>();
    
    private void OnSelectMultiple(MenuEventArgs<MenuItem> args)
    {
        string itemText = args.Item.Text;
        
        // Toggle selection (custom multi-select logic)
        if (selectedItems.Contains(itemText))
        {
            selectedItems.Remove(itemText);
        }
        else
        {
            selectedItems.Add(itemText);
        }
    }
}
```

## Performance Optimization

### Lazy Loading Submenus

Load submenu data only when parent item opens:

```razor
@page "/lazy-loading"
@using Syncfusion.Blazor.Navigations

<SfMenu Items="@MenuItems">
    <MenuFieldSettings Text="Text" Children="SubItems"></MenuFieldSettings>
    <MenuEvents TValue="MenuItem" OnOpen="OnOpenSubmenu"></MenuEvents>
</SfMenu>

@code {
    public class MenuItem
    {
        public string Text { get; set; }
        public List<MenuItem> SubItems { get; set; }
        public bool IsLoaded { get; set; }
    }
    
    public List<MenuItem> MenuItems = new List<MenuItem>
    {
        new MenuItem { Text = "Reports", IsLoaded = false, SubItems = new List<MenuItem>() },
        new MenuItem { Text = "Analytics", IsLoaded = false, SubItems = new List<MenuItem>() }
    };
    
    private async Task OnOpenSubmenu(MenuEventArgs<MenuItem> args)
    {
        var item = args.Item as MenuItem;
        
        if (!item.IsLoaded && item.SubItems.Count == 0)
        {
            // Load data on first open
            item.SubItems = await LoadSubmenusAsync(item.Text);
            item.IsLoaded = true;
        }
    }
    
    private async Task<List<MenuItem>> LoadSubmenusAsync(string parentName)
    {
        await Task.Delay(500);  // Simulate API call
        return new List<MenuItem>
        {
            new MenuItem { Text = $"{parentName} - Sub1" },
            new MenuItem { Text = $"{parentName} - Sub2" }
        };
    }
}
```

### Virtualization for Large Lists

```csharp
// For very large menus, consider virtualizing the list
// Syncfusion components have built-in virtualization support
private List<MenuItem> GetVirtualizedMenuItems(int startIndex, int count)
{
    return allMenuItems.Skip(startIndex).Take(count).ToList();
}
```

## Common Patterns and Best Practices

### Pattern 1: Breadcrumb-Style Menu

```razor
<SfMenu TValue="MenuItem" ShowItemOnClick="true">
    <MenuItems>
        <MenuItem Text="Dashboard"></MenuItem>
        <MenuItem Text="Settings">
            <MenuItems>
                <MenuItem Text="Profile"></MenuItem>
                <MenuItem Text="Security"></MenuItem>
                <MenuItem Text="Preferences"></MenuItem>
            </MenuItems>
        </MenuItem>
        <MenuItem Text="Help">
            <MenuItems>
                <MenuItem Text="Documentation"></MenuItem>
                <MenuItem Text="Support"></MenuItem>
            </MenuItems>
        </MenuItem>
    </MenuItems>
</SfMenu>
```

### Pattern 2: Command Menu (Context Action)

```csharp
private void OnItemSelected(MenuEventArgs<MenuItem> args)
{
    var command = args.Item.Text;
    
    switch (command)
    {
        case "Save":
            SaveDocument();
            break;
        case "Export":
            ExportDocument();
            break;
        case "Print":
            PrintDocument();
            break;
    }
}
```

### Pattern 3: Role-Based Menu Visibility

```razor
@using Syncfusion.Blazor.Navigations

<SfMenu TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Public Section"></MenuItem>
        @if (userRole == "Admin")
        {
            <MenuItem Text="Admin Panel">
                <MenuItems>
                    <MenuItem Text="Users"></MenuItem>
                    <MenuItem Text="Roles"></MenuItem>
                </MenuItems>
            </MenuItem>
        }
        @if (userRole != "Guest")
        {
            <MenuItem Text="Profile"></MenuItem>
        }
    </MenuItems>
</SfMenu>

@code {
    private string userRole = "User";  // From authentication context
}
```

### Pattern 4: Search Within Menu

```razor
<input type="text" placeholder="Search menu..." @oninput="@FilterMenu" />

<SfMenu Items="@FilteredMenuItems">
    <MenuFieldSettings Text="Text" Children="SubItems"></MenuFieldSettings>
</SfMenu>

@code {
    private List<MenuItem> allMenuItems;
    private List<MenuItem> FilteredMenuItems;
    
    private void FilterMenu(ChangeEventArgs e)
    {
        string searchTerm = e.Value?.ToString()?.ToLower() ?? "";
        
        FilteredMenuItems = allMenuItems
            .Where(item => item.Text.ToLower().Contains(searchTerm))
            .ToList();
    }
}
```

### Pattern 5: Menu with Analytics

```csharp
private void OnItemSelected(MenuEventArgs<MenuItem> args)
{
    // Track menu usage
    var analyticsEvent = new
    {
        EventType = "MenuItemSelected",
        ItemText = args.Item.Text,
        Timestamp = DateTime.UtcNow,
        UserId = GetCurrentUserId()
    };
    
    LogAnalytics(analyticsEvent);
}
```

## Best Practices

1. **Keep menus shallow**: Limit nesting to 2-3 levels for usability
2. **Logical grouping**: Group related items together
3. **Consistent naming**: Use clear, descriptive item labels
4. **Performance**: Lazy-load large submenus
5. **Accessibility**: Ensure keyboard navigation works
6. **Mobile testing**: Test on various screen sizes
7. **Feedback**: Provide visual feedback on item hover/selection
8. **Documentation**: Document custom menu structure for team
