# Advanced Features & Customization

## Creating Components Dynamically

Generate TextBox components at runtime based on data:

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 400px;">
    <h3>Dynamic Form Generator</h3>

    @foreach (var field in FormFields)
    {
        <div style="margin-bottom: 15px;">
            <label>@field.Label</label>
            <SfTextBox @bind-Value="@field.Value"
                       Placeholder="@field.Placeholder"
                       Multiline="@field.IsMultiline"
                       Rows="@(field.IsMultiline ? 4 : 1)"
                       FloatLabelType="@FloatLabelType.Auto">
            </SfTextBox>
        </div>
    }

    <button @onclick="SaveForm">Save</button>
</div>

@code {
    private List<FormField> FormFields = new();

    protected override void OnInitialized()
    {
        // Generate fields dynamically
        FormFields = new()
        {
            new FormField { Label = "Name", Placeholder = "Your name", Key = "name" },
            new FormField { Label = "Email", Placeholder = "your@email.com", Key = "email" },
            new FormField { Label = "Comments", Placeholder = "Your comments", IsMultiline = true, Key = "comments" },
            new FormField { Label = "Address", Placeholder = "Full address", IsMultiline = true, Key = "address" }
        };
    }

    private void SaveForm()
    {
        foreach (var field in FormFields)
        {
            Console.WriteLine($"{field.Key}: {field.Value}");
        }
    }

    public class FormField
    {
        public string Key { get; set; } = "";
        public string Label { get; set; } = "";
        public string Placeholder { get; set; } = "";
        public string Value { get; set; } = "";
        public bool IsMultiline { get; set; } = false;
    }
}
```

### Dynamic Field Count

```razor
<button @onclick="@(() => FieldCount++)">Add Field</button>
<button @onclick="@(() => FieldCount = Math.Max(1, FieldCount - 1))">Remove Field</button>

@for (int i = 0; i < FieldCount; i++)
{
    int index = i; // Capture for closure
    <SfTextBox @bind-Value="@DynamicValues[index]"
               Placeholder="Field @(index + 1)">
    </SfTextBox>
}

@code {
    private int FieldCount = 3;
    private List<string> DynamicValues = new() { "", "", "" };

    protected override void OnParametersSet()
    {
        // Ensure list size matches field count
        while (DynamicValues.Count < FieldCount)
            DynamicValues.Add("");
        while (DynamicValues.Count > FieldCount)
            DynamicValues.RemoveAt(DynamicValues.Count - 1);
    }
}
```

---

## Adding Icons Programmatically

Use `AddIconAsync()` to add icons dynamically:

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 300px;">
    <h3>Icon TextBox Examples</h3>

    <!-- Date picker icon -->
    <SfTextBox @ref="DateBox"
               Placeholder="Select date"
               Created="@AddDateIcon">
    </SfTextBox>

    <!-- Search icon -->
    <SfTextBox @ref="SearchBox"
               Placeholder="Search users..."
               Created="@AddSearchIcon">
    </SfTextBox>

    <!-- Clear icon (custom) -->
    <SfTextBox @ref="ClearBox"
               Placeholder="Clearable input"
               Created="@AddClearIcon">
    </SfTextBox>

    <!-- Password toggle icon -->
    <SfTextBox @ref="PasswordBox"
               Placeholder="Password"
               Created="@AddPasswordIcon"
               HtmlAttributes="@new Dictionary<string, object> { { \"type\", \"password\" } }">
    </SfTextBox>
</div>

@code {
    private SfTextBox DateBox, SearchBox, ClearBox, PasswordBox;

    private async void AddDateIcon()
    {
        if (DateBox != null)
        {
            await DateBox.AddIconAsync("append", "e-icons e-calendar");
        }
    }

    private async void AddSearchIcon()
    {
        if (SearchBox != null)
        {
            await SearchBox.AddIconAsync("prepend", "e-icons e-search");
        }
    }

    private async void AddClearIcon()
    {
        if (ClearBox != null)
        {
            await ClearBox.AddIconAsync("append", "e-icons e-close");
        }
    }

    private async void AddPasswordIcon()
    {
        if (PasswordBox != null)
        {
            await PasswordBox.AddIconAsync("append", "e-icons e-eye");
        }
    }
}
```

### Icon List (Common Syncfusion Icons)

| Icon | Class | Use |
|------|-------|-----|
| Calendar | `e-icons e-calendar` | Date input |
| Search | `e-icons e-search` | Search field |
| Close | `e-icons e-close` | Clear button |
| Eye | `e-icons e-eye` | Show password |
| User | `e-icons e-user` | Username |
| Mail | `e-icons e-mail` | Email |
| Phone | `e-icons e-phone` | Phone number |
| Lock | `e-icons e-lock` | Secure field |

---

## Custom TextBox with Validation

Build reusable custom TextBox components:

```razor
<!-- CustomValidatedTextBox.razor -->
@using Syncfusion.Blazor.Inputs

<div>
    <SfTextBox @bind-Value="@Value"
               Blur="@OnBlur"
               Placeholder="@Placeholder"
               FloatLabelType="@FloatLabelType.Auto"
               CssClass="@GetValidationClass()">
    </SfTextBox>

    @if (ShowError)
    {
        <p style="color: red; font-size: 12px; margin-top: 4px;">
            @ErrorMessage
        </p>
    }

    @if (ShowSuccess)
    {
        <p style="color: green; font-size: 12px; margin-top: 4px;">
            ✓ Valid
        </p>
    }
</div>

@code {
    [Parameter]
    public string Value { get; set; } = "";

    [Parameter]
    public EventCallback<string> ValueChanged { get; set; }

    [Parameter]
    public string Placeholder { get; set; } = "";

    [Parameter]
    public Func<string, (bool isValid, string message)> ValidateFunc { get; set; }

    private bool ShowError = false;
    private bool ShowSuccess = false;
    private string ErrorMessage = "";

    private async Task OnBlur(FocusOutEventArgs args)
    {
        if (ValidateFunc != null)
        {
            var (isValid, message) = ValidateFunc(Value);
            ShowError = !isValid;
            ShowSuccess = isValid;
            ErrorMessage = message;
        }
    }

    private string GetValidationClass()
    {
        return ShowSuccess ? "e-success" : ShowError ? "e-error" : "";
    }
}

<!-- Usage -->
<CustomValidatedTextBox Placeholder="Email"
                        @bind-Value="@Email"
                        ValidateFunc="@ValidateEmail" />

@code {
    private string Email = "";

    private (bool, string) ValidateEmail(string email)
    {
        if (string.IsNullOrEmpty(email))
            return (false, "Email is required");

        if (!email.Contains("@"))
            return (false, "Invalid email format");

        return (true, "");
    }
}
```

---

## Native Events Handling

Handle native HTML events alongside Syncfusion events:

```razor
@using Syncfusion.Blazor.Inputs

<SfTextBox Placeholder="Type or paste"
           @onkeydown="@OnKeyDown"
           @onkeyup="@OnKeyUp"
           @onpaste="@OnPaste"
           Input="@OnInput">
</SfTextBox>

<p>Last Key Pressed: @LastKey</p>
<p>Pasted Text: @PastedText</p>

@code {
    private string LastKey = "";
    private string PastedText = "";

    private void OnKeyDown(KeyboardEventArgs e)
    {
        if (e.Key == "Enter")
        {
            Console.WriteLine("Enter pressed");
        }
        LastKey = e.Key;
    }

    private void OnKeyUp(KeyboardEventArgs e)
    {
        Console.WriteLine($"Key up: {e.Key}");
    }

    private void OnPaste(ClipboardEventArgs e)
    {
        PastedText = "Paste detected";
        Console.WriteLine("User pasted content");
    }

    private void OnInput(InputEventArgs args)
    {
        // Handle input
    }
}
```

---

## RTL (Right-to-Left) Support

Enable RTL for Arabic, Hebrew, Urdu, and other RTL languages:

```razor
@using Syncfusion.Blazor.Inputs

<!-- Enable RTL at document level -->
<html dir="rtl">
<head>
    <meta charset="utf-8" />
    <!-- RTL theme stylesheet -->
    <link href="_content/Syncfusion.Blazor.Themes/material3-rtl.css" rel="stylesheet" />
</head>
<body>
    <div id="app"></div>

    <!-- Component automatically adapts to RTL -->
    <SfTextBox Placeholder="أدخل اسمك"></SfTextBox>
</body>
</html>
```

### Component-Level RTL

```razor
@using Syncfusion.Blazor.Inputs

<style>
    .rtl-container {
        direction: rtl;
        text-align: right;
    }
</style>

<div class="rtl-container" style="width: 300px; margin: 50px auto;">
    <SfTextBox Placeholder="الاسم" FloatLabelType="@FloatLabelType.Auto"></SfTextBox>
    <SfTextBox Placeholder="البريد الإلكتروني" FloatLabelType="@FloatLabelType.Auto"></SfTextBox>
</div>
```

---

## Performance Optimization

### Debounce Input Events

For expensive operations (search, API calls), debounce the input event:

```razor
@using Syncfusion.Blazor.Inputs
@using System.Timers

<SfTextBox @bind-Value="@SearchQuery"
           Input="@OnSearchInput"
           Placeholder="Search (debounced)...">
</SfTextBox>

<p>Debounced value: @DebouncedValue</p>

@code {
    private string SearchQuery = "";
    private string DebouncedValue = "";
    private Timer DebounceTimer;

    private void OnSearchInput(InputEventArgs args)
    {
        SearchQuery = args.Value;

        // Reset timer
        DebounceTimer?.Dispose();
        DebounceTimer = new Timer(500);
        DebounceTimer.Elapsed += (s, e) =>
        {
            DebouncedValue = SearchQuery;
            DebounceTimer.Stop();
            InvokeAsync(StateHasChanged);
        };
        DebounceTimer.Start();
    }

    public void Dispose()
    {
        DebounceTimer?.Dispose();
    }
}
```

### Virtual Scrolling with Many Fields

For forms with hundreds of inputs, use virtualization:

```razor
<Virtualize Items="@LargeFormFieldList" Context="field">
    <SfTextBox @bind-Value="@field.Value"
               Placeholder="@field.Label">
    </SfTextBox>
</Virtualize>

@code {
    private List<FormField> LargeFormFieldList = 
        Enumerable.Range(1, 1000)
            .Select(i => new FormField { Label = $"Field {i}" })
            .ToList();
}
```

---

## Accessibility Enhancements

### ARIA Labels

```razor
<label for="username-input">Username (Required)</label>
<SfTextBox Id="username-input"
           HtmlAttributes="@new Dictionary<string, object>
           {
               { \"aria-label\", \"Username input, required\" },
               { \"aria-required\", \"true\" }
           }">
</SfTextBox>
```

### Focus Management

```razor
<SfTextBox @ref="FirstInput" Placeholder="First"></SfTextBox>
<SfTextBox @ref="SecondInput" Placeholder="Second"></SfTextBox>

<button @onclick="@(() => SecondInput.FocusAsync())">
    Move to Second Input
</button>

@code {
    private SfTextBox FirstInput, SecondInput;
}
```

---

## Next Steps

- ✅ [Events & Interactions](../events-handling.md) - Event-driven features
- ✅ [Accessibility & Troubleshooting](../accessibility-troubleshooting.md) - Accessibility best practices
- ✅ Back to [SKILL.md](../SKILL.md) - Complete reference guide
