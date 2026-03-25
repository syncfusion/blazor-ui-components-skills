---
name: syncfusion-blazor-ribbon
description: Implement the Syncfusion Blazor Ribbon component. Use this skill when creating ribbon interfaces, implementing ribbon tabs/groups/items, adding file menus, handling ribbon events, or customizing ribbon styling in Syncfusion Blazor projects. Supports all ribbon item types (Button, Checkbox, DropDown, SplitButton, ComboBox, ColorPicker, GroupButton, Template), file menu operations, gallery items, contextual tabs, backstage view, keyboard navigation with KeyTips, events, theming, and accessibility features.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Syncfusion Blazor Ribbon Component

Build professional Office-style ribbon interfaces with the Syncfusion Blazor Ribbon component. This skill provides comprehensive guidance on implementing, configuring, and customizing the Ribbon for Blazor applications.

## Component Overview

The **Ribbon** is a Microsoft Office-style UI component that organizes commands and tools into tabs, groups, and collections. It provides an intuitive interface for application functionality with support for multiple item types, file menus, and extensive customization options.

### Key Concepts

- **Tabs**: Top-level navigation containers (Home, Insert, Design, etc.)
- **Groups**: Logical groupings of related items within a tab (Clipboard, Font, Alignment)
- **Collections**: Organize items within a group (vertical or horizontal)
- **Items**: Individual controls (buttons, dropdowns, checkboxes, color pickers, etc.)
- **File Menu**: Special dropdown menu for file operations (New, Open, Save)
- **Gallery**: Display visual collections of related items (styles, themes, designs)
- **Contextual Tabs**: Dynamic tabs that appear based on selected objects or context
- **Backstage View**: Full-screen menu for file operations and application settings
- **KeyTips**: Keyboard shortcuts for accessibility and productivity

## Ribbon Structure

```
SfRibbon (Main Container)
├── RibbonFileMenuSettings (Optional File Menu)
└── RibbonTabs
    └── RibbonTab (Home, Insert, Design...)
        └── RibbonGroups
            └── RibbonGroup (Clipboard, Font...)
                └── RibbonCollections
                    └── RibbonCollection (Row/Column Layout)
                        └── RibbonItems
                            └── RibbonItem (Button, Dropdown, etc.)
```

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and NuGet package setup
- Namespace imports and service registration
- Theme configuration
- Basic ribbon initialization with first tab and group
- Minimal working example

### Ribbon Structure and Layout
📄 **Read:** [references/ribbon-structure.md](references/ribbon-structure.md)
- Tab organization and configuration
- Group creation with orientation control
- Collection hierarchy and row/column layouts
- Item definition basics
- Organizational best practices

### Ribbon Items: Basic Types
📄 **Read:** [references/ribbon-items-basic.md](references/ribbon-items-basic.md)
- Button items with click handlers
- Checkbox items with toggle state
- DropDown items with menu options
- SplitButton items combining button and menu
- Disabled items and item sizing

### Ribbon Items: Advanced Types
📄 **Read:** [references/ribbon-items-advanced.md](references/ribbon-items-advanced.md)
- ComboBox items with data binding and filtering
- ColorPicker items for color selection
- GroupButton items with multiple/single selection modes
- Template items for custom HTML content
- DisplayOptions for layout-specific visibility

### File Menu Configuration
📄 **Read:** [references/file-menu.md](references/file-menu.md)
- Enabling and configuring the file menu
- Adding menu items and nested submenus
- Submenu behavior (hover vs click)
- Custom header text
- File menu events

### Events and Interactions
📄 **Read:** [references/events.md](references/events.md)
- Component-level events (Created, TabSelecting, TabSelected)
- Ribbon state events (Expanding, Collapsing)
- Item click and selection events
- Launcher icon and overflow popup events
- Event handler patterns and examples

### Gallery Items
📄 **Read:** [references/gallery.md](references/gallery.md)
- Gallery view for displaying collections of related items
- Gallery groups with custom headers
- Item dimensions and styling
- Item count configuration
- Selected item index management
- Popup dimensions customization
- Template customization for gallery items

### Contextual Tabs
📄 **Read:** [references/contextual-tabs.md](references/contextual-tabs.md)
- Dynamic tab visibility control
- Contextual tab configuration
- Tab selection state management
- Show and hide tab methods
- Context-aware UI elements

### Backstage View
📄 **Read:** [references/backstage.md](references/backstage.md)
- Backstage menu configuration
- Menu item and footer items
- Backstage separators
- Back button customization
- Backstage template customization
- Width and height configuration
- Backstage events

### Keyboard Navigation and KeyTips
📄 **Read:** [references/keytips.md](references/keytips.md)
- KeyTips for ribbon items
- File menu keytips
- Backstage menu keytips
- Layout switcher keytips
- Launcher icon keytips
- ShowKeyTips and HideKeyTips methods
- KeyTip best practices and guidelines

### Customization and Styling
📄 **Read:** [references/customization.md](references/customization.md)
- Theme selection and application
- Item sizing and layout options
- CSS customization and class application
- RTL (Right-to-Left) support
- Overflow handling and group collapsing

### Accessibility
📄 **Read:** [references/accessibility.md](references/accessibility.md)
- WCAG 2.1 compliance and standards
- Keyboard navigation (Tab, Enter, Space, Arrow keys)
- ARIA attributes and semantic markup
- Screen reader support
- Focus management and visual indicators

## Quick Start Example

```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Ribbon
@using Syncfusion.Blazor.SplitButtons

<div style="width: 100%">
    <SfRibbon>
        <RibbonTabs>
            <RibbonTab HeaderText="Home">
                <RibbonGroups>
                    <RibbonGroup HeaderText="Clipboard" Orientation="Orientation.Row">
                        <RibbonCollections>
                            <RibbonCollection>
                                <RibbonItems>
                                    <RibbonItem Type="RibbonItemType.SplitButton">
                                        <RibbonSplitButtonSettings Content="Paste" IconCss="e-icons e-paste" Items="@pasteOptions">
                                        </RibbonSplitButtonSettings>
                                    </RibbonItem>
                                </RibbonItems>
                            </RibbonCollection>
                            <RibbonCollection>
                                <RibbonItems>
                                    <RibbonItem Type="RibbonItemType.Button">
                                        <RibbonButtonSettings Content="Cut" IconCss="e-icons e-cut" OnClick="@OnCut">
                                        </RibbonButtonSettings>
                                    </RibbonItem>
                                    <RibbonItem Type="RibbonItemType.Button">
                                        <RibbonButtonSettings Content="Copy" IconCss="e-icons e-copy" OnClick="@OnCopy">
                                        </RibbonButtonSettings>
                                    </RibbonItem>
                                </RibbonItems>
                            </RibbonCollection>
                        </RibbonCollections>
                    </RibbonGroup>
                </RibbonGroups>
            </RibbonTab>
        </RibbonTabs>
    </SfRibbon>
</div>

@code {
    private List<DropDownMenuItem> pasteOptions = new List<DropDownMenuItem>()
    {
        new DropDownMenuItem{ Text = "Keep Source Format" },
        new DropDownMenuItem{ Text = "Merge Format" },
        new DropDownMenuItem{ Text = "Keep Text Only" }
    };

    private void OnCut(MouseEventArgs args) { /* Handle cut action */ }
    private void OnCopy(MouseEventArgs args) { /* Handle copy action */ }
}
```

## Common Patterns

### 1. Multi-Tab Ribbon
Organize related functionality into separate tabs with different commands:
```razor
<SfRibbon>
    <RibbonTabs>
        <RibbonTab HeaderText="Home"><!-- Home commands --></RibbonTab>
        <RibbonTab HeaderText="Insert"><!-- Insert commands --></RibbonTab>
        <RibbonTab HeaderText="Design"><!-- Design commands --></RibbonTab>
    </RibbonTabs>
</SfRibbon>
```

### 2. Grouped Commands
Group related items for better organization and UI layout:
```razor
<RibbonTab HeaderText="Home">
    <RibbonGroups>
        <RibbonGroup HeaderText="Clipboard"><!-- Copy/Paste items --></RibbonGroup>
        <RibbonGroup HeaderText="Font"><!-- Font items --></RibbonGroup>
        <RibbonGroup HeaderText="Paragraph"><!-- Alignment items --></RibbonGroup>
    </RibbonGroups>
</RibbonTab>
```

### 3. File Menu Integration
Include file operations menu:
```razor
<SfRibbon>
    <RibbonFileMenuSettings Visible="true" MenuItems="@fileMenuItems">
    </RibbonFileMenuSettings>
    <RibbonTabs><!-- tabs here --></RibbonTabs>
</SfRibbon>
```

### 4. Data-Bound ComboBox
Bind dropdown to data source:
```razor
<RibbonItem Type="RibbonItemType.ComboBox">
    <RibbonComboBoxSettings DataSource="@fontFamilies" 
                             FieldSettings="@fieldSettings"
                             AllowFiltering="true">
    </RibbonComboBoxSettings>
</RibbonItem>
```

### 5. Toggle Buttons
Create buttons with selected/unselected state:
```razor
<RibbonItem Type="RibbonItemType.GroupButton">
    <RibbonGroupButtonSettings Selection="GroupButtonSelection.Single" Items="@alignmentOptions">
    </RibbonGroupButtonSettings>
</RibbonItem>
```

## Key Properties and Configuration

### Component-Level
- **Tabs**: Collection of `RibbonTab` objects
- **FileMenuSettings**: Configure file menu visibility and items
- **Created**: Event fired when ribbon initializes

### Tab-Level
- **HeaderText**: Tab name displayed to users
- **Visible**: Show/hide tabs dynamically

### Group-Level
- **HeaderText**: Group label
- **Orientation**: `Row` or `Column` layout
- **PopupHeaderText**: Text in overflow popup (if EnableGroupOverflow=true)

### Item-Level
- **Type**: `Button`, `Checkbox`, `DropDown`, `SplitButton`, `ComboBox`, `ColorPicker`, `GroupButton`, `Template`
- **Disabled**: Disable individual items
- **DisplayOptions**: `Auto`, `Classic`, `Simplified`, `Overflow`
- **AllowedSizes**: `Small`, `Medium`, `Large`

## Common Use Cases

### 1. Document Editor
Build a word processor ribbon with file operations, formatting, and style controls.

### 2. Spreadsheet Application
Create a spreadsheet ribbon with formulas, data analysis, and cell formatting.

### 3. Design Tool
Implement a design application ribbon with drawing tools, colors, and object manipulation.

### 4. Content Management System
Design a CMS ribbon with publishing options, content organization, and workflow controls.

### 5. Admin Dashboard
Create admin tools ribbon with user management, settings, and system controls.

### 6. Email Client
Build email ribbon with message actions (Reply, Forward, Delete, Archive).

### 7. Media Editor
Implement image/video editor ribbon with editing tools, effects, and export options.

## Installation Prerequisites

- .NET 6.0 or higher
- Blazor Server or WebAssembly project
- Syncfusion.Blazor.Ribbon NuGet package
- Syncfusion.Blazor.Themes NuGet package

## Theming Support

Available themes:
- Bootstrap 5 (default)
- Material Design
- Fluent UI (Microsoft)
- Tailwind CSS
- Fabric Office
- Material Dark

Themes are applied via CSS imports in `index.html` or `App.razor`.

## Performance Considerations

- Use **DisplayOptions** to hide items not needed in specific layouts
- Implement **lazy loading** for complex item configurations
- Consider **overflow handling** for small screen sizes
- Use **templates judiciously** to avoid excessive rendering

## Browser Support

- Chrome (latest)
- Firefox (latest)
- Safari (latest)
- Edge (latest)
- Mobile browsers (responsive)

## Keyboard Navigation

- **Tab**: Navigate between tabs
- **Enter/Space**: Select items or open menus
- **Arrow Keys**: Navigate within groups and items
- **Escape**: Close open menus and popups

## Related Components

- **Toolbar** - Simpler alternative for basic toolbars
- **Menu** - Standalone menu component
- **TabsComponent** - Tabbed content without ribbon styling
- **AppBar** - Application header bar

## Troubleshooting Tips

**Ribbon not appearing:**
- Verify NuGet packages are installed
- Check namespace imports in `_Imports.razor`
- Confirm service registration in `Program.cs`

**Items not visible:**
- Check `Visible` property on tabs and items
- Verify `DisplayOptions` configuration
- Check `Disabled` property not set to `true`

**Styling issues:**
- Confirm theme CSS is referenced in `index.html`
- Check for conflicting CSS rules
- Verify `CssClass` property syntax

**Events not firing:**
- Confirm event handler is bound with `@`
- Check parameter types match event args
- Verify component lifecycle (use `Created` event for initialization)

## Next Steps

1. Start with **Getting Started** to install and set up the component
2. Learn **Ribbon Structure** to understand tab/group/item organization
3. Explore **Item Types** (Basic and Advanced) for different control options
4. Add **File Menu** for file operations
5. Implement **Events** for user interactions
6. Customize with **Styling** and **Accessibility** for production apps

For more information, visit the [official Syncfusion Blazor Ribbon documentation](https://blazor.syncfusion.com/documentation/ribbon/).
