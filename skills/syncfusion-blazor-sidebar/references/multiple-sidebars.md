# Multiple Sidebars

This guide covers managing multiple sidebar instances with different positioning and coordination.

## Table of Contents
- [Multiple Sidebar Basics](#multiple-sidebar-basics)
- [Position Property](#position-property)
- [Left and Right Sidebars](#left-and-right-sidebars)
- [Main Content Container](#main-content-container)
- [Managing Multiple States](#managing-multiple-states)
- [Complete Example](#complete-example)

## Multiple Sidebar Basics

Two or more sidebars can be initialized in a web page with the same main content. Sidebars can be positioned on the left or right side of the main content using the `Position` property.

This is useful for:
- Left sidebar for primary navigation
- Right sidebar for secondary navigation or tools
- Dual-panel layouts with collapsible navigation

## Position Property

The `Position` property controls where the sidebar appears:

```razor
<!-- Left sidebar (default) -->
<SfSidebar Position="SidebarPosition.Left">
    <!-- Left navigation -->
</SfSidebar>

<!-- Right sidebar -->
<SfSidebar Position=SidebarPosition.Right">
    <!-- Right navigation -->
</SfSidebar>
```

## Left and Right Sidebars

Here's how to set up one left and one right sidebar:

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<div id="header" style="height:45px;text-align: center;color:white;background-color:midnightblue;font-size:1.2rem;line-height:45px;">
    Header
</div>

<!-- Left Sidebar -->
<SfSidebar @ref="leftSidebarInstance" Type=SidebarType.Push Width="250px" @bind-IsOpen="LeftToggle">
    <ChildContent>
        <div style="text-align: center;" class="text-content"> Left Sidebar</div>
    </ChildContent>
</SfSidebar>

<!-- Right Sidebar -->
<SfSidebar @ref="rightSidebarInstance" Width="250px" Position=SidebarPosition.Right @bind-IsOpen="RightToggle">
    <ChildContent>
        <div style="text-align: center;" class="text-content"> Right Sidebar</div>
    </ChildContent>
</SfSidebar>

<!-- Main Content -->
<div style="text-align:center" class="text-content e-main-content">
    <div>Main content</div>
    <div>
        <SfButton @onclick="ToggleLeftSidebar" IsToggle="true" CssClass="e-btn e-info">Left Toggle Sidebar</SfButton>
    </div>
    <div style="margin-top:10px">
        <SfButton @onclick="ToggleRightSidebar" IsToggle="true" CssClass="e-btn e-info">Right Toggle Sidebar</SfButton>
    </div>
</div>

@code {
    SfSidebar leftSidebarInstance;
    SfSidebar rightSidebarInstance;
    public bool LeftToggle = false;
    public bool RightToggle = false;

    public void ToggleLeftSidebar()
    {
        LeftToggle = !LeftToggle;
    }

    public void ToggleRightSidebar()
    {
        RightToggle = !RightToggle;
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

## Main Content Container

When using multiple sidebars, mark the main content area with the `e-main-content` class. This ensures both sidebars behave correctly as side content:

```razor
<!-- Correct: Main content with e-main-content class -->
<div class="e-main-content" style="text-align:center;">
    <div>Main content area</div>
</div>

<!-- Both sidebars will push/overlay this content appropriately -->
<SfSidebar>
    <!-- Left sidebar -->
</SfSidebar>

<SfSidebar Position=SidebarPosition.Right>
    <!-- Right sidebar -->
</SfSidebar>
```

**Important:** The HTML element with class name `e-main-content` will be considered as the main content. Both the sidebars will behave as side content to this main content area of a web page.

## Managing Multiple States

Each sidebar has its own `IsOpen` state variable. Control them independently:

```razor
@code {
    // Sidebar references
    SfSidebar leftSidebarInstance;
    SfSidebar rightSidebarInstance;

    // Separate state for each sidebar
    public bool LeftToggle = false;  // Left sidebar state
    public bool RightToggle = false; // Right sidebar state

    // Toggle methods
    public void ToggleLeftSidebar()
    {
        LeftToggle = !LeftToggle;
    }

    public void ToggleRightSidebar()
    {
        RightToggle = !RightToggle;
    }

    // Close methods
    public void CloseLeftSidebar()
    {
        LeftToggle = false;
    }

    public void CloseRightSidebar()
    {
        RightToggle = false;
    }

    // Close all
    public void CloseAllSidebars()
    {
        LeftToggle = false;
        RightToggle = false;
    }
}
```

## Complete Example

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<div id="header" style="height:55px;text-align: center;color:white;background-color:midnightblue;padding-top:5px;">
    <SfButton @onclick="ToggleLeftSidebar" CssClass="e-btn">☰ Left</SfButton>
    <span style="margin-left: 40%;">Dashboard</span>
    <SfButton @onclick="ToggleRightSidebar" CssClass="e-btn" style="float:right;margin-right:10px;">Right ☰</SfButton>
</div>

<!-- Left Sidebar (Primary Navigation) -->
<SfSidebar @ref="leftSidebarInstance" Type="SidebarType.Push" Width="280px" @bind-IsOpen="LeftToggle">
    <ChildContent>
        <div style="padding: 20px;">
            <h5>Navigation</h5>
            <ul style="list-style-type: none; padding: 0;">
                <li><a href="#home">Home</a></li>
                <li><a href="#about">About</a></li>
                <li><a href="#services">Services</a></li>
                <li><a href="#portfolio">Portfolio</a></li>
                <li><a href="#contact">Contact</a></li>
            </ul>
        </div>
    </ChildContent>
</SfSidebar>

<!-- Right Sidebar (Tools/Settings) -->
<SfSidebar @ref="rightSidebarInstance" Type="SidebarType.Over" Width="250px" Position="SidebarPosition.Right" @bind-IsOpen="RightToggle">
    <ChildContent>
        <div style="padding: 20px;">
            <h5>Tools</h5>
            <ul style="list-style-type: none; padding: 0;">
                <li><a href="#settings">Settings</a></li>
                <li><a href="#profile">Profile</a></li>
                <li><a href="#notifications">Notifications</a></li>
                <li><a href="#logout">Logout</a></li>
            </ul>
        </div>
    </ChildContent>
</SfSidebar>

<!-- Main Content -->
<div class="e-main-content" style="min-height: calc(100vh - 60px); padding: 40px;">
    <h2>Welcome to Dashboard</h2>
    <p>Use the left and right sidebars to navigate through the application.</p>
    <p>Left sidebar for primary navigation, right sidebar for tools and settings.</p>
</div>

@code {
    SfSidebar leftSidebarInstance;
    SfSidebar rightSidebarInstance;
    public bool LeftToggle = false;
    public bool RightToggle = false;

    public void ToggleLeftSidebar() => LeftToggle = !LeftToggle;
    public void ToggleRightSidebar() => RightToggle = !RightToggle;
}

<style>
    .e-sidebar {
        background-color: #f5f5f5;
        color: #333;
        overflow-y: auto;
    }

    .e-sidebar ul { list-style: none; margin: 0; }
    .e-sidebar li { padding: 12px 15px; border-bottom: 1px solid #e0e0e0; }
    .e-sidebar a { color: #333; text-decoration: none; display: block; }
    .e-sidebar a:hover { background-color: #e0e0e0; padding-left: 20px; }

    .e-main-content {
        background-color: #fff;
        border-top: 1px solid #ddd;
    }

    h2, h5 { margin-top: 0; color: #1976d2; }
</style>
```

**Key Points:**
- Each sidebar requires its own `@ref` variable
- Each sidebar needs its own `IsOpen` binding variable
- Main content must have `e-main-content` class
- Use independent toggle methods for each sidebar
- Left sidebar is default; right uses `Position="SidebarPosition.Right"`
- Different `Type` values can be used (Push, Over, Slide, Auto)
