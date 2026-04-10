# InputMask Mask Patterns and Configuration

## Table of Contents
- [Standard Mask Characters](#standard-mask-characters)
- [Predefined Patterns](#predefined-patterns)
- [Mask Property Configuration](#mask-property-configuration)
- [Literal Characters](#literal-characters)
- [Optional vs Required Positions](#optional-vs-required-positions)
- [Complex Pattern Examples](#complex-pattern-examples)
- [International Format Patterns](#international-format-patterns)
- [Pattern Validation Rules](#pattern-validation-rules)

## Standard Mask Characters

The InputMask component supports the following standard mask characters:

| Character | Description | Allows | Example |
|-----------|-------------|--------|---------|
| `0` | Required digit | 0-9 | `000` → Must enter 3 digits |
| `9` | Optional digit | 0-9 or space | `999` → Can skip digits |
| `L` | Required letter | A-Z, a-z | `LLL` → Must enter 3 letters |
| `?` | Optional letter | A-Z, a-z or space | `???` → Can skip letters |
| `A` | Required alphanumeric | A-Z, a-z, 0-9 | `AAA` → Letters or digits |
| `a` | Optional alphanumeric | A-Z, a-z, 0-9 or space | `aaa` → Optional letters/digits |
| `&` | Required character | Any printable character | `&&&` → Any characters |
| `C` | Optional character | Any printable character or space | `CCC` → Optional any character |
| `#` | Digit or plus/minus | 0-9, +, - | `#00` → +123 or -456 |
| `\` | Escape character | Treats next char as literal | `\0\0` → Shows "00" literally |

## Predefined Patterns

### Phone Numbers

**US Phone Number:**
```razor
<SfMaskedTextBox Mask="(000) 000-0000"></SfMaskedTextBox>
<!-- Output: (555) 123-4567 -->
```

**International Format:**
```razor
<SfMaskedTextBox Mask="+0 (000) 000-0000"></SfMaskedTextBox>
<!-- Output: +1 (555) 123-4567 -->
```

**Extension Support:**
```razor
<SfMaskedTextBox Mask="(000) 000-0000 x0000"></SfMaskedTextBox>
<!-- Output: (555) 123-4567 x1234 -->
```

### Social Security Number

```razor
<SfMaskedTextBox Mask="000-00-0000"></SfMaskedTextBox>
<!-- Output: 123-45-6789 -->
```

### Date Formats

**MM/DD/YYYY:**
```razor
<SfMaskedTextBox Mask="00/00/0000"></SfMaskedTextBox>
<!-- Output: 12/31/2026 -->
```

**DD-MM-YYYY:**
```razor
<SfMaskedTextBox Mask="00-00-0000"></SfMaskedTextBox>
<!-- Output: 31-12-2026 -->
```

**YYYY.MM.DD:**
```razor
<SfMaskedTextBox Mask="0000.00.00"></SfMaskedTextBox>
<!-- Output: 2026.12.31 -->
```

### Time Formats

**24-Hour Format:**
```razor
<SfMaskedTextBox Mask="00:00"></SfMaskedTextBox>
<!-- Output: 23:59 -->
```

**12-Hour with AM/PM:**
```razor
<SfMaskedTextBox Mask="00:00 LL"></SfMaskedTextBox>
<!-- Output: 11:30 PM -->
```

### Credit Card

**Standard 16-digit:**
```razor
<SfMaskedTextBox Mask="0000-0000-0000-0000"></SfMaskedTextBox>
<!-- Output: 4532-1234-5678-9010 -->
```

**American Express (15-digit):**
```razor
<SfMaskedTextBox Mask="0000-000000-00000"></SfMaskedTextBox>
<!-- Output: 3782-822463-10005 -->
```

### Postal Codes

**US ZIP Code:**
```razor
<SfMaskedTextBox Mask="00000"></SfMaskedTextBox>
<!-- Output: 90210 -->
```

**US ZIP+4:**
```razor
<SfMaskedTextBox Mask="00000-0000"></SfMaskedTextBox>
<!-- Output: 90210-1234 -->
```

**Canadian Postal Code:**
```razor
<SfMaskedTextBox Mask="L0L 0L0"></SfMaskedTextBox>
<!-- Output: K1A 0B1 -->
```

**UK Postal Code:**
```razor
<SfMaskedTextBox Mask="LL00 0LL"></SfMaskedTextBox>
<!-- Output: SW1A 1AA -->
```

## Mask Property Configuration

### Basic Mask Setup

```razor
<SfMaskedTextBox Mask="000-000-0000" 
                 Placeholder="Enter phone number"
                 @bind-Value="@value">
</SfMaskedTextBox>

@code {
    private string value = "";
}
```

### Dynamic Mask Changes

```razor
<select @onchange="OnMaskTypeChanged">
    <option value="phone">Phone</option>
    <option value="ssn">SSN</option>
    <option value="zip">ZIP Code</option>
</select>

<SfMaskedTextBox Mask="@currentMask" @bind-Value="@value"></SfMaskedTextBox>

@code {
    private string currentMask = "(000) 000-0000";
    private string value = "";
    
    private void OnMaskTypeChanged(ChangeEventArgs e)
    {
        switch (e.Value?.ToString())
        {
            case "phone":
                currentMask = "(000) 000-0000";
                break;
            case "ssn":
                currentMask = "000-00-0000";
                break;
            case "zip":
                currentMask = "00000";
                break;
        }
        value = ""; // Clear value when mask changes
    }
}
```

## Literal Characters

Literal characters are fixed characters in the mask that appear automatically and cannot be changed by the user.

### Common Literals

**Parentheses and Hyphens:**
```razor
<SfMaskedTextBox Mask="(000) 000-0000"></SfMaskedTextBox>
<!-- Literals: ( ) - -->
<!-- User enters: 5551234567 -->
<!-- Displays: (555) 123-4567 -->
```

**Slashes and Dots:**
```razor
<SfMaskedTextBox Mask="00/00/0000"></SfMaskedTextBox>
<!-- Literals: / / -->
<!-- User enters: 12312026 -->
<!-- Displays: 12/31/2026 -->
```

### Escaping Mask Characters

Use backslash `\` to treat a mask character as a literal:

```razor
<SfMaskedTextBox Mask="ID: \0\0\0-000"></SfMaskedTextBox>
<!-- Shows "ID: 000-" literally, then accepts 3 digits -->
<!-- Output: ID: 000-123 -->
```

**Text with Mask Characters:**
```razor
<SfMaskedTextBox Mask="Code: AAA\-000"></SfMaskedTextBox>
<!-- The hyphen is literal, not part of mask -->
<!-- Output: Code: ABC-123 -->
```

## Optional vs Required Positions

### Required Positions (0, L, A, &)

User **must** enter a character at these positions:

```razor
<SfMaskedTextBox Mask="000-AAA"></SfMaskedTextBox>
<!-- Must enter 3 digits and 3 alphanumeric characters -->
<!-- Valid: 123-ABC -->
<!-- Invalid: 12-ABC (missing digit) -->
```

### Optional Positions (9, ?, a, C)

User **can** skip these positions:

```razor
<SfMaskedTextBox Mask="999-aaa"></SfMaskedTextBox>
<!-- Can enter: 123-ABC or 12-AB or 1-A -->
<!-- All are valid -->
```

### Mixed Required and Optional

```razor
<SfMaskedTextBox Mask="000-999"></SfMaskedTextBox>
<!-- First 3 digits required, last 3 optional -->
<!-- Valid: 123-456 or 123- or 123-4 -->
```

**Phone with Optional Extension:**
```razor
<SfMaskedTextBox Mask="(000) 000-0000 x9999"></SfMaskedTextBox>
<!-- Phone required, extension optional -->
<!-- Valid: (555) 123-4567 -->
<!-- Valid: (555) 123-4567 x1234 -->
```

## Complex Pattern Examples

### License Plate Patterns

**Standard US Format:**
```razor
<SfMaskedTextBox Mask="LLL-0000"></SfMaskedTextBox>
<!-- Output: ABC-1234 -->
```

**Mixed Format:**
```razor
<SfMaskedTextBox Mask="0AAA000"></SfMaskedTextBox>
<!-- Output: 1ABC234 -->
```

### Product Codes

**SKU Format:**
```razor
<SfMaskedTextBox Mask="LL-0000-AAA"></SfMaskedTextBox>
<!-- Output: AB-1234-X5Y -->
```

**Serial Number:**
```razor
<SfMaskedTextBox Mask="AAA-AAA-AAA-AAA"></SfMaskedTextBox>
<!-- Output: XYZ-123-ABC-789 -->
```

### Employee/Customer IDs

**Employee ID:**
```razor
<SfMaskedTextBox Mask="EMP-00000"></SfMaskedTextBox>
<!-- Output: EMP-12345 -->
```

**Customer Number:**
```razor
<SfMaskedTextBox Mask="C0000-L00"></SfMaskedTextBox>
<!-- Output: C1234-A56 -->
```

### IP Address

```razor
<SfMaskedTextBox Mask="000.000.000.000"></SfMaskedTextBox>
<!-- Output: 192.168.001.001 -->
```

**Note:** For true IP validation (0-255 per octet), use CustomCharacters (see inputmask-custom-characters.md).

### MAC Address

```razor
<SfMaskedTextBox Mask="AA:AA:AA:AA:AA:AA"></SfMaskedTextBox>
<!-- Output: 1A:2B:3C:4D:5E:6F -->
```

### GUID/UUID Format

**Short GUID:**
```razor
<SfMaskedTextBox Mask="AAAAAAAA-AAAA-AAAA-AAAA-AAAAAAAAAAAA"></SfMaskedTextBox>
<!-- Output: 12345678-90AB-CDEF-1234-567890ABCDEF -->
```

## International Format Patterns

### International Bank Account Number (IBAN)

**Generic IBAN:**
```razor
<SfMaskedTextBox Mask="LL00 AAAA AAAA AAAA AAAA AAAA AAA"></SfMaskedTextBox>
<!-- Output: GB82 WEST 1234 5698 7654 32 -->
```

### Passport Numbers

**US Passport:**
```razor
<SfMaskedTextBox Mask="A00000000"></SfMaskedTextBox>
<!-- Output: C12345678 -->
```

**UK Passport:**
```razor
<SfMaskedTextBox Mask="000000000"></SfMaskedTextBox>
<!-- Output: 123456789 -->
```

### International Phone Numbers

**Germany:**
```razor
<SfMaskedTextBox Mask="+00 (000) 000-0000"></SfMaskedTextBox>
<!-- Output: +49 (030) 123-4567 -->
```

**France:**
```razor
<SfMaskedTextBox Mask="+00 0 00 00 00 00"></SfMaskedTextBox>
<!-- Output: +33 1 23 45 67 89 -->
```

**Japan:**
```razor
<SfMaskedTextBox Mask="+00-00-0000-0000"></SfMaskedTextBox>
<!-- Output: +81-03-1234-5678 -->
```

## Pattern Validation Rules

### Validation Order

1. **Character Type Validation:** Checks if input matches mask character type (digit, letter, alphanumeric)
2. **Required vs Optional:** Validates required positions are filled
3. **Literal Matching:** Ensures literals are in correct positions
4. **Length Validation:** Confirms input length matches mask length

### Incomplete Input Handling

```razor
<SfMaskedTextBox Mask="000-000-0000" @bind-Value="@phone"></SfMaskedTextBox>

@code {
    private string phone = "";
    
    // If user enters incomplete: "555-12"
    // Value will be: "555-12_-____" (with prompt characters)
    // Check for completion before submission
    
    private bool IsPhoneComplete()
    {
        return phone.Length == 12 && !phone.Contains('_');
    }
}
```

### Best Practices

**1. Match Mask to Expected Input:**
```razor
<!-- Good: Exact format -->
<SfMaskedTextBox Mask="000-00-0000"></SfMaskedTextBox>

<!-- Avoid: Too flexible -->
<SfMaskedTextBox Mask="aaaaaaaaa"></SfMaskedTextBox>
```

**2. Use Descriptive Placeholders:**
```razor
<SfMaskedTextBox Mask="(000) 000-0000" 
                 Placeholder="(555) 123-4567">
</SfMaskedTextBox>
```

**3. Provide Visual Feedback:**
```razor
<SfMaskedTextBox Mask="000-00-0000" 
                 FloatLabelType="FloatLabelType.Auto"
                 Placeholder="SSN">
</SfMaskedTextBox>
```

**4. Validate Completeness:**
```razor
@code {
    private bool IsInputComplete(string maskedValue, string mask)
    {
        if (string.IsNullOrEmpty(maskedValue)) return false;
        
        // Check if all required positions are filled
        return !maskedValue.Contains('_') && 
               maskedValue.Length == mask.Replace("\\", "").Length;
    }
}
```

## Troubleshooting Common Issues

### Issue: User Input Not Accepted

**Problem:** User types but nothing appears.

**Solution:** Ensure mask character matches input type.
```razor
<!-- Wrong: Using letter mask for numbers -->
<SfMaskedTextBox Mask="LLL"></SfMaskedTextBox>

<!-- Correct: Use digit mask for numbers -->
<SfMaskedTextBox Mask="000"></SfMaskedTextBox>
```

### Issue: Literals Not Appearing

**Problem:** Fixed characters don't show automatically.

**Solution:** Use non-mask characters as literals or escape them properly.
```razor
<!-- Literals appear automatically -->
<SfMaskedTextBox Mask="(000) 000-0000"></SfMaskedTextBox>
```

### Issue: Value Contains Underscores

**Problem:** Retrieved value has `_` characters.

**Solution:** The prompt character appears in incomplete input. Either:
1. Validate completeness before submission
2. Strip prompt characters: `value.Replace("_", "")`
3. Configure `EnableLiterals="false"` to exclude literals and prompts

See [inputmask-prompt-configuration.md](inputmask-prompt-configuration.md) for details.
