# Chip Component - Styling

## Table of Contents
- [Color Customization](#color-customization)
- [Size Variations](#size-variations)
- [CSS Classes](#css-classes)
- [Icon and Avatar Styling](#icon-and-avatar-styling)
- [Theme Integration](#theme-integration)
- [Responsive Design](#responsive-design)

---

## Color Customization

### Primary Color

```razor
<SfChip>
    <ChipItems>
        <ChipItem Text="Primary" CssClass="e-primary"></ChipItem>
        <ChipItem Text="Primary 2" CssClass="e-primary"></ChipItem>
    </ChipItems>
</SfChip>
```

### Success Color

```razor
<SfChip>
    <ChipItems>
        <ChipItem Text="Success" CssClass="e-success"></ChipItem>
        <ChipItem Text="Success 2" CssClass="e-success"></ChipItem>
    </ChipItems>
</SfChip>
```

### Info Color

```razor
<SfChip>
    <ChipItems>
        <ChipItem Text="Info" CssClass="e-info"></ChipItem>
        <ChipItem Text="Info 2" CssClass="e-info"></ChipItem>
    </ChipItems>
</SfChip>
```

### Warning Color

```razor
<SfChip>
    <ChipItems>
        <ChipItem Text="Warning" CssClass="e-warning"></ChipItem>
        <ChipItem Text="Warning 2" CssClass="e-warning"></ChipItem>
    </ChipItems>
</SfChip>
```

### Danger Color

```razor
<SfChip>
    <ChipItems>
        <ChipItem Text="Danger" CssClass="e-danger"></ChipItem>
        <ChipItem Text="Danger 2" CssClass="e-danger"></ChipItem>
    </ChipItems>
</SfChip>
```

### Custom Colors

```razor
<SfChip CssClass="custom-chip-colors">
    <ChipItems>
        <ChipItem Text="Purple" CssClass="purple-chip"></ChipItem>
        <ChipItem Text="Pink" CssClass="pink-chip"></ChipItem>
        <ChipItem Text="Teal" CssClass="teal-chip"></ChipItem>
    </ChipItems>
</SfChip>

<style>
    .purple-chip {
        background-color: #9c27b0;
        color: white;
    }
    
    .pink-chip {
        background-color: #e91e63;
        color: white;
    }
    
    .teal-chip {
        background-color: #009688;
        color: white;
    }
</style>
```

---

## Size Variations

### Small Chips

```razor
<SfChip CssClass="e-small">
    <ChipItems>
        <ChipItem Text="Small Chip 1"></ChipItem>
        <ChipItem Text="Small Chip 2"></ChipItem>
    </ChipItems>
</SfChip>

<style>
    .e-small .e-chip {
        font-size: 11px;
        height: 24px;
    }
</style>
```

### Medium Chips (Default)

```razor
<SfChip>
    <ChipItems>
        <ChipItem Text="Medium Chip 1"></ChipItem>
        <ChipItem Text="Medium Chip 2"></ChipItem>
    </ChipItems>
</SfChip>
```

### Large Chips

```razor
<SfChip CssClass="e-large">
    <ChipItems>
        <ChipItem Text="Large Chip 1"></ChipItem>
        <ChipItem Text="Large Chip 2"></ChipItem>
    </ChipItems>
</SfChip>

<style>
    .e-large .e-chip {
        font-size: 16px;
        height: 40px;
        padding: 0 16px;
    }
</style>
```

---

## CSS Classes

### Outline Style

```razor
<SfChip>
    <ChipItems>
        <ChipItem Text="Outlined" CssClass="e-outline"></ChipItem>
        <ChipItem Text="Outlined 2" CssClass="e-outline"></ChipItem>
    </ChipItems>
</SfChip>
```

### Outline with Colors

```razor
<SfChip>
    <ChipItems>
        <ChipItem Text="Primary Outline" CssClass="e-outline e-primary"></ChipItem>
    </ChipItems>
</SfChip>

<SfChip>
    <ChipItems>
        <ChipItem Text="Success Outline" CssClass="e-outline e-success"></ChipItem>
    </ChipItems>
</SfChip>
```

### Custom Chip Styling

```razor
<SfChip CssClass="custom-chips">
    <ChipItems>
        <ChipItem Text="Chip 1"></ChipItem>
        <ChipItem Text="Chip 2"></ChipItem>
        <ChipItem Text="Chip 3"></ChipItem>
    </ChipItems>
</SfChip>

<style>
    .custom-chips .e-chip {
        border-radius: 20px;
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        margin: 4px;
        padding: 0 16px;
        font-weight: 500;
    }
</style>
```

### Rounded Chips

```razor
<SfChip CssClass="rounded-chips">
    <ChipItems>
        <ChipItem Text="Fully Rounded"></ChipItem>
    </ChipItems>
</SfChip>

<style>
    .rounded-chips .e-chip {
        border-radius: 50px;
    }
</style>
```

### Square Chips

```razor
<SfChip CssClass="square-chips">
    <ChipItems>
        <ChipItem Text="Square"></ChipItem>
    </ChipItems>
</SfChip>

<style>
    .square-chips .e-chip {
        border-radius: 4px;
    }
</style>
```

---

## Icon and Avatar Styling

### Icon Color

```razor
<SfChip CssClass="colored-icons">
    <ChipItems>
        <ChipItem Text="Home" LeadingIconCss="e-icons e-home"></ChipItem>
        <ChipItem Text="Settings" LeadingIconCss="e-icons e-settings"></ChipItem>
    </ChipItems>
</SfChip>

<style>
    .colored-icons .e-chip-icon {
        color: #ff6b6b;
    }
</style>
```

### Icon Size

```razor
<SfChip CssClass="large-icons">
    <ChipItems>
        <ChipItem Text="Star" LeadingIconCss="e-icons e-star"></ChipItem>
    </ChipItems>
</SfChip>

<style>
    .large-icons .e-chip-icon {
        font-size: 18px;
    }
</style>
```

---

## Theme Integration

### Bootstrap Theme

```razor
<SfChip CssClass="bootstrap-chip">
    <ChipItems>
        <ChipItem Text="Bootstrap"></ChipItem>
    </ChipItems>
</SfChip>
```

### Material Theme

```razor
<SfChip CssClass="material-chip">
    <ChipItems>
        <ChipItem Text="Material"></ChipItem>
    </ChipItems>
</SfChip>

<style>
    .material-chip .e-chip {
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.2);
        border-radius: 16px;
    }
</style>
```

### Tailwind-Inspired

```razor
<SfChip CssClass="tailwind-chip">
    <ChipItems>
        <ChipItem Text="Tailwind"></ChipItem>
    </ChipItems>
</SfChip>

<style>
    .tailwind-chip .e-chip {
        background-color: #3b82f6;
        color: white;
        border-radius: 0.375rem;
        padding: 0.25rem 0.75rem;
        font-size: 0.875rem;
    }
</style>
```

---

## Responsive Design

### Mobile-Friendly Chips

```razor
<SfChip CssClass="responsive-chips">
    <ChipItems>
        <ChipItem Text="Responsive 1"></ChipItem>
        <ChipItem Text="Responsive 2"></ChipItem>
        <ChipItem Text="Responsive 3"></ChipItem>
    </ChipItems>
</SfChip>

<style>
    .responsive-chips .e-chip {
        margin: 4px;
        padding: 8px 12px;
    }
    
    @media (max-width: 768px) {
        .responsive-chips .e-chip {
            font-size: 14px;
            padding: 10px 14px;
            height: auto;
        }
    }
</style>
```

### Wrap Chips

```razor
<div class="chip-container">
    <SfChip>
        <ChipItems>
            <ChipItem Text="Chip 1"></ChipItem>
            <ChipItem Text="Chip 2"></ChipItem>
            <ChipItem Text="Chip 3"></ChipItem>
            <ChipItem Text="Chip 4"></ChipItem>
            <ChipItem Text="Chip 5"></ChipItem>
        </ChipItems>
    </SfChip>
</div>

<style>
    .chip-container .e-chip-list {
        display: flex;
        flex-wrap: wrap;
        gap: 8px;
    }
</style>
```

---

## Custom Styling Examples

### Glassmorphism Chips

```razor
<SfChip CssClass="glass-chips">
    <ChipItems>
        <ChipItem Text="Glass Effect"></ChipItem>
        <ChipItem Text="Transparent"></ChipItem>
    </ChipItems>
</SfChip>

<style>
    .glass-chips .e-chip {
        background: rgba(255, 255, 255, 0.2);
        backdrop-filter: blur(10px);
        border: 1px solid rgba(255, 255, 255, 0.3);
        color: #333;
        box-shadow: 0 8px 32px rgba(0, 0, 0, 0.1);
    }
</style>
```

### Animated Hover

```razor
<SfChip CssClass="animated-chips">
    <ChipItems>
        <ChipItem Text="Hover Me"></ChipItem>
    </ChipItems>
</SfChip>

<style>
    .animated-chips .e-chip {
        transition: all 0.3s ease;
    }
    
    .animated-chips .e-chip:hover {
        transform: translateY(-2px);
        box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
    }
</style>
```

### Gradient Chips

```razor
<SfChip CssClass="gradient-chips">
    <ChipItems>
        <ChipItem Text="Gradient 1" CssClass="gradient-1"></ChipItem>
        <ChipItem Text="Gradient 2" CssClass="gradient-2"></ChipItem>
        <ChipItem Text="Gradient 3" CssClass="gradient-3"></ChipItem>
    </ChipItems>
</SfChip>

<style>
    .gradient-chips .e-chip {
        color: white;
        border: none;
    }
    
    .gradient-1 {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    }
    
    .gradient-2 {
        background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
    }
    
    .gradient-3 {
        background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
    }
</style>
```

---

## Best Practices

1. **Use consistent colors** - Stick to your application's color palette
2. **Maintain readability** - Ensure sufficient contrast between text and background
3. **Consider touch targets** - Make chips at least 32x32px for mobile
4. **Use semantic colors** - Green for success, red for errors, etc.
5. **Test responsiveness** - Verify chip appearance on all screen sizes
6. **Optimize performance** - Avoid excessive shadows and animations

---

## Next Steps

- Return to [chip-features.md](chip-features.md) for core functionality
- Learn about [chip-advanced-features.md](chip-advanced-features.md) for accessibility
