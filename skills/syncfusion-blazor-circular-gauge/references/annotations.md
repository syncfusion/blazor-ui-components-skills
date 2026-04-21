# Circular Gauge Annotations Reference

## Table of Contents
- [Overview](#overview)
- [Creating Annotations](#creating-annotations)
- [Content Types](#content-types)
- [Positioning Annotations](#positioning-annotations)
- [Multiple Annotations](#multiple-annotations)
- [Styling and Customization](#styling-and-customization)
- [Dynamic Annotations](#dynamic-annotations)
- [Use Cases](#use-cases)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)

## Overview

Annotations in Circular Gauge allow you to overlay custom content, such as text, HTML, shapes, or images, at specific positions on the gauge. They provide a powerful way to add contextual information, labels, instructions, or decorative elements that enhance the gauge's readability and visual appeal.

Annotations are highly flexible and can be positioned using polar coordinates (angle and radius) or Cartesian coordinates (X and Y offsets), making them suitable for a wide variety of scenarios.

## Creating Annotations

### Basic Annotation

Annotations are defined using the `CircularGaugeAnnotations` collection within an axis. Each annotation can contain custom content through the `ContentTemplate`.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge Height="250px" Width="250px">
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="50"></CircularGaugePointer>
            </CircularGaugePointers>
            <CircularGaugeAnnotations>
                <CircularGaugeAnnotation Angle="195" ZIndex="1">
                    <ContentTemplate>
                        <div class="custom-annotation">50</div>
                    </ContentTemplate>
                </CircularGaugeAnnotation>
            </CircularGaugeAnnotations>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

<style type="text/css">
    .custom-annotation {
        color: white;
        background-color: blue;
        height: 30px;
        width: 30px;
        border-radius: 15px;
        padding: 4px 0 0 6px;
        font-weight: bold;
    }
</style>
```

### Annotation Properties

**Essential Properties:**
- `ContentTemplate` - Defines the HTML content to display
- `Content` - Simple text content (alternative to ContentTemplate)
- `Angle` - Angular position in degrees (0-360)
- `Radius` - Radial distance from center (percentage or pixels)
- `ZIndex` - Stacking order for overlapping annotations
- `AutoAngle` - Automatically rotates annotation based on angle
- `TextStyle` - Font and text styling options

## Content Types

### Text Annotations

Simple text content using the `Content` property.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeAnnotations>
                <CircularGaugeAnnotation Content="Speed" 
                                         Angle="180" 
                                         Radius="50%" 
                                         ZIndex="1">
                </CircularGaugeAnnotation>
            </CircularGaugeAnnotations>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### HTML Annotations

Rich HTML content with styling through `ContentTemplate`.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge Height="250px" Width="250px">
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeAnnotations>
                <CircularGaugeAnnotation Angle="180" Radius="30%" ZIndex="1">
                    <ContentTemplate>
                        <div style="font-size: 16px; color: #666; font-weight: bold;">
                            <span style="color: #007bff;">Current Speed</span><br/>
                            <span style="font-size: 24px; color: #28a745;">65</span>
                            <span style="font-size: 14px;">km/h</span>
                        </div>
                    </ContentTemplate>
                </CircularGaugeAnnotation>
            </CircularGaugeAnnotations>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Image Annotations

Include images as annotations for icons or logos.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeAnnotations>
                <CircularGaugeAnnotation Angle="180" Radius="0%" ZIndex="1">
                    <ContentTemplate>
                        <div>
                            <img src="logo.png" width="50" height="50" alt="Logo"/>
                        </div>
                    </ContentTemplate>
                </CircularGaugeAnnotation>
            </CircularGaugeAnnotations>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### Styled Containers

Create custom styled containers for complex layouts.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeAnnotations>
                <CircularGaugeAnnotation Angle="180" Radius="25%" ZIndex="1">
                    <ContentTemplate>
                        <div class="gauge-card">
                            <div class="gauge-title">Temperature</div>
                            <div class="gauge-value">72°F</div>
                            <div class="gauge-status">Normal</div>
                        </div>
                    </ContentTemplate>
                </CircularGaugeAnnotation>
            </CircularGaugeAnnotations>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

<style>
    .gauge-card {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        padding: 15px;
        border-radius: 10px;
        color: white;
        text-align: center;
        box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    }
    
    .gauge-title {
        font-size: 12px;
        opacity: 0.9;
        margin-bottom: 5px;
    }
    
    .gauge-value {
        font-size: 24px;
        font-weight: bold;
        margin-bottom: 5px;
    }
    
    .gauge-status {
        font-size: 10px;
        opacity: 0.8;
    }
</style>
```

## Positioning Annotations

### Angular Positioning

Use the `Angle` property to position annotations around the gauge circumference.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge Height="250px" Width="250px">
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="50"></CircularGaugePointer>
            </CircularGaugePointers>
            <CircularGaugeAnnotations>
                <CircularGaugeAnnotation Angle="90"
                                         Radius="110%"
                                         ZIndex="1">
                    <ContentTemplate>
                        <div class="custom-annotation">50</div>
                    </ContentTemplate>
                </CircularGaugeAnnotation>
            </CircularGaugeAnnotations>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

<style type="text/css">
    .custom-annotation {
        color: white;
        background-color: blue;
        height: 30px;
        width: 30px;
        border-radius: 15px;
        padding: 4px 0 0 6px;
        font-weight: bold;
    }
</style>
```

**Angle Reference:**
- 0° - Right (3 o'clock position)
- 90° - Bottom (6 o'clock position)
- 180° - Left (9 o'clock position)
- 270° - Top (12 o'clock position)

### Radial Positioning

Use the `Radius` property to control distance from the gauge center.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeAnnotations>
                <!-- Center annotation -->
                <CircularGaugeAnnotation Angle="180" Radius="0%" ZIndex="1">
                    <ContentTemplate>
                        <div>Center</div>
                    </ContentTemplate>
                </CircularGaugeAnnotation>
                
                <!-- Middle annotation -->
                <CircularGaugeAnnotation Angle="180" Radius="50%" ZIndex="1">
                    <ContentTemplate>
                        <div>Middle</div>
                    </ContentTemplate>
                </CircularGaugeAnnotation>
                
                <!-- Outer annotation -->
                <CircularGaugeAnnotation Angle="180" Radius="120%" ZIndex="1">
                    <ContentTemplate>
                        <div>Outside</div>
                    </ContentTemplate>
                </CircularGaugeAnnotation>
            </CircularGaugeAnnotations>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

**Radius Tips:**
- 0% - Gauge center
- 50% - Halfway to axis
- 100% - On the axis
- >100% - Outside the gauge

### Z-Index for Layering

Control stacking order with `ZIndex` for overlapping annotations.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeAnnotations>
                <!-- Background annotation -->
                <CircularGaugeAnnotation Angle="180" Radius="30%" ZIndex="0">
                    <ContentTemplate>
                        <div style="background: lightgray; padding: 20px;">
                            Background
                        </div>
                    </ContentTemplate>
                </CircularGaugeAnnotation>
                
                <!-- Foreground annotation -->
                <CircularGaugeAnnotation Angle="180" Radius="30%" ZIndex="2">
                    <ContentTemplate>
                        <div style="background: blue; color: white; padding: 10px;">
                            Foreground
                        </div>
                    </ContentTemplate>
                </CircularGaugeAnnotation>
            </CircularGaugeAnnotations>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Multiple Annotations

Create complex visualizations with multiple annotations at different positions.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge Height="250px" Width="250px">
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeRanges>
                <CircularGaugeRange Start="35"
                                    End="70"
                                    Color="blue"
                                    Opacity="0.2">
                </CircularGaugeRange>
            </CircularGaugeRanges>
            <CircularGaugePointers>
                <CircularGaugePointer Value="50"></CircularGaugePointer>
            </CircularGaugePointers>
            <CircularGaugeAnnotations>
                <!-- Title annotation -->
                <CircularGaugeAnnotation Angle="325" Radius="150%" ZIndex="1">
                    <ContentTemplate>
                        <div class="custom-annotation">Speed to get higher mileage</div>
                    </ContentTemplate>
                </CircularGaugeAnnotation>
                
                <!-- Value annotation -->
                <CircularGaugeAnnotation Angle="195" ZIndex="1">
                    <ContentTemplate>
                        <div class="speed">50</div>
                    </ContentTemplate>
                </CircularGaugeAnnotation>
            </CircularGaugeAnnotations>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

<style type="text/css">
    .speed {
        color: white;
        background-color: blue;
        height: 30px;
        width: 30px;
        border-radius: 15px;
        padding: 4px 0 0 6px;
        font-weight: bold;
    }

    .custom-annotation {
        background-color: lightgray;
        width: 100%;
        padding: 1px;
    }
</style>
```

### Circular Label Annotations

Position labels around the gauge perimeter for a clock or compass effect.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeAnnotations>
                <CircularGaugeAnnotation Content="N" Angle="270" Radius="120%" ZIndex="1">
                </CircularGaugeAnnotation>
                <CircularGaugeAnnotation Content="E" Angle="0" Radius="120%" ZIndex="1">
                </CircularGaugeAnnotation>
                <CircularGaugeAnnotation Content="S" Angle="90" Radius="120%" ZIndex="1">
                </CircularGaugeAnnotation>
                <CircularGaugeAnnotation Content="W" Angle="180" Radius="120%" ZIndex="1">
                </CircularGaugeAnnotation>
            </CircularGaugeAnnotations>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

## Styling and Customization

### Text Style Properties

When using the `Content` property, apply text styling through `CircularGaugeAnnotationTextStyle`.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeAnnotations>
                <CircularGaugeAnnotation Content="Speed" 
                                         Angle="180" 
                                         Radius="50%" 
                                         ZIndex="1">
                    <CircularGaugeAnnotationTextStyle Size="20px"
                                                      Color="#007bff"
                                                      FontWeight="bold"
                                                      FontFamily="Arial">
                    </CircularGaugeAnnotationTextStyle>
                </CircularGaugeAnnotation>
            </CircularGaugeAnnotations>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>
```

### CSS Styling for ContentTemplate

Apply comprehensive styling through CSS classes.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugeAnnotations>
                <CircularGaugeAnnotation Angle="180" Radius="30%" ZIndex="1">
                    <ContentTemplate>
                        <div class="annotation-box">
                            <i class="icon-speedometer"></i>
                            <span class="value">65</span>
                            <span class="unit">mph</span>
                        </div>
                    </ContentTemplate>
                </CircularGaugeAnnotation>
            </CircularGaugeAnnotations>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

<style>
    .annotation-box {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 5px;
        padding: 10px;
        background: rgba(255, 255, 255, 0.9);
        border-radius: 8px;
        box-shadow: 0 2px 8px rgba(0, 0, 0, 0.15);
    }
    
    .value {
        font-size: 28px;
        font-weight: bold;
        color: #007bff;
    }
    
    .unit {
        font-size: 12px;
        color: #666;
    }
</style>
```

## Dynamic Annotations

Update annotation content based on gauge state or external data.

```cshtml
@using Syncfusion.Blazor.CircularGauge

<SfCircularGauge EnablePointerDrag="true">
    <CircularGaugeEvents OnDrag="@UpdateAnnotation">
    </CircularGaugeEvents>
    <CircularGaugeAxes>
        <CircularGaugeAxis>
            <CircularGaugePointers>
                <CircularGaugePointer Value="@currentValue">
                </CircularGaugePointer>
            </CircularGaugePointers>
            <CircularGaugeAnnotations>
                <CircularGaugeAnnotation Angle="180" Radius="30%" ZIndex="1">
                    <ContentTemplate>
                        <div class="dynamic-annotation">
                            <div class="label">Current Value</div>
                            <div class="value">@currentValue.ToString("F1")</div>
                            <div class="status">@GetStatus(currentValue)</div>
                        </div>
                    </ContentTemplate>
                </CircularGaugeAnnotation>
            </CircularGaugeAnnotations>
        </CircularGaugeAxis>
    </CircularGaugeAxes>
</SfCircularGauge>

@code {
    private double currentValue = 50;
    
    void UpdateAnnotation(Syncfusion.Blazor.CircularGauge.PointerDragEventArgs args)
    {
        currentValue = args.CurrentValue;
    }
    
    string GetStatus(double value)
    {
        if (value < 30) return "Low";
        if (value < 70) return "Normal";
        return "High";
    }
}

<style>
    .dynamic-annotation {
        text-align: center;
        padding: 10px;
        background: #f8f9fa;
        border-radius: 8px;
    }
    
    .label {
        font-size: 10px;
        color: #666;
        margin-bottom: 5px;
    }
    
    .value {
        font-size: 24px;
        font-weight: bold;
        color: #007bff;
        margin-bottom: 5px;
    }
    
    .status {
        font-size: 12px;
        color: #28a745;
    }
</style>
```

## Use Cases

### Value Display
Show the current pointer value with units and formatting at the gauge center.

### Status Indicators
Display textual status (Normal, Warning, Critical) based on gauge values.

### Legends and Labels
Add descriptive labels for ranges, zones, or specific values around the gauge.

### Branding
Include company logos, product names, or branding elements.

### Instructions
Provide usage instructions or guidance text for interactive gauges.

### Multi-Metric Display
Show related metrics or calculated values derived from the main gauge reading.

### Icon Indicators
Use icon fonts or images to represent states or categories visually.

### Thresholds
Mark specific threshold values with annotations for quick reference.

## Best Practices

1. **Content Strategy**
   - Keep annotation text concise and readable
   - Use appropriate font sizes for the gauge dimensions
   - Ensure text contrasts well with backgrounds

2. **Positioning**
   - Place important annotations where they won't obscure pointers
   - Use consistent positioning patterns for similar annotations
   - Consider gauge rotation and start/end angles

3. **Z-Index Management**
   - Use lower Z-index values for background elements
   - Use higher Z-index values for interactive or important content
   - Test layering with different gauge configurations

4. **Responsive Design**
   - Use percentage-based sizing where possible
   - Test annotations at different gauge sizes
   - Consider using media queries for complex layouts

5. **Performance**
   - Minimize complex HTML in ContentTemplate
   - Avoid heavy images or animations
   - Limit the number of annotations for optimal rendering

6. **Accessibility**
   - Provide meaningful text content
   - Use semantic HTML elements
   - Ensure sufficient color contrast
   - Consider screen reader compatibility

## Troubleshooting

### Annotation Not Visible

**Problem:** Annotation is defined but not appearing.

**Solutions:**
- Verify the annotation is within the gauge boundaries
- Check `Radius` value places it in a visible location
- Ensure `ZIndex` is appropriate (not hidden behind other elements)
- Verify `ContentTemplate` or `Content` has visible content
- Check for CSS that might hide the annotation

### Positioning Issues

**Problem:** Annotation appears in the wrong location.

**Solutions:**
- Verify `Angle` is in degrees (0-360)
- Check `Radius` is in correct format (percentage with % or pixels with px)
- Consider gauge's `CenterX` and `CenterY` if customized
- Account for axis `StartAngle` and `EndAngle` settings
- Review gauge dimensions and container sizing

### Text Truncation

**Problem:** Annotation text is cut off or truncated.

**Solutions:**
- Increase container size in ContentTemplate
- Adjust `Radius` to provide more space
- Use smaller font sizes or abbreviations
- Apply CSS `white-space` and `overflow` properties appropriately
- Consider word-wrap or line-break CSS properties

### Overlapping Content

**Problem:** Annotations overlap each other or gauge elements.

**Solutions:**
- Adjust `Angle` values to increase separation
- Modify `Radius` to create spacing
- Use different `ZIndex` values for proper layering
- Reduce annotation sizes or font sizes
- Consider repositioning or combining annotations

### Dynamic Content Not Updating

**Problem:** Annotation content doesn't refresh when data changes.

**Solutions:**
- Ensure StateHasChanged() is called after data updates
- Verify binding syntax is correct in ContentTemplate
- Check that the component properly re-renders
- Use @key directive if rendering multiple similar annotations
- Verify event handlers are properly triggering updates

### Styling Not Applied

**Problem:** CSS styles not affecting annotation appearance.

**Solutions:**
- Check CSS specificity and selector correctness
- Verify styles are defined within `<style>` tags in the component
- Use browser developer tools to inspect applied styles
- Ensure ContentTemplate div has appropriate class names
- Check for conflicting global styles
- Consider using inline styles for guaranteed application

### Poor Performance with Many Annotations

**Problem:** Gauge renders slowly with multiple annotations.

**Solutions:**
- Reduce the number of annotations
- Simplify HTML content in ContentTemplate
- Optimize or compress images
- Remove unnecessary styling or animations
- Consider server-side rendering for initial load
- Use simpler text annotations instead of complex HTML when possible
