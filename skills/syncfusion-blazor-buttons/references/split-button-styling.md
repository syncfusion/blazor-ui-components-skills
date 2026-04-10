# SplitButton Component - Styling

## Button Styles

```razor
<!-- Primary -->
<SfSplitButton Content="Primary" CssClass="e-primary">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>

<!-- Success -->
<SfSplitButton Content="Success" CssClass="e-success">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>

<!-- Info -->
<SfSplitButton Content="Info" CssClass="e-info">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>

<!-- Warning -->
<SfSplitButton Content="Warning" CssClass="e-warning">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>

<!-- Danger -->
<SfSplitButton Content="Danger" CssClass="e-danger">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>
```

## Size Variations

```razor
<!-- Small -->
<SfSplitButton Content="Small" CssClass="e-small">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>

<!-- Medium (Default) -->
<SfSplitButton Content="Medium">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>

<!-- Large -->
<SfSplitButton Content="Large" CssClass="e-large">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>
```

## Rounded Corners

```razor
<SfSplitButton Content="Rounded" CssClass="e-round">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>
```

## Custom Colors

```razor
<SfSplitButton Content="Custom" CssClass="custom-split">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Item 1"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>

<style>
    .custom-split {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        border: none;
    }
</style>
```

## Icon Styling

```razor
<SfSplitButton Content="Actions" IconCss="e-icons e-paste" CssClass="icon-styled">
    <DropDownMenuItems>
        <DropDownMenuItem Text="Paste"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>

<style>
    .icon-styled .e-btn-icon {
        color: #ff6b6b;
        font-size: 18px;
    }
</style>
```

## RTL Support

```razor
<SfSplitButton Content="RTL Menu" EnableRtl="true">
    <DropDownMenuItems>
        <DropDownMenuItem Text="عنصر 1"></DropDownMenuItem>
        <DropDownMenuItem Text="عنصر 2"></DropDownMenuItem>
    </DropDownMenuItems>
</SfSplitButton>
```

---

See also: [split-button-features.md](split-button-features.md) | [split-button-advanced-features.md](split-button-advanced-features.md)
