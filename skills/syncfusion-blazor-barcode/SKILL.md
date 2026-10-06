---
name: syncfusion-blazor-barcode
description: "Build and troubleshoot barcode generation in Blazor using SfBarcodeGenerator, SfQRCodeGenerator, SfDataMatrixGenerator, and SfDotCodeGenerator. Trigger for 1D barcodes (Code39, Code128, Codabar), QR codes with logo and error correction, Data Matrix, DotCode, GS1-compliant symbologies (GS1 Code 128, ITF-14, GS1 DataBar family, GS1 QR Code, GS1 Data Matrix, GS1 DotCode) for retail/logistics/inventory traceability, checksum validation, and exporting barcodes to images in Syncfusion Blazor apps."
metadata:
  author: "Syncfusion Inc"
  version: "34.1.29"
  category: "Data Visualization"
---

# Syncfusion Blazor Barcode Generator

Syncfusion's Blazor Barcode Generator package (`Syncfusion.Blazor.BarcodeGenerator`) provides four components for generating barcodes in Blazor applications:

| Component | Use For |
|-----------|---------|
| `SfBarcodeGenerator` | 1D linear barcodes (Code39, Code128, Codabar, etc.) plus GS1 Code 128, ITF-14, and the full GS1 DataBar family |
| `SfQRCodeGenerator` | QR codes with optional logo embedding, plus GS1 QR Code (`EnableGS1`) |
| `SfDataMatrixGenerator` | 2D Data Matrix codes for compact encoding, plus GS1 Data Matrix (`EnableGS1`) |
| `SfDotCodeGenerator` | High-density dot-matrix DotCode symbols, plus GS1 DotCode (`EnableGS1`) |

All four work in **Blazor Server**, **Blazor WebAssembly**, and **Blazor Web App** (.NET 8+).

---

## When to Use This Skill

- User needs to render or display a barcode in a Blazor component
- User asks about QR code generation (e.g., product links, 2FA, contact cards)
- User needs Data Matrix codes (pharmaceutical, logistics, label printing)
- User needs DotCode symbols for high-speed industrial/inkjet printing or product serialization
- User is configuring `SfBarcodeGenerator`, `SfQRCodeGenerator`, `SfDataMatrixGenerator`, or `SfDotCodeGenerator`
- User asks about exporting a barcode as an image (JPG/PNG) or Base64
- User asks about barcode types, error correction, checksums, or validation events
- User is embedding a logo in a QR code
- User asks about **GS1 standards** or GS1-compliant barcodes: GS1 Code 128, ITF-14, GS1 DataBar (Omnidirectional, Stacked, Stacked Omnidirectional, Limited, Expanded, Expanded Stacked), GS1 QR Code, GS1 Data Matrix, or GS1 DotCode
- User is building **retail, logistics, warehouse, inventory management, product traceability, or supply chain** labeling features
- User asks about GS1 Application Identifiers (AIs), FNC1 separators, `EnableGS1`, or GTIN/SSCC check-digit calculation

---

## Navigation Guide

### Setting Up the Project
📄 **Read:** [references/getting-started.md](references/getting-started.md)
- NuGet package installation
- `_Imports.razor` namespace configuration
- Service registration in `Program.cs`
- Stylesheet and script references
- Setup for Blazor Server, WebAssembly, and Web App
- Adding the first barcode component to a page

### 1D Barcode Types and Configuration
📄 **Read:** [references/barcode-types.md](references/barcode-types.md)
- All supported `BarcodeType` enum values with descriptions
- Code39, Code39 Extended, Code11, Code32, Code93, Codabar, Code128
- Choosing the right type for your use case
- `EnableCheckSum` configuration
- `OnValidationFailed` event handling

### QR Code Generator
📄 **Read:** [references/qr-code-generator.md](references/qr-code-generator.md)
- Basic QR code usage
- Error correction levels (L, M, Q, H)
- Embedding a logo image in the QR code center
- Customizing logo size
- Color, dimension, and display text customization
- `OnValidationFailed` event

### Data Matrix Generator
📄 **Read:** [references/data-matrix-generator.md](references/data-matrix-generator.md)
- Basic Data Matrix code usage
- Color, dimension, and display text customization
- Common use cases (pharmaceuticals, labels)
- `OnValidationFailed` event

### DotCode Generator
📄 **Read:** [references/dotcode-generator.md](references/dotcode-generator.md)
- Basic `SfDotCodeGenerator` usage
- Numeric, alphanumeric, byte, and GS1 encoding modes
- GS1 DotCode via `EnableGS1` for pharmaceutical/healthcare serialization
- Color and dimension customization
- `OnValidationFailed` event and error causes

### GS1 Barcode Standards (Retail, Logistics, Supply Chain)
📄 **Read:** [references/gs1-barcodes.md](references/gs1-barcodes.md)
- GS1 Application Identifier (AI) syntax and validation rules
- GS1 Code 128 (`BarcodeType.GS1Code128`) for shipping/batch/expiry labels
- ITF-14 (`BarcodeType.ITF14`) for carton/case-level GTIN tracking
- Full GS1 DataBar family: Omnidirectional, Stacked, Stacked Omnidirectional, Limited, Expanded, Expanded Stacked
- GS1 QR Code and GS1 Data Matrix via `EnableGS1`
- GS1 DotCode overview (cross-links to dotcode-generator.md)
- Choosing the right GS1 symbology for retail, inventory, and product traceability use cases

### Exporting and Customizing Barcodes
📄 **Read:** [references/export-and-customization.md](references/export-and-customization.md)
- Export barcode to image file (JPG/PNG)
- Export as Base64 string
- `ForeColor` for custom barcode color
- `Width` / `Height` for sizing
- `BarcodeGeneratorDisplayText` for label text
- Applies to all four generator types, including GS1-enabled modes

---

## Quick Start Examples

### 1D Barcode
```razor
@using Syncfusion.Blazor.BarcodeGenerator

<SfBarcodeGenerator Width="200px" Height="150px"
                    Type="@BarcodeType.Code128"
                    Value="SYNCFUSION">
</SfBarcodeGenerator>
```

### QR Code
```razor
@using Syncfusion.Blazor.BarcodeGenerator

<SfQRCodeGenerator Width="200px" Height="200px"
                   Value="https://www.syncfusion.com">
</SfQRCodeGenerator>
```

### Data Matrix
```razor
@using Syncfusion.Blazor.BarcodeGenerator

<SfDataMatrixGenerator Width="200" Height="150"
                       Value="SYNCFUSION">
</SfDataMatrixGenerator>
```

### DotCode
```razor
@using Syncfusion.Blazor.BarcodeGenerator

<SfDotCodeGenerator Width="300" Height="250"
                    Value="Product Serialization">
</SfDotCodeGenerator>
```

### GS1 Code 128 (GS1-compliant 1D barcode)
```razor
@using Syncfusion.Blazor.BarcodeGenerator

<SfBarcodeGenerator Width="300px" Height="200px"
                    Type="@BarcodeType.GS1Code128"
                    Value="(01)09506000134352(17)261231(10)ABC123">
    <BarcodeGeneratorDisplayText Text="Product Serialization" />
</SfBarcodeGenerator>
```

### GS1 QR Code (retail/traceability)
```razor
@using Syncfusion.Blazor.BarcodeGenerator

<SfQRCodeGenerator Width="250px" Height="250px"
                   Value="(01)09506000134352(17)271231(10)ABC123"
                   EnableGS1="true">
</SfQRCodeGenerator>
```

---

## Key Properties

| Property | Applies To | Description |
|----------|-----------|-------------|
| `Value` | All | The data string to encode |
| `Width` | All | Width of the barcode (px or %) |
| `Height` | All | Height of the barcode (px or %) |
| `Type` | `SfBarcodeGenerator` | `BarcodeType` enum — selects the 1D symbology, including `GS1Code128`, `ITF14`, and the GS1 DataBar variants |
| `ForeColor` | All | Color of the barcode bars (e.g., `"red"`, `"#333"`) |
| `EnableCheckSum` | `SfBarcodeGenerator` | Adds/validates checksum digit (default `true` for Code39) |
| `ErrorCorrectionLevel` | `SfQRCodeGenerator` | QR error recovery: Low, Medium, Quartile, High |
| `EnableGS1` | `SfQRCodeGenerator`, `SfDataMatrixGenerator`, `SfDotCodeGenerator` | Switches the component into GS1 mode, encoding `Value` as `(AI)data` pairs with FNC1 handling |
| `OnValidationFailed` | All | Event triggered when `Value` contains invalid characters, malformed AI syntax, or an invalid check digit |

---

## Common Use Cases

| Need | Component | Read |
|------|-----------|------|
| Product/inventory labels | `SfBarcodeGenerator` (Code128/Code39) | barcode-types.md |
| URL / contact / 2FA QR code | `SfQRCodeGenerator` | qr-code-generator.md |
| Branded QR code with logo | `SfQRCodeGenerator` + `QRCodeLogo` | qr-code-generator.md |
| Pharmaceutical / shipping labels | `SfDataMatrixGenerator` | data-matrix-generator.md |
| Industrial/inkjet product serialization | `SfDotCodeGenerator` | dotcode-generator.md |
| Shipping/batch/expiry label (GS1) | `SfBarcodeGenerator` (`GS1Code128`) | gs1-barcodes.md |
| Carton/case GTIN tracking | `SfBarcodeGenerator` (`ITF14`) | gs1-barcodes.md |
| Retail POS omnidirectional scan | `SfBarcodeGenerator` (`GS1DataBarOmnidirectional`) | gs1-barcodes.md |
| Small package / limited space GTIN | `SfBarcodeGenerator` (`GS1DataBarStacked`/`GS1DataBarLimited`) | gs1-barcodes.md |
| Variable-measure item or coupon (multi-AI) | `SfBarcodeGenerator` (`GS1DataBarExpanded`/`ExpandedStacked`) | gs1-barcodes.md |
| Supply chain / product traceability QR | `SfQRCodeGenerator` + `EnableGS1` | gs1-barcodes.md |
| Compact GS1 label (pharma/manufacturing) | `SfDataMatrixGenerator` + `EnableGS1` | gs1-barcodes.md |
| GS1 serialization on industrial printers | `SfDotCodeGenerator` + `EnableGS1` | dotcode-generator.md / gs1-barcodes.md |
| Download barcode as image | `.Export()` method | export-and-customization.md |
| Embed barcode in email/PDF | `.ExportAsBase64Image()` | export-and-customization.md |
