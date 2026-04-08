# Localization & Accessibility

This guide covers multi-language support, RTL (Right-to-Left) support, and accessibility best practices for the Query Builder component.

## Localization

### Configure Locale

Set the locale in your Blazor application:

```csharp
// In Program.cs
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

builder.Services.AddSyncfusionBlazor();
builder.Services.Configure<SyncfusionBlazorSettings>(options => 
{
    options.Locale = "fr-FR";  // Set to French
});

var app = builder.Build();
```

### Supported Locales

Query Builder supports multiple locales:
- `en-US` - English (United States, default)
- `fr-FR` - French
- `de-DE` - German
- `es-ES` - Spanish
- `ja-JP` - Japanese
- `zh-CN` - Chinese (Simplified)
- `zh-TW` - Chinese (Traditional)
- `ar-AR` - Arabic
- `ru-RU` - Russian
- And many more...

### Custom Localization

Define custom translated strings:

```csharp
// In Program.cs
var localization = new SfLocalization();

localization.SetLocale(new Dictionary<string, object>
{
    { "en-US", new Dictionary<string, string> 
    {
        { "AddRule", "Add Rule" },
        { "AddGroup", "Add Group" },
        { "DeleteGroup", "Delete Group" }
    }},
    { "es-ES", new Dictionary<string, string>
    {
        { "AddRule", "Agregar Regla" },
        { "AddGroup", "Agregar Grupo" },
        { "DeleteGroup", "Eliminar Grupo" }
    }}
});
```

## Right-to-Left (RTL) Support

### Enable RTL Mode

```razor
<SfQueryBuilder TValue="Order" EnableRtl="true">
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="معرّف الطلب" Type="ColumnType.Number"></QueryBuilderColumn>
        <QueryBuilderColumn Field="Status" Label="الحالة" Type="ColumnType.String"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>
```

### RTL with Locale

```razor
<SfQueryBuilder TValue="Order" EnableRtl="true">
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="معرّف الطلب" Type="ColumnType.Number"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>
```

### RTL CSS

```css
/* Automatic RTL adjustments for Arabic/Hebrew */
[dir="rtl"] .e-querybuilder {
    direction: rtl;
    text-align: right;
}

[dir="rtl"] .e-rule {
    text-align: right;
    padding-right: 15px;
    padding-left: 5px;
}

[dir="rtl"] .e-querybuilder button {
    margin-left: auto;
    margin-right: 0;
}
```

## Accessibility Best Practices

### WCAG Compliance

Query Builder follows WCAG 2.1 Level AA accessibility standards:
- Keyboard navigation
- Screen reader support
- Color contrast requirements
- Focus indicators
- ARIA labels

### Keyboard Navigation

Enable full keyboard navigation:

```razor
<SfQueryBuilder TValue="Order">
    <QueryBuilderColumns>
        <QueryBuilderColumn Field="OrderID" Label="Order ID" Type="ColumnType.Number"></QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>

<!-- Navigation keys:
     Tab - Move to next element
     Shift+Tab - Move to previous element
     Enter - Activate button or open dropdown
     Space - Activate button
     Escape - Close dropdown
     Arrow keys - Select options in dropdown
-->
```

### ARIA Attributes

Query Builder automatically provides ARIA attributes:

```html
<!-- Generated ARIA attributes -->
<div class="e-querybuilder" role="region" aria-label="Query Builder">
    <div class="e-rule" role="group" aria-label="Rule">
        <select aria-label="Select Field">
            <!-- options -->
        </select>
    </div>
</div>
```

### Color Contrast

Ensure sufficient contrast for readability:

```css
/* High contrast colors for accessibility */
.e-querybuilder {
    color: #212529;  /* Dark gray on light background */
    background-color: #ffffff;
}

.e-querybuilder .e-rule {
    border: 2px solid #495057;  /* Visible border */
    background-color: #ffffff;
}

/* Focus indicators */
.e-querybuilder input:focus,
.e-querybuilder select:focus,
.e-querybuilder button:focus {
    outline: 3px solid #0d6efd;  /* Blue outline */
    outline-offset: 2px;
}
```

### Screen Reader Support

Make Query Builder screen-reader friendly:

```razor
<SfQueryBuilder TValue="Order" AccessibilityLabel="Filter Orders by Conditions">
    <QueryBuilderColumns>
        <!-- Provide descriptive labels -->
        <QueryBuilderColumn 
            Field="OrderID" 
            Label="Order ID" 
            Type="ColumnType.Number"
            AriaLabel="Select Order ID to filter">
        </QueryBuilderColumn>
    </QueryBuilderColumns>
</SfQueryBuilder>
```

### Label All Fields

```razor
<!-- ✓ Good: Clear labels -->
<label for="query-field">
    <span>Filter Field</span>
    <select id="query-field" aria-label="Select field to filter">
        <!-- options -->
    </select>
</label>

<!-- ✗ Bad: Missing labels -->
<select>
    <!-- options -->
</select>
```

### Error Messages for Accessibility

```razor
@code {
    private string ErrorMessage = "";
    
    private async Task ValidateQuery() {
        try {
            var rules = QueryBuilder.GetRules();
            
            if (rules == null || rules.Rules.Count == 0) {
                ErrorMessage = "Error: Please add at least one condition";
                // Announce to screen readers
                await JS.InvokeVoidAsync("announce", ErrorMessage);
            }
        } catch (Exception ex) {
            ErrorMessage = $"Error: {ex.Message}";
            await JS.InvokeVoidAsync("announce", ErrorMessage);
        }
    }
}
```

## Accessibility Checklist

- [ ] Keyboard navigation works for all controls
- [ ] Focus indicators are visible
- [ ] Color contrast meets WCAG AA standards (4.5:1 for text)
- [ ] All inputs have associated labels
- [ ] Error messages are clear and associated with fields
- [ ] Screen readers announce all important information
- [ ] Component works with browser zoom (up to 200%)
- [ ] No content relies solely on color
- [ ] Animations can be disabled (prefers-reduced-motion)

## Reduced Motion Support

Respect user's motion preferences:

```css
/* Disable animations for users who prefer reduced motion */
@media (prefers-reduced-motion: reduce) {
    .e-querybuilder * {
        animation: none !important;
        transition: none !important;
    }
}
```

## Next Steps

- [Advanced Features](advanced-features.md) - Import/export and persistence
- [Troubleshooting](troubleshooting.md) - Accessibility issues
- [Customization](customization-and-styling.md) - Styling accessibility
