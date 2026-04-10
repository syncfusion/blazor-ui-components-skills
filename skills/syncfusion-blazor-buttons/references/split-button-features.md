# SplitButton Component - Features

## Installation

```bash
dotnet add package Syncfusion.Blazor.SplitButtons
```

## Basic SplitButton

```razor
<SfSplitButton Content="Paste">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Paste"></DropDownMenuItem>
        <DropDownMenuItem Text="Paste Special"></DropDownMenuItem>
        <DropDownMenuItem Text="Paste as Text"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>
```

## Primary Button Action

```razor
<SfSplitButton Content="Save">
    <SplitButtonEvents OnClick="OnPrimaryClick" ItemSelected="OnItemSelect"></SplitButtonEvents>
    <DropDownMenuItems>
        <DropDownMenuItem Text="Save"></DropDownMenuItem>
        <DropDownMenuItem Text="Save As"></DropDownMenuItem>
        <DropDownMenuItem Text="Save All"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>

@code {
    private void OnPrimaryClick(ClickEventArgs args)
    {
        Console.WriteLine("Primary button clicked - Save");
        SaveDocument();
    }

    private void OnItemSelect(MenuEventArgs args)
    {
        Console.WriteLine($"Menu item selected: {args.Item.Text}");
        
        switch (args.Item.Text)
        {
            case "Save":
                SaveDocument();
                break;
            case "Save As":
                SaveAsDocument();
                break;
            case "Save All":
                SaveAllDocuments();
                break;
        }
    }

    private void SaveDocument() => Console.WriteLine("Saving...");
    private void SaveAsDocument() => Console.WriteLine("Save As...");
    private void SaveAllDocuments() => Console.WriteLine("Save All...");
}
```

## With Icons

```razor
<SfSplitButton Content="Actions" IconCss="e-icons e-edit">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Edit" IconCss="e-icons e-edit"></DropDownMenuItem>
        <DropDownMenuItem Text="Delete" IconCss="e-icons e-delete"></DropDownMenuItem>
        <DropDownMenuItem Text="Share" IconCss="e-icons e-share"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>
```

## With Separators

```razor
<SfSplitButton Content="Options">
    <DropDownMenuItems>
        <DropDownMenuItem Text="New"></DropDownMenuItem>
        <DropDownMenuItem Text="Open"></DropDownMenuItem>
        <DropDownMenuItem Separator="true"></DropDownMenuItem>
        <DropDownMenuItem Text="Exit"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>
```

## Disabled Items

```razor
<SfSplitButton Content="Actions">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Edit"></DropDownMenuItem>
        <DropDownMenuItem Text="Delete" Disabled="true"></DropDownMenuItem>
        <DropDownMenuItem Text="Share"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>
```

## Disabled SplitButton

```razor
<SfSplitButton Content="Disabled" Disabled="true">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>
```

## Icon Position

```razor
<!-- Icon Left -->
<SfSplitButton Content="Left" IconCss="e-icons e-paste" IconPosition="IconPosition.Left">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Paste"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>

<!-- Icon Right -->
<SfSplitButton Content="Right" IconCss="e-icons e-paste" IconPosition="IconPosition.Right">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Paste"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>
```

## Open/Close Events

```razor
<SfSplitButton Content="Events">
    <SplitButtonEvents 
        OnOpen="OnOpen"
        OnClose="OnClose"
        Created="OnCreated">
    </SplitButtonEvents>
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>

@code {
    private void OnOpen(BeforeOpenCloseMenuEventArgs args) => Console.WriteLine("Menu opened");
    private void OnClose(BeforeOpenCloseMenuEventArgs args) => Console.WriteLine("Menu closed");
    private void OnCreated() => Console.WriteLine("SplitButton created");
}
```

---

See also: [split-button-styling.md](split-button-styling.md) | [split-button-advanced-features.md](split-button-advanced-features.md)
