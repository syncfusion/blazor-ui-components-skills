# Menu Item Management

## Table of Contents
- [Disabled Property](#disabled-property)
- [Static Disable](#static-disable)
- [Dynamic Enable/Disable](#dynamic-enabledisable)
- [Conditional Disabling with OnOpen](#conditional-disabling-with-onopen)
- [Separator Property](#separator-property)

## Disabled Property

The `Disabled` property controls whether a menu item is active or grayed out/inactive.

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Cut"></MenuItem>
        <MenuItem Text="Copy"></MenuItem>
        <MenuItem Text="Paste" Disabled="true"></MenuItem>
    </MenuItems>
</SfContextMenu>

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

**Result:** "Paste" appears grayed out and cannot be clicked.

---

## Static Disable

Disable items at component definition time (hardcoded).

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Edit" Disabled="false"></MenuItem>
        <MenuItem Text="Delete" Disabled="true"></MenuItem>
        <MenuItem Text="Archive" Disabled="false"></MenuItem>
    </MenuItems>
</SfContextMenu>

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

**Use Case:** Fixed permissions (e.g., only admins see Delete)

---

## Dynamic Enable/Disable

Use component state to dynamically toggle item disabled status.

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Undo" Disabled="@isUndoDisabled"></MenuItem>
        <MenuItem Text="Redo" Disabled="@isRedoDisabled"></MenuItem>
        <MenuItem Text="Copy" Disabled="@isCopyDisabled"></MenuItem>
        <MenuItem Text="Paste" Disabled="@isPasteDisabled"></MenuItem>
    </MenuItems>
</SfContextMenu>

<div style="margin-top: 20px;">
    <button @onclick="PerformAction">Perform Action</button>
    <button @onclick="ToggleCopyState">Toggle Copy</button>
</div>

@code {
    private bool isUndoDisabled = true;
    private bool isRedoDisabled = true;
    private bool isCopyDisabled = false;
    private bool isPasteDisabled = true;
    
    private void PerformAction() {
        // Enable Undo if action was performed
        isUndoDisabled = false;
        // Disable Redo when new action is performed
        isRedoDisabled = true;
    }
    
    private void ToggleCopyState() {
        isCopyDisabled = !isCopyDisabled;
        // If copy is disabled, paste must be disabled too
        if (isCopyDisabled) {
            isPasteDisabled = true;
        }
    }
}

<style>
    #target {
        border: 1px dashed; height: 100px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

**Logic:**
- `isUndoDisabled` = true initially (no history)
- User performs action → `isUndoDisabled = false`
- Paste only enabled if something was copied
- Dynamic state binding with `Disabled="@property"`

---

## Conditional Disabling with OnOpen

Use the `OnOpen` event to conditionally disable sub-menu items based on parent context.

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="View">
            <MenuItems>
                <MenuItem Text="Large Icons" Disabled="@largIconsDisabled"></MenuItem>
                <MenuItem Text="Medium Icons" Disabled="@mediumIconsDisabled"></MenuItem>
                <MenuItem Text="Small Icons"></MenuItem>
            </MenuItems>
        </MenuItem>
        <MenuItem Text="Sort By"></MenuItem>
        <MenuItem Text="Refresh"></MenuItem>
        <MenuItem Separator="true"></MenuItem>
        <MenuItem Text="New"></MenuItem>
        <MenuItem Separator="true"></MenuItem>
        <MenuItem Text="Display Settings" Disabled="true"></MenuItem>
        <MenuItem Text="Personalize"></MenuItem>
    </MenuItems>
    <MenuEvents TValue="MenuItem" OnOpen="@BeforeOpenHandler"></MenuEvents>
</SfContextMenu>

@code {
    private bool largIconsDisabled = false;
    private bool mediumIconsDisabled = false;
    
    private void BeforeOpenHandler(BeforeOpenCloseMenuEventArgs<MenuItem> e) {
        // OnOpen fires for sub-menu opening
        if (e.ParentItem != null && e.ParentItem.Text == "View") {
            // Toggle disabled state for Medium Icons only when View submenu opens
            mediumIconsDisabled = !mediumIconsDisabled;
        }
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
- `BeforeOpenCloseMenuEventArgs<MenuItem>` provides event data
- `e.ParentItem` - Parent menu item being opened
- Check parent text to identify which submenu opened
- Modify disabled state before submenu displays

### Advanced: Disable Based on Selection

```cshtml
@code {
    private MenuItem selectedItem;
    
    private void BeforeOpenHandler(BeforeOpenCloseMenuEventArgs<MenuItem> e) {
        if (e.ParentItem?.Text == "Edit") {
            // Only enable Delete if something is selected
            if (selectedItem == null) {
                e.Items.FirstOrDefault(x => x.Text == "Delete").Disabled = true;
            }
        }
    }
}
```

---

## Separator Property

Use `Separator="true"` to add visual dividing lines between menu groups.

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Cut"></MenuItem>
        <MenuItem Text="Copy"></MenuItem>
        <MenuItem Text="Paste"></MenuItem>
        
        <MenuItem Separator="true"></MenuItem>
        
        <MenuItem Text="Select All"></MenuItem>
        <MenuItem Text="Find"></MenuItem>
        
        <MenuItem Separator="true"></MenuItem>
        
        <MenuItem Text="Delete"></MenuItem>
    </MenuItems>
</SfContextMenu>

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
</style>
```

**Result:** Menu items grouped into 3 sections by separator lines.

### Conditional Separator

```cshtml
@code {
    private bool showAdvanced = false;
    
    private void OnInitialized() {
        // Dynamically decide to show separator
    }
}

<MenuItems>
    <MenuItem Text="Basic Action 1"></MenuItem>
    <MenuItem Text="Basic Action 2"></MenuItem>
    @if (showAdvanced) {
        <MenuItem Separator="true"></MenuItem>
        <MenuItem Text="Advanced Action 1"></MenuItem>
        <MenuItem Text="Advanced Action 2"></MenuItem>
    }
</MenuItems>
```

---

## Complete Example: Role-Based Item Management

```cshtml
@using Syncfusion.Blazor.Navigations

<div id="target">Right click to open</div>

<SfContextMenu Target="#target" TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="View" Disabled="@(!HasPermission("view"))"></MenuItem>
        <MenuItem Text="Edit" Disabled="@(!HasPermission("edit"))"></MenuItem>
        <MenuItem Text="Delete" Disabled="@(!HasPermission("delete"))"></MenuItem>
        <MenuItem Separator="true"></MenuItem>
        <MenuItem Text="Share" Disabled="@(!HasPermission("share"))"></MenuItem>
        <MenuItem Text="Archive" Disabled="@(!HasPermission("archive"))"></MenuItem>
    </MenuItems>
    <MenuEvents TValue="MenuItem" ItemSelected="@OnItemSelected"></MenuEvents>
</SfContextMenu>

<p>User Role: <strong>@currentRole</strong></p>
<p>Last Action: <strong>@lastAction</strong></p>

@code {
    private string currentRole = "viewer"; // admin, editor, viewer
    private string lastAction = "None";
    
    // Permission matrix
    private Dictionary<string, string[]> rolePermissions = new() {
        { "admin", new[] { "view", "edit", "delete", "share", "archive" } },
        { "editor", new[] { "view", "edit", "share" } },
        { "viewer", new[] { "view" } }
    };
    
    private bool HasPermission(string action) {
        return rolePermissions[currentRole].Contains(action);
    }
    
    private void OnItemSelected(MenuEventArgs<MenuItem> args) {
        lastAction = args.Item.Text;
    }
    
    private void SwitchRole(string role) {
        currentRole = role;
        StateHasChanged();
    }
}

<style>
    #target {
        border: 1px dashed; height: 150px; padding: 10px;
        position: relative; text-align: center; color: gray;
    }
    
    .role-buttons {
        margin-top: 20px;
        display: flex; gap: 10px;
    }
</style>

<div class="role-buttons">
    <button @onclick="() => SwitchRole(\"admin\")">Admin</button>
    <button @onclick="() => SwitchRole(\"editor\")">Editor</button>
    <button @onclick="() => SwitchRole(\"viewer\")">Viewer</button>
</div>
```

**Features:**
- Role-based item visibility
- Permission matrix
- Dynamic disable based on role
- Real-time role switching

---

## Best Practices

1. **Disable vs Hide:** Use Disabled for "available but not now", hide for "never available"
2. **Separate Groups:** Use Separator to logically group related actions
3. **Clear Feedback:** Disabled items should appear visually distinct (grayed out)
4. **Explain Why:** Consider tooltips for disabled items (e.g., "Paste disabled: nothing copied")
5. **Consistency:** Keep permission logic consistent across all menus

