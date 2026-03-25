# Getting Started with SpeechToText

This guide covers the essential setup steps to install and use the Syncfusion Blazor SpeechToText component in your project.

## Installation

### Step 1: Install NuGet Package

The SpeechToText component is part of the `Syncfusion.Blazor.Inputs` package. Install it via the .NET CLI or Package Manager:

**Using .NET CLI:**
```bash
dotnet add package Syncfusion.Blazor.Inputs
```

**Using Package Manager:**
```powershell
Install-Package Syncfusion.Blazor.Inputs
```

### Step 2: Import Namespaces

Add the required namespaces to your `_Imports.razor` file:

```csharp
@using Syncfusion.Blazor
@using Syncfusion.Blazor.Inputs
```

### Step 3: Register Syncfusion Service

In your `Program.cs`, register the Syncfusion Blazor service:

```csharp
builder.Services.AddSyncfusionBlazor();
```

### Step 4: Add Theme and Scripts

In your `App.razor` or main layout file, include the Syncfusion theme and core scripts:

```html
<!-- Add this in the <head> section -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- Add this before the closing </body> tag -->
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

**Available Themes:**
- `bootstrap5.css` (recommended)
- `material.css`
- `fluent.css`
- `tailwind.css`
- `fabric.css`

## Basic Implementation

Here's a minimal example to display the SpeechToText component:

```razor
@using Syncfusion.Blazor.Inputs

<SfSpeechToText></SfSpeechToText>

@code {
    // Component renders with default settings
}
```

This renders a microphone button that users can click to start voice input.

## Binding Transcript

To capture and display the transcribed text, use two-way binding with the `Transcript` property:

```razor
@using Syncfusion.Blazor.Inputs

<div style="display: flex; gap: 20px;">
    <div>
        <h4>Voice Input</h4>
        <SfSpeechToText @bind-Transcript="@speechText"></SfSpeechToText>
    </div>
    
    <div>
        <h4>Transcribed Text</h4>
        <p>@speechText</p>
    </div>
</div>

@code {
    private string speechText = "";
}
```

**What happens:**
1. User clicks the microphone button
2. Component captures audio and transcribes speech
3. `speechText` variable updates automatically
4. Transcribed text displays in the `<p>` tag

## Integration with TextArea

A common pattern is to display the transcript in a `SfTextArea` for editing:

```razor
@using Syncfusion.Blazor.Inputs

<div style="display: flex; flex-direction: column; gap: 20px; align-items: center;">
    <h3>Speech to Text Converter</h3>
    
    <SfSpeechToText @bind-Transcript="@transcript"></SfSpeechToText>
    
    <SfTextArea 
        RowCount="5" 
        ColumnCount="50" 
        @bind-Value="@transcript" 
        ResizeMode="Resize.None" 
        Placeholder="Transcribed text will appear here...">
    </SfTextArea>
    
    <button @onclick="ClearTranscript" style="padding: 10px 20px; background: #f44336; color: white; border: none; border-radius: 4px; cursor: pointer;">
        Clear
    </button>
</div>

@code {
    private string transcript = "";
    
    private void ClearTranscript()
    {
        transcript = "";
    }
}
```

**Benefits of this pattern:**
- User can speak to populate the text area
- Text is editable for corrections
- Both controls stay synchronized
- Clear button resets everything

## Styling and CSS

By default, SpeechToText integrates with your Syncfusion theme. To customize appearance, apply CSS classes:

```razor
<style>
    .custom-speech-container {
        display: flex;
        flex-direction: column;
        gap: 15px;
        padding: 20px;
        border: 1px solid #ddd;
        border-radius: 8px;
        max-width: 500px;
    }
    
    .custom-speech-container h3 {
        margin: 0 0 10px 0;
        color: #333;
    }
</style>

<div class="custom-speech-container">
    <h3>Voice Input Form</h3>
    <SfSpeechToText @bind-Transcript="@transcript"></SfSpeechToText>
    <p>Your text: @transcript</p>
</div>

@code {
    private string transcript = "";
}
```

## Checking Browser Support

Before implementing, consider checking browser compatibility. The component uses the Web Speech API, which is not supported in all browsers:

```razor
@code {
    private bool isSupported = true;

    protected override void OnInitialized()
    {
        // Note: In production, you'd typically check this client-side
        // For now, we proceed assuming support. See browser-support.md for details.
    }
}
```

## Common Properties

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `Transcript` | string | "" | Two-way binding for transcribed text |
| `Language` | string | "en-US" | Language for speech recognition |
| `AllowInterimResults` | bool | true | Show interim results while speaking |
| `ShowTooltip` | bool | true | Display tooltip on hover |
| `Disabled` | bool | false | Disable the component |

## Next Steps

- Read **Transcript and Language** to learn about language support
- Explore **Listening States** for state management
- Check **Error Handling** for production-ready code
- Review **Browser Support** to ensure compatibility
