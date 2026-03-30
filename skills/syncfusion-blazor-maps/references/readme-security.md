# 🚨 SECURITY: External GeoJSON Data Loading

## CRITICAL WARNING FOR PRODUCTION USE

**ALL code examples in this skill documentation use external GeoJSON URLs for demonstration purposes ONLY.**

### ❌ UNSAFE FOR PRODUCTION

```csharp
// ❌ DO NOT USE THIS IN PRODUCTION
<MapsLayer ShapeData='new {dataOptions = "https://cdn.syncfusion.com/maps/map-data/usa.json"}' TValue="string">
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
            "https://cdn.syncfusion.com/maps/map-data/usa.json"
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
Invoke-WebRequest -Uri "https://cdn.syncfusion.com/maps/map-data/usa.json" `
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

## Need Help?

If you're unsure about implementing secure GeoJSON loading:

1. Review the complete validation guide: [data-visualization.md](references/data-visualization.md#validating-external-geojson-data)
2. Check the security review document: [security-review](security-review.md)
3. Follow the production checklist above
4. Test with security audit tools

**Remember:** Security is not optional. Always validate external data sources before production deployment.
