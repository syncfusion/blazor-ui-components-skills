# SplitButton Component - Advanced Features

## Accessibility

### ARIA Support

SplitButton automatically includes:
- `aria-haspopup="true"`
- `aria-expanded` states
- `role="button"` attributes

```razor
<SfSplitButton Content="Accessible" CssClass="accessible-split">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Option 1"></DropDownMenuItem>
        <DropDownMenuItem Text="Option 2"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>

<style>
    .accessible-split:focus-visible {
        outline: 2px solid #0078d4;
        outline-offset: 2px;
    }
</style>
```

## Keyboard Navigation

- **Space/Enter**: Click primary button
- **Tab**: Move focus between primary and dropdown
- **Arrow Down**: Open dropdown
- **Arrow Up/Down**: Navigate items
- **Enter**: Select item
- **Esc**: Close dropdown

## State Management

```razor
<SfSplitButton Content="@buttonContent" @ref="splitButtonRef">
    <SplitButtonEvents 
        OnClick="OnPrimaryAction"
        ItemSelected="OnItemSelect">
    </SplitButtonEvents>
    <DropDownMenuItems>
        @foreach (var item in menuItems)
        {
            <DropDownMenuItem Text="@item.Text" Id="@item.Id"></DropDownMenuItem>
        }
    </DropDownMenuItems>
</SfSplitButton>

<button @onclick="ResetButton">Reset</button>

@code {
    private SfSplitButton splitButtonRef;
    private string buttonContent = "Actions";
    private List<MenuItem> menuItems = new()
    {
        new MenuItem { Id = "1", Text = "Action 1" },
        new MenuItem { Id = "2", Text = "Action 2" }
    };

    private void OnPrimaryAction(ClickEventArgs args)
    {
        Console.WriteLine("Primary action triggered");
    }

    private void OnItemSelect(MenuEventArgs args)
    {
        buttonContent = args.Item.Text;
        Console.WriteLine($"Selected: {args.Item.Text}");
    }

    private void ResetButton()
    {
        buttonContent = "Actions";
        StateHasChanged();
    }

    public class MenuItem
    {
        public string Id { get; set; }
        public string Text { get; set; }
    }
}
```

## Event Handling

```razor
<SfSplitButton Content="Events">
    <SplitButtonEvents 
        Created="OnCreated"
        OnClick="OnPrimaryClick"
        OnOpen="OnBeforeOpen"
        Opened="OnOpened"
        OnClose="OnBeforeClose"
        Closed="OnClosed"
        ItemSelected="OnItemSelect">
    </SplitButtonEvents>
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>

@code {
    private void OnCreated() => Console.WriteLine("Created");
    private void OnPrimaryClick(ClickEventArgs args) => Console.WriteLine("Primary clicked");
    private void OnBeforeOpen(BeforeOpenCloseMenuEventArgs args) => Console.WriteLine("Before Open");
    private void OnOpened(OpenCloseMenuEventArgs args) => Console.WriteLine("Opened");
    private void OnBeforeClose(BeforeOpenCloseMenuEventArgs args) => Console.WriteLine("Before Close");
    private void OnClosed(OpenCloseMenuEventArgs args) => Console.WriteLine("Closed");
    private void OnItemSelect(MenuEventArgs args) => Console.WriteLine($"Selected: {args.Item.Text}");
}
```

## Programmatic Control

```razor
<SfSplitButton @ref="splitBtn" Content="Controlled">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>

<button @onclick="ToggleMenu">Toggle Menu</button>

@code {
    private SfSplitButton splitBtn;
    private bool isOpen = false;

    private async Task ToggleMenu()
    {
        // Toggle logic if needed
        isOpen = !isOpen;
    }
}
```

## Performance

Best practices:
1. Minimize menu items (5-10 max)
2. Use icons for visual clarity
3. Group related actions
4. Handle events efficiently
5. Avoid nested dropdowns

## Testing

```csharp
[Fact]
public void SplitButton_PrimaryAction_Triggers()
{
    bool primaryClicked = false;
    var cut = RenderComponent<SfSplitButton>(parameters => parameters
        .Add(p => p.Content, "Save")
        .Add(p => p.SplitButtonEvents, new SplitButtonEvents
        {
            OnClick = _ => primaryClicked = true
        }));
    
    var primaryButton = cut.Find("button.e-btn");
    primaryButton.Click();
    
    Assert.True(primaryClicked);
}
```

---

See also: [split-button-features.md](split-button-features.md) | [split-button-styling.md](split-button-styling.md)
