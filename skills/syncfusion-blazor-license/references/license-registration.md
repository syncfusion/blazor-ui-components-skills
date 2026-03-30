# Syncfusion Blazor License Generation & Registration Guide

> **Applies to:** Syncfusion Blazor Components  
> **Framework:** .NET 8, .NET 9, .NET 10 with Blazor Web App  
> **Reference:** [Official Syncfusion Documentation](https://blazor.syncfusion.com/documentation/getting-started/license-key/overview)

**Purpose**: Complete reference guide for generating, registering, and managing Syncfusion Blazor license keys across different project types and deployment scenarios.

---

## Quick Reference Navigation

1. [Generating License Keys](#generating-license-keys)
2. [Registering License Keys in Applications](#registering-license-keys-in-applications)
3. [Registering When Consuming a Razor Class Library (RCL)](#registering-when-consuming-a-razor-class-library-rcl)
4. [Secure Registration in Blazor WASM Apps](#secure-registration-in-blazor-wasm-apps)
5. [Troubleshooting Licensing Errors](#troubleshooting-licensing-errors)
6. [Licensing FAQ](#licensing-faq)

---

## Generating License Keys

### License Portal Locations

Generate license keys from the Syncfusion account portals:

1. **Licensed Products**: [License & Downloads Portal](https://syncfusion.com/account/downloads)
2. **Trial/Evaluation**: [Trial & Downloads Portal](https://www.syncfusion.com/account/manage-trials/downloads)

### License Generation States

Depending on your account status, different keys are generated:

#### Active License
- Generated from Claim License Key page
- Valid license associated with account
- Full version key without expiration

#### Active Trial
- Trial period key with expiration date
- Valid for 30-day evaluation period
- Generated on demand from Trial & Downloads portal

#### Expired License
- Temporary 5-day validity key generated automatically
- Renew subscription for latest version
- Upgrade to full license recommended

#### No Trial or No License
- Request trial or purchase license from Claim License Key page
- Create Syncfusion account if needed
- Options: Free trial or purchase

### Important Notes on License Keys

- **Version Specific**: License keys are version-specific for the Syncfusion platform
- **Platform Specific**: Different keys for different platforms (Blazor, ASP.NET, etc.)
- **Know Your Version**: Check Which version license key should I use?
- **Key Generation Details**: See How to generate license key for licensed products

---

## Registering License Keys in Applications

### Registration Basics

**Important Requirements:**
- Register license key BEFORE any Syncfusion Blazor component is initialized
- Place license key in double quotes
- Ensure `Syncfusion.Licensing.dll` is referenced in the project
- Register during application startup
- License validation is offline - no internet required for running

### Registration Code Template

> **Security Warning (WASM):** Do NOT register license keys in client-side Blazor WebAssembly `Program.cs` or commit keys into source. Client-side assemblies are downloadable and can expose license keys to end users. Treat license keys like secrets.

```csharp
using Syncfusion.Licensing;

// Register the Syncfusion license
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR LICENSE KEY");
```

### Registration by Project Type

#### 1. Blazor Web App (Interactive Auto)

**Location**: Register in BOTH server and client projects

> **Security Warning (WASM):** Do NOT register license keys in client-side Blazor WebAssembly `Program.cs` or commit keys into source. Client-side assemblies are downloadable and can expose license keys to end users. Treat license keys like secrets.

**Server/Program.cs:**
```csharp
// Register the Syncfusion license
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR LICENSE KEY");
```

**Client/Program.cs:**
```csharp
// Register the Syncfusion license
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR LICENSE KEY");
```

#### 2. Blazor Web App (Interactive Server)

**Location**: Server project only

**Server/Program.cs (.NET 8, 9, 10):**
```csharp
var app = builder.Build();
// Register the Syncfusion license
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR LICENSE KEY");

// Configure the HTTP request pipeline.
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Home/Error");
    app.UseHsts();
}
```

#### 3. Blazor Web App (Interactive WebAssembly)

> **Security Warning (WASM):** Do NOT register license keys in client-side Blazor WebAssembly `Program.cs` or commit keys into source. Client-side assemblies are downloadable and can expose license keys to end users. Treat license keys like secrets.

**Location**: Register in BOTH server and client projects

**Server/Program.cs:**
```csharp
// Register the Syncfusion license
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR LICENSE KEY");
```

**Client/Program.cs:**
```csharp
// Register the Syncfusion license
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR LICENSE KEY");
```

#### 4. Blazor Standalone WebAssembly App

> **Security Warning (WASM):** Do NOT register license keys in client-side Blazor WebAssembly `Program.cs` or commit keys into source. Client-side assemblies are downloadable and can expose license keys to end users. Treat license keys like secrets.

**Location**: Client project only

**Program.cs:**
```csharp
// Register the Syncfusion license
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR LICENSE KEY");

var builder = WebAssemblyHostBuilder.CreateDefault(args);
// ... rest of configuration
```

### Registration Location Reference Table

| Project Type | License Location | Files to Update |
|---|---|---|
| **Blazor Web App (Interactive Auto)** | Server & Client | `Server/Program.cs`, `Client/Program.cs` |
| **Blazor Web App (Interactive Server)** | Server Only | `Server/Program.cs` |
| **Blazor Web App (Interactive WASM)** | Server & Client | `Server/Program.cs`, `Client/Program.cs` |
| **Blazor Standalone WebAssembly** | Client Only | `Program.cs` |

---

## Registering When Consuming a Razor Class Library (RCL)

### RCL Registration Strategy

When your application references a Razor Class Library (RCL) that uses Syncfusion components, register the license key in the **consuming application's** `Program.cs` — not in the RCL project itself.

**Requirement**: License key must be registered BEFORE any Syncfusion component is initialized

### By Hosting Model

#### In Blazor Web App (referencing RCL)

**Program.cs:**
```csharp
// Register the Syncfusion license
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR LICENSE KEY");
```

#### In Blazor Server App (referencing RCL)

**Program.cs (.NET 8, 9, 10):**
```csharp
var app = builder.Build();
// Register the Syncfusion license
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR LICENSE KEY");

// Configure the HTTP request pipeline.
if (!app.Environment.IsDevelopment())
{
    app.UseExceptionHandler("/Home/Error");
    app.UseHsts();
}
```

#### In Blazor WebAssembly App (referencing RCL)

**Program.cs:**
```csharp
// Register the Syncfusion license
Syncfusion.Licensing.SyncfusionLicenseProvider.RegisterLicense("YOUR LICENSE KEY");
```

---

## Secure Registration in Blazor WASM Apps

> **Security Warning (WASM):** Do NOT register license keys in client-side Blazor WebAssembly `Program.cs` or commit keys into source. Client-side assemblies are downloadable and can expose license keys to end users. Treat license keys like secrets.

### Security Concerns

**Warning**: Registering license key directly in `Program.cs` of Blazor WebAssembly client project exposes it through compiled assemblies, making it accessible in the browser.

### Recommended Solution: Licensed NuGet Packages

**BEST PRACTICE**: Use licensed NuGet packages from the web installer instead of registering keys in code.

#### Benefits of Licensed Packages
- Enhanced security - prevents license key exposure in browser
- Simplified deployment - no manual registration needed
- No browser-accessible security risks

### Implementation: Licensed NuGet Packages

#### Step 1: Download Licensed Packages

- [Web Installer How to Download](./syncfusion-blazor-web-installer-download.md)
- [Web Installer How to Install](./syncfusion-blazor-web-installer-install.md)

#### Step 2: Configure Package Source Mapping

Use NuGet.config to ensure licensed packages only:

**NuGet.config:**
```xml
<configuration>
  <packageSources>
    <add key="licensed-nuget" value="path/to/your/nuget-source" />
    <add key="nuget.org" value="https://api.nuget.org/v3/index.json" />
  </packageSources>
  <packageSourceMapping>
    <packageSource key="licensed-nuget">
      <package pattern="Syncfusion.*" />
    </packageSource>
  </packageSourceMapping>
</configuration>
```

#### Step 3: Verify Assembly Licensing

Check if you're using licensed or trial assemblies:

1. Navigate to build output directory
2. Right-click Syncfusion assembly → Properties → Details tab
3. Check File Description:
   - Contains **"LR"** = Trial version (requires registration)
   - Does NOT contain **"LR"** = Licensed version (no registration needed)

#### Step 4: Clean Build if Trial Detected

If trial assemblies found:

```bash
# Clear NuGet cache
dotnet nuget locals all --clear

# Delete bin and obj folders
# Uninstall and reinstall packages from licensed source only
```

#### Package Source Options

- **Local Folder**: Store packages locally
- **Private Repository Manager**: Use Nexus or Azure DevOps Artifacts

**Warning**: When using both local/private and nuget.org, restore may default to nuget.org trial versions, causing license issues.

### Alternative: Azure Key Vault Integration

For enhanced security in cloud environments, store license keys in Azure Key Vault:

- Retrieve at runtime
- Never exposed in browser or client-side code

---

## Troubleshooting Licensing Errors

### Error: License Key Not Registered or Trial Expired

**Message**: "This application was built using a trial version of Syncfusion® Essential Studio®. To remove the license validation message permanently, a valid license key must be included."

**Solutions:**

1. **If you have a valid Syncfusion license:**
   - Generate key from [License & Downloads](https://www.syncfusion.com/account/downloads)
   - Register in application

2. **If you have active trial:**
   - Generate key from [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)
   - Register in application

3. **If no active trial:**
   - [Purchase license](https://www.syncfusion.com/sales/products) OR
   - [Start free 30-day trial](https://www.syncfusion.com/account/manage-trials/start-trials)
   - Then generate and register key

4. **If no Syncfusion account:**
   - [Create account](https://www.syncfusion.com/account/register)
   - Purchase license OR start trial
   - Generate and register key

5. **Quick option in warning dialog:**
   - Click "Claim your FREE account" in warning message
   - See **How to Generate License Key**

### Error: Invalid Key

**Message**: "The included Syncfusion® license key is invalid."

**Causes:**
- Invalid/incorrect license key
- Different version license key than installed packages
- Different platform license key than application type
- Mismatched assembly versions

**Solutions:**
1. Verify key matches your Syncfusion version
2. Verify key platform matches (Blazor, not ASP.NET Core, etc.)
3. Ensure all Syncfusion packages are same version
4. Ensure Syncfusion.Licensing version matches other packages
5. Register key before any component initialization

### Error: Platform Mismatch

**Message**: "The included Syncfusion® license is invalid (Platform mismatch)."

**Cause**: License key is for different platform (e.g., ASP.NET Core key in Blazor app)

**Solution:**
- Generate correct platform-specific key
- Register correct key for Blazor platform

### Error: Version Mismatch

**Message**: "The included Syncfusion® license ({Registered Version}) is invalid for version {Required version}."

**Cause**: License key version doesn't match installed package version

**Solution:**
1. Identify installed package version
2. Generate license key for matching version
3. Ensure all packages same version
4. Clean bin/obj and rebuild

### Troubleshooting License Errors (General)

**If error persists after proper registration:**

1. Verify Syncfusion.Licensing NuGet package version matches other packages
2. Check all Syncfusion assemblies are same version
3. Confirm license key registered before component initialization
4. Verify same version assemblies in output and publish folders
5. After package upgrade: 
   - Clean bin/obj folders
   - Clear NuGet cache: `dotnet nuget locals all --clear`
   - Rebuild solution
6. Check referenced assemblies match license key

---

## Licensing FAQ

### Is Internet Connection Required?

**No**. 
- License validation is offline during application execution
- No internet access required to run licensed apps
- Can deploy on systems without internet connection
- Validation happens locally on the machine

### How Do I Upgrade from Trial to Paid License?

**Option 1: Replace Assemblies**
- Uninstall trial version
- Install licensed build from [License & Downloads](https://www.syncfusion.com/account/downloads) portal
- No code changes needed

**Option 2: Replace License Key (NuGet users)**
1. Generate paid license key from [License & Downloads](https://www.syncfusion.com/account/downloads)
2. Replace trial key with paid key in application code
3. See [Registration Guide](https://blazor.syncfusion.com/documentation/getting-started/license-key/how-to-register-in-an-application)

**Note**: License registration not required if using licensed installer (volume/service pack).

### Where Can I Get a License Key?

Generate from Syncfusion account portals:
- [License & Downloads Portal](https://syncfusion.com/account/downloads) - For licenses
- [Trial & Downloads Portal](https://www.syncfusion.com/account/manage-trials/downloads) - For trials

**Important:**
- Keys are version-specific - verify correct version
- Keys are platform-specific - verify correct platform
- See [Which version should I use?](https://www.syncfusion.com/kb/8951/which-version-syncfusion-license-key-should-i-use-in-my-application)

### How Do I Register a Syncfusion Account for NuGet.org Users?

If using packages directly from NuGet.org without account:

1. [Create Syncfusion account](https://www.syncfusion.com/account/register)
2. Go to [Start Trials](https://syncfusion.com/account/manage-trials/start-trials) and start trial
3. Go to [Trial & Downloads](https://www.syncfusion.com/account/manage-trials/downloads)
4. Generate trial license key
5. Register in application

