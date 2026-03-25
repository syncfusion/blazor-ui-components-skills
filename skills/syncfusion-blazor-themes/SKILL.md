```skill
---
name: syncfusion-blazor-themes
description: Customize and apply Syncfusion Blazor themes — Theme Studio color customization, CSS/SCSS integration, lightweight themes, component filtering, and Sass variable overrides
metadata:
  author: "Syncfusion Inc"
  version: "1.0.0"
---

# Syncfusion Blazor Themes

Complete guidance for applying and customizing Syncfusion Blazor component themes using Theme Studio, SCSS overrides, and production-ready best practices.

## When to Use

Use this skill when you need to:
- Apply a built-in Syncfusion theme (Material 3, Bootstrap 5, Fluent, Tailwind, etc.)
- Customize theme colors interactively using Theme Studio
- Generate a filtered, minimal CSS bundle for only the components you use
- Override theme variables with Sass `@use` and CSS custom properties
- Use lightweight theme variants to reduce bundle size
- Import and re-use saved theme settings (`settings.json`)

**Do NOT use for:**
- Initial Blazor project setup → use `syncfusion-blazor-common`
- Script loading and CRG bundles → use `syncfusion-blazor-common`
- CSP configuration for stylesheets → use `syncfusion-blazor-security`

## Quick Reference

- **Full Theme Studio guide**: See [theme-appearance.md](references/theme-appearance.md)

## Available Themes at a Glance

| Theme | Description |
|-------|-------------|
| **Material 3** | Google Material Design 3 (recommended default) |
| **Bootstrap 5** | Bootstrap 5 design system |
| **Fluent / Fluent 2** | Microsoft Fluent Design System |
| **Tailwind CSS** | Tailwind CSS design tokens |
| **Bootstrap 4** | Bootstrap 4 design system |
| **Material** | Original Material Design |
| **High Contrast** | WCAG-compliant accessible theme |

> **Lightweight variants** (e.g., `fluent2-lite.css`) exclude web fonts — ideal for offline or bandwidth-constrained environments.

## Quick Setup: Add a Theme

### Blazor Web App (.NET 8+)
Add to `<head>` in `~/Components/App.razor`:
```html
<link href="_content/Syncfusion.Blazor.Themes/material3.css" rel="stylesheet" />
```

### Blazor WebAssembly Standalone
Add to `<head>` in `wwwroot/index.html`:
```html
<link href="_content/Syncfusion.Blazor.Themes/material3.css" rel="stylesheet" />
```

> ⚠️ **Important**: Only reference **one** Syncfusion base theme at a time to avoid style conflicts.

## Theme Studio Workflow

1. Open [Blazor Theme Studio](https://blazor.syncfusion.com/themestudio/?theme=material3)
2. Select a base theme and customize colors with live preview
3. Filter to include only the components used in your app (reduces CSS size)
4. Download the package — includes `.css`, `.scss`, and `settings.json`
5. Copy the CSS to `~/wwwroot/styles/` and reference it in your host page
6. Save `settings.json` to version control for future updates

## SCSS Variable Override Example

```scss
/* Custom.scss */
:root {
  --color-primary-override: 54, 34, 89;
}

@use 'material3.scss' with (
  $primary: var(#{--color-primary-override})
);
```
