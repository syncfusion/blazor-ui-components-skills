---
name: syncfusion-blazor-license
description: Generate, register, secure, validate, and troubleshoot Syncfusion Blazor license keys across all Blazor project types
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Syncfusion Blazor License Configuration

Manage Syncfusion Blazor license keys across all Blazor project types - Server, WebAssembly, Auto, and Razor Class Libraries.

## When to Use

Use this skill when you need to:
- Generate a Syncfusion license key (paid, trial, or temporary)
- Register a license key in any Blazor project type
- Secure license keys in Blazor WebAssembly projects (avoid browser exposure)
- Validate licenses in CI/CD pipelines (Azure Pipelines, GitHub Actions, Jenkins)
- Troubleshoot license errors — trial banner, invalid key, platform or version mismatch

**Do NOT use for:**
- Initial Blazor project setup → use `syncfusion-blazor-common`

## Key Rules

- **Version Specific**: License keys are version-specific for the Syncfusion platform
- **Platform Specific**: Different keys for different platforms (Blazor, ASP.NET, etc.)
- **WASM Security**: Never register license keys in client-side Blazor WebAssembly `Program.cs` — use licensed NuGet packages instead
- **Confidentiality**: License keys are secrets. Never share or commit them to source

## Common Scenarios

### Generate a license key

* [Reference: Generating License Keys](./references/license-registration#generating-license-keys)

### Register a license key

* [Reference: Registering License Keys](./references/license-registration#registering-license-keys-in-applications)

### Razor Class Library with Syncfusion Blazor components

* [Reference: Registering in RCLs](./references/license-registration#registering-when-consuming-a-razor-class-library-rcl)

### Blazor WebAssembly app (security concern)

> **Security Warning (WASM):** Do NOT register license keys in client-side Blazor WebAssembly `Program.cs` or commit keys into source. Client-side assemblies are downloadable and can expose license keys to end users. Treat license keys like secrets.

⚠️ **Do NOT register keys in client-side code.** See secure deployment options:

* [Reference: Secure Registration in WASM Apps](./references/license-registration#secure-registration-in-blazor-wasm-apps)

Recommended: Use licensed NuGet packages instead

* [Reference: Web Installer - How to Download](./references/syncfusion-blazor-web-installer-download)
* [Reference: Web Installer - How to Install](./references/syncfusion-blazor-web-installer-install)

### Troubleshooting license errors

* [Reference:  Troubleshooting Licensing Errors](./references/license-registration#troubleshooting-licensing-errors)

### FAQ

* [Licensing FAQ](./references/license-registration#licensing-faq)

