# Events and Interactions

## Table of Contents
- [MenuEvents Component Overview](#menuevents-component-overview)
- [Available Events](#available-events)
- [Event Handler Implementation](#event-handler-implementation)
- [Capturing Event Data](#capturing-event-data)
- [Event Handling Patterns](#event-handling-patterns)

## MenuEvents Component Overview

The `MenuEvents` component provides event handlers for user interactions with the Menu Bar. All events must be defined within a single `MenuEvents` component.

### Basic Syntax

```razor
<SfMenu TValue="MenuItem">
    <MenuItems>
        <!-- Menu items here -->
    </MenuItems>
    <MenuEvents TValue="MenuItem" 
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
    private void OnCreated() { }
    private void OnItemRender() { }
    private void OnOpen() { }
    private void OnClose() { }
    private void OnOpened() { }
    private void OnClosed() { }
    private void OnItemSelected(MenuEventArgs<MenuItem> args) { }
}
```

### Important: TValue Specification

When using events, the `TValue` parameter must be specified in the `MenuEvents` component and match the `SfMenu` TValue:

```razor
<!-- ❌ Incorrect: Missing TValue in MenuEvents -->
<MenuEvents Created="OnCreated"></MenuEvents>

<!-- ✅ Correct: TValue specified -->
<MenuEvents TValue="MenuItem" Created="OnCreated"></MenuEvents>
```

## Available Events

### 1. Created Event

Triggered once the Menu Bar has been successfully created and initialized.

**Use Case**: Initialize menu state, load data, or perform setup operations

```razor
<MenuEvents TValue="MenuItem" Created="OnMenuCreated"></MenuEvents>

@code {
    private void OnMenuCreated()
    {
        Console.WriteLine("Menu Bar created successfully");
        // Load initial data, set default selections, etc.
    }
}
```

### 2. OnItemRender Event

Triggered while rendering each individual menu item.

**Use Case**: Customize item appearance, apply conditional styling, or modify item properties before rendering

```razor
<MenuEvents TValue="MenuItem" OnItemRender="OnRenderItem"></MenuEvents>

@code {
    private void OnRenderItem(MenuEventArgs<MenuItem> args)
    {
        // Customize items based on conditions
        if (args.Item.Text == "Premium")
        {
            args.Item.IconCss = "e-icons e-star";
        }
    }
}
```

### 3. OnOpen Event

Triggered before opening a submenu.

**Use Case**: Prevent certain submenus from opening, load data on demand, or validate user permissions

```razor
<MenuEvents TValue="MenuItem" OnOpen="OnOpenSubmenu"></MenuEvents>

@code {
    private void OnOpenSubmenu(MenuEventArgs<MenuItem> args)
    {
        Console.WriteLine($"Opening: {args.Item.Text}");
        // Check permissions, load submenu data, etc.
        
        // To prevent opening, set: args.Cancel = true;
    }
}
```

### 4. OnClose Event

Triggered before closing a submenu.

**Use Case**: Save unsaved changes, validate selections, or clean up resources

```razor
<MenuEvents TValue="MenuItem" OnClose="OnCloseSubmenu"></MenuEvents>

@code {
    private void OnCloseSubmenu(MenuEventArgs<MenuItem> args)
    {
        Console.WriteLine($"Closing: {args.Item.Text}");
        // Save state, clean up, etc.
    }
}
```

### 5. Opened Event

Triggered after a submenu has been successfully opened.

**Use Case**: Focus elements, trigger animations, or perform post-open operations

```razor
<MenuEvents TValue="MenuItem" Opened="OnSubmenuOpened"></MenuEvents>

@code {
    private void OnSubmenuOpened(MenuEventArgs<MenuItem> args)
    {
        Console.WriteLine($"Submenu opened: {args.Item.Text}");
        // Perform animations or updates after opening
    }
}
```

### 6. Closed Event

Triggered after a submenu has been successfully closed.

**Use Case**: Clean up resources, update UI state, or perform post-close operations

```razor
<MenuEvents TValue="MenuItem" Closed="OnSubmenuClosed"></MenuEvents>

@code {
    private void OnSubmenuClosed(MenuEventArgs<MenuItem> args)
    {
        Console.WriteLine($"Submenu closed: {args.Item.Text}");
    }
}
```

### 7. ItemSelected Event

Triggered when a menu item is selected/clicked by the user.

**Use Case**: Navigate to pages, execute commands, or respond to user selections

```razor
<MenuEvents TValue="MenuItem" ItemSelected="OnSelectItem"></MenuEvents>

@code {
    private void OnSelectItem(MenuEventArgs<MenuItem> args)
    {
        string selectedText = args.Item.Text;
        string selectedUrl = args.Item.Url;
        
        Console.WriteLine($"Selected: {selectedText}");
        
        // Handle navigation, command execution, etc.
    }
}
```

## Event Handler Implementation

### Basic Event Handler

```razor
@code {
    private void OnItemSelected(MenuEventArgs<MenuItem> args)
    {
        var item = args.Item;
        Console.WriteLine($"Item: {item.Text}");
    }
}
```

### Async Event Handler

```razor
@code {
    private async Task OnItemSelectedAsync(MenuEventArgs<MenuItem> args)
    {
        var item = args.Item;
        await ProcessSelection(item);
    }
    
    private async Task ProcessSelection(MenuItem item)
    {
        // Async operations
        await Task.Delay(100);
    }
}
```

### Event Handler with State Updates

```razor
@page "/menu-events"
@using Syncfusion.Blazor.Navigations

<div>
    <p>Selected Item: @selectedItemText</p>
    <p>Event Triggered: @lastEventName</p>
</div>

<SfMenu TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="File">
            <MenuItems>
                <MenuItem Text="Open"></MenuItem>
                <MenuItem Text="Save"></MenuItem>
                <MenuItem Text="Exit"></MenuItem>
            </MenuItems>
        </MenuItem>
        <MenuItem Text="Edit">
            <MenuItems>
                <MenuItem Text="Cut"></MenuItem>
                <MenuItem Text="Copy"></MenuItem>
            </MenuItems>
        </MenuItem>
    </MenuItems>
    <MenuEvents TValue="MenuItem" 
        ItemSelected="OnItemSelected"
        OnOpen="OnOpen"
        Closed="OnClosed">
    </MenuEvents>
</SfMenu>

@code {
    private string selectedItemText = "None";
    private string lastEventName = "Waiting for event...";

    private void OnItemSelected(MenuEventArgs<MenuItem> args)
    {
        selectedItemText = args.Item.Text;
        lastEventName = "ItemSelected";
    }

    private void OnOpen(MenuEventArgs<MenuItem> args)
    {
        lastEventName = $"Opening: {args.Item.Text}";
    }

    private void OnClosed(MenuEventArgs<MenuItem> args)
    {
        lastEventName = $"Closed: {args.Item.Text}";
    }
}
```

## Capturing Event Data

### MenuEventArgs Properties

The `MenuEventArgs<TValue>` contains:

| Property | Type | Description |
|----------|------|-------------|
| `Item` | TValue | The menu item that triggered the event |
| `Cancel` | bool | Set to true to cancel the operation (OnOpen, OnClose) |

### Accessing Item Information

```csharp
private void OnItemSelected(MenuEventArgs<MenuItem> args)
{
    // Access item properties
    string text = args.Item.Text;
    string iconCss = args.Item.IconCss;
    string url = args.Item.Url;
    bool isDisabled = args.Item.Disabled;
    bool isSeparator = args.Item.Separator;
}
```

### Canceling Operations

```csharp
private void OnOpen(MenuEventArgs<MenuItem> args)
{
    // Prevent "Admin" submenu from opening
    if (args.Item.Text == "Admin")
    {
        args.Cancel = true;
        Console.WriteLine("Admin menu is locked");
    }
}
```

## Event Handling Patterns

### Pattern 1: Navigation on Selection

```csharp
private void OnItemSelected(MenuEventArgs<MenuItem> args)
{
    if (!string.IsNullOrEmpty(args.Item.Url))
    {
        // Navigation handled by Url property
        // Or programmatically:
        NavigationManager.NavigateTo(args.Item.Url);
    }
}
```

### Pattern 2: Dynamic Menu Loading

```csharp
private async Task OnOpen(MenuEventArgs<MenuItem> args)
{
    if (args.Item.Text == "Reports" && !isReportsLoaded)
    {
        // Load reports dynamically on first open
        await LoadReports();
        isReportsLoaded = true;
    }
}

private async Task LoadReports()
{
    // Fetch data from API
    await Task.Delay(500);
}
```

### Pattern 3: User Feedback

```csharp
private string toastMessage = "";
private bool showToast = false;

private void OnItemSelected(MenuEventArgs<MenuItem> args)
{
    toastMessage = $"You selected: {args.Item.Text}";
    showToast = true;
    
    // Auto-hide toast after 3 seconds
    Task.Delay(3000).ContinueWith(_ => 
    {
        showToast = false;
        StateHasChanged();
    });
}
```

### Pattern 4: Permission-Based Menu Access

```csharp
private void OnOpen(MenuEventArgs<MenuItem> args)
{
    var userRole = GetUserRole();
    var allowedItems = new[] { "Public", "User" };
    
    if (!allowedItems.Contains(args.Item.Text) && userRole != "Admin")
    {
        args.Cancel = true;
        Console.WriteLine("Access denied");
    }
}

private string GetUserRole()
{
    // Fetch from authentication context
    return "User";
}
```

### Pattern 5: Multi-Event Tracking

```razor
<div>
    <h3>Event Log</h3>
    @foreach (var entry in eventLog)
    {
        <div>@entry</div>
    }
</div>

<SfMenu TValue="MenuItem">
    <!-- Menu items -->
    <MenuEvents TValue="MenuItem" 
        Created="LogEvent_Created"
        OnOpen="LogEvent_OnOpen"
        OnClose="LogEvent_OnClose"
        ItemSelected="LogEvent_ItemSelected">
    </MenuEvents>
</SfMenu>

@code {
    private List<string> eventLog = new();

    private void LogEvent_Created()
    {
        AddLog("Menu created");
    }

    private void LogEvent_OnOpen(MenuEventArgs<MenuItem> args)
    {
        AddLog($"Opening: {args.Item.Text}");
    }

    private void LogEvent_OnClose(MenuEventArgs<MenuItem> args)
    {
        AddLog($"Closing: {args.Item.Text}");
    }

    private void LogEvent_ItemSelected(MenuEventArgs<MenuItem> args)
    {
        AddLog($"Selected: {args.Item.Text}");
    }

    private void AddLog(string message)
    {
        eventLog.Insert(0, $"[{DateTime.Now:HH:mm:ss}] {message}");
        if (eventLog.Count > 10) eventLog.RemoveAt(10); // Keep last 10
    }
}
```

## Best Practices

1. **Always specify TValue**: Ensure MenuEvents has TValue matching SfMenu
2. **Avoid expensive operations**: Don't perform heavy computations in OnItemRender (fires frequently)
3. **Use async when needed**: Use async Task for operations that might take time
4. **Cancel carefully**: Only cancel OnOpen/OnClose when necessary for security/validation
5. **Manage state**: Call StateHasChanged() if event handlers modify component state
6. **Error handling**: Wrap event handlers in try-catch for production code
