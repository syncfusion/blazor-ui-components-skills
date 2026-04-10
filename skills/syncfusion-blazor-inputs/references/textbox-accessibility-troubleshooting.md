# Accessibility & Troubleshooting

## WCAG 2.1 Accessibility Compliance

The Syncfusion TextBox component supports WCAG 2.1 Level AA standards for accessibility:

### Key Compliance Areas

| Guideline | Implementation |
|-----------|-----------------|
| **1.4.3 Contrast** | Text and background meet 4.5:1 contrast ratio |
| **2.1.1 Keyboard** | Full keyboard navigation support (Tab, Shift+Tab, Enter) |
| **2.1.2 No Keyboard Trap** | Users can navigate away without issue |
| **2.4.3 Focus Order** | Focus order is logical and meaningful |
| **2.4.7 Focus Visible** | Clear visual indicator when focused |
| **3.2.2 Predictable** | Component behaves consistently |
| **3.3.1 Error Identification** | Errors clearly identified |
| **3.3.4 Error Prevention** | Suggestions provided for errors |

---

## Keyboard Navigation

The TextBox supports standard keyboard interactions:

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 300px;">
    <h3>Keyboard Navigation Demo</h3>
    
    <SfTextBox Placeholder="Tab to move to next field">
    </SfTextBox>

    <SfTextBox Placeholder="Shift+Tab to move to previous field">
    </SfTextBox>

    <SfTextBox Placeholder="Enter to submit (if inside form)">
    </SfTextBox>
</div>
```

### Keyboard Shortcuts

| Key | Action |
|-----|--------|
| **Tab** | Move to next field |
| **Shift+Tab** | Move to previous field |
| **Enter** | Trigger blur/commit value (form submit) |
| **Escape** | Clear focus (optional, depends on context) |
| **Ctrl+A** | Select all text |
| **Ctrl+C** | Copy selected text |
| **Ctrl+X** | Cut selected text |
| **Ctrl+V** | Paste from clipboard |

---

## ARIA Attributes

Use ARIA attributes to enhance accessibility for screen readers:

```razor
@using Syncfusion.Blazor.Inputs

<!-- Labeled with aria-label -->
<SfTextBox Placeholder="Email"
           HtmlAttributes="@new Dictionary<string, object>
           {
               { \"aria-label\", \"Email address input\" }
           }">
</SfTextBox>

<!-- Labeled with aria-labelledby -->
<label id="password-label">Password (Required)</label>
<SfTextBox Placeholder="Password"
           HtmlAttributes="@new Dictionary<string, object>
           {
               { \"aria-labelledby\", \"password-label\" },
               { \"aria-required\", \"true\" }
           }">
</SfTextBox>

<!-- With error message using aria-describedby -->
<SfTextBox Placeholder="Username"
           HtmlAttributes="@new Dictionary<string, object>
           {
               { \"aria-describedby\", \"username-error\" }
           }">
</SfTextBox>
<div id="username-error" role="alert">
    Username must be 3-20 characters
</div>
```

### Common ARIA Attributes

| Attribute | Value | Purpose |
|-----------|-------|---------|
| `aria-label` | string | Accessible name for screen readers |
| `aria-labelledby` | id | References associated label element |
| `aria-describedby` | id | References description/error message |
| `aria-required` | true/false | Indicates required field |
| `aria-disabled` | true/false | Indicates disabled state |
| `aria-invalid` | true/false | Indicates validation error |
| `aria-live` | polite/assertive | Announce dynamic changes |

---

## Screen Reader Support

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 350px;">
    <h2>Accessible Registration Form</h2>

    <!-- Each field clearly labeled -->
    <div style="margin-bottom: 15px;">
        <label for="first-name" style="display: block; margin-bottom: 5px;">
            First Name <span aria-label="required">*</span>
        </label>
        <SfTextBox Id="first-name"
                   @bind-Value="@FirstName"
                   Placeholder="John"
                   FloatLabelType="@FloatLabelType.Auto"
                   HtmlAttributes="@new Dictionary<string, object>
                   {
                       { \"aria-required\", \"true\" },
                       { \"aria-label\", \"First Name, required\" }
                   }">
        </SfTextBox>
    </div>

    <!-- Error messages announced -->
    <div style="margin-bottom: 15px;">
        <label for="email" style="display: block; margin-bottom: 5px;">
            Email <span aria-label="required">*</span>
        </label>
        <SfTextBox Id="email"
                   @bind-Value="@Email"
                   Blur="@ValidateEmail"
                   Placeholder="you@example.com"
                   FloatLabelType="@FloatLabelType.Auto"
                   CssClass="@(EmailError ? \"e-error\" : \"\")"
                   HtmlAttributes="@new Dictionary<string, object>
                   {
                       { \"aria-required\", \"true\" },
                       { \"aria-invalid\", EmailError ? \"true\" : \"false\" },
                       { \"aria-describedby\", EmailError ? \"email-error\" : \"\" }
                   }">
        </SfTextBox>
        @if (EmailError)
        {
            <div id="email-error" role="alert" style="color: red; font-size: 12px; margin-top: 4px;">
                Please enter a valid email address
            </div>
        }
    </div>

    <button aria-label="Submit registration form">Register</button>
</div>

@code {
    private string FirstName = "";
    private string Email = "";
    private bool EmailError = false;

    private void ValidateEmail(FocusOutEventArgs args)
    {
        EmailError = !Email.Contains("@");
    }
}
```

---

## Color Contrast

Ensure adequate contrast ratios for text and backgrounds:

```razor
<style>
    /* Good: 4.5:1 contrast (meets WCAG AA) -->
    .accessible-text {
        color: #000;        /* Black text -->
        background: #fff;   /* White background -->
    }

    /* Good: Dark text on light background -->
    .dark-text-light-bg {
        color: #1f1f1f;
        background: #f5f5f5;
    }

    /* Bad: Insufficient contrast (avoid) -->
    .poor-contrast {
        color: #ccc;        /* Gray on white - only 1.3:1 -->
        background: #fff;
    }

    /* Validation states with good contrast -->
    .error-accessible {
        color: #b20000;     /* Dark red: good on white background -->
        background: #fff;
    }

    .success-accessible {
        color: #007500;     /* Dark green: good on white background -->
        background: #fff;
    }
</style>
```

---

## Focus Indicators

Ensure clear visual focus indicators:

```razor
<style>
    /* Clear focus indicator (don't remove default outline) -->
    :focus {
        outline: 2px solid #2196F3;
        outline-offset: 2px;
    }

    /* Custom focus styling for TextBox -->
    .textbox-wrapper:focus-within {
        border: 2px solid #2196F3;
        box-shadow: 0 0 0 3px rgba(33, 150, 243, 0.1);
    }

    /* Avoid: Remove focus outline (accessibility violation) -->
    /* ❌ input:focus { outline: none; } --> Don't do this! */
</style>

<div class="textbox-wrapper">
    <label for="accessible-input">Required Field</label>
    <SfTextBox Id="accessible-input" 
               Placeholder="Focus on me">
    </SfTextBox>
</div>
```

---

## Common Troubleshooting Issues

### Issue: Theme Not Loading

**Problem:** TextBox renders unstyled

```
❌ TextBox appears plain without theme styling
```

**Solution:** Verify stylesheet in `index.html`:

```html
<!-- Correct -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Common mistakes to check -->
<!-- Wrong path: -->
<!-- <link href="/Syncfusion.Blazor.Themes/bootstrap5.css" ... /> -->

<!-- Missing _content prefix: -->
<!-- <link href="Syncfusion.Blazor.Themes/bootstrap5.css" ... /> -->
```

### Issue: Component Not Rendering

**Problem:** TextBox doesn't appear on page

```
❌ SfTextBox tag not rendered, console errors present
```

**Solution:** Check imports and service registration:

```razor
<!-- _Imports.razor -->
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Inputs

<!-- Program.cs -->
builder.Services.AddSyncfusionBlazor();
```

### Issue: Icons Not Displaying

**Problem:** `AddIconAsync()` icons don't appear

```
❌ Icon reference exists but not visible
```

**Solution:**

```razor
<!-- Ensure Created event is triggered -->
<SfTextBox @ref="textBox"
           Created="@AddIcon"  <!-- ✓ Must have -->
           Placeholder="Search">
</SfTextBox>

@code {
    private SfTextBox textBox;

    private async void AddIcon()
    {
        if (textBox != null)  <!-- ✓ Null check -->
        {
            await textBox.AddIconAsync("append", "e-icons e-search");
        }
    }
}
```

### Issue: Binding Not Updating

**Problem:** UI doesn't update when value changes programmatically

```
❌ Changing @bind-Value doesn't reflect in TextBox
```

**Solution:**

```razor
<!-- Use @bind-Value for two-way binding -->
<SfTextBox @bind-Value="@Value"></SfTextBox>

<!-- Or use Value + ValueChanged explicitly -->
<SfTextBox Value="@Value"
           ValueChanged="@((string v) => Value = v)">
</SfTextBox>

@code {
    private string Value = "";

    private void UpdateValue()
    {
        Value = "new value"; <!-- ✓ Updates automatically -->
    }
}
```

### Issue: Events Not Firing

**Problem:** `Blur`, `Focus`, or `Input` events don't execute

```
❌ Event handler not being called
```

**Solution:**

```razor
<!-- Correct: Use event parameter with correct type -->
<SfTextBox Blur="@OnBlur"></SfTextBox>

@code {
    private void OnBlur(FocusOutEventArgs args)  <!-- ✓ Correct type -->
    {
        Console.WriteLine("Blur fired");
    }

    /* Common mistakes:
    ❌ private void OnBlur() {} -- Missing parameter
    ❌ private void OnBlur(string value) {} -- Wrong type
    ❌ private void OnBlur(InputEventArgs args) {} -- Wrong event args type
    */
}
```

### Issue: Validation CSS Not Applying

**Problem:** `e-success`, `e-error`, `e-warning` classes not working

```
❌ CssClass="e-success" not showing green styling
```

**Solution:**

```razor
<!-- Ensure theme stylesheet is loaded -->
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>

<!-- Apply class correctly -->
<SfTextBox Value="test"
           CssClass="e-success"  <!-- ✓ Correct -->
           Readonly="true">
</SfTextBox>

<!-- Not: -->
<!-- ❌ <SfTextBox Class="e-success"> -- Wrong property name -->
<!-- ❌ <SfTextBox style="..." CssClass="e-success"> -- Both together -->
```

### Issue: HtmlAttributes Not Working

**Problem:** `maxlength`, `type`, or other attributes not applied

```
❌ HtmlAttributes="@dict" exists but attributes don't apply
```

**Solution:**

```razor
<!-- Pass Dictionary<string, object> correctly -->
<SfTextBox HtmlAttributes="@new Dictionary<string, object>
{
    { \"maxlength\", \"20\" },   <!-- ✓ Correct -->
    { \"type\", \"email\" }       <!-- ✓ Correct -->
}">
</SfTextBox>

<!-- Or use direct attributes (some are supported) -->
<SfTextBox maxlength="20"          <!-- ✓ Works -->
           Placeholder="Max 20">
</SfTextBox>

<!-- Common mistakes:
❌ HtmlAttributes="maxlength: 20" -- Wrong syntax (string instead of dict)
❌ MaxLength="20" -- Wrong property name (case-sensitive)
❌ html-attributes (HTML syntax, not Razor) -->
```

### Issue: Performance Degradation with Many TextBoxes

**Problem:** Form with 100+ TextBox inputs is slow

```
❌ Rendering takes long time, UI is sluggish
```

**Solution:**

```razor
<!-- Use Virtualize for large lists -->
<Virtualize Items="@LargeList" Context="item">
    <SfTextBox @bind-Value="@item.Value"></SfTextBox>
</Virtualize>

<!-- Debounce expensive event handlers -->
@code {
    private Timer debounceTimer;

    private void OnInput(InputEventArgs args)
    {
        debounceTimer?.Dispose();
        debounceTimer = new Timer(300);
        debounceTimer.Elapsed += (s, e) =>
        {
            PerformExpensiveOperation(args.Value);
            InvokeAsync(StateHasChanged);
        };
        debounceTimer.Start();
    }
}
```

---

## Performance Optimization Tips

1. **Use `@bind-Value` instead of manual Value + ValueChanged** - Simpler and more efficient
2. **Debounce Input events** - Don't perform expensive ops on every keystroke
3. **Virtualize large forms** - Use `<Virtualize>` for 100+ fields
4. **Avoid inline event handlers** - Use `@ref` methods instead of lambdas
5. **Use `StateHasChanged()` carefully** - Only call when necessary

---

## Browser Compatibility

| Browser | Support | Notes |
|---------|---------|-------|
| Chrome | ✅ Full | Latest versions recommended |
| Firefox | ✅ Full | Latest versions recommended |
| Safari | ✅ Full | macOS and iOS supported |
| Edge | ✅ Full | Chromium-based versions |
| IE 11 | ⚠️ Limited | Polyfills may be needed |

---

## Getting Help

If issues persist:

1. **Check browser console** for JavaScript errors
2. **Verify NuGet versions match** (Inputs + Themes)
3. **Clear browser cache** (Ctrl+Shift+Del)
4. **Check Syncfusion documentation** (syncfusion.com/blazor-components/blazor-textbox)
5. **Review sample on GitHub** (SyncfusionExamples/Blazor-Getting-Started-Examples)

---

## Next Steps

- ✅ Back to [SKILL.md](../SKILL.md) - Complete reference guide
- ✅ [Getting Started](../getting-started.md) - Installation steps
- ✅ [Data Binding & Validation](../data-binding-validation.md) - Form patterns
