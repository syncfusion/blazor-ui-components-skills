---
name: syncfusion-blazor-popups
description: Implement modal and modeless dialogs using Syncfusion Blazor Dialog component. Use this skill when creating dialogs, popup windows, modal confirmations, custom forms, or interactive overlay windows. Covers templates, events, positioning, animations, accessibility, and advanced customization.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Popups"
---

# Implementing Syncfusion Blazor Dialog

The [Syncfusion Blazor Dialog](https://www.syncfusion.com/blazor-components/blazor-modal-dialog) component provides a flexible, feature-rich solution for creating modal and modeless dialogs in Blazor applications. Dialogs are essential UI elements for displaying alerts, confirmations, forms, and interactive content overlaid on the main application.

## When to Use This Skill

Use this skill when you need to:
- Display confirmation dialogs or alerts
- Create form-based dialogs for user input
- Show modeless floating windows
- Implement multi-step wizard dialogs
- Build custom popup windows with headers and footers
- Handle dialog events (open, close, drag, resize)
- Create nested or stacked dialogs
- Apply custom styling and animations to dialogs
- Ensure accessibility compliance in dialog components
- Persist dialog state across user sessions

## Component Overview

The Dialog component supports:
- **Template-based layouts** (header, content, footer with custom HTML/components)
- **Multiple interaction modes** (modal and modeless)
- **Rich event system** (lifecycle, drag, resize, overlay interactions)
- **Advanced positioning** (fixed, absolute, relative, centered)
- **Interactive features** (draggable, resizable, minimize/maximize buttons, fullscreen mode)
- **Accessibility support** (WCAG compliance, keyboard navigation, ARIA attributes)
- **Animations** (open/close transitions)
- **State management** (visible binding, state persistence)

## Documentation and Navigation Guide

### Getting Started
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- Installation and NuGet package setup
- Blazor WebAssembly and Server project configuration
- Adding namespaces and Syncfusion services
- Basic dialog implementation
- CSS imports and theme configuration
- Displaying header, content, and setting visibility

### Templates and Content Customization
📄 **Read:** [references/templates.md](references/templates.md)
- Header template with custom HTML and icons
- Content template with forms and Blazor components
- Footer template and custom buttons
- DialogTemplates structure
- Embedding complex UI elements in dialogs

### Dialog Buttons
📄 **Read:** [references/dialog-buttons.md](references/dialog-buttons.md)
- DialogButton component configuration
- Button placement and click handlers
- Using DialogButtons vs FooterTemplate
- Standard button patterns and common use cases

### Events and Interactions
📄 **Read:** [references/events.md](references/events.md)
- Lifecycle events (Created, Destroyed)
- Opening and closing events (OnOpen, Opened, OnClose, Closed)
- Drag events (OnDragStart, OnDrag, OnDragStop)
- Resize events (OnResizeStart, Resizing, OnResizeStop)
- Modal overlay interactions (OnOverlayModalClick)

### Positioning and Visibility
📄 **Read:** [references/positioning-visibility.md](references/positioning-visibility.md)
- Position property (fixed, absolute, relative)
- Visible binding for show/hide control
- Target element configuration
- Dialog centering on page
- Width and height configuration
- Z-index management

### Dialog Behavior and Features
📄 **Read:** [references/dialog-behavior.md](references/dialog-behavior.md)
- Modal vs modeless dialogs
- AllowDragging and EnableResize properties
- AllowPrerender for performance optimization
- ShowCloseIcon configuration
- Creating nested dialogs
- Animation support
- IsModal and overlay behavior

### Methods and Programmatic Control
📄 **Read:** [references/methods.md](references/methods.md)
- ShowAsync() to open dialogs programmatically
- ShowAsync(true) to open dialogs in fullscreen mode
- HideAsync() to close dialogs programmatically
- GetDimension() to retrieve dialog size
- GetButton(index) and GetButtonItems() for button access
- RefreshPositionAsync() for position recalculation
- Complete control examples

### Advanced Customization and Styling
📄 **Read:** [references/advanced-customization.md](references/advanced-customization.md)
- CSS class customization and styling
- Appearance customization
- Accessibility features (WCAG compliance, keyboard navigation)
- Animation configurations
- State persistence strategies with EnablePersistence
- Minimize/Maximize button implementation
- Localization support
- Responsive dialog design

## Quick Start

### Basic Dialog

```csharp
@using Syncfusion.Blazor.Popups

<SfDialog Width="300px" Header="Welcome">
    <DialogTemplates>
        <Content>This is a basic dialog with content.</Content>
    </DialogTemplates>
</SfDialog>
```

### Dialog with Show/Hide Control

```csharp
@using Syncfusion.Blazor.Popups
@using Syncfusion.Blazor.Buttons

<div id="target">
    <SfButton OnClick="@OpenDialog">Open Dialog</SfButton>
    
    <SfDialog Target="#target" Width="400px" Header="Confirmation" 
              ShowCloseIcon="true" @bind-Visible="IsVisible">
        <DialogTemplates>
            <Content>Are you sure you want to proceed?</Content>
        </DialogTemplates>
        <DialogButtons>
            <DialogButton Content="OK" IconCss="e-icons e-ok-icon" IsPrimary="true" OnClick="@OnOkClick" />
            <DialogButton Content="Cancel" IconCss="e-icons e-close-icon" OnClick="@OnCancelClick" />
        </DialogButtons>
    </SfDialog>
</div>

@code {
    private bool IsVisible { get; set; } = false;
    
    private void OpenDialog() => IsVisible = true;
    
    private void OnOkClick()
    {
        // Handle OK action
        IsVisible = false;
    }
    
    private void OnCancelClick()
    {
        // Handle Cancel action
        IsVisible = false;
    }
}
```

## Common Patterns

### Alert Dialog
```csharp
<SfDialog Width="350px" IsModal="true" Header="Alert">
    <DialogTemplates>
        <Content>This action cannot be undone.</Content>
    </DialogTemplates>
</SfDialog>
```

### Form Dialog
```csharp
<SfDialog Width="400px" Header="User Information">
    <DialogTemplates>
        <Content>
            <div class="form-group">
                <input type="text" placeholder="Enter name" />
            </div>
        </Content>
    </DialogTemplates>
</SfDialog>
```

### Draggable and Resizable Dialog
```csharp
<SfDialog Width="400px" Header="Features" AllowDragging="true" EnableResize="true">
    <DialogTemplates>
        <Content>You can drag and resize this dialog.</Content>
    </DialogTemplates>
</SfDialog>
```

### Fullscreen Dialog
```csharp
@using Syncfusion.Blazor.Popups
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="@OpenFullScreenDialog">Open Fullscreen Dialog</SfButton>

<SfDialog @ref="DialogRef" Width="250px" ShowCloseIcon="true" Visible="false">
    <DialogTemplates>
        <Header>Dialog</Header>
        <Content>This is a fullscreen dialog</Content>
    </DialogTemplates>
    <DialogButtons>
        <DialogButton Content="OK" IsPrimary="true" OnClick="@CloseDialog" />
        <DialogButton Content="Cancel" OnClick="@CloseDialog" />
    </DialogButtons>
</SfDialog>

@code {
    SfDialog DialogRef;

    private async Task OpenFullScreenDialog()
    {
        await this.DialogRef.ShowAsync(true);
    }

    private async Task CloseDialog()
    {
        await this.DialogRef.HideAsync();
    }
}
```

## Key Properties

| Property | Type | Purpose |
|----------|------|---------|
| `Width` | string | Sets dialog width (e.g., "400px", "50%"). Default: "100%" |
| `Height` | string | Sets dialog height (e.g., "300px", "70%"). Default: "auto" |
| `Header` | string | Sets dialog header text |
| `Content` | string | Sets dialog content text |
| `Visible` | bool | Controls dialog visibility. Supports @bind-Visible. Default: true |
| `IsModal` | bool | Makes dialog modal (overlay blocking interaction). Default: false |
| `ShowCloseIcon` | bool | Shows close button in header. Default: false |
| `AllowDragging` | bool | Enables dialog dragging by header. Default: false |
| `EnableResize` | bool | Enables dialog resizing. Default: false |
| `CloseOnEscape` | bool | Closes dialog when Escape key is pressed. Default: true |
| `AnimationSettings` | DialogAnimationSettings | Configures open/close animations (effect, duration, delay). Default: Fade effect, 400ms |
| `Position` | string | Positioning mode: "fixed" (stays on screen), "absolute" (relative to target), or "relative" (document flow). Default: "fixed" |
| `Left` | string | X-coordinate position (e.g., "100px", "20%"). Works with Position property. Default: "auto" |
| `Top` | string | Y-coordinate position (e.g., "50px", "10%"). Works with Position property. Default: "auto" |
| `ID` | string | Unique identifier for the dialog. Required when EnablePersistence="true" |
| `MinHeight` | string | Sets minimum height constraint for resize (e.g., "150px"). Default: null |
| `MaxHeight` | string | Sets maximum height constraint for resize (e.g., "600px"). Default: null |
| `ResizeHandles` | ResizeDirection[] | Array of resize directions (e.g., SouthEast, NorthEast). Default: [SouthEast] |
| `Buttons` | List\<DialogButtonModel\> | Programmatic button configuration (alternative to DialogButtons component) |
| `CssClass` | string | Custom CSS class(es) for styling |
| `EnableRtl` | bool | Enables right-to-left layout support. Default: false |
| `EnablePersistence` | bool | Persists position, width, height to browser local storage. Default: false. Requires ID property |
| `Target` | string | CSS selector for dialog target/container element (e.g., "#container", ".target-div") |
| `FooterTemplate` | RenderFragment | Custom footer content (alternative to DialogButtons component) |
| `AllowPrerender` | bool | Keeps DOM elements when hidden for faster re-display. Default: false |
| `ZIndex` | double | Sets stacking order relative to other elements. Default: 1000 |

## Key Enums and Types

### DialogEffect Enum
Animation effects for dialog open/close transitions:

| Value | Description |
|-------|-------------|
| `None` | No animation |
| `Fade` | Fade in/out effect (default) |
| `Zoom` | Zoom in/out from center |
| `SlideLeft` | Slide from/to left |
| `SlideRight` | Slide from/to right |
| `SlideTop` | Slide from/to top |
| `SlideBottom` | Slide from/to bottom |
| `FlipX` | Horizontal flip effect |
| `FlipY` | Vertical flip effect |

### ResizeDirection Enum
Directions from which the dialog can be resized:

| Value | Description |
|-------|-------------|
| `South` | Bottom edge |
| `North` | Top edge |
| `East` | Right edge |
| `West` | Left edge |
| `SouthEast` | Bottom-right corner (default) |
| `SouthWest` | Bottom-left corner |
| `NorthEast` | Top-right corner |
| `NorthWest` | Top-left corner |
| `All` | All directions |

### ButtonType Enum
HTML button type attribute for dialog buttons:

| Value | Description |
|-------|-------------|
| `Button` | Standard button (default) |
| `Submit` | Form submission button |
| `Reset` | Form reset button |

### DialogAnimationSettings Class
Configuration for dialog animations:

| Property | Type | Description | Default |
|----------|------|-------------|---------|
| `Effect` | DialogEffect | Animation effect | DialogEffect.Fade |
| `Duration` | int | Animation duration (milliseconds) | 400 |
| `Delay` | int | Animation delay (milliseconds) | 0 |

- Use `ShowAsync(true)` for fullscreen dialogs when you need maximum focus and screen real estate
- Set `Visible="false"` initially when using programmatic control with ShowAsync()
**Example:**
```csharp
var animSettings = new DialogAnimationSettings 
{ 
    Effect = DialogEffect.Zoom, 
    Duration = 300, 
    Delay = 0 
};
```

### DialogDimension Class
Returned by `GetDimension()` method:

| Property | Type | Description |
|----------|------|-------------|
| `Width` | string | Current dialog width |
| `Height` | string | Current dialog height |

## Important Constraints and Notes

⚠️ **Critical Requirements:**
- **ID Property**: Must be set when using `EnablePersistence="true"`. The ID serves as the localStorage key.
- **Mutual Exclusivity**: `FooterTemplate` and `DialogButtons` cannot be used together. Choose one approach.
- **Target Selector**: The `Target` property must reference a valid CSS selector for existing DOM elements.
- **Resize Dependency**: `ResizeHandles` only works when `EnableResize="true"`.
- **Position Context**: The `Position` property affects how `Left` and `Top` coordinates are interpreted.

📋 **Best Practices:**
- Use `IsModal="true"` for critical actions requiring user attention
- Set `AllowPrerender="true"` for frequently toggled dialogs to improve performance
- Always provide a close mechanism (button, close icon, or Escape key)
- Use `ZIndex` values in increments (1000, 1001, 1002) for nested dialogs
- Test responsive behavior with percentage-based Width/Height values

## Related Skills

- [implementing-buttons](../buttons/implementing-buttons/) - For dialog button styling and behavior
- [implementing-dropdown-list](../../dropdowns/implementing-dropdown-list/) - For select controls within dialogs
- [implementing-text-inputs](../../inputs/implementing-text-inputs/) - For form fields in dialogs

---

## See Also

- [Syncfusion Blazor Dialog Documentation](https://www.syncfusion.com/blazor-components/blazor-modal-dialog)
- [Dialog Events Reference](https://help.syncfusion.com/cr/blazor/Syncfusion.Blazor.Popups.DialogEvents.html)
- [Blazor Themes](https://blazor.syncfusion.com/documentation/appearance/themes)
