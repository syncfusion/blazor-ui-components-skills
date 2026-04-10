# ProgressButton Component - Styling

## Button Styles

```razor
<!-- Primary -->
<SfProgressButton Content="Primary" CssClass="e-primary"></SfProgressButton>

<!-- Success -->
<SfProgressButton Content="Success" CssClass="e-success"></SfProgressButton>

<!-- Info -->
<SfProgressButton Content="Info" CssClass="e-info"></SfProgressButton>

<!-- Warning -->
<SfProgressButton Content="Warning" CssClass="e-warning"></SfProgressButton>

<!-- Danger -->
<SfProgressButton Content="Danger" CssClass="e-danger"></SfProgressButton>
```

## Custom Spinner Color

```razor
<SfProgressButton Content="Custom" CssClass="custom-progress"></SfProgressButton>

<style>
    .custom-progress .e-spinner-pane .e-path {
        stroke: #ff6b6b;
    }
</style>
```

## Size Variations

```razor
<!-- Small -->
<SfProgressButton Content="Small" CssClass="e-small"></SfProgressButton>

<!-- Medium (Default) -->
<SfProgressButton Content="Medium"></SfProgressButton>

<!-- Large -->
<SfProgressButton Content="Large" CssClass="e-large"></SfProgressButton>
```

## Custom Button Style

```razor
<SfProgressButton Content="Custom" CssClass="gradient-progress"></SfProgressButton>

<style>
    .gradient-progress {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        border: none;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }
</style>
```

## Rounded Corners

```razor
<SfProgressButton Content="Rounded" CssClass="e-round"></SfProgressButton>
```

## Disabled Appearance

```razor
<SfProgressButton Content="Disabled" Disabled="true"></SfProgressButton>
```

---

See also: [progress-button-features.md](progress-button-features.md) | [progress-button-advanced-features.md](progress-button-advanced-features.md)
