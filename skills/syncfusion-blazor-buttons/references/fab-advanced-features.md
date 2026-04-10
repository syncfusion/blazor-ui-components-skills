# Floating Action Button (FAB) - Advanced Features

## Accessibility

### ARIA Attributes

FAB automatically includes:
- `role="button"`
- `aria-label` for screen readers
- Focus indicators

```razor
<SfFab IconCss="e-icons e-edit" CssClass="accessible-fab"></SfFab>

<style>
    .accessible-fab:focus-visible {
        outline: 2px solid #0078d4;
        outline-offset: 2px;
    }
</style>
```

## Keyboard Navigation

- **Space/Enter**: Trigger FAB click
- **Tab**: Move focus to/from FAB

## Event Handling

```razor
<SfFab IconCss="e-icons e-plus">
    <FabEvents 
        Created="OnCreated"
        OnClick="OnClick">
    </FabEvents>
</SfFab>

@code {
    private void OnCreated()
    {
        Console.WriteLine("FAB created");
    }

    private void OnClick(FabClickEventArgs args)
    {
        Console.WriteLine("FAB clicked");
        // Perform action
    }
}
```

## State Management

```razor
<SfFab IconCss="@fabIcon" Visible="@showFab">
    <FabEvents OnClick="PerformAction"></FabEvents>
</SfFab>

@code {
    private bool showFab = true;
    private string fabIcon = "e-icons e-plus";

    private async Task PerformAction(FabClickEventArgs args)
    {
        showFab = false;
        await Task.Delay(2000);
        showFab = true;
    }
}
```

## Programmatic Control

```razor
<SfFab @ref="fabRef" IconCss="e-icons e-edit"></SfFab>

<button @onclick="TriggerFab">Trigger FAB</button>

@code {
    private SfFab fabRef;

    private async Task TriggerFab()
    {
        await fabRef.FocusAsync();
    }
}
```

## Performance

Best practices:
1. Use single FAB per view
2. Minimize DOM updates
3. Lazy load action handlers
4. Use efficient event handlers

---

See also: [fab-features.md](fab-features.md) | [fab-styling.md](fab-styling.md)
