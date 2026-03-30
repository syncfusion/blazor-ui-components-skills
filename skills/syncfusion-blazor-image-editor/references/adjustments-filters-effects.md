# Adjustments, Filters & Effects in the Image Editor

## Overview

The Image Editor provides fine-tuning adjustments, filters, and frame effects to enhance and modify images. All effects are non-destructive and can be undone.

## Fine-Tuning Adjustments

### Available Adjustments

Fine-tuning provides slider-based controls for image adjustments:

- **Brightness** - Increase/decrease overall luminosity
- **Contrast** - Enhance difference between light and dark areas
- **Saturation** - Adjust color intensity
- **Hue** - Shift color tones
- **Blur** - Apply blur effect
- **Shadow** - Darken shadow areas
- **Exposure** - Adjust overall exposure level

### Access Fine-Tuning

```csharp
@using Syncfusion.Blazor.ImageEditor

<SfImageEditor @ref="ImageEditor" Height="500px" Toolbar="@CustomToolbar">
    <ImageEditorEvents Created="OnCreated"></ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;
    
    private List<ImageEditorToolbarItemModel> CustomToolbar = new()
    {
        new ImageEditorToolbarItemModel { Name = "Open" },
        new ImageEditorToolbarItemModel { Name = "Finetune" },  // Enable fine-tuning
        new ImageEditorToolbarItemModel { Name = "Save" }
    };

    private async void OnCreated()
    {
        await ImageEditor.OpenAsync("YOUR_IMAGE_URL");
    }
}
```

**User workflow:**
1. Click Fine-tune button in toolbar
2. Select an adjustment from dropdown
3. Use slider to adjust value
4. Preview updates in real-time
5. Click tick icon or canvas to apply

### Adjustment Ranges

| Adjustment | Range | Default | Effect |
|-----------|-------|---------|--------|
| Brightness | -100 to +100 | 0 | Darker to brighter |
| Contrast | -100 to +100 | 0 | Low to high contrast |
| Saturation | -100 to +100 | 0 | Grayscale to vivid |
| Hue | 0 to 360 | 0 | Color rotation |
| Blur | 0 to 100 | 0 | No blur to heavy blur |
| Shadow | -100 to +100 | 0 | Shadow adjustment |
| Exposure | -100 to +100 | 0 | Underexposed to overexposed |

## Filters

### Available Filters

Predefined filters for quick image enhancement:

- **Chrome** - Enhance metallic tones
- **Cool** - Add blue/cool color cast
- **Warm** - Add orange/warm color cast
- **Grayscale** - Convert to black and white
- **Sepia** - Add vintage brownish tone
- **Invert** - Invert all colors
- **High Contrast** - Increase overall contrast
- **Blur** - Apply blur effect

### Apply Filters

```csharp
@using Syncfusion.Blazor.ImageEditor

<SfImageEditor @ref="ImageEditor" Height="500px" Toolbar="@CustomToolbar">
    <ImageEditorEvents Created="OnCreated"></ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;
    
    private List<ImageEditorToolbarItemModel> CustomToolbar = new()
    {
        new ImageEditorToolbarItemModel { Name = "Open" },
        new ImageEditorToolbarItemModel { Name = "Filter" },  // Enable filters
        new ImageEditorToolbarItemModel { Name = "Save" }
    };

    private async void OnCreated()
    {
        await ImageEditor.OpenAsync("YOUR_IMAGE_URL");
    }
}
```

**User workflow:**
1. Click Filter button in toolbar
2. Select a filter from available options
3. Filter applies automatically
4. Preview updates immediately
5. Click tick to commit or canvas to cancel

### Filter Effects

| Filter | Use Case |
|--------|----------|
| **Chrome** | Metal/industrial look |
| **Cool** | Night/underwater atmosphere |
| **Warm** | Sunset/golden hour feel |
| **Grayscale** | Professional/classic look |
| **Sepia** | Vintage/retro appearance |
| **Invert** | Negative/artistic effect |
| **High Contrast** | Enhanced clarity/drama |
| **Blur** | Background/soft focus |

## Frame Effects

### Apply Frames

Add decorative borders and frame effects to images:

```csharp
@using Syncfusion.Blazor.ImageEditor

<SfImageEditor @ref="ImageEditor" Height="500px" Toolbar="@CustomToolbar">
    <ImageEditorEvents Created="OnCreated"></ImageEditorEvents>
</SfImageEditor>

@code {
    SfImageEditor ImageEditor;
    
    private List<ImageEditorToolbarItemModel> CustomToolbar = new()
    {
        new ImageEditorToolbarItemModel { Name = "Open" },
        new ImageEditorToolbarItemModel { Name = "Frame" },  // Enable frames
        new ImageEditorToolbarItemModel { Name = "Save" }
    };

    private async void OnCreated()
    {
        await ImageEditor.OpenAsync("YOUR_IMAGE_URL");
    }
}
```

**User workflow:**
1. Click Frame button in toolbar
2. Select a frame style
3. Frame applies to image
4. Preview shows final result
5. Click tick to apply or canvas to cancel

### Frame Options

Common frame styles include:
- Solid color borders (various widths and colors)
- Decorative frames (ornate, minimalist, vintage)
- Mat styles (beveled, shadow effects)
- Professional photo frames

## Combining Effects

### Sequential Application

Apply multiple effects in sequence:

```csharp
// Example workflow:
// 1. Adjust brightness
// 2. Apply warm filter
// 3. Add frame
// 4. Export

// User can undo any effect individually
```

Each effect is tracked separately in undo/redo history.

### Effect Preview

All effects show real-time preview:
- Adjustments update as slider moves
- Filters apply instantly
- Frames display immediately
- No permanent changes until committed

## Best Practices

### Workflow Tips

1. **Adjust before filtering** - Fine-tune exposure before color adjustments
2. **Use filters sparingly** - Too many filters create unnatural results
3. **Test combinations** - Different filter+adjustment combinations work better
4. **Preview before export** - Always check final result before saving
5. **Use undo liberally** - Experiment and undo unwanted effects

### Quality Preservation

- **Adjustments** preserve image data better than filters
- **High contrast** adjustments can cause clipping
- **Blur** effects reduce sharpness permanently
- **Grayscale** conversion is reversible until export

### Optimization

- Apply adjustments in order: brightness → contrast → saturation → hue
- Use filters for style, adjustments for correction
- Save high-quality versions before destructive effects
- Test exported results before permanent storage

## Effect Combinations Examples

### Example 1: Warm Sunset Photo

```
1. Increase brightness +20
2. Increase saturation +30
3. Increase warm tones (hue adjustment)
4. Apply warm filter
5. Optional: Add vintage frame
```

### Example 2: Professional Portrait

```
1. Slight brightness increase +10
2. Increase contrast +15
3. Apply cool filter subtly
4. Add soft focus (light blur)
5. No frame or minimal border
```

### Example 3: Dramatic Black & White

```
1. Apply grayscale filter
2. Increase contrast +40
3. Optional: Increase shadows
4. No color adjustments (already grayscale)
5. Add vintage or film frame
```

### Example 4: Landscape Enhancement

```
1. Adjust exposure +15
2. Increase contrast +25
3. Increase saturation +15
4. Apply chrome filter for richness
5. Optional: Add nature-themed frame
```

All effects are cumulative and reversible until export. Use undo (Ctrl+Z) to revert any changes.
