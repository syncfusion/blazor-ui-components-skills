# Data Binding

## Table of Contents
- [Basic Data Binding](#basic-data-binding)
- [MenuFieldSettings](#menufieldsettings)
- [Custom Object Mapping](#custom-object-mapping)
- [Hierarchical Data](#hierarchical-data)
- [Dynamic Menu Population](#dynamic-menu-population)

## Basic Data Binding

Bind menu items from a data collection using the `Items` property.

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" Items="@menuItems">
    <MenuFieldSettings Text="Title"></MenuFieldSettings>
</SfContextMenu>

@code {
    private List<MenuItem> menuItems = new();
    
    protected override void OnInitialized() {
        menuItems.Add(new MenuItem { Text = "Cut" });
        menuItems.Add(new MenuItem { Text = "Copy" });
        menuItems.Add(new MenuItem { Text = "Paste" });
    }
}

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

**Key Points:**
- `Items="@menuItems"` - Bind to collection
- `MenuFieldSettings` - Map data properties to component properties
- `Text="Title"` - Maps `Title` property to display text

---

## MenuFieldSettings

Use `MenuFieldSettings` to map custom object properties to ContextMenu component properties.

### Common Mappings

```cshtml
<SfContextMenu Target="#target" Items="@menuItems">
    <MenuFieldSettings 
        Text="Label" 
        IconCss="Icon" 
        Url="Link"
        Disabled="IsDisabled">
    </MenuFieldSettings>
</SfContextMenu>
```

| Field Property | Maps to | Purpose |
|---|---|---|
| `Text` | Display text | Item label shown in menu |
| `IconCss` | Icon class | CSS class for icon |
| `Url` | Navigation URL | Link for navigation items |
| `Disabled` | Item disabled state | Disable specific items |
| `Separator` | Separator indicator | Show separator line |
| `Children` | Nested items | Submenu items (hierarchical) |

---

## Custom Object Mapping

Create custom classes and map their properties to ContextMenu.

### Simple Custom Object

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" Items="@customMenuItems">
    <MenuFieldSettings Text="DisplayName" IconCss="CssClass"></MenuFieldSettings>
</SfContextMenu>

@code {
    private List<CustomMenuItem> customMenuItems = new();
    
    protected override void OnInitialized() {
        customMenuItems.Add(new CustomMenuItem 
        { 
            DisplayName = "Cut", 
            CssClass = "e-icons e-cut",
            Action = "cut"
        });
        customMenuItems.Add(new CustomMenuItem 
        { 
            DisplayName = "Copy", 
            CssClass = "e-icons e-copy",
            Action = "copy"
        });
        customMenuItems.Add(new CustomMenuItem 
        { 
            DisplayName = "Paste", 
            CssClass = "e-icons e-paste",
            Action = "paste"
        });
    }
    
    private class CustomMenuItem {
        public string DisplayName { get; set; }
        public string CssClass { get; set; }
        public string Action { get; set; }
    }
}

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

### Custom Object with Event Handler

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" Items="@actionMenuItems">
    <MenuFieldSettings Text="Name"></MenuFieldSettings>
    <MenuEvents TValue="CustomAction" ItemSelected="@OnActionSelected"></MenuEvents>
</SfContextMenu>

<p>Last action: <strong>@lastAction</strong></p>

@code {
    private List<CustomAction> actionMenuItems = new();
    private string lastAction = "None";
    
    protected override void OnInitialized() {
        actionMenuItems.Add(new CustomAction 
        { 
            Name = "Edit",
            Callback = () => Edit()
        });
        actionMenuItems.Add(new CustomAction 
        { 
            Name = "Delete",
            Callback = () => Delete()
        });
        actionMenuItems.Add(new CustomAction 
        { 
            Name = "Archive",
            Callback = () => Archive()
        });
    }
    
    private void OnActionSelected(MenuEventArgs<CustomAction> args) {
        lastAction = args.Item.Name;
        args.Item.Callback?.Invoke();
    }
    
    private void Edit() => lastAction += " (Editing...)";
    private void Delete() => lastAction += " (Deleting...)";
    private void Archive() => lastAction += " (Archiving...)";
    
    private class CustomAction {
        public string Name { get; set; }
        public Action Callback { get; set; }
    }
}

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

---

## Hierarchical Data

Bind nested collections for submenu support using the `Children` property.

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" Items="@hierarchicalItems">
    <MenuFieldSettings 
        Text="Label" 
        Children="SubItems">
    </MenuFieldSettings>
</SfContextMenu>

@code {
    private List<MenuItem> hierarchicalItems = new();
    
    protected override void OnInitialized() {
        hierarchicalItems.Add(new MenuItem {
            Text = "File",
            Children = new List<MenuItem> {
                new MenuItem { Text = "New" },
                new MenuItem { Text = "Open" },
                new MenuItem { Text = "Save" }
            }
        });
        
        hierarchicalItems.Add(new MenuItem {
            Text = "Edit",
            Children = new List<MenuItem> {
                new MenuItem { Text = "Cut" },
                new MenuItem { Text = "Copy" },
                new MenuItem { Text = "Paste" }
            }
        });
    }
}

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

### Custom Hierarchy Model

```cshtml
@code {
    private List<MenuNode> hierarchicalItems = new();
    
    protected override void OnInitialized() {
        hierarchicalItems.Add(new MenuNode {
            Title = "Bookmarks",
            SubItems = new List<MenuNode> {
                new MenuNode {
                    Title = "Most Visited",
                    SubItems = new List<MenuNode> {
                        new MenuNode { Title = "Google" },
                        new MenuNode { Title = "GitHub" }
                    }
                },
                new MenuNode { Title = "Recently Added" }
            }
        });
    }
    
    private class MenuNode {
        public string Title { get; set; }
        public List<MenuNode> SubItems { get; set; }
    }
}
```

**Mapping:**
```cshtml
<SfContextMenu Target="#target" Items="@hierarchicalItems">
    <MenuFieldSettings 
        Text="Title" 
        Children="SubItems">
    </MenuFieldSettings>
</SfContextMenu>
```

---

## Dynamic Menu Population

Populate menu items dynamically from databases or APIs.

### From Database (Simulated)

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" Items="@dbMenuItems">
    <MenuFieldSettings Text="Name" IconCss="Icon"></MenuFieldSettings>
</SfContextMenu>

@code {
    private List<DbMenuItem> dbMenuItems = new();
    
    protected override async Task OnInitializedAsync() {
        // Simulate database fetch
        dbMenuItems = await FetchMenuFromDatabase();
    }
    
    private async Task<List<DbMenuItem>> FetchMenuFromDatabase() {
        // Simulated async DB call
        await Task.Delay(500);
        
        return new List<DbMenuItem> {
            new DbMenuItem { Name = "Users", Icon = "e-icons e-people" },
            new DbMenuItem { Name = "Reports", Icon = "e-icons e-chart" },
            new DbMenuItem { Name = "Settings", Icon = "e-icons e-settings" }
        };
    }
    
    private class DbMenuItem {
        public string Name { get; set; }
        public string Icon { get; set; }
    }
}

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

### Filtered Menu Based on User Role

```cshtml
@code {
    private List<RoleMenuItem> userMenuItems = new();
    private string currentRole = "admin"; // From auth
    
    protected override void OnInitialized() {
        var allItems = GetAllMenuItems();
        
        // Filter based on role
        userMenuItems = allItems
            .Where(item => item.AllowedRoles.Contains(currentRole))
            .ToList();
    }
    
    private List<RoleMenuItem> GetAllMenuItems() {
        return new List<RoleMenuItem> {
            new RoleMenuItem { 
                Name = "Edit", 
                AllowedRoles = new[] { "admin", "editor" } 
            },
            new RoleMenuItem { 
                Name = "Delete", 
                AllowedRoles = new[] { "admin" } 
            },
            new RoleMenuItem { 
                Name = "View", 
                AllowedRoles = new[] { "admin", "editor", "viewer" } 
            }
        };
    }
    
    private class RoleMenuItem {
        public string Name { get; set; }
        public string[] AllowedRoles { get; set; }
    }
}
```

### Real-Time Menu Update

```cshtml
@code {
    private List<MenuItem> liveMenuItems = new();
    
    protected override void OnInitialized() {
        liveMenuItems = GetInitialMenu();
    }
    
    // Called from external event or timer
    private void RefreshMenuItems() {
        liveMenuItems = GetUpdatedMenu();
        StateHasChanged(); // Force re-render
    }
    
    private List<MenuItem> GetInitialMenu() {
        return new List<MenuItem> {
            new MenuItem { Text = "Action 1" },
            new MenuItem { Text = "Action 2" }
        };
    }
    
    private List<MenuItem> GetUpdatedMenu() {
        return new List<MenuItem> {
            new MenuItem { Text = "Action 1" },
            new MenuItem { Text = "Action 2" },
            new MenuItem { Text = "Action 3 (NEW)" }
        };
    }
}
```

---

## Complete Example: Database-Driven Menu

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" Items="@contextActions">
    <MenuFieldSettings 
        Text="ActionName" 
        IconCss="IconClass"
        Disabled="IsDisabled">
    </MenuFieldSettings>
    <MenuEvents TValue="ContextAction" ItemSelected="@OnActionClick"></MenuEvents>
</SfContextMenu>

<p>Last performed: <strong>@lastAction</strong></p>

@code {
    private List<ContextAction> contextActions = new();
    private string lastAction = "None";
    
    protected override async Task OnInitializedAsync() {
        contextActions = await LoadActionsFromDatabase();
    }
    
    private async Task<List<ContextAction>> LoadActionsFromDatabase() {
        // Simulate DB fetch
        await Task.Delay(300);
        
        return new List<ContextAction> {
            new ContextAction { ActionName = "Edit", IconClass = "e-icons e-edit" },
            new ContextAction { ActionName = "Copy", IconClass = "e-icons e-copy" },
            new ContextAction { ActionName = "Delete", IconClass = "e-icons e-delete", IsDisabled = false }
        };
    }
    
    private void OnActionClick(MenuEventArgs<ContextAction> args) {
        lastAction = $"{args.Item.ActionName} executed";
    }
    
    private class ContextAction {
        public string ActionName { get; set; }
        public string IconClass { get; set; }
        public bool IsDisabled { get; set; }
    }
}

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

