# SpeedDial Component - Features

## Installation

```bash
dotnet add package Syncfusion.Blazor.SplitButtons
```

## API Reference

### SfSpeedDial Properties

**Content & Icons:**
- `Content` (string) - Text content for the speed dial button
- `OpenIconCss` (string) - CSS class for open icon
- `CloseIconCss` (string) - CSS class for close icon
- `IconPosition` (IconPosition enum) - Icon position

**Positioning:**
- `Position` (FabPosition enum) - Speed dial position (TopLeft, TopCenter, TopRight, MiddleLeft, MiddleCenter, MiddleRight, BottomLeft, BottomCenter, BottomRight)
- `Target` (string) - Target element selector

**Behavior:**
- `Mode` (SpeedDialMode enum) - Display mode (Linear or Radial)
- `Direction` (LinearDirection enum) - Linear direction (Up, Down, Left, Right, Auto)
- `OpensOnHover` (bool) - Open on hover
- `IsModal` (bool) - Show modal backdrop
- `Visible` (bool) - Visibility state
- `Disabled` (bool) - Disabled state
- `IsPrimary` (bool) - Primary button style

**Styling:**
- `CssClass` (string) - Custom CSS classes
- `HtmlAttributes` (Dictionary<string, object>) - Custom HTML attributes

**Templates:**
- `ItemTemplate` (RenderFragment<SpeedDialItem>) - Custom item template
- `PopupTemplate` (RenderFragment) - Custom popup template
- `ChildContent` (RenderFragment) - Child content

**Events:**
- `Opening` (EventCallback<SpeedDialBeforeOpenCloseEventArgs>) - Before opening (cancelable)
- `Opened` (EventCallback<SpeedDialOpenCloseEventArgs>) - After opened
- `Closing` (EventCallback<SpeedDialBeforeOpenCloseEventArgs>) - Before closing (cancelable)
- `Closed` (EventCallback<SpeedDialOpenCloseEventArgs>) - After closed
- `ItemClicked` (EventCallback<SpeedDialItemEventArgs>) - When item is clicked
- `ItemRendered` (EventCallback<SpeedDialItemEventArgs>) - When item is rendered
- `Created` (EventCallback<Object>) - Component created
- `VisibleChanged` (EventCallback<Boolean>) - Visibility changed

### SpeedDialItem Properties

- `ID` (string) - Unique identifier (note: uppercase 'ID')
- `Text` (string) - Item text
- `IconCss` (string) - Icon CSS class
- `Title` (string) - Tooltip text
- `Disabled` (bool) - Disable specific item
- `HtmlAttributes` (Dictionary<string, object>) - Custom HTML attributes

## Basic SpeedDial

```razor
<SfSpeedDial Content="Actions" OpenIconCss="e-icons e-edit">
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-cut" Text="Cut"></SpeedDialItem>
        <SpeedDialItem IconCss="e-icons e-copy" Text="Copy"></SpeedDialItem>
        <SpeedDialItem IconCss="e-icons e-paste" Text="Paste"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>
```

## Display Modes

### Linear Mode

```razor
<SfSpeedDial Mode="SpeedDialMode.Linear" Position="FabPosition.BottomRight">
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-edit"></SpeedDialItem>
        <SpeedDialItem IconCss="e-icons e-delete"></SpeedDialItem>
        <SpeedDialItem IconCss="e-icons e-share"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>
```

### Radial Mode

```razor
<SfSpeedDial Mode="SpeedDialMode.Radial">
    <SpeedDialRadialSettings Offset="80px" RadialDirection="RadialDirection.AntiClockwise"></SpeedDialRadialSettings>
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-edit"></SpeedDialItem>
        <SpeedDialItem IconCss="e-icons e-delete"></SpeedDialItem>
        <SpeedDialItem IconCss="e-icons e-share"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>
```

## Positioning

```razor
<!-- Bottom Right -->
<SfSpeedDial Position="FabPosition.BottomRight">
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-edit"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>

<!-- Top Left -->
<SfSpeedDial Position="FabPosition.TopLeft">
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-edit"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>
```

## Direction

```razor
<!-- Up -->
<SfSpeedDial Direction="LinearDirection.Up">
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-edit"></SpeedDialItem>
        <SpeedDialItem IconCss="e-icons e-delete"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>

<!-- Down -->
<SfSpeedDial Direction="LinearDirection.Down">
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-edit"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>

<!-- Left -->
<SfSpeedDial Direction="LinearDirection.Left">
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-edit"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>

<!-- Right -->
<SfSpeedDial Direction="LinearDirection.Right">
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-edit"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>
```

## Open Modes

### Click Mode

```razor
<SfSpeedDial OpenIconCss="e-icons e-plus" CloseIconCss="e-icons e-close" OpensOnClick="true">
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-edit"></SpeedDialItem>
        <SpeedDialItem IconCss="e-icons e-delete"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>
```

### Hover Mode

```razor
<SfSpeedDial OpensOnHover="true">
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-edit"></SpeedDialItem>
        <SpeedDialItem IconCss="e-icons e-delete"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>
```

## Item Click Event

```razor
<SfSpeedDial ItemClicked="OnItemClick">
    <SpeedDialItems>
        <SpeedDialItem ID="edit" IconCss="e-icons e-edit" Text="Edit"></SpeedDialItem>
        <SpeedDialItem ID="delete" IconCss="e-icons e-delete" Text="Delete"></SpeedDialItem>
        <SpeedDialItem ID="share" IconCss="e-icons e-share" Text="Share"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>

@code {
    private void OnItemClick(SpeedDialItemEventArgs args)
    {
        Console.WriteLine($"Clicked: {args.Item.Text}");
        
        switch (args.Item.ID)
        {
            case "edit":
                EditAction();
                break;
            case "delete":
                DeleteAction();
                break;
            case "share":
                ShareAction();
                break;
        }
    }

    private void EditAction() => Console.WriteLine("Edit action");
    private void DeleteAction() => Console.WriteLine("Delete action");
    private void ShareAction() => Console.WriteLine("Share action");
}
```

## Modal Backdrop

```razor
<SfSpeedDial IsModal="true">
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-edit"></SpeedDialItem>
        <SpeedDialItem IconCss="e-icons e-delete"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>
```

## Custom Icons

```razor
<SfSpeedDial OpenIconCss="e-icons e-menu" CloseIconCss="e-icons e-close">
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-home" Text="Home"></SpeedDialItem>
        <SpeedDialItem IconCss="e-icons e-settings" Text="Settings"></SpeedDialItem>
        <SpeedDialItem IconCss="e-icons e-user" Text="Profile"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>
```

---

See also: [speeddial-styling.md](speeddial-styling.md) | [speeddial-advanced-features.md](speeddial-advanced-features.md)
