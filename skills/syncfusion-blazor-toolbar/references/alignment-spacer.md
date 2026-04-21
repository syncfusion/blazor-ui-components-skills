# Align Items Using Spacer in Blazor Toolbar Component

## Table of Contents
- [Overview](#overview)
- [What is a Spacer](#what-is-a-spacer)
- [Left, Center, and Right Alignment](#left-center-and-right-alignment)
- [Left and Right Alignment](#left-and-right-alignment)
- [Right Alignment Only](#right-alignment-only)
- [Spacer vs Align Property](#spacer-vs-align-property)
- [Advanced Patterns](#advanced-patterns)
- [Best Practices](#best-practices)

## Overview

The Toolbar `Spacer` type manages item alignment by creating adjustable empty space within the toolbar. Spacers dynamically adapt to the toolbar's width, providing flexible separation between items without fixed positioning.

## What is a Spacer

A Spacer is a special toolbar item type (`ItemType.Spacer`) that expands to fill available horizontal space. It pushes subsequent items away, creating separation and enabling flexible alignment patterns.

**Basic Syntax:**

```razor
<ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
```

**Key Characteristics:**
- Takes up all available horizontal space
- Multiple spacers divide space equally
- Invisible (creates space, not visual element)
- Dynamic (adjusts as toolbar resizes)
- Works in all overflow modes

## Left, Center, and Right Alignment

Insert two spacers to create three distinct alignment zones.

### Pattern

- Left items → Spacer → Center items → Spacer → Right items

### Implementation

```razor
@using Syncfusion.Blazor.Navigations

<SfToolbar>
    <ToolbarItems>
        <!-- Left-aligned items -->
        <ToolbarItem Text="Cut"></ToolbarItem>
        <ToolbarItem Text="Copy"></ToolbarItem>
        <ToolbarItem Text="Paste"></ToolbarItem>
        
        <!-- First spacer pushes everything after to the right -->
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        
        <!-- Center-aligned items -->
        <ToolbarItem Text="Bold"></ToolbarItem>
        <ToolbarItem Text="Underline"></ToolbarItem>
        <ToolbarItem Text="Italic"></ToolbarItem>
        
        <!-- Second spacer pushes everything after to the far right -->
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        
        <!-- Right-aligned items -->
        <ToolbarItem Text="Search"></ToolbarItem>
        <ToolbarItem Text="Settings"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

### Visual Result

```
[Cut] [Copy] [Paste]  ←——— Spacer ———→  [Bold] [Underline] [Italic]  ←——— Spacer ———→  [Search] [Settings]
```

### Use Cases

- Application toolbars with left actions, center title, right settings
- Navigation bars with logo (left), menu (center), user profile (right)
- Editor toolbars with tools (left), document info (center), save buttons (right)

## Left and Right Alignment

Insert one spacer between left and right-aligned items.

### Pattern

- Left items → Spacer → Right items

### Implementation

```razor
@using Syncfusion.Blazor.Navigations

<SfToolbar>
    <ToolbarItems>
        <!-- Left-aligned items -->
        <ToolbarItem Text="Cut"></ToolbarItem>
        <ToolbarItem Text="Copy"></ToolbarItem>
        <ToolbarItem Text="Paste"></ToolbarItem>
        
        <!-- Spacer divides left and right -->
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        
        <!-- Right-aligned items -->
        <ToolbarItem Text="Bold"></ToolbarItem>
        <ToolbarItem Text="Underline"></ToolbarItem>
        <ToolbarItem Text="Italic"></ToolbarItem>
        <ToolbarItem Text="Search"></ToolbarItem>
        <ToolbarItem Text="Settings"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

### Visual Result

```
[Cut] [Copy] [Paste]  ←——————— Spacer ———————→  [Bold] [Underline] [Italic] [Search] [Settings]
```

### Use Cases

- Application headers with branding (left), actions (right)
- Toolbars with file operations (left), view options (right)
- Navigation with back button (left), menu items (right)

## Right Alignment Only

Insert spacer as the first item to push all following items to the right.

### Pattern

- Spacer → Right items

### Implementation

```razor
@using Syncfusion.Blazor.Navigations

<SfToolbar>
    <ToolbarItems>
        <!-- Spacer at start pushes everything right -->
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        
        <!-- Right-aligned items -->
        <ToolbarItem Text="Cut"></ToolbarItem>
        <ToolbarItem Text="Copy"></ToolbarItem>
        <ToolbarItem Text="Paste"></ToolbarItem>
        <ToolbarItem Text="Bold"></ToolbarItem>
        <ToolbarItem Text="Underline"></ToolbarItem>
        <ToolbarItem Text="Italic"></ToolbarItem>
        <ToolbarItem Text="Search"></ToolbarItem>
        <ToolbarItem Text="Settings"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

### Visual Result

```
←——————————— Spacer ———————————→  [Cut] [Copy] [Paste] [Bold] [Underline] [Italic] [Search] [Settings]
```

### Use Cases

- User profile toolbars aligned to the right
- Settings or preferences menus
- Right-to-left (RTL) interface patterns
- Secondary action toolbars

## Spacer vs Align Property

Both Spacer and the `Align` property can position items, but they work differently.

### Align Property

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Home" Align="ItemAlign.Left"></ToolbarItem>
        <ToolbarItem Text="My Page" Align="ItemAlign.Center"></ToolbarItem>
        <ToolbarItem Text="Search" Align="ItemAlign.Right"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Characteristics:**
- Fixed positioning (Left, Center, Right)
- Each item positioned independently
- Less flexible for dynamic layouts
- Good for simple alignment needs

### Spacer Type

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Home"></ToolbarItem>
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        <ToolbarItem Text="My Page"></ToolbarItem>
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        <ToolbarItem Text="Search"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Characteristics:**
- Fluid, responsive positioning
- Groups items naturally
- Multiple items per alignment zone
- Better for complex layouts

### Comparison Table

| Feature | Align Property | Spacer Type |
|---------|---------------|-------------|
| **Flexibility** | Fixed zones | Dynamic spacing |
| **Multiple Items per Zone** | No (each item separate) | Yes (natural grouping) |
| **Responsive** | Limited | Excellent |
| **Ease of Use** | Simple | Requires planning |
| **Best For** | Simple layouts | Complex layouts |
| **Combine Multiple Items** | Difficult | Easy |

### Recommendation

**Use Align Property when:**
- Simple alignment (one item per zone)
- Fixed positioning is sufficient
- Quick implementation needed

**Use Spacer when:**
- Multiple items per alignment zone
- Responsive, fluid layout required
- Complex toolbar with many items
- Fine control over spacing

**Important:** Avoid mixing Align property and Spacer in the same toolbar. Choose one approach for consistency.

## Advanced Patterns

### Multiple Spacers for Equal Distribution

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Item 1"></ToolbarItem>
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        <ToolbarItem Text="Item 2"></ToolbarItem>
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        <ToolbarItem Text="Item 3"></ToolbarItem>
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        <ToolbarItem Text="Item 4"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

Result: Items evenly distributed across toolbar width.

### Group Separator with Spacer

```razor
<SfToolbar>
    <ToolbarItems>
        <!-- File operations -->
        <ToolbarItem Text="New" PrefixIcon="e-icons e-plus"></ToolbarItem>
        <ToolbarItem Text="Open" PrefixIcon="e-icons e-folder-open"></ToolbarItem>
        <ToolbarItem Text="Save" PrefixIcon="e-icons e-save"></ToolbarItem>
        
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        
        <!-- Edit operations -->
        <ToolbarItem Text="Cut" PrefixIcon="e-icons e-cut"></ToolbarItem>
        <ToolbarItem Text="Copy" PrefixIcon="e-icons e-copy"></ToolbarItem>
        <ToolbarItem Text="Paste" PrefixIcon="e-icons e-paste"></ToolbarItem>
        
        <!-- Push remaining items to right -->
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        
        <!-- View controls -->
        <ToolbarItem Text="Zoom In" PrefixIcon="e-icons e-zoom-in"></ToolbarItem>
        <ToolbarItem Text="Zoom Out" PrefixIcon="e-icons e-zoom-out"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

Combines separators for visual grouping with spacer for alignment.

### Application Header Pattern

```razor
<SfToolbar Width="100%">
    <ToolbarItems>
        <!-- Logo/Brand -->
        <ToolbarItem>
            <Template>
                <div style="font-weight: bold; font-size: 18px; padding: 0 16px;">
                    MyApp
                </div>
            </Template>
        </ToolbarItem>
        
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        
        <!-- Navigation -->
        <ToolbarItem Text="Home"></ToolbarItem>
        <ToolbarItem Text="Products"></ToolbarItem>
        <ToolbarItem Text="Services"></ToolbarItem>
        <ToolbarItem Text="About"></ToolbarItem>
        
        <!-- Push user controls to right -->
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        
        <!-- User section -->
        <ToolbarItem Text="Notifications" PrefixIcon="e-icons e-bell"></ToolbarItem>
        <ToolbarItem Text="Profile" PrefixIcon="e-icons e-user"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

### Search Bar with Spacer

```razor
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Menu" PrefixIcon="e-icons e-menu"></ToolbarItem>
        
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        
        <!-- Center search -->
        <ToolbarItem Type="ItemType.Input">
            <Template>
                <input type="search" 
                       placeholder="Search..." 
                       style="width: 300px; padding: 6px 12px; border: 1px solid #ccc; border-radius: 4px;" />
            </Template>
        </ToolbarItem>
        
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        
        <ToolbarItem Text="Settings" PrefixIcon="e-icons e-settings"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

Centers the search bar with spacers on both sides.

## Best Practices

### Do:

✅ Use Spacer for flexible, responsive layouts  
✅ Group related items before/after spacers  
✅ Combine with separators for visual organization  
✅ Test toolbar at different widths  
✅ Use multiple spacers for equal distribution  

### Don't:

❌ Mix Align property and Spacer in same toolbar  
❌ Use spacer as the only item (no effect)  
❌ Add spacers between every single item (loses purpose)  
❌ Forget to test responsive behavior  
❌ Use too many spacers (dilutes spacing effect)  

### Responsive Considerations

When using spacers with responsive modes:

```razor
<!-- Good: Spacer with Popup mode -->
<SfToolbar Width="100%" OverflowMode="OverflowMode.Popup">
    <ToolbarItems>
        <ToolbarItem Text="File"></ToolbarItem>
        <ToolbarItem Text="Edit"></ToolbarItem>
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        <ToolbarItem Text="Help" Overflow="OverflowOption.Show"></ToolbarItem>
        <ToolbarItem Text="Settings" Overflow="OverflowOption.Show"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

The spacer maintains alignment while overflow items move to popup appropriately.

### Common Mistakes

**Mistake 1: Spacer at End**

```razor
<!-- ❌ Wrong: Spacer at end has no effect -->
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Item 1"></ToolbarItem>
        <ToolbarItem Text="Item 2"></ToolbarItem>
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Correction:**

```razor
<!-- ✅ Correct: Spacer between items -->
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Item 1"></ToolbarItem>
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        <ToolbarItem Text="Item 2"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Mistake 2: Too Many Spacers**

```razor
<!-- ❌ Wrong: Spacer between each item -->
<SfToolbar>
    <ToolbarItems>
        <ToolbarItem Text="Item 1"></ToolbarItem>
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        <ToolbarItem Text="Item 2"></ToolbarItem>
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        <ToolbarItem Text="Item 3"></ToolbarItem>
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        <ToolbarItem Text="Item 4"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Correction:** Use spacers strategically for alignment zones, not between every item.
