# Floating Action Button (FAB) - Features

## Installation

```bash
dotnet add package Syncfusion.Blazor.Buttons
```

## API Reference

### SfFab Properties

**Note:** SfFab inherits from SfButton, so it includes all SfButton properties plus FAB-specific ones.

**FAB-Specific Properties:**
- `Position` (FabPosition enum) - FAB position: TopLeft, TopCenter, TopRight, MiddleLeft, MiddleCenter, MiddleRight, BottomLeft, BottomCenter, BottomRight
- `Target` (string) - Target element selector for positioning
- `Visible` (bool) - Visibility state
- `IsPrimary` (bool) - Primary button styling (overrides SfButton)

**Inherited from SfButton:**
- `Content` (string) - Button text content
- `IconCss` (string) - Icon CSS class
- `IconPosition` (IconPosition enum) - Icon position (Left, Right, Top, Bottom)
- `CssClass` (string) - Custom CSS classes
- `Disabled` (bool) - Disabled state
- `EnableRtl` (bool) - RTL support
- `IsToggle` (bool) - Toggle button behavior
- `HtmlAttributes` (Dictionary<string, object>) - Custom HTML attributes
- `ChildContent` (RenderFragment) - Child content

**Events (Inherited from SfButton):**
- `OnClick` (EventCallback<MouseEventArgs>) - Click event handler
- `Created` (EventCallback<Object>) - Component created event

## Basic FAB

```razor
<SfFab IconCss="e-icons e-edit"></SfFab>
```

## With Content

```razor
<SfFab Content="Add" IconCss="e-icons e-plus"></SfFab>
```

## Positioning

```razor
<!-- Bottom Right -->
<SfFab IconCss="e-icons e-edit" Position="FabPosition.BottomRight"></SfFab>

<!-- Bottom Left -->
<SfFab IconCss="e-icons e-edit" Position="FabPosition.BottomLeft"></SfFab>

<!-- Top Right -->
<SfFab IconCss="e-icons e-edit" Position="FabPosition.TopRight"></SfFab>

<!-- Top Left -->
<SfFab IconCss="e-icons e-edit" Position="FabPosition.TopLeft"></SfFab>

<!-- Middle -->
<SfFab IconCss="e-icons e-edit" Position="FabPosition.MiddleCenter"></SfFab>
```

## Click Event

```razor
<SfFab IconCss="e-icons e-plus" OnClick="OnFabClick"></SfFab>

@code {
    private void OnFabClick(MouseEventArgs args)
    {
        Console.WriteLine("FAB clicked");
    }
}
```

## Visibility Control

```razor
<SfFab IconCss="e-icons e-edit" Visible="@isVisible"></SfFab>

<button @onclick="ToggleFab">Toggle FAB</button>

@code {
    private bool isVisible = true;

    private void ToggleFab()
    {
        isVisible = !isVisible;
    }
}
```

## Target Binding

```razor
<div id="target-area" style="position: relative; height: 400px;">
    <SfFab IconCss="e-icons e-edit" Target="#target-area"></SfFab>
</div>
```

## Disabled State

```razor
<SfFab IconCss="e-icons e-edit" Disabled="true"></SfFab>
```

---

See also: [fab-styling.md](fab-styling.md) | [fab-advanced-features.md](fab-advanced-features.md)
