# Interim Results and Configuration Options

## Table of Contents
- [AllowInterimResults Property](#allowinterimresults-property)
- [ShowTooltip Property](#showtooltip-property)
- [Disabled State](#disabled-state)
- [HTML Attributes](#html-attributes)
- [Configuration Combinations](#configuration-combinations)
- [Advanced Customization](#advanced-customization)

## AllowInterimResults Property

The `AllowInterimResults` property controls whether the component shows intermediate recognition results as the user speaks.

### What Are Interim Results?

**With AllowInterimResults="true":**
- Transcript updates in real time while user speaks
- Shows partial, incomplete text
- Useful for live feedback and interactive applications
- More responsive user experience

**With AllowInterimResults="false":**
- Only final results are shown
- Waits until speech recognition completes
- Provides clean, complete transcripts
- Better for forms that expect final text

### Real-Time Interim Results Example

```razor
@using Syncfusion.Blazor.Inputs

<div style="max-width: 600px; margin: 20px auto;">
    <h3>Live Transcription</h3>
    
    <div style="background: #f5f5f5; padding: 15px; border-radius: 4px; margin-bottom: 20px;">
        <p style="margin: 0; color: #666;">Real-time transcript as you speak:</p>
        <p style="font-size: 18px; margin: 10px 0 0 0;">@transcript</p>
    </div>
    
    <SfSpeechToText 
        AllowInterimResults="true"
        @bind-Transcript="@transcript">
    </SfSpeechToText>
</div>

@code {
    private string transcript = "";
}
```

**Result:** Text appears character by character as the user speaks.

### Final Results Only Example

```razor
@using Syncfusion.Blazor.Inputs

<div style="max-width: 600px; margin: 20px auto;">
    <h3>Final Transcription</h3>
    
    <div style="background: #e8f5e9; padding: 15px; border-radius: 4px; margin-bottom: 20px;">
        <p style="margin: 0; color: #666;">Final transcript (appears after you stop speaking):</p>
        <p style="font-size: 18px; margin: 10px 0 0 0;">@transcript</p>
    </div>
    
    <SfSpeechToText 
        AllowInterimResults="false"
        @bind-Transcript="@transcript">
    </SfSpeechToText>
    
    @if (string.IsNullOrEmpty(transcript))
    {
        <p style="color: #999; margin-top: 10px; font-size: 12px;">Result will appear after you finish speaking.</p>
    }
</div>

@code {
    private string transcript = "";
}
```

**Result:** Text only appears after the user stops speaking.

### Comparison Table

| Aspect | AllowInterimResults="true" | AllowInterimResults="false" |
|--------|---------------------------|---------------------------|
| **Timing** | Real-time updates | After speech ends |
| **Completeness** | Partial text | Complete text |
| **User Feedback** | Immediate response | Final result only |
| **API Calls** | Multiple updates | Single update |
| **Best For** | Live chat, assistants | Forms, data entry |
| **Default** | true | - |

## ShowTooltip Property

The `ShowTooltip` property controls whether a tooltip appears when hovering over the microphone button.

### Tooltip Enabled (Default)

```razor
@using Syncfusion.Blazor.Inputs

<SfSpeechToText ShowTooltip="true" @bind-Transcript="@transcript"></SfSpeechToText>
<p>Hover over the microphone button to see the tooltip.</p>

@code {
    private string transcript = "";
}
```

**Tooltip text:** "Click to start recording" (default behavior)

### Tooltip Disabled

```razor
@using Syncfusion.Blazor.Inputs

<SfSpeechToText ShowTooltip="false" @bind-Transcript="@transcript"></SfSpeechToText>
<p>No tooltip appears on hover.</p>

@code {
    private string transcript = "";
}
```

**Use case:** When you have explicit instructions or want minimal UI clutter.

### Conditional Tooltip

```razor
@using Syncfusion.Blazor.Inputs

<div style="margin-bottom: 20px;">
    <label>
        <input type="checkbox" @onchange="@((ChangeEventArgs e) => showTooltip = (bool)e.Value)" />
        Show tooltip on hover
    </label>
</div>

<SfSpeechToText ShowTooltip="@showTooltip" @bind-Transcript="@transcript"></SfSpeechToText>

@code {
    private string transcript = "";
    private bool showTooltip = true;
}
```

## Disabled State

The `Disabled` property prevents user interaction when set to `true`.

### Basic Disabled Example

```razor
@using Syncfusion.Blazor.Inputs

<SfSpeechToText Disabled="true" @bind-Transcript="@transcript"></SfSpeechToText>
<p>The microphone button is disabled and cannot be clicked.</p>

@code {
    private string transcript = "";
}
```

### Conditional Disabled State

Disable based on conditions like loading, permissions, or validation:

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <div style="margin-bottom: 20px;">
        <label>
            <input type="checkbox" @onchange="@((ChangeEventArgs e) => isProcessing = (bool)e.Value)" />
            Simulate processing (disables speech input)
        </label>
    </div>
    
    <SfSpeechToText 
        Disabled="@isProcessing" 
        @bind-Transcript="@transcript"
        style="@(isProcessing ? "opacity: 0.5;" : "")">
    </SfSpeechToText>
    
    @if (isProcessing)
    {
        <p style="color: #FF9800; margin-top: 10px;">⏳ Processing... Please wait.</p>
    }
    
    @if (!string.IsNullOrEmpty(transcript))
    {
        <p style="margin-top: 20px; padding: 10px; background: #e3f2fd; border-radius: 4px;">
            Transcript: @transcript
        </p>
    }
</div>

@code {
    private string transcript = "";
    private bool isProcessing = false;
}
```

### Disable When Conditions Met

```razor
@using Syncfusion.Blazor.Inputs

<div style="max-width: 500px;">
    <div style="margin-bottom: 20px;">
        <input 
            type="text" 
            @bind="@characterLimit" 
            placeholder="Max characters allowed"
            style="padding: 8px; width: 100%;" />
        <small>Current: @transcript.Length / @characterLimit</small>
    </div>
    
    <SfSpeechToText 
        Disabled="@(transcript.Length >= int.Parse(characterLimit))"
        @bind-Transcript="@transcript"
        style="@(transcript.Length >= int.Parse(characterLimit) ? "opacity: 0.5;" : "")">
    </SfSpeechToText>
    
    @if (transcript.Length >= int.Parse(characterLimit))
    {
        <p style="color: #f44336; margin-top: 10px;">Character limit reached.</p>
    }
</div>

@code {
    private string transcript = "";
    private string characterLimit = "500";
}
```

## HTML Attributes

The `HtmlAttributes` property allows you to add custom HTML attributes to the microphone button element.

### Adding CSS Classes

```razor
@using Syncfusion.Blazor.Inputs

<style>
    .custom-mic-button {
        background-color: #2196F3 !important;
        padding: 10px 20px !important;
    }
    
    .custom-mic-button:hover {
        background-color: #1976D2 !important;
    }
</style>

<SfSpeechToText 
    @bind-Transcript="@transcript"
    HtmlAttributes="@(new Dictionary<string, object> { { "class", "custom-mic-button" } })">
</SfSpeechToText>

@code {
    private string transcript = "";
}
```

### Adding ARIA Attributes for Accessibility

```razor
@using Syncfusion.Blazor.Inputs

<SfSpeechToText 
    @bind-Transcript="@transcript"
    HtmlAttributes="@htmlAttrs">
</SfSpeechToText>

@code {
    private string transcript = "";
    
    private Dictionary<string, object> htmlAttrs = new()
    {
        { "aria-label", "Microphone button for voice input" },
        { "aria-describedby", "mic-help-text" },
        { "role", "button" }
    };
}
```

### Adding Data Attributes

```razor
@using Syncfusion.Blazor.Inputs

<SfSpeechToText 
    @bind-Transcript="@transcript"
    HtmlAttributes="@(new Dictionary<string, object> 
    { 
        { "data-testid", "voice-input-microphone" },
        { "data-component", "speech-to-text" }
    })">
</SfSpeechToText>

@code {
    private string transcript = "";
}
```

## Configuration Combinations

### Combination 1: Strict Data Entry
- Disable interim results for clean data
- Show tooltip for guidance
- Enable by default

```razor
@using Syncfusion.Blazor.Inputs

<SfSpeechToText 
    AllowInterimResults="false"
    ShowTooltip="true"
    Disabled="false"
    @bind-Transcript="@transcript">
</SfSpeechToText>

@code {
    private string transcript = "";
}
```

### Combination 2: Real-Time Communication
- Enable interim results for live feedback
- Hide tooltip for minimal UI
- Allow disable during processing

```razor
@using Syncfusion.Blazor.Inputs

<SfSpeechToText 
    AllowInterimResults="true"
    ShowTooltip="false"
    Disabled="@isProcessing"
    @bind-Transcript="@transcript">
</SfSpeechToText>

@code {
    private string transcript = "";
    private bool isProcessing = false;
}
```

### Combination 3: Accessibility Focused
- Show tooltip for guidance
- Final results for clarity
- Custom ARIA attributes

```razor
@using Syncfusion.Blazor.Inputs

<SfSpeechToText 
    AllowInterimResults="false"
    ShowTooltip="true"
    @bind-Transcript="@transcript"
    HtmlAttributes="@(new Dictionary<string, object> 
    { 
        { "aria-label", "Voice input microphone button" },
        { "aria-live", "polite" }
    })">
</SfSpeechToText>

@code {
    private string transcript = "";
}
```

## Advanced Customization

### Create a Wrapper Component

```razor
<!-- VoiceInput.razor -->
@using Syncfusion.Blazor.Inputs

<div class="voice-input-wrapper" style="@WrapperStyle">
    <div class="voice-input-header">
        <h4>@Label</h4>
        @if (ShowHint)
        {
            <small style="color: #666;">@Hint</small>
        }
    </div>
    
    <SfSpeechToText 
        AllowInterimResults="@AllowInterim"
        ShowTooltip="@ShowTooltip"
        Disabled="@Disabled"
        Language="@Language"
        @bind-Transcript="@Transcript">
    </SfSpeechToText>
    
    @if (ShowCharCount)
    {
        <small style="color: #999;">@Transcript.Length characters</small>
    }
</div>

@code {
    [Parameter]
    public string Label { get; set; } = "Voice Input";
    
    [Parameter]
    public string Hint { get; set; } = "";
    
    [Parameter]
    public bool ShowHint { get; set; } = true;
    
    [Parameter]
    public bool AllowInterim { get; set; } = true;
    
    [Parameter]
    public bool ShowTooltip { get; set; } = true;
    
    [Parameter]
    public bool Disabled { get; set; } = false;
    
    [Parameter]
    public bool ShowCharCount { get; set; } = false;
    
    [Parameter]
    public string Language { get; set; } = "en-US";
    
    [Parameter]
    public string Transcript { get; set; } = "";
    
    [Parameter]
    public EventCallback<string> TranscriptChanged { get; set; }
    
    [Parameter]
    public string WrapperStyle { get; set; } = "padding: 15px; border: 1px solid #ddd; border-radius: 4px;";
}
```

**Usage:**
```razor
<VoiceInput 
    Label="Customer Feedback"
    Hint="Speak your feedback clearly"
    AllowInterim="false"
    Language="en-US"
    @bind-Transcript="@feedback">
</VoiceInput>
```

## Best Practices

1. **Choose AllowInterimResults based on use case:**
   - `true` for chat, real-time feedback
   - `false` for forms, data entry

2. **Provide visual feedback:**
   - Use tooltips or help text
   - Show character limits
   - Indicate disabled state

3. **Consider accessibility:**
   - Add ARIA labels
   - Use semantic HTML attributes
   - Test with screen readers

4. **Disable appropriately:**
   - When processing results
   - When form is submitted
   - When permissions are missing

5. **Customize responsibly:**
   - Maintain usability with styling
   - Keep button recognizable
   - Ensure sufficient contrast
