# Styling and Customization

This guide covers CSS customization of the Sidebar component's appearance, states, and behavior.

## Table of Contents
- [Sidebar Root Customization](#sidebar-root-customization)
- [Position-Based Styling](#position-based-styling)
- [State-Based Styling](#state-based-styling)
- [Type-Based Styling](#type-based-styling)
- [Dock State Styling](#dock-state-styling)
- [Backdrop Customization](#backdrop-customization)
- [RTL Support](#rtl-support)
- [Transition Effects](#transition-effects)

## Sidebar Root Customization

Customize the sidebar root element background and styling:

```css
/* Basic sidebar styling */
.e-sidebar {
    background: #898b2b;
    color: #333;
}

/* Add padding or borders */
.e-sidebar {
    border-right: 2px solid #ddd;
}
```

## Position-Based Styling

Customize left and right positioned sidebars differently:

### Left Position Styling

```css
.e-sidebar.e-left {
    border-right: 2px solid red;
    background-color: #f5f5f5;
}
```

### Right Position Styling

```css
.e-sidebar.e-right {
    border-left: 2px solid red;
    background-color: #f5f5f5;
}
```

## State-Based Styling

Customize sidebar appearance based on open/closed state:

### Open State (Left)

```css
.e-sidebar.e-left.e-open {
    transition: transform 2.5s ease;
    background-color: #ffffff;
    box-shadow: 2px 0 5px rgba(0,0,0,0.1);
}
```

### Open State (Right)

```css
.e-sidebar.e-right.e-open {
    transition: transform 2.5s ease;
    background-color: #ffffff;
    box-shadow: -2px 0 5px rgba(0,0,0,0.1);
}
```

### Closed State (Left)

```css
.e-sidebar.e-left.e-transition.e-close {
    transition: transform 2.5s ease, visibility 1200ms;
}
```

### Closed State (Right)

```css
.e-sidebar.e-right.e-transition.e-close {
    transition: transform 2.5s ease, visibility 1200ms;
}
```

## Type-Based Styling

Customize sidebars based on their Type property:

### Auto Type

```css
.e-sidebar.e-left.e-auto {
    background-color: pink;
}
```

### Push Type

```css
.e-sidebar.e-left.e-push {
    background-color: beige;
}
```

### Over Type

```css
.e-sidebar.e-left.e-over {
    background-color: aqua;
}
```

### Slide Type

```css
.e-sidebar.e-left.e-slide {
    background-color: green;
}
```

## Dock State Styling

When dock support is enabled, the `e-dock` class is added. Customize docked sidebars:

```css
/* Dock state styling */
.e-sidebar.e-dock {
    background: #2d323e;
    width: 72px;
}

/* Customize dock with left position */
.e-sidebar.e-left.e-dock {
    border-right: 1px solid #ccc;
}

/* Customize dock with right position */
.e-sidebar.e-right.e-dock {
    border-left: 1px solid #ccc;
}

/* Hide text when docked */
.e-sidebar.e-dock.e-close span.e-text {
    display: none;
}

/* Show text when expanded */
.e-sidebar.e-dock.e-open span.e-text {
    display: inline-block;
}
```

### Dock with Icons Example

```css
.sidebar-item {
    text-align: center;
    padding: 10px;
}

.e-sidebar .e-icons {
    color: #c0c2c5;
    font-size: 24px;
}

.e-open .e-icons {
    margin-right: 16px;
}

.e-open .e-text {
    font-size: 15px;
    overflow: hidden;
}

.e-sidebar.e-dock.e-close .sidebar-item {
    text-align: center;
}

.e-sidebar.e-dock.e-open .sidebar-item {
    text-align: left;
    padding-left: 15px;
}
```

## Backdrop Customization

Customize the overlay backdrop that appears behind the sidebar:

```css
/* Default backdrop styling */
.e-sidebar-overlay {
    background-color: rgba(0, 0, 0, 0.5);
}

/* Custom backdrop colors */
.e-sidebar-overlay {
    background-color: aqua;
}

/* Transparent backdrop */
.e-sidebar-overlay {
    background-color: rgba(0, 0, 0, 0.2);
}

/* Dark overlay */
.e-sidebar-overlay {
    background-color: rgba(0, 0, 0, 0.8);
}
```

## Animation & RTL Support

### Enabling Animations

Control sidebar animation transitions using the `Animate` property. By default, `Animate` is set to `true` to enable smooth transition animations when expanding or collapsing the sidebar.

**C# Component Properties:**
```razor
<!-- Enable animations (default) -->
<SfSidebar @ref="sidebarObj" Animate="true" Width="250px" @bind-IsOpen="SidebarToggle">
    <!-- Smooth expanding/collapsing transition -->
</SfSidebar>

<!-- Disable animations for instant expand/collapse -->
<SfSidebar @ref="sidebarObj" Animate="false" Width="250px" @bind-IsOpen="SidebarToggle">
    <!-- No transition animation -->
</SfSidebar>
```

### Right-to-Left (RTL) Support

The `EnableRtl` property displays the sidebar in right-to-left direction for RTL languages (Arabic, Hebrew, Urdu, etc.). When RTL support is enabled, the `e-rtl` class is added automatically.

**C# Component Properties:**
```razor
<!-- RTL enabled for right-to-left languages -->
<SfSidebar @ref="sidebarObj" EnableRtl="true" Width="250px" @bind-IsOpen="SidebarToggle">
    <!-- Sidebar content automatically mirrored -->
</SfSidebar>
```

**CSS Styling for RTL:**

```css
/* RTL sidebar styling - left-to-right mirror */
.e-sidebar.e-left.e-rtl {
    background-color: antiquewhite;
    border-left: 2px solid red;
    border-right: none;
}

.e-sidebar.e-right.e-rtl {
    background-color: antiquewhite;
    border-right: 2px solid red;
    border-left: none;
}
```

### Complete Animation & RTL Example

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<div id="header" style="height:45px;text-align: center;color:white;background-color:midnightblue;font-size:1.2rem;line-height:45px;">
    Header
</div>

<!-- Sidebar with animations disabled and RTL enabled -->
<SfSidebar @ref="sidebarObj" Animate="false" EnableRtl="true" Width="250px" @bind-IsOpen="SidebarToggle">
    <ChildContent>
        <div style="text-align: center;" class="text-content">
            <span>Sidebar</span>
            <span>
                <SfButton @onclick="Close" CssClass="e-btn close-btn">Close Sidebar</SfButton>
            </span>
        </div>
    </ChildContent>
</SfSidebar>

<div class="text-content" style="text-align: center;">Main content</div>

@code{
    SfSidebar sidebarObj;
    public bool SidebarToggle = false;
    
    public void Close()
    {
        SidebarToggle = false;
    }
}

<style>
    .e-sidebar {
        background-color: #f8f8f8;
        color: black;
    }

    .text-content {
        font-size: 1.5rem;
        padding: 3rem;
    }
</style>
```

## Transition Effects

Control sidebar animation and transition behavior. Animation can be enabled/disabled using the component's `Animate` property:

**Disabling Animations with Animate Property:**
```razor
<!-- Disable animations for instant expand/collapse (Animate="false") -->
<SfSidebar Animate="false" Width="250px" @bind-IsOpen="SidebarToggle">
    <!-- No smooth transition -->
</SfSidebar>
```

**CSS Transition Customization:**

```css
/* Smooth transition (default with Animate="true") */
.e-sidebar {
    transition: transform 0.3s ease-in-out;
}

/* Fast transition */
.e-sidebar {
    transition: transform 0.1s linear;
}

/* Custom easing */
.e-sidebar {
    transition: transform 0.5s cubic-bezier(0.25, 0.46, 0.45, 0.94);
}

/* Multiple properties */
.e-sidebar {
    transition: transform 0.3s ease, width 0.3s ease, opacity 0.3s ease;
}

/* No animation (similar to Animate="false") */
.e-sidebar {
    transition: none;
}

/* Custom animation for state transitions */
.e-sidebar.e-transition {
    animation: slideIn 0.3s ease-out;
}

@keyframes slideIn {
    from {
        transform: translateX(-100%);
        opacity: 0;
    }
    to {
        transform: translateX(0);
        opacity: 1;
    }
}
```

## Complete Styling Example

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<div id="header" style="height:55px;text-align: center;color:white;background-color:midnightblue;padding-top:5px;">
    <SfButton @onclick="Toggle" CssClass="e-btn header-btn">☰ Menu</SfButton>
</div>

<SfSidebar @ref="sidebarObj" Width="250px" @bind-IsOpen="SidebarToggle">
    <ChildContent>
        <nav class="sidebar-menu">
            <ul>
                <li>Home</li>
                <li>About</li>
                <li>Services</li>
                <li>Contact</li>
            </ul>
        </nav>
    </ChildContent>
</SfSidebar>

<div class="main-content">
    <p>Main content area with custom styling</p>
</div>

@code {
    SfSidebar sidebarObj;
    public bool SidebarToggle = false;

    public void Toggle()
    {
        SidebarToggle = !SidebarToggle;
    }
}

<style>
    /* Sidebar base styling */
    .e-sidebar {
        background: linear-gradient(to bottom, #2c3e50, #34495e);
        color: #ecf0f1;
        transition: transform 0.3s ease;
    }

    /* Left position styling */
    .e-sidebar.e-left {
        border-right: 1px solid #1a252f;
    }

    /* Open state styling */
    .e-sidebar.e-left.e-open {
        box-shadow: 2px 0 8px rgba(0, 0, 0, 0.3);
    }

    /* Menu styling */
    .sidebar-menu ul {
        list-style: none;
        padding: 20px 0;
        margin: 0;
    }

    .sidebar-menu li {
        padding: 15px 20px;
        border-bottom: 1px solid rgba(255, 255, 255, 0.1);
        cursor: pointer;
        transition: background-color 0.2s ease;
    }

    .sidebar-menu li:hover {
        background-color: rgba(255, 255, 255, 0.1);
    }

    /* Backdrop styling */
    .e-sidebar-overlay {
        background-color: rgba(0, 0, 0, 0.5);
    }

    /* Main content styling */
    .main-content {
        padding: 30px;
        background-color: #f5f5f5;
        min-height: calc(100vh - 55px);
    }

    /* Button styling */
    .header-btn {
        background-color: #1a252f;
        color: white;
        border: none;
    }
</style>
```

**Styling Tips:**
- Always test transitions on different browsers for compatibility
- Use `e-dock` class when enabling dock mode for consistent styling
- Combine state classes (`.e-open`, `.e-close`, `.e-left`, `.e-right`) for precise control
- Use `e-rtl` for right-to-left language support
- Keep backdrop opacity between 0.2 and 0.8 for usability
- Use CSS transitions for smooth animations instead of JavaScript
