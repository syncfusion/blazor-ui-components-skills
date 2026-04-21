# Animation and Orientation

## Table of Contents
- [Menu Animations](#menu-animations)
- [MenuAnimationSettings Configuration](#menuanimationsettings-configuration)
- [Animation Effects](#animation-effects)
- [Menu Orientation](#menu-orientation)
- [RTL Support](#rtl-support)
- [Responsive Behavior](#responsive-behavior)

## Menu Animations

Animations enhance user experience by providing visual feedback when opening and closing submenus. The Blazor Menu Bar supports multiple animation effects with configurable duration.

### Basic Animation Setup

```razor
<SfMenu TValue="MenuItem">
    <MenuAnimationSettings Effect="MenuEffect.SlideDown" Duration="800"></MenuAnimationSettings>
    <MenuItems>
        <MenuItem Text="File">
            <MenuItems>
                <MenuItem Text="Open"></MenuItem>
                <MenuItem Text="Save"></MenuItem>
            </MenuItems>
        </MenuItem>
    </MenuItems>
</SfMenu>
```

### Animation Without Effects

Disable animation for faster performance:

```razor
<MenuAnimationSettings Effect="MenuEffect.None"></MenuAnimationSettings>
```

## MenuAnimationSettings Configuration

The `MenuAnimationSettings` component controls animation behavior:

```razor
<MenuAnimationSettings 
    Effect="MenuEffect.SlideDown"
    Duration="600"
    Easing="ease-in-out">
</MenuAnimationSettings>
```

### Properties

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `Effect` | MenuEffect enum | Animation effect type | SlideDown |
| `Duration` | double | Animation duration in milliseconds | 400 |

### Setting Animations

```razor
<SfMenu TValue="MenuItem">
    <MenuAnimationSettings Effect="MenuEffect.ZoomIn" Duration="500"></MenuAnimationSettings>
    <MenuItems>
        <!-- Menu items -->
    </MenuItems>
</SfMenu>
```

## Animation Effects

### 1. SlideDown (Default)

Submenu slides down when opening:

```razor
<MenuAnimationSettings Effect="MenuEffect.SlideDown" Duration="400"></MenuAnimationSettings>
```

**Best for**: Traditional menu applications, desktop interfaces

### 2. ZoomIn

Submenu zooms in from center:

```razor
<MenuAnimationSettings Effect="MenuEffect.ZoomIn" Duration="300"></MenuAnimationSettings>
```

**Best for**: Modern UIs, web applications

### 3. FadeIn

Submenu fades in smoothly:

```razor
<MenuAnimationSettings Effect="MenuEffect.FadeIn" Duration="600"></MenuAnimationSettings>
```

**Best for**: Subtle, minimalist designs

### 4. None

No animation, instant appearance:

```razor
<MenuAnimationSettings Effect="MenuEffect.None"></MenuAnimationSettings>
```

**Best for**: Performance-critical applications, accessibility requirements

### Choosing Animation Duration

- **Fast animations (200-300ms)**: Creates snappy, responsive feel
- **Medium animations (400-600ms)**: Balanced visual feedback
- **Slow animations (800-1000ms)**: Emphasizes visual transitions

```razor
<!-- Fast -->
<MenuAnimationSettings Effect="MenuEffect.SlideDown" Duration="250"></MenuAnimationSettings>

<!-- Medium -->
<MenuAnimationSettings Effect="MenuEffect.ZoomIn" Duration="500"></MenuAnimationSettings>

<!-- Slow -->
<MenuAnimationSettings Effect="MenuEffect.FadeIn" Duration="800"></MenuAnimationSettings>
```

## Menu Orientation

### Horizontal Orientation (Default)

Menu items display left to right:

```razor
<SfMenu TValue="MenuItem" Orientation="Syncfusion.Blazor.Navigations.Orientation.Horizontal">
    <MenuItems>
        <MenuItem Text="File"></MenuItem>
        <MenuItem Text="Edit"></MenuItem>
        <MenuItem Text="View"></MenuItem>
        <MenuItem Text="Help"></MenuItem>
    </MenuItems>
</SfMenu>
```

**Layout**: [File] [Edit] [View] [Help]

**Use Case**: Traditional menu bar at top of applications

### Vertical Orientation

Menu items stack vertically:

```razor
<SfMenu TValue="MenuItem" Orientation="Syncfusion.Blazor.Navigations.Orientation.Vertical">
    <MenuItems>
        <MenuItem Text="Dashboard"></MenuItem>
        <MenuItem Text="Products"></MenuItem>
        <MenuItem Text="Reports"></MenuItem>
        <MenuItem Text="Settings"></MenuItem>
        <MenuItem Text="Logout"></MenuItem>
    </MenuItems>
</SfMenu>
```

**Layout**:
```
Dashboard
Products
Reports
Settings
Logout
```

**Use Case**: Sidebar navigation menus

### Vertical Menu with Submenus

```razor
<SfMenu TValue="MenuItem" Orientation="Syncfusion.Blazor.Navigations.Orientation.Vertical">
    <MenuItems>
        <MenuItem Text="Admin">
            <MenuItems>
                <MenuItem Text="Users"></MenuItem>
                <MenuItem Text="Roles"></MenuItem>
                <MenuItem Text="Permissions"></MenuItem>
            </MenuItems>
        </MenuItem>
        <MenuItem Text="Settings">
            <MenuItems>
                <MenuItem Text="General"></MenuItem>
                <MenuItem Text="Security"></MenuItem>
            </MenuItems>
        </MenuItem>
    </MenuItems>
</SfMenu>
```

### Responsive Orientation

Switch orientation based on screen size:

```razor
@using Syncfusion.Blazor.Navigations

<SfMenu TValue="MenuItem" Orientation="@currentOrientation">
    <MenuItems>
        <!-- Menu items -->
    </MenuItems>
</SfMenu>

@code {
    private Orientation currentOrientation = Orientation.Horizontal;
    
    protected override void OnInitialized()
    {
        // Check screen size and set orientation
        AdjustOrientation();
    }
    
    private void AdjustOrientation()
    {
        // In a real app, use JavaScript interop to detect screen size
        // For now, defaulting to horizontal
        currentOrientation = Orientation.Horizontal;
    }
}
```

## RTL Support

Right-to-Left (RTL) support for languages like Arabic, Hebrew, and Urdu:

### Enabling RTL

```razor
<SfMenu TValue="MenuItem" EnableRtl="true">
    <MenuItems>
        <MenuItem Text="ملف">
            <MenuItems>
                <MenuItem Text="فتح"></MenuItem>
                <MenuItem Text="حفظ"></MenuItem>
            </MenuItems>
        </MenuItem>
        <MenuItem Text="تحرير"></MenuItem>
    </MenuItems>
</SfMenu>
```

### RTL with Other Properties

```razor
<SfMenu TValue="MenuItem" 
    EnableRtl="true" 
    Orientation="Syncfusion.Blazor.Navigations.Orientation.Vertical">
    <MenuItems>
        <!-- RTL vertical menu items -->
    </MenuItems>
</SfMenu>
```

### Detecting RTL from Application State

```razor
@page "/menu-rtl"
@using Syncfusion.Blazor.Navigations

<SfMenu TValue="MenuItem" EnableRtl="@isRtl">
    <MenuItems>
        <MenuItem Text="@(isRtl ? "ملف" : "File")"></MenuItem>
        <MenuItem Text="@(isRtl ? "تحرير" : "Edit")"></MenuItem>
    </MenuItems>
</SfMenu>

<button @onclick="ToggleRtl">Toggle RTL</button>

@code {
    private bool isRtl = false;
    
    private void ToggleRtl()
    {
        isRtl = !isRtl;
    }
}
```

## Responsive Behavior

### Mobile Considerations

Menu Bar should adapt to different screen sizes:

```razor
<style>
    @media (max-width: 768px) {
        .e-menu {
            width: 100%;
        }
        
        .e-menu .e-menu-item {
            padding: 10px 15px;
            font-size: 14px;
        }
    }
    
    @media (max-width: 480px) {
        .e-menu .e-menu-item {
            padding: 8px 10px;
            font-size: 12px;
        }
        
        .e-menu .e-icons {
            display: none;  /* Hide icons on very small screens */
        }
    }
</style>
```

### Hamburger Menu Pattern

Convert horizontal menu to vertical with hamburger toggle on mobile:

```razor
@page "/responsive-menu"
@using Syncfusion.Blazor.Navigations

<style>
    .menu-toggle {
        display: none;
        background: none;
        border: none;
        font-size: 24px;
        cursor: pointer;
    }
    
    @media (max-width: 768px) {
        .menu-toggle {
            display: block;
        }
        
        .e-menu {
            display: @(showMenu ? "block" : "none");
            width: 100%;
            position: absolute;
            top: 50px;
            left: 0;
            background: white;
            border: 1px solid #ddd;
        }
    }
</style>

<button class="menu-toggle" @onclick="@(() => showMenu = !showMenu)">☰</button>

<SfMenu TValue="MenuItem">
    <MenuItems>
        <MenuItem Text="Home"></MenuItem>
        <MenuItem Text="Products"></MenuItem>
        <MenuItem Text="Services"></MenuItem>
        <MenuItem Text="Contact"></MenuItem>
    </MenuItems>
</SfMenu>

@code {
    private bool showMenu = true;
}
```

### Mobile-First Approach

```razor
<SfMenu TValue="MenuItem" 
    Orientation="@GetOrientation()"
    @ref="MenuRef">
    <MenuAnimationSettings Effect="MenuEffect.SlideDown" Duration="300"></MenuAnimationSettings>
    <MenuItems>
        <!-- Menu items -->
    </MenuItems>
</SfMenu>

@code {
    private SfMenu<MenuItem> MenuRef;
    private int screenWidth = 0;
    
    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            // Get screen width via JavaScript interop
            screenWidth = 768;  // Default
        }
    }
    
    private Orientation GetOrientation()
    {
        return screenWidth < 768 
            ? Orientation.Vertical 
            : Orientation.Horizontal;
    }
}
```

## Best Practices

1. **Animation Performance**: Disable animations on low-end devices or performance-critical applications
2. **Duration Consistency**: Keep animation durations consistent across your application
3. **Orientation Choice**: Use vertical for sidebars, horizontal for top navigation
4. **Mobile Testing**: Test menu behavior on various screen sizes
5. **RTL Completeness**: Ensure all content is properly translated for RTL languages
6. **Accessibility**: Verify animations don't cause issues for users with motion sensitivity
7. **Performance**: Profile animation impact on slower devices
