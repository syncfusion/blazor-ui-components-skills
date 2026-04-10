# Styling and Themes

## Table of Contents
- [Theme Integration](#theme-integration)
- [Toast Styling](#toast-styling)
- [Message Styling](#message-styling)
- [Skeleton Styling](#skeleton-styling)
- [Dark Mode Support](#dark-mode-support)
- [Responsive Design](#responsive-design)
- [Custom Themes](#custom-themes)

---

## Theme Integration

Syncfusion Blazor components support multiple built-in themes that provide consistent styling across all notification components.

### Available Themes

```html
<!-- Bootstrap 5 (Default) -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Material Design -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- Fluent UI (Microsoft) -->
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />

<!-- Tailwind CSS -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />

<!-- Office Fabric -->
<link href="_content/Syncfusion.Blazor.Themes/fabric.css" rel="stylesheet" />

<!-- High Contrast (Accessibility) -->
<link href="_content/Syncfusion.Blazor.Themes/highcontrast.css" rel="stylesheet" />
```

### Dark Mode Variants

```html
<!-- Bootstrap 5 Dark -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5-dark.css" rel="stylesheet" />

<!-- Material Dark -->
<link href="_content/Syncfusion.Blazor.Themes/material-dark.css" rel="stylesheet" />

<!-- Fluent Dark -->
<link href="_content/Syncfusion.Blazor.Themes/fluent-dark.css" rel="stylesheet" />

<!-- Tailwind Dark -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind-dark.css" rel="stylesheet" />

<!-- Fabric Dark -->
<link href="_content/Syncfusion.Blazor.Themes/fabric-dark.css" rel="stylesheet" />
```

### Theme Switcher Implementation

```razor
@page "/theme-switcher"

<div class="theme-selector">
    <label>Select Theme:</label>
    <select @onchange="OnThemeChange">
        <option value="bootstrap5">Bootstrap 5</option>
        <option value="material">Material</option>
        <option value="fluent">Fluent</option>
        <option value="tailwind">Tailwind</option>
        <option value="fabric">Fabric</option>
    </select>
    
    <label>
        <input type="checkbox" @onchange="ToggleDarkMode" />
        Dark Mode
    </label>
</div>

@code {
    private string currentTheme = "bootstrap5";
    private bool isDarkMode = false;

    private void OnThemeChange(ChangeEventArgs e)
    {
        currentTheme = e.Value.ToString();
        UpdateTheme();
    }

    private void ToggleDarkMode(ChangeEventArgs e)
    {
        isDarkMode = (bool)e.Value;
        UpdateTheme();
    }

    private void UpdateTheme()
    {
        var themeName = isDarkMode ? $"{currentTheme}-dark" : currentTheme;
        // Update theme reference in App.razor or use JavaScript interop
        JSRuntime.InvokeVoidAsync("changeTheme", themeName);
    }
}
```

---

## Toast Styling

### Built-in CSS Classes

Syncfusion Toast provides predefined CSS classes for different message types:

```razor
<!-- Success Toast -->
<SfToast CssClass="e-toast-success" Title="Success" Content="Operation completed" />

<!-- Info Toast -->
<SfToast CssClass="e-toast-info" Title="Info" Content="New update available" />

<!-- Warning Toast -->
<SfToast CssClass="e-toast-warning" Title="Warning" Content="Session expiring soon" />

<!-- Danger/Error Toast -->
<SfToast CssClass="e-toast-danger" Title="Error" Content="Failed to save" />
```

### Custom Toast Colors

```razor
<SfToast CssClass="custom-toast-primary" Title="Custom" Content="Custom styled toast" />

<style>
    .custom-toast-primary {
        background-color: #6200EA;
        color: white;
    }

    .custom-toast-primary .e-toast-title {
        color: white;
        font-weight: 600;
    }

    .custom-toast-primary .e-toast-message {
        color: rgba(255, 255, 255, 0.9);
    }

    .custom-toast-primary .e-toast-icon {
        color: white;
    }
</style>
```

### Toast Shadow and Border

```razor
<SfToast CssClass="elevated-toast" Title="Elevated" Content="Toast with custom shadow" />

<style>
    .elevated-toast {
        box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
        border: 2px solid #2196F3;
        border-radius: 12px;
    }
</style>
```

### Toast Typography

```razor
<SfToast CssClass="custom-typography" Title="Custom Font" Content="Toast with custom typography" />

<style>
    .custom-typography .e-toast-title {
        font-family: 'Arial', sans-serif;
        font-size: 18px;
        font-weight: 700;
        letter-spacing: 0.5px;
    }

    .custom-typography .e-toast-message {
        font-family: 'Georgia', serif;
        font-size: 15px;
        line-height: 1.6;
    }
</style>
```

### Gradient Toast

```razor
<SfToast CssClass="gradient-toast" Title="Gradient" Content="Beautiful gradient background" />

<style>
    .gradient-toast {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
    }

    .gradient-toast .e-toast-title,
    .gradient-toast .e-toast-message {
        color: white;
    }
</style>
```

### Toast Icon Styling

```razor
<SfToast Icon="e-custom-icon" CssClass="custom-icon-toast" Title="Custom Icon" Content="With styled icon" />

<style>
    .custom-icon-toast .e-toast-icon {
        font-size: 28px;
        color: #FF5722;
    }

    .custom-icon-toast .e-toast-icon::before {
        content: "🎉";
    }
</style>
```

### Positioning Offset

```razor
<SfToast CssClass="offset-toast" 
         Position="new ToastPosition { X = "Right", Y = "Top" }" 
         Title="Offset" 
         Content="Custom positioning" />

<style>
    .offset-toast {
        margin: 20px;
    }
</style>
```

---

## Message Styling

### Custom Message Colors

```razor
<SfMessage CssClass="custom-message-blue" ShowIcon="true">
    Custom blue message
</SfMessage>

<style>
    .custom-message-blue {
        background-color: #E3F2FD;
        border-left: 4px solid #2196F3;
        color: #1565C0;
    }

    .custom-message-blue .e-msg-icon {
        color: #2196F3;
    }
</style>
```

### Message with Custom Icon

```razor
<SfMessage CssClass="custom-icon-message" ShowIcon="true">
    Message with custom icon
</SfMessage>

<style>
    .custom-icon-message .e-msg-icon::before {
        content: "⭐";
        font-size: 24px;
    }
</style>
```

### Message Border Styles

```razor
<SfMessage CssClass="border-message" ShowIcon="true">
    Message with custom border
</SfMessage>

<style>
    .border-message {
        border: 2px dashed #FF9800;
        border-radius: 8px;
        padding: 16px;
    }
</style>
```

### Message Spacing

```razor
<SfMessage CssClass="spaced-message" ShowIcon="true">
    Message with custom spacing
</SfMessage>

<style>
    .spaced-message {
        padding: 20px;
        margin: 20px 0;
        line-height: 1.8;
    }

    .spaced-message .e-msg-icon {
        margin-right: 16px;
    }
</style>
```

### Severity-Specific Styling

```razor
<style>
    /* Custom Error Message */
    .e-message.e-error {
        background-color: #FFEBEE;
        border-left: 5px solid #F44336;
    }

    .e-message.e-error .e-msg-icon {
        color: #F44336;
        font-size: 24px;
    }

    /* Custom Success Message */
    .e-message.e-success {
        background-color: #E8F5E9;
        border-left: 5px solid #4CAF50;
    }

    .e-message.e-success .e-msg-icon {
        color: #4CAF50;
    }

    /* Custom Warning Message */
    .e-message.e-warning {
        background-color: #FFF3E0;
        border-left: 5px solid #FF9800;
    }

    .e-message.e-warning .e-msg-icon {
        color: #FF9800;
    }

    /* Custom Info Message */
    .e-message.e-info {
        background-color: #E1F5FE;
        border-left: 5px solid #03A9F4;
    }

    .e-message.e-info .e-msg-icon {
        color: #03A9F4;
    }
</style>
```

---

## Skeleton Styling

### Custom Skeleton Colors

```razor
<SfSkeleton CssClass="custom-skeleton-blue" Width="100%" Height="100px" />

<style>
    .custom-skeleton-blue {
        background: linear-gradient(90deg, #E3F2FD 0%, #BBDEFB 50%, #E3F2FD 100%);
        background-size: 200% 100%;
    }
</style>
```

### Skeleton Animation Speed

```razor
<SfSkeleton CssClass="slow-skeleton" Width="100%" Height="100px" />
<SfSkeleton CssClass="fast-skeleton" Width="100%" Height="100px" />

<style>
    .slow-skeleton {
        animation-duration: 3s !important;
    }

    .fast-skeleton {
        animation-duration: 0.8s !important;
    }
</style>
```

### Skeleton Border Radius

```razor
<SfSkeleton CssClass="rounded-skeleton" Width="100%" Height="100px" />

<style>
    .rounded-skeleton {
        border-radius: 16px;
    }
</style>
```

### Custom Wave Effect

```razor
<SfSkeleton Effect="ShimmerEffect.Wave" CssClass="custom-wave" Width="100%" Height="100px" />

<style>
    .custom-wave {
        background: linear-gradient(
            90deg,
            #f0f0f0 0%,
            #e0e0e0 20%,
            #f0f0f0 40%,
            #f0f0f0 100%
        );
        background-size: 200% 100%;
        animation-duration: 1.5s;
    }
</style>
```

### Skeleton Opacity

```razor
<SfSkeleton CssClass="subtle-skeleton" Width="100%" Height="100px" />

<style>
    .subtle-skeleton {
        opacity: 0.3;
    }
</style>
```

---

## Dark Mode Support

### Auto Dark Mode Detection

```razor
@code {
    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            var isDarkMode = await JSRuntime.InvokeAsync<bool>("matchMedia", "(prefers-color-scheme: dark)").matches;
            if (isDarkMode)
            {
                await ApplyDarkTheme();
            }
        }
    }
}
```

### Dark Mode Toast Styling

```razor
<style>
    @media (prefers-color-scheme: dark) {
        .e-toast {
            background-color: #2C2C2C;
            color: #E0E0E0;
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.5);
        }

        .e-toast.e-toast-success {
            background-color: #1B5E20;
            border-left: 4px solid #4CAF50;
        }

        .e-toast.e-toast-error {
            background-color: #B71C1C;
            border-left: 4px solid #F44336;
        }

        .e-toast.e-toast-warning {
            background-color: #E65100;
            border-left: 4px solid #FF9800;
        }

        .e-toast.e-toast-info {
            background-color: #01579B;
            border-left: 4px solid #03A9F4;
        }
    }
</style>
```

### Dark Mode Message Styling

```razor
<style>
    @media (prefers-color-scheme: dark) {
        .e-message {
            background-color: #2C2C2C;
            color: #E0E0E0;
        }

        .e-message.e-success {
            background-color: #1B5E20;
            border-color: #4CAF50;
        }

        .e-message.e-error {
            background-color: #B71C1C;
            border-color: #F44336;
        }

        .e-message.e-warning {
            background-color: #E65100;
            border-color: #FF9800;
        }

        .e-message.e-info {
            background-color: #01579B;
            border-color: #03A9F4;
        }
    }
</style>
```

### Dark Mode Skeleton Styling

```razor
<style>
    @media (prefers-color-scheme: dark) {
        .e-skeleton {
            background: linear-gradient(90deg, #2C2C2C 0%, #3C3C3C 50%, #2C2C2C 100%);
            background-size: 200% 100%;
        }

        .e-skeleton-wave {
            background: linear-gradient(90deg, #2C2C2C 0%, #4C4C4C 50%, #2C2C2C 100%);
            background-size: 200% 100%;
        }
    }
</style>
```

---

## Responsive Design

### Mobile-Optimized Toast

```razor
<SfToast CssClass="responsive-toast" 
         Position="new ToastPosition { X = "Center", Y = "Bottom" }" 
         Title="Mobile Optimized" 
         Content="Adjusts to screen size" />

<style>
    .responsive-toast {
        width: 90%;
        max-width: 500px;
    }

    @media (max-width: 768px) {
        .responsive-toast {
            width: 95%;
            font-size: 14px;
        }

        .responsive-toast .e-toast-title {
            font-size: 16px;
        }
    }

    @media (max-width: 480px) {
        .responsive-toast {
            width: 100%;
            border-radius: 0;
            margin: 0;
        }
    }
</style>
```

### Responsive Message

```razor
<SfMessage CssClass="responsive-message" ShowIcon="true">
    Responsive message adapts to screen size
</SfMessage>

<style>
    .responsive-message {
        padding: 16px;
    }

    @media (max-width: 768px) {
        .responsive-message {
            padding: 12px;
            font-size: 14px;
        }

        .responsive-message .e-msg-icon {
            font-size: 18px;
        }
    }

    @media (max-width: 480px) {
        .responsive-message {
            padding: 10px;
            font-size: 13px;
        }
    }
</style>
```

### Responsive Skeleton Grid

```razor
<div class="skeleton-grid">
    @for (int i = 0; i < 6; i++)
    {
        <div class="skeleton-item">
            <SfSkeleton Shape="SkeletonType.Rectangle" Width="100%" Height="200px" />
            <SfSkeleton Shape="SkeletonType.Text" Width="80%" />
        </div>
    }
</div>

<style>
    .skeleton-grid {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
        gap: 20px;
    }

    @media (max-width: 768px) {
        .skeleton-grid {
            grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
            gap: 15px;
        }
    }

    @media (max-width: 480px) {
        .skeleton-grid {
            grid-template-columns: 1fr;
            gap: 10px;
        }
    }
</style>
```

---

## Custom Themes

### Creating Custom Theme Variables

```css
:root {
    /* Toast Colors */
    --toast-bg: #FFFFFF;
    --toast-text: #333333;
    --toast-success: #4CAF50;
    --toast-error: #F44336;
    --toast-warning: #FF9800;
    --toast-info: #2196F3;
    
    /* Message Colors */
    --message-bg: #F5F5F5;
    --message-border: #E0E0E0;
    
    /* Skeleton Colors */
    --skeleton-bg: #E0E0E0;
    --skeleton-highlight: #F5F5F5;
}

[data-theme="dark"] {
    --toast-bg: #2C2C2C;
    --toast-text: #E0E0E0;
    --toast-success: #66BB6A;
    --toast-error: #EF5350;
    --toast-warning: #FFA726;
    --toast-info: #42A5F5;
    
    --message-bg: #2C2C2C;
    --message-border: #3C3C3C;
    
    --skeleton-bg: #2C2C2C;
    --skeleton-highlight: #3C3C3C;
}
```

### Apply Custom Theme

```css
.e-toast {
    background-color: var(--toast-bg);
    color: var(--toast-text);
}

.e-toast.e-toast-success {
    background-color: var(--toast-success);
}

.e-toast.e-toast-error {
    background-color: var(--toast-error);
}

.e-message {
    background-color: var(--message-bg);
    border-color: var(--message-border);
}

.e-skeleton {
    background: linear-gradient(
        90deg,
        var(--skeleton-bg) 0%,
        var(--skeleton-highlight) 50%,
        var(--skeleton-bg) 100%
    );
}
```

---

## Animation Customization

### Custom Toast Entrance Animation

```css
@keyframes slideInFromRight {
    from {
        transform: translateX(100%);
        opacity: 0;
    }
    to {
        transform: translateX(0);
        opacity: 1;
    }
}

.custom-animation-toast.e-toast-show {
    animation: slideInFromRight 0.5s ease-out;
}
```

### Custom Message Reveal

```css
@keyframes messageReveal {
    from {
        max-height: 0;
        opacity: 0;
        padding: 0;
    }
    to {
        max-height: 200px;
        opacity: 1;
        padding: 16px;
    }
}

.e-message {
    animation: messageReveal 0.3s ease-out;
}
```

---

## Best Practices

1. **Consistent theming** - Use the same theme across all notification components
2. **Contrast ratios** - Ensure text meets WCAG AA standards (4.5:1 minimum)
3. **Mobile-first** - Design for small screens, enhance for larger
4. **Performance** - Minimize CSS complexity, avoid heavy animations
5. **Brand alignment** - Customize colors to match brand identity
6. **Accessibility** - Test with screen readers and keyboard navigation
7. **Dark mode** - Provide dark mode alternatives for all styles
8. **Responsive typography** - Scale font sizes appropriately
9. **z-index management** - Ensure toasts appear above other content
10. **Test across themes** - Verify custom styles work with all Syncfusion themes

---

## Next Steps

- See [Use Cases and Examples](use-cases-and-examples.md) for complete implementation scenarios
- Review [Toast Features](toast-features.md) for advanced toast customization
- Explore [Message Implementation](message-implementation.md) for message styling
- Check [Skeleton Implementation](skeleton-implementation.md) for skeleton patterns
