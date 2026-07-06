---
name: syncfusion-blazor-sidebar
description: Guide to implementing Syncfusion Blazor Sidebar component for responsive navigation sidebars. Use this when building Blazor WebAssembly and .NET 8 Web Apps that need sidebars. Covers setup, open/close control, docking, state persistence, multiple sidebars, and complete styling. Includes ListView and TreeView integration.
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
  category: "Navigation Components"
---

# Implementing Syncfusion Blazor Sidebar Component

The Syncfusion Blazor Sidebar is a responsive navigation component that reserves screen space for navigation content. It supports multiple positioning modes, docking, state persistence, and integrates seamlessly with other Syncfusion components.

## When to Use This Skill

Use this skill when you need to:
- Add a collapsible navigation sidebar to your Blazor application
- Control sidebar open/close state programmatically
- Enable responsive behavior based on screen resolution
- Show icons-only navigation (dock mode)
- Persist sidebar state across page navigation
- Display multiple sidebars with different positioning
- Target specific HTML elements for sidebar context
- Integrate ListView or TreeView components in sidebar
- Apply custom CSS styling to sidebar appearance
- Build responsive layouts in Blazor WebAssembly or .NET 8 apps

## Key Component Properties

| Property | Purpose |
|----------|---------|
| `IsOpen` | Gets or sets a boolean value which indicates whether the Sidebar component's state is open or close. When the Sidebar type is set to `Auto`, the component will be expanded in desktop and collapsed in mobile mode regardless of the `IsOpen` property. |
| `Width` | Sets sidebar width in pixels or percentage |
| `Type` | Sidebar behavior: `Push`, `Over`, `Slide`, `Auto` |
| `Position` | Sidebar position: `Left` (default) or `Right` |
| `EnableDock` | Shows icons-only when collapsed (default: `false`) |
| `DockSize` | Width of dock area in pixels |
| `MediaQuery` | CSS media query for responsive behavior |
| `Target` | Specific HTML element as sidebar context |
| `EnablePersistence` | Retains state in localStorage (default: `false`) |
| `ShowBackdrop` | Overlay behind sidebar to prevent content interaction |
| `Animate` | Enables animation transitions while expanding or collapsing (default: `true`) |
| `EnableRtl` | Displays sidebar in right-to-left direction for RTL languages (default: `false`) |

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation across Visual Studio, VS Code, and .NET CLI
- NuGet package setup (Syncfusion.Blazor.Navigations, Syncfusion.Blazor.Themes)
- Namespace imports and service registration
- Theme stylesheet and script references
- Basic component rendering

### Web App Setup (.NET 8)
📄 **Read:** [references/web-app-setup.md](references/web-app-setup.md)
- Blazor Web App project creation
- Interactive render modes (Auto, Server, WebAssembly)
- Package installation for client projects
- Stylesheet and script references in App.razor
- Render mode directive usage

### Open/Close Control
📄 **Read:** [references/open-close-control.md](references/open-close-control.md)
- `IsOpen` property and two-way binding with `@bind-IsOpen`
- Toggle methods for programmatic control
- Button-based sidebar toggling
- Combining multiple open/close triggers and standard event handling

### Responsive & Docking Features
📄 **Read:** [references/responsive-docking.md](references/responsive-docking.md)
- `MediaQuery` property for responsive behavior
- `EnableDock` property for icon-only navigation mode
- `DockSize` to control icon area width
- Icon styling and font-face setup
- Docking with ListItems and icon management

### State Persistence & Target Context
📄 **Read:** [references/state-persistence-targets.md](references/state-persistence-targets.md)
- `EnablePersistence` for localStorage support
- ID requirement for persistence to work
- `Target` property for sidebar context
- Applying sidebar to specific HTML elements
- Toolbar and AppBar integration

### Styling & Animation
📄 **Read:** [references/styling-customization.md](references/styling-customization.md)
- Animation control using `Animate` property
- RTL (right-to-left) support using `EnableRtl` property
- CSS transitions and custom animation effects
- State-based styling and visual customization
- Backdrop customization and dock styling

### Multiple Sidebars
📄 **Read:** [references/multiple-sidebars.md](references/multiple-sidebars.md)
- Managing multiple sidebar instances
- `Position` property (Left/Right)
- `e-main-content` class for multi-sidebar layouts
- Left and right sidebar toggling
- Coordinating multiple sidebar states

### Content Integration
📄 **Read:** [references/content-integration.md](references/content-integration.md)
- Integrating ListView component in sidebar
- ListViewFieldSettings configuration
- TreeView integration for hierarchical menus
- Toolbar component integration
- MainLayout integration in .NET 8 apps

### API Reference
📄 **Read:** [references/api-reference.md](references/api-reference.md)
- Complete API documentation for SfSidebar component
- All 22 properties with descriptions and accepted values
- Methods and their return types
- Complete event system (Changed, Created, Destroyed, IsOpenChanged, OnClose, OnOpen)
- Event arguments (ChangeEventArgs, EventArgs)
- SfSidebarContainer documentation
- 9+ comprehensive usage examples with code
- Property use cases with practical implementations

---

## Quick Start Example

```blazor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfSidebar @ref="sidebarObj" Width="250px" @bind-IsOpen="SidebarToggle">
    <ChildContent>
        <div style="text-align: center; padding: 3rem;">
            <p>Sidebar Content</p>
            <SfButton @onclick="Close" CssClass="e-btn">Close</SfButton>
        </div>
    </ChildContent>
</SfSidebar>

<div style="padding: 3rem;">
    <div>Main Content</div>
    <SfButton @onclick="Toggle" IsToggle="true">Toggle Sidebar</SfButton>
</div>

@code {
    SfSidebar sidebarObj;
    public bool SidebarToggle = false;

    public void Toggle() => SidebarToggle = !SidebarToggle;
    public void Close() => SidebarToggle = false;
}
```

## Common Implementation Patterns

### Pattern 1: Basic Toggle Sidebar
Use `IsOpen` property with button click to show/hide sidebar.

### Pattern 2: Responsive Sidebar
Use `MediaQuery` to automatically hide sidebar on mobile and show on desktop.

### Pattern 3: Docked Navigation
Use `EnableDock` with `DockSize` for icon-only navigation that expands on hover/click.

### Pattern 4: Persistent State
Use `EnablePersistence` + unique `ID` to remember sidebar state across page navigation.

### Pattern 5: Target Property Behavior
- **Implicit (Default):** No `Target` property → sidebar targets next sibling element automatically
- **Explicit (Advanced):** `Target=".selector"` → requires **inner wrapper `<div>`** for CSS transforms
- ✓ Use implicit for most cases | Use explicit only for scoped layout control
- See [State Persistence & Target Context](references/state-persistence-targets.md#target-property-behavior---implicit-vs-explicit) for detailed patterns

### Pattern 6: Sidebar with Menu
Combine with ListView or TreeView for structured navigation menus.

### Pattern 7: Multiple Sidebars
Use `Position` property with left/right sidebars flanking main content.

---

**Next Steps:**
1. Choose your platform: WebAssembly or .NET 8 Web App
2. Follow the Getting Started guide for installation
3. Select features from the navigation guide above
4. Implement your sidebar with appropriate styling

For detailed examples and advanced scenarios, refer to individual reference files.
