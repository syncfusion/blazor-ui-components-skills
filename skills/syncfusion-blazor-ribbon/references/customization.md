# Customization and Styling

## Table of Contents
- [Theming](#theming)
- [Sizing and Layout](#sizing-and-layout)
- [CSS Customization](#css-customization)
- [RTL Support](#rtl-support)
- [Overflow Handling](#overflow-handling)
- [Responsive Design](#responsive-design)

## Theming

### Available Themes

The Ribbon component supports multiple professional themes:

```html
<!-- Bootstrap 5 (Default) -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Material Design -->
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />

<!-- Fluent UI (Microsoft) -->
<link href="_content/Syncfusion.Blazor.Themes/fluent.css" rel="stylesheet" />

<!-- Tailwind CSS -->
<link href="_content/Syncfusion.Blazor.Themes/tailwind.css" rel="stylesheet" />

<!-- Fabric Office -->
<link href="_content/Syncfusion.Blazor.Themes/fabric.css" rel="stylesheet" />
```

### Dark Mode Themes

Use dark theme variants:

```html
<!-- Bootstrap 5 Dark -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5-dark.css" rel="stylesheet" />

<!-- Material Dark -->
<link href="_content/Syncfusion.Blazor.Themes/material-dark.css" rel="stylesheet" />

<!-- Fluent Dark -->
<link href="_content/Syncfusion.Blazor.Themes/fluent-dark.css" rel="stylesheet" />
```

### Theme Selection in App

```razor
@page "/theme-selector"
@using Syncfusion.Blazor.Ribbon

<div style="margin-bottom: 20px;">
    <label>Select Theme:</label>
    <select @onchange="@ChangeTheme">
        <option value="bootstrap5">Bootstrap 5</option>
        <option value="material">Material</option>
        <option value="fluent">Fluent</option>
        <option value="tailwind">Tailwind</option>
    </select>
</div>

<SfRibbon>
    <RibbonTabs>
        <RibbonTab HeaderText="Home">
            <RibbonGroups>
                <RibbonGroup HeaderText="Tools">
                    <RibbonCollections>
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.Button">
                                    <RibbonButtonSettings Content="Action"></RibbonButtonSettings>
                                </RibbonItem>
                            </RibbonItems>
                        </RibbonCollection>
                    </RibbonCollections>
                </RibbonGroup>
            </RibbonGroups>
        </RibbonTab>
    </RibbonTabs>
</SfRibbon>

@code {
    private void ChangeTheme(ChangeEventArgs e)
    {
        string selectedTheme = e.Value.ToString();
        // Dynamically load theme CSS (implementation depends on your setup)
        Console.WriteLine($"Theme changed to: {selectedTheme}");
    }
}
```

## Sizing and Layout

### Item Sizing Options

Control visual prominence with size:

```razor
<RibbonGroup HeaderText="Emphasis">
    <RibbonCollections>
        <!-- Large: Primary action -->
        <RibbonCollection>
            <RibbonItems>
                <RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Large">
                    <RibbonButtonSettings Content="Publish" IconCss="e-icons e-publish">
                    </RibbonButtonSettings>
                </RibbonItem>
            </RibbonItems>
        </RibbonCollection>

        <!-- Small: Secondary actions -->
        <RibbonCollection>
            <RibbonItems>
                <RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Small">
                    <RibbonButtonSettings Content="Archive" IconCss="e-icons e-archive">
                    </RibbonButtonSettings>
                </RibbonItem>
                <RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Small">
                    <RibbonButtonSettings Content="Delete" IconCss="e-icons e-delete">
                    </RibbonButtonSettings>
                </RibbonItem>
            </RibbonItems>
        </RibbonCollection>
    </RibbonCollections>
</RibbonGroup>
```

### Group Orientation

**Row (Horizontal)** - Items side-by-side:

```razor
<RibbonGroup HeaderText="Clipboard" Orientation="Orientation.Row">
    <!-- Items arranged horizontally -->
</RibbonGroup>
```

**Column (Vertical)** - Items stacked:

```razor
<RibbonGroup HeaderText="View Options" Orientation="Orientation.Column">
    <!-- Items arranged vertically -->
</RibbonGroup>
```

## CSS Customization

### Custom CSS Classes

Apply custom styles to items:

```razor
<RibbonItem Type="RibbonItemType.Button">
    <RibbonButtonSettings Content="Custom Style" CssClass="custom-button">
    </RibbonButtonSettings>
</RibbonItem>

<style>
    .custom-button {
        background-color: #FF6B6B !important;
        color: white !important;
    }

    .custom-button:hover {
        background-color: #EE5A52 !important;
    }
</style>
```

### Styling Ribbon Groups

```razor
<RibbonGroup HeaderText="Important" CssClass="important-group">
    <!-- Group items -->
</RibbonGroup>

<style>
    .important-group {
        border-left: 4px solid #FF6B6B;
        background-color: #FFF5F5;
    }

    .important-group::after {
        content: "⚠";
        margin-left: 8px;
    }
</style>
```

### Custom Ribbon Width

```razor
<div style="width: 100%; max-width: 1400px; margin: 0 auto;">
    <SfRibbon style="width: 100%;">
        <!-- Ribbon content -->
    </SfRibbon>
</div>
```

### Icon Customization

```razor
<RibbonItem Type="RibbonItemType.Button">
    <RibbonButtonSettings Content="Custom Icon" 
                          IconCss="e-icons e-custom custom-icon-style">
    </RibbonButtonSettings>
</RibbonItem>

<style>
    .custom-icon-style {
        font-size: 20px !important;
        color: #007bff !important;
    }
</style>
```

## RTL Support

### Enable RTL

Apply RTL direction to the ribbon:

```razor
<SfRibbon style="direction: rtl;">
    <RibbonTabs>
        <RibbonTab HeaderText="الرئيسية">
            <!-- Content in RTL -->
        </RibbonTab>
    </RibbonTabs>
</SfRibbon>
```

### RTL Styling

```html
<html dir="rtl" lang="ar">
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
<body>
    <div id="app"></div>
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
</body>
</html>
```

## Overflow Handling

### Enable Group Overflow

Allow items to overflow to popup when space is limited:

```razor
<RibbonGroup HeaderText="Formatting" 
              Orientation="Orientation.Row"
              EnableGroupOverflow="true"
              PopupHeaderText="More Options">
    <RibbonCollections>
        <RibbonCollection>
            <RibbonItems>
                <RibbonItem Type="RibbonItemType.Button" DisplayOptions="DisplayMode.Classic">
                    <RibbonButtonSettings Content="Bold" IconCss="e-icons e-bold">
                    </RibbonButtonSettings>
                </RibbonItem>
                <!-- More items here will overflow -->
            </RibbonItems>
        </RibbonCollection>
    </RibbonCollections>
</RibbonGroup>
```

### DisplayOptions for Overflow

```razor
<!-- Always visible -->
<RibbonItem Type="RibbonItemType.Button" DisplayOptions="DisplayMode.Auto">
    <RibbonButtonSettings Content="Save"></RibbonButtonSettings>
</RibbonItem>

<!-- Only in classic layout -->
<RibbonItem Type="RibbonItemType.Button" DisplayOptions="DisplayMode.Classic">
    <RibbonButtonSettings Content="Options"></RibbonButtonSettings>
</RibbonItem>

<!-- Only in overflow -->
<RibbonItem Type="RibbonItemType.Button" DisplayOptions="DisplayMode.Overflow">
    <RibbonButtonSettings Content="Advanced"></RibbonButtonSettings>
</RibbonItem>
```

## Responsive Design

### Mobile-Friendly Ribbon

```razor
@page "/responsive-ribbon"
@using Syncfusion.Blazor.Ribbon
@using Syncfusion.Blazor.SplitButtons

<style>
    @media (max-width: 768px) {
        .ribbon-container {
            width: 100%;
        }

        .ribbon-group {
            min-width: 100%;
        }
    }
</style>

<div class="ribbon-container">
    <SfRibbon>
        <RibbonTabs>
            <RibbonTab HeaderText="Edit">
                <RibbonGroups>
                    <!-- Desktop: Horizontal layout -->
                    <RibbonGroup HeaderText="Clipboard" 
                                Orientation="Orientation.Row"
                                CssClass="desktop-group">
                        <RibbonCollections>
                            <RibbonCollection>
                                <RibbonItems>
                                    <RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Large">
                                        <RibbonButtonSettings Content="Paste" IconCss="e-icons e-paste">
                                        </RibbonButtonSettings>
                                    </RibbonItem>
                                </RibbonItems>
                            </RibbonCollection>
                        </RibbonCollections>
                    </RibbonGroup>

                    <!-- Mobile: Vertical layout -->
                    <RibbonGroup HeaderText="Clipboard"
                                Orientation="Orientation.Column"
                                CssClass="mobile-group">
                        <RibbonCollections>
                            <RibbonCollection>
                                <RibbonItems>
                                    <RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Small">
                                        <RibbonButtonSettings Content="Paste" IconCss="e-icons e-paste">
                                        </RibbonButtonSettings>
                                    </RibbonItem>
                                </RibbonItems>
                            </RibbonCollection>
                        </RibbonCollections>
                    </RibbonGroup>
                </RibbonGroups>
            </RibbonTab>
        </RibbonTabs>
    </SfRibbon>
</div>

<style>
    @media (min-width: 769px) {
        .mobile-group {
            display: none;
        }
    }

    @media (max-width: 768px) {
        .desktop-group {
            display: none;
        }
    }
</style>
```

### Compact Ribbon for Mobile

```razor
<SfRibbon style="font-size: 12px;">
    <RibbonTabs>
        <RibbonTab HeaderText="Home">
            <RibbonGroups>
                <RibbonGroup HeaderText="Tools" Orientation="Orientation.Row">
                    <RibbonCollections>
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.Button" AllowedSizes="RibbonItemSize.Small">
                                    <RibbonButtonSettings Content="Action" IconCss="e-icons e-action">
                                    </RibbonButtonSettings>
                                </RibbonItem>
                            </RibbonItems>
                        </RibbonCollection>
                    </RibbonCollections>
                </RibbonGroup>
            </RibbonGroups>
        </RibbonTab>
    </RibbonTabs>
</SfRibbon>
```

## Advanced Customization

### Custom Background Color

```razor
<SfRibbon style="background-color: linear-gradient(135deg, #667eea 0%, #764ba2 100%);">
    <!-- Ribbon content -->
</SfRibbon>
```

### Custom Font

```razor
<SfRibbon style="font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif; font-size: 14px;">
    <!-- Ribbon content -->
</SfRibbon>
```

### Custom Item Colors

```razor
<RibbonItem Type="RibbonItemType.Button">
    <RibbonButtonSettings Content="Success" 
                          CssClass="success-button"
                          IconCss="e-icons e-check">
    </RibbonButtonSettings>
</RibbonItem>

<style>
    .success-button {
        background-color: #28a745 !important;
        color: white !important;
        border-color: #28a745 !important;
    }

    .success-button:hover {
        background-color: #218838 !important;
        border-color: #218838 !important;
    }
</style>
```

## Best Practices

### 1. Use Themes Consistently
Pick one theme and use throughout the application for professional appearance.

### 2. Responsive Sizing
Adjust `AllowedSizes` based on screen size to maintain usability on mobile.

### 3. Color Contrast
Ensure sufficient contrast for accessibility (WCAG AA standard).

### 4. Icon Clarity
Use clear, recognizable icons with appropriate sizing.

### 5. RTL Consideration
Test both LTR and RTL layouts if supporting multiple languages.

### 6. Performance
Minimize CSS overrides; use theme variables when possible.

### 7. Consistency
Maintain consistent styling across all ribbons in your application.
