# Content Security Policy (CSP) for Syncfusion Blazor Components

> **Applies to:** Syncfusion Blazor Components  
> **Framework:** .NET 8, .NET 9, .NET 10 with Blazor Web App  
> **Reference:** [Official Syncfusion Documentation](https://blazor.syncfusion.com/documentation/common/content-security-policy)

---

## Table of Contents

1. [Overview](#overview)
2. [Required CSP Directives](#required-csp-directives)
3. [Implementation](#implementation)
   - [Example: Self-Hosted Resources](#example-self-hosted-resources)
   - [Example: CDN-Hosted Resources](#example-cdn-hosted-resources)

---

## Overview

Content Security Policy (CSP) is a browser security feature that helps protect against cross-site scripting (XSS) and data injection by limiting the allowed sources for scripts, styles, images, fonts, and other resources.

When enforcing a strict CSP, some browser features are blocked by default. To use Syncfusion Blazor components under a strict CSP, you must include specific directives in the CSP policy to ensure required runtime behaviors continue to work.

## Required CSP Directives

Include the following directives in the CSP policy for Syncfusion Blazor components:

- **`font-src data:`** – Allows base64-encoded font icons.
- **`style-src 'self' 'unsafe-inline'`** – Permits inline styles. Some components use inline styling for sizing, positioning, and dynamic UI behavior.
- **`connect-src 'self' https: wss:`** – Enables WebSockets and HTTPS connections.
- **`script-src 'self'`** – Permits dynamic code evaluation required by certain features (for example, animation logic).

## Implementation

### Basic CSP Configuration

These directives should be included in the `<head>` tag of your application's webpage:

**For .NET 8, 9, and 10 Blazor Web Apps** (any render mode: Server, WebAssembly, or Auto):
- Add to the `<head>` of `~/Components/App.razor`

**For Blazor WebAssembly Standalone Apps**:
- Add to the `<head>` of `wwwroot/index.html`

**NOTE** If the `App.razor` or `index.html` contains `<ImportMap />` or `<script type="importmap"></script>` then remove that and don't consider that.

### Example: Self-Hosted Resources

```html
<head>
    ...
    <meta http-equiv="Content-Security-Policy"
        content="base-uri 'self';
        default-src 'self';
        connect-src 'self' https: wss:;
        img-src data: https:;
        object-src 'none';
        script-src 'self';
        style-src 'self';
        font-src 'self' data:;
        upgrade-insecure-requests;">
    ...
</head>
```

### Example: CDN-Hosted Resources

If referencing scripts and styles from a CDN (such as Syncfusion CDN), add the CDN domain to the CSP policy under `script-src` and `style-src`:

```html
<head>
    ...
    <meta http-equiv="Content-Security-Policy"
        content="base-uri 'self';
        default-src 'self';
        connect-src 'self' https: wss:;
        img-src data: https:;
        object-src 'none';
        script-src 'self' https://cdn.syncfusion.com/blazor/;
        style-src 'self' https://cdn.syncfusion.com/blazor/;
        font-src 'self' data:;
        upgrade-insecure-requests;">
    ...
</head>
```



