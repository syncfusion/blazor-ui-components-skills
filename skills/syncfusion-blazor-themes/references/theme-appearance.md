# Theme Studio for Syncfusion Blazor Components

> **Applies to:** Syncfusion Blazor Components  
> **Framework:** .NET 8+ with Blazor  
> **Reference:** [Official Syncfusion Documentation](https://blazor.syncfusion.com/documentation/appearance/theme-studio)

---

## Overview

Theme Studio is a web-based tool for customizing Syncfusion Blazor component themes. It allows you to:
- Customize theme colors using an interactive visual editor
- Filter and apply themes to specific components
- Download customized themes in CSS/SCSS format
- Import and manage previous theme settings

**Note:** Theme Studio does not support data visualization controls such as Chart, Diagram, Gauge, Range Navigator, and Maps.

---

## Table of Contents

1. [Customize Theme Colors](#customize-theme-colors)
2. [Filter Specific Components](#filter-specific-components)
3. [Download Customized Theme](#download-customized-theme)
4. [Use Customized Theme in Web Application](#use-customized-theme-in-web-application)
5. [Import Previous Settings](#import-previous-settings)
6. [Common Theme Variables](#common-theme-variables)
7. [Override Theme Variables with Sass](#override-theme-variables-with-sass)

---

## Customize Theme Colors

### Access Theme Studio

1. Open [Blazor Theme Studio](https://blazor.syncfusion.com/themestudio/?theme=material3) application
2. The Theme Studio application page is divided into two sections:
   - **Left side:** Controls preview section showing sample components
   - **Right side:** Theme customization section with color pickers

### Customize Colors

1. Click the color pickers in the theme customization section
2. Select your desired colors from the color picker
3. Syncfusion Blazor components in the preview section render with the newly selected colors in real-time

### Available Themes

Theme Studio supports the following base themes:
- **Material 3** — Modern Material Design 3 theme
- **Bootstrap 5** — Bootstrap 5 design system
- **Fluent** — Microsoft Fluent Design System
- **Bootstrap 4** — Bootstrap 4 design system
- **Bootstrap** — Bootstrap 3 design system
- **Material** — Material Design theme
- **Tailwind CSS** — Tailwind CSS design system
- **Microsoft Office Fabric** — Office Fabric design system
- **High Contrast** — Accessible high contrast theme

---

## Filter Specific Components

When integrating a selective list of Syncfusion Blazor components, you can filter them to reduce the final CSS file size.

### Steps to Filter Components

1. Click the **Filter icon** at the top-right corner
2. Select the controls whose theme you want to customize
3. Click the **Apply** button in the Filter dialog
4. Only the selected controls will be rendered in the preview section
5. Customize the colors for the filtered controls

### Benefits

- Reduces final CSS output file size
- Faster component loading
- Cleaner stylesheets with only necessary component styles

---

## Download Customized Theme

### Export Theme Files

1. Click the **Download button** at the top-right corner
2. The Download dialog will appear
3. Assign a theme name in the **File Name** field
4. Click the **Download** button

### Downloaded Package Contents

The download includes:
- **SCSS files** — Source stylesheets for further customization
- **CSS files** — Compiled stylesheets ready to use
- **settings.json** — Current theme configuration for future imports

### Lightweight Theme Files

Theme Studio provides lightweight theme options (e.g., `fluent2-lite.css`) that:
- Optimize performance by excluding larger styles
- Exclude web font dependencies for offline networks
- Reduce bundle size while maintaining component styling

### Important Notes

- **Material and Tailwind themes** use online Roboto font
- For offline or restricted networks, downloaded package includes CSS without Google Fonts dependencies
- When using customized CSS, **remove other Syncfusion base theme references** (CDN or local) to avoid duplicate or conflicting styles

---

## Use Customized Theme in Web Application

### Add Custom CSS to Your Application

1. Copy and paste the customized CSS file from the download folder into a folder, e.g., `~/wwwroot/styles/{file-name}.css`

2. Reference the customized CSS file in the appropriate host page:

#### For Blazor Web App (.NET 8+)

Add the link to the `<head>` of **~/Components/App.razor**:

```html
<head>
    <link href="styles/{file-name}.css" rel="stylesheet"/>
</head>
```

#### For Blazor WebAssembly App

Add the link to the `<head>` of **wwwroot/index.html**:

```html
<head>
    <link href="styles/{file-name}.css" rel="stylesheet"/>
</head>
```

#### For Blazor Server App (.NET 8+)

Add the link to the `<head>` of **~/Components/App.razor** (same as Blazor Web App):

```html
<head>
    <link href="styles/{file-name}.css" rel="stylesheet"/>
</head>
```

### Verification

After adding the custom CSS:
1. Build and run the application
2. Verify that Syncfusion Blazor components display with custom theme colors
3. Check browser dev tools to ensure custom CSS is loaded without errors

---

## Import Previous Settings

### Reuse Existing Theme Configurations

If you need to change your application theme and UI design in the future, you don't need to customize from scratch.

### Steps to Import Settings

1. Click the **Import icon** at the top-right corner
2. The Import Theme dialog will open
3. Click the **Browse** button and select the `settings.json` file you exported previously
4. Click the **Import** button
5. Stored theme settings will be reflected in the Theme Studio application

### Update and Export

1. Review the imported settings
2. Make any necessary changes to colors and component selections
3. Download the updated theme
4. Replace the older custom style with the new style in your application

---

## Common Theme Variables

### Material 3 Theme

Key color variables include:

| Variable | Light Mode | Dark Mode |
|----------|-----------|-----------|
| `--color-sf-primary` | rgb(103, 80, 164) | rgb(208, 188, 255) |
| `--color-sf-on-primary` | rgb(255, 255, 255) | rgb(55, 30, 115) |
| `--color-sf-surface` | rgb(255, 255, 255) | rgb(28, 27, 31) |
| `--color-sf-on-surface` | rgb(28, 27, 31) | rgb(230, 225, 229) |
| `--color-sf-error` | rgb(179, 38, 30) | rgb(242, 184, 181) |
| `--color-sf-success` | rgb(32, 81, 7) | rgb(83, 202, 23) |
| `--color-sf-warning` | rgb(145, 76, 0) | rgb(245, 180, 130) |
| `--color-sf-info` | rgb(1, 87, 155) | rgb(71, 172, 251) |

---

## Override Theme Variables with Sass

### Modern Approach: Using `@use` and `with()`

When using Syncfusion Blazor Theme Studio, you can override predefined theme variables without editing generated files.

### Define Custom Colors

Create or update a `Custom.scss` file with CSS custom properties:

```scss
:root {
  --color-test-primary: 124, 86, 118;
}
```

### Override Theme Variables

Import your generated Theme Studio file and override variables using `@use` with `with()`:

```scss
@use 'material3.scss' with (
  $primary: var(#{--color-test-primary})
);
```

### Why This Approach Works

✓ **`@use` system** — Modern Sass import that ensures variables are scoped and safely overridden  
✓ **`with()` keyword** — Injects custom values before Theme Studio file compiles, giving full control  
✓ **CSS custom properties** — `var(#{…})` enables passing CSS custom properties to Sass variables  
✓ **No direct edits** — Keep generated files pristine for future regeneration

---

## Best Practices

### Theme Management

1. **Version Control** — Keep `settings.json` in your repository for team consistency
2. **Component Filtering** — Use filter feature for selective component styling to reduce CSS size
3. **Lightweight Themes** — Choose lightweight variants when performance is critical
4. **Documentation** — Document custom color mappings for designer and developer alignment

### CSS Integration

1. **Single Theme Reference** — Use only one Syncfusion base theme to avoid conflicts
2. **Namespace CSS** — Place custom CSS in dedicated `styles/` folder
3. **Cache Busting** — Use cache-busting strategies if updating themes frequently
4. **Testing** — Verify custom theme across all target browsers and devices

---

## Troubleshooting

### Theme Not Applying

- **Verify file path** — Ensure CSS file path is correct and file exists
- **Clear cache** — Clear browser cache or use hard refresh (Ctrl+F5)
- **Remove conflicting styles** — Ensure no other base themes are referenced
- **Check browser console** — Look for 404 errors or CSS parse issues

### Styles Look Incorrect

- **Verify import order** — Custom CSS should be imported after base styles
- **Check selector specificity** — Ensure custom styles have appropriate specificity
- **Inspect elements** — Use browser DevTools to verify which style is applied
- **Review settings.json** — Ensure exported settings match intended customization
