````markdown
# Chain of Thoughts

## Table of Contents
- [Overview](#overview)
- [Types of Response Blocks](#types-of-response-blocks)
- [Configure the Thinking Block](#configure-the-thinking-block)
  - [ThinkingBlock Properties](#thinkingblock-properties)
  - [Adding Stages](#adding-stages)
  - [Stage Status](#stage-status)
  - [Inline Context (EditableContext)](#inline-context-editablecontext)
  - [Multi-Step Streaming Pattern](#multi-step-streaming-pattern)
- [Configure Thinking Stage Template](#configure-thinking-stage-template)
- [Best Practices](#best-practices)
- [See Also](#see-also)

## Overview

The AI AssistView supports rendering **Chain of Thoughts** (also called `Thinking`) blocks, allowing you to visualize the model's reasoning process step by step before the final response is generated. The feature is ideal for extended reasoning models (such as Claude 3.5, GPT‑o1, and similar), which expose intermediate reasoning stages.

A response can mix `ThinkingBlock`, `TextBlock`, and `ToolBlock` items inside the `Blocks` collection; they are rendered in the order they appear.

## Types of Response Blocks

The AI AssistView supports the following block types in a single response:

| Type | Purpose |
|------|---------|
| `TextBlock` | A block of plain or markdown text content. |
| `ToolBlock` | A reference to a custom tool registered via `ToolUIBlock`. |
| `ThinkingBlock` | A collapsible block that visualizes reasoning stages before the final response. |

> When only `Blocks` are provided (without `Response` text), the component renders the blocks directly and skips the default text-response rendering path. When both `Blocks` and `Response` are provided, the blocks are rendered first, followed by the response text.

## Configure the Thinking Block

You can add a `ThinkingBlock` to the `Blocks` collection of an `AssistViewPrompt` or set it through `args.Blocks` in the `PromptRequested` event. This allows you to display reasoning steps, progress indicators, and contextual information before the response content.

### ThinkingBlock Properties

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `ID` | `string` | auto-generated | Unique identifier for the block, used for collapsing/expanding state. |
| `BlockType` | `BlockType` | — | Must be set to `BlockType.Thinking`. Identifies this block as a thinking block. Required. |
| `Title` | `string` | `'Thinking...'` | Heading text shown in the collapsible header. |
| `Content` | `string` | — | Markdown text rendered as a description beneath the stages. |
| `IsActive` | `bool` | `false` | When `true`, a Syncfusion spinner is shown inside the thinking header to indicate the reasoning is still in progress. |
| `Collapsed` | `bool` | `true` | Initial collapsed state of the thinking block. |
| `Collapsible` | `bool` | `true` | Whether the block can be expanded or collapsed by the user. |
| `Stages` | `List<ThinkingStage>` | — | Array of reasoning stages rendered using the Timeline component. |

### Basic Example

```cshtml

@using Syncfusion.Blazor.InteractiveChat

<div style="height:350px; width:650px;">
    <SfAIAssistView Prompts="@PromptCollection"
                    PromptRequested="@PromptRequest">
    </SfAIAssistView>
</div>

@code {
    private List<AssistViewPrompt> PromptCollection = new()
    {
        new AssistViewPrompt
        {
            Prompt = "Explain the water cycle.",
            Response = "The water cycle describes how water moves continuously through the environment via evaporation, condensation, and precipitation.",
            Blocks = new List<IResponseBlock>
            {
                new ThinkingBlock
                {
                    BlockType = BlockType.Thinking,
                    Title = "Reasoning",
                    Collapsed = true,
                    Collapsible = true,
                    IsActive = false,
                    Stages = new List<ThinkingStage>
                    {
                        new ThinkingStage
                        {
                            ID = "Step1",
                            Status = ThinkingStageStatus.Completed,
                            Content = "Identified request as a water cycle explanation."
                        },
                        new ThinkingStage
                        {
                            ID = "Step2",
                            Status = ThinkingStageStatus.Completed,
                            Content = "Summarized key stages concisely."
                        },
                        new ThinkingStage
                        {
                            ID = "Step3",
                            Status = ThinkingStageStatus.Completed,
                            Content = "Composed a clear single-paragraph response."
                        }
                    }
                }
            }
        }
    };

    private async Task PromptRequest(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "For real-time prompt processing, connect the AI AssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.";
    }
}

```

![Blazor AI AssistView Thinking Block Configure](./images/chain-of-thoughts-config.webp)

## Adding Stages

Each entry in the `Stages` array represents a single reasoning step. The following properties are available on `ThinkingStage`:

| Property | Type | Description |
|----------|------|-------------|
| `ID` | `string` | Unique identifier for the stage. |
| `Content` | `string` | Markdown content for this stage. Supports `{index}` placeholders for inline context items. |
| `Status` | `ThinkingStageStatus` | Controls the icon/spinner shown on the timeline dot. One of `Completed`, `InProgress`, `Failed`. |
| `IconCss` | `string` | Custom CSS class for the timeline dot icon; overrides the default status icon. |
| `EditableContext` | `List<ThinkingContextItem>` | Inline context items injected into the stage content via `{index}` placeholders. |

### Stage Status

Each thinking stage carries a `Status` value that controls the visual indicator on its timeline dot:

- **`Completed`** — renders a check icon (`e-check`).
- **`InProgress`** — renders an animated spinner.
- **`Failed`** — renders an error/cross icon (`e-error-treeview`).

Use this to reflect real-time reasoning progress when streaming multi-step responses.

### Inline Context (EditableContext)

`EditableContext` lets you inject badges into a stage's content. The stage's `Content` string uses zero-based `{0}`, `{1}`, … placeholders, and each placeholder is replaced with the corresponding `ThinkingContextItem`'s `Name` rendered as an interactive chip.

`ThinkingContextItem` properties:

| Property | Type | Description |
|----------|------|-------------|
| `Type` | `ThinkingContextType` | `Tool` or `Context` — controls how the chip is styled. |
| `Name` | `string` | Short label rendered inside the chip. |
| `Value` | `string` | Full description of the item (typically shown on hover). |

```csharp
new ThinkingStage
{
    Status = ThinkingStageStatus.InProgress,
    Content = "Selected {0}, {1}, and {2} for dashboard layout.",
    EditableContext = new List<ThinkingContextItem>
    {
        new ThinkingContextItem { Type = ThinkingContextType.Tool, Name = "Charts", Value = "Analytics visualization" },
        new ThinkingContextItem { Type = ThinkingContextType.Tool, Name = "Grid", Value = "Tabular data" },
        new ThinkingContextItem { Type = ThinkingContextType.Tool, Name = "Cards", Value = "KPI metrics" }
    }
}
```

Renders as: `Selected [Charts] [Grid] [Cards] for dashboard layout.`

### Multi-Step Streaming Pattern

The most common use case is to **append thinking blocks progressively** as the AI model reports each reasoning stage. Each step mutates `args.Blocks` to reflect the current state — the most recent reasoning block has `IsActive = true` and `Collapsed = false`; completed reasoning blocks have `IsActive = false` and `Collapsed = true`. The final `args.Response` is set on the last step.

```cshtml

@using Syncfusion.Blazor.InteractiveChat

<div style="height:350px; width:650px;">
    <SfAIAssistView PromptSuggestions="@PromptSuggestions"
                    PromptRequested="@PromptRequest">
    </SfAIAssistView>
</div>

@code {
    private List<string> PromptSuggestions = new()
    {
        "Build a modern dashboard for my business",
        "Create a login page with validation",
        "Make a task management board"
    };

    private async Task PromptRequest(AssistViewPromptRequestedEventArgs args)
    {
        // Step 1
        args.Blocks = new List<IResponseBlock>
        {
            new ThinkingBlock
            {
                BlockType = BlockType.Thinking,
                Title = "Understanding your request",
                Collapsible = true,
                Collapsed = false,
                IsActive = true,
                Stages = new List<ThinkingStage>
                {
                    new ThinkingStage
                    {
                        Status = ThinkingStageStatus.InProgress,
                        Content = "Identified request as a business dashboard requirement."
                    }
                }
            }
        };
        args.Response = "";
        await Task.Delay(1000);

        // Step 2
        args.Blocks = new List<IResponseBlock>
        {
            new ThinkingBlock
            {
                BlockType = BlockType.Thinking,
                Title = "Understanding your request",
                Collapsible = true,
                Collapsed = true,
                IsActive = false,
                Stages = new List<ThinkingStage>
                {
                    new ThinkingStage
                    {
                        Status = ThinkingStageStatus.Completed,
                        Content = "Identified request as a business dashboard requirement."
                    }
                }
            },
            new ThinkingBlock
            {
                BlockType = BlockType.Thinking,
                Title = "Selecting UI components",
                Collapsible = true,
                Collapsed = false,
                IsActive = true,
                Stages = new List<ThinkingStage>
                {
                    new ThinkingStage
                    {
                        Status = ThinkingStageStatus.InProgress,
                        IconCss = "e-icons e-check",
                        Content = "Selected {0}, {1}, and {2} for dashboard layout.",
                        EditableContext = new List<ThinkingContextItem>
                        {
                            new ThinkingContextItem { Type = ThinkingContextType.Tool, Name = "Charts", Value = "Analytics visualization" },
                            new ThinkingContextItem { Type = ThinkingContextType.Tool, Name = "Grid", Value = "Tabular data" },
                            new ThinkingContextItem { Type = ThinkingContextType.Tool, Name = "Cards", Value = "KPI metrics" }
                        }
                    }
                }
            }
        };
        args.Response = "";
        await Task.Delay(1000);

        // Step 3
        args.Blocks = new List<IResponseBlock>
        {
            new ThinkingBlock
            {
                BlockType = BlockType.Thinking,
                Title = "Understanding your request",
                Collapsible = true,
                Collapsed = true,
                IsActive = false,
                Stages = new List<ThinkingStage>
                {
                    new ThinkingStage
                    {
                        Status = ThinkingStageStatus.Completed,
                        Content = "Identified request as a business dashboard requirement.",
                        IconCss = "e-icons e-check"
                    }
                }
            },
            new ThinkingBlock
            {
                BlockType = BlockType.Thinking,
                Title = "Selecting UI components",
                Collapsible = true,
                Collapsed = true,
                IsActive = false,
                Stages = new List<ThinkingStage>
                {
                    new ThinkingStage
                    {
                        Status = ThinkingStageStatus.Completed,
                        IconCss = "e-icons e-check",
                        Content = "Selected {0}, {1}, and {2} for dashboard layout.",
                        EditableContext = new List<ThinkingContextItem>
                        {
                            new ThinkingContextItem { Type = ThinkingContextType.Tool, Name = "Charts", Value = "Analytics visualization" },
                            new ThinkingContextItem { Type = ThinkingContextType.Tool, Name = "Grid", Value = "Tabular data" },
                            new ThinkingContextItem { Type = ThinkingContextType.Tool, Name = "Cards", Value = "KPI metrics" }
                        }
                    }
                }
            },
            new ThinkingBlock
            {
                BlockType = BlockType.Thinking,
                Title = "Designing layout structure",
                Collapsible = true,
                Collapsed = false,
                IsActive = true,
                Stages = new List<ThinkingStage>
                {
                    new ThinkingStage
                    {
                        Status = ThinkingStageStatus.InProgress,
                        IconCss = "e-icons e-check",
                        Content = "Created responsive {0} layout structure.",
                        EditableContext = new List<ThinkingContextItem>
                        {
                            new ThinkingContextItem { Type = ThinkingContextType.Context, Name = "12-column", Value = "12-column grid" }
                        }
                    }
                }
            }
        };
        args.Response = "";
        await Task.Delay(1000);

        // Step 4 (FINAL RESPONSE)
        args.Blocks = new List<IResponseBlock>
        {
            new ThinkingBlock
            {
                BlockType = BlockType.Thinking,
                Title = "Understanding your request",
                Collapsible = true,
                Collapsed = true,
                IsActive = false,
                Stages = new List<ThinkingStage>
                {
                    new ThinkingStage
                    {
                        Status = ThinkingStageStatus.Completed,
                        Content = "Identified request as a business dashboard requirement.",
                        IconCss = "e-icons e-check"
                    }
                }
            },
            new ThinkingBlock
            {
                BlockType = BlockType.Thinking,
                Title = "Selecting UI components",
                Collapsible = true,
                Collapsed = true,
                IsActive = false,
                Stages = new List<ThinkingStage>
                {
                    new ThinkingStage
                    {
                        Status = ThinkingStageStatus.Completed,
                        IconCss = "e-icons e-check",
                        Content = "Selected {0}, {1}, and {2} for dashboard layout.",
                        EditableContext = new List<ThinkingContextItem>
                        {
                            new ThinkingContextItem { Type = ThinkingContextType.Tool, Name = "Charts", Value = "Analytics visualization" },
                            new ThinkingContextItem { Type = ThinkingContextType.Tool, Name = "Grid", Value = "Tabular data" },
                            new ThinkingContextItem { Type = ThinkingContextType.Tool, Name = "Cards", Value = "KPI metrics" }
                        }
                    }
                }
            },
            new ThinkingBlock
            {
                BlockType = BlockType.Thinking,
                Title = "Designing layout structure",
                Collapsible = true,
                Collapsed = true,
                IsActive = false,
                Stages = new List<ThinkingStage>
                {
                    new ThinkingStage
                    {
                        Status = ThinkingStageStatus.Completed,
                        IconCss = "e-icons e-check",
                        Content = "Created responsive {0} layout structure.",
                        EditableContext = new List<ThinkingContextItem>
                        {
                            new ThinkingContextItem { Type = ThinkingContextType.Context, Name = "12-column", Value = "12-column grid" }
                        }
                    }
                }
            },
            new ThinkingBlock
            {
                BlockType = BlockType.Thinking,
                Title = "Finalizing output",
                Collapsible = true,
                Collapsed = false,
                IsActive = false,
                Stages = new List<ThinkingStage>
                {
                    new ThinkingStage
                    {
                        Status = ThinkingStageStatus.Completed,
                        IconCss = "e-icons e-check",
                        Content = "Generated final dashboard structure successfully."
                    }
                }
            }
        };
        args.Response = @"## Business Dashboard Structure

**Generated successfully.**

### Features Included:
- Key performance indicator cards
- Revenue and sales charts
- Recent activity data grid
- Responsive layout for all devices
- Clean navigation structure

### Recommended Syncfusion Components:
- Chart
- Grid
- Card
- Sidebar
- DropDownList";
    }
}

```

![Blazor AI AssistView Adding Stage Status](./images/chain-of-thoughts-items.webp)

## Configure Thinking Stage Template

You can use the `ThinkingStageTemplate` tag to customize the rendering of thinking stages. The template receives a context object with the following properties:

| Context property | Type | Description |
|------------------|------|-------------|
| `Block` | `ThinkingBlock` | The full thinking block model. |
| `BlockIndex` | `int` | Zero-based index of this block in the `Blocks` collection. |
| `Stage` | `ThinkingStage` | The current stage being rendered (use this to read `Content`, `IconCss`, `Status`, etc.). |

```cshtml

@using Syncfusion.Blazor.InteractiveChat

<div class="aiassist-container" style="height: 350px; width: 650px;">
    <SfAIAssistView PromptRequested="@PromptRequest" Prompts="@prompts">
        <ThinkingStageTemplate>
            <div class="custom-stage-item">
                <span class="e-icons @context.Stage.IconCss"></span>
                <div class="custom-stage-content">@context.Stage.Content</div>
            </div>
        </ThinkingStageTemplate>
    </SfAIAssistView>
</div>

@code {
    private readonly List<AssistViewPrompt> prompts = new()
    {
        new AssistViewPrompt
        {
            Prompt = "What is the capital of France?",
            Response = "The capital of France is Paris.",
            Blocks = new List<IResponseBlock>
            {
                new ThinkingBlock
                {
                    BlockType = BlockType.Thinking,
                    Title = "Fact lookup",
                    IsActive = false,
                    Collapsed = false,
                    Collapsible = false,
                    Stages = new List<ThinkingStage>
                    {
                        new ThinkingStage
                        {
                            Status = ThinkingStageStatus.Completed,
                            Content = "Checked knowledge base for European capitals."
                        }
                    }
                }
            }
        }
    };

    private async Task PromptRequest(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        args.Response = "For real-time prompt processing, connect the AI AssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.";
    }
}

```

![Blazor AI AssistView Block Template](./images/chain-of-thoughts-template.webp)

> When `ThinkingStageTemplate` is set, the default collapsible header, spinner, and Timeline rendering are completely replaced by your template. Collapse/expand behavior and spinner life cycle management must be handled within the template itself.

## Best Practices

1. **Start each stage with `IsActive = true` and `Collapsed = false`** on the most recent `ThinkingBlock`, and mark earlier blocks as `IsActive = false`, `Collapsed = true`. This keeps the user's focus on the live reasoning while preserving history.
2. **Use `Status` to reflect reality.** `InProgress` triggers a spinner, `Completed` renders a check icon, `Failed` renders an error icon. Misusing these values confuses the user.
3. **Keep `Stages` short and scannable.** 2–5 stages per block reads better than long lists. If the model produces more steps, group them into multiple `ThinkingBlock`s instead of one long timeline.
4. **Prefer `Content` over the default header** for descriptive text. `Title` is best kept short (e.g., `"Reasoning"`, `"Selecting UI components"`); the per-step `Content` is where the explanation belongs.
5. **Use `EditableContext` for chips that the user might want to inspect** (e.g., component names, file names). They render as inline badges in the stage content via `{index}` placeholders.
6. **Combine with `EnableStreaming`.** When the AI service streams reasoning, push a new `ThinkingBlock` per major step and update `args.Blocks` incrementally — the AI AssistView renders the new state automatically.
7. **Reset state between prompts.** Each call to `PromptRequested` should return a fresh `Blocks` list that includes all completed stages. The component does not automatically carry state across calls.

## See Also

* [Generative UI](./generative-ui.md)
* [Streaming Responses](./streaming.md)
* [AI Service Integration](./ai-service-integration.md)
* [Methods & Programmatic API](./methods-api.md)
````