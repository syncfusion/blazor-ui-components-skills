# Transcript and Language Configuration

## Table of Contents
- [Retrieving Transcripts](#retrieving-transcripts)
- [Two-Way Binding](#two-way-binding)
- [Setting Language](#setting-language)
- [Supported Languages](#supported-languages)
- [Language Switching](#language-switching)
- [Multi-Language Examples](#multi-language-examples)

## Retrieving Transcripts

The `Transcript` property allows you to access the transcribed text from speech recognition. This property is updated automatically as the user speaks or completes their speech.

### Basic Transcript Access

```razor
@using Syncfusion.Blazor.Inputs

<SfSpeechToText @bind-Transcript="@transcript"></SfSpeechToText>
<p>You said: @transcript</p>

@code {
    private string transcript = "";
}
```

**How it works:**
1. User clicks the microphone button
2. Speaks into the microphone
3. `transcript` variable updates automatically with recognized text
4. Display updates to show the transcribed text

### Reading Transcript from Template

You can display the transcript in various ways:

```razor
@using Syncfusion.Blazor.Inputs

<div style="padding: 20px;">
    <SfSpeechToText @bind-Transcript="@transcript"></SfSpeechToText>
    
    @if (string.IsNullOrEmpty(transcript))
    {
        <p style="color: gray;">No speech recognized yet. Click the microphone to start.</p>
    }
    else
    {
        <div style="background: #f5f5f5; padding: 15px; margin-top: 20px; border-radius: 4px;">
            <h4>Transcribed Text:</h4>
            <p>@transcript</p>
            <small>Length: @transcript.Length characters</small>
        </div>
    }
</div>

@code {
    private string transcript = "";
}
```

## Two-Way Binding

The `@bind-Transcript` directive creates a two-way binding, meaning:
- Changes from speech recognition update the `transcript` variable
- Changes to the `transcript` variable (programmatically) would reflect in the component

### Practical Two-Way Binding Example

```razor
@using Syncfusion.Blazor.Inputs

<div style="max-width: 600px;">
    <h3>Voice-Enabled Form</h3>
    
    <div style="margin-bottom: 20px;">
        <label>Name (or speak):</label>
        <SfSpeechToText @bind-Transcript="@formData.Name" Language="en-US"></SfSpeechToText>
        <input type="text" @bind="@formData.Name" placeholder="Or type here..." style="width: 100%; padding: 8px; margin-top: 5px;" />
    </div>
    
    <div style="margin-bottom: 20px;">
        <label>Email:</label>
        <input type="email" @bind="@formData.Email" placeholder="Email address..." style="width: 100%; padding: 8px;" />
    </div>
    
    <div style="margin-bottom: 20px;">
        <label>Message (or speak):</label>
        <SfSpeechToText @bind-Transcript="@formData.Message" Language="en-US"></SfSpeechToText>
        <textarea @bind="@formData.Message" rows="4" placeholder="Or type here..." style="width: 100%; padding: 8px; margin-top: 5px;"></textarea>
    </div>
    
    <button @onclick="SubmitForm" style="padding: 10px 20px; background: #4CAF50; color: white; border: none; border-radius: 4px; cursor: pointer;">Submit</button>
</div>

@code {
    private FormData formData = new FormData();
    
    private void SubmitForm()
    {
        Console.WriteLine($"Name: {formData.Name}");
        Console.WriteLine($"Email: {formData.Email}");
        Console.WriteLine($"Message: {formData.Message}");
    }
    
    public class FormData
    {
        public string Name { get; set; } = "";
        public string Email { get; set; } = "";
        public string Message { get; set; } = "";
    }
}
```

## Setting Language

The `Language` property specifies which language the speech recognition engine should use. This ensures accurate transcription based on the spoken language.

### Basic Language Setting

```razor
@using Syncfusion.Blazor.Inputs

<SfSpeechToText Language="fr-FR" @bind-Transcript="@transcript"></SfSpeechToText>
<p>Speaking French: @transcript</p>

@code {
    private string transcript = "";
}
```

### Common Language Codes

```razor
<!-- English (United States) -->
<SfSpeechToText Language="en-US"></SfSpeechToText>

<!-- English (British) -->
<SfSpeechToText Language="en-GB"></SfSpeechToText>

<!-- French (France) -->
<SfSpeechToText Language="fr-FR"></SfSpeechToText>

<!-- German (Germany) -->
<SfSpeechToText Language="de-DE"></SfSpeechToText>

<!-- Spanish (Spain) -->
<SfSpeechToText Language="es-ES"></SfSpeechToText>

<!-- Italian (Italy) -->
<SfSpeechToText Language="it-IT"></SfSpeechToText>

<!-- Chinese (Mandarin - Simplified) -->
<SfSpeechToText Language="zh-CN"></SfSpeechToText>

<!-- Japanese (Japan) -->
<SfSpeechToText Language="ja-JP"></SfSpeechToText>

<!-- Korean (Korea) -->
<SfSpeechToText Language="ko-KR"></SfSpeechToText>

<!-- Portuguese (Brazil) -->
<SfSpeechToText Language="pt-BR"></SfSpeechToText>
```

## Supported Languages

The SpeechToText component supports 100+ language and locale combinations. Here's a comprehensive list of commonly supported languages:

### European Languages
- **English:** en-US, en-GB, en-AU, en-CA, en-IN, en-NZ, en-ZA
- **French:** fr-FR, fr-CA, fr-BE, fr-CH
- **German:** de-DE, de-AT, de-CH
- **Spanish:** es-ES, es-MX, es-AR, es-CO, es-PE
- **Italian:** it-IT, it-CH
- **Portuguese:** pt-PT, pt-BR
- **Dutch:** nl-NL, nl-BE
- **Polish:** pl-PL
- **Russian:** ru-RU, ru-BY, ru-KZ
- **Turkish:** tr-TR
- **Greek:** el-GR
- **Swedish:** sv-SE
- **Norwegian:** nb-NO, nn-NO
- **Danish:** da-DK
- **Finnish:** fi-FI

### Asian Languages
- **Chinese:** zh-CN (Mandarin Simplified), zh-TW (Mandarin Traditional), zh-HK (Cantonese)
- **Japanese:** ja-JP
- **Korean:** ko-KR
- **Hindi:** hi-IN
- **Thai:** th-TH
- **Vietnamese:** vi-VN
- **Filipino:** fil-PH
- **Indonesian:** id-ID
- **Malay:** ms-MY, ms-BN
- **Myanmar:** my-MM
- **Burmese:** my-MM

### Middle Eastern Languages
- **Arabic:** ar-AE, ar-SA, ar-EG, ar-KW, ar-QA
- **Hebrew:** he-IL
- **Persian/Farsi:** fa-IR
- **Turkish:** tr-TR
- **Urdu:** ur-PK

### African Languages
- **Afrikaans:** af-ZA
- **Zulu:** zu-ZA
- **Xhosa:** xh-ZA

## Language Switching

Allow users to dynamically change the language:

```razor
@using Syncfusion.Blazor.Inputs

<div style="max-width: 600px; margin: 20px auto;">
    <h3>Multi-Language Speech Recognition</h3>
    
    <div style="margin-bottom: 20px;">
        <label style="display: block; margin-bottom: 5px; font-weight: bold;">Select Language:</label>
        <select @onchange="@OnLanguageChanged" value="@selectedLanguage" style="padding: 8px; font-size: 14px; width: 200px;">
            <option value="en-US">English (US)</option>
            <option value="en-GB">English (UK)</option>
            <option value="fr-FR">Français (France)</option>
            <option value="de-DE">Deutsch (Deutschland)</option>
            <option value="es-ES">Español (España)</option>
            <option value="it-IT">Italiano (Italia)</option>
            <option value="pt-BR">Português (Brasil)</option>
            <option value="zh-CN">中文 (简体)</option>
            <option value="ja-JP">日本語 (日本)</option>
            <option value="ko-KR">한국어 (대한민국)</option>
        </select>
    </div>
    
    <div style="padding: 15px; background: #f9f9f9; border-radius: 4px; margin-bottom: 20px;">
        <p style="margin: 0 0 10px 0; color: #666;">Current language: <strong>@GetLanguageName(selectedLanguage)</strong></p>
        <p style="margin: 0; color: #999; font-size: 13px;">Speak in this language for accurate transcription</p>
    </div>
    
    <SfSpeechToText 
        Language="@selectedLanguage" 
        @bind-Transcript="@transcript">
    </SfSpeechToText>
    
    @if (!string.IsNullOrEmpty(transcript))
    {
        <div style="margin-top: 20px; padding: 15px; background: #e8f5e9; border-radius: 4px;">
            <h4>Result:</h4>
            <p>@transcript</p>
        </div>
    }
</div>

@code {
    private string selectedLanguage = "en-US";
    private string transcript = "";
    
    private void OnLanguageChanged(ChangeEventArgs e)
    {
        selectedLanguage = e.Value?.ToString() ?? "en-US";
        transcript = ""; // Clear transcript when language changes
    }
    
    private string GetLanguageName(string languageCode)
    {
        return languageCode switch
        {
            "en-US" => "English (US)",
            "en-GB" => "English (UK)",
            "fr-FR" => "French (France)",
            "de-DE" => "German (Germany)",
            "es-ES" => "Spanish (Spain)",
            "it-IT" => "Italian (Italy)",
            "pt-BR" => "Portuguese (Brazil)",
            "zh-CN" => "Chinese (Simplified)",
            "ja-JP" => "Japanese",
            "ko-KR" => "Korean",
            _ => languageCode
        };
    }
}
```

## Multi-Language Examples

### Example 1: Language Preference Storage

```razor
@using Syncfusion.Blazor.Inputs

<div>
    <div style="margin-bottom: 15px;">
        <label>Preferred Language:</label>
        <select @onchange="@OnLanguageChanged" style="padding: 8px;">
            <option value="en-US">English</option>
            <option value="fr-FR">Français</option>
            <option value="es-ES">Español</option>
        </select>
    </div>
    
    <SfSpeechToText Language="@userPreferredLanguage" @bind-Transcript="@transcript"></SfSpeechToText>
    <p>@transcript</p>
</div>

@code {
    private string userPreferredLanguage = "en-US";
    private string transcript = "";
    
    private void OnLanguageChanged(ChangeEventArgs e)
    {
        userPreferredLanguage = e.Value?.ToString() ?? "en-US";
        // In production, save to localStorage or database
        SaveUserPreference(userPreferredLanguage);
    }
    
    private void SaveUserPreference(string language)
    {
        // Example: Save to local storage or user profile
        Console.WriteLine($"Language preference saved: {language}");
    }
}
```

### Example 2: Auto-Detect and Fallback

```razor
@using Syncfusion.Blazor.Inputs

<div>
    <p>Current language: @effectiveLanguage</p>
    <SfSpeechToText Language="@effectiveLanguage" @bind-Transcript="@transcript"></SfSpeechToText>
    <p>@transcript</p>
</div>

@code {
    private string effectiveLanguage = "en-US";
    private string transcript = "";
    
    protected override void OnInitialized()
    {
        // Example: Try to detect browser language, fallback to en-US
        string browserLanguage = GetBrowserLanguage();
        effectiveLanguage = IsLanguageSupported(browserLanguage) ? browserLanguage : "en-US";
    }
    
    private string GetBrowserLanguage()
    {
        // In a real implementation, you'd get this from JavaScript interop
        return "en-US";
    }
    
    private bool IsLanguageSupported(string language)
    {
        // Check if language is in the supported list
        return language.StartsWith("en") || language.StartsWith("fr") || language.StartsWith("es");
    }
}
```

## Best Practices

1. **Always specify Language explicitly** - Don't rely on browser defaults
2. **Store user preference** - Remember the user's language choice
3. **Provide language selection** - Let users change language easily
4. **Clear text on language change** - Avoid confusion from mixed-language transcripts
5. **Test with native speakers** - Verify accuracy before deploying
