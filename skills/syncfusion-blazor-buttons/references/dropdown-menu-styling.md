# DropdownMenu Component - Styling

## Button Styles

```razor
<SfDropDownButton Content="Primary" CssClass="e-primary">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfDropDownButton>

<SfDropDownButton Content="Success" CssClass="e-success">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfDropDownButton>
```

## Icon Customization

```razor
<SfDropDownButton Content="Custom Icon" IconCss="e-icons e-chevron-down" IconPosition="IconPosition.Right">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfDropDownButton>
```

## Hide Caret Icon

```razor
<SfDropDownButton Content="No Icon" IconCss="">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfDropDownButton>
```

## Custom Popup Width

```razor
<SfDropDownButton Content="Wide Menu" CssClass="custom-popup">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfDropDownButton>

<style>
    .custom-popup .e-dropdown-popup {
        min-width: 300px;
    }
</style>
```

## Rounded Corners

```razor
<SfDropDownButton Content="Rounded" CssClass="e-round">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfDropDownButton>
```

## RTL Support

```razor
<SfDropDownButton Content="RTL Menu" EnableRtl="true">
    <DropDownMenuItems>
        <DropDownMenuItem Text="عنصر 1"></DropDownMenuItem>
        <DropDownMenuItem Text="عنصر 2"></DropDownMenuItem>
    </DropDownMenuItems>
</SfDropDownButton>
```

## Custom Colors

```razor
<SfDropDownButton Content="Custom" CssClass="custom-dropdown">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfDropDownButton>

<style>
    .custom-dropdown {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        border: none;
    }
</style>
```

## Item Styling

```razor
<SfDropDownButton Content="Styled Items" CssClass="styled-items">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1" CssClass="item-success"></DropDownMenuItem>
        <DropDownMenuItem Text="Item 2" CssClass="item-danger"></DropDownMenuItem>
    </DropDownMenuItems>
</SfDropDownButton>

<style>
    .item-success { color: #28a745; }
    .item-danger { color: #dc3545; }
</style>
```

---

See also: [dropdown-menu-features.md](dropdown-menu-features.md) | [dropdown-menu-advanced-features.md](dropdown-menu-advanced-features.md)
