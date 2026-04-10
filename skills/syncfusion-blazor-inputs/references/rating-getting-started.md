# Rating - Getting Started

## Overview

The Syncfusion Blazor Rating component provides an intuitive star rating interface for collecting user feedback, reviews, and assessments. It supports multiple precision modes, custom templates, labels, tooltips, and comprehensive event handling for seamless integration into Blazor applications.

**Key Features:**
- Configurable item count (stars, hearts, etc.)
- Multiple precision modes: Full, Half, Quarter, Exact
- Custom templates for selected and unselected states
- Labels and tooltips for additional context
- Keyboard navigation and accessibility support
- Two-way data binding with Value property
- Animation effects for smooth interactions

**Common Use Cases:**
- Product reviews and ratings
- Feedback forms and surveys
- Skill level assessment
- Movie/book ratings
- Service quality ratings
- User experience feedback

---

## Installation and NuGet Package Setup

### Step 1: Install Syncfusion.Blazor.Inputs Package

**Via .NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.Inputs
```

**Via NuGet Package Manager:**
```powershell
Install-Package Syncfusion.Blazor.Inputs
```

**Via Visual Studio:**
1. Right-click project → Manage NuGet Packages
2. Search for "Syncfusion.Blazor.Inputs"
3. Click Install

### Step 2: Register Syncfusion Blazor Service

**For Blazor Server / Blazor Web App (.NET 6+):**

In `Program.cs`:
```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);

// Add Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

// Rest of configuration...
var app = builder.Build();
```

**For Blazor WebAssembly:**

In `Program.cs`:
```csharp
using Syncfusion.Blazor;

var builder = WebAssemblyHostBuilder.CreateDefault(args);

// Add Syncfusion Blazor service
builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

---

## CSS Theme Configuration

### Step 3: Import Syncfusion CSS

**Option 1: Via CDN (Recommended for quick start)**

Add to `wwwroot/index.html` (Blazor WebAssembly) or `Pages/_Host.cshtml` / `Pages/_Layout.cshtml` (Blazor Server):

```html
<head>
    <!-- Syncfusion Blazor Theme -->
    <link href="https://cdn.syncfusion.com/blazor/24.1.41/styles/bootstrap5.css" rel="stylesheet" />
</head>
```

**Available Themes:**
- `bootstrap5.css` - Bootstrap 5 theme
- `material.css` - Material Design
- `fabric.css` - Microsoft Fabric
- `tailwind.css` - Tailwind CSS
- `fluent.css` - Microsoft Fluent
- `bootstrap5-dark.css` - Bootstrap 5 Dark

**Option 2: Via Static Assets**

```html
<head>
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
```

**Option 3: Individual Component CSS (Smaller Bundle)**

```html
<head>
    <link href="_content/Syncfusion.Blazor.Inputs/styles/bootstrap5.css" rel="stylesheet" />
</head>
```

---

## Namespace Imports

### Step 4: Import Required Namespace

**Option A: Per-Page Import**

Add at the top of your `.razor` file:
```razor
@using Syncfusion.Blazor.Inputs
```

**Option B: Global Import (Recommended)**

Add to `_Imports.razor`:
```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Inputs
```

---

## Basic SfRating Component Setup

### Minimal Example

The simplest rating with default 5-star configuration:

```razor
@page "/rating-basic"
@using Syncfusion.Blazor.Inputs

<h3>Rate Your Experience</h3>

<SfRating @bind-Value="@ratingValue"></SfRating>

<p>Your rating: @ratingValue out of 5</p>

@code {
    private double ratingValue = 0;
}
```

**What this does:**
- Creates 5 star rating items (default)
- Binds value to `ratingValue` property
- User can click to select rating
- Displays selected rating value

---

## ItemsCount Configuration

The `ItemsCount` property determines how many rating items (stars) are displayed.

### Default 5-Star Rating

```razor
<SfRating @bind-Value="@rating"></SfRating>

@code {
    private double rating = 0; // Default: 5 items
}
```

### Custom Item Counts

**3-Star Rating (Simple satisfaction):**
```razor
<SfRating @bind-Value="@satisfaction" ItemsCount="3"></SfRating>

@code {
    private double satisfaction = 0;
}
```

**10-Star Rating (Detailed feedback):**
```razor
<SfRating @bind-Value="@detailed" ItemsCount="10"></SfRating>

@code {
    private double detailed = 0;
}
```

### ItemsCount Examples

```razor
@page "/rating-counts"
@using Syncfusion.Blazor.Inputs

<div class="rating-examples">
    <div class="rating-section">
        <label>3-Star Rating (Simple)</label>
        <SfRating @bind-Value="@rating3" ItemsCount="3"></SfRating>
        <p>Value: @rating3</p>
    </div>
    
    <div class="rating-section">
        <label>5-Star Rating (Standard)</label>
        <SfRating @bind-Value="@rating5" ItemsCount="5"></SfRating>
        <p>Value: @rating5</p>
    </div>
    
    <div class="rating-section">
        <label>7-Star Rating (Extended)</label>
        <SfRating @bind-Value="@rating7" ItemsCount="7"></SfRating>
        <p>Value: @rating7</p>
    </div>
    
    <div class="rating-section">
        <label>10-Star Rating (Detailed)</label>
        <SfRating @bind-Value="@rating10" ItemsCount="10"></SfRating>
        <p>Value: @rating10</p>
    </div>
</div>

@code {
    private double rating3 = 0;
    private double rating5 = 0;
    private double rating7 = 0;
    private double rating10 = 0;
}

<style>
    .rating-section {
        margin: 20px 0;
        padding: 15px;
        background: #f8f9fa;
        border-radius: 6px;
    }
    
    .rating-section label {
        display: block;
        margin-bottom: 10px;
        font-weight: 600;
    }
    
    .rating-section p {
        margin-top: 10px;
        color: #666;
    }
</style>
```

---

## Value Binding Basics

### Two-Way Binding with @bind-Value

The Rating component supports two-way data binding using the `@bind-Value` directive.

```razor
<SfRating @bind-Value="@userRating"></SfRating>

@code {
    private double userRating = 0;
}
```

**How it works:**
- User clicks stars → `userRating` updates automatically
- You can programmatically set `userRating` → UI updates automatically
- Value ranges from 0 to ItemsCount

### Pre-Setting Rating Values

```razor
@page "/rating-preset"
@using Syncfusion.Blazor.Inputs

<h3>Product Rating: @productRating stars</h3>

<SfRating @bind-Value="@productRating" ItemsCount="5"></SfRating>

<div class="button-group">
    <button @onclick="SetExcellent">Set Excellent (5★)</button>
    <button @onclick="SetGood">Set Good (4★)</button>
    <button @onclick="SetAverage">Set Average (3★)</button>
    <button @onclick="SetPoor">Set Poor (2★)</button>
    <button @onclick="ClearRating">Clear</button>
</div>

@code {
    private double productRating = 4; // Pre-set to 4 stars
    
    private void SetExcellent() => productRating = 5;
    private void SetGood() => productRating = 4;
    private void SetAverage() => productRating = 3;
    private void SetPoor() => productRating = 2;
    private void ClearRating() => productRating = 0;
}

<style>
    .button-group {
        margin-top: 20px;
        display: flex;
        gap: 10px;
        flex-wrap: wrap;
    }
    
    .button-group button {
        padding: 8px 16px;
        background: #f0f0f0;
        border: 1px solid #ccc;
        border-radius: 4px;
        cursor: pointer;
    }
    
    .button-group button:hover {
        background: #e0e0e0;
    }
</style>
```

---

## Minimal Working Example

Here's a complete, copy-paste-ready example for a product review system:

```razor
@page "/product-review"
@using Syncfusion.Blazor.Inputs

<div class="review-container">
    <h2>Product Review</h2>
    
    <div class="product-info">
        <h3>Wireless Bluetooth Headphones</h3>
        <p>Share your experience with this product</p>
    </div>
    
    <div class="rating-section">
        <label>Overall Rating</label>
        <SfRating @bind-Value="@overallRating" ItemsCount="5"></SfRating>
        <span class="rating-text">
            @if (overallRating > 0)
            {
                <text>@overallRating out of 5 stars</text>
            }
            else
            {
                <text>Click to rate</text>
            }
        </span>
    </div>
    
    <div class="rating-section">
        <label>Sound Quality</label>
        <SfRating @bind-Value="@soundQuality" ItemsCount="5"></SfRating>
        <span class="rating-text">@soundQuality / 5</span>
    </div>
    
    <div class="rating-section">
        <label>Comfort</label>
        <SfRating @bind-Value="@comfort" ItemsCount="5"></SfRating>
        <span class="rating-text">@comfort / 5</span>
    </div>
    
    <div class="rating-section">
        <label>Battery Life</label>
        <SfRating @bind-Value="@batteryLife" ItemsCount="5"></SfRating>
        <span class="rating-text">@batteryLife / 5</span>
    </div>
    
    <button class="submit-button" 
            @onclick="SubmitReview"
            disabled="@(overallRating == 0)">
        Submit Review
    </button>
    
    @if (showConfirmation)
    {
        <div class="confirmation">
            ✓ Thank you for your review!
        </div>
    }
</div>

@code {
    private double overallRating = 0;
    private double soundQuality = 0;
    private double comfort = 0;
    private double batteryLife = 0;
    private bool showConfirmation = false;
    
    private async Task SubmitReview()
    {
        // Submit review to server
        await Task.Delay(500);
        
        showConfirmation = true;
        
        // Auto-hide after 3 seconds
        await Task.Delay(3000);
        showConfirmation = false;
    }
}

<style>
    .review-container {
        max-width: 600px;
        margin: 40px auto;
        padding: 30px;
        background: white;
        border-radius: 10px;
        box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    }
    
    .product-info {
        margin-bottom: 30px;
        padding-bottom: 20px;
        border-bottom: 1px solid #e0e0e0;
    }
    
    .product-info h3 {
        margin: 0 0 10px;
        color: #333;
    }
    
    .product-info p {
        margin: 0;
        color: #666;
    }
    
    .rating-section {
        margin: 25px 0;
        display: flex;
        align-items: center;
        gap: 15px;
    }
    
    .rating-section label {
        min-width: 120px;
        font-weight: 600;
        color: #333;
    }
    
    .rating-text {
        color: #666;
        font-size: 14px;
        margin-left: 10px;
    }
    
    .submit-button {
        width: 100%;
        padding: 14px;
        margin-top: 25px;
        background: #0d6efd;
        color: white;
        border: none;
        border-radius: 6px;
        font-size: 16px;
        font-weight: 600;
        cursor: pointer;
        transition: background 0.3s;
    }
    
    .submit-button:hover:not(:disabled) {
        background: #0b5ed7;
    }
    
    .submit-button:disabled {
        background: #ccc;
        cursor: not-allowed;
    }
    
    .confirmation {
        margin-top: 20px;
        padding: 15px;
        background: #d1e7dd;
        color: #0f5132;
        border-radius: 6px;
        text-align: center;
        font-weight: 600;
        animation: fadeIn 0.3s;
    }
    
    @keyframes fadeIn {
        from { opacity: 0; transform: translateY(-10px); }
        to { opacity: 1; transform: translateY(0); }
    }
</style>
```

---

## Next Steps

Once you have the basic Rating working:

1. **Precision Modes** → Read [rating-precision-and-values.md](rating-precision-and-values.md) to learn about Full, Half, Quarter, and Exact precision modes for more granular ratings.

2. **Labels and Tooltips** → Read [rating-labels-and-tooltips.md](rating-labels-and-tooltips.md) to add contextual information with labels and hover tooltips.

3. **Customization** → Read [rating-templates-customization.md](rating-templates-customization.md) to use custom icons (hearts, thumbs) and styling.

4. **Events** → Read [rating-events-and-states.md](rating-events-and-states.md) to handle ValueChanged, OnItemHover events, and manage ReadOnly/Disabled states.

---

## Quick Troubleshooting

**Issue: Rating component not visible**
- ✓ Ensure CSS theme is imported in `_Host.cshtml` or `index.html`
- ✓ Verify `AddSyncfusionBlazor()` is called in `Program.cs`
- ✓ Check browser console for CSS loading errors

**Issue: Two-way binding not working**
- ✓ Use `@bind-Value` (not just `Value`)
- ✓ Ensure variable is declared in `@code` block as `double` type
- ✓ Check for typos in variable names

**Issue: Cannot click to rate**
- ✓ Remove `Disabled="true"` if present
- ✓ Remove `ReadOnly="true"` if present
- ✓ Ensure component is not inside a disabled form

**Issue: Value not changing**
- ✓ Check if `AllowReset="false"` is preventing re-selection
- ✓ Verify no JavaScript conflicts
- ✓ Test in a clean page without other components
