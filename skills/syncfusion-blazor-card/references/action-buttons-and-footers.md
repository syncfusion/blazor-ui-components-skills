# Action Buttons and Footers

## Table of Contents

- [Overview](#overview)
- [CardFooter Component](#cardfooter-component)
- [Adding Buttons](#adding-buttons)
- [Button Styling](#button-styling)
- [Vertical Button Orientation](#vertical-button-orientation)
- [Multiple Buttons](#multiple-buttons)
- [Button Events](#button-events)
- [Interactive Patterns](#interactive-patterns)
- [Troubleshooting](#troubleshooting)

## Overview

Card footers are used to display action buttons and interactive elements at the bottom of cards. The `CardFooter` and `CardFooterContent` components provide the container for buttons. Buttons can be styled, oriented vertically or horizontally, and respond to user clicks with event handlers.

## CardFooter Component

### Basic CardFooter Structure

The `CardFooter` component creates a footer section at the bottom of the card:

```razor
@using Syncfusion.Blazor.Cards

<SfCard>
    <CardHeader Title="Product"></CardHeader>
    <CardContent Content="Description"></CardContent>
    <CardFooter>
        <CardFooterContent>
            <!-- Buttons go here -->
        </CardFooterContent>
    </CardFooter>
</SfCard>
```

### CardFooter with Simple Content

Use CardFooter for text or HTML content without buttons:

```razor
<SfCard>
    <CardHeader Title="Article"></CardHeader>
    <CardContent Content="Article content here."></CardContent>
    <CardFooter>
        <CardFooterContent>
            <small>Published on March 15, 2026</small>
        </CardFooterContent>
    </CardFooter>
</SfCard>
```

## Adding Buttons

### Single Action Button

Add a single button to the card footer:

```razor
@using Syncfusion.Blazor.Cards
@using Syncfusion.Blazor.Buttons

<SfCard>
    <CardHeader Title="Call to Action"></CardHeader>
    <CardContent Content="Click the button below to proceed."></CardContent>
    <CardFooter>
        <CardFooterContent>
            <SfButton CssClass="e-btn e-primary">CLICK ME</SfButton>
        </CardFooterContent>
    </CardFooter>
</SfCard>
```

### Multiple Buttons

Add multiple buttons in the footer:

```razor
<SfCard>
    <CardHeader Title="Decision"></CardHeader>
    <CardContent Content="Make your choice:"></CardContent>
    <CardFooter>
        <CardFooterContent>
            <SfButton CssClass="e-btn e-primary">ACCEPT</SfButton>
            <SfButton CssClass="e-btn e-outline">DECLINE</SfButton>
            <SfButton CssClass="e-btn e-outline">MAYBE</SfButton>
        </CardFooterContent>
    </CardFooter>
</SfCard>
```

### Button Spacing

Control spacing between buttons with CSS:

```css
.e-card .e-card-actions .e-card-btn {
    margin-right: 10px;
}

.e-card .e-card-actions .e-card-btn:last-child {
    margin-right: 0;
}
```

## Button Styling

### Button Types

Syncfusion buttons support different CSS classes for styling:

```razor
<SfCard>
    <CardContent Content="Button Styles:"></CardContent>
    <CardFooter>
        <CardFooterContent>
            <SfButton CssClass="e-btn e-primary">Primary</SfButton>
            <SfButton CssClass="e-btn e-success">Success</SfButton>
            <SfButton CssClass="e-btn e-info">Info</SfButton>
            <SfButton CssClass="e-btn e-warning">Warning</SfButton>
            <SfButton CssClass="e-btn e-danger">Danger</SfButton>
            <SfButton CssClass="e-btn e-outline">Outline</SfButton>
        </CardFooterContent>
    </CardFooter>
</SfCard>
```

### Button Sizes

Create buttons of different sizes:

```razor
<SfCard>
    <CardContent Content="Button Sizes:"></CardContent>
    <CardFooter>
        <CardFooterContent>
            <SfButton CssClass="e-btn e-small e-primary">Small</SfButton>
            <SfButton CssClass="e-btn e-primary">Normal</SfButton>
            <SfButton CssClass="e-btn e-large e-primary">Large</SfButton>
        </CardFooterContent>
    </CardFooter>
</SfCard>
```

### Custom Button Styling

Apply custom CSS to buttons:

```css
.card-action-btn {
    padding: 12px 24px;
    font-weight: 600;
    border-radius: 4px;
    transition: all 0.3s ease;
}

.card-action-btn:hover {
    transform: translateY(-2px);
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
}
```

Use in button:
```razor
<SfButton CssClass="e-btn e-primary card-action-btn">Custom Style</SfButton>
```

## Vertical Button Orientation

### Card with Vertical Orientation

Set the Card's `Orientation` property to display buttons vertically:

```razor
<SfCard Orientation="CardOrientation.Vertical">
    <CardHeader Title="Vertical Layout"></CardHeader>
    <CardContent Content="Content with vertical buttons."></CardContent>
    <CardFooter>
        <CardFooterContent>
            <SfButton CssClass="e-btn e-primary">BUTTON 1</SfButton>
            <SfButton CssClass="e-btn e-outline">BUTTON 2</SfButton>
        </CardFooterContent>
    </CardFooter>
</SfCard>
```

### CSS for Vertical Button Alignment

Force vertical alignment with CSS flexbox:

```css
.vertical-button-group {
    display: flex;
    flex-direction: column;
    gap: 10px;
}

.vertical-button-group .e-btn {
    width: 100%;
}
```

Use in component:
```razor
<CardFooter>
    <CardFooterContent>
        <div class="vertical-button-group">
            <SfButton CssClass="e-btn e-primary">BUTTON 1</SfButton>
            <SfButton CssClass="e-btn e-outline">BUTTON 2</SfButton>
        </div>
    </CardFooterContent>
</CardFooter>
```

## Multiple Buttons

### Button Groups

Organize buttons logically:

```razor
@using Syncfusion.Blazor.Cards
@using Syncfusion.Blazor.Buttons

<SfCard>
    <CardHeader Title="Product Actions"></CardHeader>
    <CardContent Content="Select an action:"></CardContent>
    <CardFooter>
        <CardFooterContent>
            <!-- Primary Action -->
            <SfButton CssClass="e-btn e-primary">BUY NOW</SfButton>
            
            <!-- Secondary Actions -->
            <SfButton CssClass="e-btn e-outline">WISHLIST</SfButton>
            <SfButton CssClass="e-btn e-outline">SHARE</SfButton>
        </CardFooterContent>
    </CardFooter>
</SfCard>
```

### Justified Buttons

Make buttons equal width across the footer:

```css
.button-group-justified {
    display: grid;
    grid-template-columns: 1fr 1fr;
    gap: 10px;
}

.button-group-justified .e-btn {
    width: 100%;
}
```

## Button Events

### Click Event Handling

Handle button clicks with Blazor event handlers:

```razor
@using Syncfusion.Blazor.Cards
@using Syncfusion.Blazor.Buttons

@page "/card-events"

<SfCard>
    <CardHeader Title="Interactive Card"></CardHeader>
    <CardContent Content="Click a button to see the result."></CardContent>
    <CardFooter>
        <CardFooterContent>
            <SfButton CssClass="e-btn e-primary" OnClick="HandlePrimaryClick">CONFIRM</SfButton>
            <SfButton CssClass="e-btn e-outline" OnClick="HandleSecondaryClick">CANCEL</SfButton>
        </CardFooterContent>
    </CardFooter>
</SfCard>

<p>@Message</p>

@code {
    private string Message = "";

    private void HandlePrimaryClick()
    {
        Message = "You confirmed the action!";
    }

    private void HandleSecondaryClick()
    {
        Message = "You cancelled the action.";
    }
}
```

### Event with Parameters

Pass data with button clicks:

```razor
@using Syncfusion.Blazor.Cards
@using Syncfusion.Blazor.Buttons

@page "/card-with-data"

<SfCard>
    <CardHeader Title="Product @ProductId"></CardHeader>
    <CardContent Content="Product details here."></CardContent>
    <CardFooter>
        <CardFooterContent>
            <SfButton CssClass="e-btn e-primary" OnClick="() => HandleAction('BUY', ProductId)">BUY</SfButton>
            <SfButton CssClass="e-btn e-outline" OnClick="() => HandleAction('DETAILS', ProductId)">DETAILS</SfButton>
        </CardFooterContent>
    </CardFooter>
</SfCard>

@code {
    private int ProductId = 123;

    private void HandleAction(string action, int id)
    {
        Console.WriteLine($"{action} - Product {id}");
    }
}
```

## Interactive Patterns

### Pattern 1: E-commerce Card

Product card with buy and wishlist buttons:

```razor
@using Syncfusion.Blazor.Cards
@using Syncfusion.Blazor.Buttons

<SfCard>
    <CardImage Image="images/product.jpg" />
    <CardHeader Title="Premium Headphones" SubTitle="$199.99"></CardHeader>
    <CardContent EnableSeparator="true">
        High-quality audio with noise cancellation
    </CardContent>
    <CardContent EnableSeparator="true">
        <strong>In Stock:</strong> 15 units available
    </CardContent>
    <CardFooter>
        <CardFooterContent>
            <SfButton CssClass="e-btn e-primary" OnClick="AddToCart">ADD TO CART</SfButton>
            <SfButton CssClass="e-btn e-outline" OnClick="AddToWishlist">❤️ WISHLIST</SfButton>
        </CardFooterContent>
    </CardFooter>
</SfCard>

@code {
    private void AddToCart()
    {
        // Handle add to cart
    }

    private void AddToWishlist()
    {
        // Handle wishlist
    }
}
```

### Pattern 2: Confirmation Card

Request user confirmation with Yes/No buttons:

```razor
<SfCard>
    <CardHeader Title="Confirm Action"></CardHeader>
    <CardContent Content="Are you sure you want to delete this item? This action cannot be undone."></CardContent>
    <CardFooter>
        <CardFooterContent>
            <SfButton CssClass="e-btn e-danger" OnClick="ConfirmDelete">DELETE</SfButton>
            <SfButton CssClass="e-btn e-outline" OnClick="Cancel">CANCEL</SfButton>
        </CardFooterContent>
    </CardFooter>
</SfCard>
```

### Pattern 3: Call-to-Action Card

Single prominent button with secondary link:

```razor
<SfCard>
    <CardHeader Title="Limited Time Offer"></CardHeader>
    <CardContent Content="Get 50% off on all items for the next 24 hours!"></CardContent>
    <CardFooter>
        <CardFooterContent>
            <SfButton CssClass="e-btn e-primary" Style="width: 100%; padding: 12px;">SHOP NOW</SfButton>
        </CardFooterContent>
    </CardFooter>
</SfCard>
```

## Best Practices

### Do's
- ✓ Use clear, action-oriented button text (CONFIRM, DELETE, SAVE)
- ✓ Highlight primary actions with primary button color
- ✓ Group related actions together
- ✓ Provide feedback after button clicks
- ✓ Use disabled state for unavailable actions

### Don'ts
- ✗ Don't use too many buttons per card (max 3-4)
- ✗ Don't make button text too long
- ✗ Don't use confusing button labels
- ✗ Don't forget event handlers
- ✗ Don't mix too many button styles

## Troubleshooting

### Buttons Not Appearing
- Verify SfButton component is imported
- Check that CardFooter wraps CardFooterContent
- Ensure buttons are inside CardFooterContent

### Click Events Not Firing
- Verify OnClick handler is properly bound
- Check component is interactive (not static render mode)
- Ensure handler method signature is correct

### Button Styling Not Applied
- Verify CssClass attribute is set correctly
- Check CSS classes exist (e-btn, e-primary, etc.)
- Clear browser cache and rebuild

### Buttons Overflow
- Use CSS Grid or Flexbox for layout
- Set width explicitly if needed
- Add media queries for responsive button arrangement
