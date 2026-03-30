---
name: syncfusion-blazor-smart-textarea
description: Set up and configure the Syncfusion Blazor Smart TextArea for AI-powered inline or popup text autocompletion. Covers OpenAI, Azure OpenAI, Ollama, and custom IChatInferenceService backends, plus UserRole, UserPhrases, and suggestion display mode customization.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Syncfusion Blazor Smart TextArea

AI-powered text input component that provides context-aware autocompletion suggestions inline or in a popup, driven by OpenAI, Azure OpenAI, Ollama, or custom AI backends.

## When to Use

Use this skill when you need to:
- Add `SfSmartTextArea` to a Blazor Web App for AI-assisted text input
- Configure OpenAI, Azure OpenAI, Ollama, or a custom `IChatInferenceService`
- Switch between inline and popup suggestion display modes
- Tune suggestions with `UserRole` and `UserPhrases` context
- Implement a custom AI backend for Claude, DeepSeek, Groq, Gemini, etc.
- Troubleshoot missing suggestions or AI connectivity issues

## Prerequisites

- Blazor Web App (.NET 8.0+) with **Interactive Server** render mode
- Visual Studio 2022 or Visual Studio Code
- Active OpenAI / Azure OpenAI account, Ollama instance, or custom AI backend
- Basic knowledge of Blazor and C#

## Core Documentation

### 1. Getting Started with Smart TextArea
Complete setup guide including prerequisites, NuGet package installation, service registration, and component implementation.

→ [Full Getting Started Guide](./references/getting-started.md)

**Key Topics:**
- Creating a new Blazor Web App
- Registering Syncfusion services
- Configuring AI services (OpenAI, Azure OpenAI, Ollama)
- Adding stylesheets and scripts
- Implementing the first Smart TextArea component

### 2. Customization and Features
Guidance on customizing suggestion appearance and leveraging inherited TextArea features.

→ [Full Customization & Features Guide](./references/customization-features.md)

**Key Topics:**
- Popup vs inline suggestion display modes
- UserRole and UserPhrases configuration
- Inherited TextArea features (forms, floating labels, events, methods, styling)
- Best practices and complete examples

### 3. Custom AI Service Integration
Detailed procedures for implementing custom AI backends beyond the default providers.

→ [Full Custom AI Integration Guide](https://blazor.syncfusion.com/documentation/smart-textarea/custom-inference-backend)

**Key Topics:**
- `IChatInferenceService` interface overview
- Mock AI service examples
- Integration with Claude, DeepSeek, Groq, Gemini
- Service registration patterns
- Testing and troubleshooting custom services

## Quick Start Checklist

```
□ Ensure Blazor Web App is created with Server Interactivity mode
□ Install required NuGet packages (SmartComponents, Themes)
□ Register Syncfusion Blazor service in Program.cs
□ Register and configure AI service (OpenAI/Azure/Ollama/Custom)
□ Add stylesheet and script references to App.razor
□ Add SfSmartTextArea component with UserRole
□ Test with Ctrl+F5 (Windows) or ⌘+F5 (macOS)
```

## Key Concepts

| Concept | Reference |
|---------|-----------|
| **UserRole** | Defines context for AI autocompletion suggestions |
| **UserPhrases** | Optional predefined expressions aligned with tone/usage patterns |
| **ShowSuggestionOnPopup** | Controls display mode: `true` (popup) or `false` (inline, default) |
| **IChatInferenceService** | Interface for implementing custom AI backends |
| **ChatParameters** | Standardized input for AI response generation |

## NuGet Packages

**Core Packages:**
- `Syncfusion.Blazor.SmartComponents` (v33.1.44+)
- `Syncfusion.Blazor.Themes` (v33.1.44+)

**AI Service Packages (based on provider):**
- OpenAI: `Microsoft.Extensions.AI`, `Microsoft.Extensions.AI.OpenAI`
- Azure OpenAI: `Microsoft.Extensions.AI`, `Microsoft.Extensions.AI.OpenAI`, `Azure.AI.OpenAI`
- Ollama: `Microsoft.Extensions.AI`, `OllamaSharp`
