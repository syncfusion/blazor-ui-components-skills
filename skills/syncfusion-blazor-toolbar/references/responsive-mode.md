# Responsive Mode in Blazor Toolbar Component

## Table of Contents
- [Overview](#overview)
- [Scrollable Mode](#scrollable-mode)
- [Popup Mode](#popup-mode)
  - [Priority of Items](#priority-of-items)
  - [Text Mode for Buttons](#text-mode-for-buttons)
- [MultiRow Mode](#multirow-mode)
- [Extended Mode](#extended-mode)
- [Choosing the Right Mode](#choosing-the-right-mode)
- [Common Scenarios](#common-scenarios)

## Overview

The Toolbar component provides four display modes (`OverflowMode`) to handle content that exceeds the viewing area:

- **Scrollable** - Horizontal scrolling with navigation arrows (default)
- **Popup** - Overflow items move to a dropdown popup
- **MultiRow** - Overflow items wrap to additional inline rows
- **Extended** - Overflow items displayed in next row below toolbar

Use the `OverflowMode` property to set the desired mode:

```razor
<SfToolbar OverflowMode="OverflowMode.Popup">
    <!-- Items -->
</SfToolbar>
```

## Scrollable Mode

The default `OverflowMode` displays toolbar items in a single line with horizontal scrolling enabled when items overflow.

### Features

- Left and right navigation arrows at toolbar edges
- Click arrows to scroll
- Hold arrow to scroll continuously
- Touch swipe gestures on mobile devices
- Left arrow disabled initially (scroll right first)
- Arrow disables when reaching start/end

### Implementation

```razor
@using Syncfusion.Blazor.Navigations

<SfToolbar Width="600">
    <ToolbarItems>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-cut" Text="Cut"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-copy" Text="Copy"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-paste" Text="Paste"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-bold" Text="Bold"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-underline" Text="Underline"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-italic" Text="Italic"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-paint-bucket" Text="Color"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-list-unordered" Text="Bullets"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-list-ordered" Text="Numbering"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-upload-1" Text="Upload"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-download" Text="Download"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

### Scrollable Mode is Best For:

- Desktop applications with sufficient horizontal space
- Toolbars where all items should remain visible but space-constrained
- Text editor toolbars with many formatting options
- Applications where users expect horizontal scrolling

### Customizing Scroll Distance

Use the `ScrollStep` property to control how far the toolbar scrolls per arrow click:

```razor
<SfToolbar Width="600" ScrollStep="50">
    <ToolbarItems>
        <!-- Items -->
    </ToolbarItems>
</SfToolbar>
```

- Default: 30 pixels
- Larger values: Faster scrolling
- Smaller values: Finer control

## Popup Mode

In `Popup` mode, items that don't fit in the available space move to an overflow popup dropdown.

### Features

- Overflow items accessible via dropdown icon
- Clean, compact toolbar appearance
- Configurable item priority (Show/Hide/None)
- Text display modes (Toolbar/Overflow/Both)
- Popup opens on dropdown icon click

### Implementation

```razor
@using Syncfusion.Blazor.Navigations

<SfToolbar Width="380" OverflowMode="OverflowMode.Popup">
    <ToolbarItems>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-cut" Text="Cut"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-copy" Text="Copy"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-paste" Text="Paste"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-bold" Text="Bold"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-underline" Text="Underline"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-italic" Text="Italic"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-paint-bucket" Text="Color"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-erase" Text="Clear"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-refresh" Text="Reload"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-export" Text="Export"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

### Popup Mode is Best For:

- Mobile applications with limited screen width
- Toolbars with many items that users access infrequently
- Responsive designs that adapt to different screen sizes
- Clean, minimal UI design

**Note:** If popup content exceeds page height, overflowing items in the popup will not be visible.

### Priority of Items

Control which items stay on the toolbar vs move to popup using the `Overflow` property.

**Values:**
- `OverflowOption.Show` - Primary priority (always on toolbar)
- `OverflowOption.Hide` - Secondary priority (always in popup)
- `OverflowOption.None` - No priority (default order)

**Example:**

```razor
<SfToolbar Width="300px" OverflowMode="OverflowMode.Popup">
    <ToolbarItems>
        <!-- Primary: Always visible on toolbar -->
        <ToolbarItem Text="Cut" 
                     PrefixIcon="e-icons e-cut" 
                     Overflow="OverflowOption.Show">
        </ToolbarItem>
        <ToolbarItem Text="Copy" 
                     PrefixIcon="e-icons e-copy" 
                     Overflow="OverflowOption.Show">
        </ToolbarItem>
        
        <!-- Secondary: Always in popup -->
        <ToolbarItem Text="Settings" 
                     Overflow="OverflowOption.Hide">
        </ToolbarItem>
        
        <!-- Normal: Moves to popup when no space -->
        <ToolbarItem Text="Paste"></ToolbarItem>
        <ToolbarItem Text="Bold"></ToolbarItem>
        <ToolbarItem Text="Italic"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Priority Order:**
1. Items with `Overflow="OverflowOption.Show"` appear first on toolbar
2. If primary items also overflow (not enough space), they move to top of popup
3. Items with `Overflow="OverflowOption.None"` fill remaining toolbar space
4. Items with `Overflow="OverflowOption.Hide"` always appear in popup

### ShowAlwaysInPopup Property

Alternative way to keep an item in popup permanently:

```razor
<SfToolbar Width="300px" OverflowMode="OverflowMode.Popup">
    <ToolbarItems>
        <ToolbarItem Text="Cut" ShowAlwaysInPopup="true"></ToolbarItem>
        <ToolbarItem Text="Copy"></ToolbarItem>
        <ToolbarItem Text="Paste"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

**Note:** `ShowAlwaysInPopup` doesn't work with `Overflow="OverflowOption.Show"`.

### Text Mode for Buttons

The `ShowTextOn` property controls where button text appears in Popup mode.

**Values:**
- `DisplayMode.Both` - Text visible on toolbar and popup (default)
- `DisplayMode.Overflow` - Text visible only in popup
- `DisplayMode.Toolbar` - Text visible only on toolbar

**Example: Icon-Only Toolbar, Text in Popup**

```razor
<SfToolbar Width="300" OverflowMode="OverflowMode.Popup">
    <ToolbarItems>
        <ToolbarItem Type="ItemType.Button" 
                     PrefixIcon="e-icons e-cut" 
                     Text="Cut" 
                     ShowTextOn="DisplayMode.Overflow" 
                     Overflow="OverflowOption.Show">
        </ToolbarItem>
        <ToolbarItem Type="ItemType.Button" 
                     PrefixIcon="e-icons e-copy" 
                     Text="Copy" 
                     ShowTextOn="DisplayMode.Overflow" 
                     Overflow="OverflowOption.Show">
        </ToolbarItem>
        <ToolbarItem Type="ItemType.Button" 
                     PrefixIcon="e-icons e-paste" 
                     Text="Paste" 
                     ShowTextOn="DisplayMode.Overflow" 
                     Overflow="OverflowOption.Show">
        </ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" 
                     PrefixIcon="e-icons e-bold" 
                     Text="Bold" 
                     ShowTextOn="DisplayMode.Overflow">
        </ToolbarItem>
        <ToolbarItem Type="ItemType.Button" 
                     PrefixIcon="e-icons e-italic" 
                     Text="Italic" 
                     ShowTextOn="DisplayMode.Overflow">
        </ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

This creates a compact icon-only toolbar while providing text labels in the popup for better understanding.

## MultiRow Mode

In `MultiRow` mode, overflow items wrap to additional inline rows within the toolbar itself.

### Features

- No popup or scrolling needed
- All items visible at once
- Toolbar height increases automatically
- Good for toolbars with logical groupings

### Implementation

```razor
@using Syncfusion.Blazor.Navigations

<SfToolbar Width="380" OverflowMode="OverflowMode.MultiRow">
    <ToolbarItems>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-cut" Text="Cut"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-copy" Text="Copy"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-paste" Text="Paste"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-bold" Text="Bold"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-underline" Text="Underline"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-italic" Text="Italic"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-paint-bucket" Text="Color"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-erase" Text="Clear"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-refresh" Text="Reload"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-export" Text="Export"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

### MultiRow Mode is Best For:

- Toolbars where all items should be immediately visible
- Applications with vertical space available
- Grouping related tools across multiple rows
- Touch interfaces where popups are inconvenient

**Note:** Items wrap naturally as toolbar width changes, creating a responsive layout.

## Extended Mode

In `Extended` mode, overflow items are displayed in the next row below the toolbar.

### Features

- Similar to MultiRow but with different layout
- Overflow items appear below primary row
- Creates distinct primary and secondary action areas
- Toolbar height increases when items overflow

### Implementation

```razor
@using Syncfusion.Blazor.Navigations

<SfToolbar Width="380" OverflowMode="OverflowMode.Extended">
    <ToolbarItems>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-cut" Text="Cut"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-copy" Text="Copy"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-paste" Text="Paste"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-bold" Text="Bold"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-underline" Text="Underline"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-italic" Text="Italic"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-paint-bucket" Text="Color"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-erase" Text="Clear"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-refresh" Text="Reload"></ToolbarItem>
        <ToolbarItem Type="ItemType.Button" PrefixIcon="e-icons e-export" Text="Export"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

### Extended Mode is Best For:

- Toolbars with clear primary/secondary action distinction
- Applications where vertical space is acceptable
- Maintaining visual separation between action groups
- Responsive designs that expand vertically

**Difference from MultiRow:** Extended mode typically creates a more distinct visual separation between primary and extended rows.

## Choosing the Right Mode

| Mode | Best For | Pros | Cons |
|------|----------|------|------|
| **Scrollable** | Desktop apps, sufficient horizontal space | All items accessible, familiar pattern | Requires scrolling, hidden items not immediately visible |
| **Popup** | Mobile apps, limited width | Clean UI, compact, good for many items | Items hidden in popup, extra click needed |
| **MultiRow** | Vertical space available, all items visible | All items visible, no interaction needed | Takes vertical space, can look cluttered |
| **Extended** | Primary/secondary action split | Clear action hierarchy, responsive | Takes vertical space when extended |

## Common Scenarios

### Responsive Toolbar for All Devices

```razor
<SfToolbar Width="100%" OverflowMode="OverflowMode.Popup">
    <ToolbarItems>
        <!-- Primary actions: Always visible -->
        <ToolbarItem Text="New" 
                     PrefixIcon="e-icons e-plus" 
                     Overflow="OverflowOption.Show">
        </ToolbarItem>
        <ToolbarItem Text="Save" 
                     PrefixIcon="e-icons e-save" 
                     Overflow="OverflowOption.Show">
        </ToolbarItem>
        
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        
        <!-- Secondary actions: Overflow when needed -->
        <ToolbarItem Text="Export" ShowTextOn="DisplayMode.Overflow"></ToolbarItem>
        <ToolbarItem Text="Print" ShowTextOn="DisplayMode.Overflow"></ToolbarItem>
        <ToolbarItem Text="Settings" ShowTextOn="DisplayMode.Overflow"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

### Text Editor with Many Tools

```razor
<SfToolbar Width="900" OverflowMode="OverflowMode.Scrollable" ScrollStep="60">
    <ToolbarItems>
        <!-- File operations -->
        <ToolbarItem PrefixIcon="e-icons e-save" TooltipText="Save"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-folder-open" TooltipText="Open"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        
        <!-- Formatting -->
        <ToolbarItem PrefixIcon="e-icons e-bold" TooltipText="Bold"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-italic" TooltipText="Italic"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-underline" TooltipText="Underline"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        
        <!-- Alignment -->
        <ToolbarItem PrefixIcon="e-icons e-align-left" TooltipText="Left"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-align-center" TooltipText="Center"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-align-right" TooltipText="Right"></ToolbarItem>
        <ToolbarItem PrefixIcon="e-icons e-justify" TooltipText="Justify"></ToolbarItem>
        <!-- More items... -->
    </ToolbarItems>
</SfToolbar>
```

### Compact Mobile Toolbar

```razor
<SfToolbar Width="100%" OverflowMode="OverflowMode.Popup">
    <ToolbarItems>
        <ToolbarItem PrefixIcon="e-icons e-menu" 
                     Overflow="OverflowOption.Show" 
                     TooltipText="Menu">
        </ToolbarItem>
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        <ToolbarItem Text="Action" 
                     Overflow="OverflowOption.Show">
        </ToolbarItem>
        
        <!-- Overflow items -->
        <ToolbarItem Text="Settings"></ToolbarItem>
        <ToolbarItem Text="Help"></ToolbarItem>
        <ToolbarItem Text="About"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```

### Form Actions Toolbar

```razor
<SfToolbar Width="100%" OverflowMode="OverflowMode.Extended">
    <ToolbarItems>
        <ToolbarItem Text="Save" PrefixIcon="e-icons e-save" CssClass="primary"></ToolbarItem>
        <ToolbarItem Text="Save & Close" CssClass="primary"></ToolbarItem>
        <ToolbarItem Type="ItemType.Separator"></ToolbarItem>
        <ToolbarItem Text="Cancel"></ToolbarItem>
        <ToolbarItem Type="ItemType.Spacer"></ToolbarItem>
        <ToolbarItem Text="Delete" CssClass="danger"></ToolbarItem>
    </ToolbarItems>
</SfToolbar>
```
