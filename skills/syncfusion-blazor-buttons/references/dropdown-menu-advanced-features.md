# DropdownMenu Component - Advanced Features

## Accessibility

### ARIA Support

The DropdownMenu automatically includes:
- `aria-haspopup="true"`
- `aria-expanded` states
- `aria-label` for screen readers

```razor
<SfDropDownButton Content="Accessible Menu" CssClass="accessible-dropdown">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Option 1"></DropDownMenuItem>
        <DropDownMenuItem Text="Option 2"></DropDownMenuItem>
    </DropDownMenuItems>
</SfDropDownButton>
```

### Keyboard Navigation

- **Space/Enter**: Open dropdown
- **Arrow Up/Down**: Navigate items
- **Enter**: Select item
- **Esc**: Close dropdown
- **Tab**: Move focus

## State Management

```razor
<SfDropDownButton Content="@selectedText" @ref="dropdownRef">
    <DropDownButtonEvents ItemSelected="OnSelect"></DropDownButtonEvents>
    <DropDownMenuItems>
        @foreach (var item in items)
        {
            <DropDownMenuItem Text="@item" Id="@item"></DropDownMenuItem>
        }
    </DropDownMenuItems>
</SfDropDownButton>

<button @onclick="ResetSelection">Reset</button>

@code {
    private SfDropDownButton dropdownRef;
    private string selectedText = "Select Option";
    private List<string> items = new() { "Option 1", "Option 2", "Option 3" };

    private void OnSelect(MenuEventArgs args)
    {
        selectedText = args.Item.Text;
    }

    private void ResetSelection()
    {
        selectedText = "Select Option";
        StateHasChanged();
    }
}
```

## Programmatic Control

```razor
<SfDropDownButton @ref="dropdown" Content="Controlled">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfDropDownButton>

<button @onclick="OpenMenu">Open</button>
<button @onclick="CloseMenu">Close</button>

@code {
    private SfDropDownButton dropdown;

    private async Task OpenMenu()
    {
        // Open programmatically if needed
    }

    private async Task CloseMenu()
    {
        // Close programmatically if needed
    }
}
```

## Event Handling

```razor
<SfDropDownButton Content="Events">
    <DropDownButtonEvents 
        Created="OnCreated"
        OnOpen="OnBeforeOpen"
        Opened="OnOpened"
        OnClose="OnBeforeClose"
        Closed="OnClosed"
        ItemSelected="OnItemSelect">
    </DropDownButtonEvents>
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfDropDownButton>

@code {
    private void OnCreated() => Console.WriteLine("Created");
    private void OnBeforeOpen(BeforeOpenCloseMenuEventArgs args) => Console.WriteLine("Before Open");
    private void OnOpened(OpenCloseMenuEventArgs args) => Console.WriteLine("Opened");
    private void OnBeforeClose(BeforeOpenCloseMenuEventArgs args) => Console.WriteLine("Before Close");
    private void OnClosed(OpenCloseMenuEventArgs args) => Console.WriteLine("Closed");
    private void OnItemSelect(MenuEventArgs args) => Console.WriteLine($"Selected: {args.Item.Text}");
}
```

## Performance

### Lazy Loading

```razor
<SfDropDownButton Content="Lazy Load">
    <DropDownButtonEvents OnOpen="LoadItems"></DropDownButtonEvents>
    <DropDownMenuItems>
        @if (itemsLoaded)
        {
            @foreach (var item in items)
            {
                <DropDownMenuItem Text="@item"></DropDownMenuItem>
            }
        }
    </DropDownMenuItems>
</SfDropDownButton>

@code {
    private List<string> items = new();
    private bool itemsLoaded = false;

    private void LoadItems(BeforeOpenCloseMenuEventArgs args)
    {
        if (!itemsLoaded)
        {
            items = GetItemsFromDatabase();
            itemsLoaded = true;
        }
    }

    private List<string> GetItemsFromDatabase()
    {
        // Simulated data loading
        return new List<string> { "Item 1", "Item 2", "Item 3" };
    }
}
```

## Testing

```csharp
[Fact]
public void DropdownMenu_Opens_OnClick()
{
    var cut = RenderComponent<SfDropDownButton>();
    var button = cut.Find("button");
    
    button.Click();
    
    var popup = cut.Find(".e-dropdown-popup");
    Assert.NotNull(popup);
}
```

---

See also: [dropdown-menu-features.md](dropdown-menu-features.md) | [dropdown-menu-styling.md](dropdown-menu-styling.md)
