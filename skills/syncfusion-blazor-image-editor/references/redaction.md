# Redaction in the Image Editor

## Overview

Redaction allows you to obscure or hide sensitive information in images by applying blur or pixelate effects to specific regions. The Image Editor provides comprehensive redaction capabilities for privacy protection and data anonymization.

## Table of Contents
- [Drawing Redactions](#drawing-redactions)
- [Redaction Types](#redaction-types)
- [Managing Redactions](#managing-redactions)
- [Redaction Settings](#redaction-settings)
- [Use Cases](#use-cases)

## Drawing Redactions

### DrawRedactAsync Method

The `DrawRedactAsync` method creates redacted regions on the image with blur or pixelate effects.

**Method Signature:**
```csharp
Task<bool> DrawRedactAsync(
    RedactType redactType,
    double startX = -1,
    double startY = -1,
    double width = -1,
    double height = -1,
    int value = -1
)
```

**Parameters:**
- **redactType** (RedactType): Type of redaction - `Blur` or `Pixelate`
- **startX** (double): X-coordinate of redaction area (defaults to image center if -1)
- **startY** (double): Y-coordinate of redaction area (defaults to image center if -1)
- **width** (double): Width of redaction area (defaults to 100 if -1)
- **height** (double): Height of redaction area (defaults to 50 if -1)
- **value** (int): Blur intensity (for Blur type) or pixel size (for Pixelate type), defaults to 20 if -1

### Basic Redaction Example

```csharp
@using Syncfusion.Blazor.ImageEditor

<SfImageEditor @ref="ImageEditor" Height="500px">
    <ImageEditorEvents Created="OnCreated"></ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;

    private async void OnCreated()
    {
        await ImageEditor.OpenAsync("YOUR_IMAGE_URL");
    }

    private async Task AddBlurRedaction()
    {
        ImageDimension dim = await ImageEditor.GetImageDimensionAsync();
        
        // Add blur redaction at specific location
        await ImageEditor.DrawRedactAsync(
            redactType: RedactType.Blur,
            startX: dim.X.Value + 50,
            startY: dim.Y.Value + 50,
            width: 200,
            height: 100,
            value: 30  // Blur intensity
        );
    }
}
```

## Redaction Types

### RedactType Enum

The `RedactType` enum defines the type of redaction effect:

```csharp
public enum RedactType
{
    Blur,      // Applies Gaussian blur effect
    Pixelate   // Applies pixelation effect
}
```

### Blur Redaction

Applies a Gaussian blur effect to obscure content:

```csharp
private async Task ApplyBlurRedaction()
{
    ImageDimension dim = await ImageEditor.GetImageDimensionAsync();
    
    // Light blur (value: 10-20)
    await ImageEditor.DrawRedactAsync(
        RedactType.Blur,
        dim.X.Value + 100,
        dim.Y.Value + 100,
        150, 80, 15
    );
    
    // Medium blur (value: 20-40)
    await ImageEditor.DrawRedactAsync(
        RedactType.Blur,
        dim.X.Value + 100,
        dim.Y.Value + 200,
        150, 80, 30
    );
    
    // Heavy blur (value: 40-100)
    await ImageEditor.DrawRedactAsync(
        RedactType.Blur,
        dim.X.Value + 100,
        dim.Y.Value + 300,
        150, 80, 60
    );
}
```

**Blur Intensity Guide:**
- **10-20**: Light blur - text partially readable
- **20-40**: Medium blur - content obscured but recognizable
- **40-100**: Heavy blur - content completely unreadable

### Pixelate Redaction

Applies a pixelation effect to hide details:

```csharp
private async Task ApplyPixelateRedaction()
{
    ImageDimension dim = await ImageEditor.GetImageDimensionAsync();
    
    // Small pixels (value: 5-15)
    await ImageEditor.DrawRedactAsync(
        RedactType.Pixelate,
        dim.X.Value + 300,
        dim.Y.Value + 100,
        150, 80, 10
    );
    
    // Medium pixels (value: 15-30)
    await ImageEditor.DrawRedactAsync(
        RedactType.Pixelate,
        dim.X.Value + 300,
        dim.Y.Value + 200,
        150, 80, 20
    );
    
    // Large pixels (value: 30-50)
    await ImageEditor.DrawRedactAsync(
        RedactType.Pixelate,
        dim.X.Value + 300,
        dim.Y.Value + 300,
        150, 80, 40
    );
}
```

**Pixel Size Guide:**
- **5-15**: Small pixels - basic obscuring
- **15-30**: Medium pixels - good privacy protection
- **30-50**: Large pixels - maximum privacy

## Managing Redactions

### Get All Redactions

Retrieve all redactions applied to the image:

```csharp
private async Task ListRedactions()
{
    RedactSettings[] redacts = await ImageEditor.GetRedactsAsync();
    
    if (redacts != null && redacts.Length > 0)
    {
        foreach (var redact in redacts)
        {
            Console.WriteLine($"Redaction ID: {redact.ID}");
            Console.WriteLine($"Type: {redact.Type}");
            Console.WriteLine($"Position: ({redact.StartX}, {redact.StartY})");
            Console.WriteLine($"Size: {redact.Width}x{redact.Height}");
            
            if (redact.Type == RedactType.Blur)
            {
                Console.WriteLine($"Blur Intensity: {redact.BlurIntensity}");
            }
            else
            {
                Console.WriteLine($"Pixel Size: {redact.PixelSize}");
            }
        }
    }
}
```

### Select Redaction

Select a redaction by its ID to modify or interact with it:

```csharp
private async Task SelectRedactionById(string redactId)
{
    bool success = await ImageEditor.SelectRedactAsync(redactId);
    
    if (success)
    {
        // Redaction is now selected and can be moved or resized
        Console.WriteLine("Redaction selected successfully");
    }
}
```

### Update Redaction

Modify existing redaction properties:

```csharp
private async Task UpdateRedactionProperties()
{
    RedactSettings[] redacts = await ImageEditor.GetRedactsAsync();
    
    if (redacts != null && redacts.Length > 0)
    {
        RedactSettings redact = redacts[0];
        
        // Update redaction properties
        redact.Width = 250;
        redact.Height = 150;
        
        if (redact.Type == RedactType.Blur)
        {
            redact.BlurIntensity = 50;  // Increase blur
        }
        else
        {
            redact.PixelSize = 30;  // Increase pixel size
        }
        
        bool success = await ImageEditor.UpdateRedactAsync(redact, isSelected: true);
        
        if (success)
        {
            Console.WriteLine("Redaction updated successfully");
        }
    }
}
```

### Delete Redaction

Remove a specific redaction from the image:

```csharp
private async Task DeleteRedactionById(string redactId)
{
    await ImageEditor.DeleteRedactAsync(redactId);
    Console.WriteLine("Redaction deleted");
}

private async Task DeleteAllRedactions()
{
    RedactSettings[] redacts = await ImageEditor.GetRedactsAsync();
    
    if (redacts != null)
    {
        foreach (var redact in redacts)
        {
            await ImageEditor.DeleteRedactAsync(redact.ID);
        }
    }
}
```

## Redaction Settings

### RedactSettings Class

The `RedactSettings` class contains properties for each redaction:

```csharp
public class RedactSettings
{
    public string ID { get; set; }              // Unique identifier
    public RedactType Type { get; set; }        // Blur or Pixelate
    public double StartX { get; set; }          // X-coordinate
    public double StartY { get; set; }          // Y-coordinate
    public double Width { get; set; }           // Redaction width
    public double Height { get; set; }          // Redaction height
    public int BlurIntensity { get; set; }      // For Blur type (0-100)
    public int PixelSize { get; set; }          // For Pixelate type
}
```

### Creating Custom Redaction Settings

```csharp
private async Task ApplyCustomRedaction()
{
    var settings = new RedactSettings
    {
        Type = RedactType.Blur,
        StartX = 100,
        StartY = 100,
        Width = 200,
        Height = 150,
        BlurIntensity = 40
    };
    
    // Note: Use DrawRedactAsync to create new redactions
    await ImageEditor.DrawRedactAsync(
        settings.Type,
        settings.StartX,
        settings.StartY,
        settings.Width,
        settings.Height,
        settings.BlurIntensity
    );
}
```

## Use Cases

### Use Case 1: Redact Personal Information

Hide sensitive personal data like names, addresses, phone numbers:

```csharp
private async Task RedactPersonalInfo()
{
    ImageDimension dim = await ImageEditor.GetImageDimensionAsync();
    
    // Redact name field
    await ImageEditor.DrawRedactAsync(
        RedactType.Blur,
        dim.X.Value + 150,
        dim.Y.Value + 80,
        200, 30, 35
    );
    
    // Redact phone number
    await ImageEditor.DrawRedactAsync(
        RedactType.Blur,
        dim.X.Value + 150,
        dim.Y.Value + 120,
        200, 30, 35
    );
    
    // Redact address
    await ImageEditor.DrawRedactAsync(
        RedactType.Blur,
        dim.X.Value + 150,
        dim.Y.Value + 160,
        300, 60, 35
    );
}
```

### Use Case 2: Anonymize Faces

Protect identity by obscuring faces in photos:

```csharp
private async Task AnonymizeFaces()
{
    // Face 1 - pixelate effect
    await ImageEditor.DrawRedactAsync(
        RedactType.Pixelate,
        200, 150,
        120, 140, 25
    );
    
    // Face 2 - blur effect
    await ImageEditor.DrawRedactAsync(
        RedactType.Blur,
        450, 180,
        110, 130, 45
    );
}
```

### Use Case 3: Hide Financial Information

Redact account numbers, balances, transaction details:

```csharp
private async Task RedactFinancialData()
{
    ImageDimension dim = await ImageEditor.GetImageDimensionAsync();
    
    // Account number - heavy blur
    await ImageEditor.DrawRedactAsync(
        RedactType.Blur,
        dim.X.Value + 200,
        dim.Y.Value + 100,
        250, 40, 50
    );
    
    // Balance - pixelate
    await ImageEditor.DrawRedactAsync(
        RedactType.Pixelate,
        dim.X.Value + 200,
        dim.Y.Value + 200,
        150, 35, 20
    );
}
```

### Use Case 4: GDPR/Privacy Compliance

Automatically redact sensitive areas before sharing:

```csharp
private async Task ApplyGDPRRedaction()
{
    // Define sensitive regions from configuration
    var sensitiveRegions = new[]
    {
        new { X = 100, Y = 50, Width = 200, Height = 30 },
        new { X = 100, Y = 90, Width = 250, Height = 40 },
        new { X = 100, Y = 140, Width = 180, Height = 30 }
    };
    
    // Apply consistent blur to all sensitive regions
    foreach (var region in sensitiveRegions)
    {
        await ImageEditor.DrawRedactAsync(
            RedactType.Blur,
            region.X,
            region.Y,
            region.Width,
            region.Height,
            40  // Consistent blur intensity
        );
    }
}
```

## Best Practices

### Choosing Redaction Type

- **Blur**: Best for text, documents, smooth transitions
- **Pixelate**: Best for faces, photographs, recognizable objects

### Redaction Intensity

- **Light (10-20)**: Use when context is needed but specific details should be hidden
- **Medium (20-40)**: Standard privacy protection for most use cases
- **Heavy (40-100)**: Maximum security for highly sensitive information

### Performance Considerations

```csharp
// Efficient: Apply multiple redactions, then export once
await ImageEditor.DrawRedactAsync(RedactType.Blur, 100, 100, 200, 50, 30);
await ImageEditor.DrawRedactAsync(RedactType.Blur, 100, 160, 200, 50, 30);
await ImageEditor.DrawRedactAsync(RedactType.Blur, 100, 220, 200, 50, 30);
await ImageEditor.ExportAsync("redacted-document.png");

// Update existing redactions instead of deleting and recreating
RedactSettings[] redacts = await ImageEditor.GetRedactsAsync();
redacts[0].BlurIntensity = 50;
await ImageEditor.UpdateRedactAsync(redacts[0]);
```

### Undo/Redo Support

All redaction operations support undo/redo:

```csharp
// Apply redaction
await ImageEditor.DrawRedactAsync(RedactType.Blur, 100, 100, 200, 100, 30);

// Undo if needed
await ImageEditor.UndoAsync();

// Redo
await ImageEditor.RedoAsync();
```

## Security Considerations

1. **Permanent Application**: Redactions become permanent only after export
2. **Original Image**: Original remains unchanged until saved
3. **Intensity Verification**: Always verify redaction effectiveness before sharing
4. **Multiple Passes**: For maximum security, apply multiple overlapping redactions
5. **Export Format**: Use lossless formats (PNG) to preserve redaction quality

## Limitations

- Redactions are visual overlays; original data remains in memory until export
- Very high blur/pixelate values may impact performance
- Redaction coordinates must be within image boundaries
- Maximum recommended blur intensity: 100
- Maximum recommended pixel size: 50

All redaction operations are tracked in the undo/redo history and can be exported with the final image.
