# Floating Action Button (FAB) - Styling

## Appearance Styles

```razor
<!-- Primary -->
<SfFab IconCss="e-icons e-edit" CssClass="e-primary"></SfFab>

<!-- Success -->
<SfFab IconCss="e-icons e-check" CssClass="e-success"></SfFab>

<!-- Danger -->
<SfFab IconCss="e-icons e-delete" CssClass="e-danger"></SfFab>

<!-- Warning -->
<SfFab IconCss="e-icons e-warning" CssClass="e-warning"></SfFab>

<!-- Info -->
<SfFab IconCss="e-icons e-info" CssClass="e-info"></SfFab>
```

## Size Variations

```razor
<!-- Small -->
<SfFab IconCss="e-icons e-edit" CssClass="e-small"></SfFab>

<!-- Medium (Default) -->
<SfFab IconCss="e-icons e-edit"></SfFab>

<!-- Large -->
<SfFab IconCss="e-icons e-edit" CssClass="e-large"></SfFab>
```

## Custom Colors

```razor
<SfFab IconCss="e-icons e-edit" CssClass="custom-fab"></SfFab>

<style>
    .custom-fab {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        box-shadow: 0 8px 16px rgba(102, 126, 234, 0.4);
    }
    
    .custom-fab:hover {
        box-shadow: 0 12px 20px rgba(102, 126, 234, 0.6);
    }
</style>
```

## Icon Positioning

```razor
<SfFab Content="Create" IconCss="e-icons e-plus" IconPosition="IconPosition.Left"></SfFab>
<SfFab Content="Create" IconCss="e-icons e-plus" IconPosition="IconPosition.Right"></SfFab>
```

## Rounded FAB

```razor
<SfFab IconCss="e-icons e-edit" CssClass="e-round"></SfFab>
```

## Custom Size

```razor
<SfFab IconCss="e-icons e-edit" CssClass="custom-size-fab"></SfFab>

<style>
    .custom-size-fab {
        width: 70px;
        height: 70px;
        font-size: 24px;
    }
</style>
```

---

See also: [fab-features.md](fab-features.md) | [fab-advanced-features.md](fab-advanced-features.md)
