---
name: syncfusion-blazor-security
description: Configure Content Security Policy (CSP) for Syncfusion Blazor components across Blazor Server, WebAssembly, and Auto render modes � self-hosted and CDN scenarios
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Syncfusion Blazor Security

Content Security Policy (CSP) configuration for Syncfusion Blazor components in browser-hosted applications (.NET 8, 9, 10).

## When to Use

Use this skill when you need to:
- Add a CSP `<meta>` tag to `App.razor` or `index.html` for Syncfusion components
- Configure CSP for self-hosted (Static Web Assets) script and style delivery
- Configure CSP for CDN-hosted Syncfusion scripts and styles
- Understand which CSP directives Syncfusion components require and why

## Quick Reference

- **Full CSP guide**: See [content-security-policy.md](references/content-security-policy.md)

## Required CSP Directives at a Glance

| Directive | Value | Reason |
|-----------|-------|--------|
| `font-src` | `'self' data:` | Base64-encoded icon fonts |
| `style-src` | `'self' 'unsafe-inline'` | Inline styles for sizing/positioning |
| `script-src` | `'self'` | Dynamic code evaluation (animations, etc.) |
| `connect-src` | `'self' https: wss:` | WebSocket and HTTPS connections |
| `img-src` | `data: https:` | Inline images and remote assets |
| `object-src` | `'none'` | Block plugin content |

> **CDN users**: also append `https://cdn.syncfusion.com/blazor/` to `script-src` and `style-src`.

## Where to Add the CSP Tag

| Project Type | File |
|---|---|
| Blazor Web App (.NET 8/9/10) any render mode | `~/Components/App.razor` |
| Blazor WebAssembly Standalone | `wwwroot/index.html` |

