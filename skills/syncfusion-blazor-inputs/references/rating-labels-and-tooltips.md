# Rating - Labels and Tooltips

## Overview

Labels and tooltips enhance the Rating component by providing additional context, descriptions, and real-time feedback. Labels display static or dynamic text alongside the rating, while tooltips show information when hovering over rating items.

**Key Features:**
- **ShowLabel:** Display/hide the rating label
- **LabelPosition:** Position label relative to rating (Top, Bottom, Left, Right)
- **LabelTemplate:** Customize label content with templates
- **ShowTooltip:** Enable hover tooltips
- **TooltipTemplate:** Customize tooltip content
- **Dynamic Content:** Real-time updates based on rating value

---

## Label Configuration Overview

Labels provide context and display the current rating value in a user-friendly format.

### Basic Label Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| **ShowLabel** | bool | false | Show/hide rating label |
| **LabelPosition** | LabelPosition | Bottom | Label placement (Top, Bottom, Left, Right) |
| **LabelTemplate** | RenderFragment | null | Custom label content template |

---

## ShowLabel Property

The `ShowLabel` property controls whether to display the rating value as text.

### Basic Label Display

```razor
<SfRating @bind-Value="@rating" 
          ShowLabel="true">
</SfRating>

@code {
    private double rating = 3.5;
    // Displays: "3.5" below the stars
}
```

### Label ON vs OFF Comparison

```razor
@page "/label-comparison"
@using Syncfusion.Blazor.Inputs

<div class="label-demo">
    <h3>Label Display Comparison</h3>
    
    <div class="demo-section">
        <label>Without Label (Default)</label>
        <SfRating @bind-Value="@rating1" ShowLabel="false"></SfRating>
    </div>
    
    <div class="demo-section">
        <label>With Label</label>
        <SfRating @bind-Value="@rating2" ShowLabel="true"></SfRating>
    </div>
    
    <div class="demo-section">
        <label>With Label + Half Precision</label>
        <SfRating @bind-Value="@rating3" 
                  ShowLabel="true"
                  Precision="PrecisionType.Half">
        </SfRating>
    </div>
</div>

@code {
    private double rating1 = 4;
    private double rating2 = 4;
    private double rating3 = 4.5;
}

<style>
    .demo-section {
        margin: 25px 0;
        padding: 20px;
        background: #f8f9fa;
        border-radius: 8px;
    }
    
    .demo-section > label {
        display: block;
        margin-bottom: 15px;
        font-weight: 600;
    }
</style>
```

---

## LabelPosition Options

The `LabelPosition` property determines where the label appears relative to the rating stars.

### LabelPosition Enum Values

| Position | Description | Best For |
|----------|-------------|----------|
| **Bottom** | Label below stars (default) | Most common layout |
| **Top** | Label above stars | Emphasis on value |
| **Left** | Label to the left of stars | Compact horizontal layout |
| **Right** | Label to the right of stars | Natural reading order |

### Position Examples

```razor
@page "/label-positions"
@using Syncfusion.Blazor.Inputs

<div class="position-demo">
    <h3>Label Position Options</h3>
    
    <div class="position-example">
        <label>Top Position</label>
        <SfRating @bind-Value="@rating" 
                  ShowLabel="true"
                  LabelPosition="LabelPosition.Top">
        </SfRating>
    </div>
    
    <div class="position-example">
        <label>Bottom Position (Default)</label>
        <SfRating @bind-Value="@rating" 
                  ShowLabel="true"
                  LabelPosition="LabelPosition.Bottom">
        </SfRating>
    </div>
    
    <div class="position-example">
        <label>Left Position</label>
        <SfRating @bind-Value="@rating" 
                  ShowLabel="true"
                  LabelPosition="LabelPosition.Left">
        </SfRating>
    </div>
    
    <div class="position-example">
        <label>Right Position</label>
        <SfRating @bind-Value="@rating" 
                  ShowLabel="true"
                  LabelPosition="LabelPosition.Right">
        </SfRating>
    </div>
</div>

@code {
    private double rating = 4.5;
}

<style>
    .position-example {
        margin: 30px 0;
        padding: 25px;
        background: white;
        border: 2px solid #e0e0e0;
        border-radius: 8px;
    }
    
    .position-example > label {
        display: block;
        margin-bottom: 15px;
        font-weight: 600;
        color: #333;
    }
</style>
```

### Horizontal Layout with Left Label

```razor
@page "/horizontal-layout"
@using Syncfusion.Blazor.Inputs

<div class="horizontal-ratings">
    <h3>Product Ratings</h3>
    
    <div class="rating-row">
        <SfRating @bind-Value="@quality" 
                  ShowLabel="true"
                  LabelPosition="LabelPosition.Left"
                  Precision="PrecisionType.Half">
        </SfRating>
        <span class="category">Quality</span>
    </div>
    
    <div class="rating-row">
        <SfRating @bind-Value="@value" 
                  ShowLabel="true"
                  LabelPosition="LabelPosition.Left"
                  Precision="PrecisionType.Half">
        </SfRating>
        <span class="category">Value</span>
    </div>
    
    <div class="rating-row">
        <SfRating @bind-Value="@service" 
                  ShowLabel="true"
                  LabelPosition="LabelPosition.Left"
                  Precision="PrecisionType.Half">
        </SfRating>
        <span class="category">Service</span>
    </div>
</div>

@code {
    private double quality = 4.5;
    private double value = 4.0;
    private double service = 4.5;
}

<style>
    .rating-row {
        display: flex;
        align-items: center;
        gap: 20px;
        margin: 20px 0;
        padding: 15px;
        background: #f8f9fa;
        border-radius: 6px;
    }
    
    .category {
        font-weight: 600;
        color: #333;
    }
</style>
```

---

## LabelTemplate for Custom Labels

The `LabelTemplate` property allows you to customize how the label is displayed using a Blazor render fragment.

### Basic LabelTemplate

```razor
<SfRating @bind-Value="@rating" 
          ShowLabel="true">
    <LabelTemplate>
        <span>@context out of 5 stars</span>
    </LabelTemplate>
</SfRating>

@code {
    private double rating = 4;
    // Displays: "4 out of 5 stars"
}
```

**Note:** The `context` parameter contains the current rating value as a `double`.

### Custom Label Examples

```razor
@page "/custom-labels"
@using Syncfusion.Blazor.Inputs

<div class="custom-labels">
    <h3>Custom Label Templates</h3>
    
    <div class="label-example">
        <h4>Simple Text</h4>
        <SfRating @bind-Value="@rating1" ShowLabel="true">
            <LabelTemplate>
                <span>Rating: @context / 5</span>
            </LabelTemplate>
        </SfRating>
    </div>
    
    <div class="label-example">
        <h4>With Icon</h4>
        <SfRating @bind-Value="@rating2" ShowLabel="true">
            <LabelTemplate>
                <span>⭐ @context stars</span>
            </LabelTemplate>
        </SfRating>
    </div>
    
    <div class="label-example">
        <h4>Styled Badge</h4>
        <SfRating @bind-Value="@rating3" ShowLabel="true" Precision="PrecisionType.Half">
            <LabelTemplate>
                <span class="badge">@context.ToString("F1")</span>
            </LabelTemplate>
        </SfRating>
    </div>
    
    <div class="label-example">
        <h4>With Description</h4>
        <SfRating @bind-Value="@rating4" ShowLabel="true">
            <LabelTemplate>
                <div class="label-description">
                    <strong>@context / 5</strong>
                    <span>@GetRatingText(context)</span>
                </div>
            </LabelTemplate>
        </SfRating>
    </div>
</div>

@code {
    private double rating1 = 4;
    private double rating2 = 5;
    private double rating3 = 4.5;
    private double rating4 = 3;
    
    private string GetRatingText(double rating)
    {
        return rating switch
        {
            5 => "Excellent!",
            >= 4 => "Very Good",
            >= 3 => "Good",
            >= 2 => "Fair",
            _ => "Poor"
        };
    }
}

<style>
    .label-example {
        margin: 25px 0;
        padding: 20px;
        background: #f8f9fa;
        border-radius: 8px;
    }
    
    .label-example h4 {
        margin: 0 0 15px;
        color: #333;
    }
    
    .badge {
        display: inline-block;
        padding: 6px 14px;
        background: #0d6efd;
        color: white;
        border-radius: 20px;
        font-weight: 600;
        font-size: 14px;
    }
    
    .label-description {
        display: flex;
        flex-direction: column;
        gap: 5px;
    }
    
    .label-description strong {
        font-size: 18px;
        color: #333;
    }
    
    .label-description span {
        color: #666;
        font-size: 14px;
    }
</style>
```

### Dynamic Label with Color Coding

```razor
@page "/dynamic-label"
@using Syncfusion.Blazor.Inputs

<div class="dynamic-label">
    <h3>Customer Satisfaction Rating</h3>
    
    <SfRating @bind-Value="@satisfaction" 
              ShowLabel="true"
              LabelPosition="LabelPosition.Right"
              Precision="PrecisionType.Half">
        <LabelTemplate>
            <div class="dynamic-label-content" style="color: @GetLabelColor(context)">
                <strong>@context.ToString("F1")</strong>
                <span>@GetSatisfactionLevel(context)</span>
            </div>
        </LabelTemplate>
    </SfRating>
</div>

@code {
    private double satisfaction = 3.5;
    
    private string GetLabelColor(double rating)
    {
        return rating switch
        {
            >= 4.5 => "#28a745", // Green
            >= 3.5 => "#17a2b8", // Blue
            >= 2.5 => "#ffc107", // Yellow
            >= 1.5 => "#fd7e14", // Orange
            _ => "#dc3545"       // Red
        };
    }
    
    private string GetSatisfactionLevel(double rating)
    {
        return rating switch
        {
            >= 4.5 => "Highly Satisfied",
            >= 3.5 => "Satisfied",
            >= 2.5 => "Neutral",
            >= 1.5 => "Dissatisfied",
            _ => "Highly Dissatisfied"
        };
    }
}

<style>
    .dynamic-label-content {
        display: flex;
        flex-direction: column;
        gap: 5px;
        margin-left: 15px;
    }
    
    .dynamic-label-content strong {
        font-size: 24px;
    }
    
    .dynamic-label-content span {
        font-size: 14px;
        font-weight: 600;
    }
</style>
```

---

## Tooltip Configuration

Tooltips provide contextual information when users hover over rating items.

### ShowTooltip Property

The `ShowTooltip` property (default: `false`) enables tooltips on hover.

```razor
<SfRating @bind-Value="@rating" 
          ShowTooltip="true">
</SfRating>

@code {
    private double rating = 3;
    // Hovering over each star shows its value
}
```

### Basic Tooltip Example

```razor
@page "/basic-tooltip"
@using Syncfusion.Blazor.Inputs

<div class="tooltip-demo">
    <h3>Hover Over Stars to See Tooltip</h3>
    
    <div class="demo-section">
        <label>Default Tooltip (Shows Value)</label>
        <SfRating @bind-Value="@rating1" 
                  ShowTooltip="true"
                  Precision="PrecisionType.Half">
        </SfRating>
    </div>
    
    <div class="demo-section">
        <label>With Label and Tooltip</label>
        <SfRating @bind-Value="@rating2" 
                  ShowTooltip="true"
                  ShowLabel="true"
                  Precision="PrecisionType.Full">
        </SfRating>
    </div>
</div>

@code {
    private double rating1 = 4.5;
    private double rating2 = 3;
}

<style>
    .demo-section {
        margin: 30px 0;
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

---

## TooltipTemplate Customization

The `TooltipTemplate` property allows you to customize tooltip content using a Blazor render fragment.

### Basic TooltipTemplate

```razor
<SfRating @bind-Value="@rating" 
          ShowTooltip="true">
    <TooltipTemplate>
        <span>@context stars</span>
    </TooltipTemplate>
</SfRating>

@code {
    private double rating = 3;
}
```

**Note:** The `context` parameter contains the hovered item's value as a `double`.

### Custom Tooltip Examples

```razor
@page "/custom-tooltips"
@using Syncfusion.Blazor.Inputs

<div class="custom-tooltips">
    <h3>Custom Tooltip Templates</h3>
    
    <div class="tooltip-example">
        <h4>Text Description</h4>
        <SfRating @bind-Value="@rating1" ShowTooltip="true">
            <TooltipTemplate>
                <span>@GetRatingDescription(context)</span>
            </TooltipTemplate>
        </SfRating>
    </div>
    
    <div class="tooltip-example">
        <h4>Styled Tooltip</h4>
        <SfRating @bind-Value="@rating2" ShowTooltip="true" Precision="PrecisionType.Half">
            <TooltipTemplate>
                <div class="custom-tooltip">
                    <strong>Rating: @context.ToString("F1")</strong>
                    <small>@GetRatingDescription(context)</small>
                </div>
            </TooltipTemplate>
        </SfRating>
    </div>
    
    <div class="tooltip-example">
        <h4>With Icon</h4>
        <SfRating @bind-Value="@rating3" ShowTooltip="true">
            <TooltipTemplate>
                <span>@GetRatingIcon(context) @context / 5</span>
            </TooltipTemplate>
        </SfRating>
    </div>
</div>

@code {
    private double rating1 = 4;
    private double rating2 = 4.5;
    private double rating3 = 3;
    
    private string GetRatingDescription(double rating)
    {
        return rating switch
        {
            5 => "Perfect!",
            4 => "Very Good",
            3 => "Good",
            2 => "Fair",
            1 => "Poor",
            _ => ""
        };
    }
    
    private string GetRatingIcon(double rating)
    {
        return rating switch
        {
            >= 4 => "😊",
            >= 3 => "🙂",
            >= 2 => "😐",
            _ => "😞"
        };
    }
}

<style>
    .tooltip-example {
        margin: 30px 0;
        padding: 20px;
        background: #f8f9fa;
        border-radius: 8px;
    }
    
    .tooltip-example h4 {
        margin: 0 0 15px;
        color: #333;
    }
    
    .custom-tooltip {
        display: flex;
        flex-direction: column;
        gap: 5px;
    }
    
    .custom-tooltip strong {
        color: #333;
    }
    
    .custom-tooltip small {
        color: #666;
        font-size: 12px;
    }
</style>
```

### Product Review with Detailed Tooltips

```razor
@page "/product-tooltips"
@using Syncfusion.Blazor.Inputs

<div class="product-review">
    <h3>Product Review with Tooltips</h3>
    <p class="instruction">Hover over stars to see criteria</p>
    
    <div class="rating-row">
        <label>Quality</label>
        <SfRating @bind-Value="@quality" ShowTooltip="true">
            <TooltipTemplate>
                <div class="review-tooltip">
                    <strong>@GetQualityLevel(context)</strong>
                    <p>@GetQualityDescription(context)</p>
                </div>
            </TooltipTemplate>
        </SfRating>
    </div>
    
    <div class="rating-row">
        <label>Value</label>
        <SfRating @bind-Value="@value" ShowTooltip="true">
            <TooltipTemplate>
                <div class="review-tooltip">
                    <strong>@GetValueLevel(context)</strong>
                    <p>@GetValueDescription(context)</p>
                </div>
            </TooltipTemplate>
        </SfRating>
    </div>
</div>

@code {
    private double quality = 4;
    private double value = 5;
    
    private string GetQualityLevel(double rating)
    {
        return rating switch
        {
            5 => "Exceptional Quality",
            4 => "High Quality",
            3 => "Good Quality",
            2 => "Fair Quality",
            1 => "Poor Quality",
            _ => ""
        };
    }
    
    private string GetQualityDescription(double rating)
    {
        return rating switch
        {
            5 => "Outstanding craftsmanship and materials",
            4 => "Well-made with minor imperfections",
            3 => "Acceptable build with some issues",
            2 => "Below average construction",
            1 => "Significant quality concerns",
            _ => ""
        };
    }
    
    private string GetValueLevel(double rating)
    {
        return rating switch
        {
            5 => "Excellent Value",
            4 => "Good Value",
            3 => "Fair Value",
            2 => "Overpriced",
            1 => "Poor Value",
            _ => ""
        };
    }
    
    private string GetValueDescription(double rating)
    {
        return rating switch
        {
            5 => "Worth every penny, highly recommend",
            4 => "Good price for the quality",
            3 => "Price matches quality",
            2 => "Could be cheaper for what you get",
            1 => "Not worth the cost",
            _ => ""
        };
    }
}

<style>
    .instruction {
        color: #666;
        margin-bottom: 20px;
        font-size: 14px;
    }
    
    .rating-row {
        display: flex;
        align-items: center;
        gap: 20px;
        margin: 20px 0;
        padding: 15px;
        background: #f8f9fa;
        border-radius: 6px;
    }
    
    .rating-row label {
        min-width: 100px;
        font-weight: 600;
    }
    
    .review-tooltip {
        max-width: 250px;
    }
    
    .review-tooltip strong {
        display: block;
        margin-bottom: 8px;
        color: #333;
    }
    
    .review-tooltip p {
        margin: 0;
        font-size: 13px;
        color: #666;
        line-height: 1.4;
    }
</style>
```

---

## Dynamic Content Examples

### Real-Time Label Updates

```razor
@page "/dynamic-content"
@using Syncfusion.Blazor.Inputs

<div class="dynamic-content">
    <h3>Dynamic Label and Tooltip</h3>
    
    <SfRating @bind-Value="@userRating" 
              ShowLabel="true"
              ShowTooltip="true"
              LabelPosition="LabelPosition.Bottom"
              Precision="PrecisionType.Half">
        <LabelTemplate>
            <div class="dynamic-label" style="color: @GetRatingColor(context)">
                <span class="rating-value">@context.ToString("F1")</span>
                <span class="rating-description">@GetRatingText(context)</span>
                <span class="rating-emoji">@GetRatingEmoji(context)</span>
            </div>
        </LabelTemplate>
        <TooltipTemplate>
            <div class="dynamic-tooltip">
                <strong>@context / 5.0</strong>
                <p>@GetDetailedFeedback(context)</p>
            </div>
        </TooltipTemplate>
    </SfRating>
</div>

@code {
    private double userRating = 4;
    
    private string GetRatingColor(double rating)
    {
        return rating switch
        {
            >= 4.5 => "#28a745",
            >= 3.5 => "#17a2b8",
            >= 2.5 => "#ffc107",
            >= 1.5 => "#fd7e14",
            _ => "#dc3545"
        };
    }
    
    private string GetRatingText(double rating)
    {
        return rating switch
        {
            >= 4.5 => "Outstanding",
            >= 4.0 => "Excellent",
            >= 3.5 => "Very Good",
            >= 3.0 => "Good",
            >= 2.5 => "Average",
            >= 2.0 => "Below Average",
            >= 1.0 => "Poor",
            _ => "Not Rated"
        };
    }
    
    private string GetRatingEmoji(double rating)
    {
        return rating switch
        {
            >= 4.5 => "🤩",
            >= 3.5 => "😊",
            >= 2.5 => "🙂",
            >= 1.5 => "😐",
            >= 0.5 => "😞",
            _ => ""
        };
    }
    
    private string GetDetailedFeedback(double rating)
    {
        return rating switch
        {
            5.0 => "Perfect score! This is as good as it gets.",
            >= 4.5 => "Nearly perfect! Just minor improvements possible.",
            >= 4.0 => "Excellent performance with room for minor enhancements.",
            >= 3.5 => "Very good quality with some areas to improve.",
            >= 3.0 => "Good overall, meets expectations.",
            >= 2.5 => "Average quality, meets basic requirements.",
            >= 2.0 => "Below expectations, needs improvement.",
            >= 1.0 => "Poor quality, significant issues present.",
            _ => "No rating provided yet."
        };
    }
}

<style>
    .dynamic-label {
        display: flex;
        flex-direction: column;
        align-items: center;
        gap: 5px;
        margin-top: 15px;
    }
    
    .rating-value {
        font-size: 28px;
        font-weight: 700;
    }
    
    .rating-description {
        font-size: 16px;
        font-weight: 600;
    }
    
    .rating-emoji {
        font-size: 32px;
        margin-top: 5px;
    }
    
    .dynamic-tooltip {
        max-width: 250px;
    }
    
    .dynamic-tooltip strong {
        display: block;
        margin-bottom: 10px;
        font-size: 16px;
        color: #333;
    }
    
    .dynamic-tooltip p {
        margin: 0;
        font-size: 13px;
        color: #666;
        line-height: 1.5;
    }
</style>
```

---

## Best Practices

### 1. Label Visibility

**Use labels when:**
- Displaying aggregate ratings (4.5 / 5.0)
- Showing calculated averages
- Users need exact numeric value
- Space permits

**Omit labels when:**
- Minimalist design required
- Stars are self-explanatory
- Space is limited (mobile)

### 2. Tooltip Usage

**Use tooltips for:**
- Explaining rating criteria
- Providing additional context
- Multi-criteria ratings
- First-time user guidance

**Avoid tooltips when:**
- Rating is self-evident
- Mobile-only interface (hover doesn't work well)
- Tooltips add no value

### 3. Custom Templates

**Good practices:**
- Keep templates concise
- Maintain readability
- Use consistent styling
- Test on mobile devices
- Consider accessibility

### 4. Performance

- Avoid heavy computation in template rendering
- Cache formatted strings when possible
- Use simple conditional logic
- Test with many rating components on one page

This comprehensive guide covers all label and tooltip configuration options for the Syncfusion Blazor Rating component.
