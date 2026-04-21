# Card Layouts and Orientation

## Table of Contents

- [Overview](#overview)
- [Vertical Layout (Default)](#vertical-layout-default)
- [Horizontal Layout](#horizontal-layout)
- [CardStacked Component](#cardstacked-component)
- [Mixed Orientations](#mixed-orientations)
- [Responsive Layouts](#responsive-layouts)
- [Layout Patterns](#layout-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

Card layout controls how card elements are arranged and displayed. By default, cards use a vertical layout where elements stack from top to bottom. The `Orientation` property allows you to change this behavior and create more dynamic layouts using horizontal alignment and the `CardStacked` component.

## Vertical Layout (Default)

### Basic Vertical Card

By default, all card elements are aligned vertically one after another:

```razor
@using Syncfusion.Blazor.Cards

<SfCard>
    <CardImage Image="images/card.jpg" />
    <CardHeader Title="Product Title"></CardHeader>
    <CardContent Content="Product description goes here."></CardContent>
</SfCard>
```

**Visual Stack:**
```
┌─────────────┐
│   Image     │
├─────────────┤
│   Header    │
├─────────────┤
│  Content    │
└─────────────┘
```

### Vertical Card with Multiple Sections

Use multiple CardContent components for vertical stacking:

```razor
<SfCard>
    <CardHeader Title="Multi-Section"></CardHeader>
    <CardContent EnableSeparator="true">
        Section 1 content
    </CardContent>
    <CardContent EnableSeparator="true">
        Section 2 content
    </CardContent>
</SfCard>
```

## Horizontal Layout

### Basic Horizontal Card

Set `Orientation="CardOrientation.Horizontal"` to align elements horizontally:

```razor
<SfCard Orientation="CardOrientation.Horizontal">
    <CardStacked>
        <CardHeader Title="Product"></CardHeader>
        <CardContent Content="Description"></CardContent>
    </CardStacked>
    <img src="images/product.png" />
</SfCard>
```

**Visual Layout:**
```
┌──────────────────────────────┐
│ Header    │                  │
│ Content   │    Image         │
│ Content   │                  │
└──────────────────────────────┘
```

### Horizontal Card Example

Create a featured product card with image on the right:

```razor
@using Syncfusion.Blazor.Cards

<SfCard Orientation="CardOrientation.Horizontal" ID="HorizontalCard">
    <CardStacked>
        <CardHeader Title="Philips Trimmer" SubTitle="Premium Grooming Tool"></CardHeader>
        <CardContent Content="Philips trimmers are designed to last longer than ordinary trimmers with DuraPower Technology for optimized performance."></CardContent>
    </CardStacked>
    <img src="images/trimmer.png" alt="Trimmer" style="height: 200px; width: 200px; object-fit: cover;" />
</SfCard>

<style>
    #HorizontalCard {
        width: 500px;
    }
</style>
```

### Horizontal Card with Buttons

Add footer buttons to horizontal layout:

```razor
@using Syncfusion.Blazor.Cards
@using Syncfusion.Blazor.Buttons

<SfCard Orientation="CardOrientation.Horizontal">
    <CardStacked>
        <CardHeader Title="Item" SubTitle="Price: $29.99"></CardHeader>
        <CardContent Content="High-quality item with great features."></CardContent>
        <CardFooter>
            <CardFooterContent>
                <SfButton CssClass="e-btn e-primary">Buy</SfButton>
            </CardFooterContent>
        </CardFooter>
    </CardStacked>
    <img src="images/item.png" />
</SfCard>
```

## CardStacked Component

### Purpose of CardStacked

`CardStacked` creates a vertically aligned section within a horizontal card. It's essential for horizontal layouts as it groups elements that should remain stacked.

### Basic CardStacked Usage

```razor
<SfCard Orientation="CardOrientation.Horizontal">
    <CardStacked>
        <!-- These elements stack vertically -->
        <CardHeader Title="Title"></CardHeader>
        <CardContent Content="Content"></CardContent>
    </CardStacked>
    <!-- This element appears beside the stacked section -->
    <img src="image.jpg" />
</SfCard>
```

### Multiple CardStacked Sections

Create more complex layouts with multiple stacked sections:

```razor
<SfCard Orientation="CardOrientation.Horizontal">
    <CardStacked>
        <CardHeader Title="Left Section"></CardHeader>
        <CardContent Content="Content for left side."></CardContent>
    </CardStacked>
    <img src="center-image.jpg" />
    <CardStacked>
        <CardContent Content="Right side content."></CardContent>
    </CardStacked>
</SfCard>
```

### CardStacked with Multiple Content

```razor
<SfCard Orientation="CardOrientation.Horizontal">
    <CardStacked>
        <CardHeader Title="Details"></CardHeader>
        <CardContent EnableSeparator="true">
            <strong>Feature 1:</strong> Description
        </CardContent>
        <CardContent EnableSeparator="true">
            <strong>Feature 2:</strong> Description
        </CardContent>
    </CardStacked>
    <img src="images/product.jpg" style="height: 300px;" />
</SfCard>
```

## Mixed Orientations

### Combining Vertical and Horizontal

Create complex layouts by nesting cards with different orientations:

```razor
<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px;">
    <!-- Vertical Card -->
    <SfCard>
        <CardImage Image="images/vertical.jpg" />
        <CardHeader Title="Vertical Layout"></CardHeader>
        <CardContent Content="Standard vertical arrangement."></CardContent>
    </SfCard>
    
    <!-- Horizontal Card -->
    <SfCard Orientation="CardOrientation.Horizontal">
        <CardStacked>
            <CardHeader Title="Horizontal Layout"></CardHeader>
            <CardContent Content="Image beside content."></CardContent>
        </CardStacked>
        <img src="images/horizontal.jpg" />
    </SfCard>
</div>
```

## Responsive Layouts

### CSS Grid Layout

Create responsive card grids:

```razor
<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(300px, 1fr)); gap: 20px;">
    @foreach(var item in Items)
    {
        <SfCard>
            <CardImage Image="@item.ImageUrl" />
            <CardHeader Title="@item.Title"></CardHeader>
            <CardContent Content="@item.Description"></CardContent>
        </SfCard>
    }
</div>
```

### Flexbox Layout

Use flexbox for flexible card arrangements:

```razor
<div style="display: flex; flex-wrap: wrap; gap: 20px;">
    @foreach(var item in Items)
    {
        <div style="flex: 1 1 300px;">
            <SfCard>
                <CardImage Image="@item.ImageUrl" />
                <CardHeader Title="@item.Title"></CardHeader>
                <CardContent Content="@item.Description"></CardContent>
            </SfCard>
        </div>
    }
</div>
```

### Media Queries for Responsive Design

Adapt card layouts based on screen size:

```css
/* Desktop */
@media (min-width: 1024px) {
    .card-container {
        display: grid;
        grid-template-columns: repeat(3, 1fr);
        gap: 20px;
    }
}

/* Tablet */
@media (min-width: 768px) and (max-width: 1023px) {
    .card-container {
        display: grid;
        grid-template-columns: repeat(2, 1fr);
        gap: 20px;
    }
}

/* Mobile */
@media (max-width: 767px) {
    .card-container {
        display: grid;
        grid-template-columns: 1fr;
        gap: 20px;
    }
}
```

## Layout Patterns

### Pattern 1: Featured Product Card

Horizontal layout with image and details:

```razor
<SfCard Orientation="CardOrientation.Horizontal" ID="FeaturedProduct">
    <CardStacked>
        <CardHeader Title="Featured Product" SubTitle="Limited Time Offer"></CardHeader>
        <CardContent Content="Get the latest model with advanced features at an unbeatable price."></CardContent>
        <CardFooter>
            <CardFooterContent>
                <SfButton CssClass="e-btn e-primary">Shop Now</SfButton>
            </CardFooterContent>
        </CardFooter>
    </CardStacked>
    <img src="images/featured.jpg" style="width: 300px; height: 300px; object-fit: cover;" />
</SfCard>

<style>
    #FeaturedProduct {
        width: 600px;
    }
</style>
```

### Pattern 2: Card Grid

Display multiple vertical cards in a responsive grid:

```razor
<div style="display: grid; grid-template-columns: repeat(auto-fill, minmax(280px, 1fr)); gap: 20px; padding: 20px;">
    @foreach(var item in ProductList)
    {
        <SfCard>
            <CardImage Image="@item.Image" />
            <CardHeader Title="@item.Name" SubTitle="@item.Price"></CardHeader>
            <CardContent Content="@item.Description"></CardContent>
        </SfCard>
    }
</div>
```

### Pattern 3: Dashboard Cards

Mix of horizontal and vertical cards for dashboard:

```razor
<!-- Wide Card -->
<SfCard Orientation="CardOrientation.Horizontal" style="margin-bottom: 20px;">
    <CardStacked>
        <CardHeader Title="Sales Overview"></CardHeader>
        <CardContent Content="Total: $50,000 | Growth: +15%"></CardContent>
    </CardStacked>
    <img src="chart.jpg" style="width: 200px;" />
</SfCard>

<!-- Vertical Cards Grid -->
<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 20px;">
    <SfCard>
        <CardHeader Title="Metric 1"></CardHeader>
        <CardContent Content="Value: 1,234"></CardContent>
    </SfCard>
    <!-- More cards -->
</div>
```

## Best Practices

### Do's
- ✓ Use vertical layout for simple content (default)
- ✓ Use horizontal layout for featured items with images
- ✓ Always wrap stacked content in CardStacked for horizontal cards
- ✓ Use responsive grid layouts for multiple cards
- ✓ Test layouts on different screen sizes

### Don'ts
- ✗ Don't use CardStacked in vertical layouts
- ✗ Don't create overly complex nested layouts
- ✗ Don't assume card dimensions on responsive layouts
- ✗ Don't mix many card orientations without clear purpose

## Troubleshooting

### Horizontal Card Layout Not Working
- Verify Orientation="CardOrientation.Horizontal" is set
- Ensure content is wrapped in CardStacked
- Check image is placed after CardStacked

### Elements Not Aligning Properly
- Use CSS for precise alignment and spacing
- Check parent container CSS for conflicts
- Verify card width is explicitly set if needed

### Responsive Layout Issues
- Use CSS Grid or Flexbox for containers, not cards
- Set proper responsive breakpoints
- Test on actual devices or browser dev tools

### Image Sizing Problems
- Set explicit width/height on img elements
- Use CSS `object-fit` for proper scaling
- Ensure image source path is correct
