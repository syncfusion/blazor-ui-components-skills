---
name: syncfusion-blazor-buttons
description: Implement Syncfusion Blazor button components including Button, ButtonGroup, CheckBox, Chip, DropdownMenu, FloatingActionButton, ProgressButton, SpeedDial, SplitButton, and ToggleSwitch. Use this skill when working with button-related components in Blazor applications. Covers installation, configuration, styling, accessibility, event handling, and advanced features for all button component types.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Blazor UI Components - Buttons"
---

# Syncfusion Blazor Buttons Components

## Table of Contents
- [When to Use This Skill](#when-to-use-this-skill)
- [Components Overview](#components-overview)
- [Common Setup and Installation](#common-setup-and-installation)
- [Documentation and Navigation Guide](#documentation-and-navigation-guide)
- [Common Patterns Across Components](#common-patterns-across-components)
- [Shared Configuration](#shared-configuration)
- [Common Use Cases](#common-use-cases)

---

## Components Overview

The **Syncfusion.Blazor.Buttons** package provides 11 comprehensive button components:

### Core Buttons
1. **Button (SfButton)** - Standard button with multiple types, styles, icons, and states
2. **ButtonGroup (SfButtonGroup)** - Grouped button controls with selection modes
3. **CheckBox (SfCheckBox)** - Tri-state checkbox with checked, unchecked, and indeterminate states
4. **RadioButton (SfRadioButton)** - Radio button for mutually exclusive selections
5. **ToggleSwitch (SfSwitch)** - On/off toggle switch control

### Interactive Buttons
6. **DropdownMenu (SfDropDownButton)** - Button with dropdown menu options
7. **SplitButton (SfSplitButton)** - Button with primary action and dropdown menu
8. **ProgressButton (SfProgressButton)** - Button with built-in progress indication

### Floating Actions
9. **FloatingActionButton (SfFab)** - Circular floating action button
10. **SpeedDial (SfSpeedDial)** - FAB with expandable action items (linear/radial)

### Chip Components
11. **Chip (SfChip)** - Compact elements for tags, filters, selections, and actions

**Package:** `Syncfusion.Blazor.Buttons` and `Syncfusion.Blazor.SplitButtons`

---

## Common Setup and Installation

All button components share the same installation and setup process:

### NuGet Packages

Install the required packages based on component type:

```bash
# For Button, Chip, FAB, ToggleSwitch
dotnet add package Syncfusion.Blazor.Buttons

# For DropdownMenu, SplitButton, ProgressButton, ButtonGroup, SpeedDial
dotnet add package Syncfusion.Blazor.SplitButtons

# Required theme package
dotnet add package Syncfusion.Blazor.Themes
```

### Setup Steps (Common for All)

**1. Add Namespaces** (`~/_Imports.razor`):
```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Buttons
@using Syncfusion.Blazor.SplitButtons
```

**2. Register Service** (`~/Program.cs`):
```csharp
using Syncfusion.Blazor;
builder.Services.AddSyncfusionBlazor();
```

**3. Add Theme and Script** (`~/index.html` or `~/App.razor`):
```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
</head>
```

---

## Documentation and Navigation Guide

### Button Component
📄 **Read:** [references/button-features.md](references/button-features.md)
- Getting started and installation
- Basic button implementation
- Button types (flat, outline, round, primary, toggle)
- Button styles (primary, success, info, warning, danger, link)
- Icons and icon positioning
- Button sizes (small, normal)
- Repeat buttons
- Tooltips
- HTML attribute support
- Native event handling

📄 **Read:** [references/button-styling.md](references/button-styling.md)
- CSS class customization
- Appearance customization
- Block buttons
- Rounded corners
- RTL (right-to-left) support
- Theme integration
- Color schemes and variations
- Responsive design patterns

📄 **Read:** [references/button-advanced-features.md](references/button-advanced-features.md)
- Accessibility (WCAG compliance, keyboard navigation, ARIA attributes)
- Screen reader support
- Disabled state handling
- Toggle button functionality
- Server vs Web App implementation differences
- Performance optimization
- Best practices and common pitfalls

---

### RadioButton Component
📄 **Read:** [references/radio-button-features.md](references/radio-button-features.md)
- Getting started and installation
- Basic radio button implementation
- Radio button grouping with Name property
- Label configuration and positioning
- State and value management
- Two-way data binding (@bind-Checked)
- Value property for identification
- Pre-selecting default values
- Form integration patterns

📄 **Read:** [references/radio-button-styling.md](references/radio-button-styling.md)
- CssClass for custom styling
- HtmlAttributes dictionary usage
- EnableRtl for right-to-left layouts
- Disabled state configuration
- EnablePersistence for state storage
- Custom CSS class patterns
- Theme customization techniques
- Responsive design approaches

📄 **Read:** [references/radio-button-advanced-features.md](references/radio-button-advanced-features.md)
- ValueChange event handler
- ChangeArgs event data structure
- CheckedChanged callback implementation
- Created lifecycle event
- Event handling patterns
- ARIA attributes for accessibility
- Keyboard navigation support
- Best practices and common patterns

---

### CheckBox Component
📄 **Read:** [references/checkbox-features.md](references/checkbox-features.md)
- Getting started and installation
- Basic checkbox implementation
- Checkbox states (checked, unchecked, disabled)
- Label configuration
- Change event handling
- Two-way data binding (@bind-Checked)
- Multiple checkboxes and checkbox groups
- Indeterminate (tristate) state

📄 **Read:** [references/checkbox-styling.md](references/checkbox-styling.md)
- CSS class customization
- Appearance customization
- Theme integration (Bootstrap5, Material, Fabric, Tailwind)
- Color variations (primary, success, warning, danger)
- Size customization
- RTL (right-to-left) support
- Disabled appearance
- Label styling and spacing

📄 **Read:** [references/checkbox-advanced-features.md](references/checkbox-advanced-features.md)
- Disabled state handling and conditional disabling
- Accessibility (WCAG, ARIA attributes, screen readers)
- Keyboard navigation (Tab, Space)
- Form integration and validation
- State management patterns
- Best practices and common patterns

---

### Button Group Component
📄 **Read:** [references/button-group-features.md](references/button-group-features.md)
- Getting started and installation
- Basic button group implementation
- Horizontal and vertical orientation
- Selection modes (single, multiple, none)
- Nesting button groups
- Native event handling
- Item configuration and data binding

📄 **Read:** [references/button-group-styling.md](references/button-group-styling.md)
- Appearance customization
- Button types and styles within groups
- Icons in button groups
- Size variations
- Custom CSS classes
- Theme integration

📄 **Read:** [references/button-group-advanced-features.md](references/button-group-advanced-features.md)
- Accessibility features
- Keyboard navigation
- Selection state management
- Event handling patterns
- Best practices for button groups

---

### Chip Component
📄 **Read:** [references/chip-features.md](references/chip-features.md)
- Getting started and installation
- Basic chip implementation
- Chip types (input, filter, choice, action)
- Icon integration
- Delete functionality
- Selection handling
- Events (click, delete, created, beforeItemRender)
- Dynamic chip collections
- Rendering from data sources

📄 **Read:** [references/chip-styling.md](references/chip-styling.md)
- Appearance customization
- Color variations
- Size options
- Custom CSS classes
- Icon and avatar styling
- Theme integration
- Responsive design

📄 **Read:** [references/chip-advanced-features.md](references/chip-advanced-features.md)
- Accessibility support
- Keyboard navigation
- Custom template support
- State management patterns
- Event handling
- Best practices

---

### Dropdown Menu Component
📄 **Read:** [references/dropdown-menu-features.md](references/dropdown-menu-features.md)
- Getting started and installation
- Basic dropdown menu implementation
- Popup items configuration (text, ID, icon, separator)
- Icons in menu items
- Item click events
- Opening and closing behavior
- Position customization
- Dynamically adding/removing items
- Grouped popups with ListView integration
- Dialog opening on item click
- DropdownList vs DropdownButton comparison

📄 **Read:** [references/dropdown-menu-styling.md](references/dropdown-menu-styling.md)
- Appearance customization
- Caret icon customization
- Hiding dropdown arrow
- Icon and width customization
- Popup width customization
- Rounded corners
- RTL support
- Animation effects
- Template customization
- Theme integration

📄 **Read:** [references/dropdown-menu-advanced-features.md](references/dropdown-menu-advanced-features.md)
- Accessibility features
- Disabled state handling
- Native event handling
- Keyboard navigation
- Focus management
- Best practices and patterns

---

### Floating Action Button (FAB) Component
📄 **Read:** [references/fab-features.md](references/fab-features.md)
- Getting started and installation
- Basic FAB implementation
- Position options (TopLeft, TopCenter, TopRight, MiddleLeft, MiddleCenter, MiddleRight, BottomLeft, BottomCenter, BottomRight)
- Icons configuration
- Content customization
- Target element binding
- Click event handling
- Show/hide behavior
- Visibility control

📄 **Read:** [references/fab-styling.md](references/fab-styling.md)
- Appearance styles (primary, success, info, warning, danger)
- Size variations (small, medium, large)
- Icon positioning
- Color customization
- CSS class customization
- Disabled appearance
- Theme integration

📄 **Read:** [references/fab-advanced-features.md](references/fab-advanced-features.md)
- Accessibility support
- Keyboard navigation
- Event handling patterns
- State management
- Best practices for FAB usage

---

### Progress Button Component
📄 **Read:** [references/progress-button-features.md](references/progress-button-features.md)
- Getting started and installation
- Basic progress button implementation
- Spinner types and positions (left, right, top, bottom, center)
- Progress states (Begin, Progress, End, Fail)
- Duration configuration
- Content customization during progress
- Events (OnBegin, Progress, End, Fail)
- Stopping progress programmatically
- Hiding spinner
- Progress tracking

📄 **Read:** [references/progress-button-styling.md](references/progress-button-styling.md)
- Appearance customization
- Button types and icons
- CSS class customization for progress states
- Progress indicator styling
- Spinner customization
- Content changes during progress
- Theme integration

📄 **Read:** [references/progress-button-advanced-features.md](references/progress-button-advanced-features.md)
- Accessibility features
- Trace events for debugging
- Native event handling
- State management patterns
- Error handling
- Best practices for progress buttons

---

### Speed Dial Component
📄 **Read:** [references/speeddial-features.md](references/speeddial-features.md)
- Getting started and installation
- Basic SpeedDial implementation
- Display modes (Linear, Radial)
- Action items configuration
- Position options (9 positions)
- Open and close behavior
- Item click events
- Open modes (Hover, Click)
- Direction options (Up, Down, Left, Right, Auto)
- Radial settings (offset, direction, startAngle, endAngle)
- Icon configuration (open/close icons)

📄 **Read:** [references/speeddial-styling.md](references/speeddial-styling.md)
- Appearance customization
- Icons (open icon, close icon, item icons)
- Color variations
- Item templates
- CSS class customization
- Animation effects
- Theme integration

📄 **Read:** [references/speeddial-advanced-features.md](references/speeddial-advanced-features.md)
- Accessibility support
- Keyboard navigation
- Event handling (all events)
- State management patterns
- Modal backdrop
- Best practices

---

### Split Button Component
📄 **Read:** [references/split-button-features.md](references/split-button-features.md)
- Getting started and installation
- Basic split button implementation
- Dropdown menu items configuration
- Primary button action
- Item selection events
- Icons in items
- Separator support
- Opening and closing behavior
- Event handling

📄 **Read:** [references/split-button-styling.md](references/split-button-styling.md)
- Appearance customization
- Button styles
- Icons and icon positioning
- Size variations
- Custom CSS classes
- RTL support
- Theme integration

📄 **Read:** [references/split-button-advanced-features.md](references/split-button-advanced-features.md)
- Accessibility features
- Disabled state handling
- Keyboard navigation
- Native event handling
- Best practices

---

### Toggle Switch Component
📄 **Read:** [references/toggle-switch-features.md](references/toggle-switch-features.md)
- Getting started and installation
- Basic toggle switch implementation
- Checked and unchecked states
- Two-way data binding (@bind-Checked)
- On/Off label configuration
- Change events
- Value binding
- Name attribute for forms
- Default state configuration

📄 **Read:** [references/toggle-switch-styling.md](references/toggle-switch-styling.md)
- Appearance customization
- Size variations
- Label customization (OnLabel, OffLabel)
- Color schemes
- Custom CSS classes
- RTL support
- Material theme considerations (no text support)
- Theme integration

📄 **Read:** [references/toggle-switch-advanced-features.md](references/toggle-switch-advanced-features.md)
- Accessibility support
- Keyboard navigation
- Disabled state handling
- Form integration
- State management patterns
- Best practices

---

## Common Patterns Across Components

### Event Handling Pattern
All button components support standard Blazor event handling:

```razor
<SfButton OnClick="HandleClick">Click Me</SfButton>

@code {
    private void HandleClick()
    {
        // Handle button click
    }
}
```

### Styling Pattern
All components support CSS class customization:

```razor
<SfButton CssClass="e-primary custom-button">Styled Button</SfButton>
```

### Icon Integration Pattern
Most components support icon CSS classes:

```razor
<SfButton IconCss="e-icons e-plus-icon">Add Item</SfButton>
```

### Disabled State Pattern
All interactive components support disabled state:

```razor
<SfButton Disabled="@isDisabled">Button</SfButton>
```

### RTL Support Pattern
All components support right-to-left rendering:

```razor
<SfButton EnableRtl="true">RTL Button</SfButton>
```

---

## Shared Configuration

### Theme Options
All button components support Syncfusion themes:
- **Bootstrap5** (default): `bootstrap5.css`
- **Material**: `material.css`
- **Fabric**: `fabric.css`
- **Tailwind**: `tailwind.css`
- **Fluent**: `fluent.css`

### Common CSS Classes
Standard CSS utility classes work across components:
- **e-primary**: Primary action styling
- **e-success**: Success/positive action
- **e-info**: Informational action
- **e-warning**: Warning/caution action
- **e-danger**: Danger/negative action
- **e-link**: Hyperlink styling
- **e-small**: Small size variant
- **e-flat**: Flat style (no background)
- **e-outline**: Outline style (border only)
- **e-round**: Circular/rounded style

### Accessibility Features
All components include:
- ARIA attributes
- Keyboard navigation support
- Screen reader compatibility
- Focus indicators
- WCAG 2.1 compliance

---

## Common Use Cases

### Standard Actions
Use **Button** for primary, secondary, and tertiary actions in forms, dialogs, and pages.

### Multi-Selection
Use **CheckBox** for multiple independent selections, list items, and preferences.

### Exclusive Selection
Use **RadioButton** for single selection from multiple options, preferences, settings, and survey questions.

### Grouped Actions
Use **ButtonGroup** for related actions like text alignment, view modes, or filter options.

### Tags and Filters
Use **Chip** for tags, contact lists, filter selections, and removable items.

### Menu Actions
Use **DropdownMenu** for contextual menus with multiple options.

### Dual Actions
Use **SplitButton** when you need a primary action with additional options.

### Long Operations
Use **ProgressButton** for operations that take time (file upload, form submission, API calls).

### Primary Page Actions
Use **FloatingActionButton** for the primary action on mobile or dashboard pages.

### Multiple Quick Actions
Use **SpeedDial** when you need multiple related actions accessible from a single FAB.

### On/Off States
Use **ToggleSwitch** for binary settings, feature toggles, and preferences.

---

## Quick Start Examples

### Basic Button
```razor
<SfButton CssClass="e-primary">Primary Action</SfButton>
```

### Basic CheckBox
```razor
<SfCheckBox TChecked="bool" Label="Accept Terms"></SfCheckBox>
```

### CheckBox with Two-Way Binding
```razor
<SfCheckBox @bind-Checked="isEnabled" Label="Enable Feature"></SfCheckBox>

@code {
    private bool isEnabled = false;
}
```

### Multiple Checkboxes
```razor
<SfCheckBox TChecked="bool"  Label="Option 1"></SfCheckBox>
<SfCheckBox TChecked="bool" Label="Option 2"></SfCheckBox>
<SfCheckBox TChecked="bool" Label="Option 3"></SfCheckBox>
```

### Tristate CheckBox
```razor
<SfCheckBox EnableTriState="true" Checked="null" Label="Select All" TChecked="bool?"></SfCheckBox>
```

### Basic RadioButton
```razor
<SfRadioButton Label="Option 1" Name="group" Value="option1" @bind-Checked="@selected"></SfRadioButton>
<SfRadioButton Label="Option 2" Name="group" Value="option2" @bind-Checked="@selected"></SfRadioButton>
<SfRadioButton Label="Option 3" Name="group" Value="option3" @bind-Checked="@selected"></SfRadioButton>

@code {
    private string selected = "option1";
}
```

### RadioButton Group
```razor
<h4>Choose Your Plan</h4>
<SfRadioButton Label="Free" Name="plan" Value="free" @bind-Checked="@plan"></SfRadioButton>
<SfRadioButton Label="Pro" Name="plan" Value="pro" @bind-Checked="@plan"></SfRadioButton>
<SfRadioButton Label="Enterprise" Name="plan" Value="enterprise" @bind-Checked="@plan"></SfRadioButton>

@code {
    private string plan = "free";
}
```

### Button with Icon
```razor
<SfButton IconCss="e-icons e-plus-icon" CssClass="e-primary">
    Add New
</SfButton>
```

### Toggle Switch
```razor
<SfSwitch @bind-Checked="isEnabled" OnLabel="On" OffLabel="Off"></SfSwitch>

@code {
    private bool isEnabled = true;
}
```

### Dropdown Menu
```razor
<SfDropDownButton Content="Actions">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Edit"></DropDownMenuItem>
        <DropDownMenuItem Text="Delete"></DropDownMenuItem>
    </DropDownMenuItems>
</SfDropDownButton>
```

### Floating Action Button
```razor
<div id="target" style="min-height:200px; position:relative;">
    <SfFab Target="#target" IconCss="e-icons e-plus" Content="Add"></SfFab>
</div>
```

### Chip Collection
```razor
<SfChip EnableDelete="true">
    <ChipItems>
        <ChipItem Text="Tag 1"></ChipItem>
        <ChipItem Text="Tag 2"></ChipItem>
        <ChipItem Text="Tag 3"></ChipItem>
    </ChipItems>
</SfChip>
```

---

## Navigation Tips

- For **installation and basic setup**, refer to any component's features file
- For **styling and appearance**, refer to the styling files for each component
- For **accessibility and advanced features**, refer to the advanced-features files
- For **specific component features**, navigate to that component's documentation sections above

---

## Support and Resources

- **Official Demos:** https://blazor.syncfusion.com/demos/buttons/
- **API Documentation:** https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Buttons.html
- **GitHub Examples:** https://github.com/SyncfusionExamples/Blazor-Getting-Started-Examples/
- **System Requirements:** https://blazor.syncfusion.com/documentation/system-requirements
- **Licensing:** Commercial license required for production use

---

**Remember:** Always start with the appropriate component's features file for implementation guidance, then move to styling and advanced features as needed.
