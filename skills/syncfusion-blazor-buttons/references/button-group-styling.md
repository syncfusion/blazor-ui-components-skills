# Button Group Component - Styling

## Table of Contents
- [Appearance Customization](#appearance-customization)
- [Button Types and Styles](#button-types-and-styles)
- [Icons in Button Groups](#icons-in-button-groups)
- [Size Variations](#size-variations)
- [Custom CSS Classes](#custom-css-classes)
- [Theme Integration](#theme-integration)
- [Spacing and Layout](#spacing-and-layout)
- [Responsive Design](#responsive-design)

---

## Appearance Customization

### Custom Colors

```razor
<SfButtonGroup CssClass="custom-colors">
    <ButtonGroupButton>Button 1</ButtonGroupButton>
    <ButtonGroupButton>Button 2</ButtonGroupButton>
    <ButtonGroupButton>Button 3</ButtonGroupButton>
</SfButtonGroup>

<style>
    .custom-colors .e-btn {
        background-color: #667eea;
        color: white;
        border-color: #5a67d8;
    }

    .custom-colors .e-btn:hover {
        background-color: #5a67d8;
    }

    .custom-colors .e-btn.e-active {
        background-color: #4c51bf;
    }
</style>
```

### Gradient Backgrounds

```razor
<SfButtonGroup CssClass="gradient-group">
    <ButtonGroupButton>Gradient 1</ButtonGroupButton>
    <ButtonGroupButton>Gradient 2</ButtonGroupButton>
    <ButtonGroupButton>Gradient 3</ButtonGroupButton>
</SfButtonGroup>

<style>
    .gradient-group .e-btn {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        border: none;
    }

    .gradient-group .e-btn:hover {
        background: linear-gradient(135deg, #764ba2 0%, #667eea 100%);
    }
</style>
```

### Rounded Corners

```razor
<SfButtonGroup CssClass="rounded-group">
    <ButtonGroupButton>First</ButtonGroupButton>
    <ButtonGroupButton>Middle</ButtonGroupButton>
    <ButtonGroupButton>Last</ButtonGroupButton>
</SfButtonGroup>

<style>
    .rounded-group .e-btn:first-child {
        border-top-left-radius: 20px;
        border-bottom-left-radius: 20px;
    }

    .rounded-group .e-btn:last-child {
        border-top-right-radius: 20px;
        border-bottom-right-radius: 20px;
    }
</style>
```

---

## Button Types and Styles

### Primary Style Group

```razor
<SfButtonGroup>
    <ButtonGroupButton CssClass="e-primary">Primary 1</ButtonGroupButton>
    <ButtonGroupButton CssClass="e-primary">Primary 2</ButtonGroupButton>
    <ButtonGroupButton CssClass="e-primary">Primary 3</ButtonGroupButton>
</SfButtonGroup>
```

### Outline Style Group

```razor
<SfButtonGroup>
    <ButtonGroupButton CssClass="e-outline">Outline 1</ButtonGroupButton>
    <ButtonGroupButton CssClass="e-outline">Outline 2</ButtonGroupButton>
    <ButtonGroupButton CssClass="e-outline">Outline 3</ButtonGroupButton>
</SfButtonGroup>
```

### Flat Style Group

```razor
<SfButtonGroup>
    <ButtonGroupButton CssClass="e-flat">Flat 1</ButtonGroupButton>
    <ButtonGroupButton CssClass="e-flat">Flat 2</ButtonGroupButton>
    <ButtonGroupButton CssClass="e-flat">Flat 3</ButtonGroupButton>
</SfButtonGroup>
```

### Mixed Styles

```razor
<SfButtonGroup>
    <ButtonGroupButton CssClass="e-primary">Primary</ButtonGroupButton>
    <ButtonGroupButton CssClass="e-success">Success</ButtonGroupButton>
    <ButtonGroupButton CssClass="e-danger">Danger</ButtonGroupButton>
</SfButtonGroup>
```

---

## Icons in Button Groups

### Icon Only Buttons

```razor
<SfButtonGroup>
    <ButtonGroupButton IconCss="e-icons e-bold"></ButtonGroupButton>
    <ButtonGroupButton IconCss="e-icons e-italic"></ButtonGroupButton>
    <ButtonGroupButton IconCss="e-icons e-underline"></ButtonGroupButton>
</SfButtonGroup>

<style>
    .e-bold::before { content: '\e736'; }
    .e-italic::before { content: '\e737'; }
    .e-underline::before { content: '\e738'; }
</style>
```

### Icon with Text

```razor
<SfButtonGroup>
    <ButtonGroupButton IconCss="e-icons e-cut">Cut</ButtonGroupButton>
    <ButtonGroupButton IconCss="e-icons e-copy">Copy</ButtonGroupButton>
    <ButtonGroupButton IconCss="e-icons e-paste">Paste</ButtonGroupButton>
</SfButtonGroup>

<style>
    .e-cut::before { content: '\e73f'; }
    .e-copy::before { content: '\e721'; }
    .e-paste::before { content: '\e739'; }
</style>
```

### Custom Icon Styling

```razor
<SfButtonGroup CssClass="custom-icons">
    <ButtonGroupButton IconCss="e-icons e-icon-1">Action 1</ButtonGroupButton>
    <ButtonGroupButton IconCss="e-icons e-icon-2">Action 2</ButtonGroupButton>
</SfButtonGroup>

<style>
    .custom-icons .e-icon-1 {
        color: #3498db;
        font-size: 18px;
    }

    .custom-icons .e-icon-2 {
        color: #e74c3c;
        font-size: 18px;
    }

    .e-icon-1::before { content: '\e823'; }
    .e-icon-2::before { content: '\e7fc'; }
</style>
```

---

## Size Variations

### Small Button Group

```razor
<SfButtonGroup CssClass="e-small">
    <ButtonGroupButton>Small 1</ButtonGroupButton>
    <ButtonGroupButton>Small 2</ButtonGroupButton>
    <ButtonGroupButton>Small 3</ButtonGroupButton>
</SfButtonGroup>
```

### Normal Button Group (Default)

```razor
<SfButtonGroup>
    <ButtonGroupButton>Normal 1</ButtonGroupButton>
    <ButtonGroupButton>Normal 2</ButtonGroupButton>
    <ButtonGroupButton>Normal 3</ButtonGroupButton>
</SfButtonGroup>
```

### Large Custom Size

```razor
<SfButtonGroup CssClass="large-group">
    <ButtonGroupButton>Large 1</ButtonGroupButton>
    <ButtonGroupButton>Large 2</ButtonGroupButton>
    <ButtonGroupButton>Large 3</ButtonGroupButton>
</SfButtonGroup>

<style>
    .large-group .e-btn {
        padding: 12px 24px;
        font-size: 18px;
        min-height: 48px;
    }
</style>
```

---

## Custom CSS Classes

### Themed Button Groups

```razor
<SfButtonGroup CssClass="theme-primary">
    <ButtonGroupButton>Theme 1</ButtonGroupButton>
    <ButtonGroupButton>Theme 2</ButtonGroupButton>
    <ButtonGroupButton>Theme 3</ButtonGroupButton>
</SfButtonGroup>

<style>
    .theme-primary .e-btn {
        background-color: #1e3a8a;
        color: white;
        border-color: #1e40af;
    }

    .theme-primary .e-btn:hover {
        background-color: #1e40af;
    }

    .theme-primary .e-btn.e-active {
        background-color: #1d4ed8;
        box-shadow: inset 0 2px 4px rgba(0,0,0,0.2);
    }
</style>
```

### Segmented Control Style

```razor
<SfButtonGroup CssClass="segmented-control" Mode="SelectionMode.Single">
    <ButtonGroupButton @onclick="() => selected = 1"
                       Selected="@(selected == 1)">
        Daily
    </ButtonGroupButton>
    <ButtonGroupButton @onclick="() => selected = 2"
                       Selected="@(selected == 2)">
        Weekly
    </ButtonGroupButton>
    <ButtonGroupButton @onclick="() => selected = 3"
                       Selected="@(selected == 3)">
        Monthly
    </ButtonGroupButton>
</SfButtonGroup>

@code {
    private int selected = 1;
}

<style>
    .segmented-control {
        background: #e5e7eb;
        padding: 4px;
        border-radius: 8px;
    }

    .segmented-control .e-btn {
        background: transparent;
        border: none;
        color: #4b5563;
        transition: all 0.2s ease;
    }

    .segmented-control .e-btn.e-active {
        background: white;
        color: #1f2937;
        box-shadow: 0 1px 3px rgba(0,0,0,0.1);
    }
</style>
```

### Pill Style

```razor
<SfButtonGroup CssClass="pill-group">
    <ButtonGroupButton>Pill 1</ButtonGroupButton>
    <ButtonGroupButton>Pill 2</ButtonGroupButton>
    <ButtonGroupButton>Pill 3</ButtonGroupButton>
</SfButtonGroup>

<style>
    .pill-group .e-btn:first-child {
        border-radius: 50px 0 0 50px;
    }

    .pill-group .e-btn:last-child {
        border-radius: 0 50px 50px 0;
    }
</style>
```

---

## Theme Integration

### Bootstrap Theme

```razor
<SfButtonGroup CssClass="bootstrap-style">
    <ButtonGroupButton>Bootstrap 1</ButtonGroupButton>
    <ButtonGroupButton>Bootstrap 2</ButtonGroupButton>
    <ButtonGroupButton>Bootstrap 3</ButtonGroupButton>
</SfButtonGroup>

<style>
    .bootstrap-style .e-btn {
        background-color: #0d6efd;
        border-color: #0d6efd;
        color: white;
    }

    .bootstrap-style .e-btn:hover {
        background-color: #0b5ed7;
        border-color: #0a58ca;
    }
</style>
```

### Material Theme

```razor
<SfButtonGroup CssClass="material-style">
    <ButtonGroupButton>Material 1</ButtonGroupButton>
    <ButtonGroupButton>Material 2</ButtonGroupButton>
    <ButtonGroupButton>Material 3</ButtonGroupButton>
</SfButtonGroup>

<style>
    .material-style .e-btn {
        background-color: #6200ea;
        color: white;
        border-radius: 4px;
        text-transform: uppercase;
        letter-spacing: 0.5px;
    }

    .material-style .e-btn:hover {
        box-shadow: 0 2px 4px rgba(98, 0, 234, 0.4);
    }
</style>
```

---

## Spacing and Layout

### Gap Between Buttons

```razor
<SfButtonGroup CssClass="spaced-group">
    <ButtonGroupButton>Button 1</ButtonGroupButton>
    <ButtonGroupButton>Button 2</ButtonGroupButton>
    <ButtonGroupButton>Button 3</ButtonGroupButton>
</SfButtonGroup>

<style>
    .spaced-group .e-btn {
        margin-right: 5px;
    }

    .spaced-group .e-btn:last-child {
        margin-right: 0;
    }
</style>
```

### Vertical Spacing

```razor
<SfButtonGroup Orientation="Orientation.Vertical" CssClass="vertical-spaced">
    <ButtonGroupButton>Top</ButtonGroupButton>
    <ButtonGroupButton>Middle</ButtonGroupButton>
    <ButtonGroupButton>Bottom</ButtonGroupButton>
</SfButtonGroup>

<style>
    .vertical-spaced .e-btn {
        margin-bottom: 5px;
    }

    .vertical-spaced .e-btn:last-child {
        margin-bottom: 0;
    }
</style>
```

### Full Width

```razor
<SfButtonGroup CssClass="full-width">
    <ButtonGroupButton>33.3%</ButtonGroupButton>
    <ButtonGroupButton>33.3%</ButtonGroupButton>
    <ButtonGroupButton>33.3%</ButtonGroupButton>
</SfButtonGroup>

<style>
    .full-width {
        width: 100%;
        display: flex;
    }

    .full-width .e-btn {
        flex: 1;
    }
</style>
```

---

## Responsive Design

### Mobile-First Approach

```razor
<SfButtonGroup CssClass="responsive-group">
    <ButtonGroupButton>Button 1</ButtonGroupButton>
    <ButtonGroupButton>Button 2</ButtonGroupButton>
    <ButtonGroupButton>Button 3</ButtonGroupButton>
</SfButtonGroup>

<style>
    /* Mobile: Vertical */
    .responsive-group {
        flex-direction: column;
    }

    /* Tablet and up: Horizontal */
    @media (min-width: 768px) {
        .responsive-group {
            flex-direction: row;
        }
    }
</style>
```

### Adaptive Button Text

```razor
<SfButtonGroup>
    <ButtonGroupButton>
        <span class="mobile-text">B</span>
        <span class="desktop-text">Bold</span>
    </ButtonGroupButton>
    <ButtonGroupButton>
        <span class="mobile-text">I</span>
        <span class="desktop-text">Italic</span>
    </ButtonGroupButton>
</SfButtonGroup>

<style>
    .desktop-text { display: none; }
    .mobile-text { display: inline; }

    @media (min-width: 768px) {
        .desktop-text { display: inline; }
        .mobile-text { display: none; }
    }
</style>
```

### Touch-Friendly Sizes

```razor
<SfButtonGroup CssClass="touch-friendly">
    <ButtonGroupButton>Touch 1</ButtonGroupButton>
    <ButtonGroupButton>Touch 2</ButtonGroupButton>
    <ButtonGroupButton>Touch 3</ButtonGroupButton>
</SfButtonGroup>

<style>
    .touch-friendly .e-btn {
        min-height: 44px;
        min-width: 44px;
        padding: 12px 16px;
    }

    @media (hover: hover) and (pointer: fine) {
        .touch-friendly .e-btn {
            min-height: 36px;
            padding: 8px 12px;
        }
    }
</style>
```

---

## Best Practices

1. **Maintain visual hierarchy** - Use styles to indicate importance
2. **Ensure consistent spacing** - Keep gaps uniform
3. **Test on mobile devices** - Verify touch targets are adequate
4. **Use semantic colors** - Colors should convey meaning
5. **Provide clear selected state** - Make active buttons obvious
6. **Consider color contrast** - Ensure readability (WCAG AA)
7. **Use animations sparingly** - Smooth transitions without distraction
8. **Test with themes** - Verify appearance across different themes

---

## Next Steps

- Review [button-group-features.md](button-group-features.md) for functionality
- Explore [button-group-advanced-features.md](button-group-advanced-features.md) for accessibility
