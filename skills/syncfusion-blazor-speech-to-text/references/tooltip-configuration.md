# Tooltip Configuration

## Table of Contents
- [TooltipSettings Overview](#tooltip-settings-overview)
- [Tooltip Text Configuration](#tooltip-text-configuration)
- [Tooltip Position](#tooltip-position)
- [Enabling and Disabling Tooltips](#enabling-and-disabling-tooltips)
- [Position Reference Guide](#position-reference-guide)
- [Complete Examples](#complete-examples)
- [Best Practices](#best-practices)

## TooltipSettings Overview

The `TooltipSettings` property in `SfSpeechToText` allows you to customize the tooltip displayed when hovering over the microphone button. Tooltips provide helpful guidance to users and can be positioned in various locations around the button.

### Key Properties

| Property | Type | Default | Purpose |
|----------|------|---------|---------|
| `Text` | string | "Start Listening" | Tooltip text in start state |
| `StopStateText` | string | "Stop Listening" | Tooltip text in stop/listening state |
| `Position` | TooltipPosition | TopCenter | Where tooltip appears relative to button |

### ShowTooltip Control

The main component has a `ShowTooltip` property that enables/disables tooltips globally:

```razor
<SfSpeechToText ShowTooltip="true">  <!-- Enables tooltips -->
</SfSpeechToText>

<SfSpeechToText ShowTooltip="false"> <!-- Disables tooltips -->
</SfSpeechToText>
```

## Tooltip Text Configuration

### Basic Tooltip Text

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Custom Tooltip Text</h3>
    
    <SfSpeechToText ShowTooltip="true">
        <SpeechToTextTooltipSettings
            Text="Click here to start recording your voice"
            StopStateText="Click to stop the recording">
        </SpeechToTextTooltipSettings>
    </SfSpeechToText>
</div>

@code {
    // Hovering shows: "Click here to start recording your voice"
    // While listening shows: "Click to stop the recording"
}
```

### Multi-Language Tooltips

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Multi-Language Tooltips</h3>
    
    <div style="margin-bottom: 20px;">
        <label>Language:</label>
        <select @onchange="@((ChangeEventArgs e) => language = e.Value?.ToString())" style="padding: 8px;">
            <option value="en">English</option>
            <option value="es">Español</option>
            <option value="fr">Français</option>
        </select>
    </div>
    
    <SfSpeechToText Language="@language" ShowTooltip="true">
        <SpeechToTextTooltipSettings
            Text="@GetTooltipText()"
            StopStateText="@GetStopTooltipText()">
        </SpeechToTextTooltipSettings>
    </SfSpeechToText>
</div>

@code {
    private string language = "en";
    
    private string GetTooltipText()
    {
        return language switch
        {
            "es" => "Haz clic para grabar tu voz",
            "fr" => "Cliquez pour enregistrer votre voix",
            _ => "Click to record your voice"
        };
    }
    
    private string GetStopTooltipText()
    {
        return language switch
        {
            "es" => "Haz clic para detener la grabación",
            "fr" => "Cliquez pour arrêter l'enregistrement",
            _ => "Click to stop recording"
        };
    }
}
```

### Emoji and Special Characters in Tooltips

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Rich Tooltip Text</h3>
    
    <SfSpeechToText ShowTooltip="true">
        <SpeechToTextTooltipSettings
            Text="🎤 Click to start recording"
            StopStateText="⏹️ Click to stop recording">
        </SpeechToTextTooltipSettings>
    </SfSpeechToText>
</div>

@code {
    // Emojis can be used to provide visual context
}
```

## Tooltip Position

The `Position` property controls where the tooltip appears relative to the microphone button.

### Position Values Reference

| Value | Location | Visual Placement |
|-------|----------|-----------------|
| `TopLeft` | Top-left corner | Tooltip above-left of button |
| `TopCenter` | Top center | Tooltip directly above button (Default) |
| `TopRight` | Top-right corner | Tooltip above-right of button |
| `BottomLeft` | Bottom-left corner | Tooltip below-left of button |
| `BottomCenter` | Bottom center | Tooltip directly below button |
| `BottomRight` | Bottom-right corner | Tooltip below-right of button |
| `LeftTop` | Left-top | Tooltip left-top of button |
| `LeftCenter` | Left center | Tooltip directly left of button |
| `LeftBottom` | Left-bottom | Tooltip left-bottom of button |
| `RightTop` | Right-top | Tooltip right-top of button |
| `RightCenter` | Right center | Tooltip directly right of button |
| `RightBottom` | Right-bottom | Tooltip right-bottom of button |

### Top Positions

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 40px; display: flex; gap: 30px; justify-content: flex-start;">
    <div>
        <h4>Top-Left</h4>
        <SfSpeechToText ShowTooltip="true">
            <SpeechToTextTooltipSettings
                Position="Syncfusion.Blazor.Inputs.TooltipPosition.TopLeft"
                Text="Start Speaking">
            </SpeechToTextTooltipSettings>
        </SfSpeechToText>
    </div>
    
    <div>
        <h4>Top-Center (Default)</h4>
        <SfSpeechToText ShowTooltip="true">
            <SpeechToTextTooltipSettings
                Position="Syncfusion.Blazor.Inputs.TooltipPosition.TopCenter"
                Text="Start Speaking">
            </SpeechToTextTooltipSettings>
        </SfSpeechToText>
    </div>
    
    <div>
        <h4>Top-Right</h4>
        <SfSpeechToText ShowTooltip="true">
            <SpeechToTextTooltipSettings
                Position="Syncfusion.Blazor.Inputs.TooltipPosition.TopRight"
                Text="Start Speaking">
            </SpeechToTextTooltipSettings>
        </SfSpeechToText>
    </div>
</div>

@code {
}
```

### Bottom Positions

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 40px; display: flex; gap: 30px; justify-content: flex-start;">
    <div>
        <h4>Bottom-Left</h4>
        <SfSpeechToText ShowTooltip="true">
            <SpeechToTextTooltipSettings
                Position="Syncfusion.Blazor.Inputs.TooltipPosition.BottomLeft"
                Text="Start Speaking">
            </SpeechToTextTooltipSettings>
        </SfSpeechToText>
    </div>
    
    <div>
        <h4>Bottom-Center</h4>
        <SfSpeechToText ShowTooltip="true">
            <SpeechToTextTooltipSettings
                Position="Syncfusion.Blazor.Inputs.TooltipPosition.BottomCenter"
                Text="Start Speaking">
            </SpeechToTextTooltipSettings>
        </SfSpeechToText>
    </div>
    
    <div>
        <h4>Bottom-Right</h4>
        <SfSpeechToText ShowTooltip="true">
            <SpeechToTextTooltipSettings
                Position="Syncfusion.Blazor.Inputs.TooltipPosition.BottomRight"
                Text="Start Speaking">
            </SpeechToTextTooltipSettings>
        </SfSpeechToText>
    </div>
</div>

@code {
}
```

### Left Positions

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 40px; display: flex; gap: 50px; justify-content: flex-start;">
    <div>
        <h4>Left-Top</h4>
        <SfSpeechToText ShowTooltip="true">
            <SpeechToTextTooltipSettings
                Position="Syncfusion.Blazor.Inputs.TooltipPosition.LeftTop"
                Text="Listen">
            </SpeechToTextTooltipSettings>
        </SfSpeechToText>
    </div>
    
    <div style="margin-top: 20px;">
        <h4>Left-Center</h4>
        <SfSpeechToText ShowTooltip="true">
            <SpeechToTextTooltipSettings
                Position="Syncfusion.Blazor.Inputs.TooltipPosition.LeftCenter"
                Text="Listen">
            </SpeechToTextTooltipSettings>
        </SfSpeechToText>
    </div>
    
    <div>
        <h4>Left-Bottom</h4>
        <SfSpeechToText ShowTooltip="true">
            <SpeechToTextTooltipSettings
                Position="Syncfusion.Blazor.Inputs.TooltipPosition.LeftBottom"
                Text="Listen">
            </SpeechToTextTooltipSettings>
        </SfSpeechToText>
    </div>
</div>

@code {
}
```

### Right Positions

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 40px; display: flex; gap: 50px; justify-content: flex-start;">
    <div>
        <h4>Right-Top</h4>
        <SfSpeechToText ShowTooltip="true">
            <SpeechToTextTooltipSettings
                Position="Syncfusion.Blazor.Inputs.TooltipPosition.RightTop"
                Text="Record">
            </SpeechToTextTooltipSettings>
        </SfSpeechToText>
    </div>
    
    <div style="margin-top: 20px;">
        <h4>Right-Center</h4>
        <SfSpeechToText ShowTooltip="true">
            <SpeechToTextTooltipSettings
                Position="Syncfusion.Blazor.Inputs.TooltipPosition.RightCenter"
                Text="Record">
            </SpeechToTextTooltipSettings>
        </SfSpeechToText>
    </div>
    
    <div>
        <h4>Right-Bottom</h4>
        <SfSpeechToText ShowTooltip="true">
            <SpeechToTextTooltipSettings
                Position="Syncfusion.Blazor.Inputs.TooltipPosition.RightBottom"
                Text="Record">
            </SpeechToTextTooltipSettings>
        </SfSpeechToText>
    </div>
</div>

@code {
}
```

## Enabling and Disabling Tooltips

### Disable Tooltips Globally

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Tooltips Disabled</h3>
    
    <SfSpeechToText ShowTooltip="false">
        <SpeechToTextTooltipSettings
            Text="This won't show"
            StopStateText="Neither will this">
        </SpeechToTextTooltipSettings>
    </SfSpeechToText>
    
    <p style="color: #999; font-size: 12px;">Hover over button - no tooltip appears</p>
</div>

@code {
}
```

### Dynamic Tooltip Control

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Dynamic Tooltip Control</h3>
    
    <div style="margin-bottom: 20px;">
        <label>
            <input type="checkbox" @onchange="@((ChangeEventArgs e) => showTooltips = (bool)e.Value)" />
            Show Tooltips
        </label>
    </div>
    
    <SfSpeechToText ShowTooltip="@showTooltips">
        <SpeechToTextTooltipSettings
            Text="Click to start recording"
            StopStateText="Click to stop">
        </SpeechToTextTooltipSettings>
    </SfSpeechToText>
    
    <p style="margin-top: 15px; color: #666;">
        Tooltips: @(showTooltips ? "Enabled ✓" : "Disabled ✗")
    </p>
</div>

@code {
    private bool showTooltips = true;
}
```

## Position Reference Guide

### Choosing the Right Position

| Position | Best For | Example Use Case |
|----------|----------|------------------|
| `TopCenter` | General use | Default, most common, center alignment |
| `TopLeft` / `TopRight` | Responsive layouts | When button is near screen edges |
| `BottomCenter` | Top-heavy UI | When space above button is limited |
| `LeftCenter` / `RightCenter` | Horizontal layouts | When space above/below is limited |
| `TopLeft` | Corner positioning | When button is in top-right corner |
| `BottomRight` | Corner positioning | When button is in top-left corner |

### Responsive Tooltip Position Selection

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Responsive Tooltip Positioning</h3>
    
    <div style="margin-bottom: 20px;">
        <label>Button Position:</label>
        <select @onchange="@((ChangeEventArgs e) => buttonPosition = e.Value?.ToString())" style="padding: 8px;">
            <option value="top-left">Top-Left</option>
            <option value="top-right">Top-Right</option>
            <option value="bottom-left">Bottom-Left</option>
            <option value="bottom-right">Bottom-Right</option>
        </select>
    </div>
    
    <div style="@GetButtonPositionStyle()">
        <SfSpeechToText ShowTooltip="true">
            <SpeechToTextTooltipSettings
                Position="@GetTooltipPosition()"
                Text="Start Recording">
            </SpeechToTextTooltipSettings>
        </SfSpeechToText>
    </div>
</div>

@code {
    private string buttonPosition = "top-left";
    
    private string GetButtonPositionStyle()
    {
        return buttonPosition switch
        {
            "top-right" => "position: absolute; top: 50px; right: 20px;",
            "bottom-left" => "position: absolute; bottom: 50px; left: 20px;",
            "bottom-right" => "position: absolute; bottom: 50px; right: 20px;",
            _ => "position: absolute; top: 50px; left: 20px;"
        };
    }
    
    private Syncfusion.Blazor.Inputs.TooltipPosition GetTooltipPosition()
    {
        return buttonPosition switch
        {
            "top-right" => Syncfusion.Blazor.Inputs.TooltipPosition.TopLeft,
            "bottom-left" => Syncfusion.Blazor.Inputs.TooltipPosition.TopCenter,
            "bottom-right" => Syncfusion.Blazor.Inputs.TooltipPosition.TopLeft,
            _ => Syncfusion.Blazor.Inputs.TooltipPosition.TopRight
        };
    }
}
```

## Complete Examples

### Example 1: Professional Setup

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px; max-width: 600px;">
    <h3>Professional Voice Input</h3>
    
    <SfSpeechToText 
        @bind-Transcript="@transcript"
        ShowTooltip="true"
        AllowInterimResults="true">
        <SpeechToTextButtonSettings
            Text="🎤 Listen"
            StopStateText="⏹️ Stop"
            IconCss="e-icons e-microphone"
            StopIconCss="e-icons e-stop"
            IsPrimary="true">
        </SpeechToTextButtonSettings>
        <SpeechToTextTooltipSettings
            Text="Click to start speaking clearly"
            StopStateText="Click to stop recording"
            Position="Syncfusion.Blazor.Inputs.TooltipPosition.TopCenter">
        </SpeechToTextTooltipSettings>
    </SfSpeechToText>
    
    <div style="margin-top: 20px; padding: 15px; border: 1px solid #ddd; border-radius: 4px;">
        <strong>Transcribed Text:</strong>
        <p style="margin: 10px 0 0 0;">@transcript</p>
    </div>
</div>

@code {
    private string transcript = "";
}
```

### Example 2: Compact Inline Button

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Quick Voice Input</h3>
    
    <div style="display: flex; align-items: center; gap: 10px;">
        <input type="text" @bind="@transcript" style="padding: 8px; flex: 1; border: 1px solid #ccc; border-radius: 4px;" />
        
        <SfSpeechToText 
            @bind-Transcript="@transcript"
            ShowTooltip="true">
            <SpeechToTextButtonSettings
                IconCss="e-icons e-microphone"
                StopIconCss="e-icons e-stop">
            </SpeechToTextButtonSettings>
            <SpeechToTextTooltipSettings
                Text="Voice input"
                Position="Syncfusion.Blazor.Inputs.TooltipPosition.TopCenter">
            </SpeechToTextTooltipSettings>
        </SfSpeechToText>
    </div>
</div>

@code {
    private string transcript = "";
}
```

### Example 3: Multi-Position Showcase

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 40px;">
    <h3>Tooltip Position Showcase</h3>
    
    <div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 30px;">
        @foreach(var pos in new[] { 
            ("TopCenter", Syncfusion.Blazor.Inputs.TooltipPosition.TopCenter),
            ("BottomCenter", Syncfusion.Blazor.Inputs.TooltipPosition.BottomCenter),
            ("LeftCenter", Syncfusion.Blazor.Inputs.TooltipPosition.LeftCenter),
            ("RightCenter", Syncfusion.Blazor.Inputs.TooltipPosition.RightCenter)
        })
        {
            <div style="text-align: center; padding: 20px; border: 1px solid #eee; border-radius: 4px;">
                <p style="font-weight: bold; margin-bottom: 15px;">@pos.Item1</p>
                <SfSpeechToText ShowTooltip="true">
                    <SpeechToTextTooltipSettings
                        Position="@pos.Item2"
                        Text="@pos.Item1">
                    </SpeechToTextTooltipSettings>
                </SfSpeechToText>
            </div>
        }
    </div>
</div>

@code {
}
```

## Best Practices

### ✅ DO

- **Provide clear, concise tooltip text** - Users should understand the action at a glance
- **Use consistent positioning** - Keep tooltips in the same position throughout your app
- **Position tooltips away from screen edges** - Tooltips should be fully visible
- **Keep tooltip text short** - Single line messages are best
- **Use tooltips for additional context** - Don't repeat button text verbatim
- **Test on different screen sizes** - Ensure tooltips fit on mobile devices
- **Use emojis strategically** - Add visual context without cluttering

### ❌ DON'T

- **Use overly long tooltip text** - Breaks layout and hides important information
- **Change tooltip position dynamically** - Creates confusing UX
- **Use tooltips as primary instructions** - Users may not discover hover tooltips
- **Disable tooltips without good reason** - They improve usability
- **Use same text for tooltip and button** - Provides no additional value
- **Ignore accessibility** - Ensure tooltips are readable
- **Use tooltips on touch devices exclusively** - Touch users can't hover

### Accessibility Considerations

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <h3>Accessible Tooltip Setup</h3>
    
    <SfSpeechToText 
        ID="speechToTextControl"
        ShowTooltip="true"
        @bind-Transcript="@transcript">
        <SpeechToTextButtonSettings
            Text="🎤 Listen"
            StopStateText="⏹️ Stop"
            IconCss="e-icons e-microphone"
            StopIconCss="e-icons e-stop"
            IsPrimary="true">
        </SpeechToTextButtonSettings>
        <SpeechToTextTooltipSettings
            Text="Press to start voice recording"
            StopStateText="Press to stop recording"
            Position="Syncfusion.Blazor.Inputs.TooltipPosition.TopCenter">
        </SpeechToTextTooltipSettings>
    </SfSpeechToText>
    
    <ul style="margin-top: 20px; color: #666; font-size: 12px;">
        <li>Tooltips help keyboard-only users</li>
        <li>Clear text benefits screen readers</li>
        <li>Proper positioning improves mobile UX</li>
    </ul>
</div>

@code {
    private string transcript = "";
}
```

---