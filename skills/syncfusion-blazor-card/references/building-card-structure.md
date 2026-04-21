# Building Card Structure

## Table of Contents

- [Overview](#overview)
- [Header Elements](#header-elements)
- [Content Sections](#content-sections)
- [Card Images](#card-images)
- [Dividers and Separators](#dividers-and-separators)
- [Multiple Content Sections](#multiple-content-sections)
- [Complex Layouts](#complex-layouts)
- [Troubleshooting](#troubleshooting)

## Overview

The Card structure is built using Blazor components that nest together to create flexible, organized layouts. The main components are `CardHeader`, `CardContent`, `CardImage`, and `CardFooter`, which can be combined in various ways to create different card designs.

## Header Elements

### Basic Header with Title and Subtitle

Use `CardHeader` component to display title and subtitle:

```razor
@using Syncfusion.Blazor.Cards

<SfCard>
    <CardHeader Title="Product Name" SubTitle="Category - Price"></CardHeader>
</SfCard>
```

### Header with Image

Add an image to the header using `ImageUrl` property:

```razor
<SfCard>
    <CardHeader 
        Title="John Doe" 
        SubTitle="Senior Developer" 
        ImageUrl="images/john-profile.png" />
</SfCard>
```

The image appears alongside the title and subtitle, creating a profile-like header.

### Header Image Styling

By default, header images are small (48px). Customize with CSS:

```css
.e-card .e-card-header .e-card-header-image {
    height: 80px;
    width: 80px;
    border-radius: 50%;
}
```

## Content Sections

### Single Content Section

Add primary content using `CardContent`:

```razor
<SfCard>
    <CardHeader Title="Article Title"></CardHeader>
    <CardContent Content="This is the main content of the article. It can be plain text or HTML."></CardContent>
</SfCard>
```

### Content with HTML Elements

Embed HTML directly in CardContent:

```razor
<SfCard>
    <CardHeader Title="Blog Post"></CardHeader>
    <CardContent>
        <p>This paragraph contains <strong>formatted text</strong> with various <em>styling options</em>.</p>
        <ul>
            <li>Feature 1</li>
            <li>Feature 2</li>
        </ul>
    </CardContent>
</SfCard>
```

### Content Styling

Customize content appearance with CSS:

```css
.e-card .e-card-content {
    font-size: 16px;
    color: #333;
    line-height: 1.6;
}
```

## Card Images

### Basic Card Image

Use `CardImage` component to display images:

```razor
<SfCard>
    <CardImage Image="images/nature.jpg" />
    <CardHeader Title="Nature" SubTitle="Beautiful Landscape"></CardHeader>
</SfCard>
```

By default, card images occupy the full width of the card.

### Image with Title

Add a title overlay on the image:

```razor
<SfCard>
    <CardImage Image="images/mountain.jpg" />
    <CardHeader Title="Mountain Peak"></CardHeader>
</SfCard>
```

The header title appears as an overlay on the image.

### Image Sizing and Styling

Control image dimensions with CSS:

```css
.e-card .e-card-image {
    background-image: url('images/cards/camera.jpg');
    background-size: cover;
    background-position: center;
    height: 200px;
    width: 100%;
}
```

### Inline Images in Content

Include images within CardContent:

```razor
<SfCard>
    <CardHeader Title="Product Photo"></CardHeader>
    <CardContent>
        <img src="images/product.png" alt="Product" style="width: 100%; height: auto;" />
        <p>High-resolution product image with details.</p>
    </CardContent>
</SfCard>
```

## Dividers and Separators

### Single Content with Divider

Use `EnableSeparator` property to add a visual divider:

```razor
<SfCard>
    <CardHeader Title="Information"></CardHeader>
    <CardContent EnableSeparator="true">
        First section of content here.
    </CardContent>
</SfCard>
```

### Multiple Sections with Dividers

Create multi-section cards using multiple `CardContent` components:

```razor
<SfCard>
    <CardHeader Title="Multi-Section Card"></CardHeader>
    <CardContent EnableSeparator="true">
        <strong>Section 1</strong><br/>
        Content for the first section with details.
    </CardContent>
    <CardContent EnableSeparator="true">
        <strong>Section 2</strong><br/>
        Content for the second section with more details.
    </CardContent>
    <CardContent EnableSeparator="true">
        <strong>Section 3</strong><br/>
        Final section of the card with concluding information.
    </CardContent>
</SfCard>
```

### Customizing Divider Styling

Modify divider appearance with CSS:

```css
.e-card .e-card-separator {
    padding-bottom: 30px;
    border-bottom: 2px solid #ddd;
}
```

## Multiple Content Sections

### Information Card Example

Display structured information with multiple sections:

```razor
<SfCard>
    <CardHeader Title="Employee Profile"></CardHeader>
    <CardContent EnableSeparator="true">
        <strong>Name:</strong> Jane Smith
    </CardContent>
    <CardContent EnableSeparator="true">
        <strong>Department:</strong> Engineering
    </CardContent>
    <CardContent EnableSeparator="true">
        <strong>Experience:</strong> 5 years
    </CardContent>
    <CardContent EnableSeparator="true">
        <strong>Email:</strong> jane.smith@company.com
    </CardContent>
</SfCard>
```

### City Information Example

Display location-based information:

```razor
<SfCard>
    <CardHeader Title="City Guide"></CardHeader>
    <CardContent EnableSeparator="true">
        <h4>Sydney</h4>
        Sydney is a city on the east coast of Australia. It is the capital city of New South Wales.
    </CardContent>
    <CardContent EnableSeparator="true">
        <h4>New York</h4>
        New York City has been described as the cultural, financial, and media capital of the world.
    </CardContent>
    <CardContent EnableSeparator="true">
        <h4>London</h4>
        London is the capital and largest city of the United Kingdom.
    </CardContent>
</SfCard>
```

## Complex Layouts

### Card with Image, Header, Multiple Content, and Footer

Combine all card elements:

```razor
@using Syncfusion.Blazor.Cards
@using Syncfusion.Blazor.Buttons

<SfCard>
    <CardImage Image="images/product.jpg"/>
    <CardHeader Title="Premium Product" SubTitle="by TechCorp"></CardHeader>
    <CardContent EnableSeparator="true">
        <strong>Features:</strong>
        <ul>
            <li>High performance</li>
            <li>Durable design</li>
            <li>Eco-friendly</li>
        </ul>
    </CardContent>
    <CardContent EnableSeparator="true">
        <strong>Specifications:</strong><br/>
        Weight: 500g | Size: 25x15x10cm
    </CardContent>
    <CardFooter>
        <CardFooterContent>
            <SfButton CssClass="e-btn e-primary">Buy Now</SfButton>
            <SfButton CssClass="e-btn e-outline">Add to Cart</SfButton>
        </CardFooterContent>
    </CardFooter>
</SfCard>
```

### Nested Content with Lists

Create structured cards with complex content:

```razor
<SfCard>
    <CardHeader Title="Project Details"></CardHeader>
    <CardContent EnableSeparator="true">
        <h5>Team Members</h5>
        <ol>
            <li>Project Manager - Alice Johnson</li>
            <li>Lead Developer - Bob Smith</li>
            <li>Designer - Carol Davis</li>
        </ol>
    </CardContent>
    <CardContent EnableSeparator="true">
        <h5>Timeline</h5>
        <strong>Phase 1:</strong> Planning (Week 1-2)<br/>
        <strong>Phase 2:</strong> Development (Week 3-6)<br/>
        <strong>Phase 3:</strong> Testing (Week 7-8)<br/>
    </CardContent>
</SfCard>
```

## Best Practices

### Do's
- ✓ Use clear, descriptive titles
- ✓ Keep content concise and well-organized
- ✓ Use dividers to separate logical sections
- ✓ Include images for visual interest
- ✓ Use consistent spacing and alignment

### Don'ts
- ✗ Overcrowd cards with too much content
- ✗ Use images without optimization (watch file sizes)
- ✗ Mix too many text sizes and colors
- ✗ Create cards without clear hierarchy
- ✗ Nest cards infinitely deep

## Troubleshooting

### Content Not Displaying
- Verify CardContent is nested within SfCard
- Check that Content property value is not empty or null
- Ensure correct Blazor component syntax

### Images Not Loading
- Verify image path is correct and relative to wwwroot
- Check browser console for 404 errors
- Ensure image file exists at specified location

### Dividers Not Showing
- Set EnableSeparator="true" on CardContent
- Verify CSS for .e-card-separator is not hidden
- Check that multiple CardContent components are used

### Layout Issues
- Validate component hierarchy is correct
- Check CSS for conflicting styles
- Clear browser cache and rebuild
