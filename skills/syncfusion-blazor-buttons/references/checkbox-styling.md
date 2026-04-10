# CheckBox Styling and Appearance

## Table of Contents
- [CSS Class Customization](#css-class-customization)
- [Appearance Options](#appearance-options)
- [Size Variations](#size-variations)
- [Color Customization](#color-customization)
- [RTL Support](#rtl-support)
- [Theme Integration](#theme-integration)
- [Custom Styling Examples](#custom-styling-examples)

---

## CSS Class Customization

The CheckBox component supports standard Syncfusion CSS classes for quick styling:

### Basic CSS Classes

```razor
<!-- Primary checkbox (blue) -->
<SfCheckBox TChecked="bool" Label="Primary" CssClass="e-primary"></SfCheckBox>

<!-- Success checkbox (green) -->
<SfCheckBox TChecked="bool" Label="Success" CssClass="e-success"></SfCheckBox>

<!-- Info checkbox (light blue) -->
<SfCheckBox TChecked="bool" Label="Info" CssClass="e-info"></SfCheckBox>

<!-- Warning checkbox (orange) -->
<SfCheckBox TChecked="bool" Label="Warning" CssClass="e-warning"></SfCheckBox>

<!-- Danger checkbox (red) -->
<SfCheckBox TChecked="bool" Label="Danger" CssClass="e-danger"></SfCheckBox>
```

### Multiple CSS Classes

```razor
<!-- Combine multiple classes -->
<SfCheckBox TChecked="bool" Label="Custom Style" CssClass="e-primary custom-checkbox"></SfCheckBox>

<!-- Add spacing classes -->
<SfCheckBox TChecked="bool" Label="Spaced" CssClass="e-checkbox-spacing"></SfCheckBox>
```

---

## Appearance Options

### Label Positioning

```razor
<!-- Label to the right (default) -->
<SfCheckBox TChecked="bool" Label="Label Right"></SfCheckBox>

<!-- Long label text -->
<SfCheckBox TChecked="bool" Label="I agree to the terms and conditions of service. Please read carefully before proceeding."></SfCheckBox>
```

### Rounded Checkbox Style

```razor
<!-- Create rounded appearance with CSS -->
<SfCheckBox TChecked="bool" Label="Rounded Style" CssClass="e-checkbox-rounded"></SfCheckBox>

<style>
    .e-checkbox-rounded .e-frame {
        border-radius: 4px;
    }
</style>
```

### Flat vs Outlined

```razor
<!-- Flat checkbox (default) -->
<SfCheckBox TChecked="bool" Label="Flat Checkbox"></SfCheckBox>

<!-- Outlined checkbox with custom CSS -->
<SfCheckBox TChecked="bool" Label="Outlined" CssClass="e-checkbox-outline"></SfCheckBox>

<style>
    .e-checkbox-outline .e-frame {
        border: 2px solid #ccc;
        background: transparent;
    }
</style>
```

---

## Size Variations

### Small Checkbox

```razor
<!-- Small checkbox -->
<SfCheckBox TChecked="bool" Label="Small" CssClass="e-small"></SfCheckBox>

<!-- Custom CSS for small size -->
<SfCheckBox TChecked="bool" Label="Extra Small" CssClass="e-extra-small"></SfCheckBox>

<style>
    .e-extra-small .e-frame {
        width: 16px;
        height: 16px;
    }
    
    .e-extra-small .e-label {
        font-size: 12px;
    }
</style>
```

### Large Checkbox

```razor
<!-- Large checkbox with bigger label -->
<SfCheckBox TChecked="bool" Label="Large" CssClass="e-large"></SfCheckBox>

<!-- Custom CSS for large size -->
<style>
    .e-large .e-frame {
        width: 28px;
        height: 28px;
    }
    
    .e-large .e-label {
        font-size: 16px;
    }
</style>
```

---

## Color Customization

### Using CSS Variables

```razor
<SfCheckBox TChecked="bool" Label="Custom Color" CssClass="e-custom-color"></SfCheckBox>

<style>
    .e-custom-color .e-frame {
        border-color: #ff6b6b;
    }
    
    .e-custom-color.e-check .e-frame {
        background-color: #ff6b6b;
        border-color: #ff6b6b;
    }
</style>
```

### Theme-based Colors

```razor
<!-- Bootstrap theme -->
<SfCheckBox TChecked="bool" Label="Bootstrap Primary" CssClass="e-primary"></SfCheckBox>

<!-- Material theme -->
<SfCheckBox TChecked="bool" Label="Material Primary" CssClass="e-primary"></SfCheckBox>

<!-- Tailwind theme -->
<SfCheckBox TChecked="bool" Label="Tailwind Primary" CssClass="e-primary"></SfCheckBox>
```

### Custom Color Scheme

```razor
<SfCheckBox TChecked="bool" Label="Custom Purple" CssClass="e-purple-theme"></SfCheckBox>
<SfCheckBox TChecked="bool" Label="Custom Teal" CssClass="e-teal-theme"></SfCheckBox>

<style>
    .e-purple-theme .e-frame {
        border-color: #9c27b0;
    }
    
    .e-purple-theme.e-check .e-frame {
        background-color: #9c27b0;
        border-color: #9c27b0;
    }
    
    .e-teal-theme .e-frame {
        border-color: #009688;
    }
    
    .e-teal-theme.e-check .e-frame {
        background-color: #009688;
        border-color: #009688;
    }
</style>
```

---

## RTL Support

### Enable RTL

```razor
<!-- Enable RTL for checkbox -->
<SfCheckBox TChecked="bool" Label="RTL CheckBox" EnableRtl="true"></SfCheckBox>

<!-- RTL container -->
<div dir="rtl">
    <SfCheckBox TChecked="bool" Label="עברית"></SfCheckBox>
    <SfCheckBox TChecked="bool" Label="العربية"></SfCheckBox>
</div>
```

### RTL Styling Considerations

```razor
<!-- RTL with custom spacing -->
<SfCheckBox TChecked="bool" Label="RTL with Custom Spacing" EnableRtl="true" CssClass="e-rtl-spacing"></SfCheckBox>

<style>
    /* RTL-specific styles */
    [dir="rtl"] .e-checkbox-wrapper {
        flex-direction: row-reverse;
    }
    
    [dir="rtl"] .e-label {
        margin-right: 8px;
        margin-left: 0;
    }
</style>
```

---

## Theme Integration

### Switching Themes

The CheckBox component respects the current theme applied to your application:

```razor
<!-- Bootstrap5 theme -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Material theme -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- Fabric theme -->
<link href="_content/Syncfusion.Blazor.Themes/fabric.css" rel="stylesheet" />

<!-- Tailwind theme -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />

<!-- Fluent theme -->
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />
```

### Theme-Specific Styling

```razor
<style>
    /* Bootstrap5 specific -->
    .e-bootstrap5 .e-checkbox .e-frame {
        border-radius: 3px;
    }
    
    /* Material specific -->
    .e-material .e-checkbox .e-frame {
        border-radius: 2px;
    }
    
    /* Tailwind specific */
    .e-tailwind .e-checkbox .e-frame {
        border-radius: 4px;
    }
</style>
```

---

## Custom Styling Examples

### Gradient Background Checkbox

```razor
<SfCheckBox TChecked="bool" Label="Gradient Checkbox" CssClass="e-gradient-check"></SfCheckBox>

<style>
    .e-gradient-check.e-check .e-frame {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        border-color: #667eea;
    }
</style>
```

### Shadow and Elevation

```razor
<SfCheckBox TChecked="bool" Label="Elevated Checkbox" CssClass="e-elevated-check"></SfCheckBox>

<style>
    .e-elevated-check .e-checkbox-wrapper {
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
        padding: 8px;
        border-radius: 4px;
    }
</style>
```

### Animated Checkbox

```razor
<SfCheckBox TChecked="bool" Label="Animated Checkbox" CssClass="e-animated-check"></SfCheckBox>

<style>
    .e-animated-check .e-frame {
        transition: all 0.3s ease;
    }
    
    .e-animated-check.e-check .e-frame {
        transform: scale(1.1);
        background-color: #4caf50;
    }
</style>
```

### Hover Effect

```razor
<SfCheckBox TChecked="bool" Label="Interactive Checkbox" CssClass="e-interactive-check"></SfCheckBox>

<style>
    .e-interactive-check:hover .e-frame {
        border-color: #2196f3;
        box-shadow: 0 0 5px rgba(33, 150, 243, 0.3);
    }
</style>
```

### Disabled Appearance

```razor
<SfCheckBox TChecked="bool" Label="Disabled Style" Disabled="true" CssClass="e-disabled-style"></SfCheckBox>

<style>
    .e-disabled-style.e-checkbox.e-disabled .e-frame {
        background-color: #f5f5f5;
        border-color: #ddd;
        opacity: 0.6;
    }
    
    .e-disabled-style.e-checkbox.e-disabled .e-label {
        color: #999;
        cursor: not-allowed;
    }
</style>
```

### Dark Mode Checkbox

```razor
<SfCheckBox TChecked="bool" Label="Dark Mode" CssClass="e-dark-mode"></SfCheckBox>

<style>
    @media (prefers-color-scheme: dark) {
        .e-dark-mode .e-frame {
            background-color: #333;
            border-color: #555;
        }
        
        .e-dark-mode.e-check .e-frame {
            background-color: #4caf50;
            border-color: #4caf50;
        }
        
        .e-dark-mode .e-label {
            color: #f0f0f0;
        }
    }
</style>
```

---

## Container and Layout Styling

### Full Width Checkbox

```razor
<SfCheckBox TChecked="bool" Label="Full Width" CssClass="e-full-width"></SfCheckBox>

<style>
    .e-full-width {
        display: block;
        width: 100%;
    }
    
    .e-full-width .e-checkbox-wrapper {
        width: 100%;
        padding: 12px;
    }
</style>
```

### Checkbox with Icon

```razor
<SfCheckBox TChecked="bool" Label="With Icon" CssClass="e-icon-check"></SfCheckBox>

<style>
    .e-icon-check::before {
        content: "✓";
        margin-right: 8px;
        color: #4caf50;
        font-weight: bold;
    }
</style>
```

### Grouped Checkboxes

```razor
<div class="e-checkbox-group">
    <SfCheckBox TChecked="bool" Label="Option 1"></SfCheckBox>
    <SfCheckBox TChecked="bool" Label="Option 2"></SfCheckBox>
    <SfCheckBox TChecked="bool" Label="Option 3"></SfCheckBox>
</div>

<style>
    .e-checkbox-group {
        display: flex;
        flex-direction: column;
        gap: 12px;
        padding: 16px;
        border: 1px solid #ddd;
        border-radius: 4px;
        background-color: #fafafa;
    }
    
    .e-checkbox-group .e-checkbox-wrapper {
        padding: 4px 0;
    }
</style>
```

---

## Best Practices for Styling

1. **Use CSS Variables:** Define reusable colors and spacing using CSS variables for consistency
2. **Respect Theme:** Build upon the selected theme rather than completely overriding it
3. **Accessibility:** Maintain sufficient contrast ratios (WCAG AA minimum 4.5:1)
4. **Performance:** Use CSS classes instead of inline styles for better performance
5. **Responsive:** Use media queries for different screen sizes
6. **Transitions:** Keep animations under 300ms for better UX
7. **Consistency:** Match your application's design system and component styling

---

## Troubleshooting Styling Issues

**Checkbox color not changing:**
- Check if CSS specificity is high enough
- Verify the checkbox is in a checked state (`e-check` class)
- Ensure theme CSS is loaded before custom styles

**Label not aligning properly:**
- Check flexbox properties in the checkbox wrapper
- Verify line-height settings for the label
- Adjust margin/padding as needed

**Custom styles not applying in RTL:**
- Use `[dir="rtl"]` selector for RTL-specific styles
- Test flexbox direction changes
- Verify CSS file is loaded after theme CSS

