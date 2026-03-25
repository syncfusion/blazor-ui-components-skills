# Ribbon Structure and Layout

## Table of Contents
- [Overview](#overview)
- [Ribbon Hierarchy](#ribbon-hierarchy)
- [Tabs Configuration](#tabs-configuration)
- [Groups and Organization](#groups-and-organization)
- [Collections and Layout](#collections-and-layout)
- [Item Definition](#item-definition)
- [Best Practices](#best-practices)

## Overview

The Ribbon component uses a hierarchical structure to organize UI elements:

```
SfRibbon (Container)
  └─ RibbonTabs (Tab collection)
      └─ RibbonTab (Individual tab: Home, Insert, etc.)
          └─ RibbonGroups (Group collection within tab)
              └─ RibbonGroup (Logical group of items)
                  └─ RibbonCollections (Collection container)
                      └─ RibbonCollection (Row or column layout)
                          └─ RibbonItems (Item collection)
                              └─ RibbonItem (Button, Dropdown, etc.)
```

## Ribbon Hierarchy

### Main Ribbon Component

```razor
<SfRibbon>
    <!-- Configuration goes here -->
</SfRibbon>
```

**Properties:**
- `Created` - Event fired when ribbon initializes
- `TabSelecting` - Event before tab selection
- `TabSelected` - Event after tab selection
- `RibbonExpanding` - Event before expansion
- `RibbonCollapsing` - Event before collapse
- `LauncherIconClick` - Event for launcher icon click
- `OverflowPopupOpening` - Event before overflow popup opens
- `OverflowPopupClosing` - Event before overflow popup closes

## Tabs Configuration

### Creating Ribbon Tabs

Tabs are the primary navigation level in a ribbon. Each tab represents a category of related commands.

```razor
<SfRibbon>
    <RibbonTabs>
        <RibbonTab HeaderText="Home">
            <!-- Home tab content -->
        </RibbonTab>
        <RibbonTab HeaderText="Insert">
            <!-- Insert tab content -->
        </RibbonTab>
        <RibbonTab HeaderText="Design">
            <!-- Design tab content -->
        </RibbonTab>
    </RibbonTabs>
</SfRibbon>
```

### Tab Properties

```razor
<RibbonTab HeaderText="Home" Visible="true">
    <!-- Content -->
</RibbonTab>
```

**Available Properties:**
- `HeaderText` (required) - Display name of the tab
- `Visible` - Show/hide the tab (default: `true`)
- `ID` - Unique identifier for the tab

### Dynamic Tab Visibility

Hide tabs conditionally:

```razor
@page "/ribbon"
@using Syncfusion.Blazor.Ribbon

<button @onclick="ToggleAdvanced">Toggle Advanced Tab</button>

<SfRibbon>
    <RibbonTabs>
        <RibbonTab HeaderText="Home"><!-- content --></RibbonTab>
        <RibbonTab HeaderText="Advanced" Visible="@showAdvanced">
            <!-- Advanced features -->
        </RibbonTab>
    </RibbonTabs>
</SfRibbon>

@code {
    private bool showAdvanced = false;

    private void ToggleAdvanced()
    {
        showAdvanced = !showAdvanced;
    }
}
```

## Groups and Organization

### Creating Ribbon Groups

Groups organize related items within a tab. Each group has a header and contains collections of items.

```razor
<RibbonTab HeaderText="Home">
    <RibbonGroups>
        <RibbonGroup HeaderText="Clipboard" Orientation="Orientation.Row">
            <!-- Items here -->
        </RibbonGroup>
        <RibbonGroup HeaderText="Font" Orientation="Orientation.Column">
            <!-- Items here -->
        </RibbonGroup>
    </RibbonGroups>
</RibbonTab>
```

### Group Properties

```razor
<RibbonGroup HeaderText="Clipboard" 
              Orientation="Orientation.Row"
              PopupHeaderText="Clipboard Options"
              EnableGroupOverflow="false"
              LauncherIcon="e-icons e-launcher">
    <!-- Content -->
</RibbonGroup>
```

**Key Properties:**
- `HeaderText` (required) - Group display name
- `Orientation` - `Row` (horizontal, default) or `Column` (vertical)
- `PopupHeaderText` - Text shown in overflow popup
- `EnableGroupOverflow` - Allow group items to overflow
- `LauncherIcon` - Show launcher icon for dialog/details

### Orientation Differences

**Row Orientation** (Horizontal):
```razor
<RibbonGroup HeaderText="Clipboard" Orientation="Orientation.Row">
    <!-- Items displayed horizontally -->
</RibbonGroup>
```

Items are displayed side-by-side (left to right).

**Column Orientation** (Vertical):
```razor
<RibbonGroup HeaderText="Font" Orientation="Orientation.Column">
    <!-- Items displayed vertically -->
</RibbonGroup>
```

Items are displayed top-to-bottom (vertical stack).

### Group Overflow Example

When groups are too wide for screen:

```razor
<RibbonGroup HeaderText="Formatting" 
              Orientation="Orientation.Row"
              EnableGroupOverflow="true"
              PopupHeaderText="Formatting Options">
    <!-- Items that overflow go to popup -->
</RibbonGroup>
```

## Collections and Layout

### Creating Collections

Collections organize items with a specific layout pattern. Multiple collections within a group provide visual separation.

```razor
<RibbonGroup HeaderText="Clipboard">
    <RibbonCollections>
        <RibbonCollection>
            <!-- Primary clipboard action -->
        </RibbonCollection>
        <RibbonCollection>
            <!-- Secondary clipboard actions -->
        </RibbonCollection>
    </RibbonCollections>
</RibbonGroup>
```

### Multiple Collections Pattern

Organize related but distinct items:

```razor
<RibbonGroup HeaderText="Clipboard" Orientation="Orientation.Row">
    <RibbonCollections>
        <!-- Collection 1: Paste (large) -->
        <RibbonCollection>
            <RibbonItems>
                <RibbonItem Type="RibbonItemType.SplitButton" AllowedSizes="RibbonItemSize.Large">
                    <RibbonSplitButtonSettings Content="Paste" IconCss="e-icons e-paste" Items="@pasteOptions">
                    </RibbonSplitButtonSettings>
                </RibbonItem>
            </RibbonItems>
        </RibbonCollection>

        <!-- Vertical separator (visual break) -->

        <!-- Collection 2: Cut, Copy (small) -->
        <RibbonCollection>
            <RibbonItems>
                <RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Small">
                    <RibbonButtonSettings Content="Cut" IconCss="e-icons e-cut"></RibbonButtonSettings>
                </RibbonItem>
                <RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Small">
                    <RibbonButtonSettings Content="Copy" IconCss="e-icons e-copy"></RibbonButtonSettings>
                </RibbonItem>
            </RibbonItems>
        </RibbonCollection>
    </RibbonCollections>
</RibbonGroup>
```

### Collection Visualization

Collection 1 (large):     | Separator | Collection 2 (small)
Paste ▼                   |    |      | Cut
                          |    |      | Copy

## Item Definition

### Basic Item Structure

```razor
<RibbonItem Type="RibbonItemType.Button">
    <RibbonButtonSettings Content="Save" IconCss="e-icons e-save">
    </RibbonButtonSettings>
</RibbonItem>
```

### Item Properties

```razor
<RibbonItem Type="RibbonItemType.Button"
            Disabled="false"
            AllowedSizes="RibbonItemSize.Large"
            DisplayOptions="DisplayMode.Auto"
            ID="saveButton">
    <RibbonButtonSettings Content="Save" IconCss="e-icons e-save">
    </RibbonButtonSettings>
</RibbonItem>
```

**Common Properties:**
- `Type` (required) - Item type (Button, Checkbox, DropDown, etc.)
- `Disabled` - Disable the item
- `AllowedSizes` - `Small`, `Medium`, `Large`
- `DisplayOptions` - Visibility control
- `ID` - Unique identifier

## Sizing Options

Control item size with `AllowedSizes`:

```razor
<!-- Large button -->
<RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Large">
    <RibbonButtonSettings Content="Paste"></RibbonButtonSettings>
</RibbonItem>

<!-- Small button -->
<RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Small">
    <RibbonButtonSettings Content="Cut"></RibbonButtonSettings>
</RibbonItem>

<!-- Medium button (default) -->
<RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Medium">
    <RibbonButtonSettings Content="Copy"></RibbonButtonSettings>
</RibbonItem>
```

## Best Practices

### 1. Logical Grouping
Group related commands together:
```razor
<!-- Good: Related items in same group -->
<RibbonGroup HeaderText="Format Text">
    <!-- Bold, Italic, Underline, Color -->
</RibbonGroup>

<!-- Avoid: Unrelated items mixed -->
```

### 2. Tab Organization
Organize tabs by primary workflow:
```
Home Tab       → Most common actions
Insert Tab     → Adding new content
Design Tab     → Styling and appearance
View Tab       → Display options
```

### 3. Collection Hierarchy
Separate primary from secondary actions:
```razor
<RibbonCollections>
    <!-- Collection 1: Primary action (larger) -->
    <RibbonCollection>
        <RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Large">
            <!-- Primary command -->
        </RibbonItem>
    </RibbonCollection>

    <!-- Collection 2: Secondary actions (smaller) -->
    <RibbonCollection>
        <RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Small">
            <!-- Secondary command -->
        </RibbonItem>
    </RibbonCollection>
</RibbonCollections>
```

### 4. Orientation Selection
- **Row**: When you have 2-3 small or 1-2 large items
- **Column**: When you have many small items or need vertical alignment

### 5. Item Type Selection
- **Button**: Simple click action
- **Checkbox**: Toggle feature on/off
- **DropDown**: Choose from list
- **SplitButton**: Primary action + options
- **ComboBox**: Select or type value
- **ColorPicker**: Choose color
- **GroupButton**: Related single/multiple selection

## Complete Example

Full ribbon structure with multiple tabs, groups, and items:

```razor
@using Syncfusion.Blazor.Ribbon
@using Syncfusion.Blazor.SplitButtons

<SfRibbon>
    <RibbonTabs>
        <!-- Home Tab -->
        <RibbonTab HeaderText="Home">
            <RibbonGroups>
                <!-- Clipboard Group -->
                <RibbonGroup HeaderText="Clipboard" Orientation="Orientation.Row">
                    <RibbonCollections>
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.SplitButton" AllowedSizes="RibbonItemSize.Large">
                                    <RibbonSplitButtonSettings Content="Paste" IconCss="e-icons e-paste" Items="@pasteOptions">
                                    </RibbonSplitButtonSettings>
                                </RibbonItem>
                            </RibbonItems>
                        </RibbonCollection>
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Small">
                                    <RibbonButtonSettings Content="Cut" IconCss="e-icons e-cut"></RibbonButtonSettings>
                                </RibbonItem>
                                <RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Small">
                                    <RibbonButtonSettings Content="Copy" IconCss="e-icons e-copy"></RibbonButtonSettings>
                                </RibbonItem>
                            </RibbonItems>
                        </RibbonCollection>
                    </RibbonCollections>
                </RibbonGroup>

                <!-- Font Group -->
                <RibbonGroup HeaderText="Font" Orientation="Orientation.Column">
                    <RibbonCollections>
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.Button">
                                    <RibbonButtonSettings Content="Bold" IconCss="e-icons e-bold"></RibbonButtonSettings>
                                </RibbonItem>
                                <RibbonItem Type="RibbonItemType.Button">
                                    <RibbonButtonSettings Content="Italic" IconCss="e-icons e-italic"></RibbonButtonSettings>
                                </RibbonItem>
                            </RibbonItems>
                        </RibbonCollection>
                    </RibbonCollections>
                </RibbonGroup>
            </RibbonGroups>
        </RibbonTab>

        <!-- Insert Tab -->
        <RibbonTab HeaderText="Insert">
            <RibbonGroups>
                <RibbonGroup HeaderText="Illustrations">
                    <RibbonCollections>
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.Button">
                                    <RibbonButtonSettings Content="Picture" IconCss="e-icons e-image"></RibbonButtonSettings>
                                </RibbonItem>
                                <RibbonItem Type="RibbonItemType.Button">
                                    <RibbonButtonSettings Content="Chart" IconCss="e-icons e-chart"></RibbonButtonSettings>
                                </RibbonItem>
                            </RibbonItems>
                        </RibbonCollection>
                    </RibbonCollections>
                </RibbonGroup>
            </RibbonGroups>
        </RibbonTab>
    </RibbonTabs>
</SfRibbon>

@code {
    private List<DropDownMenuItem> pasteOptions = new List<DropDownMenuItem>()
    {
        new DropDownMenuItem{ Text = "Keep Source Format" },
        new DropDownMenuItem{ Text = "Merge Format" },
        new DropDownMenuItem{ Text = "Keep Text Only" }
    };
}
```

This creates a professional ribbon with proper organization, sizing, and layout patterns.
