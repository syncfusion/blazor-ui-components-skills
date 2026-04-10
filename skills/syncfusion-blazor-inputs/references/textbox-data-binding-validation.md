# Data Binding & Validation

## Table of Contents
- [Two-way Binding](#two-way-binding)
- [Programmatic Updates](#programmatic-updates)
- [Binding in Forms](#binding-in-forms)
- [Validation Styles](#validation-styles)
- [Character Limits](#character-limits)
- [Input Validation Patterns](#input-validation-patterns)

---

## Two-way Binding

Use the `@bind-Value` directive for automatic two-way data binding:

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 300px;">
    <SfTextBox @bind-Value="@UserName"
               Placeholder="Enter your name">
    </SfTextBox>

    <p>Hello, @UserName!</p>
    <button @onclick="ClearName">Clear</button>
</div>

@code {
    private string UserName = "";

    private void ClearName()
    {
        UserName = ""; // Updates TextBox automatically
    }
}
```

### How Two-way Binding Works

```razor
<!-- @bind-Value is equivalent to: -->
<SfTextBox Value="@UserName"
           ValueChanged="@((string value) => UserName = value)"
           Placeholder="Enter your name">
</SfTextBox>
```

**Benefits:**
- ✅ Automatic synchronization
- ✅ Programmatic updates reflect in UI
- ✅ UI changes update component data
- ✅ Simple and declarative

---

## Programmatic Updates

Update TextBox value from code:

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 400px;">
    <SfTextBox @bind-Value="@Message"
               Placeholder="Your message">
    </SfTextBox>

    <div style="margin-top: 15px;">
        <button @onclick="@(() => Message = \"Hello World\")">
            Set to "Hello World"
        </button>
        <button @onclick="@(() => Message = \"\")">
            Clear
        </button>
        <button @onclick="AppendText">
            Append Text
        </button>
    </div>

    <p>Current: @Message</p>
</div>

@code {
    private string Message = "";

    private void AppendText()
    {
        Message += " [appended]";
    }
}
```

**Common Patterns:**

| Operation | Code |
|-----------|------|
| Set value | `TextBoxValue = "new value"` |
| Clear | `TextBoxValue = ""` |
| Append | `TextBoxValue += " more"` |
| Format | `TextBoxValue = TextBoxValue.ToUpper()` |
| Restore | `TextBoxValue = DefaultValue` |

---

## Binding in Forms

Bind multiple TextBox inputs to a form model:

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 350px;">
    <h3>Contact Form</h3>
    
    <SfTextBox @bind-Value="@contact.FirstName"
               Placeholder="First Name"
               FloatLabelType="@FloatLabelType.Auto">
    </SfTextBox>

    <SfTextBox @bind-Value="@contact.LastName"
               Placeholder="Last Name"
               FloatLabelType="@FloatLabelType.Auto">
    </SfTextBox>

    <SfTextBox @bind-Value="@contact.Email"
               Placeholder="Email"
               FloatLabelType="@FloatLabelType.Auto"
               HtmlAttributes="@new Dictionary<string, object> { { \"type\", \"email\" } }">
    </SfTextBox>

    <SfTextBox @bind-Value="@contact.Phone"
               Placeholder="Phone"
               FloatLabelType="@FloatLabelType.Auto"
               HtmlAttributes="@new Dictionary<string, object> { { \"type\", \"tel\" } }">
    </SfTextBox>

    <button @onclick="SaveContact">Save</button>

    <div style="margin-top: 20px; background-color: #f5f5f5; padding: 10px; border-radius: 4px;">
        <h4>Preview:</h4>
        <p><strong>Name:</strong> @contact.FirstName @contact.LastName</p>
        <p><strong>Email:</strong> @contact.Email</p>
        <p><strong>Phone:</strong> @contact.Phone</p>
    </div>
</div>

@code {
    private Contact contact = new();

    private void SaveContact()
    {
        Console.WriteLine($"Saving: {contact.FirstName} {contact.LastName}");
        Console.WriteLine($"  Email: {contact.Email}");
        Console.WriteLine($"  Phone: {contact.Phone}");
    }

    public class Contact
    {
        public string FirstName { get; set; } = "";
        public string LastName { get; set; } = "";
        public string Email { get; set; } = "";
        public string Phone { get; set; } = "";
    }
}
```

### Form Model Pattern

```razor
<!-- Using a C# record (concise syntax) -->
private record UserForm(
    string FirstName = "",
    string LastName = "",
    string Email = ""
);

private UserForm form = new();

<!-- Binding in template -->
<SfTextBox @bind-Value="@form.FirstName"></SfTextBox>
```

---

## Validation Styles

Add validation visual states using CSS classes:

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 300px;">
    <!-- Success state (green) -->
    <SfTextBox Value="validation@example.com"
               CssClass="e-success"
               Readonly="true"
               Placeholder="Valid email">
    </SfTextBox>

    <!-- Warning state (orange) -->
    <SfTextBox Value="incomplete"
               CssClass="e-warning"
               Readonly="true"
               Placeholder="Incomplete">
    </SfTextBox>

    <!-- Error state (red) -->
    <SfTextBox Value="invalid"
               CssClass="e-error"
               Readonly="true"
               Placeholder="Invalid input">
    </SfTextBox>
</div>
```

### Dynamic Validation States

```razor
@using Syncfusion.Blazor.Inputs

<style>
    .validation-feedback {
        font-size: 12px;
        margin-top: 5px;
    }

    .success-msg { color: green; }
    .warning-msg { color: orange; }
    .error-msg { color: red; }
</style>

<div style="margin: 50px auto; width: 300px;">
    <SfTextBox @bind-Value="@Email"
               Blur="@ValidateEmail"
               Placeholder="Enter email"
               CssClass="@GetValidationClass()">
    </SfTextBox>
    
    @if (ValidationMessage != null)
    {
        <p class="validation-feedback @GetValidationMessageClass()">
            @ValidationMessage
        </p>
    }
</div>

@code {
    private string Email = "";
    private string ValidationMessage = null;
    private string ValidationState = ""; // "", "success", "warning", "error"

    private void ValidateEmail(FocusOutEventArgs args)
    {
        if (Email.Length == 0)
        {
            ValidationState = "error";
            ValidationMessage = "Email is required";
        }
        else if (!Email.Contains("@"))
        {
            ValidationState = "warning";
            ValidationMessage = "Missing @ symbol";
        }
        else if (!Email.Contains("."))
        {
            ValidationState = "warning";
            ValidationMessage = "Missing domain extension";
        }
        else if (Email.Length < 5)
        {
            ValidationState = "error";
            ValidationMessage = "Email too short";
        }
        else
        {
            ValidationState = "success";
            ValidationMessage = "✓ Valid email";
        }
    }

    private string GetValidationClass()
    {
        return ValidationState switch
        {
            "success" => "e-success",
            "warning" => "e-warning",
            "error" => "e-error",
            _ => ""
        };
    }

    private string GetValidationMessageClass()
    {
        return ValidationState switch
        {
            "success" => "success-msg",
            "warning" => "warning-msg",
            "error" => "error-msg",
            _ => ""
        };
    }
}
```

### Validation State Summary

| Class | Color | Use Case |
|-------|-------|----------|
| `e-success` | Green | Valid, confirmed, complete |
| `e-warning` | Orange | Incomplete, needs attention |
| `e-error` | Red | Invalid, required, failed |

---

## Character Limits

Limit character input using `maxlength` attribute and display remaining count:

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 400px;">
    <!-- Character limit with display -->
    <SfTextBox @bind-Value="@Bio"
               Input="@UpdateCharCount"
               Placeholder="Write your bio (max 150 chars)"
               Multiline="true"
               Rows="4"
               HtmlAttributes="@new Dictionary<string, object> { { \"maxlength\", \"150\" } }">
    </SfTextBox>

    <div style="margin-top: 10px; display: flex; justify-content: space-between;">
        <p style="color: @GetCountColor();">
            @Bio.Length / 150 characters
        </p>
        <button @onclick="@(() => Bio = \"\")" 
                disabled="@(Bio.Length == 0)">
            Clear
        </button>
    </div>

    @if (CharCountWarning)
    {
        <p style="color: orange; font-size: 12px;">
            ⚠️ You're nearing the character limit!
        </p>
    }
</div>

@code {
    private string Bio = "";
    private bool CharCountWarning = false;

    private void UpdateCharCount(InputEventArgs args)
    {
        Bio = args.Value;
        CharCountWarning = Bio.Length > 130; // Warn at 130/150
    }

    private string GetCountColor()
    {
        return Bio.Length switch
        {
            < 50 => "gray",
            < 130 => "green",
            _ => "orange"
        };
    }
}
```

---

## Input Validation Patterns

### Email Validation

```razor
<SfTextBox @bind-Value="@Email"
           Blur="@ValidateEmail"
           CssClass="@(IsValidEmail ? \"e-success\" : IsInvalidEmail ? \"e-error\" : \"\")"
           HtmlAttributes="@new Dictionary<string, object> { { \"type\", \"email\" } }">
</SfTextBox>

@code {
    private string Email = "";
    private bool IsValidEmail = false;
    private bool IsInvalidEmail = false;

    private void ValidateEmail(FocusOutEventArgs args)
    {
        string pattern = @"^[^\s@]+@[^\s@]+\.[^\s@]+$";
        IsValidEmail = System.Text.RegularExpressions.Regex.IsMatch(Email, pattern);
        IsInvalidEmail = !string.IsNullOrEmpty(Email) && !IsValidEmail;
    }
}
```

### URL Validation

```razor
<SfTextBox @bind-Value="@Website"
           Blur="@ValidateUrl"
           CssClass="@(IsValidUrl ? \"e-success\" : IsInvalidUrl ? \"e-error\" : \"\")"
           Placeholder="https://example.com">
</SfTextBox>

@code {
    private string Website = "";
    private bool IsValidUrl = false;
    private bool IsInvalidUrl = false;

    private void ValidateUrl(FocusOutEventArgs args)
    {
        if (string.IsNullOrEmpty(Website))
        {
            IsInvalidUrl = false;
            IsValidUrl = false;
            return;
        }

        IsValidUrl = Uri.TryCreate(Website, UriKind.Absolute, out _);
        IsInvalidUrl = !IsValidUrl;
    }
}
```

### Password Strength

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 300px;">
    <SfTextBox @bind-Value="@Password"
               Input="@CheckPasswordStrength"
               Placeholder="Password"
               HtmlAttributes="@new Dictionary<string, object> { { \"type\", \"password\" } }">
    </SfTextBox>

    <div style="margin-top: 10px;">
        <div style="background-color: #e0e0e0; height: 4px; border-radius: 2px; overflow: hidden;">
            <div style="background-color: @GetStrengthColor(); 
                        width: @GetStrengthPercentage()%; 
                        height: 100%; 
                        transition: all 0.3s ease;">
            </div>
        </div>
        <p style="font-size: 12px; margin-top: 5px; color: @GetStrengthColor();">
            Strength: @PasswordStrength
        </p>
    </div>
</div>

@code {
    private string Password = "";
    private string PasswordStrength = "Very Weak";

    private void CheckPasswordStrength(InputEventArgs args)
    {
        Password = args.Value;

        int score = 0;
        if (Password.Length >= 8) score++;
        if (System.Text.RegularExpressions.Regex.IsMatch(Password, @"[a-z]")) score++;
        if (System.Text.RegularExpressions.Regex.IsMatch(Password, @"[A-Z]")) score++;
        if (System.Text.RegularExpressions.Regex.IsMatch(Password, @"[0-9]")) score++;
        if (System.Text.RegularExpressions.Regex.IsMatch(Password, @"[!@#$%^&*]")) score++;

        PasswordStrength = score switch
        {
            0 => "Very Weak",
            1 => "Weak",
            2 => "Fair",
            3 => "Good",
            4 => "Strong",
            _ => "Very Strong"
        };
    }

    private string GetStrengthColor()
    {
        return PasswordStrength switch
        {
            "Very Weak" => "#ff0000",
            "Weak" => "#ff6600",
            "Fair" => "#ffcc00",
            "Good" => "#99cc00",
            "Strong" => "#00cc00",
            _ => "#00aa00"
        };
    }

    private int GetStrengthPercentage()
    {
        return PasswordStrength switch
        {
            "Very Weak" => 10,
            "Weak" => 25,
            "Fair" => 50,
            "Good" => 75,
            "Strong" => 90,
            _ => 100
        };
    }
}
```

---

## Form Submission with Validation

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin: 50px auto; width: 350px;">
    <h3>Registration Form</h3>

    <SfTextBox @bind-Value="@form.Username"
               Blur="@ValidateUsername"
               Placeholder="Username (3-20 chars)"
               CssClass="@(UsernameValid == true ? \"e-success\" : UsernameValid == false ? \"e-error\" : \"\")">
    </SfTextBox>

    <SfTextBox @bind-Value="@form.Email"
               Blur="@ValidateEmail"
               Placeholder="Email"
               CssClass="@(EmailValid == true ? \"e-success\" : EmailValid == false ? \"e-error\" : \"\")">
    </SfTextBox>

    <button @onclick="Submit" 
            disabled="@(!CanSubmit())">
        Register
    </button>
</div>

@code {
    private record RegistrationForm(string Username = "", string Email = "");
    private RegistrationForm form = new();

    private bool? UsernameValid = null;
    private bool? EmailValid = null;

    private void ValidateUsername(FocusOutEventArgs args)
    {
        UsernameValid = form.Username.Length >= 3 && form.Username.Length <= 20;
    }

    private void ValidateEmail(FocusOutEventArgs args)
    {
        string pattern = @"^[^\s@]+@[^\s@]+\.[^\s@]+$";
        EmailValid = System.Text.RegularExpressions.Regex.IsMatch(form.Email, pattern);
    }

    private bool CanSubmit()
    {
        return UsernameValid == true && EmailValid == true;
    }

    private void Submit()
    {
        Console.WriteLine($"Registering: {form.Username} ({form.Email})");
    }
}
```

---

## Next Steps

- ✅ [Events & Interactions](../events-handling.md) - Handle validation events
- ✅ [Advanced Features](../advanced-features.md) - Custom validation components
- ✅ [Accessibility & Troubleshooting](../accessibility-troubleshooting.md) - Form accessibility
