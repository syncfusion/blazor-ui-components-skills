# InputMask Prompt and Display Configuration

This guide covers the configuration of prompt characters, placeholders, and display behavior for the InputMask component, including the EnableLiterals property and visual feedback customization.

## PromptChar Property

The `PromptChar` property defines the character displayed in unfilled mask positions. Default is underscore (`_`).

### Default Behavior

```razor
<SfMaskedTextBox Mask="000-00-0000" @bind-Value="@ssn"></SfMaskedTextBox>

<!-- Display before input: ___-__-____ -->
<!-- Display partial input: 123-__-____ -->
```

### Custom Prompt Character

```razor
<SfMaskedTextBox Mask="000-00-0000" 
                 PromptChar="*"
                 @bind-Value="@ssn">
</SfMaskedTextBox>

<!-- Display before input: ***-**-**** -->
<!-- Display partial input: 123-**-**** -->
```

### Common Prompt Characters

| Character | Use Case | Example Display |
|-----------|----------|-----------------|
| `_` (default) | General purpose | `___-__-____` |
| `*` | Password-like fields | `***-**-****` |
| `#` | Numeric emphasis | `###-##-####` |
| `•` | Dots for modern look | `•••-••-••••` |
| ` ` (space) | Minimal UI | `   -  -    ` |

### Example: Different Prompt Characters

```razor
<div class="mask-examples">
    <div>
        <label>Default (_):</label>
        <SfMaskedTextBox Mask="(000) 000-0000" 
                         PromptChar="_"
                         @bind-Value="@phone1">
        </SfMaskedTextBox>
        <!-- Display: (___) ___-____ -->
    </div>
    
    <div>
        <label>Asterisk (*):</label>
        <SfMaskedTextBox Mask="(000) 000-0000" 
                         PromptChar="*"
                         @bind-Value="@phone2">
        </SfMaskedTextBox>
        <!-- Display: (***) ***-**** -->
    </div>
    
    <div>
        <label>Hash (#):</label>
        <SfMaskedTextBox Mask="(000) 000-0000" 
                         PromptChar="#"
                         @bind-Value="@phone3">
        </SfMaskedTextBox>
        <!-- Display: (###) ###-#### -->
    </div>
    
    <div>
        <label>Space:</label>
        <SfMaskedTextBox Mask="(000) 000-0000" 
                         PromptChar=" "
                         @bind-Value="@phone4">
        </SfMaskedTextBox>
        <!-- Display: (   )    -     -->
    </div>
</div>

@code {
    private string phone1 = "";
    private string phone2 = "";
    private string phone3 = "";
    private string phone4 = "";
}
```

## PromptPlaceholder Property

`PromptPlaceholder` defines an alternate character shown in empty fields before user interaction. Once the user focuses on the field, it switches to `PromptChar`.

### Basic Usage

```razor
<SfMaskedTextBox Mask="000-00-0000" 
                 PromptChar="_"
                 PromptPlaceholder="•"
                 @bind-Value="@ssn">
</SfMaskedTextBox>

<!-- Before focus: •••-••-•••• -->
<!-- After focus: ___-__-____ -->
<!-- During input: 123-__-____ -->
```

### Use Cases for PromptPlaceholder

**1. Cleaner Initial Appearance:**
```razor
<SfMaskedTextBox Mask="(000) 000-0000" 
                 PromptChar="_"
                 PromptPlaceholder=" "
                 @bind-Value="@phone">
</SfMaskedTextBox>

<!-- Before focus: (   )    -     (cleaner look) -->
<!-- After focus: (___) ___-____ (shows structure) -->
```

**2. Visual Distinction:**
```razor
<SfMaskedTextBox Mask="0000-0000-0000-0000" 
                 PromptChar="_"
                 PromptPlaceholder="•"
                 @bind-Value="@cardNumber">
</SfMaskedTextBox>

<!-- Empty: ••••-••••-••••-•••• (softer) -->
<!-- Focused: ____-____-____-____ (actionable) -->
```

**3. Modern Material Design Style:**
```razor
<SfMaskedTextBox Mask="00/00/0000" 
                 PromptChar="_"
                 PromptPlaceholder=" "
                 FloatLabelType="FloatLabelType.Auto"
                 Placeholder="MM/DD/YYYY"
                 @bind-Value="@date">
</SfMaskedTextBox>

<!-- Clean, minimal appearance until focused -->
```

## Placeholder Property

The `Placeholder` property shows hint text when the field is completely empty. It disappears when the user starts typing or focuses on the field.

### Placeholder vs PromptChar

```razor
<!-- With Placeholder only -->
<SfMaskedTextBox Mask="000-00-0000" 
                 Placeholder="Enter SSN"
                 @bind-Value="@ssn1">
</SfMaskedTextBox>
<!-- Empty: "Enter SSN" (gray text) -->
<!-- Focused: ___-__-____ (prompt characters) -->

<!-- With both Placeholder and custom PromptChar -->
<SfMaskedTextBox Mask="000-00-0000" 
                 Placeholder="Enter SSN"
                 PromptChar="*"
                 @bind-Value="@ssn2">
</SfMaskedTextBox>
<!-- Empty: "Enter SSN" (gray text) -->
<!-- Focused: ***-**-**** (asterisk prompts) -->
```

### Best Practice: Descriptive Placeholders

```razor
<div class="form-group">
    <label>Phone Number:</label>
    <SfMaskedTextBox Mask="(000) 000-0000" 
                     Placeholder="(555) 123-4567"
                     @bind-Value="@phone">
    </SfMaskedTextBox>
</div>

<div class="form-group">
    <label>Credit Card:</label>
    <SfMaskedTextBox Mask="0000-0000-0000-0000" 
                     Placeholder="1234-5678-9012-3456"
                     @bind-Value="@card">
    </SfMaskedTextBox>
</div>

<div class="form-group">
    <label>Date of Birth:</label>
    <SfMaskedTextBox Mask="00/00/0000" 
                     Placeholder="MM/DD/YYYY"
                     @bind-Value="@dob">
    </SfMaskedTextBox>
</div>
```

## EnableLiterals Property

`EnableLiterals` determines whether literal characters (separators like `-`, `/`, `()`) are included in the value returned from the component.

### EnableLiterals = true (Default)

Literals are included in the value:

```razor
<SfMaskedTextBox Mask="(000) 000-0000" 
                 EnableLiterals="true"
                 @bind-Value="@phone">
</SfMaskedTextBox>

@code {
    private string phone = "";
    
    // User enters: 5551234567
    // Value stored: "(555) 123-4567"  ← Includes ( ) - characters
}
```

### EnableLiterals = false

Literals are excluded from the value:

```razor
<SfMaskedTextBox Mask="(000) 000-0000" 
                 EnableLiterals="false"
                 @bind-Value="@phone">
</SfMaskedTextBox>

@code {
    private string phone = "";
    
    // User enters: 5551234567
    // Display: (555) 123-4567
    // Value stored: "5551234567"  ← Only digits, no literals
}
```

### When to Use Each Setting

**EnableLiterals = true (Include literals):**
- Display values in formatted form
- Logging/audit trails with readable format
- User-facing reports
- Copy-paste scenarios where format matters

```razor
<SfMaskedTextBox Mask="000-00-0000" 
                 EnableLiterals="true"
                 @bind-Value="@ssn">
</SfMaskedTextBox>

@code {
    // Value: "123-45-6789" ← Ready for display
    private void DisplaySSN()
    {
        Console.WriteLine($"SSN: {ssn}"); // Output: SSN: 123-45-6789
    }
}
```

**EnableLiterals = false (Exclude literals):**
- Database storage (save only actual data)
- API calls requiring raw values
- Numeric/processing operations
- International format conversion

```razor
<SfMaskedTextBox Mask="(000) 000-0000" 
                 EnableLiterals="false"
                 @bind-Value="@phone">
</SfMaskedTextBox>

@code {
    // Value: "5551234567" ← Just digits, no formatting
    private async Task SaveToDatabase()
    {
        // Store clean value without formatting characters
        await DatabaseService.SavePhone(phone); // Saves: "5551234567"
    }
    
    private async Task CallAPI()
    {
        // API expects phone without formatting
        await API.SendSMS(phone, message); // Uses: "5551234567"
    }
}
```

### Comparison Example

```razor
<h3>EnableLiterals Comparison</h3>

<div class="comparison">
    <div>
        <label>With Literals (true):</label>
        <SfMaskedTextBox Mask="(000) 000-0000" 
                         EnableLiterals="true"
                         @bind-Value="@phoneWithLiterals">
        </SfMaskedTextBox>
        <p>Value: "@phoneWithLiterals"</p>
    </div>
    
    <div>
        <label>Without Literals (false):</label>
        <SfMaskedTextBox Mask="(000) 000-0000" 
                         EnableLiterals="false"
                         @bind-Value="@phoneWithoutLiterals">
        </SfMaskedTextBox>
        <p>Value: "@phoneWithoutLiterals"</p>
    </div>
</div>

@code {
    private string phoneWithLiterals = "5551234567";
    private string phoneWithoutLiterals = "5551234567";
    
    // User enters the same: 5551234567
    // phoneWithLiterals value: "(555) 123-4567"
    // phoneWithoutLiterals value: "5551234567"
}
```

## Visual Feedback Configuration

### Combining Properties for Optimal UX

**Pattern 1: Clean, Modern Look**
```razor
<SfMaskedTextBox Mask="(000) 000-0000" 
                 PromptChar="_"
                 PromptPlaceholder=" "
                 Placeholder="Enter phone number"
                 FloatLabelType="FloatLabelType.Auto"
                 EnableLiterals="false"
                 @bind-Value="@phone">
</SfMaskedTextBox>

<!-- Empty: Shows "Enter phone number" -->
<!-- Focus: Shows clean (   )    -     -->
<!-- Typing: Shows (555) ___-____ -->
<!-- Value: "5551234567" (no literals) -->
```

**Pattern 2: Clear Structure**
```razor
<SfMaskedTextBox Mask="000-00-0000" 
                 PromptChar="_"
                 Placeholder="SSN: 123-45-6789"
                 EnableLiterals="true"
                 ShowClearButton="true"
                 @bind-Value="@ssn">
</SfMaskedTextBox>

<!-- Shows structure clearly with underscores -->
<!-- Value includes hyphens for readability -->
<!-- Clear button for easy reset -->
```

**Pattern 3: Security-Focused**
```razor
<SfMaskedTextBox Mask="0000" 
                 PromptChar="*"
                 Placeholder="Enter PIN"
                 EnableLiterals="false"
                 @bind-Value="@pin">
</SfMaskedTextBox>

<!-- Asterisks suggest sensitive data -->
<!-- Value is just digits -->
```

## User Experience Considerations

### 1. Match PromptChar to Field Purpose

```razor
<!-- General input: underscore -->
<SfMaskedTextBox Mask="000-000" PromptChar="_"></SfMaskedTextBox>

<!-- Sensitive data: asterisk -->
<SfMaskedTextBox Mask="0000" PromptChar="*"></SfMaskedTextBox>

<!-- Modern minimal: space -->
<SfMaskedTextBox Mask="00/00/0000" PromptChar=" "></SfMaskedTextBox>
```

### 2. Use Placeholder for Examples

```razor
<SfMaskedTextBox Mask="(000) 000-0000" 
                 Placeholder="Example: (555) 123-4567"
                 @bind-Value="@phone">
</SfMaskedTextBox>
```

### 3. Consider Color/Theme

```razor
<style>
    /* Softer prompt character color */
    .e-input-group .e-mask-prompt-char {
        color: #cccccc;
    }
    
    /* Highlight entered characters */
    .e-input-group .e-mask-value {
        color: #000000;
        font-weight: 500;
    }
</style>

<SfMaskedTextBox Mask="000-00-0000" 
                 CssClass="custom-mask"
                 PromptChar="_"
                 @bind-Value="@value">
</SfMaskedTextBox>
```

### 4. Accessibility Considerations

Always pair visual cues with proper labels:

```razor
<div class="form-group">
    <label for="phone-input">
        Phone Number <span class="required">*</span>
    </label>
    <SfMaskedTextBox ID="phone-input"
                     Mask="(000) 000-0000" 
                     Placeholder="Enter your phone number"
                     PromptChar="_"
                     @bind-Value="@phone">
    </SfMaskedTextBox>
    <small class="form-text text-muted">Format: (###) ###-####</small>
</div>
```

## Complete Configuration Example

```razor
@page "/mask-configuration-demo"
@using Syncfusion.Blazor.Inputs

<div class="demo-container">
    <h2>Mask Configuration Examples</h2>
    
    <!-- Example 1: Standard with literals -->
    <div class="form-group">
        <label>Phone (with literals):</label>
        <SfMaskedTextBox Mask="(000) 000-0000" 
                         PromptChar="_"
                         Placeholder="(555) 123-4567"
                         EnableLiterals="true"
                         ShowClearButton="true"
                         @bind-Value="@phone1">
        </SfMaskedTextBox>
        <small>Value: @phone1</small>
    </div>
    
    <!-- Example 2: Clean storage without literals -->
    <div class="form-group">
        <label>Phone (without literals - for database):</label>
        <SfMaskedTextBox Mask="(000) 000-0000" 
                         PromptChar="_"
                         Placeholder="(555) 123-4567"
                         EnableLiterals="false"
                         @bind-Value="@phone2">
        </SfMaskedTextBox>
        <small>Value: @phone2</small>
    </div>
    
    <!-- Example 3: Modern minimal look -->
    <div class="form-group">
        <label>Date of Birth:</label>
        <SfMaskedTextBox Mask="00/00/0000" 
                         PromptChar=" "
                         PromptPlaceholder=" "
                         Placeholder="MM/DD/YYYY"
                         FloatLabelType="FloatLabelType.Auto"
                         EnableLiterals="false"
                         @bind-Value="@dob">
        </SfMaskedTextBox>
        <small>Value: @dob</small>
    </div>
    
    <!-- Example 4: Security-focused -->
    <div class="form-group">
        <label>PIN Code:</label>
        <SfMaskedTextBox Mask="0000" 
                         PromptChar="*"
                         Placeholder="Enter 4-digit PIN"
                         EnableLiterals="false"
                         @bind-Value="@pin">
        </SfMaskedTextBox>
        <small>Value: @pin</small>
    </div>
    
    <!-- Example 5: Clear structure -->
    <div class="form-group">
        <label>SSN:</label>
        <SfMaskedTextBox Mask="000-00-0000" 
                         PromptChar="_"
                         PromptPlaceholder="•"
                         Placeholder="Social Security Number"
                         EnableLiterals="true"
                         ShowClearButton="true"
                         @bind-Value="@ssn">
        </SfMaskedTextBox>
        <small>Value: @ssn</small>
    </div>
</div>

<style>
    .demo-container {
        max-width: 600px;
        margin: 20px;
    }
    
    .form-group {
        margin-bottom: 25px;
    }
    
    .form-group label {
        display: block;
        margin-bottom: 8px;
        font-weight: 500;
    }
    
    .form-group small {
        display: block;
        margin-top: 5px;
        color: #666;
        font-size: 12px;
    }
</style>

@code {
    private string phone1 = "";
    private string phone2 = "";
    private string dob = "";
    private string pin = "";
    private string ssn = "";
}
```

## Best Practices Summary

1. **Use descriptive placeholders** - Show format examples
2. **Match PromptChar to context** - Underscore for general, asterisk for sensitive
3. **Set EnableLiterals based on use case** - true for display, false for storage
4. **Consider PromptPlaceholder** - Cleaner initial appearance
5. **Test across themes** - Ensure visibility in light/dark modes
6. **Provide clear labels** - Don't rely solely on visual cues
7. **Add helper text** - Explain format requirements when needed

## Troubleshooting

### Issue: Prompt Character Not Visible

**Problem:** PromptChar not showing in the input.

**Solution:** Check CSS theme is loaded and PromptChar is a visible character.

```razor
<!-- Ensure theme CSS is loaded -->
<link href="_content/Syncfusion.Blazor.Themes/material3.css" rel="stylesheet" />

<!-- Use visible prompt character -->
<SfMaskedTextBox PromptChar="_"></SfMaskedTextBox>
```

### Issue: Value Contains Unwanted Characters

**Problem:** Retrieved value includes prompt characters or literals.

**Solution:** Use `EnableLiterals="false"` and validate completeness before use.

```razor
<SfMaskedTextBox Mask="000-00-0000" 
                 EnableLiterals="false"
                 @bind-Value="@ssn">
</SfMaskedTextBox>

@code {
    private bool IsInputComplete()
    {
        // Ensure no prompt characters remain
        return !string.IsNullOrEmpty(ssn) && 
               !ssn.Contains('_') && 
               ssn.Length == 9; // Expected length without literals
    }
}
```

### Issue: Placeholder Not Showing

**Problem:** Placeholder text doesn't appear.

**Solution:** Placeholder only shows when field is empty and unfocused. Ensure Value is empty.

```razor
<SfMaskedTextBox Mask="000-00-0000" 
                 Placeholder="Enter SSN"
                 @bind-Value="@ssn">
</SfMaskedTextBox>

@code {
    private string ssn = ""; // Must be empty for placeholder to show
}
```
