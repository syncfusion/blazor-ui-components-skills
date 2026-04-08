# Localization in Blazor DataForm

Localize form validation messages, labels, and error messages for multi-language support.

## Built-in Localization Resources

Syncfusion provides default localization strings for validation messages in multiple languages:

```razor
<SfDataForm Model="@employee">
    <FormValidator>
        <DataAnnotationsValidator></DataAnnotationsValidator>
    </FormValidator>
    <FormItems>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
</SfDataForm>

@code {
    public class Employee
    {
        [Required]
        public string Name { get; set; }

        [EmailAddress]
        public string Email { get; set; }
    }

    private Employee employee = new();
}
```

Supported locales: `en`, `de`, `fr`, `es`, `pt`, `ar`, `ja`, `ko`, `zh`, `it`, `nl`, `ru`, and more.

## Supported Languages

| Language | Locale Code | Notes |
|----------|------------|-------|
| English | `en` | Default |
| German | `de` | German validation messages |
| French | `fr` | French validation messages |
| Spanish | `es` | Spanish validation messages |
| Portuguese | `pt` | Portuguese validation messages |
| Arabic | `ar` | Arabic with RTL support |
| Japanese | `ja` | Japanese validation messages |
| Korean | `ko` | Korean validation messages |
| Chinese | `zh` | Simplified Chinese |
| Italian | `it` | Italian validation messages |
| Dutch | `nl` | Dutch validation messages |
| Russian | `ru` | Russian validation messages |

## Localizing Validation Messages

### Localizing Field Labels

Use `[Display]` with `ResourceType` for label localization:

```csharp
public class EmployeeLocalized
{
    [Display(Name = nameof(Resources.FirstName), 
             ResourceType = typeof(Resources))]
    [Required(ErrorMessageResourceName = nameof(Resources.FirstNameRequired), 
              ErrorMessageResourceType = typeof(Resources))]
    public string FirstName { get; set; }

    [Display(Name = nameof(Resources.Email), 
             ResourceType = typeof(Resources))]
    [Required(ErrorMessageResourceName = nameof(Resources.EmailRequired), 
              ErrorMessageResourceType = typeof(Resources))]
    [EmailAddress(ErrorMessageResourceName = nameof(Resources.EmailInvalid), 
                  ErrorMessageResourceType = typeof(Resources))]
    public string Email { get; set; }
}
```

### Resource File Setup

Create resource files for different languages:

**Resources.en.resx** (English):
```
FirstName = First Name
Email = Email Address
FirstNameRequired = First Name is required
EmailRequired = Email is required
EmailInvalid = Invalid email format
```

**Resources.de.resx** (German):
```
FirstName = Vorname
Email = E-Mail-Adresse
FirstNameRequired = Vorname ist erforderlich
EmailRequired = E-Mail ist erforderlich
EmailInvalid = Ungültiges E-Mail-Format
```

## Custom Localization Setup

### Create Custom Localization Resource Class

```csharp
public static class LocalizationStrings
{
    public static Dictionary<string, Dictionary<string, string>> Resources = new()
    {
        {
            "en",
            new Dictionary<string, string>
            {
                { "Required", "This field is required" },
                { "Email", "Please enter a valid email" },
                { "MinLength", "Minimum {0} characters required" },
                { "MaxLength", "Maximum {0} characters allowed" },
                { "Range", "Value must be between {0} and {1}" },
                { "FirstName", "First Name" },
                { "LastName", "Last Name" },
                { "EmailAddress", "Email Address" },
                { "PhoneNumber", "Phone Number" },
                { "Age", "Age" }
            }
        },
        {
            "de",
            new Dictionary<string, string>
            {
                { "Required", "Dieses Feld ist erforderlich" },
                { "Email", "Bitte geben Sie eine gültige E-Mail ein" },
                { "MinLength", "Mindestens {0} Zeichen erforderlich" },
                { "MaxLength", "Maximal {0} Zeichen zulässig" },
                { "Range", "Der Wert muss zwischen {0} und {1} liegen" },
                { "FirstName", "Vorname" },
                { "LastName", "Nachname" },
                { "EmailAddress", "E-Mail-Adresse" },
                { "PhoneNumber", "Telefonnummer" },
                { "Age", "Alter" }
            }
        },
        {
            "es",
            new Dictionary<string, string>
            {
                { "Required", "Este campo es obligatorio" },
                { "Email", "Por favor ingrese un correo válido" },
                { "MinLength", "Se requieren mínimo {0} caracteres" },
                { "MaxLength", "Se permiten máximo {0} caracteres" },
                { "Range", "El valor debe estar entre {0} y {1}" },
                { "FirstName", "Nombre" },
                { "LastName", "Apellido" },
                { "EmailAddress", "Correo electrónico" },
                { "PhoneNumber", "Número de teléfono" },
                { "Age", "Edad" }
            }
        }
    };

    public static string GetString(string key, string culture = "en")
    {
        if (Resources.TryGetValue(culture, out var translations))
        {
            return translations.TryGetValue(key, out var value) ? value : key;
        }
        return key;
    }
}
```

### Using Custom Localization

```razor
@page "/custom-form/{culture}"
@inject NavigationManager Nav

<div class="language-selector">
    <button @onclick="() => ChangeCulture('en')">English</button>
    <button @onclick="() => ChangeCulture('de')">Deutsch</button>
    <button @onclick="() => ChangeCulture('es')">Español</button>
</div>

<SfDataForm Model="@employee">
    <FormValidator>
        <DataAnnotationsValidator></DataAnnotationsValidator>
    </FormValidator>
    <FormItems>
        <FormItem Field="@nameof(employee.FirstName)" 
                  LabelText="@GetLabel('FirstName')">
        </FormItem>
        <FormItem Field="@nameof(employee.LastName)" 
                  LabelText="@GetLabel('LastName')">
        </FormItem>
        <FormItem Field="@nameof(employee.Email)" 
                  LabelText="@GetLabel('EmailAddress')">
        </FormItem>
    </FormItems>
</SfDataForm>

@code {
    [Parameter]
    public string Culture { get; set; } = "en";

    public class Employee
    {
        [Required(ErrorMessage = "")]
        public string FirstName { get; set; }

        [Required(ErrorMessage = "")]
        public string LastName { get; set; }

        [EmailAddress(ErrorMessage = "")]
        public string Email { get; set; }
    }

    private Employee employee = new();

    private string GetLabel(string key) => LocalizationStrings.GetString(key, Culture);

    private void ChangeCulture(string culture)
    {
        Nav.NavigateTo($"/custom-form/{culture}", true);
    }
}
```

## Culture Configuration

### Set Application Culture

Configure default culture in `Program.cs`:

```csharp
var builder = WebAssemblyHostBuilder.CreateDefault(args);

// Set default culture
var culture = CultureInfo.GetCultureInfo("de");
CultureInfo.DefaultThreadCurrentCulture = culture;
CultureInfo.DefaultThreadCurrentUICulture = culture;

builder.RootComponents.Add<App>("#app");
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

### Culture from Browser Settings

```csharp
protected override async Task OnInitializedAsync()
{
    var culture = await GetBrowserCultureAsync();
    SetCulture(culture);
}

private async Task<string> GetBrowserCultureAsync()
{
    // Get culture from browser or user preference
    return "de";
}

private void SetCulture(string culture)
{
    CultureInfo.CurrentCulture = new CultureInfo(culture);
    CultureInfo.CurrentUICulture = new CultureInfo(culture);
}
```

## RTL (Right-to-Left) Support

For Arabic and other RTL languages:

```razor
<SfDataForm Model="@employee" 
            EnableRtl="true">
    <FormValidator>
        <DataAnnotationsValidator></DataAnnotationsValidator>
    </FormValidator>
    <FormItems>
        <FormAutoGenerateItems></FormAutoGenerateItems>
    </FormItems>
</SfDataForm>

@code {
    public class Employee
    {
        public string Name { get; set; }
        public string Email { get; set; }
    }

    private Employee employee = new();
}
```

RTL automatically adjusts:
- Field and label alignment
- Text direction
- Button order
- Layout flow

## Best Practices

✅ **DO:**
- Set culture at application startup
- Use resource files for maintenance
- Support RTL for Arabic/Hebrew
- Test with multiple languages
- Provide fallback English strings
- Allow user language preference selection

❌ **DON'T:**
- Hardcode validation messages
- Forget RTL support for RTL languages
- Leave error messages untranslated
- Ignore date/number format differences
- Skip testing with different cultures

## See Also

- [Validation](validation.md) - Set up validation rules
- [Getting Started](getting-started.md) - Initial setup
