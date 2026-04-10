# Rating - Events and States

## Table of Contents
- [Events Overview](#events-overview)
- [ValueChanged Event Callback](#valuechanged-event-callback)
- [OnItemHover Event](#onitemnhover-event)
- [RatingHoverEventArgs Structure](#ratinghovereventargs-structure)
- [Created Lifecycle Event](#created-lifecycle-event)
- [Form Validation Integration](#form-validation-integration)
- [ReadOnly State for Display](#readonly-state-for-display)
- [Disabled State Management](#disabled-state-management)
- [Visible Property](#visible-property)
- [Keyboard Navigation](#keyboard-navigation)
- [Accessibility Features](#accessibility-features)
- [Event-Driven Patterns](#event-driven-patterns)

---

## Events Overview

The Syncfusion Blazor Rating component provides several events for tracking user interactions and component lifecycle.

### Available Events

| Event | Type | Description | When Fired |
|-------|------|-------------|------------|
| **ValueChanged** | EventCallback<double> | Fires when rating value changes | On click/keyboard selection |
| **OnItemHover** | EventCallback<RatingHoverEventArgs> | Fires when hovering over items | On mouse hover |
| **Created** | EventCallback<object> | Fires after component initialization | On component creation |

---

## ValueChanged Event Callback

The `ValueChanged` event fires whenever the rating value changes through user interaction or programmatic updates.

### Basic ValueChanged Handler

```razor
<SfRating Value="@rating" 
          ValueChanged="OnRatingChanged">
</SfRating>

@code {
    private double rating = 0;
    
    private void OnRatingChanged(double newValue)
    {
        rating = newValue;
        // Additional logic here
        Console.WriteLine($"Rating changed to: {newValue}");
    }
}
```

**Important:** When using `ValueChanged`, you must manually update the bound value (cannot use `@bind-Value`).

### Real-Time Feedback Example

```razor
@page "/rating-feedback"
@using Syncfusion.Blazor.Inputs

<div class="feedback-demo">
    <h3>Rate Your Experience</h3>
    
    <SfRating Value="@userRating" 
              ValueChanged="HandleRatingChange"
              ItemsCount="5">
    </SfRating>
    
    @if (!string.IsNullOrEmpty(feedbackMessage))
    {
        <div class="feedback-message @feedbackClass">
            @feedbackMessage
        </div>
    }
</div>

@code {
    private double userRating = 0;
    private string feedbackMessage = "";
    private string feedbackClass = "";
    
    private void HandleRatingChange(double newValue)
    {
        userRating = newValue;
        
        // Provide immediate feedback
        (feedbackMessage, feedbackClass) = newValue switch
        {
            5 => ("🤩 Excellent! Thank you for the 5-star rating!", "excellent"),
            4 => ("😊 Great! We appreciate your positive feedback.", "great"),
            3 => ("🙂 Good! We'll keep improving.", "good"),
            2 => ("😐 Thanks for your feedback. We'll work on improvements.", "fair"),
            1 => ("😞 We're sorry to hear that. Please let us know how we can improve.", "poor"),
            _ => ("", "")
        };
        
        StateHasChanged();
    }
}

<style>
    .feedback-message {
        margin-top: 20px;
        padding: 15px;
        border-radius: 8px;
        font-weight: 600;
        animation: fadeIn 0.3s;
    }
    
    .feedback-message.excellent { background: #d1e7dd; color: #0f5132; }
    .feedback-message.great { background: #cfe2ff; color: #084298; }
    .feedback-message.good { background: #e7f3ff; color: #055160; }
    .feedback-message.fair { background: #fff3cd; color: #997404; }
    .feedback-message.poor { background: #f8d7da; color: #842029; }
    
    @keyframes fadeIn {
        from { opacity: 0; transform: translateY(-10px); }
        to { opacity: 1; transform: translateY(0); }
    }
</style>
```

### Auto-Submit on Rating

```razor
@page "/auto-submit-rating"
@using Syncfusion.Blazor.Inputs

<div class="auto-submit">
    <h3>Quick Rating (Auto-Submit)</h3>
    <p class="instruction">Click stars to rate - automatically submits</p>
    
    <SfRating Value="@productRating" 
              ValueChanged="SubmitRating"
              ItemsCount="5"
              Disabled="@isSubmitting">
    </SfRating>
    
    @if (isSubmitting)
    {
        <p class="status">Submitting your rating...</p>
    }
    else if (hasSubmitted)
    {
        <div class="success">
            ✓ Thank you! Your @productRating-star rating has been submitted.
        </div>
    }
</div>

@code {
    private double productRating = 0;
    private bool isSubmitting = false;
    private bool hasSubmitted = false;
    
    private async void SubmitRating(double newValue)
    {
        productRating = newValue;
        isSubmitting = true;
        hasSubmitted = false;
        StateHasChanged();
        
        // Simulate API call
        await Task.Delay(1000);
        
        // Submit to server
        // await Http.PostAsJsonAsync("api/ratings", new { rating = newValue });
        
        isSubmitting = false;
        hasSubmitted = true;
        StateHasChanged();
    }
}

<style>
    .instruction {
        color: #666;
        margin-bottom: 20px;
    }
    
    .status {
        margin-top: 15px;
        color: #0d6efd;
        font-weight: 600;
    }
    
    .success {
        margin-top: 15px;
        padding: 15px;
        background: #d1e7dd;
        color: #0f5132;
        border-radius: 6px;
        font-weight: 600;
    }
</style>
```

---

## OnItemHover Event

The `OnItemHover` event fires when the user hovers over rating items, providing real-time hover feedback.

### Basic OnItemHover Handler

```razor
<SfRating @bind-Value="@rating" 
          OnItemHover="HandleItemHover">
</SfRating>

@code {
    private double rating = 0;
    
    private void HandleItemHover(RatingHoverEventArgs args)
    {
        Console.WriteLine($"Hovering over item with value: {args.Value}");
    }
}
```

### Hover Preview Example

```razor
@page "/hover-preview"
@using Syncfusion.Blazor.Inputs

<div class="hover-preview">
    <h3>Hover to Preview Rating</h3>
    
    <SfRating @bind-Value="@selectedRating" 
              OnItemHover="ShowHoverPreview"
              ItemsCount="5">
    </SfRating>
    
    <div class="preview-box">
        @if (hoverValue > 0)
        {
            <p class="hover-text">
                Preview: @hoverValue stars - @GetRatingLabel(hoverValue)
            </p>
        }
        else if (selectedRating > 0)
        {
            <p class="selected-text">
                Selected: @selectedRating stars
            </p>
        }
        else
        {
            <p class="default-text">
                Hover over stars to preview
            </p>
        }
    </div>
</div>

@code {
    private double selectedRating = 0;
    private double hoverValue = 0;
    
    private void ShowHoverPreview(RatingHoverEventArgs args)
    {
        hoverValue = args.Value;
    }
    
    private string GetRatingLabel(double rating)
    {
        return rating switch
        {
            5 => "Excellent!",
            4 => "Very Good",
            3 => "Good",
            2 => "Fair",
            1 => "Poor",
            _ => ""
        };
    }
}

<style>
    .preview-box {
        margin-top: 20px;
        padding: 15px;
        background: #f8f9fa;
        border-radius: 6px;
        min-height: 60px;
    }
    
    .hover-text {
        color: #0d6efd;
        font-weight: 600;
        margin: 0;
    }
    
    .selected-text {
        color: #28a745;
        font-weight: 600;
        margin: 0;
    }
    
    .default-text {
        color: #666;
        margin: 0;
    }
</style>
```

---

## RatingHoverEventArgs Structure

The `OnItemHover` event provides a `RatingHoverEventArgs` object with hover information.

### RatingHoverEventArgs Properties

| Property | Type | Description |
|----------|------|-------------|
| **Value** | double | Rating value of the hovered item |
| **Element** | ElementReference | Reference to the hovered DOM element |

### Using Event Arguments

```razor
@page "/hover-details"
@using Syncfusion.Blazor.Inputs

<div class="hover-details">
    <h3>Hover Event Details</h3>
    
    <SfRating @bind-Value="@rating" 
              OnItemHover="LogHoverDetails"
              ItemsCount="5">
    </SfRating>
    
    <div class="event-log">
        <h4>Hover Log:</h4>
        @foreach (var log in hoverLogs.TakeLast(5).Reverse())
        {
            <div class="log-entry">@log</div>
        }
    </div>
</div>

@code {
    private double rating = 3;
    private List<string> hoverLogs = new();
    
    private void LogHoverDetails(RatingHoverEventArgs args)
    {
        string timestamp = DateTime.Now.ToString("HH:mm:ss");
        string log = $"[{timestamp}] Hovered: {args.Value} stars";
        hoverLogs.Add(log);
        
        // Keep only last 20 logs
        if (hoverLogs.Count > 20)
            hoverLogs.RemoveAt(0);
        
        StateHasChanged();
    }
}

<style>
    .event-log {
        margin-top: 25px;
        padding: 15px;
        background: #f8f9fa;
        border-radius: 6px;
        font-family: monospace;
        font-size: 13px;
    }
    
    .event-log h4 {
        margin: 0 0 10px;
        font-size: 14px;
        color: #333;
    }
    
    .log-entry {
        padding: 5px 0;
        border-bottom: 1px solid #e0e0e0;
        color: #666;
    }
</style>
```

---

## Created Lifecycle Event

The `Created` event fires once when the Rating component is fully initialized and rendered.

### Basic Created Handler

```razor
<SfRating @bind-Value="@rating" 
          Created="OnComponentCreated">
</SfRating>

@code {
    private double rating = 0;
    
    private void OnComponentCreated(object args)
    {
        Console.WriteLine("Rating component created and ready");
        // Initialization logic here
    }
}
```

### Initialization Example

```razor
@page "/rating-initialization"
@using Syncfusion.Blazor.Inputs

<div class="initialization-demo">
    <h3>Rating Initialization</h3>
    
    @if (isLoading)
    {
        <p>Loading rating component...</p>
    }
    else
    {
        <SfRating @bind-Value="@userRating" 
                  Created="OnRatingCreated"
                  ItemsCount="5">
        </SfRating>
        
        <p class="status">@statusMessage</p>
    }
</div>

@code {
    private double userRating = 0;
    private bool isLoading = true;
    private string statusMessage = "";
    
    private async void OnRatingCreated(object args)
    {
        // Simulate loading user's previous rating from API
        await Task.Delay(500);
        
        userRating = 4; // Loaded from API
        isLoading = false;
        statusMessage = "Your previous rating has been loaded.";
        
        StateHasChanged();
    }
}

<style>
    .status {
        margin-top: 15px;
        color: #28a745;
        font-weight: 600;
    }
</style>
```

---

## Form Validation Integration

### Integration with EditForm

```razor
@page "/rating-form"
@using Syncfusion.Blazor.Inputs
@using System.ComponentModel.DataAnnotations

<EditForm Model="@reviewModel" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    
    <div class="form-group">
        <label>Product Name</label>
        <InputText @bind-Value="reviewModel.ProductName" class="form-control" />
        <ValidationMessage For="@(() => reviewModel.ProductName)" />
    </div>
    
    <div class="form-group">
        <label>Your Rating *</label>
        <SfRating @bind-Value="@reviewModel.Rating" ItemsCount="5"></SfRating>
        <ValidationMessage For="@(() => reviewModel.Rating)" />
    </div>
    
    <div class="form-group">
        <label>Review Comment</label>
        <InputTextArea @bind-Value="reviewModel.Comment" class="form-control" rows="4" />
        <ValidationMessage For="@(() => reviewModel.Comment)" />
    </div>
    
    <button type="submit" class="btn-submit">Submit Review</button>
</EditForm>

@if (hasSubmitted)
{
    <div class="success-message">
        ✓ Thank you! Your review has been submitted.
    </div>
}

@code {
    private ReviewModel reviewModel = new();
    private bool hasSubmitted = false;
    
    private async Task HandleValidSubmit()
    {
        // Submit review to server
        await Task.Delay(1000);
        
        hasSubmitted = true;
        reviewModel = new ReviewModel(); // Reset form
    }
    
    public class ReviewModel
    {
        [Required(ErrorMessage = "Product name is required")]
        public string ProductName { get; set; } = "";
        
        [Required(ErrorMessage = "Please provide a rating")]
        [Range(1, 5, ErrorMessage = "Rating must be between 1 and 5")]
        public double Rating { get; set; } = 0;
        
        [Required(ErrorMessage = "Comment is required")]
        [MinLength(10, ErrorMessage = "Comment must be at least 10 characters")]
        public string Comment { get; set; } = "";
    }
}

<style>
    .form-group {
        margin: 20px 0;
    }
    
    .form-group label {
        display: block;
        margin-bottom: 8px;
        font-weight: 600;
        color: #333;
    }
    
    .form-control {
        width: 100%;
        padding: 10px;
        border: 1px solid #ccc;
        border-radius: 4px;
    }
    
    .btn-submit {
        width: 100%;
        padding: 12px;
        margin-top: 20px;
        background: #0d6efd;
        color: white;
        border: none;
        border-radius: 6px;
        font-size: 16px;
        font-weight: 600;
        cursor: pointer;
    }
    
    .btn-submit:hover {
        background: #0b5ed7;
    }
    
    .success-message {
        margin-top: 20px;
        padding: 15px;
        background: #d1e7dd;
        color: #0f5132;
        border-radius: 6px;
        font-weight: 600;
    }
</style>
```

---

## ReadOnly State for Display

The `ReadOnly` property (default: `false`) displays ratings without allowing user interaction.

### Basic ReadOnly Display

```razor
<SfRating Value="@averageRating" 
          ReadOnly="true"
          Precision="PrecisionType.Exact">
</SfRating>

@code {
    private double averageRating = 4.7;
}
```

### Read-Only Use Cases

```razor
@page "/readonly-ratings"
@using Syncfusion.Blazor.Inputs

<div class="readonly-demo">
    <h3>Product Ratings (Read-Only Display)</h3>
    
    @foreach (var product in products)
    {
        <div class="product-item">
            <div class="product-info">
                <h4>@product.Name</h4>
                <SfRating Value="@product.Rating" 
                          ReadOnly="true"
                          Precision="PrecisionType.Exact"
                          ShowLabel="true"
                          LabelPosition="LabelPosition.Right">
                    <LabelTemplate>
                        <span>@context.ToString("F1") (@product.ReviewCount reviews)</span>
                    </LabelTemplate>
                </SfRating>
            </div>
            <p class="product-price">$@product.Price</p>
        </div>
    }
</div>

@code {
    private List<Product> products = new()
    {
        new Product { Name = "Laptop Pro", Rating = 4.8, ReviewCount = 523, Price = 1299 },
        new Product { Name = "Wireless Mouse", Rating = 4.5, ReviewCount = 312, Price = 49 },
        new Product { Name = "USB-C Hub", Rating = 4.2, ReviewCount = 187, Price = 79 }
    };
    
    public class Product
    {
        public string Name { get; set; }
        public double Rating { get; set; }
        public int ReviewCount { get; set; }
        public decimal Price { get; set; }
    }
}

<style>
    .product-item {
        display: flex;
        justify-content: space-between;
        align-items: center;
        padding: 20px;
        margin: 15px 0;
        background: #f8f9fa;
        border-radius: 8px;
    }
    
    .product-info h4 {
        margin: 0 0 10px;
        color: #333;
    }
    
    .product-price {
        font-size: 24px;
        font-weight: 700;
        color: #28a745;
        margin: 0;
    }
</style>
```

---

## Disabled State Management

The `Disabled` property (default: `false`) completely disables the rating component.

### Basic Disabled State

```razor
<SfRating @bind-Value="@rating" 
          Disabled="true">
</SfRating>

@code {
    private double rating = 3;
}
```

### Conditional Disabling

```razor
@page "/conditional-disabled"
@using Syncfusion.Blazor.Inputs

<div class="disabled-demo">
    <h3>Rating Submission</h3>
    
    <div class="checkbox-group">
        <label>
            <input type="checkbox" @bind="hasAgreed" />
            I agree to the terms and conditions
        </label>
    </div>
    
    <div class="rating-section">
        <label>Rate Your Experience</label>
        <SfRating @bind-Value="@userRating" 
                  Disabled="@(!hasAgreed)"
                  ItemsCount="5">
        </SfRating>
        
        @if (!hasAgreed)
        {
            <p class="warning">Please agree to terms to enable rating</p>
        }
    </div>
    
    <button class="btn-submit" 
            @onclick="SubmitRating"
            disabled="@(!hasAgreed || userRating == 0)">
        Submit Rating
    </button>
</div>

@code {
    private bool hasAgreed = false;
    private double userRating = 0;
    
    private void SubmitRating()
    {
        // Submit rating logic
    }
}

<style>
    .checkbox-group {
        margin: 20px 0;
    }
    
    .rating-section {
        margin: 25px 0;
    }
    
    .rating-section label {
        display: block;
        margin-bottom: 10px;
        font-weight: 600;
    }
    
    .warning {
        margin-top: 10px;
        color: #dc3545;
        font-size: 14px;
    }
    
    .btn-submit {
        padding: 12px 24px;
        background: #0d6efd;
        color: white;
        border: none;
        border-radius: 6px;
        font-weight: 600;
        cursor: pointer;
    }
    
    .btn-submit:disabled {
        background: #ccc;
        cursor: not-allowed;
    }
</style>
```

---

## Visible Property

The `Visible` property (default: `true`) controls component visibility.

### Conditional Visibility

```razor
@page "/conditional-visibility"
@using Syncfusion.Blazor.Inputs

<div class="visibility-demo">
    <h3>Conditional Rating Display</h3>
    
    <div class="toggle-group">
        <label>
            <input type="checkbox" @bind="showRating" />
            Show rating component
        </label>
    </div>
    
    <SfRating @bind-Value="@rating" 
              Visible="@showRating"
              ItemsCount="5">
    </SfRating>
    
    @if (showRating && rating > 0)
    {
        <p>Your rating: @rating / 5</p>
    }
</div>

@code {
    private bool showRating = true;
    private double rating = 0;
}
```

---

## Keyboard Navigation

The Rating component supports full keyboard navigation for accessibility.

### Supported Keyboard Shortcuts

| Key | Action |
|-----|--------|
| **Arrow Right →** | Move to next rating item |
| **Arrow Left ←** | Move to previous rating item |
| **Enter / Space** | Select current rating item |
| **Home** | Move to first rating item |
| **End** | Move to last rating item |
| **Tab** | Move focus to next element |
| **Shift + Tab** | Move focus to previous element |

### Keyboard Navigation Example

```razor
@page "/keyboard-nav"
@using Syncfusion.Blazor.Inputs

<div class="keyboard-demo">
    <h3>Keyboard Navigation Demo</h3>
    <p class="instructions">
        <strong>Try these keyboard shortcuts:</strong><br/>
        ← → : Navigate between stars<br/>
        Enter/Space: Select rating<br/>
        Home/End: Jump to first/last star
    </p>
    
    <SfRating @bind-Value="@rating" ItemsCount="5"></SfRating>
    
    <p class="current-rating">Current Rating: @rating</p>
</div>

@code {
    private double rating = 3;
}

<style>
    .instructions {
        padding: 15px;
        background: #f8f9fa;
        border-left: 4px solid #0d6efd;
        margin: 20px 0;
        line-height: 1.8;
    }
    
    .current-rating {
        margin-top: 20px;
        font-weight: 600;
        color: #333;
    }
</style>
```

---

## Accessibility Features

The Rating component is built with accessibility in mind, following WCAG 2.1 guidelines.

### ARIA Attributes

The component automatically includes:
- `role="slider"` for screen reader compatibility
- `aria-label` describing the rating purpose
- `aria-valuemin`, `aria-valuemax`, `aria-valuenow` for current state
- `aria-disabled` when disabled
- `aria-readonly` when read-only

### Screen Reader Friendly Example

```razor
@page "/accessible-rating"
@using Syncfusion.Blazor.Inputs

<div class="accessible-rating">
    <h3 id="rating-title">Product Quality Rating</h3>
    <p id="rating-description">
        Rate the product quality from 1 to 5 stars. 
        Use arrow keys to navigate and Enter to select.
    </p>
    
    <SfRating @bind-Value="@qualityRating" 
              ItemsCount="5"
              HtmlAttributes="@ariaAttributes">
    </SfRating>
    
    <!-- Live region for screen reader announcements -->
    <div aria-live="polite" aria-atomic="true" class="sr-only">
        @announcement
    </div>
</div>

@code {
    private double qualityRating = 0;
    private string announcement = "";
    
    private Dictionary<string, object> ariaAttributes = new()
    {
        { "aria-labelledby", "rating-title" },
        { "aria-describedby", "rating-description" }
    };
    
    protected override void OnParametersSet()
    {
        if (qualityRating > 0)
        {
            announcement = $"Quality rated {qualityRating} out of 5 stars";
        }
    }
}

<style>
    .sr-only {
        position: absolute;
        left: -10000px;
        width: 1px;
        height: 1px;
        overflow: hidden;
    }
</style>
```

---

## Event-Driven Patterns

### Multi-Category Rating with Events

```razor
@page "/multi-category-rating"
@using Syncfusion.Blazor.Inputs

<div class="multi-category">
    <h3>Complete Product Review</h3>
    
    <div class="category-rating">
        <label>Quality</label>
        <SfRating Value="@quality" ValueChanged="(v) => UpdateRating(ref quality, v, nameof(quality))"></SfRating>
    </div>
    
    <div class="category-rating">
        <label>Value</label>
        <SfRating Value="@value" ValueChanged="(v) => UpdateRating(ref valueRating, v, nameof(valueRating))"></SfRating>
    </div>
    
    <div class="category-rating">
        <label>Service</label>
        <SfRating Value="@service" ValueChanged="(v) => UpdateRating(ref service, v, nameof(service))"></SfRating>
    </div>
    
    <div class="summary">
        <h4>Review Summary</h4>
        <p>Overall Rating: @overallRating.ToString("F1") / 5</p>
        <p>Categories Rated: @ratedCount / 3</p>
        
        @if (isComplete)
        {
            <button class="btn-submit" @onclick="SubmitReview">
                Submit Complete Review
            </button>
        }
        else
        {
            <p class="incomplete">Please rate all categories</p>
        }
    </div>
</div>

@code {
    private double quality = 0;
    private double valueRating = 0;
    private double service = 0;
    
    private int ratedCount => (quality > 0 ? 1 : 0) + (valueRating > 0 ? 1 : 0) + (service > 0 ? 1 : 0);
    private bool isComplete => ratedCount == 3;
    private double overallRating => isComplete ? (quality + valueRating + service) / 3 : 0;
    
    private void UpdateRating(ref double category, double newValue, string categoryName)
    {
        category = newValue;
        Console.WriteLine($"{categoryName} updated to {newValue}");
        StateHasChanged();
    }
    
    private async Task SubmitReview()
    {
        // Submit review to server
        await Task.Delay(1000);
        Console.WriteLine("Review submitted successfully");
    }
}

<style>
    .category-rating {
        display: flex;
        align-items: center;
        gap: 20px;
        margin: 20px 0;
        padding: 15px;
        background: #f8f9fa;
        border-radius: 6px;
    }
    
    .category-rating label {
        min-width: 100px;
        font-weight: 600;
    }
    
    .summary {
        margin-top: 30px;
        padding: 20px;
        background: #e7f3ff;
        border-radius: 8px;
    }
    
    .summary h4 {
        margin: 0 0 15px;
    }
    
    .btn-submit {
        width: 100%;
        padding: 12px;
        margin-top: 15px;
        background: #28a745;
        color: white;
        border: none;
        border-radius: 6px;
        font-weight: 600;
        cursor: pointer;
    }
    
    .incomplete {
        color: #dc3545;
        font-weight: 600;
        margin: 15px 0 0;
    }
</style>
```

This comprehensive guide covers all events, states, and interaction patterns for building robust rating functionality with the Syncfusion Blazor Rating component.
