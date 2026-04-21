# Events and Interactions

## Table of Contents
- [ItemSelected Event](#itemselected-event)
- [MenuEventArgs Properties](#menueventargs-properties)
- [Programmatic Open](#programmatic-open)
- [Programmatic Close](#programmatic-close)
- [Common Event Patterns](#common-event-patterns)

## ItemSelected Event

The `ItemSelected` event fires when a user clicks on a menu item. Use it to capture which item was clicked and execute custom logic.

### Basic ItemSelected Handler

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Cut"></MenuItem>
        <MenuItem Text="Copy"></MenuItem>
        <MenuItem Text="Paste"></MenuItem>
    </MenuItems>
    <MenuEvents TValue="MenuItem" ItemSelected="@OnItemSelected"></MenuEvents>
</SfContextMenu>

<p>Selected: <strong>@selectedItem</strong></p>

@code {
    private string selectedItem = "None";
    
    private void OnItemSelected(MenuEventArgs<MenuItem> args) {
        selectedItem = args.Item.Text;
    }
}

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

**How it works:**
1. User right-clicks → ContextMenu appears
2. User clicks menu item (e.g., "Copy")
3. `ItemSelected` event fires
4. `OnItemSelected()` method called with event data
5. `args.Item.Text` contains "Copy"

### Handle Multiple Actions

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Cut"></MenuItem>
        <MenuItem Text="Copy"></MenuItem>
        <MenuItem Text="Paste"></MenuItem>
        <MenuItem Separator="true"></MenuItem>
        <MenuItem Text="Delete"></MenuItem>
    </MenuItems>
    <MenuEvents TValue="MenuItem" ItemSelected="@HandleMenuAction"></MenuEvents>
</SfContextMenu>

<p>@message</p>

@code {
    private string message = "Ready...";
    
    private void HandleMenuAction(MenuEventArgs<MenuItem> args) {
        message = args.Item.Text switch {
            "Cut" => "Text cut to clipboard",
            "Copy" => "Text copied to clipboard",
            "Paste" => "Text pasted from clipboard",
            "Delete" => "Text deleted",
            _ => "Unknown action"
        };
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

## MenuEventArgs Properties

The `MenuEventArgs<MenuItem>` object passed to event handlers contains:

| Property | Type | Description |
|---|---|---|
| `Item` | MenuItem | The clicked menu item object |
| `Item.Text` | string | Text of the menu item |
| `Item.Disabled` | bool | Whether item is disabled |
| `Item.Separator` | bool | Whether item is separator |
| `Item.Url` | string | Navigation URL (if set) |
| `Item.IconCss` | string | Icon CSS class |

### Access Item Properties

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Edit" IconCss="e-icons e-edit"></MenuItem>
        <MenuItem Text="Delete" IconCss="e-icons e-delete"></MenuItem>
        <MenuItem Text="Help" Url="https://help.example.com"></MenuItem>
    </MenuItems>
    <MenuEvents TValue="MenuItem" ItemSelected="@OnItemSelected"></MenuEvents>
</SfContextMenu>

<div>
    <p><strong>Item Text:</strong> @selectedText</p>
    <p><strong>Icon CSS:</strong> @selectedIcon</p>
    <p><strong>Has URL:</strong> @hasUrl</p>
</div>

@code {
    private string selectedText = "";
    private string selectedIcon = "";
    private bool hasUrl = false;
    
    private void OnItemSelected(MenuEventArgs<MenuItem> args) {
        selectedText = args.Item.Text;
        selectedIcon = args.Item.IconCss ?? "None";
        hasUrl = !string.IsNullOrEmpty(args.Item.Url);
    }
}

<style>
    #target {
        border: 1px dashed; height: 100px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

---

## Programmatic Open

Use the `@ref` attribute to get reference to `SfContextMenu`, then call `Open()` method to manually open the menu.

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton @onclick="OpenMenu">Open Menu</SfButton>

<div id="target">
    <SfContextMenu @ref="contextMenuRef" TValue="MenuItem">
        <MenuItems>
            <MenuItem Text="Option 1"></MenuItem>
            <MenuItem Text="Option 2"></MenuItem>
            <MenuItem Text="Option 3"></MenuItem>
        </MenuItems>
    </SfContextMenu>
</div>

@code {
    private SfContextMenu<MenuItem> contextMenuRef;
    
    private async Task OpenMenu(MouseEventArgs e) {
        await contextMenuRef.Open(e.ClientX, e.ClientY);
    }
}

<style>
    #target {
        margin-top: 20px; width: 300px; height: 200px;
        border: 1px dashed; padding: 10px; color: gray;
    }
</style>
```

**Key Points:**
- `@ref="contextMenuRef"` - Store component reference
- `contextMenuRef.Open(x, y)` - Open at coordinates (x, y in pixels)
- `e.ClientX, e.ClientY` - Mouse coordinates from click event
- Can also use fixed coordinates: `Open(100, 100)`

### Open on Button Click

```cshtml
<SfButton @onclick="@((e) => contextMenuRef.Open(100, 50))">
    Open Menu
</SfButton>
```

---

## Programmatic Close

Use the `Close()` method to programmatically close an open menu.

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<div id="target">Right click or use buttons</div>

<SfContextMenu @ref="contextMenuRef" Target="#target" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Option 1"></MenuItem>
        <MenuItem Text="Option 2"></MenuItem>
        <MenuItem Text="Close Menu">
            <MenuItem Text="Nested Item"></MenuItem>
        </MenuItem>
    </MenuItems>
    <MenuEvents TValue="MenuItem" ItemSelected="@OnItemSelected"></MenuEvents>
</SfContextMenu>

<div style="margin-top: 20px;">
    <SfButton @onclick="CloseMenu">Close Menu</SfButton>
</div>

@code {
    private SfContextMenu<MenuItem> contextMenuRef;
    private string lastAction = "";
    
    private async Task OnItemSelected(MenuEventArgs<MenuItem> args) {
        lastAction = $"Selected: {args.Item.Text}";
        await contextMenuRef.Close();
    }
    
    private async Task CloseMenu() {
        await contextMenuRef.Close();
    }
}

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

**Use Cases:**
- Auto-close menu after action completed
- Close on external event (ESC key, etc.)
- Manual close button for touch devices

---

## Common Event Patterns

### Pattern 1: Conditional Action Based on Item

```cshtml
private void HandleMenuAction(MenuEventArgs<MenuItem> args) {
    if (args.Item.Text == "Delete") {
        // Show confirmation dialog
        ShowDeleteConfirmation();
    } else if (args.Item.Text == "Edit") {
        // Open edit form
        OpenEditForm();
    }
}
```

### Pattern 2: Async Operation

```cshtml
private async Task OnItemSelected(MenuEventArgs<MenuItem> args) {
    // Perform async operation
    var result = await FetchDataAsync(args.Item.Text);
    ProcessResult(result);
    
    // Close menu after operation
    await contextMenuRef.Close();
}
```

### Pattern 3: Event with State Update

```cshtml
@code {
    private string selectedItem = "";
    private List<string> history = new();
    
    private void OnItemSelected(MenuEventArgs<MenuItem> args) {
        selectedItem = args.Item.Text;
        history.Add($"{DateTime.Now:HH:mm:ss} - {selectedItem}");
        
        // Trigger component re-render
        StateHasChanged();
    }
}
```

### Pattern 4: Nested Menu Item Selection

```cshtml
<MenuEvents TValue="MenuItem" ItemSelected="@HandleSelection"></MenuEvents>

@code {
    private void HandleSelection(MenuEventArgs<MenuItem> args) {
        // Works for both parent and child items
        var path = BuildItemPath(args.Item);
        Console.WriteLine($"Selected path: {path}");
    }
    
    private string BuildItemPath(MenuItem item) {
        // Custom logic to build hierarchical path
        return item.Text;
    }
}
```

### Pattern 5: Context Data with Event

```cshtml
<div id="target" data-id="123">
    Right click item #123
</div>

@code {
    private async Task OnItemSelected(MenuEventArgs<MenuItem> args) {
        // Access context element data
        var elementId = args.Item.Text; // Could also get from context
        var action = args.Item.Text;
        
        await PerformAction(elementId, action);
    }
}
```

---

## Complete Example: File Manager Context Menu

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click file</div>

<SfContextMenu Target="#target" TValue="MenuItem" @ref="contextMenuRef">
    <MenuItems>
        <MenuItem Text="Cut" IconCss="e-icons e-cut"></MenuItem>
        <MenuItem Text="Copy" IconCss="e-icons e-copy"></MenuItem>
        <MenuItem Text="Paste" IconCss="e-icons e-paste"></MenuItem>
        <MenuItem Separator="true"></MenuItem>
        <MenuItem Text="Delete" IconCss="e-icons e-delete"></MenuItem>
        <MenuItem Text="Rename"></MenuItem>
        <MenuItem Separator="true"></MenuItem>
        <MenuItem Text="Properties"></MenuItem>
    </MenuItems>
    <MenuEvents TValue="MenuItem" ItemSelected="@HandleFileAction"></MenuEvents>
</SfContextMenu>

<div style="margin-top: 20px;">
    <p><strong>Last Action:</strong> @lastAction</p>
    <p><strong>Timestamp:</strong> @actionTime</p>
</div>

@code {
    private SfContextMenu<MenuItem> contextMenuRef;
    private string lastAction = "None";
    private string actionTime = "";
    
    private async Task HandleFileAction(MenuEventArgs<MenuItem> args) {
        lastAction = args.Item.Text;
        actionTime = DateTime.Now.ToString("HH:mm:ss.fff");
        
        // Simulate async operation
        await Task.Delay(200);
        
        // Auto-close menu
        await contextMenuRef.Close();
    }
}

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
        cursor: context-menu;
    }
</style>
```

