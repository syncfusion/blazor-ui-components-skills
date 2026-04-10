# SpeedDial Component - Advanced Features

## Accessibility

### ARIA Support

SpeedDial includes:
- `role="menu"` for action items
- `aria-label` for screen readers
- `aria-expanded` states

```razor
<SfSpeedDial CssClass="accessible-speeddial">
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-edit" Text="Edit"></SpeedDialItem>
        <SpeedDialItem IconCss="e-icons e-delete" Text="Delete"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>
```

## Keyboard Navigation

- **Space/Enter**: Open/close SpeedDial
- **Arrow Keys**: Navigate items
- **Enter**: Select item
- **Esc**: Close SpeedDial

## Event Handling

```razor
<SfSpeedDial
        Created="OnCreated"
        OnOpen="OnBeforeOpen"
        Opened="OnOpened"
        OnClose="OnBeforeClose"
        Closed="OnClosed"
        ItemClicked="OnItemClick">
    
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-edit"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>

@code {
    private void OnCreated() => Console.WriteLine("Created");
    private void OnBeforeOpen(SpeedDialBeforeOpenCloseEventArgs args) => Console.WriteLine("Before Open");
    private void OnOpened(SpeedDialOpenCloseEventArgs args) => Console.WriteLine("Opened");
    private void OnBeforeClose(SpeedDialBeforeOpenCloseEventArgs args) => Console.WriteLine("Before Close");
    private void OnClosed(SpeedDialOpenCloseEventArgs args) => Console.WriteLine("Closed");
    private void OnItemClick(SpeedDialItemEventArgs args) => Console.WriteLine($"Item: {args.Item.Text}");
}
```

## State Management

```razor
<SfSpeedDial @ref="speedDialRef" Visible="@isVisible">
    <SpeedDialItems>
        <SpeedDialItem Id="edit" IconCss="e-icons e-edit"></SpeedDialItem>
        <SpeedDialItem Id="delete" IconCss="e-icons e-delete"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>

<button @onclick="ToggleSpeedDial">Toggle</button>

@code {
    private SfSpeedDial speedDialRef;
    private bool isVisible = true;

    private void ToggleSpeedDial()
    {
        isVisible = !isVisible;
    }
}
```

## Programmatic Control

```razor
<SfSpeedDial @ref="speedDialRef">
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-edit"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>

<button @onclick="ShowSpeedDial">Show</button>
<button @onclick="HideSpeedDial">Hide</button>

@code {
    private SfSpeedDial speedDialRef;

    private async Task ShowSpeedDial()
    {
        await speedDialRef.ShowAsync();
    }

    private async Task HideSpeedDial()
    {
        await speedDialRef.HideAsync();
    }
}
```

## Performance

Best practices:
1. Limit action items to 5-7 for usability
2. Use icons instead of text when possible
3. Position appropriately to avoid overlap
4. Use modal backdrop sparingly

---

See also: [speeddial-features.md](speeddial-features.md) | [speeddial-styling.md](speeddial-styling.md)
