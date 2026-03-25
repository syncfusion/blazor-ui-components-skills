# QR Code Generator — SfQRCodeGenerator

## Table of Contents
- [Overview](#overview)
- [Basic Usage](#basic-usage)
- [Error Correction Level](#error-correction-level)
- [QR Code with Embedded Logo](#qr-code-with-embedded-logo)
  - [Customizing Logo Size](#customizing-logo-size)
- [Customizing Color](#customizing-color)
- [Customizing Dimensions](#customizing-dimensions)
- [Customizing Display Text](#customizing-display-text)
- [OnValidationFailed Event](#onvalidationfailed-event)
- [Common Patterns](#common-patterns)

---

## Overview

`SfQRCodeGenerator` renders **2D QR codes** — a grid of dark and light blocks arranged in a square. QR codes support three character sets:

- **Numeric** — digits only
- **Alphanumeric** — uppercase letters + digits + a few symbols
- **Shift JIS8** — extended Japanese characters

QR codes use **versions 1–40**, where version 1 is 21×21 modules and each version adds 4 modules per side (version 40 = 177×177). The component **automatically selects the version** based on input length and error correction level.

---

## Basic Usage

```razor
@using Syncfusion.Blazor.BarcodeGenerator

<SfQRCodeGenerator Width="200px" Height="200px"
                   Value="https://www.syncfusion.com">
</SfQRCodeGenerator>
```

> Square dimensions are recommended for QR codes (equal `Width` and `Height`).

---

## Error Correction Level

QR codes include error correction codewords that allow the symbol to be scanned even when partially damaged or obscured. Set `ErrorCorrectionLevel` to control the tradeoff between data density and damage tolerance.

| Level | Enum Value | Recovery Capacity |
|-------|-----------|-------------------|
| Low | `ErrorCorrectionLevel.Low` | ~7% |
| Medium | `ErrorCorrectionLevel.Medium` | ~15% |
| Quartile | `ErrorCorrectionLevel.Quartile` | ~25% |
| High | `ErrorCorrectionLevel.High` | ~30% |

**Default:** `ErrorCorrectionLevel.Low`

Higher correction = larger QR code (more modules) for the same data. Use **High** when the QR code may be printed on textured surfaces or partially covered by a logo.

```razor
<SfQRCodeGenerator Width="200px" Height="200px"
                   ErrorCorrectionLevel="ErrorCorrectionLevel.High"
                   Value="https://www.syncfusion.com">
    <QRCodeGeneratorDisplayText Visibility="false"></QRCodeGeneratorDisplayText>
</SfQRCodeGenerator>
```

---

## QR Code with Embedded Logo

Embed a brand logo at the center of the QR code using `QRCodeLogo` with the `ImageSource` property. By default, the logo renders at **one-third** of the QR code's size.

```razor
@using Syncfusion.Blazor.BarcodeGenerator

<SfQRCodeGenerator Width="200px" Height="200px"
                   Value="https://www.syncfusion.com/blazor-components">
    <QRCodeGeneratorDisplayText Visibility="false"></QRCodeGeneratorDisplayText>
    <QRCodeLogo ImageSource="images/syncfusion-logo.png"></QRCodeLogo>
</SfQRCodeGenerator>
```

> **Important:** The `ErrorCorrectionLevel` property is **ignored** when a logo is embedded. The logo effectively replaces the center modules, so ensure the rest of the QR code has enough data redundancy by using a higher correction level before adding a logo.

### Customizing Logo Size

Control the logo size using `Height` and `Width` on `QRCodeLogo`. The effective size is capped at **30% of the QR code's dimensions** — if you specify a larger value, 30% of the QR size is used instead.

```razor
<SfQRCodeGenerator Width="200px" Height="200px"
                   Value="https://www.syncfusion.com">
    <QRCodeLogo Width="40" Height="40"
                ImageSource="images/syncfusion-logo.png">
    </QRCodeLogo>
</SfQRCodeGenerator>
```

> **Rule of thumb:** Keep logo size ≤ 30% of the QR code size to maintain scannability. Larger logos risk obscuring too many data modules.

---

## Customizing Color

Use `ForeColor` to change the barcode bar color. Useful when embedding a QR code in a colored layout or printed design.

```razor
<SfQRCodeGenerator Width="200px" Height="200px"
                   ForeColor="#1a237e"
                   Value="https://www.syncfusion.com">
</SfQRCodeGenerator>
```

> **Contrast note:** Always ensure sufficient contrast between `ForeColor` and the background. Light-colored QR codes on white backgrounds may not scan correctly.

---

## Customizing Dimensions

Set `Width` and `Height` as pixel values or CSS units:

```razor
<SfQRCodeGenerator Width="300px" Height="300px"
                   Value="https://www.syncfusion.com">
</SfQRCodeGenerator>
```

QR codes are always square by nature. Providing unequal width/height will stretch the rendering.

---

## Customizing Display Text

By default, a text label is shown below the QR code. Use `QRCodeGeneratorDisplayText` to:
- Change the displayed text (independent of the encoded `Value`)
- Hide the text entirely

```razor
<!-- Custom display text -->
<SfQRCodeGenerator Width="200px" Height="200px"
                   Value="https://www.syncfusion.com">
    <QRCodeGeneratorDisplayText Text="Scan to visit Syncfusion">
    </QRCodeGeneratorDisplayText>
</SfQRCodeGenerator>

<!-- Hide display text -->
<SfQRCodeGenerator Width="200px" Height="200px"
                   Value="https://www.syncfusion.com">
    <QRCodeGeneratorDisplayText Visibility="false">
    </QRCodeGeneratorDisplayText>
</SfQRCodeGenerator>
```

---

## OnValidationFailed Event

Fires when `Value` contains characters that are invalid for the QR code encoding. Handle this to inform users or prevent invalid submissions.

```razor
@using Syncfusion.Blazor.BarcodeGenerator

<SfQRCodeGenerator Width="200px" Height="200px"
                   Value="@qrValue"
                   OnValidationFailed="@HandleValidationFailed">
</SfQRCodeGenerator>

@if (!string.IsNullOrEmpty(errorMsg))
{
    <p style="color: red;">@errorMsg</p>
}

@code {
    private string qrValue = "https://www.syncfusion.com";
    private string errorMsg = "";

    private void HandleValidationFailed(ValidationFailedEventArgs args)
    {
        errorMsg = $"QR code validation failed: {args.Message}";
    }
}
```

---

## Common Patterns

### URL QR Code (website link)
```razor
<SfQRCodeGenerator Width="200px" Height="200px"
                   Value="https://www.syncfusion.com">
    <QRCodeGeneratorDisplayText Visibility="false"></QRCodeGeneratorDisplayText>
</SfQRCodeGenerator>
```

### 2FA / TOTP QR Code
```razor
<SfQRCodeGenerator Width="200px" Height="200px"
                   Value="otpauth://totp/MyApp:user@example.com?secret=BASE32SECRET&issuer=MyApp">
    <QRCodeGeneratorDisplayText Visibility="false"></QRCodeGeneratorDisplayText>
</SfQRCodeGenerator>
```

### Branded QR Code with Logo and High Error Correction
```razor
<SfQRCodeGenerator Width="250px" Height="250px"
                   ErrorCorrectionLevel="ErrorCorrectionLevel.High"
                   Value="https://www.syncfusion.com">
    <QRCodeGeneratorDisplayText Visibility="false"></QRCodeGeneratorDisplayText>
    <QRCodeLogo Width="50" Height="50"
                ImageSource="images/brand-logo.png">
    </QRCodeLogo>
</SfQRCodeGenerator>
```

> Use `ErrorCorrectionLevel.High` when embedding a logo, since the logo covers a portion of the QR code modules.

### Contact Card (vCard)
```razor
<SfQRCodeGenerator Width="200px" Height="200px"
                   Value="BEGIN:VCARD&#10;VERSION:3.0&#10;FN:John Doe&#10;TEL:+1234567890&#10;END:VCARD">
    <QRCodeGeneratorDisplayText Text="Scan for contact info"></QRCodeGeneratorDisplayText>
</SfQRCodeGenerator>
```
