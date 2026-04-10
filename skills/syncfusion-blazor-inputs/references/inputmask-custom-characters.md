# InputMask Custom Characters and Advanced Patterns

## Table of Contents
- [CustomCharacters Dictionary](#customcharacters-dictionary)
- [Creating Custom Mask Rules](#creating-custom-mask-rules)
- [Regex-Based Validation](#regex-based-validation)
- [Advanced Pattern Customization](#advanced-pattern-customization)
- [Real-World Use Cases](#real-world-use-cases)
- [Combining Custom with Standard Masks](#combining-custom-with-standard-masks)
- [Best Practices](#best-practices)

## CustomCharacters Dictionary

The `CustomCharacters` property allows you to define your own mask character rules using comma-separated character lists. This enables validation beyond the standard mask characters.

### Basic Syntax

```razor
<SfMaskedTextBox Mask="..." CustomCharacters="@customRules"></SfMaskedTextBox>

@code {
    private Dictionary<string, string> customRules = new Dictionary<string, string>
    {
        { "P", "P,p,A,a" },      // P = P or p or A or a
        { "M", "M,m" },          // M = M or m
        { "H", "0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F,a,b,c,d,e,f" }  // H = Hexadecimal digit
    };
}
```

### Dictionary Structure

**Key:** Single character that will be used in the mask  
**Value:** Comma-separated list of allowed characters

```csharp
Dictionary<string, string> customChars = new Dictionary<string, string>
{
    { "MaskChar", "char1,char2,char3" }
};
```

## Creating Custom Mask Rules

### Uppercase Letters Only

```razor
<SfMaskedTextBox Mask="XXX-XXX" CustomCharacters="@uppercaseOnly"></SfMaskedTextBox>

@code {
    private Dictionary<string, string> uppercaseOnly = new Dictionary<string, string>
    {
        { "X", "A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z" }  // Only uppercase A-Z allowed
    };
}

<!-- User can enter: ABC-XYZ -->
<!-- User cannot enter: abc-xyz (lowercase rejected) -->
```

### Lowercase Letters Only

```razor
<SfMaskedTextBox Mask="xxx-xxx" CustomCharacters="@lowercaseOnly"></SfMaskedTextBox>

@code {
    private Dictionary<string, string> lowercaseOnly = new Dictionary<string, string>
    {
        { "x", "a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z" }  // Only lowercase a-z allowed
    };
}

<!-- User can enter: abc-xyz -->
<!-- User cannot enter: ABC-XYZ (uppercase rejected) -->
```

### Hexadecimal Digits

```razor
<label>Hex Color Code:</label>
<SfMaskedTextBox Mask="#HHHHHH" CustomCharacters="@hexRules"></SfMaskedTextBox>

@code {
    private Dictionary<string, string> hexRules = new Dictionary<string, string>
    {
        { "H", "0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F,a,b,c,d,e,f" }  // Hex digit: 0-9, A-F, a-f
    };
}

<!-- User can enter: #FF5733 or #ff5733 or #Ff5733 -->
<!-- Output: #FF5733 -->
```

### Specific Digit Ranges

```razor
<label>Month (01-12):</label>
<SfMaskedTextBox Mask="MM" CustomCharacters="@monthRules"></SfMaskedTextBox>

@code {
    private Dictionary<string, string> monthRules = new Dictionary<string, string>
    {
        { "M", "0,1" }  // First digit: 0 or 1
    };
    
    // Note: This allows 00-19, you'll need additional validation
    // For true 01-12 validation, use form validation after input
}
```

### Vowels Only

```razor
<SfMaskedTextBox Mask="VVV" CustomCharacters="@vowelRules"></SfMaskedTextBox>

@code {
    private Dictionary<string, string> vowelRules = new Dictionary<string, string>
    {
        { "V", "A,E,I,O,U,a,e,i,o,u" }
    };
}

<!-- User can enter: aEi or OUa -->
<!-- Consonants rejected -->
```

### Consonants Only

```razor
<SfMaskedTextBox Mask="CCC" CustomCharacters="@consonantRules"></SfMaskedTextBox>

@code {
    private Dictionary<string, string> consonantRules = new Dictionary<string, string>
    {
        { "C", "B,C,D,F,G,H,J,K,L,M,N,P,Q,R,S,T,V,W,X,Y,Z,b,c,d,f,g,h,j,k,l,m,n,p,q,r,s,t,v,w,x,y,z" }
    };
}

<!-- User can enter: BCd or XyZ -->
<!-- Vowels rejected -->
```

## Regex-Based Validation

**Note:** CustomCharacters support both comma-separated values and inline regex patterns using square bracket notation in the mask.

### Using Regex Patterns in Mask

Instead of CustomCharacters, you can use inline regex patterns directly in the Mask property:

```razor
<SfMaskedTextBox Placeholder="Enter value" 
                 Mask="[0-2][0-9][0-9].[0-2][0-9][0-9].[0-2][0-9][0-9].[0-2][0-9][0-9]">
</SfMaskedTextBox>

<!-- Regex pattern in square brackets: [0-2] matches digits 0-2 -->
<!-- Output: 192.168.001.001 -->
```

### Common Character List Examples

```csharp
private Dictionary<string, string> customCharExamples = new Dictionary<string, string>
{
    // Digits
    { "D", "0,1,2,3,4,5,6,7,8,9" },              // Any digit
    { "N", "1,2,3,4,5,6,7,8,9" },              // Non-zero digit
    
    // Letters
    { "U", "A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z" },        // Uppercase letter
    { "l", "a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z" },      // Lowercase letter
    
    // Special patterns
    { "H", "0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F,a,b,c,d,e,f" },        // Hexadecimal
    { "V", "A,E,I,O,U,a,e,i,o,u" },       // Vowels
    { "P", "!,@,#,$,%,^,&,*" },         // Special symbols
};
```

### Complex Character Patterns

**Alphanumeric excluding similar characters:**
```razor
<SfMaskedTextBox Mask="TTTTTTTT" CustomCharacters="@safeChars"></SfMaskedTextBox>

@code {
    // Exclude confusing characters: 0, O, l, I, 1
    private Dictionary<string, string> safeChars = new Dictionary<string, string>
    {
        { "T", "2,3,4,5,6,7,8,9,A,B,C,D,E,F,G,H,J,K,L,M,N,P,Q,R,S,T,U,V,W,X,Y,Z" }
    };
}

<!-- Good for: License keys, verification codes -->
<!-- Avoids: 0 vs O, 1 vs l vs I confusion -->
```

**Business code pattern:**
```razor
<SfMaskedTextBox Mask="BBBBB-NNNNN" CustomCharacters="@businessRules"></SfMaskedTextBox>

@code {
    private Dictionary<string, string> businessRules = new Dictionary<string, string>
    {
        { "B", "A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z,0,1,2,3,4,5,6,7,8,9" },   // Any uppercase letter or digit
        { "N", "0,1,2,3,4,5,6,7,8,9" }        // Only digits
    };
}

<!-- Output: ABC12-34567 -->
```

## Advanced Pattern Customization

### License Plate Patterns

**California Format (1ABC234):**
```razor
<label>CA License Plate:</label>
<SfMaskedTextBox Mask="0XXX000" CustomCharacters="@licensePlateRules"></SfMaskedTextBox>

@code {
    private Dictionary<string, string> licensePlateRules = new Dictionary<string, string>
    {
        { "X", "A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z" }  // Uppercase letters only
    };
}

<!-- Output: 1ABC234 -->
```

**New York Format (ABC-1234):**
```razor
<SfMaskedTextBox Mask="XXX-0000" CustomCharacters="@nyPlateRules"></SfMaskedTextBox>

@code {
    private Dictionary<string, string> nyPlateRules = new Dictionary<string, string>
    {
        { "X", "A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z" }
    };
}

<!-- Output: ABC-1234 -->
```

### Product Serial Numbers

**Product code with checksum character:**
```razor
<label>Product Serial:</label>
<SfMaskedTextBox Mask="PP-YYYY-SSSS-K" CustomCharacters="@productRules"></SfMaskedTextBox>

@code {
    private Dictionary<string, string> productRules = new Dictionary<string, string>
    {
        { "P", "A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z" },      // Product type code
        { "Y", "0,1,2,3,4,5,6,7,8,9" },      // Year
        { "S", "0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z" },   // Serial number
        { "K", "0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F,X" }   // Checksum (0-9, A-F, or X)
    };
}

<!-- Output: AB-2026-X5Y9-C -->
```

### Government IDs

**Passport with country prefix:**
```razor
<SfMaskedTextBox Mask="XX0000000" CustomCharacters="@passportRules"></SfMaskedTextBox>

@code {
    private Dictionary<string, string> passportRules = new Dictionary<string, string>
    {
        { "X", "A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z" }  // Country code uppercase
    };
}

<!-- Output: US1234567 -->
```

### Medical/Lab Codes

**Lab specimen ID:**
```razor
<label>Specimen ID:</label>
<SfMaskedTextBox Mask="LLLL-000000-TT" CustomCharacters="@labRules"></SfMaskedTextBox>

@code {
    private Dictionary<string, string> labRules = new Dictionary<string, string>
    {
        { "L", "A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z" },          // Lab location code
        { "T", "A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z" }           // Test type code
    };
}

<!-- Output: NYMC-123456-BL -->
```

## Real-World Use Cases

### Case 1: Activation Codes

```razor
<h3>Software Activation</h3>
<SfMaskedTextBox Mask="KKKK-KKKK-KKKK-KKKK" CustomCharacters="@activationRules"></SfMaskedTextBox>

@code {
    private Dictionary<string, string> activationRules = new Dictionary<string, string>
    {
        // Exclude confusing characters for better user experience
        { "K", "2,3,4,5,6,7,8,9,A,B,C,D,E,F,G,H,J,K,L,M,N,P,Q,R,S,T,U,V,W,X,Y,Z" }
    };
}

<!-- Output: 2BC3-D4F5-G6H7-J8K9 -->
<!-- Excludes: 0, O, 1, I for clarity -->
```

### Case 2: Container/Shipping Codes

```razor
<label>Container Number:</label>
<SfMaskedTextBox Mask="CCCC 000000 0" CustomCharacters="@containerRules"></SfMaskedTextBox>

@code {
    private Dictionary<string, string> containerRules = new Dictionary<string, string>
    {
        { "C", "A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z" }  // Owner code (4 uppercase letters)
    };
}

<!-- Output: MSCU 123456 7 -->
<!-- Format: Owner code (4 letters) + Serial (6 digits) + Check digit (1 digit) -->
```

### Case 3: VIN (Vehicle Identification Number)

```razor
<label>VIN:</label>
<SfMaskedTextBox Mask="VVVVVVVVVVVVVVVVV" CustomCharacters="@vinRules"></SfMaskedTextBox>

@code {
    private Dictionary<string, string> vinRules = new Dictionary<string, string>
    {
        // VIN excludes I, O, Q to avoid confusion
        { "V", "0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F,G,H,J,K,L,M,N,P,R,S,T,U,V,W,X,Y,Z" }
    };
}

<!-- 17 characters, no I, O, Q -->
<!-- Output: 1HGBH41JXMN109186 -->
```

### Case 4: Tracking Numbers

**FedEx-style tracking:**
```razor
<SfMaskedTextBox Mask="0000 0000 0000"></SfMaskedTextBox>

<!-- 0 is standard mask character for required digit -->
<!-- Output: 1234 5678 9012 -->
```

**UPS-style with prefix:**
```razor
<SfMaskedTextBox Mask="1T KKK KKK KKK KKK KK" CustomCharacters="@upsRules"></SfMaskedTextBox>

@code {
    private Dictionary<string, string> upsRules = new Dictionary<string, string>
    {
        { "K", "0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z" }
    };
}

<!-- Output: 1T ABC 123 XYZ 456 78 -->
```

### Case 5: Bitcoin/Crypto Addresses (simplified)

```razor
<label>Wallet Address (Simplified):</label>
<SfMaskedTextBox Mask="HHHHHHHH-HHHHHHHH" CustomCharacters="@cryptoRules"></SfMaskedTextBox>

@code {
    private Dictionary<string, string> cryptoRules = new Dictionary<string, string>
    {
        { "H", "0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z,a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z" }  // Base58/Base64 characters
    };
}

<!-- Note: Real crypto addresses are much longer and complex -->
<!-- This is a simplified pattern for demonstration -->
```

## Combining Custom with Standard Masks

You can mix custom character rules with standard mask characters:

```razor
<SfMaskedTextBox Mask="XX-00-aaa-HHH" CustomCharacters="@mixedRules"></SfMaskedTextBox>

@code {
    private Dictionary<string, string> mixedRules = new Dictionary<string, string>
    {
        { "X", "A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z" },          // Custom: Uppercase only
        { "H", "0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F,a,b,c,d,e,f" }     // Custom: Hexadecimal
    };
    
    // Standard mask characters still work:
    // 0 = required digit
    // a = optional alphanumeric
}

<!-- Output: AB-12-xyz-3F5 -->
```

### Mixed Pattern Example

```razor
<label>Custom Product Code:</label>
<SfMaskedTextBox Mask="LL-0000-AAA-HH" 
                 CustomCharacters="@productCodeRules">
</SfMaskedTextBox>

@code {
    private Dictionary<string, string> productCodeRules = new Dictionary<string, string>
    {
        { "H", "0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F" }  // Hex for checksum
    };
    
    // L = standard mask (required letter)
    // 0 = standard mask (required digit)
    // A = standard mask (required alphanumeric)
    // H = custom (hex digit)
}

<!-- Output: AB-1234-XY5-3F -->
```

## Best Practices

### 1. Choose Clear Custom Characters

```csharp
// Good: Meaningful character choices
{ "H", "0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F,a,b,c,d,e,f" }  // H for Hex
{ "U", "A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z" }        // U for Uppercase
{ "V", "A,E,I,O,U,a,e,i,o,u" } // V for Vowel

// Avoid: Confusing or arbitrary
{ "Z", "0,1,2,3,4,5,6,7,8,9" }        // Z doesn't suggest "digit"
{ "Q", "A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z" }        // Q doesn't suggest "uppercase"
```

### 2. Document Custom Patterns

```razor
@code {
    // Custom character rules for license key validation
    // K = Alphanumeric excluding confusing characters (0,O,1,I)
    // Used to prevent user input errors during activation
    private Dictionary<string, string> licenseKeyRules = new Dictionary<string, string>
    {
        { "K", "2,3,4,5,6,7,8,9,A,B,C,D,E,F,G,H,J,K,L,M,N,P,Q,R,S,T,U,V,W,X,Y,Z" }
    };
}
```

### 3. Validate Custom Character Lists

Test your custom characters before using them:

```csharp
@code {
    private void TestCustomCharacters()
    {
        string validChars = "2,3,4,5,6,7,8,9,A,B,C,D,E,F,G,H,J,K,L,M,N,P,Q,R,S,T,U,V,W,X,Y,Z";
        var charList = validChars.Split(',');
        
        // Test valid inputs
        Assert.IsTrue(charList.Contains("5"));  // Valid
        Assert.IsTrue(charList.Contains("A"));  // Valid
        
        // Test invalid inputs
        Assert.IsFalse(charList.Contains("0")); // Invalid
        Assert.IsFalse(charList.Contains("I")); // Invalid
    }
}
```

### 4. Provide User Guidance

```razor
<div class="form-group">
    <label>Activation Code:</label>
    <small class="form-text">Format: XXXX-XXXX-XXXX (letters and numbers, no 0, O, 1, I)</small>
    <SfMaskedTextBox Mask="KKKK-KKKK-KKKK" 
                     CustomCharacters="@activationRules"
                     Placeholder="2BC3-D4F5-G6H7">
    </SfMaskedTextBox>
</div>

@code {
    private Dictionary<string, string> activationRules = new Dictionary<string, string>
    {
        { "K", "2,3,4,5,6,7,8,9,A,B,C,D,E,F,G,H,J,K,L,M,N,P,Q,R,S,T,U,V,W,X,Y,Z" }
    };
}
```

### 5. Consider Case Sensitivity

```csharp
// Case-insensitive (both uppercase and lowercase accepted)
{ "B", "A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z,a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z" }

// Case-sensitive uppercase only
{ "U", "A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z" }

// Case-sensitive lowercase only
{ "l", "a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z" }
```

### 6. Keep Patterns Simple

```csharp
// Good: Simple, clear character list
{ "H", "0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F" }

// Note: CustomCharacters validates individual characters
// Use form validation for complex business logic instead
// Don't rely on CustomCharacters for multi-character validation rules
```

### 7. Combine with Form Validation

CustomCharacters validate individual characters. For complex business logic, use additional form validation:

```razor
<EditForm Model="@model" OnValidSubmit="HandleSubmit">
    <DataAnnotationsValidator />
    
    <SfMaskedTextBox Mask="XX-0000" 
                     @bind-Value="@model.ProductCode"
                     CustomCharacters="@rules">
    </SfMaskedTextBox>
    
    <ValidationMessage For="@(() => model.ProductCode)" />
    <button type="submit">Submit</button>
</EditForm>

@code {
    private ProductModel model = new ProductModel();
    
    private Dictionary<string, string> rules = new Dictionary<string, string>
    {
        { "X", "A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z" }
    };
    
    public class ProductModel
    {
        [Required]
        [RegularExpression(@"^[A-Z]{2}-\d{4}$", ErrorMessage = "Invalid format")]
        [CustomValidation(typeof(ProductModel), nameof(ValidateProductCode))]
        public string ProductCode { get; set; }
        
        public static ValidationResult ValidateProductCode(string code, ValidationContext context)
        {
            // Additional business logic validation
            if (code.StartsWith("00"))
                return new ValidationResult("Product code cannot start with 00");
            
            return ValidationResult.Success;
        }
    }
}
```

## Troubleshooting

### Issue: Custom Character Not Working

**Problem:** Custom character rule not being applied.

**Solution:** Ensure the custom character doesn't conflict with standard mask characters. Use unique characters and use comma-separated format.

```csharp
// Avoid: Using standard mask character
{ "0", "A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z" }  // ❌ 0 is already a standard mask character

// Good: Use unique character with comma-separated values
{ "X", "A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z" }  // ✅ X is custom with proper format
```

### Issue: Input Not Being Accepted

**Problem:** Valid input rejected or characters not appearing.

**Solution:** Use comma-separated format for CustomCharacters. Ensure each character is listed separately.

```csharp
// Wrong: Range format (not supported in CustomCharacters)
{ "H", "0-9,A-F" }  // ❌ Ranges not supported

// Correct: Comma-separated individual characters
{ "H", "0,1,2,3,4,5,6,7,8,9,A,B,C,D,E,F" }  // ✅ Valid format

// Or use inline regex in mask instead:
// Mask="[0-9A-F]" - regex directly in the mask property
```

### Issue: Performance with Many Characters

**Problem:** Input sluggish or slow response.

**Solution:** Use shorter character lists or consider using inline regex in the mask instead of CustomCharacters.

```csharp
// Slower: Very long comma-separated list
{ "C", "A,B,C,D,E,F,G,H,I,J,K,L,M,N,O,P,Q,R,S,T,U,V,W,X,Y,Z,0,1,2,3,4,5,6,7,8,9" }  // ❌ Long list

// Better: Use inline regex in mask for complex patterns
// Mask="[A-Z0-9]" // ✅ More efficient
```
