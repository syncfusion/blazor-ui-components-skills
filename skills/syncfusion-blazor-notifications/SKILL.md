---
name: syncfusion-blazor-notifications
description: Implement Syncfusion Blazor notification components (Toast, Message, Skeleton) for user feedback and loading states. ALWAYS use this when users need toast notifications, popup messages, alert boxes, success/error/warning/info messages, loading skeletons, shimmer effects, content placeholders, or any feedback UI. Trigger immediately when users mention notifications, toasts, alerts, messages, loading states, skeleton screens, shimmer loading, user feedback, status messages, SfToast, SfMessage, SfSkeleton, notification popups, or need to show temporary messages, form validation feedback, or loading placeholders.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Blazor UI Components - Notifications"
---

# Implementing Syncfusion Blazor Notifications

A comprehensive skill for implementing Toast, Message, and Skeleton components from Syncfusion Blazor Notifications. These components provide essential user feedback mechanisms including temporary notifications, inline messages, and content loading states.

## Component Overview

### SfToast
Lightweight, non-blocking notification popups that appear temporarily to provide feedback. Supports positioning, animations, action buttons, and templates.

**Key Features:**
- Configurable positions (top-left, top-center, top-right, bottom-left, bottom-center, bottom-right)
- Show/hide animations with customizable timing
- Auto-dismiss with timeout and progress bar
- Action buttons for user interaction
- Template support for custom content
- Event callbacks (OnOpen, OnClose, OnClick)
- Multiple toast stacking (newest on top/bottom)

### SfMessage
Inline alert component for displaying contextual feedback messages with different severity levels.

**Key Features:**
- Severity types (Normal, Info, Success, Warning, Error) with color-coded styling
- Variants (Text, Outlined, Filled) for different visual styles
- Built-in icons matching severity levels
- Optional close button for dismissible messages
- Content alignment (left, center, right)
- Visibility control and close event handling
- Custom CSS styling support

### SfSkeleton
Loading placeholder component that displays animated shapes to indicate content is loading.

**Key Features:**
- Shapes (Circle, Square, Rectangle, Text) for different content types
- Shimmer effects (Pulse, Wave, Fade, None) for visual feedback
- Configurable dimensions (width, height)
- Accessibility labels for screen readers
- Visibility toggle for showing/hiding
- CSS customization for styling

## Quick Start

### Toast - Basic Example

```razor
@page "/toast-demo"
@using Syncfusion.Blazor.Notifications
@using Syncfusion.Blazor.Buttons

<SfButton @onclick="ShowToast">Show Toast</SfButton>

<SfToast @ref="ToastObj" Title="Notification" Content="Your changes have been saved successfully!" />

@code {
    SfToast ToastObj;

    private async Task ShowToast()
    {
        await ToastObj.ShowAsync();
    }
}
```

### Message - Basic Example

```razor
@page "/message-demo"
@using Syncfusion.Blazor.Notifications

<SfMessage Severity="MessageSeverity.Success" ShowIcon="true" ShowCloseIcon="true">
    Your profile has been updated successfully!
</SfMessage>

<SfMessage Severity="MessageSeverity.Warning" ShowIcon="true">
    Your session will expire in 5 minutes.
</SfMessage>

<SfMessage Severity="MessageSeverity.Error" ShowIcon="true" ShowCloseIcon="true">
    Failed to save changes. Please try again.
</SfMessage>
```

### Skeleton - Basic Example

```razor
@page "/skeleton-demo"
@using Syncfusion.Blazor.Notifications

<div class="skeleton-container">
    <SfSkeleton Shape="SkeletonType.Circle" Width="60px" Height="60px" />
    <div class="skeleton-text">
        <SfSkeleton Shape="SkeletonType.Text" Width="80%" />
        <SfSkeleton Shape="SkeletonType.Text" Width="60%" />
    </div>
</div>

<style>
    .skeleton-container {
        display: flex;
        gap: 16px;
        padding: 16px;
    }
    .skeleton-text {
        flex: 1;
        display: flex;
        flex-direction: column;
        gap: 8px;
    }
</style>
```

## Documentation and Navigation Guide

### Toast Component

#### Getting Started with Toast
📄 **Read:** [references/toast-getting-started.md](references/toast-getting-started.md)
- Installation and NuGet package setup
- Service registration in Program.cs
- Basic toast implementation
- Showing and hiding toasts programmatically
- Position configuration basics
- Simple success/error notifications

#### Advanced Toast Features
📄 **Read:** [references/toast-features.md](references/toast-features.md)
- Position and alignment options (9 predefined positions)
- Animation settings (show/hide with effects and duration)
- Auto-dismiss with timeout and extended timeout
- Progress bar display and direction
- Action buttons with event handlers
- Title and content templates
- Multiple toast management and stacking order
- Event callbacks (OnOpen, OnClose, OnClick, BeforeOpen, BeforeClose)
- RTL (right-to-left) support
- Custom width, height, and CSS styling
- Target container specification

### Message Component

#### Message Implementation
📄 **Read:** [references/message-implementation.md](references/message-implementation.md)
- Getting started with Message component
- Severity types (Normal, Info, Success, Warning, Error)
- Variant styles (Text, Outlined, Filled)
- Icon configuration (built-in and custom)
- Close icon and dismissal handling
- Content alignment options
- Visibility control and two-way binding
- Close event handling
- Custom content with ChildContent
- CSS class customization
- Accessibility features
- Integration with forms and validation

### Skeleton Component

#### Skeleton Implementation
📄 **Read:** [references/skeleton-implementation.md](references/skeleton-implementation.md)
- Getting started with Skeleton component
- Skeleton shapes (Circle, Square, Rectangle, Text)
- Shimmer effects (Pulse, Wave, Fade, None)
- Width and height configuration
- Visibility toggle for loading states
- Accessibility labels for screen readers
- Multiple skeleton layouts (lists, cards, tables)
- Common loading patterns (profile, article, grid)
- CSS customization and theming
- Performance considerations

### Styling and Themes

#### Styling All Notification Components
📄 **Read:** [references/styling-and-themes.md](references/styling-and-themes.md)
- Theme integration (Bootstrap, Material, Fluent, Tailwind)
- Custom CSS classes for Toast
- Custom CSS classes for Message
- Custom CSS classes for Skeleton
- Dark mode support for all components
- Responsive design patterns
- Color customization
- Animation customization
- Component-specific styling techniques

### Use Cases and Examples

#### Real-World Implementation Examples
📄 **Read:** [references/use-cases-and-examples.md](references/use-cases-and-examples.md)
- Form submission feedback (Toast + Message)
- E-commerce cart notifications
- Dashboard data updates
- File upload progress and feedback
- User authentication feedback
- Data loading with Skeleton placeholders
- Error handling and recovery
- Multi-step process feedback
- Notification center/history
- Combining all three components effectively

## Common Patterns

### Pattern 1: Success/Error Feedback with Toast

```razor
@code {
    SfToast ToastObj;

    private async Task SaveData()
    {
        try
        {
            await DataService.SaveAsync();
            ToastObj.Title = "Success";
            ToastObj.Content = "Data saved successfully!";
            await ToastObj.ShowAsync();
        }
        catch (Exception ex)
        {
            ToastObj.Title = "Error";
            ToastObj.Content = $"Failed to save: {ex.Message}";
            await ToastObj.ShowAsync();
        }
    }
}
```

### Pattern 2: Form Validation with Message

```razor
<EditForm Model="@model" OnValidSubmit="HandleValidSubmit">
    <DataAnnotationsValidator />
    
    @if (!string.IsNullOrEmpty(errorMessage))
    {
        <SfMessage Severity="MessageSeverity.Error" 
                   ShowIcon="true" 
                   ShowCloseIcon="true"
                   Closed="@(() => errorMessage = null)">
            @errorMessage
        </SfMessage>
    }
    
    <!-- Form fields -->
</EditForm>
```

### Pattern 3: Loading State with Skeleton

```razor
@if (isLoading)
{
    <div class="product-card-skeleton">
        <SfSkeleton Shape="SkeletonType.Rectangle" Width="100%" Height="200px" />
        <SfSkeleton Shape="SkeletonType.Text" Width="80%" />
        <SfSkeleton Shape="SkeletonType.Text" Width="60%" />
    </div>
}
else
{
    <div class="product-card">
        <img src="@product.ImageUrl" alt="@product.Name" />
        <h3>@product.Name</h3>
        <p>@product.Description</p>
    </div>
}
```

### Pattern 4: Multiple Toast Types

```razor
<SfToast @ref="SuccessToast" 
         Title="Success" 
         Icon="e-success" 
         CssClass="e-toast-success"
         <ToastPosition X="Right" Y="Top"></ToastPositio

<SfToast @ref="ErrorToast" 
         Title="Error" 
         Icon="e-error" 
         CssClass="e-toast-danger"
         <ToastPosition X="Right" Y="Top"></ToastPosition>

<SfToast @ref="WarningToast" 
         Title="Warning" 
         Icon="e-warning" 
         CssClass="e-toast-warning"
         <ToastPosition X="Right" Y="Top"></ToastPosition>

@code {
    SfToast SuccessToast, ErrorToast, WarningToast;

    private async Task ShowSuccess(string message)
    {
        SuccessToast.Content = message;
        await SuccessToast.ShowAsync();
    }

    private async Task ShowError(string message)
    {
        ErrorToast.Content = message;
        await ErrorToast.ShowAsync();
    }

    private async Task ShowWarning(string message)
    {
        WarningToast.Content = message;
        await WarningToast.ShowAsync();
    }
}
```

## Installation

### NuGet Package

```bash
dotnet add package Syncfusion.Blazor.Notifications
dotnet add package Syncfusion.Blazor.Themes
```

### Service Registration

**Program.cs:**
```csharp
using Syncfusion.Blazor;

var builder = WebApplication.CreateBuilder(args);
builder.Services.AddSyncfusionBlazor();
```

### Namespace Import

**_Imports.razor:**
```razor
@using Syncfusion.Blazor.Notifications
```

### Theme Reference

**App.razor or layout file:**
```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

## Key Properties Reference

### Toast Properties

| Property | Type | Description |
|----------|------|-------------|
| Content | string | Toast message content |
| Title | string | Toast title text |
| Icon | string | Icon CSS class |
| Position | ToastPosition | X and Y position (Top, Bottom, Left, Right, Center) |
| Timeout | int | Auto-hide duration in milliseconds (default: 5000) |
| ShowCloseButton | bool | Display close button (default: false) |
| ShowProgressBar | bool | Display progress bar (default: false) |
| NewestOnTop | bool | Stack newest toast on top (default: true) |
| CssClass | string | Custom CSS classes |

### Message Properties

| Property | Type | Description |
|----------|------|-------------|
| Severity | MessageSeverity | Normal, Info, Success, Warning, Error |
| Variant | MessageVariant | Text, Outlined, Filled |
| ShowIcon | bool | Display severity icon (default: true) |
| ShowCloseIcon | bool | Display close button (default: false) |
| Visible | bool | Control visibility |
| ContentAlignment | HorizontalAlign | Left, Center, Right alignment |
| CssClass | string | Custom CSS classes |

### Skeleton Properties

| Property | Type | Description |
|----------|------|-------------|
| Shape | SkeletonType | Circle, Square, Rectangle, Text |
| Effect | ShimmerEffect | Pulse, Wave, Fade, None |
| Width | string | Width in px, %, or other CSS units |
| Height | string | Height in px, %, or other CSS units |
| Visible | bool | Control visibility |
| Label | string | Accessibility label |
| CssClass | string | Custom CSS classes |

## Common Use Cases

1. **Form Submission Feedback** - Show success/error toast after form submission
2. **Validation Messages** - Display inline messages for validation errors
3. **Loading States** - Use skeleton during data fetching
4. **Cart Operations** - Toast notifications for add/remove from cart
5. **File Upload** - Progress feedback and completion notification
6. **API Errors** - Display error messages with retry options
7. **Session Warnings** - Show warning message before session expires
8. **Data Refresh** - Skeleton placeholders during refresh operations
9. **Multi-Step Forms** - Progress feedback at each step
10. **Real-time Updates** - Toast notifications for live data changes

## Troubleshooting

### Toast Not Appearing
- Ensure `ShowAsync()` is called to display the toast
- Verify toast component has content or template
- Check if toast timeout is too short
- Confirm ToastPosition is within viewport bounds

### Message Not Visible
- Check `Visible` property is set to `true`
- Verify message has content in ChildContent or Content property
- Ensure component is rendered in DOM (not hidden by parent)
- Check CSS z-index conflicts

### Skeleton Not Animating
- Verify `Effect` property is set (Pulse, Wave, or Fade)
- Check if shimmer effect CSS is loaded with theme
- Ensure skeleton has defined width and height
- Confirm `Visible` is set to `true`

### Theme Not Applied
- Verify theme CSS is referenced in App.razor or layout
- Check Syncfusion.Blazor.Themes package is installed
- Ensure CSS link is before component usage
- Try clearing browser cache

### Service Not Registered
- Add `builder.Services.AddSyncfusionBlazor()` in Program.cs
- Verify Syncfusion.Blazor.Core package is installed
- Ensure using statement is present
- Rebuild project after adding service

## Best Practices

1. **Use appropriate notification type** - Toast for temporary, Message for persistent
2. **Limit toast duration** - 3-7 seconds for most messages, longer for errors
3. **Provide clear actions** - Include retry/dismiss buttons where appropriate
4. **Skeleton resemblance** - Make skeleton shapes match actual content structure
5. **Accessibility** - Always provide labels for skeletons and icons
6. **Position consistency** - Use same toast position throughout app
7. **Message severity** - Use correct severity level for message context
8. **Avoid overuse** - Don't overwhelm users with too many notifications
9. **Progressive enhancement** - Use skeleton for perceived performance improvement
10. **Error handling** - Always handle toast/message display errors gracefully

## Next Steps

- Explore [Toast Advanced Features](references/toast-features.md) for animations and templates
- Learn [Message Implementation](references/message-implementation.md) for form validation
- Review [Skeleton Patterns](references/skeleton-implementation.md) for loading states
- Check [Styling Guide](references/styling-and-themes.md) for customization
- See [Real-World Examples](references/use-cases-and-examples.md) for complete scenarios
