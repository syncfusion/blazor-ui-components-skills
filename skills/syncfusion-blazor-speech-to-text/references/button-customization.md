# Button Customization

## Table of Contents
- [ButtonSettings Overview](#button-settings-overview)
- [Text Configuration](#text-configuration)
- [Icon Configuration](#icon-configuration)
- [Icon Position](#icon-position)
- [Primary Button Styling](#primary-button-styling)
- [Complete Customization Examples](#complete-customization-examples)
- [Advanced Patterns](#advanced-patterns)

## ButtonSettings Overview

The `ButtonSettings` property in `SfSpeechToText` allows you to customize the microphone button's appearance, text, icons, and behavior. This provides control over how the voice input button is presented to users.

### Key Properties

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `Text` | string | Empty | Text displayed on the button in start state |
| `StopStateText` | string | Empty | Text displayed on the button in stop state |
| `IconCss` | string | Empty | CSS class for the start state icon |
| `StopIconCss` | string | Empty | CSS class for the stop state icon |
| `IconPosition` | IconPosition | Left | Position of icon relative to text (Left, Right, Top, Bottom) |
| `IsPrimary` | bool | false | Whether to apply primary button styling |

## Text Configuration

### Basic Text Example

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Custom Button Text</h3>
    
    <SfSpeechToText>
        <SpeechToTextButtonSettings
            Text="🎤 Start Recording"
            StopStateText="⏹️ Stop Recording">
        </SpeechToTextButtonSettings>
    </SfSpeechToText>
</div>

@code {
    // Button displays "🎤 Start Recording" by default
    // Changes to "⏹️ Stop Recording" when listening
}
```

### Empty Text (Icon Only)

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Icon-Only Button</h3>
    
    <SfSpeechToText>
        <SpeechToTextButtonSettings
            IconCss="e-icons e-microphone"
            StopIconCss="e-icons e-stop">
        </SpeechToTextButtonSettings>
    </SfSpeechToText>
</div>

@code {
    // Button displays only icons, no text
}
```

## Icon Configuration

### Using Syncfusion Icon Classes

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Syncfusion Icons</h3>
    
    <SfSpeechToText>
        <SpeechToTextButtonSettings
            Text="Listen"
            IconCss="e-icons e-microphone"
            StopStateText="Stop"
            StopIconCss="e-icons e-stop">
        </SpeechToTextButtonSettings>
    </SfSpeechToText>
</div>

@code {
    // Uses Syncfusion's built-in icon library
    // Microphone icon in start state, stop icon in listening state
}
```

### Using Font Awesome Icons

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Font Awesome Icons</h3>
    
    <SfSpeechToText>
        <SpeechToTextButtonSettings
            Text="Record"
            IconCss="fas fa-microphone"
            StopStateText="Stop"
            StopIconCss="fas fa-stop">
        </SpeechToTextButtonSettings>
    </SfSpeechToText>
</div>

@code {
    // Requires Font Awesome CSS to be included in the application
    // FontAwesome icons provide a different visual style
}
```

### Using Bootstrap Icons

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Bootstrap Icons</h3>
    
    <SfSpeechToText>
        <SpeechToTextButtonSettings
            Text="Voice"
            IconCss="bi bi-mic"
            StopStateText="Stop"
            StopIconCss="bi bi-stop">
        </SpeechToTextButtonSettings>
    </SfSpeechToText>
</div>

@code {
    // Requires Bootstrap Icons CSS to be included
    // Bootstrap icons are lightweight and modern
}
```

## Icon Position

The `IconPosition` enum controls where the icon appears relative to the button text.

### Icon Position Values

| Value | Position | Visual Effect |
|-------|----------|---------------|
| `Left` | Left of text | `[icon] Text` (Default) |
| `Right` | Right of text | `Text [icon]` |
| `Top` | Above text | `[icon]` + `Text` (stacked) |
| `Bottom` | Below text | `Text` + `[icon]` (stacked) |

### Left Position (Default)

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Icon Left (Default)</h3>
    
    <SfSpeechToText>
        <SpeechToTextButtonSettings
            Text="Listen"
            IconCss="e-icons e-microphone"
            IconPosition="Syncfusion.Blazor.Buttons.IconPosition.Left">
        </SpeechToTextButtonSettings>
    </SfSpeechToText>
</div>

@code {
    // Button displays: [microphone icon] Listen
}
```

### Right Position

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Icon Right</h3>
    
    <SfSpeechToText>
        <SpeechToTextButtonSettings
            Text="Listen"
            IconCss="e-icons e-microphone"
            IconPosition="Syncfusion.Blazor.Buttons.IconPosition.Right">
        </SpeechToTextButtonSettings>
    </SfSpeechToText>
</div>

@code {
    // Button displays: Listen [microphone icon]
}
```

### Top Position

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Icon Top</h3>
    
    <SfSpeechToText>
        <SpeechToTextButtonSettings
            Text="Listen"
            IconCss="e-icons e-microphone"
            IconPosition="Syncfusion.Blazor.Buttons.IconPosition.Top">
        </SpeechToTextButtonSettings>
    </SfSpeechToText>
</div>

@code {
    // Button displays icon above text (stacked vertically)
}
```

### Bottom Position

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Icon Bottom</h3>
    
    <SfSpeechToText>
        <SpeechToTextButtonSettings
            Text="Listen"
            IconCss="e-icons e-microphone"
            IconPosition="Syncfusion.Blazor.Buttons.IconPosition.Bottom">
        </SpeechToTextButtonSettings>
    </SfSpeechToText>
</div>

@code {
    // Button displays text above icon (stacked vertically)
}
```

## Primary Button Styling

The `IsPrimary` property applies primary button styling to make the SpeechToText button stand out.

### Primary Button Example

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px; display: flex; gap: 20px;">
    <div>
        <h4>Default Button</h4>
        <SfSpeechToText>
            <SpeechToTextButtonSettings
                Text="Listen"
                IconCss="e-icons e-microphone"
                IsPrimary="false">
            </SpeechToTextButtonSettings>
        </SfSpeechToText>
    </div>
    
    <div>
        <h4>Primary Button</h4>
        <SfSpeechToText>
            <SpeechToTextButtonSettings
                Text="Listen"
                IconCss="e-icons e-microphone"
                IsPrimary="true">
            </SpeechToTextButtonSettings>
        </SfSpeechToText>
    </div>
</div>

@code {
    // Primary button has distinct color/styling to draw attention
}
```

## Complete Customization Examples

### Example 1: Professional Voice Input Button

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px; max-width: 500px;">
    <h3>Professional Voice Input</h3>
    
    <SfSpeechToText 
        @bind-Transcript="@transcript"
        ShowTooltip="true">
        <SpeechToTextButtonSettings
            Text="🎤 Speak Now"
            StopStateText="⏸️ Recording..."
            IconCss="e-icons e-microphone"
            StopIconCss="e-icons e-stop"
            IconPosition="Syncfusion.Blazor.Buttons.IconPosition.Left"
            IsPrimary="true">
        </SpeechToTextButtonSettings>
        <SpeechToTextTooltipSettings
            Text="Click to start speaking"
            StopStateText="Click to stop recording">
        </SpeechToTextTooltipSettings>
    </SfSpeechToText>
    
    <div style="margin-top: 20px; padding: 10px; border: 1px solid #ddd; border-radius: 4px; min-height: 50px;">
        <strong>Transcript:</strong>
        <p>@transcript</p>
    </div>
</div>

@code {
    private string transcript = "";
}
```

### Example 2: Compact Icon-Only Button

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Compact Icon Button</h3>
    
    <SfSpeechToText 
        @bind-Transcript="@transcript">
        <SpeechToTextButtonSettings
            IconCss="e-icons e-microphone"
            StopIconCss="e-icons e-stop"
            IconPosition="Syncfusion.Blazor.Buttons.IconPosition.Left">
        </SpeechToTextButtonSettings>
    </SfSpeechToText>
    
    <div style="margin-top: 15px;">
        <span>You said: @transcript</span>
    </div>
</div>

@code {
    private string transcript = "";
}
```

### Example 3: Vertical Stack Layout

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px; display: flex; gap: 30px;">
    <div style="text-align: center;">
        <h4>Vertical Icon Top</h4>
        <SfSpeechToText>
            <SpeechToTextButtonSettings
                Text="Record"
                StopStateText="Stop"
                IconCss="e-icons e-microphone"
                StopIconCss="e-icons e-stop"
                IconPosition="Syncfusion.Blazor.Buttons.IconPosition.Top"
                IsPrimary="true">
            </SpeechToTextButtonSettings>
        </SfSpeechToText>
    </div>
    
    <div style="text-align: center;">
        <h4>Vertical Icon Bottom</h4>
        <SfSpeechToText>
            <SpeechToTextButtonSettings
                Text="Record"
                StopStateText="Stop"
                IconCss="e-icons e-microphone"
                StopIconCss="e-icons e-stop"
                IconPosition="Syncfusion.Blazor.Buttons.IconPosition.Bottom">
            </SpeechToTextButtonSettings>
        </SfSpeechToText>
    </div>
</div>

@code {
}
```

### Example 4: Disabled State

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <div style="margin-bottom: 20px;">
        <label>
            <input type="checkbox" @onchange="@((ChangeEventArgs e) => isDisabled = (bool)e.Value)" />
            Disable Speech-to-Text
        </label>
    </div>
    
    <SfSpeechToText 
        @bind-Transcript="@transcript"
        Disabled="@isDisabled">
        <SpeechToTextButtonSettings
            Text="🎤 Speak"
            StopStateText="⏹️ Stop"
            IconCss="e-icons e-microphone"
            StopIconCss="e-icons e-stop"
            IsPrimary="true">
        </SpeechToTextButtonSettings>
    </SfSpeechToText>
    
    <div style="margin-top: 15px;">
        <span>Status: @(isDisabled ? "Disabled" : "Enabled")</span>
    </div>
</div>

@code {
    private string transcript = "";
    private bool isDisabled = false;
}
```

## Advanced Patterns

### Pattern 1: Conditional Button Styling Based on State

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Conditional Styling</h3>
    
    <SfSpeechToText 
        @bind-Transcript="@transcript"
        ListeningState="@listeningState"
        SpeechRecognitionStarted="@OnListeningStarted"
        SpeechRecognitionStopped="@OnListeningStopped">
        <SpeechToTextButtonSettings
            Text="@(listeningState == SpeechToTextState.Listening ? "🔴 Recording" : "🎤 Record")"
            StopStateText="⏹️ Stop"
            IconCss="e-icons e-microphone"
            StopIconCss="e-icons e-stop"
            IsPrimary="@(listeningState == SpeechToTextState.Listening)">
        </SpeechToTextButtonSettings>
    </SfSpeechToText>
    
    <div style="margin-top: 15px; color: @(listeningState == SpeechToTextState.Listening ? "red" : "green");">
        <strong>State: @listeningState</strong>
    </div>
</div>

@code {
    private string transcript = "";
    private SpeechToTextState listeningState = SpeechToTextState.Inactive;
    
    private void OnListeningStarted(SpeechRecognitionStartedEventArgs args)
    {
        listeningState = args.State;
    }
    
    private void OnListeningStopped(SpeechRecognitionStoppedEventArgs args)
    {
        listeningState = args.State;
    }
}
```

### Pattern 2: Dynamic Icon Classes

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Dynamic Icons</h3>
    
    <div style="margin-bottom: 20px;">
        <label>Select Icon Library:</label>
        <select @onchange="@((ChangeEventArgs e) => selectedLibrary = e.Value?.ToString())" style="padding: 8px;">
            <option value="syncfusion">Syncfusion</option>
            <option value="fontawesome">Font Awesome</option>
            <option value="bootstrap">Bootstrap</option>
        </select>
    </div>
    
    <SfSpeechToText 
        @bind-Transcript="@transcript">
        <SpeechToTextButtonSettings
            Text="Listen"
            IconCss="@GetIconClass()"
            StopStateText="Stop"
            StopIconCss="@GetStopIconClass()"
            IsPrimary="true">
        </SpeechToTextButtonSettings>
    </SfSpeechToText>
</div>

@code {
    private string transcript = "";
    private string selectedLibrary = "syncfusion";
    
    private string GetIconClass()
    {
        return selectedLibrary switch
        {
            "fontawesome" => "fas fa-microphone",
            "bootstrap" => "bi bi-mic",
            _ => "e-icons e-microphone"
        };
    }
    
    private string GetStopIconClass()
    {
        return selectedLibrary switch
        {
            "fontawesome" => "fas fa-stop",
            "bootstrap" => "bi bi-stop",
            _ => "e-icons e-stop"
        };
    }
}
```

### Pattern 3: Multi-Language Button Text

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Multi-Language Support</h3>
    
    <div style="margin-bottom: 20px;">
        <label>Language:</label>
        <select @onchange="@((ChangeEventArgs e) => selectedLanguage = e.Value?.ToString())" style="padding: 8px;">
            <option value="en">English</option>
            <option value="es">Español</option>
            <option value="fr">Français</option>
            <option value="de">Deutsch</option>
        </select>
    </div>
    
    <SfSpeechToText 
        Language="@selectedLanguage"
        @bind-Transcript="@transcript">
        <SpeechToTextButtonSettings
            Text="@GetButtonText()"
            StopStateText="@GetStopButtonText()"
            IconCss="e-icons e-microphone"
            StopIconCss="e-icons e-stop">
        </SpeechToTextButtonSettings>
    </SfSpeechToText>
    
    <div style="margin-top: 15px;">
        <span>Transcript: @transcript</span>
    </div>
</div>

@code {
    private string transcript = "";
    private string selectedLanguage = "en";
    
    private string GetButtonText()
    {
        return selectedLanguage switch
        {
            "es" => "Hablar",
            "fr" => "Parler",
            "de" => "Sprechen",
            _ => "Listen"
        };
    }
    
    private string GetStopButtonText()
    {
        return selectedLanguage switch
        {
            "es" => "Parar",
            "fr" => "Arrêter",
            "de" => "Stoppen",
            _ => "Stop"
        };
    }
}
```

## Key Takeaways

✅ **Use `ButtonSettings` to:**
- Customize button text for different states
- Add icons from various icon libraries
- Control icon positioning relative to text
- Apply primary styling for emphasis
- Support multiple languages
- Create responsive, user-friendly interfaces

❌ **Avoid:**
- Using overly long text that might break button layout
- Mixing icon libraries without proper CSS
- Ignoring accessibility (always include text labels)
- Complex icon positioning without testing on different screen sizes

---
