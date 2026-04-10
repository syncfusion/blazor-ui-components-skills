# SpeedDial Component - Styling

## Button Styles

```razor
<!-- Primary -->
<SfSpeedDial CssClass="e-primary">
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-edit"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>

<!-- Success -->
<SfSpeedDial CssClass="e-success">
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-check"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>
```

## Item Styles

```razor
<SfSpeedDial>
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-edit" CssClass="edit-item"></SpeedDialItem>
        <SpeedDialItem IconCss="e-icons e-delete" CssClass="delete-item"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>

<style>
    .edit-item { background-color: #4caf50; }
    .delete-item { background-color: #f44336; }
</style>
```

## Custom Colors

```razor
<SfSpeedDial CssClass="custom-speeddial">
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-edit"></SpeedDialItem>
        <SpeedDialItem IconCss="e-icons e-delete"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>

<style>
    .custom-speeddial {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    }
    
    .custom-speeddial .e-speeddial-li {
        background-color: #764ba2;
    }
</style>
```

## Animation Effects

```razor
<SfSpeedDial CssClass="animated-speeddial">
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-edit"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>

<style>
    .animated-speeddial .e-speeddial-li {
        transition: all 0.3s ease;
    }
    
    .animated-speeddial .e-speeddial-li:hover {
        transform: scale(1.1);
    }
</style>
```

## Size Customization

```razor
<SfSpeedDial CssClass="large-speeddial">
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-edit"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>

<style>
    .large-speeddial {
        width: 70px;
        height: 70px;
        font-size: 24px;
    }
    
    .large-speeddial .e-speeddial-li {
        width: 50px;
        height: 50px;
    }
</style>
```

## RTL Support

```razor
<SfSpeedDial EnableRtl="true">
    <SpeedDialItems>
        <SpeedDialItem IconCss="e-icons e-edit" Text="تعديل"></SpeedDialItem>
        <SpeedDialItem IconCss="e-icons e-delete" Text="حذف"></SpeedDialItem>
    </SpeedDialItems>
</SfSpeedDial>
```

---

See also: [speeddial-features.md](speeddial-features.md) | [speeddial-advanced-features.md](speeddial-advanced-features.md)
