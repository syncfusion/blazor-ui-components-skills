---
name: syncfusion-blazor-license
description: Generate, register, secure, validate, and troubleshoot Syncfusion Blazor license keys across all Blazor project types
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Syncfusion Blazor License Configuration

Manage Syncfusion Blazor license keys across all project types — Server, WebAssembly, Auto, and Razor Class Libraries.

## When to Use

Use this skill when you need to:
- Generate a Syncfusion license key (paid, trial, or temporary)
- Register a license key in any Blazor project type
- Secure license keys in Blazor WebAssembly projects (avoid browser exposure)
- Validate licenses in CI/CD pipelines (Azure Pipelines, GitHub Actions, Jenkins)
- Troubleshoot license errors — trial banner, invalid key, platform or version mismatch

**Do NOT use for:**
- Initial Blazor project setup → use `syncfusion-blazor-common`

## Quick Reference

| Topic | Reference |
|-------|-----------|
| Generate Key | [license-registration.md — Part 1](references/license-registration.md#part-1-generating-license-keys) |
| Register Key | [license-registration.md — Part 2](references/license-registration.md#part-2-registering-license-keys-in-applications) |
| Razor Class Library | [license-registration.md — Part 3](references/license-registration.md#part-3-registering-when-consuming-a-razor-class-library-rcl) |
| WASM Security | [license-registration.md — Part 4](references/license-registration.md#part-4-secure-registration-in-blazor-wasm-apps) |
| Troubleshooting | [license-registration.md — Part 5](references/license-registration.md#part-6-troubleshooting-licensing-errors) |
| FAQ | [license-registration.md — Part 6](references/license-registration.md#part-7-licensing-faq) |

## Prerequisites

- Syncfusion account ([start a free trial](https://www.syncfusion.com/account/manage-trials/start-trials) if needed)
- Syncfusion NuGet packages installed in the project
- Access to `Program.cs` of the server and/or client project

> **Key rules**: License keys are **version-specific** and **platform-specific**. Always generate a key for the **Blazor** platform that matches your installed NuGet package version.

**NOTE**: DON'T ask user to share their license key if they encounter errors. Instead, guide them to generate a new key and register it correctly. License keys are confidential and should never be shared publicly.

## 1. Generate a License Key

**Detailed guide**: [license-registration.md — Part 1](references/license-registration.md#part-1-generating-license-keys)

| Account Status | Portal | Key Type |
|----------------|--------|----------|
| Active paid | [License & Downloads](https://syncfusion.com/account/downloads) | Full, no expiry |
| Active trial | [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads) | 30-day trial |
| Expired | [License & Downloads](https://syncfusion.com/account/downloads) | Temporary 5-day |
| No account | [Register](https://www.syncfusion.com/account/register) then [Start Trial](https://syncfusion.com/account/manage-trials/start-trials) | 30-day trial |

## 2. Register the License Key

**Detailed guide**: [license-registration.md — Part 2](references/license-registration.md#part-2-registering-license-keys-in-applications)

Register **before** any Syncfusion component initializes in `Program.cs`:

```csharp
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_LICENSE_KEY");
```

### Registration by Project Type

| Project Type | Where to Register |
|--------------|-------------------|
| Interactive Server | Server `Program.cs` only |
| Interactive Auto | Server **and** Client `Program.cs` |
| Interactive WASM | Server **and** Client `Program.cs` |
| Standalone WASM | Client `Program.cs` only |
| Razor Class Library | Consuming app's `Program.cs` |

## 3. WASM Security — Licensed NuGet Packages

**Detailed guide**: [license-registration.md — Part 4](references/license-registration.md#part-4-secure-registration-in-blazor-wasm-apps)

⚠️ **Security Issue**: Registering license keys in WASM `Program.cs` exposes them in browser-downloadable assemblies. Use licensed NuGet packages instead.

### Recommended Solution

Use licensed NuGet packages (no key registration needed):

1. Download from [web installer](https://blazor.syncfusion.com/documentation/installation/web-installer/how-to-download)
2. Configure local/private NuGet source
3. Verify DLL properties: No "LR" in file description = licensed

**If trial assemblies persist**:
```bash
dotnet nuget locals all --clear
# Delete bin/ and obj/, rebuild
```

### Alternative: Azure Key Vault

Store keys in [Azure Key Vault](https://help.syncfusion.com/common/essential-studio/licensing/licensing-faq/how-to-securely-store-and-use-syncfusion-license-keys-in-azure-key-vault) and retrieve at runtime

## 4. CI License Validation

**Detailed guide**: [license-registration.md — Part 5](references/license-registration.md#part-5-ci-license-validation)

### LicenseKeyValidator Tool

1. Download [LicenseKeyValidator.zip](https://s3.amazonaws.com/files2.syncfusion.com/Installs/LicenseKeyValidation/LicenseKeyValidator.zip)
2. Edit `LicenseKeyValidation.ps1`:
   ```powershell
   $result = & $PSScriptRoot"\LicenseKeyValidatorConsole.exe" /platform:"Blazor" /version:"26.2.4" /licensekey:"Your Key"
   Write-Host $result
   ```
3. Integrate into Azure Pipelines, GitHub Actions, or other CI systems

### Programmatic Validation

```csharp
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR_KEY");
bool isValid = SyncfusionLicenseProvider.ValidateLicense(Platform.Blazor);
```

## 5. Troubleshooting License Errors

**Detailed guide**: [license-registration.md — Part 6](references/license-registration.md#part-6-troubleshooting-licensing-errors)

| Error | Cause | Fix |
|-------|-------|-----|
| Trial banner | Key not registered | Register before component init |
| Invalid key | Wrong version/platform | Regenerate for Blazor + current NuGet version |
| Platform mismatch | Non-Blazor key | Generate Blazor-specific key |
| Version mismatch | Key ≠ package version | Clean bin/obj, clear NuGet cache, rebuild |

### Quick Checklist

1. ✓ `Syncfusion.Licensing` version matches other packages
2. ✓ Key registered **before** component initialization
3. ✓ Key platform is **Blazor**
4. ✓ Key version matches installed NuGet version
