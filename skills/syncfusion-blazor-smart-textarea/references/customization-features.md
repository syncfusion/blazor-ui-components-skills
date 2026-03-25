# Syncfusion Blazor Smart TextArea - Customization & Features

## Overview
This guide covers customization options for the Smart TextArea component and inherited features from the base TextArea component.

## Customizing Suggestion Display

The Smart TextArea provides flexibility in how suggestions are displayed to users through the `ShowSuggestionOnPopup` attribute.

### Popup-Based Suggestions

When `ShowSuggestionOnPopup` is set to `true`, suggestions are displayed in a popup window.

**Example:**

```razor
@using Syncfusion.Blazor.SmartComponents

<SfSmartTextArea UserRole="@userRole" 
                Placeholder="Enter your queries here" 
                @bind-Value="prompt" 
                Width="75%" 
                RowCount="5" 
                ShowSuggestionOnPopup="true">
</SfSmartTextArea>

@code {
    private string? prompt;
    public string userRole = "Maintainer of an open-source project replying to GitHub issues";
}
```

**Benefits:**
- Clean interface with suggestions in a separate popup window
- Reduced visual clutter in the text input area
- Better control over suggestion display timing

### Inline Suggestions

When `ShowSuggestionOnPopup` is set to `false`, suggestions are displayed inline within the text area.

**Example:**

```razor
@using Syncfusion.Blazor.SmartComponents

<SfSmartTextArea UserRole="@userRole" 
                Placeholder="Enter your queries here" 
                @bind-Value="prompt" 
                Width="75%" 
                RowCount="5" 
                ShowSuggestionOnPopup="false">
</SfSmartTextArea>

@code {
    private string? prompt;
    public string userRole = "Maintainer of an open-source project replying to GitHub issues";
}
```

**Benefits:**
- Real-time suggestions as you type
- Natural text flow with suggestions appearing inline
- Easier one-handed interaction

### Default Behavior

By default, `ShowSuggestionOnPopup` is set to `false` (inline suggestions).

## Inherited Features from TextArea

The Syncfusion Blazor Smart TextArea component fully inherits all properties, features, and styling options from the Syncfusion Blazor TextArea component. This means you can leverage all existing TextArea features while benefiting from the enhanced AI-powered functionality of the Smart TextArea.

### Available Inherited Features

#### 1. Form Support
- Integrate the Smart TextArea seamlessly with HTML forms
- Support for form submissions and data binding
- Validation integration with form frameworks
- Reference: **Syncfusion Blazor Textarea Form Support Documentation**

#### 2. Floating Labels
- Display floating labels that move up when the textarea is focused or contains text
- Improves UI/UX with a modern, clean design
- Reduces the visual footprint of labels
- Reference: **Syncfusion Blazor Textarea Floating Labels Documentation**

#### 3. Events
- Access to comprehensive event system for user interactions
- Events like `OnChange`, `OnFocus`, `OnBlur`, and more
- Enable custom logic based on user interactions
- Reference: **Syncfusion Blazor Textarea Events Documentation**

#### 4. Methods
- Invoke methods programmatically to control the textarea
- Methods for focus management, value manipulation, and validation
- Enable dynamic behavior and user interactions
- Reference: **Syncfusion Blazor Textarea Methods Documentation**

#### 5. Style and Appearance
- Comprehensive styling options to match your application design
- CSS classes and inline styling support
- Theme customization and appearance control
- Reference: **Syncfusion Blazor Textarea Styles and Appearance Documentation**

## Key Attributes

| Attribute | Type | Description | Default |
|-----------|------|-------------|---------|
| `ShowSuggestionOnPopup` | bool | Controls whether suggestions display in popup or inline | false |
| `UserRole` | string | Defines the context for AI autocompletion | Required |
| `UserPhrases` | string[] | Predefined expressions for the user's tone | Optional |
| `Placeholder` | string | Placeholder text when input is empty | - |
| `Width` | string | Width of the component | - |
| `RowCount` | int | Number of visible rows | - |
| `Value` | string | Current text value (use @bind-Value for two-way binding) | - |

## Complete Example with Customization

```razor
@using Syncfusion.Blazor.SmartComponents

<div class="container">
    <h2>GitHub Issue Response Assistant</h2>
    
    <div class="textarea-wrapper">
        <SfSmartTextArea UserRole="@userRole" 
                        UserPhrases="@userPhrases" 
                        Placeholder="Type your response here..." 
                        @bind-Value="response" 
                        Width="100%" 
                        RowCount="8"
                        ShowSuggestionOnPopup="true">
        </SfSmartTextArea>
    </div>
    
    <div class="action-buttons">
        <button @onclick="SendResponse">Send Response</button>
        <button @onclick="ClearText">Clear</button>
    </div>
</div>

@code {
    private string? response;
    
    public string userRole = "Maintainer of an open-source project replying to GitHub issues";
    
    public string[] userPhrases = 
    [
        "Thank you for contacting us.",
        "To investigate, we will need a reproducible example as a public Git repository.",
        "Could you please post a screenshot of the issue?",
        "This sounds like a usage question rather than a bug report.",
        "We do not accept ZIP files as reproducible examples.",
        "Bug report: Error occurred in the following version",
        "Please provide detailed steps to reproduce the issue.",
        "Have you tried the latest version?"
    ];
    
    private void SendResponse()
    {
        // Handle response submission
        Console.WriteLine($"Response sent: {response}");
    }
    
    private void ClearText()
    {
        response = string.Empty;
    }
}
```

## Best Practices

1. **Choose Display Mode Wisely**
   - Use popup mode for large forms where inline suggestions might crowd the input
   - Use inline mode for dedicated single-field inputs

2. **Optimize UserPhrases**
   - Keep phrases concise and relevant
   - Update phrases based on actual patterns in your use case
   - Limit to 10-20 most common phrases for best UX

3. **Leverage Inherited Features**
   - Combine with form validation for better data quality
   - Use floating labels for a modern UI appearance
   - Implement event handlers for contextual behavior

4. **Accessibility**
   - Use appropriate ARIA labels with the textarea
   - Ensure sufficient contrast for text and suggestions
   - Test with screen readers

