# Customization and Styling in Blazor Accordion Component

## Table of Contents
- [Overview](#overview)
- [Customizing Accordion Border](#customizing-accordion-border)
- [Customizing Accordion Items](#customizing-accordion-items)
- [Customizing Header Content](#customizing-header-content)
- [Customizing Expand/Collapse Icons](#customizing-expandcollapse-icons)
- [Customizing Hover State](#customizing-hover-state)
- [Customizing Selected Items](#customizing-selected-items)
- [Using CssClass Property](#using-cssclass-property)
- [CSS Class Reference](#css-class-reference)
- [Theme Integration](#theme-integration)

## Overview

The Accordion component provides extensive customization options through CSS. You can modify the appearance of the accordion container, individual items, headers, icons, and interaction states to match your application's design requirements.

## Customizing Accordion Border

Apply custom border styles to the entire accordion component:

```cshtml
<SfAccordion>
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Content 1"></AccordionItem>
        <AccordionItem Header="Item 2" Content="Content 2"></AccordionItem>
    </AccordionItems>
</SfAccordion>

<style>
    .e-accordion {
        border: 5px solid #4CAF50;
    }
</style>
```

**Customization options:**
- Border width, style, and color
- Border radius for rounded corners
- Box shadow for depth effect
- Background color

## Customizing Accordion Items

Style individual accordion items:

```cshtml
<SfAccordion>
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Content 1"></AccordionItem>
        <AccordionItem Header="Item 2" Content="Content 2"></AccordionItem>
    </AccordionItems>
</SfAccordion>

<style>
    .e-accordion .e-acrdn-item.e-select {
        text-align: center;
        background-color: #E3F2FD;
    }
</style>
```

**Customization options:**
- Background color
- Text alignment
- Padding and margins
- Border styles

## Customizing Header Content

Apply custom styles to accordion header text:

```cshtml
<SfAccordion>
    <AccordionItems>
        <AccordionItem Header="Custom Header" Content="Content here"></AccordionItem>
    </AccordionItems>
</SfAccordion>

<style>
    .e-accordion .e-acrdn-item .e-acrdn-header .e-acrdn-header-content {
        color: #1976D2;
        font-style: italic;
        font-weight: bold;
        font-size: 16px;
    }
</style>
```

**Customization options:**
- Font color, size, and weight
- Font family and style
- Text transformation
- Letter spacing

## Customizing Expand/Collapse Icons

Customize the toggle icons that indicate expand/collapse state:

```cshtml
<SfAccordion>
    <AccordionItems>
        <AccordionItem Header="Item 1" Content="Content 1"></AccordionItem>
    </AccordionItems>
</SfAccordion>

<style>
    .e-accordion .e-acrdn-item .e-acrdn-header .e-toggle-icon .e-icons {
        color: #FF5722;
        font-size: 18px;
    }
</style>
```

**Customization options:**
- Icon color
- Icon size
- Icon position
- Custom icon fonts

## Customizing Hover State

Apply styles when users hover over accordion headers:

```cshtml
<SfAccordion>
    <AccordionItems>
        <AccordionItem Header="Hover over me" Content="Content here"></AccordionItem>
    </AccordionItems>
</SfAccordion>

<style>
    .e-accordion .e-acrdn-item .e-acrdn-header:hover {
        border: 2px solid #2196F3;
        background-color: #E3F2FD;
    }
</style>
```

**Customization options:**
- Background color change
- Border highlight
- Text color change
- Cursor style

## Customizing Selected Items

Style accordion items when they are expanded:

### Selected Item Header

```cshtml
<SfAccordion>
    <AccordionItems>
        <AccordionItem Expanded="true" Header="Selected Item" Content="Content here"></AccordionItem>
    </AccordionItems>
</SfAccordion>

<style>
    .e-accordion .e-acrdn-item.e-select.e-selected.e-expand-state > .e-acrdn-header,
    .e-accordion .e-acrdn-item.e-select.e-expand-state > .e-acrdn-header,
    .e-accordion .e-acrdn-item.e-selected.e-select > .e-acrdn-header,
    .e-accordion .e-acrdn-item.e-selected.e-select.e-expand-state > .e-acrdn-header:focus {
        background-color: #1565C0;
    }
</style>
```

### Selected Item Text

```cshtml
<style>
    .e-accordion .e-acrdn-item.e-select.e-selected.e-expand-state > .e-acrdn-header .e-acrdn-header-content,
    .e-accordion .e-acrdn-item.e-select.e-expand-state > .e-acrdn-header .e-acrdn-header-content,
    .e-accordion .e-acrdn-item.e-selected > .e-acrdn-header > .e-acrdn-header-content {
        color: #FFFFFF;
        font-weight: bold;
    }
</style>
```

## Using CssClass Property

Apply custom CSS classes to individual accordion items using the `CssClass` property:

```cshtml
<SfAccordion>
    <AccordionItems>
        <AccordionItem CssClass="primary-item" Header="Primary Item" Content="Primary content"></AccordionItem>
        <AccordionItem CssClass="secondary-item" Header="Secondary Item" Content="Secondary content"></AccordionItem>
        <AccordionItem CssClass="success-item" Header="Success Item" Content="Success content"></AccordionItem>
    </AccordionItems>
</SfAccordion>

<style>
    .e-accordion .e-acrdn-item .e-acrdn-header .e-toggle-icon .e-icons {
        color: #000000;
    }
    
    .e-accordion .primary-item.e-acrdn-item.e-select > .e-acrdn-header {
        background: #2196F3;
        color: #FFFFFF;
    }
    
    .e-accordion .secondary-item.e-acrdn-item.e-select > .e-acrdn-header {
        background: #9C27B0;
        color: #FFFFFF;
    }
    
    .e-accordion .success-item.e-acrdn-item.e-select > .e-acrdn-header {
        background: #4CAF50;
        color: #FFFFFF;
    }
</style>
```

**Benefits:**
- Target specific items without affecting others
- Create themed accordion items
- Apply different styles based on item type
- Maintain clean, maintainable CSS

## CSS Class Reference

### Root and Item Classes

| CSS Class | Description |
|-----------|-------------|
| `.e-accordion` | Root container element |
| `.e-acrdn-item` | Individual accordion item wrapper |
| `.e-acrdn-item:first-child` | First accordion item |
| `.e-acrdn-item:last-child` | Last accordion item |

### Header Classes

| CSS Class | Description |
|-----------|-------------|
| `.e-acrdn-header` | Accordion item header element |
| `.e-acrdn-header-content` | Header text/content wrapper |
| `.e-acrdn-header-icon` | Custom header icon container |
| `.e-acrdn-header:hover` | Header hover state |
| `.e-acrdn-header:active` | Header active/pressed state |
| `.e-acrdn-header:focus` | Header focused state |

### Toggle Icon Classes

| CSS Class | Description |
|-----------|-------------|
| `.e-toggle-icon` | Toggle icon container |
| `.e-tgl-collapse-icon` | Actual icon element |
| `.e-acrdn-icons` | Icon font element |
| `.e-icons` | Base Syncfusion icon class |

### Panel and Content Classes

| CSS Class | Description |
|-----------|-------------|
| `.e-acrdn-panel` | Collapsible content container |
| `.e-acrdn-content` | Inner content wrapper |

### State Classes

| CSS Class | Description |
|-----------|-------------|
| `.e-select` | Interactive/selectable item |
| `.e-selected` | Currently expanded item |
| `.e-expand-state` | Item in expanded state |
| `.e-active` | Currently active item |
| `.e-item-focus` | Keyboard-focused item |
| `.e-overlay` | Item during animation |
| `.e-hide` | Hidden item |
| `.e-content-hide` | Hidden content |

### Nested and RTL Classes

| CSS Class | Description |
|-----------|-------------|
| `.e-acrdn-panel.e-nested` | Nested accordion panel |
| `.e-rtl` | Right-to-left mode |

## Theme Integration

### Using Built-in Themes

The Accordion component supports multiple built-in themes:

```html
<!-- Fluent 2 Theme -->
<link href="_content/Syncfusion.Blazor.Themes/fluent2.css" rel="stylesheet" />

<!-- Material Theme -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- Bootstrap 5 Theme -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```

### Creating Custom Themes

Override theme variables for consistent customization across themes:

```css
:root {
    --accordion-bg-color: #FFFFFF;
    --accordion-header-color: #333333;
    --accordion-hover-bg: #F5F5F5;
    --accordion-selected-bg: #2196F3;
    --accordion-selected-color: #FFFFFF;
}

.e-accordion {
    background-color: var(--accordion-bg-color);
}

.e-accordion .e-acrdn-header {
    color: var(--accordion-header-color);
}

.e-accordion .e-acrdn-header:hover {
    background-color: var(--accordion-hover-bg);
}

.e-accordion .e-acrdn-item.e-selected > .e-acrdn-header {
    background-color: var(--accordion-selected-bg);
    color: var(--accordion-selected-color);
}
```

### Responsive Styling

Adapt accordion styles for different screen sizes:

```css
/* Desktop */
.e-accordion .e-acrdn-header {
    padding: 15px 20px;
    font-size: 16px;
}

/* Tablet */
@media (max-width: 768px) {
    .e-accordion .e-acrdn-header {
        padding: 12px 15px;
        font-size: 14px;
    }
}

/* Mobile */
@media (max-width: 480px) {
    .e-accordion .e-acrdn-header {
        padding: 10px 12px;
        font-size: 13px;
    }
    
    .e-accordion {
        border-width: 1px;
    }
}
```

## Advanced Customization Examples

### Card-Style Accordion

```cshtml
<SfAccordion>
    <AccordionItems>
        <AccordionItem Header="Card Item 1" Content="Content 1"></AccordionItem>
        <AccordionItem Header="Card Item 2" Content="Content 2"></AccordionItem>
    </AccordionItems>
</SfAccordion>

<style>
    .e-accordion {
        border: none;
        background: transparent;
    }
    
    .e-accordion .e-acrdn-item {
        margin-bottom: 10px;
        border-radius: 8px;
        box-shadow: 0 2px 4px rgba(0,0,0,0.1);
        border: 1px solid #E0E0E0;
    }
    
    .e-accordion .e-acrdn-item .e-acrdn-header {
        border-radius: 8px 8px 0 0;
        background: #F5F5F5;
    }
    
    .e-accordion .e-acrdn-item.e-selected .e-acrdn-header {
        background: #2196F3;
        color: #FFFFFF;
    }
</style>
```

### Gradient Background Accordion

```cshtml
<SfAccordion>
    <AccordionItems>
        <AccordionItem Header="Gradient Item" Content="Content here"></AccordionItem>
    </AccordionItems>
</SfAccordion>

<style>
    .e-accordion .e-acrdn-item.e-selected > .e-acrdn-header {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: #FFFFFF;
    }
    
    .e-accordion .e-acrdn-item.e-selected .e-toggle-icon .e-icons {
        color: #FFFFFF;
    }
</style>
```

### Minimal/Flat Design

```cshtml
<SfAccordion>
    <AccordionItems>
        <AccordionItem Header="Minimal Item 1" Content="Content 1"></AccordionItem>
        <AccordionItem Header="Minimal Item 2" Content="Content 2"></AccordionItem>
    </AccordionItems>
</SfAccordion>

<style>
    .e-accordion {
        border: none;
    }
    
    .e-accordion .e-acrdn-item {
        border: none;
        border-bottom: 1px solid #E0E0E0;
    }
    
    .e-accordion .e-acrdn-header {
        background: transparent;
        padding: 15px 10px;
    }
    
    .e-accordion .e-acrdn-item.e-selected > .e-acrdn-header {
        background: transparent;
        color: #2196F3;
        border-left: 3px solid #2196F3;
        padding-left: 12px;
    }
</style>
```

### Custom Icon Styling

```cshtml
<SfAccordion>
    <AccordionItems>
        <AccordionItem Header="Item with Custom Icon" Content="Content here"></AccordionItem>
    </AccordionItems>
</SfAccordion>

<style>
    .e-accordion .e-toggle-icon {
        width: 30px;
        height: 30px;
        background: #2196F3;
        border-radius: 50%;
        display: flex;
        align-items: center;
        justify-content: center;
    }
    
    .e-accordion .e-toggle-icon .e-icons {
        color: #FFFFFF;
    }
</style>
```

## Best Practices

### Styling Guidelines

1. **Use specific selectors** to avoid affecting other components
2. **Test with different themes** to ensure compatibility
3. **Maintain consistent spacing** across items
4. **Consider accessibility** when changing colors (contrast ratios)
5. **Use CSS variables** for easy theme switching

### Performance Tips

1. **Minimize CSS complexity** - avoid deeply nested selectors
2. **Use CSS classes** instead of inline styles
3. **Avoid !important** - use proper specificity instead
4. **Group similar styles** for better maintainability
5. **Test on mobile devices** for responsive behavior

### Common Pitfalls

**Avoid:**
- Overriding component structure with display/position changes
- Breaking animation behavior with transform overrides
- Conflicting with theme updates
- Using inline styles that can't be easily changed
- Forgetting to test hover/focus/active states

## Troubleshooting

**Styles not applying:**
- Check CSS specificity (use browser dev tools)
- Ensure styles are loaded after theme CSS
- Verify class names are correct
- Check for typos in selectors

**Hover effects not working:**
- Ensure `.e-select` class is present in selector
- Check for z-index issues
- Verify hover is not disabled via CSS

**Custom colors affecting readability:**
- Test color contrast ratios (WCAG AA: 4.5:1 minimum)
- Provide sufficient contrast for text
- Test with color blindness simulators

## Related Topics

- [Getting Started](getting-started.md) - Adding theme stylesheets
- [Accessibility and Animations](accessibility-animations.md) - Color contrast requirements
- [How-To Scenarios](how-to-scenarios.md) - Practical styling examples
