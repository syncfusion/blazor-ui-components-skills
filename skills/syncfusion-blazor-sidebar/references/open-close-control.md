# Open and Close Sidebar Control

This guide covers controlling sidebar visibility programmatically using the `IsOpen` property and event handlers.

## Table of Contents
- [IsOpen Property](#isopen-property)
- [Two-Way Binding](#two-way-binding)
- [Toggle Method](#toggle-method)
- [Close Method](#close-method)
- [Button-Based Control](#button-based-control)
- [Multiple Trigger Points](#multiple-trigger-points)

## IsOpen Property

The `IsOpen` property gets or sets a boolean value which indicates whether the Sidebar component's state is open or close.

**⚠️ CRITICAL - Default Type is `Auto`:**

By default, the Sidebar `Type` is set to `Auto`. When `Type="Auto"`:
- **Desktop**: Sidebar is **ALWAYS EXPANDED (Visible)** - `IsOpen` value is **IGNORED**
- **Mobile**: Sidebar is **ALWAYS COLLAPSED (Hidden)** - `IsOpen` value is **IGNORED**

**Example - Default Type="Auto" (IsOpen has NO effect):**
```razor
<!-- Default Type="Auto" - IsOpen is ignored on desktop and mobile -->
<SfSidebar Width="250px" IsOpen="false">
    <ChildContent>
        <div>Desktop: ALWAYS Shows | Mobile: ALWAYS Hidden (Regardless of IsOpen)</div>
    </ChildContent>
</SfSidebar>
```

**To Use IsOpen for Manual Control - Set Type Explicitly:**

You must explicitly specify `Type="Push"`, `Type="Over"`, or `Type="Slide"` to have manual control over sidebar visibility using the `IsOpen` property:

```razor
<!-- Type="Push" - IsOpen works as expected on all devices -->
<SfSidebar Width="250px" Type="SidebarType.Push" IsOpen="false">
    <ChildContent>
        <div>Sidebar is HIDDEN on all devices</div>
    </ChildContent>
</SfSidebar>

<!-- Type="Over" - Full IsOpen control -->
<SfSidebar Width="250px" Type="SidebarType.Over" IsOpen="true">
    <ChildContent>
        <div>Sidebar is VISIBLE on all devices</div>
    </ChildContent>
</SfSidebar>
```

## Two-Way Binding

The `@bind-IsOpen` directive works only when you explicitly set a `Type` other than `Auto`.

**⚠️ With Default Type="Auto" - Binding Has NO Effect:**

```razor
@using Syncfusion.Blazor.Navigations

<!-- Default Type="Auto" - @bind-IsOpen does NOTHING -->
<!-- Desktop: ALWAYS Expanded | Mobile: ALWAYS Collapsed -->
<SfSidebar @ref="sidebarObj" Width="250px" @bind-IsOpen="SidebarToggle">
    <ChildContent>
        <div style="text-align: center;" class="text-content">
            Desktop: Always Visible | Mobile: Always Hidden
            (SidebarToggle value: @SidebarToggle has NO effect)
        </div>
    </ChildContent>
</SfSidebar>

@code {
    SfSidebar sidebarObj;
    public bool SidebarToggle = false; // This does nothing with Type="Auto"
}
```

**✓ For Binding to Work - Set Type Explicitly:**

```razor
<!-- Type="Push" - @bind-IsOpen WORKS correctly -->
<SfSidebar @ref="sidebarObjPush" Width="250px" Type="SidebarType.Push" @bind-IsOpen="SidebarTogglePush">
    <ChildContent>
        <div style="text-align: center;" class="text-content">
            Sidebar state: @(SidebarTogglePush ? "Open" : "Closed")
        </div>
    </ChildContent>
</SfSidebar>

@code {
    SfSidebar sidebarObjPush;
    public bool SidebarTogglePush = false;
    
    // Now this boolean actually controls the sidebar
    // SidebarTogglePush = true  → Sidebar visible
    // SidebarTogglePush = false → Sidebar hidden
}
```

## Toggle Method

Create a method to toggle the sidebar between open and closed states.

**⚠️ With Default Type="Auto" - Toggle Methods Do NOT Work:**

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<!-- Default Type="Auto" - Toggle() has NO EFFECT -->
<!-- Desktop: ALWAYS Expanded | Mobile: ALWAYS Collapsed -->
<SfSidebar @ref="sidebarObj" Width="250px" @bind-IsOpen="SidebarToggle">
    <ChildContent>
        <div>Sidebar state cannot be changed with Toggle()</div>
    </ChildContent>
</SfSidebar>

<SfButton @onclick="Toggle">Toggle Button (Does Nothing)</SfButton>

@code {
    SfSidebar sidebarObj;
    public bool SidebarToggle = false;

    public void Toggle()
    {
        SidebarToggle = !SidebarToggle; // This has NO EFFECT with Type="Auto"
    }
}
```

**✓ To Use Toggle - Set Type Explicitly:**

```razor
<!-- Type="Push" - Toggle() WORKS correctly -->
<SfSidebar @ref="sidebarObjPush" Width="250px" Type="SidebarType.Push" @bind-IsOpen="SidebarTogglePush">
    <ChildContent>
        <div>Sidebar: @(SidebarTogglePush ? "Open" : "Closed")</div>
    </ChildContent>
</SfSidebar>

<SfButton @onclick="TogglePush" IsToggle="true">Toggle Sidebar (Works!)</SfButton>

@code {
    SfSidebar sidebarObjPush;
    public bool SidebarTogglePush = false;

    public void TogglePush()
    {
        SidebarTogglePush = !SidebarTogglePush; // Now this works!
    }
}
```

## Close Method

Create a method to explicitly close the sidebar.

**⚠️ With Default Type="Auto" - Close() Does NOT Work:**

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<!-- Default Type="Auto" - Close() has NO EFFECT -->
<!-- Desktop: ALWAYS Expanded | Mobile: ALWAYS Collapsed -->
<SfSidebar @ref="sidebarObj" Width="250px" @bind-IsOpen="SidebarToggle">
    <ChildContent>
        <div>Cannot be closed on desktop</div>
    </ChildContent>
</SfSidebar>

<SfButton @onclick="Close">Close Button (Does Nothing)</SfButton>

@code {
    SfSidebar sidebarObj;
    public bool SidebarToggle = false;

    public void Close()
    {
        SidebarToggle = false; // This has NO EFFECT with Type="Auto"
    }
}
```

**✓ To Use Close - Set Type Explicitly:**

```razor
<!-- Type="Push" - Close() WORKS correctly -->
<SfSidebar @ref="sidebarObjPush" Width="250px" Type="SidebarType.Push" @bind-IsOpen="SidebarTogglePush">
    <ChildContent>
        <div>Sidebar can be closed</div>
    </ChildContent>
</SfSidebar>

<SfButton @onclick="ClosePush">Close Sidebar (Works!)</SfButton>

@code {
    SfSidebar sidebarObjPush;
    public bool SidebarTogglePush = false;

    public void ClosePush()
    {
        SidebarTogglePush = false; // Now this works!
    }
}
```

## Button-Based Control

Implement sidebar control with buttons.

**⚠️ WITH DEFAULT Type="Auto" - Buttons Do NOT Work:**

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<div id="header" style="height:45px;text-align: center;color:white;background-color:midnightblue;font-size:1.2rem;line-height:45px;">
    Header
</div>

<!-- Default Type="Auto" -->
<!-- Desktop: ALWAYS EXPANDED | Mobile: ALWAYS COLLAPSED -->
<!-- Buttons have NO EFFECT -->
<SfSidebar @ref="sidebarObj" Width="250px" @bind-IsOpen="SidebarToggle">
    <ChildContent>
        <div style="text-align: center;" class="text-content">
            <span>Sidebar Content</span>
            <span>
                <SfButton @onclick="Close" CssClass="e-btn close-btn">Close Button (Does Nothing)</SfButton>
            </span>
        </div>
    </ChildContent>
</SfSidebar>

<div class="text-content" style="text-align: center;">
    <div>Main content</div>
    <div>
        <SfButton @onclick="Toggle" IsToggle="true" CssClass="e-btn e-info">Toggle Button (Does Nothing)</SfButton>
    </div>
</div>

@code{
    SfSidebar sidebarObj;
    public bool SidebarToggle = false;

    public void Close() => SidebarToggle = false;    // NO EFFECT
    public void Toggle() => SidebarToggle = !SidebarToggle;  // NO EFFECT
}
```

**✓ FOR WORKING BUTTONS - Set Type Explicitly:**

```razor
<!-- Type="Push" - Buttons WORK correctly -->
<SfSidebar @ref="sidebarObjPush" Width="250px" Type="SidebarType.Push" @bind-IsOpen="SidebarTogglePush">
    <ChildContent>
        <div style="text-align: center;" class="text-content">
            <span>Sidebar: @(SidebarTogglePush ? "Open" : "Closed")</span>
            <span>
                <SfButton @onclick="ClosePush" CssClass="e-btn close-btn">Close Sidebar</SfButton>
            </span>
        </div>
    </ChildContent>
</SfSidebar>

<SfButton @onclick="TogglePush" IsToggle="true" CssClass="e-btn e-info">Toggle Sidebar</SfButton>

@code{
    SfSidebar sidebarObjPush;
    public bool SidebarTogglePush = false;

    public void ClosePush() => SidebarTogglePush = false;      // Works!
    public void TogglePush() => SidebarTogglePush = !SidebarTogglePush;  // Works!
}
```

## Multiple Trigger Points

Control the sidebar from multiple locations in your layout.

**⚠️ WITH DEFAULT Type="Auto" - ALL Buttons Do NOT Work:**

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<!-- Header with menu button (Does Nothing) -->
<div id="header" style="height:45px;text-align: center;color:white;background-color:midnightblue;font-size:1.2rem;line-height:45px;">
    <SfButton @onclick="Toggle" CssClass="e-btn header-toggle">☰ Menu (Does Nothing)</SfButton>
</div>

<!-- Default Type="Auto" - ALWAYS EXPANDED on desktop, ALWAYS COLLAPSED on mobile -->
<SfSidebar @ref="sidebarObj" Width="250px" @bind-IsOpen="SidebarToggle">
    <ChildContent>
        <nav class="sidebar-nav">
            <ul>
                <li><a href="#home">Home</a></li>
                <li><a href="#about">About</a></li>
                <li><SfButton @onclick="Close" CssClass="e-btn close-btn">Close Button (Does Nothing)</SfButton></li>
            </ul>
        </nav>
    </ChildContent>
</SfSidebar>

<!-- Main Content -->
<div class="main-content" style="padding: 3rem;">
    <div style="text-align: center;">
        <p>Desktop: Sidebar Always Visible</p>
        <SfButton @onclick="Toggle" CssClass="e-btn e-info">Toggle Button (Does Nothing)</SfButton>
    </div>
</div>

@code{
    SfSidebar sidebarObj;
    public bool SidebarToggle = false;

    public void Toggle() => SidebarToggle = !SidebarToggle;  // NO EFFECT
    public void Close() => SidebarToggle = false;            // NO EFFECT
}
```

**✓ FOR WORKING MULTI-TRIGGER PATTERN - Set Type Explicitly:**

```razor
<!-- Type="Push" - Multiple buttons work correctly -->
<SfSidebar @ref="sidebarObjPush" Width="250px" Type="SidebarType.Push" @bind-IsOpen="SidebarTogglePush">
    <ChildContent>
        <nav class="sidebar-nav">
            <ul>
                <li><a href="#home">Home</a></li>
                <li><a href="#about">About</a></li>
                <li><SfButton @onclick="ClosePush" CssClass="e-btn close-btn">Close Sidebar</SfButton></li>
            </ul>
        </nav>
    </ChildContent>
</SfSidebar>

<!-- Main Content with multiple trigger points -->
<div class="main-content" style="padding: 3rem;">
    <SfButton @onclick="TogglePush">Toggle Sidebar (Header)</SfButton>
    <SfButton @onclick="ClosePush">Close Sidebar (Main)</SfButton>
</div>

@code {
    SfSidebar sidebarObjPush;
    public bool SidebarTogglePush = false;

    public void TogglePush() => SidebarTogglePush = !SidebarTogglePush;  // Works!
    public void ClosePush() => SidebarTogglePush = false;                // Works!
}
```

**Key Points:**

- **⚠️ CRITICAL: Default Type is `Auto`** - All above methods have **NO EFFECT** with default type:
  - `IsOpen=false` will **NOT hide** the sidebar on desktop
  - `IsOpen=true` will **NOT show** the sidebar on mobile
  - `Toggle()`, `Close()`, and `@bind-IsOpen` binding do **NOTHING**
  - Button click handlers do **NOTHING**

- **With Type="Auto" (default):**
  - Desktop: Sidebar is **ALWAYS EXPANDED/VISIBLE** regardless of `IsOpen` value
  - Mobile: Sidebar is **ALWAYS COLLAPSED/HIDDEN** regardless of `IsOpen` value

- **With Explicit Type (`"Push"`, `"Over"`, `"Slide"`):**
  - `IsOpen` property controls visibility on **all devices**
  - `@bind-IsOpen="variable"` bindings work correctly
  - `Toggle()` and `Close()` methods work correctly
  - Button handlers work correctly

- **Best Practice:** Always explicitly set `Type` if you need to programmatically control sidebar visibility
