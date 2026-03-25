# Styling and Theming

## Table of Contents
- [Available Themes](#available-themes)
- [Theme Stylesheet Setup](#theme-stylesheet-setup)
- [CSS Class Customization](#css-class-customization)
- [CSS Variables](#css-variables)
- [Custom Styling Examples](#custom-styling-examples)
- [Tab Header Styling](#tab-header-styling)
- [Tab Content Styling](#tab-content-styling)

## Available Themes

Syncfusion provides multiple pre-built themes that you can use directly:

| Theme | File | Use Case |
|-------|------|----------|
| **Bootstrap5** | bootstrap5.css | Default, clean, modern design |
| **Material** | material.css | Material Design specifications |
| **Fluent** | fluent.css | Microsoft Fluent Design System |
| **Tailwind** | tailwind.css | Tailwind CSS based styling |
| **Bootstrap4** | bootstrap4.css | Legacy Bootstrap 4 support |
| **Material Dark** | material-dark.css | Material Design dark theme |
| **Fluent Dark** | fluent-dark.css | Fluent Design dark theme |
| **Tailwind Dark** | tailwind-dark.css | Tailwind CSS dark theme |

## Theme Stylesheet Setup

Add the desired theme stylesheet to your **~/index.html** file within the `<head>` section:

### Bootstrap5 Theme (Default)

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

### Material Design Theme

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

### Fluent Theme

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

### Dark Theme

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5-dark.css" rel="stylesheet" />
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js" type="text/javascript"></script>
</head>
```

### Switching Themes Dynamically

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <!-- Tab items -->
</SfTab>

<!-- Dynamically change theme -->
<button @onclick="ChangeTheme">Switch Theme</button>

@code {
    private IJSRuntime JS;
    
    private async Task ChangeTheme()
    {
        // Remove current theme
        await JS.InvokeVoidAsync("eval", 
            "document.querySelector('link[href*=\"bootstrap5.css\"]').remove()");
        
        // Add new theme
        var link = new
        {
            rel = "stylesheet",
            href = "_content/Syncfusion.Blazor.Themes/material.css"
        };
        await JS.InvokeVoidAsync("eval",
            $"var link = document.createElement('link'); link.rel='stylesheet'; link.href='{link.href}'; document.head.appendChild(link);");
    }
    
    @inject IJSRuntime JS;
}
```

## CSS Class Customization

Override default styles using CSS classes. Syncfusion Tabs uses consistent class names:

### Key CSS Classes

```css
/* Tab component container */
.e-tab { }

/* Tab header container */
.e-tab-header { }

/* Individual tab header item */
.e-tab-header-item { }

/* Tab header item text */
.e-tab-header-text { }

/* Tab content container */
.e-content { }

/* Individual tab content item */
.e-tab-content { }

/* Active/selected tab item */
.e-active { }

/* Disabled tab item */
.e-disabled { }

/* Tab scrolling buttons */
.e-scrollable-prev,
.e-scrollable-next { }
```

### Custom Styling Example

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabItems>
        <TabItem Content="Custom styled content">
            <ChildContent>
                <TabHeader Text="Styled Tab"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

<style>
    /* Custom tab styling */
    .e-tab .e-tab-header-item {
        background-color: #f0f0f0;
        border-radius: 8px 8px 0 0;
        margin-right: 5px;
        padding: 10px 15px;
    }
    
    /* Active tab styling */
    .e-tab .e-tab-header-item.e-active {
        background-color: #2196F3;
        color: white;
    }
    
    /* Tab content styling */
    .e-tab .e-content .e-tab-content {
        padding: 20px;
        background-color: #ffffff;
        border: 1px solid #ddd;
    }
</style>
```

## CSS Variables

Syncfusion themes expose CSS variables for easy customization without full stylesheet overrides.

### Common CSS Variables

```css
/* Colors */
--e-tab-primary-background: #2196F3;
--e-tab-text-color: #333;
--e-tab-hover-background: #f5f5f5;
--e-tab-active-background: #2196F3;

/* Sizing */
--e-tab-header-height: 40px;
--e-tab-header-padding: 10px 15px;

/* Borders */
--e-tab-border-color: #ddd;
--e-tab-border-width: 1px;
```

### Using CSS Variables

```razor
<SfTab>
    <TabItems>
        <TabItem Content="CSS Variable styled content">
            <ChildContent>
                <TabHeader Text="Custom Tab"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

<style>
    /* Override CSS variables */
    :root {
        --e-tab-primary-background: #FF6B6B;
        --e-tab-text-color: #222;
        --e-tab-hover-background: #fff3cd;
    }
    
    .e-tab .e-tab-header-item {
        color: var(--e-tab-text-color);
        background-color: var(--e-tab-hover-background);
    }
    
    .e-tab .e-tab-header-item.e-active {
        background-color: var(--e-tab-primary-background);
        color: white;
    }
</style>
```

## Custom Styling Examples

### Example 1: Pill-Style Tabs

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabItems>
        <TabItem Content="Pill style content 1">
            <ChildContent>
                <TabHeader Text="Option 1"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Pill style content 2">
            <ChildContent>
                <TabHeader Text="Option 2"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Pill style content 3">
            <ChildContent>
                <TabHeader Text="Option 3"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

<style>
    .e-tab .e-tab-header-item {
        background-color: #e9ecef;
        border-radius: 20px;
        margin: 5px;
        padding: 8px 15px;
        border: none;
        transition: all 0.3s ease;
    }
    
    .e-tab .e-tab-header-item:hover {
        background-color: #dee2e6;
    }
    
    .e-tab .e-tab-header-item.e-active {
        background-color: #007bff;
        color: white;
    }
    
    .e-tab .e-content .e-tab-content {
        padding: 30px;
        border-radius: 0 0 8px 8px;
    }
</style>
```

### Example 2: Bordered Tabs

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabItems>
        <TabItem Content="Content with borders">
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="More bordered content">
            <ChildContent>
                <TabHeader Text="Tab 2"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

<style>
    .e-tab {
        border: 2px solid #333;
        border-radius: 8px;
        overflow: hidden;
    }
    
    .e-tab .e-tab-header {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        border-bottom: 2px solid #333;
    }
    
    .e-tab .e-tab-header-item {
        color: white;
        border-right: 1px solid rgba(255, 255, 255, 0.2);
    }
    
    .e-tab .e-tab-header-item:last-child {
        border-right: none;
    }
    
    .e-tab .e-tab-header-item.e-active {
        background: rgba(255, 255, 255, 0.2);
    }
    
    .e-tab .e-content .e-tab-content {
        padding: 20px;
        background: #f8f9fa;
    }
</style>
```

### Example 3: Icon Tabs

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabItems>
        <TabItem Content="Home dashboard content">
            <ChildContent>
                <TabHeader Text="🏠 Home"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Profile information">
            <ChildContent>
                <TabHeader Text="👤 Profile"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Settings panel">
            <ChildContent>
                <TabHeader Text="⚙️ Settings"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

<style>
    .e-tab .e-tab-header-item {
        font-size: 16px;
        padding: 12px 20px;
    }
    
    .e-tab .e-tab-header-item .e-tab-header-text {
        display: flex;
        align-items: center;
        gap: 8px;
    }
</style>
```

## Tab Header Styling

### Custom Header Appearance

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabItems>
        <TabItem Content="Content 1">
            <ChildContent>
                <TabHeader Text="Custom Header"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

<style>
    /* Header styling */
    .e-tab .e-tab-header {
        background: #f0f0f0;
        border-bottom: 3px solid #2196F3;
        padding: 10px 0;
    }
    
    /* Header item styling */
    .e-tab .e-tab-header-item {
        font-weight: 500;
        transition: all 0.3s ease;
        border-bottom: 3px solid transparent;
    }
    
    /* Active header styling */
    .e-tab .e-tab-header-item.e-active {
        border-bottom-color: #2196F3;
        color: #2196F3;
    }
    
    /* Hover state */
    .e-tab .e-tab-header-item:hover:not(.e-active) {
        background: #e0e0e0;
    }
</style>
```

## Tab Content Styling

### Custom Content Appearance

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabItems>
        <TabItem>
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div class="custom-content">
                    <h3>Custom Styled Content</h3>
                    <p>This content has custom styling applied.</p>
                </div>
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>

<style>
    /* Content container styling */
    .e-tab .e-content {
        background: white;
        border: 1px solid #ddd;
        border-radius: 0 0 8px 8px;
    }
    
    /* Individual tab content styling */
    .e-tab .e-content .e-tab-content {
        padding: 25px;
        animation: fadeIn 0.3s ease-in;
    }
    
    /* Custom content styling */
    .custom-content h3 {
        color: #2196F3;
        margin-bottom: 10px;
    }
    
    .custom-content p {
        line-height: 1.6;
        color: #666;
    }
    
    /* Animation */
    @keyframes fadeIn {
        from {
            opacity: 0;
        }
        to {
            opacity: 1;
        }
    }
</style>
```

### Scrollable Content

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabItems>
        <TabItem>
            <ChildContent>
                <TabHeader Text="Scrollable Tab"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div class="scrollable-content">
                    <p>Large amount of content here...</p>
                    <!-- Content that might overflow -->
                </div>
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>

<style>
    .scrollable-content {
        max-height: 400px;
        overflow-y: auto;
        padding: 15px;
    }
    
    /* Custom scrollbar styling */
    .scrollable-content::-webkit-scrollbar {
        width: 8px;
    }
    
    .scrollable-content::-webkit-scrollbar-track {
        background: #f1f1f1;
    }
    
    .scrollable-content::-webkit-scrollbar-thumb {
        background: #888;
        border-radius: 4px;
    }
    
    .scrollable-content::-webkit-scrollbar-thumb:hover {
        background: #555;
    }
</style>
```

## Best Practices

1. **Use built-in themes** as base rather than starting from scratch
2. **Override CSS variables** for quick theme adjustments
3. **Keep custom CSS minimal** and well-organized
4. **Test with different themes** to ensure compatibility
5. **Use consistent spacing** and sizing conventions
6. **Consider dark mode** needs in your styling
7. **Apply transitions** for smooth visual changes
8. **Test accessibility** with your custom styles (color contrast, focus states)
