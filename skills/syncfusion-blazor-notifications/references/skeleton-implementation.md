# Skeleton - Complete Implementation Guide

This guide covers the Syncfusion Blazor Skeleton component for creating loading placeholders with shimmer effects.

## Overview

The SfSkeleton component displays animated placeholder shapes while content is loading, providing visual feedback and improving perceived performance. It's a modern alternative to traditional loading spinners.

**When to use Skeleton:**
- Content is loading asynchronously (API calls, images, data)
- Need to show layout structure before content arrives
- Want to improve perceived performance
- Display loading state for cards, lists, profiles, articles
- Replace spinning loaders with content-aware placeholders

**Benefits:**
- Better UX - Users see layout structure immediately
- Reduced perceived wait time
- Content-aware loading (shapes match actual content)
- Professional, modern appearance
- Accessibility-friendly loading states

---

## Getting Started

### Installation

Requires Syncfusion.Blazor.Notifications package:

```bash
dotnet add package Syncfusion.Blazor.Notifications
dotnet add package Syncfusion.Blazor.Themes
```

### Basic Skeleton

```razor
@page "/skeleton-basic"
@using Syncfusion.Blazor.Notifications

<SfSkeleton />
```

This creates a default rectangular skeleton with pulse animation.

### Skeleton with Dimensions

```razor
<SfSkeleton Width="100%" Height="100px" />

<SfSkeleton Width="200px" Height="200px" />

<SfSkeleton Width="80%" Height="50px" />
```

---

## Skeleton Shapes

The Skeleton supports four predefined shapes for different content types.

### Rectangle (Default)

```razor
<SfSkeleton Shape="SkeletonType.Rectangle" Width="100%" Height="150px" />
```

**Use for:**
- Images
- Video thumbnails
- Cards
- Banners
- Content blocks

### Circle

```razor
<SfSkeleton Shape="SkeletonType.Circle" Width="60px" Height="60px" />
```

**Use for:**
- Avatar placeholders
- Profile pictures
- Icon placeholders
- Status indicators
- Circular images

### Square

```razor
<SfSkeleton Shape="SkeletonType.Square" Width="100px" Height="100px" />
```

**Use for:**
- Square thumbnails
- App icons
- Product images
- Grid items
- Tile layouts

### Text

```razor
<SfSkeleton Shape="SkeletonType.Text" Width="100%"  Height="15px" />
```

**Use for:**
- Text lines
- Headings
- Paragraphs
- Labels
- Descriptions

### All Shapes Example

```razor
@page "/skeleton-shapes"

<div class="skeleton-demo">
    <h3>Skeleton Shapes</h3>
    
    <div class="shape-example">
        <h4>Circle</h4>
        <SfSkeleton Shape="SkeletonType.Circle" Width="80px" Height="80px" />
    </div>
    
    <div class="shape-example">
        <h4>Square</h4>
        <SfSkeleton Shape="SkeletonType.Square" Width="100px" Height="100px" />
    </div>
    
    <div class="shape-example">
        <h4>Rectangle</h4>
        <SfSkeleton Shape="SkeletonType.Rectangle" Width="200px" Height="120px" />
    </div>
    
    <div class="shape-example">
        <h4>Text</h4>
        <SfSkeleton Shape="SkeletonType.Text" Width="250px" Height="15px" />
        <SfSkeleton Shape="SkeletonType.Text" Width="200px" Height="30px" />
        <SfSkeleton Shape="SkeletonType.Text" Width="180px" Height="45px" />
    </div>
</div>

<style>
    .skeleton-demo {
        padding: 20px;
    }
    
    .shape-example {
        margin-bottom: 30px;
    }
    
    .shape-example h4 {
        margin-bottom: 10px;
        color: #666;
    }
</style>
```

---

## Shimmer Effects

Control the animation style with different shimmer effects.

### Pulse (Default)

Fading in and out animation.

```razor
<SfSkeleton Effect="ShimmerEffect.Pulse" Width="100%" Height="100px" />
```

**Best for:**
- General loading states
- Subtle animations
- Conservative designs

### Wave

Shimmer wave moving across the skeleton.

```razor
<SfSkeleton Effect="ShimmerEffect.Wave" Width="100%" Height="100px" />
```

**Best for:**
- Modern, dynamic appearance
- Grabbing attention
- Loading larger content blocks

### Fade

Smooth fade in/out effect.

```razor
<SfSkeleton Effect="ShimmerEffect.Fade" Width="100%" Height="100px" />
```

**Best for:**
- Gentle, subtle loading
- Minimalist designs
- Background loading

### None

No animation.

```razor
<SfSkeleton Effect="ShimmerEffect.None" Width="100%" Height="100px" />
```

**Best for:**
- Static placeholders
- Performance-critical scenarios
- High-density layouts with many skeletons

### Effects Comparison

```razor
@page "/skeleton-effects"

<div class="effects-demo">
    <h3>Shimmer Effects</h3>
    
    <div class="effect-row">
        <div class="effect-item">
            <h4>Pulse</h4>
            <SfSkeleton Effect="ShimmerEffect.Pulse" Width="100%" Height="80px" />
        </div>
        
        <div class="effect-item">
            <h4>Wave</h4>
            <SfSkeleton Effect="ShimmerEffect.Wave" Width="100%" Height="80px" />
        </div>
        
        <div class="effect-item">
            <h4>Fade</h4>
            <SfSkeleton Effect="ShimmerEffect.Fade" Width="100%" Height="80px" />
        </div>
        
        <div class="effect-item">
            <h4>None</h4>
            <SfSkeleton Effect="ShimmerEffect.None" Width="100%" Height="80px" />
        </div>
    </div>
</div>

<style>
    .effects-demo {
        padding: 20px;
    }
    
    .effect-row {
        display: grid;
        grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
        gap: 20px;
    }
    
    .effect-item h4 {
        margin-bottom: 10px;
        color: #666;
    }
</style>
```

---

## Visibility Control

Toggle skeleton visibility based on loading state.

### Basic Toggle

```razor
@page "/skeleton-visibility"

<SfButton @onclick="ToggleLoading">
    @(isLoading ? "Stop" : "Start") Loading
</SfButton>

<div class="content-area">
    <SfSkeleton Visible="@isLoading" Width="100%" Height="100px" />
    
    @if (!isLoading)
    {
        <div class="actual-content">
            <h3>Content Loaded</h3>
            <p>This is the actual content that replaced the skeleton.</p>
        </div>
    }
</div>

@code {
    private bool isLoading = true;

    private void ToggleLoading()
    {
        isLoading = !isLoading;
    }
}
```

### Async Data Loading

```razor
@page "/skeleton-data"

@if (isLoading)
{
    <div class="product-skeleton">
        <SfSkeleton Shape="SkeletonType.Rectangle" Width="100%" Height="200px" />
        <SfSkeleton Shape="SkeletonType.Text" Width="80%" />
        <SfSkeleton Shape="SkeletonType.Text" Width="60%" />
    </div>
}
else if (product != null)
{
    <div class="product-card">
        <img src="@product.ImageUrl" alt="@product.Name" />
        <h3>@product.Name</h3>
        <p>@product.Description</p>
        <p class="price">$@product.Price</p>
    </div>
}

@code {
    private bool isLoading = true;
    private Product product;

    protected override async Task OnInitializedAsync()
    {
        await LoadProduct();
    }

    private async Task LoadProduct()
    {
        isLoading = true;
        await Task.Delay(2000); // Simulate API call
        
        product = new Product
        {
            Name = "Wireless Headphones",
            Description = "Premium sound quality with noise cancellation",
            Price = 199.99m,
            ImageUrl = "/images/product.jpg"
        };
        
        isLoading = false;
    }

    private class Product
    {
        public string Name { get; set; }
        public string Description { get; set; }
        public decimal Price { get; set; }
        public string ImageUrl { get; set; }
    }
}

<style>
    .product-skeleton, .product-card {
        max-width: 300px;
        padding: 15px;
        border: 1px solid #ddd;
        border-radius: 8px;
    }
    
    .product-skeleton > * {
        margin-bottom: 10px;
    }
    
    .product-card img {
        width: 100%;
        height: 200px;
        object-fit: cover;
        border-radius: 4px;
        margin-bottom: 10px;
    }
    
    .price {
        font-size: 24px;
        font-weight: bold;
        color: #2196F3;
    }
</style>
```

---

## Common Loading Patterns

### Profile Card Skeleton

```razor
<div class="profile-skeleton">
    <SfSkeleton Shape="SkeletonType.Circle" Width="80px" Height="80px" />
    <div class="profile-text">
        <SfSkeleton Shape="SkeletonType.Text" Width="60%" Height="20px" />
        <SfSkeleton Shape="SkeletonType.Text" Width="40%" Height="16px" />
    </div>
</div>

<style>
    .profile-skeleton {
        display: flex;
        align-items: center;
        gap: 16px;
        padding: 16px;
        border: 1px solid #e0e0e0;
        border-radius: 8px;
    }
    
    .profile-text {
        flex: 1;
        display: flex;
        flex-direction: column;
        gap: 8px;
    }
</style>
```

### Article Skeleton

```razor
<div class="article-skeleton">
    <SfSkeleton Shape="SkeletonType.Rectangle" Width="100%" Height="250px" />
    <SfSkeleton Shape="SkeletonType.Text" Width="90%" Height="28px" />
    <SfSkeleton Shape="SkeletonType.Text" Width="100%" Height="16px" />
    <SfSkeleton Shape="SkeletonType.Text" Width="100%" Height="16px" />
    <SfSkeleton Shape="SkeletonType.Text" Width="75%" Height="16px" />
</div>

<style>
    .article-skeleton {
        max-width: 600px;
        display: flex;
        flex-direction: column;
        gap: 12px;
    }
</style>
```

### List Skeleton

```razor
<div class="list-skeleton">
    @for (int i = 0; i < 5; i++)
    {
        <div class="list-item-skeleton">
            <SfSkeleton Shape="SkeletonType.Circle" Width="40px" Height="40px" />
            <div class="list-item-text">
                <SfSkeleton Shape="SkeletonType.Text" Width="70%" Height="16px" />
                <SfSkeleton Shape="SkeletonType.Text" Width="50%" Height="14px" />
            </div>
        </div>
    }
</div>

<style>
    .list-skeleton {
        max-width: 400px;
    }
    
    .list-item-skeleton {
        display: flex;
        align-items: center;
        gap: 12px;
        padding: 12px;
        border-bottom: 1px solid #f0f0f0;
    }
    
    .list-item-text {
        flex: 1;
        display: flex;
        flex-direction: column;
        gap: 6px;
    }
</style>
```

### Grid Skeleton

```razor
<div class="grid-skeleton">
    @for (int i = 0; i < 6; i++)
    {
        <div class="grid-item-skeleton">
            <SfSkeleton Shape="SkeletonType.Rectangle" Width="100%" Height="180px" />
            <SfSkeleton Shape="SkeletonType.Text" Width="80%" Height="18px" />
            <SfSkeleton Shape="SkeletonType.Text" Width="60%" Height="14px" />
        </div>
    }
</div>

<style>
    .grid-skeleton {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(200px, 1fr));
        gap: 16px;
    }
    
    .grid-item-skeleton {
        display: flex;
        flex-direction: column;
        gap: 8px;
    }
</style>
```

### Table Skeleton

```razor
<div class="table-skeleton">
    <div class="table-header-skeleton">
        <SfSkeleton Shape="SkeletonType.Text" Width="100px" Height="16px" />
        <SfSkeleton Shape="SkeletonType.Text" Width="120px" Height="16px" />
        <SfSkeleton Shape="SkeletonType.Text" Width="80px" Height="16px" />
    </div>
    
    @for (int i = 0; i < 4; i++)
    {
        <div class="table-row-skeleton">
            <SfSkeleton Shape="SkeletonType.Text" Width="100px" Height="14px" />
            <SfSkeleton Shape="SkeletonType.Text" Width="120px" Height="14px" />
            <SfSkeleton Shape="SkeletonType.Text" Width="80px" Height="14px" />
        </div>
    }
</div>

<style>
    .table-skeleton {
        max-width: 600px;
        border: 1px solid #e0e0e0;
        border-radius: 4px;
        overflow: hidden;
    }
    
    .table-header-skeleton {
        display: flex;
        gap: 20px;
        padding: 16px;
        background: #f5f5f5;
        border-bottom: 1px solid #e0e0e0;
    }
    
    .table-row-skeleton {
        display: flex;
        gap: 20px;
        padding: 16px;
        border-bottom: 1px solid #f0f0f0;
    }
    
    .table-row-skeleton:last-child {
        border-bottom: none;
    }
</style>
```

---

## Accessibility

### Accessibility Labels

Provide descriptive labels for screen readers.

```razor
<SfSkeleton Shape="SkeletonType.Circle" 
            Width="60px" 
            Height="60px" 
            Label="Loading user profile picture" />

<SfSkeleton Shape="SkeletonType.Text" 
            Width="100%" 
            Label="Loading article content" />
```

### ARIA Attributes

Skeletons automatically include `aria-busy="true"` and `aria-label` attributes for accessibility.

### Complete Accessible Pattern

```razor
<div role="region" aria-busy="@isLoading" aria-label="Product information">
    @if (isLoading)
    {
        <div class="product-skeleton" aria-label="Loading product details">
            <SfSkeleton Shape="SkeletonType.Rectangle" 
                        Width="100%" 
                        Height="200px" 
                        Label="Loading product image" />
            <SfSkeleton Shape="SkeletonType.Text" 
                        Width="80%" 
                        Label="Loading product name" />
            <SfSkeleton Shape="SkeletonType.Text" 
                        Width="60%" 
                        Label="Loading product description" />
        </div>
    }
    else
    {
        <!-- Actual content -->
    }
</div>
```

---

## Custom Styling

### Custom Colors

```razor
<SfSkeleton Width="100%" 
            Height="100px" 
            CssClass="custom-skeleton-color" />

<style>
    .custom-skeleton-color {
        background: linear-gradient(90deg, #e3f2fd 0%, #bbdefb 50%, #e3f2fd 100%);
    }
</style>
```

### Custom Animation Duration

```razor
<SfSkeleton Width="100%" 
            Height="100px" 
            CssClass="slow-animation" />

<style>
    .slow-animation {
        animation-duration: 3s !important;
    }
</style>
```

### Rounded Corners

```razor
<SfSkeleton Width="100%" 
            Height="100px" 
            CssClass="rounded-skeleton" />

<style>
    .rounded-skeleton {
        border-radius: 12px;
    }
</style>
```

---

## Complete Working Examples

### E-commerce Product Grid

```razor
@page "/skeleton-ecommerce"

<div class="product-grid">
    @if (isLoading)
    {
        @for (int i = 0; i < 8; i++)
        {
            <div class="product-card-skeleton">
                <SfSkeleton Shape="SkeletonType.Rectangle" Width="100%" Height="200px" />
                <SfSkeleton Shape="SkeletonType.Text" Width="80%" Height="20px" />
                <SfSkeleton Shape="SkeletonType.Text" Width="60%" Height="16px" />
                <SfSkeleton Shape="SkeletonType.Text" Width="40%" Height="24px" />
            </div>
        }
    }
    else
    {
        @foreach (var product in products)
        {
            <div class="product-card">
                <img src="@product.ImageUrl" alt="@product.Name" />
                <h3>@product.Name</h3>
                <p>@product.Description</p>
                <p class="price">$@product.Price</p>
            </div>
        }
    }
</div>

@code {
    private bool isLoading = true;
    private List<Product> products = new();

    protected override async Task OnInitializedAsync()
    {
        await LoadProducts();
    }

    private async Task LoadProducts()
    {
        await Task.Delay(2000); // Simulate API call
        // Load products...
        isLoading = false;
    }
}

<style>
    .product-grid {
        display: grid;
        grid-template-columns: repeat(auto-fill, minmax(250px, 1fr));
        gap: 20px;
        padding: 20px;
    }
    
    .product-card-skeleton, .product-card {
        padding: 15px;
        border: 1px solid #e0e0e0;
        border-radius: 8px;
    }
    
    .product-card-skeleton > * {
        margin-bottom: 10px;
    }
</style>
```

### Social Media Feed

```razor
@page "/skeleton-social"

<div class="feed-container">
    @if (isLoading)
    {
        @for (int i = 0; i < 3; i++)
        {
            <div class="post-skeleton">
                <div class="post-header-skeleton">
                    <SfSkeleton Shape="SkeletonType.Circle" Width="50px" Height="50px" />
                    <div class="header-text">
                        <SfSkeleton Shape="SkeletonType.Text" Width="150px" Height="18px" />
                        <SfSkeleton Shape="SkeletonType.Text" Width="100px" Height="14px" />
                    </div>
                </div>
                <SfSkeleton Shape="SkeletonType.Text" Width="100%" Height="16px" />
                <SfSkeleton Shape="SkeletonType.Text" Width="90%" Height="16px" />
                <SfSkeleton Shape="SkeletonType.Rectangle" Width="100%" Height="300px" />
            </div>
        }
    }
    else
    {
        <!-- Actual posts -->
    }
</div>

<style>
    .feed-container {
        max-width: 600px;
        margin: 0 auto;
    }
    
    .post-skeleton {
        padding: 20px;
        margin-bottom: 20px;
        border: 1px solid #e0e0e0;
        border-radius: 8px;
        background: white;
    }
    
    .post-header-skeleton {
        display: flex;
        gap: 12px;
        margin-bottom: 15px;
    }
    
    .header-text {
        display: flex;
        flex-direction: column;
        gap: 6px;
    }
    
    .post-skeleton > .e-skeleton {
        margin-bottom: 10px;
    }
</style>
```

---

## Performance Considerations

1. **Use `Effect="ShimmerEffect.None"`** for high-density layouts with many skeletons
2. **Limit skeleton count** - Don't render hundreds of skeletons at once
3. **Match skeleton to content** - Skeleton should closely resemble actual content
4. **Remove from DOM** - Hide skeletons when content loads (`Visible="false"` or conditional rendering)
5. **Optimize dimensions** - Use percentage widths where possible

---

## Best Practices

1. **Match content structure** - Skeleton shapes should mirror actual content layout
2. **Consistent timing** - Show skeleton for at least 300-500ms for smooth transition
3. **Progressive loading** - Show content as it loads, not all at once
4. **Appropriate effects** - Use Wave for attention, Pulse for subtlety
5. **Accessibility first** - Always provide descriptive labels
6. **Mobile responsive** - Ensure skeletons work on all screen sizes
7. **Avoid overuse** - Don't skeleton everything; use for main content only
8. **Test loading states** - Throttle network to test skeleton appearance
9. **Gradual reveal** - Fade in content when replacing skeleton
10. **Combine with suspense** - Use with error boundaries for robust loading states

---

## Next Steps

- Learn about [Toast Notifications](toast-getting-started.md) for feedback messages
- Explore [Message Component](message-implementation.md) for inline alerts
- Review [Styling Guide](styling-and-themes.md) for customization
- See [Use Cases](use-cases-and-examples.md) for combined examples
