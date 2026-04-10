# DropdownMenu Component - Features

## Installation

```bash
dotnet add package Syncfusion.Blazor.SplitButtons
```

## Basic Usage

```razor
<SfDropDownButton Content="Settings">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Account"></DropDownMenuItem>
        <DropDownMenuItem Text="Profile"></DropDownMenuItem>
        <DropDownMenuItem Text="Logout"></DropDownMenuItem>
    </DropDownMenuItems>
</SfDropDownButton>
```

## With Icons

```razor
<SfDropDownButton Content="Actions" IconCss="e-icons e-down">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Edit" IconCss="e-icons e-edit"></DropDownMenuItem>
        <DropDownMenuItem Text="Delete" IconCss="e-icons e-delete"></DropDownMenuItem>
        <DropDownMenuItem Text="Share" IconCss="e-icons e-share"></DropDownMenuItem>
    </DropDownMenuItems>
</SfDropDownButton>
```

## With Separators

```razor
<SfDropDownButton Content="File">
    <DropDownMenuItems>
        <DropDownMenuItem Text="New"></DropDownMenuItem>
        <DropDownMenuItem Text="Open"></DropDownMenuItem>
        <DropDownMenuItem Separator="true"></DropDownMenuItem>
        <DropDownMenuItem Text="Save"></DropDownMenuItem>
        <DropDownMenuItem Text="Exit"></DropDownMenuItem>
    </DropDownMenuItems>
</SfDropDownButton>
```

## Item Click Event

```razor
<SfDropDownButton Content="Options">
    <DropDownButtonEvents ItemSelected="OnItemSelect"></DropDownButtonEvents>
    <DropDownMenuItems>
        <DropDownMenuItem Id="opt1" Text="Option 1"></DropDownMenuItem>
        <DropDownMenuItem Id="opt2" Text="Option 2"></DropDownMenuItem>
    </DropDownMenuItems>
</SfDropDownButton>

@code {
    private void OnItemSelect(MenuEventArgs args)
    {
        Console.WriteLine($"Selected: {args.Item.Text}");
    }
}
```

## Disabled Items

```razor
<SfDropDownButton Content="Actions">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Edit"></DropDownMenuItem>
        <DropDownMenuItem Text="Delete" Disabled="true"></DropDownMenuItem>
        <DropDownMenuItem Text="Share"></DropDownMenuItem>
    </DropDownMenuItems>
</SfDropDownButton>
```

## Custom Positioning

```razor
<SfDropDownButton Content="Menu" Target="#target">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
        <DropDownMenuItem Text="Item 2"></DropDownMenuItem>
    </DropDownMenuItems>
</SfDropDownButton>
```

## Dynamic Items

```razor
<SfDropDownButton Content="@buttonContent">
    <DropDownMenuItems>
        @foreach (var item in menuItems)
        {
            <DropDownMenuItem Text="@item.Text" Id="@item.Id"></DropDownMenuItem>
        }
    </DropDownMenuItems>
</SfDropDownButton>

@code {
    private string buttonContent = "Select";
    private List<MenuItem> menuItems = new()
    {
        new MenuItem { Id = "1", Text = "Option 1" },
        new MenuItem { Id = "2", Text = "Option 2" }
    };

    public class MenuItem
    {
        public string Id { get; set; }
        public string Text { get; set; }
    }
}
```

## Open/Close Events

```razor
<SfDropDownButton Content="Menu">
    <DropDownButtonEvents OnOpen="OnOpen" OnClose="OnClose"></DropDownButtonEvents>
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfDropDownButton>

@code {
    private void OnOpen(BeforeOpenCloseMenuEventArgs args)
    {
        Console.WriteLine("Menu opened");
    }

    private void OnClose(BeforeOpenCloseMenuEventArgs args)
    {
        Console.WriteLine("Menu closed");
    }
}
```

## Best Practices

1. Use descriptive menu item text
2. Group related items with separators
3. Provide icons for better UX
4. Handle item click events appropriately
5. Consider accessibility with keyboard navigation

---

See also: [dropdown-menu-styling.md](dropdown-menu-styling.md) | [dropdown-menu-advanced-features.md](dropdown-menu-advanced-features.md)
