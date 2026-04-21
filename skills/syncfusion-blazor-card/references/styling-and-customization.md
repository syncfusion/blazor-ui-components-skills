# Styling and Customization

## Table of Contents

- [Overview](#overview)
- [CSS Classes](#css-classes)
- [Card Styling](#card-styling)
- [Header Styling](#header-styling)
- [Content Styling](#content-styling)
- [Image Styling](#image-styling)
- [Footer Styling](#footer-styling)
- [Theme Customization](#theme-customization)
- [Advanced Styling](#advanced-styling)
- [Responsive Styling](#responsive-styling)
- [Troubleshooting](#troubleshooting)

## Overview

The Syncfusion Blazor Card component provides comprehensive styling capabilities through CSS classes and custom CSS. You can customize every aspect of the card including colors, spacing, borders, shadows, and layout. Theme variables and CSS custom properties allow for consistent theming across your application.

## CSS Classes

### Main Card Classes

| Class | Element | Purpose |
|-------|---------|---------|
| `.e-card` | Card root | Main card container styles |
| `.e-card-header` | Header section | Header container styles |
| `.e-card-content` | Content section | Content area styles |
| `.e-card-image` | Image element | Image container styles |
| `.e-card-footer` | Footer section | Footer container styles |
| `.e-card-separator` | Divider | Divider line styles |

### Header Specific Classes

| Class | Purpose |
|-------|---------|
| `.e-card-header-caption` | Header text wrapper |
| `.e-card-header-title` | Main title text |
| `.e-card-sub-title` | Subtitle text |
| `.e-card-header-image` | Header image element |

## Card Styling

### Basic Card Customization

```css
.e-card {
    background-color: #ffffff;
    padding: 16px;
    margin-bottom: 16px;
    border-radius: 4px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    border: 1px solid #e0e0e0;
}
```

### Card Colors

Customize card background and border colors:

```css
.e-card {
    background-color: #f5f5f5;
    border-color: #ddd;
}

.e-card.primary-card {
    background-color: #e3f2fd;
    border-color: #2196f3;
}

.e-card.success-card {
    background-color: #e8f5e9;
    border-color: #4caf50;
}

.e-card.danger-card {
    background-color: #ffebee;
    border-color: #f44336;
}
```

Usage:
```razor
<SfCard CssClass="primary-card">
    <!-- card content -->
</SfCard>
```

### Card Spacing

Control card padding and margins:

```css
.e-card {
    padding: 20px;
    margin-bottom: 20px;
}

.e-card.compact {
    padding: 12px;
    margin-bottom: 12px;
}

.e-card.spacious {
    padding: 28px;
    margin-bottom: 28px;
}
```

### Card Shadows

Add depth with shadow effects:

```css
.e-card {
    box-shadow: 0 1px 3px rgba(0, 0, 0, 0.12);
}

.e-card.elevated {
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.15);
}

.e-card.floating {
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
    transition: box-shadow 0.3s ease;
}

.e-card.floating:hover {
    box-shadow: 0 12px 24px rgba(0, 0, 0, 0.25);
}
```

## Header Styling

### Header Background

Customize header appearance:

```css
.e-card .e-card-header {
    background-color: #f5f5f5;
    padding-bottom: 12px;
    border-bottom: 1px solid #e0e0e0;
}

.e-card .e-card-header.dark-header {
    background-color: #333;
    color: #fff;
}

.e-card .e-card-header.colored-header {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: #fff;
}
```

### Title and Subtitle

```css
.e-card .e-card-header .e-card-header-caption .e-card-header-title {
    font-size: 18px;
    font-weight: 600;
    color: #333;
    margin-bottom: 4px;
}

.e-card .e-card-header .e-card-header-caption .e-card-sub-title {
    font-size: 14px;
    color: #666;
    font-weight: 400;
}
```

### Header Image

Customize header image styling:

```css
.e-card .e-card-header .e-card-header-image {
    height: 64px;
    width: 64px;
    border-radius: 50%;
    object-fit: cover;
    border: 2px solid #ddd;
    margin-right: 12px;
}

.e-card .e-card-header .e-card-header-image.bordered {
    border: 3px solid #2196f3;
}

.e-card .e-card-header .e-card-header-image.shadow {
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
```

## Content Styling

### Content Appearance

```css
.e-card .e-card-content {
    font-size: 14px;
    color: #555;
    line-height: 1.6;
    padding: 12px 0;
}

.e-card .e-card-content.large-text {
    font-size: 16px;
    line-height: 1.8;
}

.e-card .e-card-content.small-text {
    font-size: 12px;
    line-height: 1.4;
}
```

### Text Formatting in Content

```css
.e-card .e-card-content strong {
    color: #333;
    font-weight: 600;
}

.e-card .e-card-content em {
    color: #2196f3;
    font-style: italic;
}

.e-card .e-card-content a {
    color: #2196f3;
    text-decoration: none;
}

.e-card .e-card-content a:hover {
    text-decoration: underline;
}
```

## Image Styling

### Card Image

Customize CardImage component:

```css
.e-card .e-card-image {
    background-size: cover;
    background-position: center;
    height: 200px;
    width: 100%;
    border-radius: 4px 4px 0 0;
}

.e-card .e-card-image.large {
    height: 300px;
}

.e-card .e-card-image.small {
    height: 150px;
}

.e-card .e-card-image.with-overlay {
    position: relative;
    opacity: 0.8;
    transition: opacity 0.3s ease;
}

.e-card .e-card-image.with-overlay::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    background: rgba(0, 0, 0, 0.2);
}
```

### Image Hover Effects

```css
.e-card .e-card-image {
    transition: transform 0.3s ease, filter 0.3s ease;
}

.e-card:hover .e-card-image {
    transform: scale(1.05);
    filter: brightness(0.9);
}
```

## Footer Styling

### Divider Styling

```css
.e-card .e-card-separator {
    border-bottom: 1px solid #e0e0e0;
    padding-bottom: 16px;
    margin-bottom: 16px;
}

.e-card .e-card-separator.thick {
    border-bottom: 2px solid #ccc;
}

.e-card .e-card-separator.dashed {
    border-bottom: 1px dashed #ddd;
}
```

### Card Footer

```css
.e-card .e-card-footer {
    background-color: #f9f9f9;
    padding-top: 12px;
    border-top: 1px solid #e0e0e0;
}

.e-card .e-card-footer.dark-footer {
    background-color: #333;
}
```

### Footer Content

```css
.e-card .e-card-actions {
    display: flex;
    gap: 10px;
    align-items: center;
    justify-content: flex-start;
}

.e-card .e-card-actions.centered {
    justify-content: center;
}

.e-card .e-card-actions.right-aligned {
    justify-content: flex-end;
}
```

## Theme Customization

### Using Bootstrap Theme

```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```

### Using Material Theme

```html
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />
```

### CSS Custom Properties (Variables)

Override theme variables:

```css
:root {
    --e-card-background: #ffffff;
    --e-card-border: 1px solid #e0e0e0;
    --e-card-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
    --e-card-header-background: #f5f5f5;
    --e-card-header-color: #333;
    --e-card-text-color: #555;
}
```

## Advanced Styling

### Card States

Create different card states:

```css
.e-card.active {
    border-left: 4px solid #2196f3;
    background-color: #f0f8ff;
}

.e-card.disabled {
    opacity: 0.6;
    pointer-events: none;
}

.e-card.loading {
    position: relative;
    pointer-events: none;
}

.e-card.error {
    border-color: #f44336;
    background-color: #ffebee;
}

.e-card.success {
    border-color: #4caf50;
    background-color: #e8f5e9;
}
```

### Transitions and Animations

```css
.e-card {
    transition: all 0.3s ease;
}

.e-card:hover {
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
    transform: translateY(-4px);
}

@keyframes slideIn {
    from {
        opacity: 0;
        transform: translateY(20px);
    }
    to {
        opacity: 1;
        transform: translateY(0);
    }
}

.e-card.animate {
    animation: slideIn 0.3s ease;
}
```

### Dark Mode Support

```css
@media (prefers-color-scheme: dark) {
    .e-card {
        background-color: #424242;
        color: #fff;
        border-color: #555;
    }

    .e-card .e-card-header {
        background-color: #333;
        border-bottom-color: #555;
    }

    .e-card .e-card-content {
        color: #ddd;
    }
}
```

## Responsive Styling

### Mobile Responsive

```css
/* Desktop */
@media (min-width: 1024px) {
    .e-card {
        padding: 20px;
        margin-bottom: 20px;
    }
}

/* Tablet */
@media (min-width: 768px) and (max-width: 1023px) {
    .e-card {
        padding: 16px;
        margin-bottom: 16px;
    }
}

/* Mobile */
@media (max-width: 767px) {
    .e-card {
        padding: 12px;
        margin-bottom: 12px;
        border-radius: 2px;
    }

    .e-card .e-card-image {
        height: 150px;
    }
}
```

### Flexible Layout

```css
.card-grid {
    display: grid;
    gap: 20px;
}

@media (min-width: 1200px) {
    .card-grid {
        grid-template-columns: repeat(4, 1fr);
    }
}

@media (min-width: 768px) and (max-width: 1199px) {
    .card-grid {
        grid-template-columns: repeat(2, 1fr);
    }
}

@media (max-width: 767px) {
    .card-grid {
        grid-template-columns: 1fr;
    }
}
```

## Complete Styling Example

```razor
@page "/styled-card"

<SfCard CssClass="premium-card">
    <CardImage Image="images/product.jpg" />
    <CardHeader Title="Premium Product" SubTitle="Exclusive Collection" />
    <CardContent EnableSeparator="true">
        <strong>Features:</strong> High quality, durable, eco-friendly
    </CardContent>
    <CardContent EnableSeparator="true">
        <strong>Price:</strong> $99.99 <span class="discount">-20%</span>
    </CardContent>
    <CardFooter>
        <CardFooterContent>
            <SfButton CssClass="e-btn e-primary">BUY NOW</SfButton>
        </CardFooterContent>
    </CardFooter>
</SfCard>

<style>
    .premium-card {
        background: linear-gradient(to bottom, #ffffff 0%, #f5f5f5 100%);
        border: 2px solid #ddd;
        border-radius: 8px;
        box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        overflow: hidden;
        max-width: 300px;
    }

    .premium-card:hover {
        box-shadow: 0 8px 20px rgba(0, 0, 0, 0.15);
        transform: translateY(-4px);
    }

    .premium-card .e-card-header {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        padding: 16px;
    }

    .premium-card .e-card-header .e-card-header-title {
        color: white;
        font-size: 20px;
    }

    .discount {
        background-color: #ff5722;
        color: white;
        padding: 2px 8px;
        border-radius: 4px;
        font-weight: 600;
    }
</style>
```

## Best Practices

### Do's
- ✓ Use CSS classes for consistent styling
- ✓ Leverage theme variables for maintainability
- ✓ Test responsive designs on multiple screen sizes
- ✓ Use transitions for smooth interactions
- ✓ Follow accessibility color contrast guidelines

### Don'ts
- ✗ Don't override critical component styles unnecessarily
- ✗ Don't mix inline styles with CSS classes
- ✗ Don't create overly complex selectors
- ✗ Don't forget dark mode support
- ✗ Don't ignore responsive design

## Troubleshooting

### Styles Not Applying
- Check CSS specificity (use !important sparingly)
- Verify selector matches card elements
- Clear browser cache
- Rebuild and redeploy

### Theme Not Loading
- Verify theme CSS link is correct
- Check file path in wwwroot
- Ensure theme package is installed
- Check browser console for 404 errors

### Responsive Issues
- Test with browser dev tools
- Use proper media query breakpoints
- Verify CSS grid/flexbox syntax
- Check parent container constraints
