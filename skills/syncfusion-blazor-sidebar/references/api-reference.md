# Syncfusion Blazor Sidebar API Reference

## Table of Contents

- [Overview](#overview)
- [Properties](#properties)
- [Methods](#methods)
- [Events](#events)
- [Enumerations](#enumerations)
  - [SidebarType](#sidebartype)
  - [SidebarPosition](#sidebarposition)
- [Event Arguments](#event-arguments)
  - [ChangeEventArgs](#changeeventargs)
  - [EventArgs](#eventargs)
- [SfSidebarContainer](#sfsidebarcontainer)
- [Usage Examples](#usage-examples)
- [Property Use Cases](#property-use-cases)
- [Related Components](#related-components)
- [References](#references)

---

## Overview

The Syncfusion Blazor Sidebar (`SfSidebar`) is a responsive navigation component that expands and collapses to show/hide primary or secondary content alongside the main content area. It supports multiple sidebar types, positioning, state persistence, and programmatic control.

---

## Properties

| Property | Type | Default | Accepted Values | Description | Reference |
|----------|------|---------|-----------------|-------------|-----------|
| `Animate` | bool | true | true/false | Enables or disables animation transitions when expanding or collapsing the sidebar | [Styling & Animation](styling-customization.md#animation--rtl-support) |
| `ChildContent` | RenderFragment | - | - | Specifies the child content rendered inside the sidebar | [Getting Started](getting-started.md) |
| `CloseOnDocumentClick` | bool | false | true/false | Closes the sidebar when the document area is clicked | [Open/Close Control](open-close-control.md) \| [Use Case Example](#closeondocumentclick) |
| `DockSize` | string | "auto" | pixel values e.g., "72px" | Sets the width of the sidebar in dock state | [Responsive & Docking Features](responsive-docking.md) |
| `EnableDock` | bool | false | true/false | Enables docking mode (icon-only navigation) | [Responsive & Docking Features](responsive-docking.md) |
| `EnableGestures` | bool | true | true/false | Enables swipe gestures to expand/collapse on touch devices (mobile/tablet) | [Responsive & Docking Features](responsive-docking.md) \| [Use Case Example](#enablegestures) |
| `EnablePersistence` | bool | false | true/false | Persists the sidebar state (open/closed) in localStorage | [State Persistence & Target Context](state-persistence-targets.md) |
| `EnableRtl` | bool | false | true/false | Enables right-to-left (RTL) rendering for languages like Arabic, Hebrew, Urdu | [Styling & Animation](styling-customization.md#animation--rtl-support) |
| `HtmlAttributes` | Dictionary\<string, object\> | - | key-value pairs (e.g., disabled, value) | Allows you to add additional HTML attributes such as disabled, value, and more to the root element. **Note:** This property is deprecated. Use `@attributes` to set additional attributes for sidebar element. | [Use Case Example](#HtmlAttributes-for-Custom-Attributes) |
| `ID` | string | - | any string | Sets the unique identifier for the sidebar element (required for persistence) | [State Persistence & Target Context](state-persistence-targets.md) |
| `IsOpen` | bool | false | true/false | Gets or sets whether the sidebar is in open or closed state. **Note:** With Type="Auto", this is ignored on desktop (always expanded) and mobile (always collapsed) | [Open/Close Control](open-close-control.md) |
| `IsOpenChanged` | EventCallback\<bool\> | - | - | Event callback for two-way binding when `IsOpen` value changes via `@bind-IsOpen` | [Use Case Example](#IsOpenChanged-event) |
| `MediaQuery` | string | - | CSS media queries e.g., "(min-width: 600px)" | Sets media query to control responsive behavior of the sidebar | [Responsive & Docking Features](responsive-docking.md) |
| `Position` | SidebarPosition | Left | Left, Right | Controls sidebar position relative to main content | [Multiple Sidebars](multiple-sidebars.md) |
| `ShowBackdrop` | bool | false | true/false | Applies an overlay backdrop behind the sidebar when open (blocking interaction with main content) | [Getting Started](getting-started.md) |
| `Target` | string | - | CSS selector string | Specifies the target HTML element for sidebar context | [State Persistence & Target Context](state-persistence-targets.md) |
| `Type` | SidebarType | Auto | Auto, Push, Over, Slide | Defines the sidebar behavior type | [Open/Close Control](open-close-control.md) |
| `Width` | string | "auto" | pixel/percentage values e.g., "250px", "30%" | Sets the width of the sidebar | [Use Case Example](#width-property-variations) |
| `ZIndex` | int | 1000 | any integer | Sets the CSS z-index for overlay sidebar types (Over, Slide) | [Use Case Example](#zindex-for-overlay-sidebar) |

---

## Methods

| Method | Parameters | Return Type | Description |
|--------|------------|-------------|-------------|
| `GetProperties()` | - | Dictionary\<string, object\> | Returns all component properties as a dictionary for serialization/debugging |

**Example: Using GetProperties() Method**

```razor
@using Syncfusion.Blazor.Navigations

<SfSidebar @ref="sidebarObj" Width="250px" @bind-IsOpen="SidebarToggle">
    <ChildContent>
        <nav>Navigation Menu</nav>
    </ChildContent>
</SfSidebar>

<button @onclick="GetComponentProperties">Get Properties</button>

@code {
    SfSidebar sidebarObj;
    public bool SidebarToggle = false;

    private async Task GetComponentProperties()
    {
        var properties = await sidebarObj.GetProperties();
        
        foreach (var prop in properties)
        {
            Console.WriteLine($"{prop.Key}: {prop.Value}");
        }
    }
}
```

---

## Events

| Event | Arguments | Description | Usage Example |
|-------|-----------|-------------|---------------|
| `Changed` | ChangeEventArgs | Fires after the sidebar state (open/close) has successfully changed | `<SfSidebar Changed="OnStateChanged">` |
| `Created` | object | Fires when the sidebar component is created and initialized | `<SfSidebar Created="OnSidebarCreated">` |
| `Destroyed` | object | Fires when the sidebar component is destroyed | `<SfSidebar Destroyed="OnSidebarDestroyed">` |
| `IsOpenChanged` | bool | Fires when the `@bind-IsOpen` two-way binding value changes | `<SfSidebar @bind-IsOpen="isOpen">` |
| `OnClose` | EventArgs | Fires before the sidebar closes (allows cancellation) | `<SfSidebar OnClose="OnBeforeClose">` |
| `OnOpen` | EventArgs | Fires before the sidebar opens (allows cancellation) | `<SfSidebar OnOpen="OnBeforeOpen">` |

---

## Enumerations

### SidebarType

Defines the behavior type for the sidebar component.

| Value | Description |
|-------|-------------|
| `Auto` | Sidebar with `Over` type in mobile resolution and `Push` type in other higher resolutions (default) |
| `Push` | Sidebar pushes the main content area to appear side-by-side, shrinking the main content within screen width |
| `Over` | Sidebar floats over the main content area without pushing it |
| `Slide` | Sidebar translates the main content area based on sidebar width; main content is not adjusted within screen width |

### SidebarPosition

Defines the positioning of the sidebar.

| Value | Description |
|-------|-------------|
| `Left` | Sidebar positioned on the left side (default) |
| `Right` | Sidebar positioned on the right side |

---

## Event Arguments

### ChangeEventArgs

Provides data for the `Changed` event when the sidebar state has successfully transitioned.

**Example: Handling ChangeEventArgs**

```razor
@using Syncfusion.Blazor.Navigations

<SfSidebar @ref="sidebarObj" 
           Width="250px" 
           Changed="OnStateChanged"
           @bind-IsOpen="SidebarToggle">
    <ChildContent>
        <nav>Navigation Menu</nav>
    </ChildContent>
</SfSidebar>

@code {
    SfSidebar sidebarObj;
    public bool SidebarToggle = false;

    private void OnStateChanged(ChangeEventArgs args)
    {
        // Access the sidebar element reference
        var element = args.Element;
        
        // Check if user interaction triggered this or programmatic
        if (args.IsInteracted) 
        {
            Console.WriteLine("User triggered state change");
        }
        else
        {
            Console.WriteLine("Programmatic state change");
        }
        
        // Get event name
        Console.WriteLine($"Event name: {args.Name}");
    }
}
```

| Property | Type | Description |
|----------|------|-------------|
| `Element` | ElementReference | DOM element reference for the sidebar component |
| `IsInteracted` | bool | True if closed/opened via user interaction (click/swipe), false if programmatic |
| `Name` | string | Returns the event name ("Changed") |

### EventArgs

Base event arguments for `OnOpen` and `OnClose` events (fired before state change).

**Example: Handling OnOpen and OnClose Events**

```razor
@using Syncfusion.Blazor.Navigations

<SfSidebar @ref="sidebarObj" 
           Width="250px" 
           OnOpen="OnBeforeOpen"
           OnClose="OnBeforeClose"
           @bind-IsOpen="SidebarToggle">
    <ChildContent>
        <nav>Navigation Menu</nav>
    </ChildContent>
</SfSidebar>

@code {
    SfSidebar sidebarObj;
    public bool SidebarToggle = false;

    private void OnBeforeOpen(EventArgs args)
    {
        // Prevent opening if certain conditions are not met
        args.Cancel = false;
        
        // Check if user or programmatic
        if (!args.IsInteracted)
        {
            Console.WriteLine("Programmatic open triggered");
        }
        
        // Get interaction coordinates
        double? x = args.Left;
        double? y = args.Top;
        Console.WriteLine($"Click position - X: {x}, Y: {y}");
        
        // Get event name
        Console.WriteLine($"Event: {args.Name}");
    }

    private void OnBeforeClose(EventArgs args)
    {
        // Get the element reference
        var element = args.Element;
        
        // Prevent closing conditionally
        if (!args.IsInteracted)
        {
            args.Cancel = true;
            Console.WriteLine("Programmatic close prevented");
        }
    }
}
```

| Property | Type | Description |
|----------|------|-------------|
| `Cancel` | bool | Set to `true` to prevent the action (open/close) |
| `Element` | ElementReference | DOM element reference for the sidebar |
| `IsInteracted` | bool | True if triggered by user interaction |
| `Left` | double? | ClientX position of the interaction point (mouse/touch) |
| `Name` | string | Event name ("OnOpen" or "OnClose") |
| `Top` | double? | ClientY position of the interaction point (mouse/touch) |

---

## SfSidebarContainer

Parent container component for the Sidebar.

### Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ChildContent` | RenderFragment | - | Specifies the child content (typically includes SfSidebar component) |

**Example: Using SfSidebarContainer**

```razor
@using Syncfusion.Blazor.Navigations

<SfSidebarContainer>
    <SfSidebar Width="250px" @bind-IsOpen="SidebarToggle">
        <ChildContent>
            <nav>Navigation Menu</nav>
        </ChildContent>
    </SfSidebar>

    <div>Main Content Area</div>
</SfSidebarContainer>

@code {
    public bool SidebarToggle = false;
}
```

---

## Usage Examples

### Basic Sidebar

```razor
@using Syncfusion.Blazor.Navigations

<SfSidebar @ref="sidebarObj" Width="250px" @bind-IsOpen="SidebarToggle">
    <ChildContent>
        <nav>Navigation Menu</nav>
    </ChildContent>
</SfSidebar>

@code {
    SfSidebar sidebarObj;
    public bool SidebarToggle = false;
}
```

### Sidebar with Events

```razor
<SfSidebar @ref="sidebarObj" 
           Width="250px" 
           @bind-IsOpen="SidebarToggle"
           Type="SidebarType.Push"
           OnOpen="OnSidebarOpen"
           OnClose="OnSidebarClose"
           Changed="OnStateChanged">
    <ChildContent>
        <!-- Sidebar content -->
    </ChildContent>
</SfSidebar>

@code {
    private void OnSidebarOpen(EventArgs args) 
    { 
        // Handle sidebar open
    }
    
    private void OnSidebarClose(EventArgs args) 
    { 
        // Handle sidebar close
    }
    
    private void OnStateChanged(ChangeEventArgs args)
    {
        // Handle state change
    }
}
```

### RTL Sidebar with Animation Control

```razor
<SfSidebar Width="250px" 
           EnableRtl="true" 
           Animate="false"
           @bind-IsOpen="SidebarToggle">
    <ChildContent>
        <!-- Content for RTL direction -->
    </ChildContent>
</SfSidebar>
```

### Lifecycle Events Example

```razor
<SfSidebar @ref="sidebarObj" 
           Width="250px"
           Created="OnSidebarCreated"
           Destroyed="OnSidebarDestroyed"
           @bind-IsOpen="IsOpen">
    <ChildContent>
        <p>Sidebar with lifecycle events</p>
    </ChildContent>
</SfSidebar>

@code {
    private void OnSidebarCreated(object args)
    {
        Console.WriteLine("Sidebar initialized");
    }
    
    private void OnSidebarDestroyed(object args)
    {
        Console.WriteLine("Sidebar destroyed");
    }
}
```

### Preventing Open/Close Actions

```razor
<SfSidebar Width="250px"
           OnOpen="OnBeforeOpen"
           OnClose="OnBeforeClose"
           @bind-IsOpen="IsOpen">
    <ChildContent>
        <p>Try closing the sidebar</p>
    </ChildContent>
</SfSidebar>

@code {
    private async Task OnBeforeOpen(EventArgs args)
    {
        // Allow opening
        args.Cancel = false;
        Console.WriteLine("Opening sidebar at position: X=" + args.Left + ", Y=" + args.Top);
    }
    
    private async Task OnBeforeClose(EventArgs args)
    {
        // Prevent closing if user didn't interact (programmatic)
        if (!args.IsInteracted)
        {
            args.Cancel = true;
            Console.WriteLine("Programmatic close prevented");
        }
    }
}
```

### State Changed Event with User Interaction Detection

```razor
<SfSidebar Width="250px"
           Changed="OnStateChanged"
           @bind-IsOpen="IsOpen">
    <ChildContent>
        <p>Sidebar content</p>
    </ChildContent>
</SfSidebar>

@code {
    private void OnStateChanged(ChangeEventArgs args)
    {
        if (args.IsInteracted)
        {
            Console.WriteLine($"User triggered state change: {args.Name}");
        }
        else
        {
            Console.WriteLine($"Programmatic state change: {args.Name}");
        }
    }
}
```

### Gesture Control on Mobile Example

```razor
<SfSidebar Width="250px"
           EnableGestures="true"
           Type="SidebarType.Over"
           @bind-IsOpen="IsOpen">
    <ChildContent>
        <p>Swipe left/right on mobile to toggle sidebar</p>
    </ChildContent>
</SfSidebar>

@code {
    public bool IsOpen = false;
}
```

### Using Component ID with Persistence

```razor
<SfSidebar ID="mySidebar"
           Width="250px"
           EnablePersistence="true"
           @bind-IsOpen="IsOpen">
    <ChildContent>
        <!-- State will be restored on page reload -->
    </ChildContent>
</SfSidebar>

@code {
    public bool IsOpen = false;
}
```

### Complete Advanced Example

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<div style="display: flex; height: 100vh;">
    <SfSidebar @ref="sidebarObj"
               ID="advancedSidebar"
               Width="280px"
               Type="SidebarType.Push"
               Position="SidebarPosition.Left"
               EnablePersistence="true"
               EnableGestures="true"
               Animate="true"
               ShowBackdrop="true"
               CloseOnDocumentClick="true"
               @bind-IsOpen="IsOpen"
               Created="OnCreated"
               OnOpen="OnBeforeOpen"
               OnClose="OnBeforeClose"
               Changed="OnChanged">
        <ChildContent>
            <nav style="padding: 20px;">
                <h3>Navigation</h3>
                <ul>
                    <li><a href="#home">Home</a></li>
                    <li><a href="#about">About</a></li>
                    <li><a href="#contact">Contact</a></li>
                </ul>
                <SfButton @onclick="CloseSidebar">Close</SfButton>
            </nav>
        </ChildContent>
    </SfSidebar>

    <div style="flex: 1; padding: 20px;">
        <SfButton @onclick="ToggleSidebar">Toggle Sidebar</SfButton>
        <p>Main content area</p>
    </div>
</div>

@code {
    private SfSidebar sidebarObj;
    private bool IsOpen = false;
    private string logMessage = "";

    private void OnCreated(object args)
    {
        logMessage = "Sidebar created";
        Console.WriteLine(logMessage);
    }

    private void OnBeforeOpen(EventArgs args)
    {
        args.Cancel = false;
        logMessage = $"Before Open - User Interaction: {args.IsInteracted}";
    }

    private void OnBeforeClose(EventArgs args)
    {
        args.Cancel = false;
        logMessage = $"Before Close - User Interaction: {args.IsInteracted}";
    }

    private void OnChanged(ChangeEventArgs args)
    {
        logMessage = $"State Changed - New state: {IsOpen}";
    }

    private void ToggleSidebar()
    {
        IsOpen = !IsOpen;
    }

    private void CloseSidebar()
    {
        IsOpen = false;
    }
}
```

---

## Property Use Cases

### CloseOnDocumentClick

Auto-closes the sidebar when clicking outside the sidebar area. Useful for dismissing navigation on desktop when users click the main content.

```razor
@using Syncfusion.Blazor.Navigations

<div style="display: flex; height: 100vh;">
    <SfSidebar Width="250px" 
               Type="SidebarType.Over"
               CloseOnDocumentClick="true"
               @bind-IsOpen="IsOpen">
        <ChildContent>
            <nav style="padding: 20px;">
                <h3>Menu</h3>
                <ul>
                    <li><a href="#home">Home</a></li>
                    <li><a href="#about">About</a></li>
                </ul>
            </nav>
        </ChildContent>
    </SfSidebar>

    <div style="flex: 1; padding: 20px;">
        <button @onclick="@(() => IsOpen = !IsOpen)">Toggle Sidebar</button>
        <p>Click anywhere here to close the sidebar automatically</p>
    </div>
</div>

@code {
    private bool IsOpen = false;
}
```

### EnableGestures

Enables swipe gestures on mobile/tablet devices to toggle the sidebar. Users can swipe left/right to expand/collapse without buttons.

```razor
@using Syncfusion.Blazor.Navigations

<div style="display: flex; height: 100vh;">
    <SfSidebar Width="250px" 
               Type="SidebarType.Over"
               EnableGestures="true"
               @bind-IsOpen="IsOpen">
        <ChildContent>
            <nav style="padding: 20px;">
                <h3>Navigation</h3>
                <ul>
                    <li><a href="#home">Home</a></li>
                    <li><a href="#products">Products</a></li>
                    <li><a href="#contact">Contact</a></li>
                </ul>
                <p style="margin-top: 30px; font-size: 12px; color: #999;">
                    💡 Swipe left/right on mobile to toggle
                </p>
            </nav>
        </ChildContent>
    </SfSidebar>

    <div style="flex: 1; padding: 20px;">
        <p>Swipe from the left edge (or tap the hamburger) to open navigation</p>
    </div>
</div>

@code {
    private bool IsOpen = false;
}
```

### IsOpenChanged Event

Fires when `@bind-IsOpen` two-way binding value changes. Useful for tracking state changes and syncing with parent components.

```razor
@using Syncfusion.Blazor.Navigations

<div style="display: flex; height: 100vh;">
    <SfSidebar Width="250px" 
               IsOpenChanged="OnIsOpenChanged">
        <ChildContent>
            <nav style="padding: 20px;">
                <h3>Sidebar</h3>
            </nav>
        </ChildContent>
    </SfSidebar>

    <div style="flex: 1; padding: 20px;">
        <p style="color: #666;">Last event: @lastEvent</p>
    </div>
</div>

@code {
    private string lastEvent = "None";

    private async Task OnIsOpenChanged(bool newValue)
    {
        lastEvent = $"IsOpenChanged fired with value: {newValue}";
        await Task.CompletedTask;
    }
}
```

### Width Property Variations

Demonstrates different width specifications for the sidebar (pixels, percentages, and auto).

```razor
@using Syncfusion.Blazor.Navigations

<div style="display: flex; gap: 20px; margin: 20px;">
    <div style="flex: 1; border: 1px solid #ccc; padding: 10px;">
        <h4>Width: 250px (Fixed Pixels)</h4>
        <SfSidebar Width="250px" @bind-IsOpen="IsOpen1">
            <ChildContent>
                <nav style="padding: 20px;">Fixed width sidebar</nav>
            </ChildContent>
        </SfSidebar>
    </div>

    <div style="flex: 1; border: 1px solid #ccc; padding: 10px;">
        <h4>Width: 30% (Percentage)</h4>
        <SfSidebar Width="30%" @bind-IsOpen="IsOpen2">
            <ChildContent>
                <nav style="padding: 20px;">Percentage-based width</nav>
            </ChildContent>
        </SfSidebar>
    </div>

    <div style="flex: 1; border: 1px solid #ccc; padding: 10px;">
        <h4>Width: auto (Content-based)</h4>
        <SfSidebar Width="auto" @bind-IsOpen="IsOpen3">
            <ChildContent>
                <nav style="padding: 20px;">Width: auto sidebar content</nav>
            </ChildContent>
        </SfSidebar>
    </div>
</div>

@code {
    private bool IsOpen1 = false;
    private bool IsOpen2 = false;
    private bool IsOpen3 = false;
}
```

### ZIndex for Overlay Sidebar

Controls the CSS z-index for overlay sidebar types (Over, Slide). Higher z-index places sidebar above other elements.

```razor
@using Syncfusion.Blazor.Navigations

<div style="position: relative;">
    <!-- Backdrop with higher z-index than sidebar -->
    <div id="backdrop" style="display: @(IsOpen ? "block" : "none"); 
                              position: fixed; top: 0; left: 0; 
                              width: 100%; height: 100%; 
                              background: rgba(0,0,0,0.5); 
                              z-index: 999;"></div>

    <!-- Sidebar with z-index 1000 (default) -->
    <SfSidebar Width="300px" 
               Type="SidebarType.Over"
               ZIndex="1000"
               ShowBackdrop="true"
               @bind-IsOpen="IsOpen">
        <ChildContent>
            <nav style="padding: 20px;">
                <h3>Navigation</h3>
                <ul>
                    <li><a href="#home">Home</a></li>
                    <li><a href="#about">About</a></li>
                </ul>
            </nav>
        </ChildContent>
    </SfSidebar>

    <div style="padding: 20px;">
        <button @onclick="@(() => IsOpen = !IsOpen)">Toggle Sidebar (z-index: 1000)</button>
        <p>The sidebar floats above the backdrop due to higher z-index value</p>
    </div>
</div>

@code {
    private bool IsOpen = false;
}
```

### HtmlAttributes for Custom Attributes

Adds additional HTML attributes such as `data-*`, `aria-*`, and custom attributes to the root element. **Note:** This property is deprecated—use `@attributes` directive instead.

```razor
@using Syncfusion.Blazor.Navigations

<div style="display: flex; height: 100vh;">
    <SfSidebar Width="250px" 
               HtmlAttributes="customAttributes"
               @bind-IsOpen="IsOpen">
        <ChildContent>
            <nav style="padding: 20px;">
                <h3>Accessible Navigation</h3>
                <ul>
                    <li><a href="#home">Home</a></li>
                    <li><a href="#about">About</a></li>
                    <li><a href="#contact">Contact</a></li>
                </ul>
            </nav>
        </ChildContent>
    </SfSidebar>

    <div style="flex: 1; padding: 20px;">
        <button @onclick="@(() => IsOpen = !IsOpen)">Toggle Sidebar</button>
        <p>This sidebar has custom data attributes for tracking and accessibility roles</p>
    </div>
</div>

@code {
    private bool IsOpen = false;

    private Dictionary<string, object> customAttributes = new Dictionary<string, object>
    {
        { "data-sidebar-id", "main-navigation" },
        { "data-region", "primary" },
        { "aria-label", "Main navigation menu" },
        { "role", "navigation" }
    };
}
```

**Recommended Modern Approach with @attributes:**

```razor
@using Syncfusion.Blazor.Navigations

<SfSidebar Width="250px" 
           @attributes="customAttributes"
           @bind-IsOpen="IsOpen">
    <ChildContent>
        <!-- Sidebar content -->
    </ChildContent>
</SfSidebar>

@code {
    private bool IsOpen = false;

    private Dictionary<string, object> customAttributes = new Dictionary<string, object>
    {
        { "data-sidebar-id", "main-navigation" },
        { "aria-label", "Main navigation menu" },
        { "role", "navigation" }
    };
}
```

---

## Related Components

- **SfSidebarContainer**: Parent container for managing sidebar layout
- **SfButton**: For trigger buttons to control sidebar visibility
- **SfListView**: For hierarchical menu items in sidebar
- **SfTreeView**: For tree-based navigation structures

---

## References

- [Getting Started](getting-started.md)
- [Open/Close Control](open-close-control.md)
- [Responsive & Docking Features](responsive-docking.md)
- [State Persistence & Target Context](state-persistence-targets.md)
- [Multiple Sidebars](multiple-sidebars.md)
- [Content Integration](content-integration.md)
- [Styling & Animation](styling-customization.md)

For complete API documentation, visit [Syncfusion Sidebar API Documentation](url)
