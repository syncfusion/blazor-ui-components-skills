# Input Configuration & Formatting

## Floating Labels

Floating labels lift above the input when focused or filled. Configure with the `FloatLabelType` property:

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 350px;">
    <!-- Auto: Floats on focus or when filled -->
    <SfTextBox Placeholder="First Name" 
               FloatLabelType="@FloatLabelType.Auto">
    </SfTextBox>

    <!-- Always: Label always floated -->
    <SfTextBox Placeholder="Last Name" 
               FloatLabelType="@FloatLabelType.Always">
    </SfTextBox>

    <!-- Never: No floating effect (default) -->
    <SfTextBox Placeholder="Email" 
               FloatLabelType="@FloatLabelType.Never">
    </SfTextBox>
</div>
```

### FloatLabelType Values

| Value | Behavior | Best For |
|-------|----------|----------|
| `Never` | No animation, static placeholder | Traditional forms |
| `Auto` | Float on focus or filled | Modern, space-saving layouts |
| `Always` | Always floated above | Accessible, clear labels |

**Best Practice**: Use `FloatLabelType.Auto` for modern Material Design forms.

---

## Placeholder Text

Placeholder text provides hints when the TextBox is empty:

```razor
<SfTextBox Placeholder="Enter your email address"></SfTextBox>
<SfTextBox Placeholder="Phone (123) 456-7890"></SfTextBox>
<SfTextBox Placeholder="Search by name..."></SfTextBox>
```

### Placeholder Tips

- Keep placeholder text short (under 50 characters)
- Use examples (e.g., `name@example.com` for email)
- Do NOT use placeholder as label (accessibility issue)
- Combine with `FloatLabelType.Auto` for better UX

---

## HTML Attributes

Pass HTML attributes via the `HtmlAttributes` property:

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 300px;">
    <!-- Limit character count -->
    <SfTextBox Placeholder="Max 20 characters"
               HtmlAttributes="@MaxLengthAttrs">
    </SfTextBox>

    <!-- Type attribute for validation -->
    <SfTextBox Placeholder="Email only"
               HtmlAttributes="@EmailAttrs">
    </SfTextBox>

    <!-- Multiple attributes -->
    <SfTextBox Placeholder="Phone"
               HtmlAttributes="@PhoneAttrs">
    </SfTextBox>
</div>

@code {
    // Limit input to 20 characters
    private Dictionary<string, object> MaxLengthAttrs = new()
    {
        { "maxlength", "20" },
        { "autocomplete", "off" }
    };

    // Email input type (native browser validation)
    private Dictionary<string, object> EmailAttrs = new()
    {
        { "type", "email" },
        { "autocomplete", "email" }
    };

    // Phone input with placeholder and max length
    private Dictionary<string, object> PhoneAttrs = new()
    {
        { "type", "tel" },
        { "maxlength", "12" },
        { "inputmode", "numeric" }
    };
}
```

### Common HTML Attributes

| Attribute | Value | Purpose |
|-----------|-------|---------|
| `maxlength` | number | Limit character count |
| `minlength` | number | Require minimum characters |
| `type` | text, email, tel, url, password | Input type validation |
| `pattern` | regex | Custom validation pattern |
| `autocomplete` | on, off, email, tel | Browser autocomplete |
| `inputmode` | decimal, numeric, email, url, tel | Mobile keyboard type |
| `readonly` | (attribute) | Read-only state |
| `disabled` | (attribute) | Disabled state |
| `required` | (attribute) | Required field |

**Example: Phone Number Input**

```razor
@using Syncfusion.Blazor.Inputs

<SfTextBox @bind-Value="@Phone"
           Placeholder="(555) 123-4567"
           HtmlAttributes="@new Dictionary<string, object>
           {
               { "type", "tel" },
               { "maxlength", "14" },
               { "inputmode", "numeric" }
           }">
</SfTextBox>

@code {
    private string Phone = "";
}
```

---

## Input Masking & Formatting

Apply input formatting using patterns and CSS masks:

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 300px;">
    <!-- Phone number mask -->
    <SfTextBox @bind-Value="@Phone"
               Placeholder="(___) ___-____"
               Input="@FormatPhone"
               FloatLabelType="@FloatLabelType.Auto">
    </SfTextBox>
</div>

@code {
    private string Phone = "";

    private void FormatPhone(InputEventArgs args)
    {
        // Remove non-digits
        string digits = System.Text.RegularExpressions.Regex.Replace(args.Value, @"\D", "");
        
        // Apply phone format: (XXX) XXX-XXXX
        if (digits.Length >= 3)
        {
            Phone = $"({digits.Substring(0, 3)}) ";
            if (digits.Length >= 6)
            {
                Phone += $"{digits.Substring(3, 3)}-";
                if (digits.Length >= 10)
                {
                    Phone += digits.Substring(6, 4);
                }
                else
                {
                    Phone += digits.Substring(6);
                }
            }
            else
            {
                Phone += digits.Substring(3);
            }
        }
        else
        {
            Phone = digits;
        }
    }
}
```

### Common Format Patterns

| Format | Example | Pattern |
|--------|---------|---------|
| Phone | (555) 123-4567 | `(\d{3}) \d{3}-\d{4}` |
| Date | 03/18/2026 | `\d{2}/\d{2}/\d{4}` |
| Time | 14:30 | `\d{2}:\d{2}` |
| Credit Card | 1234-5678-9012-3456 | `\d{4}-\d{4}-\d{4}-\d{4}` |
| ZIP Code | 12345 | `\d{5}` |

---

## TextBox Groups

Combine multiple TextBox inputs into grouped controls:

```razor
@using Syncfusion.Blazor.Inputs

<style>
    .textbox-group {
        display: flex;
        gap: 10px;
        margin: 30px auto;
        width: 400px;
    }

    .group-input {
        flex: 1;
    }

    .group-label {
        font-weight: bold;
        color: #666;
        margin-bottom: 5px;
    }
</style>

<div class="textbox-group">
    <div class="group-input">
        <div class="group-label">Area Code</div>
        <SfTextBox @bind-Value="@AreaCode"
                   Placeholder="555"
                   HtmlAttributes="@new Dictionary<string, object> { { \"maxlength\", \"3\" } }">
        </SfTextBox>
    </div>

    <div class="group-input">
        <div class="group-label">Prefix</div>
        <SfTextBox @bind-Value="@Prefix"
                   Placeholder="123"
                   HtmlAttributes="@new Dictionary<string, object> { { \"maxlength\", \"3\" } }">
        </SfTextBox>
    </div>

    <div class="group-input">
        <div class="group-label">Line</div>
        <SfTextBox @bind-Value="@Line"
                   Placeholder="4567"
                   HtmlAttributes="@new Dictionary<string, object> { { \"maxlength\", \"4\" } }">
        </SfTextBox>
    </div>
</div>

<p>Full Phone: (@AreaCode) @Prefix-@Line</p>

@code {
    private string AreaCode = "";
    private string Prefix = "";
    private string Line = "";
}
```

### Grouped Input Patterns

**Address Entry (3-part)**

```razor
<div style="display: flex; gap: 10px; width: 500px;">
    <SfTextBox @bind-Value="@Street" Placeholder="Street" style="flex: 3;"></SfTextBox>
    <SfTextBox @bind-Value="@City" Placeholder="City" style="flex: 2;"></SfTextBox>
    <SfTextBox @bind-Value="@Zip" Placeholder="ZIP" style="flex: 1;"></SfTextBox>
</div>
```

**Name Entry (2-part)**

```razor
<div style="display: flex; gap: 10px; width: 400px;">
    <SfTextBox @bind-Value="@FirstName" Placeholder="First" style="flex: 1;"></SfTextBox>
    <SfTextBox @bind-Value="@LastName" Placeholder="Last" style="flex: 1;"></SfTextBox>
</div>
```

---

## Input Configuration Tips

### Best Practices

1. **Always pair with label or placeholder** - Never leave input unlabeled
2. **Use `FloatLabelType.Auto`** - Better UX than static placeholders
3. **Add `maxlength`** - Prevent excessive input
4. **Use `type` attribute** - Enable browser validation
5. **Provide visual feedback** - Use colors to indicate state

### Accessibility

```razor
<!-- Accessible TextBox with label -->
<label for="email-input">Email Address:</label>
<SfTextBox @ref="emailInput"
           Id="email-input"
           Placeholder="your@email.com"
           HtmlAttributes="@new Dictionary<string, object>
           {
               { \"aria-label\", \"Email Address\" },
               { \"type\", \"email\" }
           }">
</SfTextBox>
```

---

## Next Steps

- ✅ [Events & Interactions](../events-handling.md) - Handle input changes and formatting
- ✅ [Data Binding & Validation](../data-binding-validation.md) - Bind to data models
- ✅ [Styling & Appearance](../styling-appearance.md) - Customize with CSS
