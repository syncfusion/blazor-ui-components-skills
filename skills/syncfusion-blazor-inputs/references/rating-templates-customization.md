# Rating - Templates and Customization

## Table of Contents
- [Templates Overview](#templates-overview)
- [EmptyTemplate Configuration](#emptytemplate-configuration)
- [FullTemplate Configuration](#fulltemplate-configuration)
- [RatingItemContext Usage](#ratingitemcontext-usage)
- [Custom Icon Examples](#custom-icon-examples)
- [CssClass for Custom Styling](#cssclass-for-custom-styling)
- [EnableAnimation Property](#enableanimation-property)
- [Theme Customization](#theme-customization)
- [Responsive Design Patterns](#responsive-design-patterns)
- [Advanced Customization Examples](#advanced-customization-examples)

---

## Templates Overview

The Rating component provides two template properties to fully customize the appearance of rating items:

| Template | Description | Use For |
|----------|-------------|---------|
| **EmptyTemplate** | Defines unselected/empty rating items | Custom icons for unselected state |
| **FullTemplate** | Defines selected/filled rating items | Custom icons for selected state |

Both templates receive a `RatingItemContext` object containing:
- `Index`: 0-based index of the rating item
- `Value`: Current rating value for that item position

---

## EmptyTemplate Configuration

The `EmptyTemplate` defines how unselected rating items appear.

### Basic EmptyTemplate

```razor
<SfRating @bind-Value="@rating" ItemsCount="5">
    <EmptyTemplate>
        <span style="font-size: 24px;">☆</span>
    </EmptyTemplate>
</SfRating>

@code {
    private double rating = 3;
}
```

### Empty vs Full Template Example

```razor
@page "/template-comparison"
@using Syncfusion.Blazor.Inputs

<div class="template-demo">
    <h3>Default Stars (No Custom Templates)</h3>
    <SfRating @bind-Value="@rating1" ItemsCount="5"></SfRating>
    
    <h3>Custom Templates (Unicode Stars)</h3>
    <SfRating @bind-Value="@rating2" ItemsCount="5">
        <EmptyTemplate>
            <span style="font-size: 28px; color: #ddd;">☆</span>
        </EmptyTemplate>
        <FullTemplate>
            <span style="font-size: 28px; color: #FFD700;">★</span>
        </FullTemplate>
    </SfRating>
</div>

@code {
    private double rating1 = 3;
    private double rating2 = 3;
}
```

---

## FullTemplate Configuration

The `FullTemplate` defines how selected rating items appear.

### Basic FullTemplate

```razor
<SfRating @bind-Value="@rating" ItemsCount="5">
    <FullTemplate>
        <span style="font-size: 24px; color: gold;">★</span>
    </FullTemplate>
</SfRating>

@code {
    private double rating = 4;
}
```

---

## RatingItemContext Usage

Both templates receive a `RatingItemContext` object with item information.

### RatingItemContext Properties

| Property | Type | Description |
|----------|------|-------------|
| **Index** | int | 0-based position (0 to ItemsCount-1) |
| **Value** | double | Rating value at this position |

### Using Context in Templates

```razor
@page "/context-usage"
@using Syncfusion.Blazor.Inputs

<div class="context-demo">
    <h3>Numbered Rating Items</h3>
    
    <SfRating @bind-Value="@rating" ItemsCount="5">
        <EmptyTemplate>
            <div class="rating-box empty">
                @(context.Index + 1)
            </div>
        </EmptyTemplate>
        <FullTemplate>
            <div class="rating-box filled">
                @(context.Index + 1)
            </div>
        </FullTemplate>
    </SfRating>
</div>

@code {
    private double rating = 3;
}

<style>
    .rating-box {
        width: 40px;
        height: 40px;
        display: flex;
        align-items: center;
        justify-content: center;
        font-weight: 700;
        font-size: 18px;
        border-radius: 6px;
        cursor: pointer;
        transition: all 0.3s;
    }
    
    .rating-box.empty {
        background: #f0f0f0;
        color: #999;
        border: 2px solid #ddd;
    }
    
    .rating-box.filled {
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        color: white;
        border: 2px solid #764ba2;
    }
</style>
```

---

## Custom Icon Examples

### Heart Icons

```razor
@page "/heart-rating"
@using Syncfusion.Blazor.Inputs

<div class="heart-rating">
    <h3>Love This Product? ❤️</h3>
    
    <SfRating @bind-Value="@loveRating" ItemsCount="5">
        <EmptyTemplate>
            <span style="font-size: 32px; color: #ddd;">♡</span>
        </EmptyTemplate>
        <FullTemplate>
            <span style="font-size: 32px; color: #ff4757;">♥</span>
        </FullTemplate>
    </SfRating>
    
    <p class="rating-text">
        @if (loveRating >= 4)
        {
            <text>You absolutely love it! ❤️</text>
        }
        else if (loveRating >= 3)
        {
            <text>You like it! 💕</text>
        }
        else if (loveRating > 0)
        {
            <text>It's okay 💛</text>
        }
        else
        {
            <text>Share your feelings...</text>
        }
    </p>
</div>

@code {
    private double loveRating = 0;
}

<style>
    .rating-text {
        margin-top: 20px;
        font-size: 18px;
        font-weight: 600;
        color: #333;
    }
</style>
```

### Thumbs Up/Down

```razor
@page "/thumbs-rating"
@using Syncfusion.Blazor.Inputs

<div class="thumbs-rating">
    <h3>Rate Your Experience</h3>
    
    <SfRating @bind-Value="@satisfaction" ItemsCount="5">
        <EmptyTemplate>
            <span style="font-size: 36px; color: #ccc;">👍</span>
        </EmptyTemplate>
        <FullTemplate>
            <span style="font-size: 36px; color: #28a745;">👍</span>
        </FullTemplate>
    </SfRating>
    
    <p>@satisfaction thumbs up!</p>
</div>

@code {
    private double satisfaction = 4;
}
```

### Emoji Rating

```razor
@page "/emoji-rating"
@using Syncfusion.Blazor.Inputs

<div class="emoji-rating">
    <h3>How Was Your Experience?</h3>
    
    <SfRating @bind-Value="@experience" 
              ItemsCount="5"
              EnableSingleSelection="true">
        <EmptyTemplate>
            <span style="font-size: 48px; opacity: 0.3;">
                @GetEmoji(context.Index + 1)
            </span>
        </EmptyTemplate>
        <FullTemplate>
            <span style="font-size: 48px;">
                @GetEmoji(context.Index + 1)
            </span>
        </FullTemplate>
    </SfRating>
    
    <div class="emoji-labels">
        <span>😢 Terrible</span>
        <span>😞 Bad</span>
        <span>😐 Okay</span>
        <span>😊 Good</span>
        <span>🤩 Amazing</span>
    </div>
    
    <p class="feedback">@GetFeedbackText(experience)</p>
</div>

@code {
    private double experience = 0;
    
    private string GetEmoji(int position)
    {
        return position switch
        {
            1 => "😢",
            2 => "😞",
            3 => "😐",
            4 => "😊",
            5 => "🤩",
            _ => "😐"
        };
    }
    
    private string GetFeedbackText(double rating)
    {
        return rating switch
        {
            1 => "We're sorry to hear that. We'll work on improvements.",
            2 => "Thanks for your feedback. We'll do better.",
            3 => "Thanks! We'll keep improving.",
            4 => "Great to hear! We appreciate your support.",
            5 => "Wow! Thank you for the amazing feedback!",
            _ => "Click an emoji to rate your experience"
        };
    }
}

<style>
    .emoji-labels {
        display: flex;
        justify-content: space-between;
        margin-top: 15px;
        font-size: 12px;
        color: #666;
    }
    
    .feedback {
        margin-top: 25px;
        padding: 15px;
        background: #f8f9fa;
        border-radius: 8px;
        font-weight: 500;
        color: #333;
        text-align: center;
    }
</style>
```

### Custom SVG Icons

```razor
@page "/svg-rating"
@using Syncfusion.Blazor.Inputs

<div class="svg-rating">
    <h3>Custom SVG Star Rating</h3>
    
    <SfRating @bind-Value="@rating" ItemsCount="5">
        <EmptyTemplate>
            <svg width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="#ccc" stroke-width="2">
                <polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/>
            </svg>
        </EmptyTemplate>
        <FullTemplate>
            <svg width="32" height="32" viewBox="0 0 24 24" fill="#FFD700" stroke="#FFA500" stroke-width="1">
                <polygon points="12 2 15.09 8.26 22 9.27 17 14.14 18.18 21.02 12 17.77 5.82 21.02 7 14.14 2 9.27 8.91 8.26 12 2"/>
            </svg>
        </FullTemplate>
    </SfRating>
</div>

@code {
    private double rating = 4;
}
```

---

## CssClass for Custom Styling

The `CssClass` property applies custom CSS classes to the rating component wrapper.

### Basic Custom Styling

```razor
<SfRating @bind-Value="@rating" 
          CssClass="custom-rating">
</SfRating>

<style>
    .custom-rating {
        /* Your custom styles */
    }
</style>

@code {
    private double rating = 3;
}
```

### Styled Rating Examples

```razor
@page "/styled-ratings"
@using Syncfusion.Blazor.Inputs

<div class="styled-ratings">
    <h3>Custom Styled Ratings</h3>
    
    <div class="rating-example">
        <h4>Large Rating</h4>
        <SfRating @bind-Value="@rating1" CssClass="large-rating"></SfRating>
    </div>
    
    <div class="rating-example">
        <h4>Colorful Rating</h4>
        <SfRating @bind-Value="@rating2" CssClass="colorful-rating">
            <EmptyTemplate>
                <span>☆</span>
            </EmptyTemplate>
            <FullTemplate>
                <span>★</span>
            </FullTemplate>
        </SfRating>
    </div>
    
    <div class="rating-example">
        <h4>Rounded Background</h4>
        <SfRating @bind-Value="@rating3" CssClass="rounded-rating"></SfRating>
    </div>
</div>

@code {
    private double rating1 = 4;
    private double rating2 = 3;
    private double rating3 = 5;
}

<style>
    .rating-example {
        margin: 30px 0;
        padding: 20px;
        background: #f8f9fa;
        border-radius: 8px;
    }
    
    .rating-example h4 {
        margin: 0 0 15px;
    }
    
    /* Large Rating */
    .large-rating {
        font-size: 48px;
    }
    
    /* Colorful Rating */
    .colorful-rating span {
        font-size: 36px;
        transition: all 0.3s;
    }
    
    .colorful-rating span:nth-child(1) { color: #ff4757; }
    .colorful-rating span:nth-child(2) { color: #ff6348; }
    .colorful-rating span:nth-child(3) { color: #ffa502; }
    .colorful-rating span:nth-child(4) { color: #26de81; }
    .colorful-rating span:nth-child(5) { color: #20bf6b; }
    
    /* Rounded Background */
    .rounded-rating {
        padding: 15px 25px;
        background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
        border-radius: 50px;
        display: inline-block;
    }
</style>
```

---

## EnableAnimation Property

The `EnableAnimation` property (default: `true`) adds smooth transitions when rating changes.

### Animation Control

```razor
@page "/animation-control"
@using Syncfusion.Blazor.Inputs

<div class="animation-demo">
    <h3>Animation Comparison</h3>
    
    <div class="demo-section">
        <label>With Animation (Default)</label>
        <SfRating @bind-Value="@rating1" EnableAnimation="true"></SfRating>
    </div>
    
    <div class="demo-section">
        <label>Without Animation</label>
        <SfRating @bind-Value="@rating2" EnableAnimation="false"></SfRating>
    </div>
</div>

@code {
    private double rating1 = 3;
    private double rating2 = 3;
}

<style>
    .demo-section {
        margin: 25px 0;
        padding: 20px;
        background: #f8f9fa;
        border-radius: 8px;
    }
    
    .demo-section label {
        display: block;
        margin-bottom: 15px;
        font-weight: 600;
    }
</style>
```

**When to Disable Animation:**
- Performance-critical scenarios
- Accessibility requirements (motion sensitivity)
- Read-only displays
- Bulk rating operations

---

## Theme Customization

### Using Syncfusion Theme Studio

1. Visit [Syncfusion Theme Studio](https://blazor.syncfusion.com/themestudio/)
2. Select Rating component
3. Customize colors, sizes, spacing
4. Download custom theme CSS
5. Replace default theme in your app

### Custom Theme Example

```css
/* Custom Rating Theme */
.e-rating {
    --rating-color: #667eea;
    --rating-empty-color: #e0e0e0;
    --rating-hover-color: #764ba2;
}

.e-rating .e-rating-item {
    color: var(--rating-empty-color);
    transition: color 0.3s;
}

.e-rating .e-rating-item.e-rating-selected {
    color: var(--rating-color);
}

.e-rating .e-rating-item:hover {
    color: var(--rating-hover-color);
}
```

---

## Responsive Design Patterns

### Mobile-Friendly Rating

```razor
@page "/responsive-rating"
@using Syncfusion.Blazor.Inputs

<div class="responsive-rating">
    <h3>Responsive Rating Component</h3>
    
    <SfRating @bind-Value="@rating" 
              ItemsCount="5"
              CssClass="adaptive-rating">
        <EmptyTemplate>
            <span class="rating-icon">☆</span>
        </EmptyTemplate>
        <FullTemplate>
            <span class="rating-icon filled">★</span>
        </FullTemplate>
    </SfRating>
</div>

@code {
    private double rating = 4;
}

<style>
    /* Base styles (mobile-first) */
    .rating-icon {
        font-size: 32px;
        color: #ddd;
        padding: 5px;
    }
    
    .rating-icon.filled {
        color: #FFD700;
    }
    
    /* Tablet */
    @media (min-width: 768px) {
        .rating-icon {
            font-size: 40px;
            padding: 8px;
        }
    }
    
    /* Desktop */
    @media (min-width: 1024px) {
        .rating-icon {
            font-size: 48px;
            padding: 10px;
        }
    }
</style>
```

---

## Advanced Customization Examples

### Multi-Color Gradient Rating

```razor
@page "/gradient-rating"
@using Syncfusion.Blazor.Inputs

<div class="gradient-rating">
    <h3>Multi-Color Rating (Red to Green)</h3>
    
    <SfRating @bind-Value="@rating" ItemsCount="5">
        <EmptyTemplate>
            <div class="gradient-box empty"></div>
        </EmptyTemplate>
        <FullTemplate>
            <div class="gradient-box" style="background: @GetGradientColor(context.Index)"></div>
        </FullTemplate>
    </SfRating>
    
    <p class="score">Score: @rating / 5</p>
</div>

@code {
    private double rating = 3;
    
    private string GetGradientColor(int index)
    {
        return index switch
        {
            0 => "#dc3545", // Red
            1 => "#fd7e14", // Orange
            2 => "#ffc107", // Yellow
            3 => "#20c997", // Teal
            4 => "#28a745", // Green
            _ => "#6c757d"
        };
    }
}

<style>
    .gradient-box {
        width: 50px;
        height: 50px;
        border-radius: 8px;
        transition: all 0.3s;
    }
    
    .gradient-box.empty {
        background: #f0f0f0;
        border: 2px solid #ddd;
    }
    
    .gradient-box:not(.empty) {
        box-shadow: 0 4px 8px rgba(0,0,0,0.2);
        transform: scale(1.1);
    }
    
    .score {
        margin-top: 20px;
        font-size: 20px;
        font-weight: 600;
    }
</style>
```

### Animated Icon Rating

```razor
@page "/animated-rating"
@using Syncfusion.Blazor.Inputs

<div class="animated-rating">
    <h3>Animated Star Rating</h3>
    
    <SfRating @bind-Value="@rating" 
              ItemsCount="5"
              CssClass="star-animation">
        <EmptyTemplate>
            <span class="star-icon empty">⭐</span>
        </EmptyTemplate>
        <FullTemplate>
            <span class="star-icon filled">⭐</span>
        </FullTemplate>
    </SfRating>
</div>

@code {
    private double rating = 4;
}

<style>
    .star-icon {
        font-size: 48px;
        display: inline-block;
        transition: all 0.3s;
    }
    
    .star-icon.empty {
        filter: grayscale(100%);
        opacity: 0.3;
    }
    
    .star-icon.filled {
        animation: starPulse 0.5s ease;
    }
    
    .star-icon:hover {
        transform: scale(1.2) rotate(15deg);
    }
    
    @keyframes starPulse {
        0%, 100% { transform: scale(1); }
        50% { transform: scale(1.3); }
    }
</style>
```

### Product Card with Styled Rating

```razor
@page "/product-card"
@using Syncfusion.Blazor.Inputs

<div class="product-card">
    <img src="https://via.placeholder.com/300x200" alt="Product" class="product-image" />
    
    <div class="product-details">
        <h3>Premium Wireless Headphones</h3>
        <p class="price">$199.99</p>
        
        <div class="rating-section">
            <SfRating Value="@averageRating" 
                      ReadOnly="true"
                      Precision="PrecisionType.Exact"
                      CssClass="product-rating">
                <EmptyTemplate>
                    <span style="color: #e0e0e0; font-size: 20px;">★</span>
                </EmptyTemplate>
                <FullTemplate>
                    <span style="color: #FFD700; font-size: 20px;">★</span>
                </FullTemplate>
            </SfRating>
            <span class="rating-info">@averageRating.ToString("F1") (@reviewCount reviews)</span>
        </div>
        
        <p class="description">
            Experience premium sound quality with active noise cancellation 
            and up to 30 hours of battery life.
        </p>
        
        <h4>Rate This Product:</h4>
        <SfRating @bind-Value="@userRating" 
                  Precision="PrecisionType.Half"
                  ShowLabel="true"
                  LabelPosition="LabelPosition.Right">
            <LabelTemplate>
                <span class="user-rating-label">@context / 5</span>
            </LabelTemplate>
        </SfRating>
        
        <button class="btn-primary" @onclick="SubmitRating">
            Submit Rating
        </button>
    </div>
</div>

@code {
    private double averageRating = 4.7;
    private int reviewCount = 1243;
    private double userRating = 0;
    
    private async Task SubmitRating()
    {
        // Submit rating to server
        await Task.Delay(500);
        // Show confirmation, update average, etc.
    }
}

<style>
    .product-card {
        max-width: 400px;
        background: white;
        border-radius: 12px;
        box-shadow: 0 4px 12px rgba(0,0,0,0.1);
        overflow: hidden;
        margin: 20px auto;
    }
    
    .product-image {
        width: 100%;
        height: 200px;
        object-fit: cover;
    }
    
    .product-details {
        padding: 20px;
    }
    
    .product-details h3 {
        margin: 0 0 10px;
        color: #333;
    }
    
    .price {
        font-size: 24px;
        font-weight: 700;
        color: #28a745;
        margin: 10px 0;
    }
    
    .rating-section {
        display: flex;
        align-items: center;
        gap: 10px;
        margin: 15px 0;
    }
    
    .rating-info {
        color: #666;
        font-size: 14px;
    }
    
    .description {
        color: #666;
        line-height: 1.6;
        margin: 15px 0;
    }
    
    .product-details h4 {
        margin: 20px 0 10px;
        color: #333;
    }
    
    .user-rating-label {
        margin-left: 10px;
        font-weight: 600;
        color: #333;
    }
    
    .btn-primary {
        width: 100%;
        padding: 12px;
        margin-top: 15px;
        background: #0d6efd;
        color: white;
        border: none;
        border-radius: 6px;
        font-size: 16px;
        font-weight: 600;
        cursor: pointer;
    }
    
    .btn-primary:hover {
        background: #0b5ed7;
    }
</style>
```

This comprehensive guide covers all template and customization options for creating unique and visually appealing rating experiences with the Syncfusion Blazor Rating component.
