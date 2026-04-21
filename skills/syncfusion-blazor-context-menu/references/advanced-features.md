# Advanced Features

## Table of Contents
- [Animation Settings](#animation-settings)
- [Animation Effects](#animation-effects)
- [Dialog Integration](#dialog-integration)
- [Scrollable Menus](#scrollable-menus)
- [Large Menu Handling](#large-menu-handling)

## Animation Settings

Use `MenuAnimationSettings` to control how sub-menus appear and disappear.

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
                <MenuItem Text="Undo"></MenuItem>
                <MenuItem Text="Redo"></MenuItem>
            </MenuItems>
        </MenuItem>
    </MenuItems>
    <MenuAnimationSettings Effect="MenuEffect.FadeIn" Duration="800"></MenuAnimationSettings>
</SfContextMenu>

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

**Properties:**
- `Effect` - Animation type (see Animation Effects below)
- `Duration` - Duration in milliseconds (default 400ms)

---

## Animation Effects

Supported animation effects for sub-menu display:

| Effect | Description | Use Case |
|---|---|---|
| `MenuEffect.None` | No animation (instant) | Performance-sensitive or simple menus |
| `MenuEffect.SlideDown` | Sub-menu slides down from parent | Default feel, natural transition |
| `MenuEffect.ZoomIn` | Sub-menu zooms in from center | Modern, eye-catching |
| `MenuEffect.FadeIn` | Sub-menu fades in gradually | Smooth, professional appearance |

### SlideDown Effect

```cshtml
<MenuAnimationSettings 
    Effect="MenuEffect.SlideDown" 
    Duration="500">
</MenuAnimationSettings>
```

Sub-menu slides down smoothly (500ms) when parent item is hovered.

### ZoomIn Effect

```cshtml
<MenuAnimationSettings 
    Effect="MenuEffect.ZoomIn" 
    Duration="600">
</MenuAnimationSettings>
```

Sub-menu grows from center point, appears to zoom in.

### FadeIn Effect

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="File">
            <MenuItems>
                <MenuItem Text="New"></MenuItem>
                <MenuItem Text="Save"></MenuItem>
            </MenuItems>
        </MenuItem>
    </MenuItems>
    <MenuAnimationSettings 
        Effect="MenuEffect.FadeIn" 
        Duration="800">
    </MenuAnimationSettings>
</SfContextMenu>

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

Sub-menu appears by gradually fading in (800ms opacity transition).

### No Animation

```cshtml
<MenuAnimationSettings 
    Effect="MenuEffect.None" 
    Duration="0">
</MenuAnimationSettings>
```

Sub-menu appears instantly (best for performance).

---

## Dialog Integration

Open dialogs from menu item selection using the `ItemSelected` event.

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Popups

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Back"></MenuItem>
        <MenuItem Text="Forward"></MenuItem>
        <MenuItem Text="Reload"></MenuItem>
        <MenuItem Separator="true"></MenuItem>
        <MenuItem Text="Save As..."></MenuItem>
        <MenuItem Text="Print"></MenuItem>
        <MenuItem Text="Cast"></MenuItem>
    </MenuItems>
    <MenuEvents TValue="MenuItem" ItemSelected="@SelectedHandler"></MenuEvents>
</SfContextMenu>

<SfDialog @ref="dialogObj" 
    Content="@content" 
    Visible="false" 
    Target="#target" 
    Width="200px" 
    Height="110px">
    <DialogButtons>
        <DialogButton Content="Save" IsPrimary="true" @onclick="@SaveClicked"></DialogButton>
        <DialogButton Content="Cancel" @onclick="@CancelClicked"></DialogButton>
    </DialogButtons>
</SfDialog>

@code {
    private SfDialog dialogObj;
    private string content = "Save this file as PDF?";
    
    private async Task SelectedHandler(MenuEventArgs<MenuItem> args) {
        if (args.Item.Text == "Save As...") {
            await dialogObj.Show();
        }
    }
    
    private async Task SaveClicked() {
        await dialogObj.Hide();
        // Process save
    }
    
    private async Task CancelClicked() {
        await dialogObj.Hide();
    }
}

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

**Workflow:**
1. User right-clicks → ContextMenu appears
2. User clicks "Save As..." → ItemSelected fires
3. Check item text → `if (args.Item.Text == "Save As...")`
4. Call `dialogObj.Show()` → Dialog appears
5. User interacts with dialog

### Multiple Dialog Integration

```cshtml
@code {
    private async Task SelectedHandler(MenuEventArgs<MenuItem> args) {
        switch (args.Item.Text) {
            case "Save As...":
                await saveDialogRef.Show();
                break;
            case "Print":
                await printDialogRef.Show();
                break;
            case "Share":
                await shareDialogRef.Show();
                break;
        }
    }
}
```

---

## Scrollable Menus

Enable scrolling for large menus that exceed available viewport height.

```cshtml
@using Syncfusion.Blazor.Navigations

<SfContextMenu Target="#target" 
    TValue="MenuItem" 
    EnableScrolling="true"
    OnOpen="@OnBeforeOpen">
    <MenuEvents TValue="MenuItem" OnOpen="@OnBeforeOpen"></MenuEvents>
    <MenuItems>
        <MenuItem Text="View">
            <MenuItems>
                <MenuItem Text="Option 1"></MenuItem>
                <MenuItem Text="Option 2"></MenuItem>
                <MenuItem Text="Option 3"></MenuItem>
                <MenuItem Text="Option 4"></MenuItem>
                <MenuItem Text="Option 5"></MenuItem>
                <MenuItem Text="Option 6"></MenuItem>
                <MenuItem Text="Option 7"></MenuItem>
                <MenuItem Text="Option 8"></MenuItem>
                <MenuItem Text="Option 9"></MenuItem>
                <MenuItem Text="Option 10"></MenuItem>
            </MenuItems>
        </MenuItem>
        <MenuItem Text="Refresh"></MenuItem>
        <MenuItem Text="Paste"></MenuItem>
    </MenuItems>
</SfContextMenu>

<div id="target">Right click to open</div>

@code {
    private void OnBeforeOpen(BeforeOpenCloseMenuEventArgs<MenuItem> args) {
        // Set scroll height for submenu
        args.ScrollHeight = 150; // pixels
    }
}

<style>
    #target {
        border: 1px dashed; height: 250px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

**Key Points:**
- `EnableScrolling="true"` - Enable scroll functionality
- `OnBeforeOpen` event - Set scroll height per submenu
- `args.ScrollHeight` - Max height in pixels before scroll appears
- Scroll bars appear automatically when items exceed height

### Configure Scroll Height

```cshtml
@code {
    private void OnBeforeOpen(BeforeOpenCloseMenuEventArgs<MenuItem> args) {
        // Different scroll heights for different submenus
        if (args.ParentItem?.Text == "Large Menu") {
            args.ScrollHeight = 200;
        } else if (args.ParentItem?.Text == "View") {
            args.ScrollHeight = 150;
        } else {
            args.ScrollHeight = 100;
        }
    }
}
```

---

## Large Menu Handling

Strategies for menus with 20+ items:

### Strategy 1: Categorize with Nesting

```cshtml
<MenuItems>
    <MenuItem Text="Recently Used">
        <MenuItems>
            <MenuItem Text="Document 1"></MenuItem>
            <MenuItem Text="Document 2"></MenuItem>
            <MenuItem Text="Document 3"></MenuItem>
        </MenuItems>
    </MenuItem>
    <MenuItem Text="All Documents">
        <MenuItems>
            <MenuItem Text="Project A"></MenuItem>
            <MenuItem Text="Project B"></MenuItem>
            <!-- More items -->
        </MenuItems>
    </MenuItem>
</MenuItems>
```

Group items into logical categories, use sub-menus for organization.

### Strategy 2: Enable Scrolling + Set Height

```cshtml
<SfContextMenu Target="#target" 
    TValue="MenuItem" 
    EnableScrolling="true"
    OnOpen="@OnBeforeOpen">
    <!-- Many items -->
</SfContextMenu>

@code {
    private void OnBeforeOpen(BeforeOpenCloseMenuEventArgs<MenuItem> args) {
        args.ScrollHeight = 200; // Allow vertical scroll
    }
}
```

Limit visible height, enable scroll for overflow.

### Strategy 3: Data Binding + Search/Filter

```cshtml
@using Syncfusion.Blazor.Navigations

<input type="text" placeholder="Filter..." @onchange="@OnFilterChanged" />

<SfContextMenu Target="#target" Items="@filteredItems">
    <MenuFieldSettings Text="Name"></MenuFieldSettings>
</SfContextMenu>

@code {
    private List<MenuItem> allItems = new();
    private List<MenuItem> filteredItems = new();
    
    protected override void OnInitialized() {
        // Load 50+ items
        for (int i = 1; i <= 50; i++) {
            allItems.Add(new MenuItem { Text = $"Item {i}" });
        }
        filteredItems = allItems;
    }
    
    private void OnFilterChanged(ChangeEventArgs e) {
        string filter = e.Value?.ToString().ToLower() ?? "";
        filteredItems = allItems
            .Where(x => x.Text.ToLower().Contains(filter))
            .ToList();
    }
}
```

Filter menu items dynamically as user types.

### Strategy 4: Pagination

```cshtml
@code {
    private List<MenuItem> GetPagedItems(int pageNumber, int pageSize) {
        return allItems
            .Skip(pageNumber * pageSize)
            .Take(pageSize)
            .ToList();
    }
    
    private List<MenuItem> GetAllWithPagination() {
        var paged = GetPagedItems(0, 10);
        paged.Add(new MenuItem { Text = "Load More..." });
        return paged;
    }
}
```

Show limited items with "Load More" option.

---

## Complete Example: Advanced Feature Showcase

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Popups

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" 
    TValue="MenuItem"
    EnableScrolling="true"
    OnOpen="@OnMenuOpen">
    <MenuItems>
        <MenuItem Text="File">
            <MenuItems>
                <MenuItem Text="New"></MenuItem>
                <MenuItem Text="Open"></MenuItem>
                <MenuItem Text="Recent Files">
                    <MenuItems>
                        <MenuItem Text="Document 1"></MenuItem>
                        <MenuItem Text="Document 2"></MenuItem>
                        <MenuItem Text="Document 3"></MenuItem>
                    </MenuItems>
                </MenuItem>
            </MenuItems>
        </MenuItem>
        <MenuItem Text="Edit">
            <MenuItems>
                <MenuItem Text="Cut"></MenuItem>
                <MenuItem Text="Copy"></MenuItem>
                <MenuItem Text="Paste"></MenuItem>
            </MenuItems>
        </MenuItem>
        <MenuItem Separator="true"></MenuItem>
        <MenuItem Text="Settings"></MenuItem>
        <MenuItem Text="Help"></MenuItem>
    </MenuItems>
    <MenuAnimationSettings Effect="MenuEffect.FadeIn" Duration="600"></MenuAnimationSettings>
    <MenuEvents TValue="MenuItem" ItemSelected="@OnItemSelected"></MenuEvents>
</SfContextMenu>

<SfDialog @ref="settingsDialog" 
    Visible="false" 
    Title="Settings"
    Width="300px">
    <DialogContent>
        Settings panel content...
    </DialogContent>
</SfDialog>

<p>Last Action: <strong>@lastAction</strong></p>

@code {
    private SfDialog settingsDialog;
    private string lastAction = "None";
    
    private void OnMenuOpen(BeforeOpenCloseMenuEventArgs<MenuItem> args) {
        args.ScrollHeight = 200;
    }
    
    private async Task OnItemSelected(MenuEventArgs<MenuItem> args) {
        lastAction = args.Item.Text;
        
        if (args.Item.Text == "Settings") {
            await settingsDialog.Show();
        }
    }
}

<style>
    #target {
        border: 1px dashed; height: 200px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

**Features Demonstrated:**
- Nested menus with categorization
- FadeIn animation (600ms)
- Scroll support for large lists
- Dialog integration
- Event handling
- Custom styling

