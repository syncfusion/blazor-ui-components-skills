# Button Component - Styling

## Table of Contents
- [CSS Class Customization](#css-class-customization)
- [Appearance Customization](#appearance-customization)
- [Block Buttons](#block-buttons)
- [Rounded Corners](#rounded-corners)
- [RTL Support](#rtl-support)
- [Theme Integration](#theme-integration)
- [Color Schemes](#color-schemes)
- [Responsive Design](#responsive-design)
- [Custom Styling Examples](#custom-styling-examples)

---

## CSS Class Customization

### Using CssClass Property

The `CssClass` property allows you to apply custom CSS classes to buttons.

```razor
<SfButton CssClass="custom-button e-primary">Custom Styled Button</SfButton>

<style>
    .custom-button.e-btn {
        border-radius: 20px;
        padding: 10px 30px;
        font-weight: bold;
    }
</style>
```

### Multiple CSS Classes

```razor
<SfButton CssClass="custom-primary large-button shadow">
    Multi-Class Button
</SfButton>

<style>
    .custom-primary.e-btn {
        background: linear-gradient(45deg, #667eea 0%, #764ba2 100%);
        color: white;
        border: none;
    }

    .large-button.e-btn {
        padding: 15px 40px;
        font-size: 18px;
    }

    .shadow.e-btn {
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }
</style>
```

### Scoped Styles with ::deep

For component-scoped styles that penetrate Syncfusion components:

```razor
<SfButton CssClass="scoped-custom">Scoped Button</SfButton>

<style>
    ::deep .scoped-custom.e-btn {
        background-color: #ff6b6b;
        color: white;
    }

    ::deep .scoped-custom.e-btn:hover {
        background-color: #ff5252;
    }
</style>
```

---

## Appearance Customization

### Custom Background and Text Colors

```razor
<SfButton CssClass="color-custom">Custom Colors</SfButton>

<style>
    .color-custom.e-btn {
        background-color: #8e44ad;
        color: #ffffff;
        border: 2px solid #9b59b6;
    }

    .color-custom.e-btn:hover {
        background-color: #9b59b6;
        border-color: #8e44ad;
    }

    .color-custom.e-btn:active {
        background-color: #7d3c98;
    }
</style>
```

### Gradient Backgrounds

```razor
<SfButton CssClass="gradient-btn">Gradient Button</SfButton>

<style>
    .gradient-btn.e-btn {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        border: none;
    }

    .gradient-btn.e-btn:hover {
        background: linear-gradient(135deg, #764ba2 0%, #667eea 100%);
    }
</style>
```

### Box Shadow and Elevation

```razor
<SfButton CssClass="elevated-btn e-primary">Elevated Button</SfButton>

<style>
    .elevated-btn.e-btn {
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        transition: box-shadow 0.3s ease;
    }

    .elevated-btn.e-btn:hover {
        box-shadow: 0 4px 8px rgba(0,0,0,0.15);
    }

    .elevated-btn.e-btn:active {
        box-shadow: 0 1px 2px rgba(0,0,0,0.1);
    }
</style>
```

### Border Customization

```razor
<SfButton CssClass="custom-border">Custom Border</SfButton>

<style>
    .custom-border.e-btn {
        border: 3px solid #3498db;
        border-style: dashed;
        border-radius: 10px;
        background: transparent;
        color: #3498db;
    }

    .custom-border.e-btn:hover {
        background: #3498db;
        color: white;
    }
</style>
```

---

## Block Buttons

Block buttons span the full width of their container.

### Basic Block Button

```razor
<SfButton CssClass="e-block e-primary">Full Width Button</SfButton>
```

### Custom Block Styling

```razor
<div class="button-container">
    <SfButton CssClass="e-block e-primary custom-block">
        Primary Block
    </SfButton>
    <SfButton CssClass="e-block e-success custom-block">
        Success Block
    </SfButton>
</div>

<style>
    .button-container {
        width: 100%;
        display: flex;
        flex-direction: column;
        gap: 10px;
    }

    .custom-block.e-btn {
        padding: 15px;
        font-size: 16px;
        font-weight: 600;
    }
</style>
```

### Responsive Block Buttons

```razor
<SfButton CssClass="responsive-block e-primary">
    Responsive Button
</SfButton>

<style>
    .responsive-block.e-btn {
        width: auto;
    }

    @media (max-width: 768px) {
        .responsive-block.e-btn {
            width: 100%;
            display: block;
        }
    }
</style>
```

---

## Rounded Corners

### Default Rounded Corners

```razor
<SfButton CssClass="rounded-default e-primary">Rounded Button</SfButton>

<style>
    .rounded-default.e-btn {
        border-radius: 20px;
    }
</style>
```

### Fully Rounded (Pill Shape)

```razor
<SfButton CssClass="pill-button e-primary">Pill Button</SfButton>

<style>
    .pill-button.e-btn {
        border-radius: 50px;
        padding: 10px 30px;
    }
</style>
```

### Sharp Corners

```razor
<SfButton CssClass="sharp-corners e-primary">Sharp Button</SfButton>

<style>
    .sharp-corners.e-btn {
        border-radius: 0;
    }
</style>
```

### Asymmetric Rounding

```razor
<SfButton CssClass="asymmetric-round e-primary">Asymmetric</SfButton>

<style>
    .asymmetric-round.e-btn {
        border-radius: 20px 0 20px 0;
    }
</style>
```

---

## RTL Support

### Enable RTL Mode

```razor
<SfButton EnableRtl="true" IconCss="e-icons e-arrow-icon">
    RTL Button
</SfButton>

<style>
    .e-arrow-icon::before {
        content: '\e71a';
    }
</style>
```

### RTL with Icon Positioning

```razor
<div dir="rtl">
    <SfButton EnableRtl="true" 
              IconCss="e-icons e-save-icon" 
              IconPosition="IconPosition.Right">
        حفظ (Save)
    </SfButton>
</div>

<style>
    .e-save-icon::before {
        content: '\e74e';
    }
</style>
```

### RTL Layout Example

```razor
<div dir="rtl" style="display: flex; gap: 10px;">
    <SfButton EnableRtl="true" CssClass="e-primary">أساسي</SfButton>
    <SfButton EnableRtl="true" CssClass="e-success">نجاح</SfButton>
    <SfButton EnableRtl="true" CssClass="e-danger">خطر</SfButton>
</div>
```

---

## Theme Integration

### Available Themes

Syncfusion Blazor supports multiple themes:

```html
<!-- Bootstrap 5 -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Material -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- Fabric -->
<link href="_content/Syncfusion.Blazor.Themes/fabric.css" rel="stylesheet" />

<!-- Tailwind -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />

<!-- Fluent -->
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />
```

### Theme Variants

```html
<!-- Dark themes -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5-dark.css" rel="stylesheet" />
<link href="_content/Syncfusion.Blazor.Themes/material-dark.css" rel="stylesheet" />
<link href="_content/Syncfusion.Blazor.Themes/fabric-dark.css" rel="stylesheet" />
```

### Custom Theme via CSS Variables

```razor
<SfButton CssClass="theme-custom e-primary">Themed Button</SfButton>

<style>
    :root {
        --button-primary-bg: #667eea;
        --button-primary-hover: #5568d3;
        --button-primary-text: #ffffff;
    }

    .theme-custom.e-btn.e-primary {
        background-color: var(--button-primary-bg);
        color: var(--button-primary-text);
    }

    .theme-custom.e-btn.e-primary:hover {
        background-color: var(--button-primary-hover);
    }
</style>
```

### Theme Studio Integration

Use Syncfusion Theme Studio to generate custom themes:
1. Visit https://blazor.syncfusion.com/themestudio/
2. Customize colors, typography, and component styles
3. Download generated CSS
4. Reference in your application

---

## Color Schemes

### Brand Colors

```razor
<div class="brand-buttons">
    <SfButton CssClass="brand-primary">Primary Brand</SfButton>
    <SfButton CssClass="brand-secondary">Secondary Brand</SfButton>
    <SfButton CssClass="brand-accent">Accent</SfButton>
</div>

<style>
    .brand-primary.e-btn {
        background-color: #1e3a8a;
        color: white;
    }

    .brand-secondary.e-btn {
        background-color: #64748b;
        color: white;
    }

    .brand-accent.e-btn {
        background-color: #f59e0b;
        color: white;
    }
</style>
```

### Semantic Colors

```razor
<div class="semantic-colors">
    <SfButton CssClass="color-info">Info</SfButton>
    <SfButton CssClass="color-warning">Warning</SfButton>
    <SfButton CssClass="color-error">Error</SfButton>
    <SfButton CssClass="color-neutral">Neutral</SfButton>
</div>

<style>
    .color-info.e-btn { background: #3b82f6; color: white; }
    .color-warning.e-btn { background: #f59e0b; color: white; }
    .color-error.e-btn { background: #ef4444; color: white; }
    .color-neutral.e-btn { background: #6b7280; color: white; }
</style>
```

### Monochrome Palette

```razor
<div class="mono-palette">
    <SfButton CssClass="mono-100">100</SfButton>
    <SfButton CssClass="mono-300">300</SfButton>
    <SfButton CssClass="mono-500">500</SfButton>
    <SfButton CssClass="mono-700">700</SfButton>
    <SfButton CssClass="mono-900">900</SfButton>
</div>

<style>
    .mono-100.e-btn { background: #f3f4f6; color: #111827; }
    .mono-300.e-btn { background: #d1d5db; color: #111827; }
    .mono-500.e-btn { background: #6b7280; color: white; }
    .mono-700.e-btn { background: #374151; color: white; }
    .mono-900.e-btn { background: #111827; color: white; }
</style>
```

---

## Responsive Design

### Mobile-First Approach

```razor
<SfButton CssClass="responsive-btn e-primary">
    Responsive Button
</SfButton>

<style>
    /* Mobile (default) */
    .responsive-btn.e-btn {
        padding: 12px 20px;
        font-size: 14px;
        width: 100%;
    }

    /* Tablet */
    @media (min-width: 768px) {
        .responsive-btn.e-btn {
            padding: 10px 24px;
            font-size: 16px;
            width: auto;
        }
    }

    /* Desktop */
    @media (min-width: 1024px) {
        .responsive-btn.e-btn {
            padding: 12px 32px;
            font-size: 18px;
        }
    }
</style>
```

### Adaptive Button Sizes

```razor
<div class="button-group">
    <SfButton CssClass="adaptive-btn e-primary">
        <span class="mobile-text">Add</span>
        <span class="desktop-text">Add New Item</span>
    </SfButton>
</div>

<style>
    .desktop-text { display: none; }
    .mobile-text { display: inline; }

    @media (min-width: 768px) {
        .desktop-text { display: inline; }
        .mobile-text { display: none; }
    }

    .adaptive-btn.e-btn {
        min-width: 60px;
    }

    @media (min-width: 768px) {
        .adaptive-btn.e-btn {
            min-width: 120px;
        }
    }
</style>
```

### Touch-Friendly Buttons

```razor
<SfButton CssClass="touch-friendly e-primary">Touch Button</SfButton>

<style>
    .touch-friendly.e-btn {
        min-height: 44px;  /* Minimum touch target */
        min-width: 44px;
        padding: 12px 20px;
    }

    @media (hover: hover) and (pointer: fine) {
        /* Desktop: reduce size */
        .touch-friendly.e-btn {
            min-height: 36px;
            padding: 8px 16px;
        }
    }
</style>
```

---

## Custom Styling Examples

### Glassmorphism Button

```razor
<SfButton CssClass="glass-btn">Glassmorphism</SfButton>

<style>
    .glass-btn.e-btn {
        background: rgba(255, 255, 255, 0.1);
        backdrop-filter: blur(10px);
        border: 1px solid rgba(255, 255, 255, 0.2);
        color: #333;
        box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.15);
    }

    .glass-btn.e-btn:hover {
        background: rgba(255, 255, 255, 0.2);
    }
</style>
```

### Neon Glow Button

```razor
<SfButton CssClass="neon-btn">Neon Glow</SfButton>

<style>
    .neon-btn.e-btn {
        background-color: #000;
        color: #0ff;
        border: 2px solid #0ff;
        text-shadow: 0 0 5px #0ff;
        box-shadow: 0 0 10px #0ff;
        transition: all 0.3s ease;
    }

    .neon-btn.e-btn:hover {
        box-shadow: 0 0 20px #0ff, 0 0 40px #0ff;
        text-shadow: 0 0 10px #0ff;
    }
</style>
```

### Neumorphism Button

```razor
<SfButton CssClass="neumorphic-btn">Neumorphism</SfButton>

<style>
    .neumorphic-btn.e-btn {
        background: #e0e5ec;
        border: none;
        color: #333;
        box-shadow: 9px 9px 16px rgba(163, 177, 198, 0.6),
                    -9px -9px 16px rgba(255, 255, 255, 0.5);
    }

    .neumorphic-btn.e-btn:hover {
        box-shadow: inset 9px 9px 16px rgba(163, 177, 198, 0.6),
                    inset -9px -9px 16px rgba(255, 255, 255, 0.5);
    }

    .neumorphic-btn.e-btn:active {
        box-shadow: inset 6px 6px 12px rgba(163, 177, 198, 0.6),
                    inset -6px -6px 12px rgba(255, 255, 255, 0.5);
    }
</style>
```

### Animated Hover Effects

```razor
<SfButton CssClass="animated-hover e-primary">Hover Me</SfButton>

<style>
    .animated-hover.e-btn {
        position: relative;
        overflow: hidden;
        transition: all 0.3s ease;
    }

    .animated-hover.e-btn::before {
        content: '';
        position: absolute;
        top: 50%;
        left: 50%;
        width: 0;
        height: 0;
        border-radius: 50%;
        background: rgba(255, 255, 255, 0.3);
        transform: translate(-50%, -50%);
        transition: width 0.6s, height 0.6s;
    }

    .animated-hover.e-btn:hover::before {
        width: 300px;
        height: 300px;
    }
</style>
```

### Outlined with Hover Fill

```razor
<SfButton CssClass="outline-fill">Outline Fill</SfButton>

<style>
    .outline-fill.e-btn {
        background: transparent;
        border: 2px solid #3498db;
        color: #3498db;
        transition: all 0.3s ease;
        position: relative;
        z-index: 1;
        overflow: hidden;
    }

    .outline-fill.e-btn::before {
        content: '';
        position: absolute;
        top: 0;
        left: -100%;
        width: 100%;
        height: 100%;
        background: #3498db;
        transition: left 0.3s ease;
        z-index: -1;
    }

    .outline-fill.e-btn:hover {
        color: white;
    }

    .outline-fill.e-btn:hover::before {
        left: 0;
    }
</style>
```

### Button with Loading Spinner

```razor
<SfButton CssClass="loading-btn @(isLoading ? "loading" : "")" 
          OnClick="SimulateLoading"
          Disabled="@isLoading">
    @(isLoading ? "Loading..." : "Submit")
</SfButton>

@code {
    private bool isLoading = false;

    private async Task SimulateLoading()
    {
        isLoading = true;
        await Task.Delay(2000);
        isLoading = false;
    }
}

<style>
    .loading-btn.e-btn.loading::after {
        content: '';
        display: inline-block;
        width: 14px;
        height: 14px;
        margin-left: 8px;
        border: 2px solid rgba(255, 255, 255, 0.3);
        border-top-color: white;
        border-radius: 50%;
        animation: spin 0.6s linear infinite;
    }

    @keyframes spin {
        to { transform: rotate(360deg); }
    }
</style>
```

---

## Best Practices

1. **Maintain Consistency** - Use a consistent color palette and style across your application
2. **Consider Accessibility** - Ensure sufficient color contrast (WCAG AA: 4.5:1 minimum)
3. **Use CSS Variables** - Define reusable color and spacing variables
4. **Test Responsiveness** - Verify button appearance on different screen sizes
5. **Optimize Animations** - Use `transform` and `opacity` for performant animations
6. **Provide Hover States** - Give visual feedback for interactive elements
7. **Use Semantic Classes** - Name classes based on purpose, not appearance
8. **Consider Dark Mode** - Design buttons that work in both light and dark themes
9. **Avoid Over-Styling** - Keep designs clean and functional
10. **Test Cross-Browser** - Verify styles work in all target browsers

---

## Common Styling Issues

### Issue: Custom Colors Not Applying

**Problem:** Button keeps default theme colors

**Solution:** Increase CSS specificity or use `!important`

```css
/* Increase specificity */
::deep .custom-btn.e-btn.e-primary {
    background-color: #custom-color;
}

/* Or use !important (use sparingly) */
.custom-btn.e-btn {
    background-color: #custom-color !important;
}
```

### Issue: Hover Styles Not Working

**Problem:** Hover effects don't apply

**Solution:** Ensure proper selector hierarchy

```css
/* Correct */
::deep .custom-btn.e-btn:hover {
    background-color: #hover-color;
}

/* Also works */
:global(.custom-btn.e-btn:hover) {
    background-color: #hover-color;
}
```

### Issue: Border Radius Not Applying

**Problem:** Rounded corners not showing

**Solution:** Override Syncfusion's default styles

```css
.rounded-btn.e-btn {
    border-radius: 20px !important;
}
```

---

## Next Steps

- Explore [button-features.md](button-features.md) for component functionality
- Learn about [button-advanced-features.md](button-advanced-features.md) for accessibility
