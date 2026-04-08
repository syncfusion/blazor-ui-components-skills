# 🚨 SECURITY: External GeoJSON Data Loading

## CRITICAL WARNING FOR PRODUCTION USE

**ALL code examples in this skill documentation use external GeoJSON URLs for demonstration purposes ONLY.**

### ❌ UNSAFE FOR PRODUCTION

```csharp
// ❌ DO NOT USE THIS IN PRODUCTION
<MapsLayer ShapeData='new {dataOptions = "SERVER_VALIDATED_GEOJSON_URL"}' TValue="string">
```

**Why this is unsafe:**
- Loads data from external CDN without validation
- Vulnerable to CDN compromise
- Susceptible to man-in-the-middle attacks
- No GeoJSON structure validation
- Could inject malicious geographic data

### ✅ SECURE FOR PRODUCTION

```csharp
// ✅ PRODUCTION-READY: Load from local cached file
<MapsLayer ShapeData='new {dataOptions = "/data/usa-map.json"}' TValue="string">
```

**OR with validation:**

```csharp
@code {
    private object ValidatedGeoJson;
    
    protected override async Task OnInitializedAsync()
    {
        // Load with validation
        ValidatedGeoJson = await LoadSecureGeoJson(
            "SERVER_VALIDATED_GEOJSON_URL"
        );
    }
    
    private async Task<object> LoadSecureGeoJson(string url)
    {
        // 1. Validate URL format
        if (!Uri.TryCreate(url, UriKind.Absolute, out var uri))
            throw new SecurityException("Invalid URL");
            
        // 2. Enforce HTTPS
        if (uri.Scheme != "https")
            throw new SecurityException("HTTPS required");
            
        // 3. Check trusted domains
        var trusted = new[] { "cdn.syncfusion.com", "yourdomain.com" };
        if (!trusted.Any(d => uri.Host.EndsWith(d)))
            throw new SecurityException($"Untrusted: {uri.Host}");
            
        // 4. Fetch and validate
        var data = await Http.GetStringAsync(url);
        return ValidateGeoJsonStructure(data);
    }
}
```

## Quick Security Checklist

Before deploying to production, ensure:

- [ ] **NOT loading GeoJSON directly from external URLs**
- [ ] **GeoJSON files cached locally in `wwwroot/data/`**
- [ ] **If loading externally, URLs are validated against allow-list**
- [ ] **HTTPS enforced for all external requests**
- [ ] **GeoJSON structure validated before rendering**
- [ ] **Content Security Policy headers configured**
- [ ] **No hardcoded API keys in code**
- [ ] **External HTML content (tooltips/annotations) sanitized**

## Production Deployment Steps

### Step 1: Download and Bundle GeoJSON Files

```powershell
# Download GeoJSON files during build
New-Item -Path "wwwroot/data" -ItemType Directory -Force
Invoke-WebRequest -Uri "SERVER_VALIDATED_GEOJSON_URL" `
                  -OutFile "wwwroot/data/usa-map.json"
```

### Step 2: Update Code to Use Local Files

```csharp
// Change from:
ShapeData='new {dataOptions = "https://cdn.syncfusion.com/maps/map-data/usa.json"}'

// To:
ShapeData='new {dataOptions = "/data/usa-map.json"}'
```

### Step 3: Configure Content Security Policy

```csharp
// Program.cs
app.Use(async (context, next) =>
{
    context.Response.Headers.Add("Content-Security-Policy",
        "default-src 'self'; " +
        "connect-src 'self'; " + // No external data sources
        "img-src 'self' data: https://tile.openstreetmap.org;");
    await next();
});
```

## Documentation References

- **Complete Security Guidance:** [security-review](security-review.md)
- **GeoJSON Validation:** [data-visualization.md#validating-external-geojson-data](references/data-visualization.md#validating-external-geojson-data)
- **API Key Security:** [map-providers.md#security-best-practices](references/map-providers.md#security-best-practices)
- **Content Sanitization:** [user-interactions.md#sanitizing-external-content](references/user-interactions.md#sanitizing-external-content)

## Development vs Production

| Aspect | Development | Production |
|--------|-------------|------------|
| **GeoJSON Source** | External CDN URLs | Local bundled files |
| **Validation** | Optional | **MANDATORY** |
| **HTTPS** | Recommended | **REQUIRED** |
| **Domain Allow-list** | Optional | **REQUIRED** |
| **CSP Headers** | Optional | **REQUIRED** |
| **API Keys** | appsettings.json | Azure Key Vault/Secrets |

## Service Implementation Templates

### Template 1: GeoJson Validator Service
```csharp
using System;
using System.Collections.Generic;
using System.Linq;

public class GeoJsonValidator
{
    private static readonly string[] SuspiciousPatterns = new[]
    {
        "system:", "ignore", "bypass", "execute", "admin:",
        "[system]", "[command]", "do not", "javascript:"
    };
    
    private static readonly string[] TrustedDomains = new[]
    {
        "yourdomain.com", "api.yourdomain.com"
    };

    public bool ValidateCoordinates(double latitude, double longitude)
    {
        return latitude >= -90 && latitude <= 90 &&
               longitude >= -180 && longitude <= 180;
    }

    public bool ContainsSuspiciousPatterns(string content)
    {
        if (string.IsNullOrEmpty(content))
            return false;
        return SuspiciousPatterns.Any(p => content.ToLower().Contains(p));
    }

    public bool IsValidDomain(string hostname)
    {
        return TrustedDomains.Any(d => hostname.EndsWith(d));
    }
}
```

### Template 2: Content Sanitizer Service
```bash
# Install NuGet package
dotnet add package HtmlSanitizer
```

```csharp
using HtmlSanitizer;

public class ContentSanitizer
{
    private readonly HtmlSanitizer.HtmlSanitizer _sanitizer;

    public ContentSanitizer()
    {
        _sanitizer = new HtmlSanitizer.HtmlSanitizer();
        _sanitizer.AllowedTags.Clear();
        _sanitizer.AllowedTags.Add("b");
        _sanitizer.AllowedTags.Add("i");
        _sanitizer.AllowedTags.Add("strong");
        _sanitizer.AllowedAttributes.Clear();
    }

    public string SanitizeHtml(string unsafeContent)
    {
        if (string.IsNullOrEmpty(unsafeContent))
            return "";
        try
        {
            return _sanitizer.Sanitize(unsafeContent);
        }
        catch
        {
            return System.Net.WebUtility.HtmlEncode(unsafeContent);
        }
    }
}
```

## Register Services

Add to `Program.cs`:

```csharp
builder.Services.AddScoped<GeoJsonValidator>();
builder.Services.AddScoped<ContentSanitizer>();
```

## Content Security Policy

Add to `Program.cs`:

```csharp
app.Use(async (context, next) =>
{
    context.Response.Headers["Content-Security-Policy"] = 
        "default-src 'self'; " +
        "img-src 'self' data: https://tile.openstreetmap.org; " +
        "style-src 'self' 'unsafe-inline' _content/Syncfusion.Blazor/; " +
        "script-src 'self' 'unsafe-inline' 'unsafe-eval'; " +
        "connect-src 'self'; " +
        "base-uri 'self'; " +
        "form-action 'self'";
    await next();
});
```

## Need Help?

If you're unsure about implementing secure GeoJSON loading:

1. Review the complete validation guide: [data-visualization.md](references/data-visualization.md#validating-external-geojson-data)
2. Check the security review document: [security-review](security-review.md)
3. Follow the production checklist above
4. Test with security audit tools

**Remember:** Security is not optional. Always validate external data sources before production deployment.
