# Message - Complete Implementation Guide

## Table of Contents
- [Overview](#overview)
- [Getting Started](#getting-started)
- [Severity Types](#severity-types)
- [Variant Styles](#variant-styles)
- [Icons](#icons)
- [Dismissible Messages](#dismissible-messages)
- [Content Alignment](#content-alignment)
- [Visibility Control](#visibility-control)
- [Event Handling](#event-handling)
- [Form Integration](#form-integration)
- [Accessibility](#accessibility)

---

## Overview

The SfMessage component displays inline alert messages with contextual feedback. Unlike Toast (which is temporary and positioned), Message is persistent, inline, and remains visible until dismissed or hidden programmatically.

**When to use Message:**
- Form validation feedback that should stay visible
- Persistent status information (warnings, errors, success)
- Inline contextual help or informational banners
- Page-level alerts that don't auto-dismiss
- Multi-line messages with detailed information

**When to use Toast instead:**
- Temporary notifications that auto-dismiss
- Feedback that appears in a specific position (corner, center)
- Non-critical updates that shouldn't interrupt workflow
- Multiple stacked notifications

---

## Getting Started

### Installation

Same as Toast - requires Syncfusion.Blazor.Notifications package:

```bash
dotnet add package Syncfusion.Blazor.Notifications
dotnet add package Syncfusion.Blazor.Themes
```

### Basic Message

```razor
@page "/message-basic"
@using Syncfusion.Blazor.Notifications

<SfMessage>
    This is a message using ChildContent.
</SfMessage>
```

### Message with Severity

```razor
<SfMessage Severity="MessageSeverity.Info">
    Information message
</SfMessage>
<SfMessage Severity="MessageSeverity.Success">
    Success message
</SfMessage>
<SfMessage Severity="MessageSeverity.Warning">
    Warning message
</SfMessage>
<SfMessage Severity="MessageSeverity.Error">
    Error message
</SfMessage>
```

---

## Severity Types

Messages come in five severity levels, each with distinct styling and semantic meaning.

### Normal (Default)

```razor
<SfMessage Severity="MessageSeverity.Normal">
    This is a normal message with neutral styling.
</SfMessage>
```

**Use for:**
- General information
- Neutral updates
- Non-critical notifications

### Info

```razor
<SfMessage Severity="MessageSeverity.Info" ShowIcon="true">
    Your profile is set to private. Only connections can view your information.
</SfMessage>
```

**Use for:**
- Informational content
- Tips and hints
- Feature explanations
- Status information

### Success

```razor
<SfMessage Severity="MessageSeverity.Success" ShowIcon="true">
    Your changes have been saved successfully!
</SfMessage>
```

**Use for:**
- Successful operations
- Confirmation messages
- Completion notifications
- Positive feedback

### Warning

```razor
<SfMessage Severity="MessageSeverity.Warning" ShowIcon="true">
    Your subscription expires in 7 days. Renew now to avoid service interruption.
</SfMessage>
```

**Use for:**
- Cautionary information
- Potential issues
- Expiring items
- Recommendations to act

### Error

```razor
<SfMessage Severity="MessageSeverity.Error" ShowIcon="true">
    Failed to save changes. Please check your input and try again.
</SfMessage>
```

**Use for:**
- Error messages
- Failed operations
- Validation errors
- Critical issues

### All Severity Types Example

```razor
@page "/message-severity"

<div class="message-demo">
    <h3>Message Severity Types</h3>
    
    <SfMessage Severity="MessageSeverity.Normal" ShowIcon="true">
        <strong>Normal:</strong> This is a general message with neutral information.
    </SfMessage>
    
    <SfMessage Severity="MessageSeverity.Info" ShowIcon="true">
        <strong>Info:</strong> Did you know? You can customize your dashboard layout.
    </SfMessage>
    
    <SfMessage Severity="MessageSeverity.Success" ShowIcon="true">
        <strong>Success:</strong> Your profile has been updated successfully.
    </SfMessage>
    
    <SfMessage Severity="MessageSeverity.Warning" ShowIcon="true">
        <strong>Warning:</strong> Your password will expire in 3 days.
    </SfMessage>
    
    <SfMessage Severity="MessageSeverity.Error" ShowIcon="true">
        <strong>Error:</strong> Unable to connect to the server. Please try again later.
    </SfMessage>
</div>

<style>
    .message-demo > * {
        margin-bottom: 15px;
    }
</style>
```

---

## Variant Styles

Messages support three visual variants: Text, Outlined, and Filled.

### Text Variant (Default)

Subtle styling with colored text and border.

```razor
<SfMessage Severity="MessageSeverity.Info" 
           Variant="MessageVariant.Text"
           ShowIcon="true">
    Text variant with subtle background
</SfMessage>
```

### Outlined Variant

Emphasized border with transparent background.

```razor
<SfMessage Severity="MessageSeverity.Success" 
           Variant="MessageVariant.Outlined"
           ShowIcon="true">
    Outlined variant with prominent border
</SfMessage>
```

### Filled Variant

Solid background with white text.

```razor
<SfMessage Severity="MessageSeverity.Warning" 
           Variant="MessageVariant.Filled"
           ShowIcon="true">
    Filled variant with solid background
</SfMessage>
```

### All Variants Comparison

```razor
@page "/message-variants"

<h4>Info Messages</h4>
<SfMessage Severity="MessageSeverity.Info" Variant="MessageVariant.Text" ShowIcon="true">
    Info - Text Variant
</SfMessage>
<SfMessage Severity="MessageSeverity.Info" Variant="MessageVariant.Outlined" ShowIcon="true">
    Info - Outlined Variant
</SfMessage>
<SfMessage Severity="MessageSeverity.Info" Variant="MessageVariant.Filled" ShowIcon="true">
    Info - Filled Variant
</SfMessage>

<h4>Success Messages</h4>
<SfMessage Severity="MessageSeverity.Success" Variant="MessageVariant.Text" ShowIcon="true">
    Success - Text Variant
</SfMessage>
<SfMessage Severity="MessageSeverity.Success" Variant="MessageVariant.Outlined" ShowIcon="true">
    Success - Outlined Variant
</SfMessage>
<SfMessage Severity="MessageSeverity.Success" Variant="MessageVariant.Filled" ShowIcon="true">
    Success - Filled Variant
</SfMessage>

<h4>Warning Messages</h4>
<SfMessage Severity="MessageSeverity.Warning" Variant="MessageVariant.Text" ShowIcon="true">
    Warning - Text Variant
</SfMessage>
<SfMessage Severity="MessageSeverity.Warning" Variant="MessageVariant.Outlined" ShowIcon="true">
    Warning - Outlined Variant
</SfMessage>
<SfMessage Severity="MessageSeverity.Warning" Variant="MessageVariant.Filled" ShowIcon="true">
    Warning - Filled Variant
</SfMessage>

<h4>Error Messages</h4>
<SfMessage Severity="MessageSeverity.Error" Variant="MessageVariant.Text" ShowIcon="true">
    Error - Text Variant
</SfMessage>
<SfMessage Severity="MessageSeverity.Error" Variant="MessageVariant.Outlined" ShowIcon="true">
    Error - Outlined Variant
</SfMessage>
<SfMessage Severity="MessageSeverity.Error" Variant="MessageVariant.Filled" ShowIcon="true">
    Error - Filled Variant
</SfMessage>

<style>
    h4 {
        margin-top: 20px;
        margin-bottom: 10px;
    }
    
    .e-message {
        margin-bottom: 10px;
    }
</style>
```

---

## Icons

Control icon display with built-in or custom icons.

### Built-in Icons

Each severity has a default icon that appears when `ShowIcon` is enabled.

```razor
<SfMessage Severity="MessageSeverity.Info" ShowIcon="true">
    Info with built-in icon (ℹ️)
</SfMessage>

<SfMessage Severity="MessageSeverity.Success" ShowIcon="true">
    Success with built-in icon (✓)
</SfMessage>

<SfMessage Severity="MessageSeverity.Warning" ShowIcon="true">
    Warning with built-in icon (⚠️)
</SfMessage>

<SfMessage Severity="MessageSeverity.Error" ShowIcon="true">
    Error with built-in icon (✕)
</SfMessage>
```

### Hide Icons

```razor
<SfMessage Severity="MessageSeverity.Info" ShowIcon="false">
    Message without icon
</SfMessage>
```

### Custom Icons with CSS

```razor
<SfMessage Severity="MessageSeverity.Info" ShowIcon="true" CssClass="custom-icon-message">
    Message with custom icon
</SfMessage>

<style>
    .custom-icon-message .e-msg-icon::before {
        content: "🎉";
        font-size: 20px;
    }
</style>
```

---

## Dismissible Messages

Allow users to close messages with a close button.

### Basic Dismissible Message

```razor
<SfMessage Severity="MessageSeverity.Info" 
           ShowIcon="true"
           ShowCloseIcon="true">
    You can close this message by clicking the X button.
</SfMessage>
```

### Close Event Handling

```razor
@page "/message-dismissible"

@if (showMessage)
{
    <SfMessage Severity="MessageSeverity.Success" 
               ShowIcon="true"
               ShowCloseIcon="true"
               Closed="OnMessageClosed">
        Your settings have been saved. This message can be dismissed.
    </SfMessage>
}

<SfButton @onclick="ShowMessageAgain" Disabled="@showMessage">
    Show Message Again
</SfButton>

@code {
    private bool showMessage = true;

    private void OnMessageClosed(MessageCloseEventArgs args)
    {
        showMessage = false;
        Console.WriteLine("Message was closed by user");
        // Can perform actions like logging, analytics, etc.
    }

    private void ShowMessageAgain()
    {
        showMessage = true;
    }
}
```

### Multiple Dismissible Messages

```razor
@page "/message-multiple"

@if (showInfoMessage)
{
    <SfMessage Severity="MessageSeverity.Info" 
               ShowIcon="true" 
               ShowCloseIcon="true"
               Closed="@(() => showInfoMessage = false)">
        <strong>Tip:</strong> You can customize your dashboard layout.
    </SfMessage>
}

@if (showWarningMessage)
{
    <SfMessage Severity="MessageSeverity.Warning" 
               ShowIcon="true" 
               ShowCloseIcon="true"
               Closed="@(() => showWarningMessage = false)">
        <strong>Reminder:</strong> Complete your profile for better matches.
    </SfMessage>
}

@if (showErrorMessage)
{
    <SfMessage Severity="MessageSeverity.Error" 
               ShowIcon="true" 
               ShowCloseIcon="true"
               Closed="@(() => showErrorMessage = false)">
        <strong>Error:</strong> Failed to load user preferences.
    </SfMessage>
}

<SfButton @onclick="ShowAllMessages">Show All Messages</SfButton>

@code {
    private bool showInfoMessage = true;
    private bool showWarningMessage = true;
    private bool showErrorMessage = true;

    private void ShowAllMessages()
    {
        showInfoMessage = true;
        showWarningMessage = true;
        showErrorMessage = true;
    }
}
```

---

## Content Alignment

Control horizontal alignment of message content.

```razor
<SfMessage ContentAlignment="HorizontalAlign.Left" 
           ShowIcon="true">
           Left aligned (default)
</SfMessage>

<SfMessage ContentAlignment="HorizontalAlign.Center" 
           ShowIcon="true">
           Center aligned
</SfMessage>

<SfMessage ContentAlignment="HorizontalAlign.Right" 
           ShowIcon="true">
           Right aligned
</SfMessage>
```

---

## Visibility Control

Dynamically show/hide messages with the `Visible` property.

### Programmatic Visibility

```razor
@page "/message-visibility"

<SfButton @onclick="ToggleMessage">
    @(messageVisible ? "Hide" : "Show") Message
</SfButton>

<SfMessage Severity="MessageSeverity.Info" 
           ShowIcon="true"
           Visible="@messageVisible">
    This message visibility is controlled programmatically.
</SfMessage>

@code {
    private bool messageVisible = true;

    private void ToggleMessage()
    {
        messageVisible = !messageVisible;
    }
}
```

### Two-Way Binding

```razor
<SfMessage Severity="MessageSeverity.Warning"
           ShowIcon="true"
           ShowCloseIcon="true"
           @bind-Visible="messageVisible">
    Two-way binding allows tracking visibility changes.
</SfMessage>

@code {
    private bool messageVisible = true;

    protected override void OnParametersSet()
    {
        if (!messageVisible)
        {
            SaveUserPreference("message_dismissed", true);
        }
    }

    private void SaveUserPreference(string key, bool value)
    {
        Console.WriteLine($"{key} saved with value: {value}");
    }
}
```

### Conditional Display

```razor
@page "/message-conditional"

<EditForm Model="@user" OnValidSubmit="SaveUser">
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
    
    @if (!string.IsNullOrEmpty(successMessage))
    {
        <SfMessage Severity="MessageSeverity.Success" 
                   ShowIcon="true">
            @successMessage
        </SfMessage>
    }
    
    <!-- Form fields -->
    <SfButton Type="ButtonType.Submit">Save</SfButton>
</EditForm>

@code {
    private User user = new();
    private string errorMessage;
    private string successMessage;

    private async Task SaveUser()
    {
        try
        {
            await UserService.SaveAsync(user);
            successMessage = "User saved successfully!";
            errorMessage = null;
        }
        catch (Exception ex)
        {
            errorMessage = $"Failed to save user: {ex.Message}";
            successMessage = null;
        }
    }
}
```

---

## Event Handling

### Closed Event

Triggered when user closes the message by clicking the close icon.

```razor
<SfMessage Severity="MessageSeverity.Info" 
           ShowCloseIcon="true"
           Closed="OnClosed">
    Close this message
</SfMessage>

@code {
    private void OnClosed(MessageCloseEventArgs args)
    {
        // Event triggered when close icon is clicked
        Console.WriteLine("Message closed");
        
        // Can access event data
        // args contains information about the close event
    }
}
```

---

## Form Integration

Messages work excellently for form validation feedback.

### Form Validation Example

```razor
@page "/form-validation"
@using System.ComponentModel.DataAnnotations

<EditForm Model="@model" OnValidSubmit="HandleValidSubmit" OnInvalidSubmit="HandleInvalidSubmit">
    <DataAnnotationsValidator />
    
    @if (showSuccessMessage)
    {
        <SfMessage Severity="MessageSeverity.Success" 
                   ShowIcon="true" 
                   ShowCloseIcon="true"
                   Closed="@(() => showSuccessMessage = false)">
            <strong>Success!</strong> Your registration has been submitted.
        </SfMessage>
    }
    
    @if (showErrorMessage)
    {
        <SfMessage Severity="MessageSeverity.Error" 
                   ShowIcon="true" 
                   ShowCloseIcon="true"
                   Closed="@(() => showErrorMessage = false)">
            <strong>Error!</strong> Please correct the errors below and try again.
        </SfMessage>
    }
    
    <div class="form-group">
        <label>Email:</label>
        <InputText @bind-Value="model.Email" class="form-control" />
        <ValidationMessage For="@(() => model.Email)" />
    </div>
    
    <div class="form-group">
        <label>Password:</label>
        <InputText @bind-Value="model.Password" type="password" class="form-control" />
        <ValidationMessage For="@(() => model.Password)" />
    </div>
    
    <SfButton Type="ButtonType.Submit">Submit</SfButton>
</EditForm>

@code {
    private RegistrationModel model = new();
    private bool showSuccessMessage = false;
    private bool showErrorMessage = false;

    private async Task HandleValidSubmit()
    {
        try
        {
            await SubmitRegistration();
            showSuccessMessage = true;
            showErrorMessage = false;
        }
        catch
        {
            showSuccessMessage = false;
            showErrorMessage = true;
        }
    }

    private void HandleInvalidSubmit()
    {
        showErrorMessage = true;
        showSuccessMessage = false;
    }

    public class RegistrationModel
    {
        [Required(ErrorMessage = "Email is required")]
        [EmailAddress(ErrorMessage = "Invalid email format")]
        public string Email { get; set; }

        [Required(ErrorMessage = "Password is required")]
        [MinLength(8, ErrorMessage = "Password must be at least 8 characters")]
        public string Password { get; set; }
    }
}
```

### API Error Display

```razor
@code {
    private string apiErrorMessage;

    private async Task LoadData()
    {
        try
        {
            var data = await ApiService.GetDataAsync();
            apiErrorMessage = null; // Clear any previous errors
        }
        catch (HttpRequestException ex)
        {
            apiErrorMessage = $"Network error: {ex.Message}";
        }
        catch (Exception ex)
        {
            apiErrorMessage = $"An error occurred: {ex.Message}";
        }
    }
}

@if (!string.IsNullOrEmpty(apiErrorMessage))
{
    <SfMessage Severity="MessageSeverity.Error" 
               ShowIcon="true" 
               ShowCloseIcon="true"
               Closed="@(() => apiErrorMessage = null)">
        @apiErrorMessage
    </SfMessage>
}
```

---

## Accessibility

Messages are designed with accessibility in mind.

### ARIA Attributes

Messages automatically include appropriate ARIA roles and attributes:

```razor
<!-- Rendered HTML includes: -->
<!-- role="alert" for error/warning -->
<!-- aria-live="polite" for info/success -->
```

### Screen Reader Support

```razor
<SfMessage Severity="MessageSeverity.Error" ShowIcon="true">
    <span class="sr-only">Error:</span>
    Your session has expired. Please log in again.
</SfMessage>

<style>
    .sr-only {
        position: absolute;
        width: 1px;
        height: 1px;
        padding: 0;
        margin: -1px;
        overflow: hidden;
        clip: rect(0,0,0,0);
        border: 0;
    }
</style>
```

### Keyboard Navigation

Close icons are keyboard accessible:
- **Tab:** Focus the close button
- **Enter/Space:** Close the message

---

## Best Practices

1. **Choose appropriate severity** - Match severity to message importance
2. **Use icons** - Enable icons for better visual recognition
3. **Be concise** - Keep messages brief and actionable
4. **Provide actions** - Include buttons or links for user response when appropriate
5. **Don't overuse** - Too many messages can overwhelm users
6. **Consider persistence** - Use Message for important info that shouldn't auto-dismiss
7. **Test accessibility** - Verify screen reader compatibility
8. **Position strategically** - Place messages near relevant content
9. **Clear errors** - Remove error messages when issues are resolved
10. **Consistent styling** - Use same variant throughout your app

---

## Next Steps

- Explore [Toast Component](toast-getting-started.md) for temporary notifications
- Learn [Skeleton Component](skeleton-implementation.md) for loading states
- Review [Styling Guide](styling-and-themes.md) for customization
- See [Use Cases](use-cases-and-examples.md) for complete examples
