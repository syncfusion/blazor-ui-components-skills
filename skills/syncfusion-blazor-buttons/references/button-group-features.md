# Button Group Component - Features

## Table of Contents
- [Getting Started](#getting-started)
- [Installation](#installation)
- [Basic Button Group](#basic-button-group)
- [Orientation](#orientation)
- [Selection Modes](#selection-modes)
- [Nesting Button Groups](#nesting-button-groups)
- [Native Event Handling](#native-event-handling)
- [Item Configuration](#item-configuration)
- [Common Scenarios](#common-scenarios)
- [Troubleshooting](#troubleshooting)

---

## Getting Started

The Blazor ButtonGroup component groups multiple buttons together as a single unit. It supports horizontal/vertical orientation, single/multiple selection modes, and nested groups.

### Prerequisites
- .NET SDK 6.0 or later
- Blazor WebAssembly or Blazor Server application
- Visual Studio 2022 or VS Code

---

## Installation

### Step 1: Install NuGet Packages

```bash
dotnet add package Syncfusion.Blazor.SplitButtons
dotnet add package Syncfusion.Blazor.Themes
```

### Step 2: Add Namespaces

In `~/_Imports.razor`:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.SplitButtons
```

### Step 3: Register Service

In `~/Program.cs`:

```csharp
using Syncfusion.Blazor;

builder.Services.AddSyncfusionBlazor();
await builder.Build().RunAsync();
```

### Step 4: Add Theme and Script

In `~/index.html`:

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
</head>
```

---

## Basic Button Group

### Simple Button Group

```razor
<SfButtonGroup>
    <ButtonGroupButton>Left</ButtonGroupButton>
    <ButtonGroupButton>Center</ButtonGroupButton>
    <ButtonGroupButton>Right</ButtonGroupButton>
</SfButtonGroup>
```

### Button Group with Events

```razor
<SfButtonGroup>
    <ButtonGroupButton @onclick="OnLeftClick">Left</ButtonGroupButton>
    <ButtonGroupButton @onclick="OnCenterClick">Center</ButtonGroupButton>
    <ButtonGroupButton @onclick="OnRightClick">Right</ButtonGroupButton>
</SfButtonGroup>

@code {
    private void OnLeftClick() => Console.WriteLine("Left clicked");
    private void OnCenterClick() => Console.WriteLine("Center clicked");
    private void OnRightClick() => Console.WriteLine("Right clicked");
}
```

### Button Group with Icons

```razor
<SfButtonGroup>
    <ButtonGroupButton IconCss="e-icons e-left-icon"></ButtonGroupButton>
    <ButtonGroupButton IconCss="e-icons e-middle-icon"></ButtonGroupButton>
    <ButtonGroupButton IconCss="e-icons e-right-icon"></ButtonGroupButton>
</SfButtonGroup>

<style>
    .e-left-icon::before { content: '\e729'; }
    .e-middle-icon::before { content: '\e72a'; }
    .e-right-icon::before { content: '\e72b'; }
</style>
```

---

## Orientation

### Horizontal Orientation (Default)

```razor
<SfButtonGroup>
    <ButtonGroupButton>Button 1</ButtonGroupButton>
    <ButtonGroupButton>Button 2</ButtonGroupButton>
    <ButtonGroupButton>Button 3</ButtonGroupButton>
</SfButtonGroup>
```

### Vertical Orientation

```razor
<SfButtonGroup Orientation="Orientation.Vertical">
    <ButtonGroupButton>Top</ButtonGroupButton>
    <ButtonGroupButton>Middle</ButtonGroupButton>
    <ButtonGroupButton>Bottom</ButtonGroupButton>
</SfButtonGroup>
```

### Dynamic Orientation

```razor
<label>
    <input type="checkbox" @bind="isVertical" />
    Vertical Orientation
</label>

<SfButtonGroup Orientation="@(isVertical ? Orientation.Vertical : Orientation.Horizontal)">
    <ButtonGroupButton>Button 1</ButtonGroupButton>
    <ButtonGroupButton>Button 2</ButtonGroupButton>
    <ButtonGroupButton>Button 3</ButtonGroupButton>
</SfButtonGroup>

@code {
    private bool isVertical = false;
}
```

---

## Selection Modes

### Single Selection

```razor
<SfButtonGroup Mode="SelectionMode.Single">
    <ButtonGroupButton @onclick="() => SelectOption(1)" 
                       Selected="@(selectedOption == 1)">
        Option 1
    </ButtonGroupButton>
    <ButtonGroupButton @onclick="() => SelectOption(2)"
                       Selected="@(selectedOption == 2)">
        Option 2
    </ButtonGroupButton>
    <ButtonGroupButton @onclick="() => SelectOption(3)"
                       Selected="@(selectedOption == 3)">
        Option 3
    </ButtonGroupButton>
</SfButtonGroup>

<p>Selected: Option @selectedOption</p>

@code {
    private int selectedOption = 1;

    private void SelectOption(int option)
    {
        selectedOption = option;
    }
}
```

### Multiple Selection

```razor
<SfButtonGroup Mode="SelectionMode.Multiple">
    <ButtonGroupButton @onclick="() => ToggleSelection(1)"
                       Selected="@selectedOptions.Contains(1)">
        Bold
    </ButtonGroupButton>
    <ButtonGroupButton @onclick="() => ToggleSelection(2)"
                       Selected="@selectedOptions.Contains(2)">
        Italic
    </ButtonGroupButton>
    <ButtonGroupButton @onclick="() => ToggleSelection(3)"
                       Selected="@selectedOptions.Contains(3)">
        Underline
    </ButtonGroupButton>
</SfButtonGroup>

<p>Selected: @string.Join(", ", selectedOptions)</p>

@code {
    private List<int> selectedOptions = new();

    private void ToggleSelection(int option)
    {
        if (selectedOptions.Contains(option))
            selectedOptions.Remove(option);
        else
            selectedOptions.Add(option);
    }
}
```

### None Selection Mode

```razor
<SfButtonGroup Mode="SelectionMode.None">
    <ButtonGroupButton @onclick="OnAction1">Action 1</ButtonGroupButton>
    <ButtonGroupButton @onclick="OnAction2">Action 2</ButtonGroupButton>
    <ButtonGroupButton @onclick="OnAction3">Action 3</ButtonGroupButton>
</SfButtonGroup>

@code {
    private void OnAction1() => Console.WriteLine("Action 1");
    private void OnAction2() => Console.WriteLine("Action 2");
    private void OnAction3() => Console.WriteLine("Action 3");
}
```

---

## Nesting Button Groups

### Nested Horizontal Groups

```razor
<SfButtonGroup>
    <ButtonGroupButton>File</ButtonGroupButton>
    <ButtonGroupButton>Edit</ButtonGroupButton>
    <SfButtonGroup>
        <ButtonGroupButton IconCss="e-icons e-bold"></ButtonGroupButton>
        <ButtonGroupButton IconCss="e-icons e-italic"></ButtonGroupButton>
        <ButtonGroupButton IconCss="e-icons e-underline"></ButtonGroupButton>
    </SfButtonGroup>
</SfButtonGroup>

<style>
    .e-bold::before { content: '\e736'; }
    .e-italic::before { content: '\e737'; }
    .e-underline::before { content: '\e738'; }
</style>
```

### Mixed Orientation Nesting

```razor
<SfButtonGroup Orientation="Orientation.Vertical">
    <ButtonGroupButton>Top Group</ButtonGroupButton>
    <SfButtonGroup>
        <ButtonGroupButton>H1</ButtonGroupButton>
        <ButtonGroupButton>H2</ButtonGroupButton>
        <ButtonGroupButton>H3</ButtonGroupButton>
    </SfButtonGroup>
    <ButtonGroupButton>Bottom Group</ButtonGroupButton>
</SfButtonGroup>
```

---

## Native Event Handling

### Click Events

```razor
<SfButtonGroup>
    <ButtonGroupButton @onclick="HandleClick1">
        Button 1
    </ButtonGroupButton>
    <ButtonGroupButton @onclick="HandleClick2">
        Button 2
    </ButtonGroupButton>
</SfButtonGroup>

@code {
    private void HandleClick1()
    {
        Console.WriteLine("Button 1 clicked");
    }

    private void HandleClick2()
    {
        Console.WriteLine("Button 2 clicked");
    }
}
```

### Mouse Events

```razor
<SfButtonGroup>
    <ButtonGroupButton @onclick="OnClick"
                       @onmouseover="OnMouseOver"
                       @onmouseout="OnMouseOut">
        Hover Me
    </ButtonGroupButton>
</SfButtonGroup>

<p>@eventMessage</p>

@code {
    private string eventMessage = "";

    private void OnClick() => eventMessage = "Clicked";
    private void OnMouseOver() => eventMessage = "Mouse Over";
    private void OnMouseOut() => eventMessage = "Mouse Out";
}
```

### Keyboard Events

```razor
<SfButtonGroup>
    <ButtonGroupButton @onkeydown="OnKeyDown">
        Press Key
    </ButtonGroupButton>
</SfButtonGroup>

@code {
    private void OnKeyDown(KeyboardEventArgs e)
    {
        Console.WriteLine($"Key pressed: {e.Key}");
    }
}
```

---

## Item Configuration

### Disabled Buttons

```razor
<SfButtonGroup>
    <ButtonGroupButton>Enabled</ButtonGroupButton>
    <ButtonGroupButton Disabled="true">Disabled</ButtonGroupButton>
    <ButtonGroupButton>Enabled</ButtonGroupButton>
</SfButtonGroup>
```

### Buttons with Content

```razor
<SfButtonGroup>
    <ButtonGroupButton IconCss="e-icons e-cut">Cut</ButtonGroupButton>
    <ButtonGroupButton IconCss="e-icons e-copy">Copy</ButtonGroupButton>
    <ButtonGroupButton IconCss="e-icons e-paste">Paste</ButtonGroupButton>
</SfButtonGroup>

<style>
    .e-cut::before { content: '\e73f'; }
    .e-copy::before { content: '\e721'; }
    .e-paste::before { content: '\e739'; }
</style>
```

### Custom CSS Classes

```razor
<SfButtonGroup>
    <ButtonGroupButton CssClass="custom-btn-1">Custom 1</ButtonGroupButton>
    <ButtonGroupButton CssClass="custom-btn-2">Custom 2</ButtonGroupButton>
    <ButtonGroupButton CssClass="custom-btn-3">Custom 3</ButtonGroupButton>
</SfButtonGroup>

<style>
    .custom-btn-1 { background-color: #e3f2fd; }
    .custom-btn-2 { background-color: #f3e5f5; }
    .custom-btn-3 { background-color: #e8f5e9; }
</style>
```

---

## Common Scenarios

### Text Alignment Selector

```razor
<SfButtonGroup Mode="SelectionMode.Single">
    <ButtonGroupButton IconCss="e-icons e-align-left"
                       @onclick="() => SetAlignment(\"left\")"
                       Selected="@(alignment == \"left\")">
    </ButtonGroupButton>
    <ButtonGroupButton IconCss="e-icons e-align-center"
                       @onclick="() => SetAlignment(\"center\")"
                       Selected="@(alignment == \"center\")">
    </ButtonGroupButton>
    <ButtonGroupButton IconCss="e-icons e-align-right"
                       @onclick="() => SetAlignment(\"right\")"
                       Selected="@(alignment == \"right\")">
    </ButtonGroupButton>
</SfButtonGroup>

<div style="text-align: @alignment; margin-top: 20px;">
    <p>This text is aligned @alignment</p>
</div>

@code {
    private string alignment = "left";

    private void SetAlignment(string value)
    {
        alignment = value;
    }
}

<style>
    .e-align-left::before { content: '\e739'; }
    .e-align-center::before { content: '\e73a'; }
    .e-align-right::before { content: '\e73b'; }
</style>
```

### View Mode Switcher

```razor
<SfButtonGroup Mode="SelectionMode.Single">
    <ButtonGroupButton IconCss="e-icons e-grid-view"
                       @onclick="() => viewMode = \"grid\""
                       Selected="@(viewMode == \"grid\")">
    </ButtonGroupButton>
    <ButtonGroupButton IconCss="e-icons e-list-view"
                       @onclick="() => viewMode = \"list\""
                       Selected="@(viewMode == \"list\")">
    </ButtonGroupButton>
</SfButtonGroup>

<div style="margin-top: 20px;">
    @if (viewMode == "grid")
    {
        <div class="grid-view">Grid View Content</div>
    }
    else
    {
        <div class="list-view">List View Content</div>
    }
</div>

@code {
    private string viewMode = "grid";
}

<style>
    .e-grid-view::before { content: '\e7a8'; }
    .e-list-view::before { content: '\e7c5'; }
</style>
```

### Zoom Controls

```razor
<SfButtonGroup>
    <ButtonGroupButton IconCss="e-icons e-zoom-out"
                       @onclick="ZoomOut"
                       Disabled="@(zoomLevel <= 50)">
    </ButtonGroupButton>
    <ButtonGroupButton @onclick="ResetZoom">
        @zoomLevel%
    </ButtonGroupButton>
    <ButtonGroupButton IconCss="e-icons e-zoom-in"
                       @onclick="ZoomIn"
                       Disabled="@(zoomLevel >= 200)">
    </ButtonGroupButton>
</SfButtonGroup>

@code {
    private int zoomLevel = 100;

    private void ZoomIn() => zoomLevel = Math.Min(200, zoomLevel + 10);
    private void ZoomOut() => zoomLevel = Math.Max(50, zoomLevel - 10);
    private void ResetZoom() => zoomLevel = 100;
}

<style>
    .e-zoom-in::before { content: '\e7a3'; }
    .e-zoom-out::before { content: '\e7a4'; }
</style>
```

### Toolbar with Button Groups

```razor
<div class="toolbar">
    <SfButtonGroup>
        <ButtonGroupButton IconCss="e-icons e-cut">Cut</ButtonGroupButton>
        <ButtonGroupButton IconCss="e-icons e-copy">Copy</ButtonGroupButton>
        <ButtonGroupButton IconCss="e-icons e-paste">Paste</ButtonGroupButton>
    </SfButtonGroup>

    <SfButtonGroup Mode="SelectionMode.Multiple">
        <ButtonGroupButton IconCss="e-icons e-bold"></ButtonGroupButton>
        <ButtonGroupButton IconCss="e-icons e-italic"></ButtonGroupButton>
        <ButtonGroupButton IconCss="e-icons e-underline"></ButtonGroupButton>
    </SfButtonGroup>

    <SfButtonGroup>
        <ButtonGroupButton IconCss="e-icons e-undo"></ButtonGroupButton>
        <ButtonGroupButton IconCss="e-icons e-redo"></ButtonGroupButton>
    </SfButtonGroup>
</div>

<style>
    .toolbar {
        display: flex;
        gap: 20px;
        padding: 10px;
        background: #f5f5f5;
        border-radius: 4px;
    }
    .e-cut::before { content: '\e73f'; }
    .e-copy::before { content: '\e721'; }
    .e-paste::before { content: '\e739'; }
    .e-bold::before { content: '\e736'; }
    .e-italic::before { content: '\e737'; }
    .e-underline::before { content: '\e738'; }
    .e-undo::before { content: '\e7d7'; }
    .e-redo::before { content: '\e7d8'; }
</style>
```

---

## Troubleshooting

### Selection Not Working

**Problem:** Button selection state not updating

**Solution:** Use proper selection mode and state management

```razor
<!-- Correct -->
<SfButtonGroup Mode="SelectionMode.Single">
    <ButtonGroupButton @onclick="() => selected = 1"
                       Selected="@(selected == 1)">
        Button 1
    </ButtonGroupButton>
</SfButtonGroup>

@code {
    private int selected = 1;
}
```

### Icons Not Displaying

**Problem:** Icons show as empty boxes

**Solution:** Ensure icon CSS is properly defined

```razor
<SfButtonGroup>
    <ButtonGroupButton IconCss="e-icons e-custom-icon">
    </ButtonGroupButton>
</SfButtonGroup>

<style>
    .e-custom-icon::before {
        content: '\e823';  /* Valid unicode */
    }
</style>
```

### Nested Groups Not Aligned

**Problem:** Nested button groups misaligned

**Solution:** Use proper CSS for layout

```razor
<SfButtonGroup>
    <ButtonGroupButton>Button 1</ButtonGroupButton>
    <SfButtonGroup CssClass="nested-group">
        <ButtonGroupButton>Nested 1</ButtonGroupButton>
        <ButtonGroupButton>Nested 2</ButtonGroupButton>
    </SfButtonGroup>
</SfButtonGroup>

<style>
    .nested-group {
        margin-left: 10px;
    }
</style>
```

---

## Best Practices

1. **Use selection modes appropriately** - Single for mutually exclusive options, Multiple for independent toggles
2. **Provide clear visual feedback** - Ensure selected state is obvious
3. **Keep groups focused** - Don't mix unrelated actions in one group
4. **Use icons consistently** - Either all buttons have icons or none
5. **Consider mobile** - Ensure touch targets are adequate (44px minimum)
6. **Test keyboard navigation** - Verify tab order and selection
7. **Provide tooltips** - Especially for icon-only buttons
8. **Maintain consistency** - Use same patterns across the application

---

## Next Steps

- Explore [button-group-styling.md](button-group-styling.md) for customization
- Learn about [button-group-advanced-features.md](button-group-advanced-features.md) for accessibility
