````markdown
# Generative UI

## Table of Contents
- [Overview](#overview)
- [Core Concepts](#core-concepts)
- [Registering Custom Tools](#registering-custom-tools)
- [Adding Tools in AI Responses](#adding-tools-in-ai-responses)
- [Configuring AI for Generative UI Responses](#configuring-ai-for-generative-ui-responses)
- [Passing Props to Tools](#passing-props-to-tools)
- [Combining Text and Tool Blocks](#combining-text-and-tool-blocks)
- [Best Practices](#best-practices)
- [See Also](#see-also)

## Overview

The **Generative UI** feature in the Blazor AI AssistView enables you to render dynamic tools and UI elements within the chat interface. It allows seamless integration of interactive components based on AI-generated responses, such as cards, forms, or custom widgets.

A tool is a piece of interactive UI that you register once in the component and then reference from any AI response. The AI service returns a structured `Blocks` array (mixing `Text` and `Tool` blocks) and the AI AssistView renders each block using the appropriate template.

## Core Concepts

| Concept | Type | Purpose |
|---------|------|---------|
| `ToolUIBlock` | Tag directive | Container that defines templates for each registered tool. The context exposes `context.Block` (the current `IResponseBlock`). |
| `IResponseBlock` | Interface | Base type for any block that can appear in an AI response. |
| `TextBlock` | Class | A block of plain/markdown text. `BlockType = BlockType.Text`, `Content` holds the text. |
| `ToolBlock` | Class | A reference to a registered tool. `BlockType = BlockType.Tool`, `ToolName` matches the tool, `Props` (optional) carries parameters. |
| `BlockType` | Enum | `Text` or `Tool`. Determines how the block is rendered. |
| `ToolName` | `string` | The unique name that links a `ToolBlock` to a `ToolUIBlock` branch in your template. |
| `Props` | `IDictionary<string, object>` | Optional parameters passed from the AI response into the tool template. |
| `args.Blocks` | `List<IResponseBlock>` | The response property on `AssistViewPromptRequestedEventArgs` used to return a structured response (instead of `args.Response`). |

> **Note:** When adding a tool block in a response, set `BlockType` to `Tool` and provide the registered tool name in `ToolName`. The tool must be registered before it is referenced in a response.

## Registering Custom Tools

Use the `ToolUIBlock` tag directive inside `SfAIAssistView` to register one or more tools. Each tool is identified by its `ToolName` and is rendered by the `if/else if` branch whose condition matches `context.Block?.ToolName`.

### Example: Registering a tool

```cshtml

@using Syncfusion.Blazor.InteractiveChat
@inject IJSRuntime JS

<SfAIAssistView @ref="aiAssistView" PromptRequested="PromptRequest" PromptSuggestions="@PromptSuggestions" EnableStreaming="true" Prompts="@Prompts">
    <ToolUIBlock>
        @if (context.Block?.ToolName == "weather-card")
        {
            <div class="e-card weather-card" role="button">
                <div class="e-card-header">
                    <div class="e-card-header-caption">
                        <div class="e-card-header-title">Today</div>
                        <div class="e-card-sub-title">New York - Scattered Showers</div>
                    </div>
                </div>
                <div class="e-card-header weather_report">
                    <div class="e-card-header-caption">
                        <div class="e-card-header-title">1C / -4C</div>
                        <div class="e-card-sub-title">Chance for snow: 100%</div>
                    </div>
                </div>
            </div>
        }
        else if (context.Block?.ToolName == "recipe-maker")
        {
            <div class="recipe-panel e-card">
                <h2 class="recipe-title">@CurrentRecipe.Title</h2>
                <div class="recipe-section">
                    <div class="recipe-header">
                        <h4>🥕 Ingredients</h4>
                        <button class="e-btn e-primary e-small" @onclick="AddIngredient">Add Ingredient</button>
                    </div>
                    <div class="ingredients-list">
                        @for (var index = 0; index < CurrentRecipe.Ingredients.Count; index++)
                        {
                            var ingredientIndex = index;
                            <div class="ingredient-item">
                                <input class="ingredient-name" @bind="CurrentRecipe.Ingredients[ingredientIndex].Name" />
                                <input class="ingredient-qty" @bind="CurrentRecipe.Ingredients[ingredientIndex].Quantity" />
                                <button class="e-btn e-small e-danger" @onclick="() => RemoveIngredient(ingredientIndex)">X</button>
                            </div>
                        }
                    </div>
                </div>
                <div class="recipe-section">
                    <div class="recipe-header">
                        <h4>Instructions</h4>
                        <button class="e-btn e-primary e-small" @onclick="AddStep">Add Step</button>
                    </div>
                    <div class="instructions-list">
                        @for (var index = 0; index < CurrentRecipe.Instructions.Count; index++)
                        {
                            var stepIndex = index;
                            <div class="step-item">
                                <input class="step-text" @bind="CurrentRecipe.Instructions[stepIndex]" />
                                <button class="e-btn e-small e-danger" @onclick="() => RemoveStep(stepIndex)">X</button>
                            </div>
                        }
                    </div>
                </div>
                <button class="e-btn e-primary" @onclick="CheckRecipeScore">Check Recipe Score</button>
            </div>
        }
        else if (context.Block?.ToolName == "recipe-score-gauge")
        {
             var props = context.Block?.Props as IDictionary<string, object>;
             int score = 0;
             string title = "Recipe Score";
             if (props != null)
             {
                if (props.TryGetValue("score", out var scoreObj))
                {
                    score = Convert.ToInt32(scoreObj);
                }
                if (props.TryGetValue("title", out var titleObj))
                {
                    title = titleObj?.ToString() ?? "Recipe Score";
                }
             }
             <div class="score-gauge-panel e-card">
                 <h3>@title</h3>
                 <div class="score-value"> @score/100</div>
                 <p class="score-comment"> @GetScoreComment(score)</p>
             </div>
        }
    </ToolUIBlock>
</SfAIAssistView>

@code {
    private List<string> PromptSuggestions = new() {
        "Suggest a healthy breakfast recipe under 5 ingredients",
        "What is the weather in New York?"
    };

    private List<AssistViewPrompt> Prompts = new() {
        new AssistViewPrompt {
            Prompt = "Suggest a healthy breakfast recipe under 5 ingredients",
            Blocks = RecipeData
        }
    };
    private static List<IResponseBlock> RecipeData => new() {
        new TextBlock { BlockType = BlockType.Text, Content = "**Here is your recipe!** Feel free to edit ingredients and steps, then click **Check Recipe Score**." },
        new ToolBlock { BlockType = BlockType.Tool, ToolName = "recipe-maker" }
    };
    private static List<IResponseBlock> WeatherData => new() {
        new TextBlock { BlockType = BlockType.Text, Content = "Here is the current weather forecast for your location:" },
        new ToolBlock { BlockType = BlockType.Tool, ToolName = "weather-card" },
        new TextBlock { BlockType = BlockType.Text, Content = "**Scattered Showers Expected** with temperatures ranging from **1°C to -4°C**. There is a **100% chance of snow**, so it's recommended to bundle up and exercise caution if traveling. The weather system is expected to continue throughout the day with moderate precipitation." }
    };

    private List<IResponseBlock> ScoreBlocks = new();
    private SfAIAssistView aiAssistView;
    private RecipeDraft CurrentRecipe { get; set; } = new()
    {
        Title = "Butter Toast",
        Ingredients = new List<Ingredient>
        {
            new() { Name = "Bread slices", Quantity = "2" },
            new() { Name = "Butter", Quantity = "1 tbsp" },
            new() { Name = "Sugar", Quantity = "1 tsp" }
        },
        Instructions = new List<string>
        {
            "Spread butter on bread slices",
            "Toast until golden and sprinkle sugar on top"
        }
    };
    private async Task PromptRequest(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        if (args.Prompt == "What is the weather in New York?")
        {
            args.Blocks = WeatherData;
        }
        else if (args.Prompt == "Generate a score analysis for this recipe.")
        {
            args.Blocks = ScoreBlocks;
        }
        else if (args.Prompt == "Suggest a healthy breakfast recipe under 5 ingredients")
        {
            CurrentRecipe = new RecipeDraft
            {
                Title = "Butter Toast",
                Ingredients = new List<Ingredient>
                {
                    new() { Name = "Bread slices", Quantity = "2" },
                    new() { Name = "Butter", Quantity = "1 tbsp" },
                    new() { Name = "Sugar", Quantity = "1 tsp" }
                },
                Instructions = new List<string>
                {
                    "Spread butter on bread slices",
                    "Toast until golden and sprinkle sugar on top"
                }
            };
            args.Blocks = new List<IResponseBlock> {
                new TextBlock { BlockType = BlockType.Text, Content = "**Here is your recipe!** Feel free to edit ingredients and steps, then click **Check Recipe Score**." },
                new ToolBlock { BlockType = BlockType.Tool, ToolName = "recipe-maker" }
            };
        }
        else
        {
            args.Response = "For real-time prompt processing, connect the AIAssistView component to your preferred AI service, such as OpenAI or Azure Cognitive Services. Ensure you obtain the necessary API credentials to authenticate and enable seamless integration.";
        }
    }

    private void AddIngredient()
    {
        CurrentRecipe.Ingredients.Add(new Ingredient { Name = "New Ingredient", Quantity = "qty" });
    }

    private void RemoveIngredient(int index)
    {
        if (index >= 0 && index < CurrentRecipe.Ingredients.Count)
        {
            CurrentRecipe.Ingredients.RemoveAt(index);
        }
    }

    private void AddStep()
    {
        CurrentRecipe.Instructions.Add("New instruction step...");
    }

    private void RemoveStep(int index)
    {
        if (index >= 0 && index < CurrentRecipe.Instructions.Count)
        {
            CurrentRecipe.Instructions.RemoveAt(index);
        }
    }

    private async Task CheckRecipeScore()
    {
    var score = CalculateRecipeScore(CurrentRecipe);
    ScoreBlocks = new List<IResponseBlock>
    {
        new TextBlock
        {
            BlockType = BlockType.Text,
            Content = $"**Recipe Score Analysis**\n\nHere is the health and quality score for **{CurrentRecipe.Title}**."
        },

        new ToolBlock
        {
            BlockType = BlockType.Tool,
            ToolName = "recipe-score-gauge",
            Props = new Dictionary<string, object>
            {
                ["score"] = score,
                ["title"] = CurrentRecipe.Title
            }
        },

        new TextBlock
        {
            BlockType = BlockType.Text,
            Content = "You can continue editing the recipe above and check the score again anytime."
        }
    };

    await InvokeAsync(StateHasChanged);

    await aiAssistView.ExecutePromptAsync(
        "Generate a score analysis for this recipe.");
    }
    private sealed class RecipeDraft
    {
        public string Title { get; set; } = "Custom Recipe";
        public List<Ingredient> Ingredients { get; set; } = new();
        public List<string> Instructions { get; set; } = new();
    }
    private sealed class Ingredient
    {
        public string Name { get; set; } = string.Empty;
        public string Quantity { get; set; } = string.Empty;
    }
    private int CalculateRecipeScore(RecipeDraft recipe)
    {
        int score = 100;
        var ingredients = recipe.Ingredients;
        var instructions = recipe.Instructions;
        int validIng = 0, validSteps = 0;
        if (!ingredients.Any()) return 15;
        if (!instructions.Any()) return 20;
        foreach (var ingredient in ingredients)
        {
            var n = ingredient.Name.Trim();
            var q = ingredient.Quantity.Trim();
            if (string.IsNullOrEmpty(n) || string.IsNullOrEmpty(q)) score -= 12;
            else validIng++;
        }
        score += (validIng >= 5 ? 10 : validIng == 1 ? -20 : validIng == 2 ? -10 : 0);
        foreach (var step in instructions)
        {
            var s = step?.ToString().Trim();
            if (string.IsNullOrEmpty(s)) score -= 15;
            else validSteps++;
        }
        score += (validSteps >= 4 ? 10 : validSteps == 1 ? -25 : validSteps == 2 ? -15 : validSteps == 3 ? -5 : 0);
        if (validIng >= 3 && validSteps >= 3) score += 8;
        score += new Random().Next(0, 6);
        return Math.Clamp(score, 10, 100);
    }
    private string GetScoreComment(int score)
    {
        if (score >= 90) return "Outstanding recipe! Highly recommended.";
        if (score >= 80) return "Very good recipe with excellent balance.";
        if (score >= 70) return "Solid recipe. Minor improvements possible.";
        return "Average recipe. Consider refining ingredients or steps.";
    }
}
<style>
.recipe-panel {
    max-width: 720px;
    margin: 1rem auto;
    padding: 1.5rem;
    border: 1px solid #e2e8f0;
    border-radius: 18px;
    box-shadow: 0 8px 24px rgba(15, 23, 42, 0.12);
}

.recipe-title {
    margin: 0 0 1.5rem;
    text-align: center;
    font-size: 2rem;
}

.recipe-section {
    margin-top: 1.25rem;
}

.recipe-header {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 1rem;
    margin-bottom: .75rem;
}

.recipe-header h4 {
    margin: 0;
}

.ingredient-item,
.step-item {
    display: flex;
    align-items: center;
    gap: .6rem;
    margin-bottom: .6rem;
}

.ingredient-name,
.ingredient-qty,
.step-text {
    min-width: 0;
    padding: .55rem .7rem;
    border: 1px solid #d6dee8;
    border-radius: 6px;
    font: inherit;
}

.ingredient-name,
.step-text {
    flex: 1;
}

.ingredient-qty {
    width: 7rem;
}

.score-gauge-panel {
    max-width: 420px;
    margin: 1rem auto;
    padding: 1.5rem;
    text-align: center;
    border: 1px solid #e2e8f0;
    border-radius: 18px;
    box-shadow: 0 8px 24px rgba(15, 23, 42, 0.12);
}

.score-value {
    margin: .75rem 0;
    color: #1769aa;
    font-size: 2.5rem;
    font-weight: 700;
}

@@media (max-width: 640px) {
    .recipe-panel {
        padding: 1rem;
    }

    .ingredient-item,
    .step-item,
    .recipe-header {
        align-items: stretch;
        flex-direction: column;
    }

    .ingredient-qty {
        width: auto;
    }
}
</style>

```

![Blazor AI AssistView Gen UI](./images/ai-assistview-gen-ui.webp)

## Adding Tools in AI Responses

To display a tool in the chat, return a response block with `BlockType` set to `Tool` and the registered `ToolName` from your AI or backend logic. You can combine tool blocks with text or other block types in the same response.

```csharp
private static List<IResponseBlock> WeatherData => new()
{
    new TextBlock { BlockType = BlockType.Text, Content = "Here is the current weather forecast for your location:" },
    new ToolBlock { BlockType = BlockType.Tool, ToolName = "weather-card" },
    new TextBlock { BlockType = BlockType.Text, Content = "**Scattered Showers Expected** with temperatures ranging from **1°C to -4°C**." }
};
```

## Passing Props to Tools

Tools can accept parameters from the AI response via the `Props` dictionary. In the `ToolUIBlock` template, read the values from `context.Block?.Props` and use them in the rendered UI.

```cshtml
else if (context.Block?.ToolName == "recipe-score-gauge")
{
    var props = context.Block?.Props as IDictionary<string, object>;
    int score = 0;
    string title = "Recipe Score";
    if (props != null)
    {
        if (props.TryGetValue("score", out var scoreObj))
        {
            score = Convert.ToInt32(scoreObj);
        }
        if (props.TryGetValue("title", out var titleObj))
        {
            title = titleObj?.ToString() ?? "Recipe Score";
        }
    }
    <div class="score-gauge-panel e-card">
        <h3>@title</h3>
        <div class="score-value"> @score/100</div>
    </div>
}
```

The corresponding `ToolBlock` is created with the props populated:

```csharp
new ToolBlock
{
    BlockType = BlockType.Tool,
    ToolName = "recipe-score-gauge",
    Props = new Dictionary<string, object>
    {
        ["score"] = 87,
        ["title"] = "Butter Toast"
    }
}
```

## Combining Text and Tool Blocks

A response can mix `TextBlock` and `ToolBlock` entries in a single `Blocks` array. The AI AssistView renders them in order:

```csharp
new List<IResponseBlock>
{
    new TextBlock { BlockType = BlockType.Text, Content = "**Recipe Score Analysis**" },
    new ToolBlock
    {
        BlockType = BlockType.Tool,
        ToolName = "recipe-score-gauge",
        Props = new Dictionary<string, object> { ["score"] = 87 }
    },
    new TextBlock { BlockType = BlockType.Text, Content = "You can continue editing the recipe above." }
};
```

## Configuring AI for Generative UI Responses

Configure your AI service to return structured JSON blocks. This ensures that the AI AssistView can render both text and interactive tool blocks in the chat interface.

> **Tip:** Always return a single `blocks` array in your AI response. Each block can be of type `text`, `tool`, or other supported types.

### Suggested system prompt

```text
You are an AI assistant that generates Syncfusion AIAssistView blocks.
Return ONLY valid JSON.
Output format:
{
   "blocks": [
        {
        "blockType": "text",
        "content": "Description"
        },
        {
        "blockType": "tool",
        "toolName": "toolname",
        "props": { ... }
        }
    ]
}
Rules:
1. Always return a single "blocks" array.
2. Return ONLY valid JSON.
3. You may return ANY number of blocks.
4. Whenever weather-related queries are requested, invoke the weather-tool block with blockType "tool" and toolName "weather-tool".
```

### Example: Handling AI response in Blazor

```cshtml

@using Syncfusion.Blazor.InteractiveChat

<SfAIAssistView ID="aiAssistView" PromptRequested="PromptRequest">
    <ToolUIBlock>
        @if (context.Block?.ToolName == "weather-tool")
        {
    var props = context.Block?.Props as Dictionary<string, object>;
    var location = props != null && props.TryGetValue("location", out var locationValue)
        ? locationValue?.ToString()
        : "Unknown";
    var temperature = props != null && props.TryGetValue("temperature", out var temperatureValue)
        ? temperatureValue?.ToString()
        : "--";


            <div tabindex="0" class="e-card" id="weather_card" role="button">
                <div class="e-card-header">
                    <div class="e-card-header-caption">
                        <div class="e-card-header-title">@location</div>
                        <div class="e-card-sub-title">@temperature</div>
                    </div>
                </div>
            </div>
        }
    </ToolUIBlock>
</SfAIAssistView>

@code {
    private const string ApiKey = ""; // Your API key here
    private const string ApiUrl = ""; // Your AI response URL here

    private static readonly string SystemPrompt = @"
    You are an AI assistant that generates Syncfusion AIAssistView blocks.
    Return ONLY valid JSON.
    Output format:
    {
       ""blocks"": [
            {
            ""blockType"": ""text"",
            ""content"": ""Description""
            },
            {
            ""blockType"": ""tool"",
            ""toolName"": ""toolname"",
            ""props"": { ... }
            }
        ]
    }
    Rules:
    1. Always return a single ""blocks"" array.
    2. Return ONLY valid JSON.
    3. You may return ANY number of blocks.
    4. Whenever weather-related queries are requested, invoke the weather-tool block with blockType ""tool"" and toolName ""weather-tool"".";

    [Inject]
    private HttpClient Http { get; set; }

    private async Task PromptRequest(AssistViewPromptRequestedEventArgs args)
    {
        await Task.Delay(1000);
        var prompt = args.Prompt?.Trim();
        // If API key and URL are set, call the AI service
        if (!string.IsNullOrWhiteSpace(ApiKey) && !string.IsNullOrWhiteSpace(ApiUrl))
        {
            try
            {
                var payload = new
                {
                    model = "gpt-5",
                    messages = new
                    {
                        messages = new[]
                        {
                            new { role = "system", content = SystemPrompt },
                            new { role = "user", content = prompt }
                        }
                    },
                    max_output_tokens = 1000
                };

                var request = new HttpRequestMessage(HttpMethod.Post, ApiUrl);
                request.Headers.Authorization = new System.Net.Http.Headers.AuthenticationHeaderValue("Bearer", ApiKey);
                request.Content = new StringContent(System.Text.Json.JsonSerializer.Serialize(payload), System.Text.Encoding.UTF8, "application/json");

                var response = await Http.SendAsync(request);
                response.EnsureSuccessStatusCode();

                var jsonText = await response.Content.ReadAsStringAsync();
                using var doc = System.Text.Json.JsonDocument.Parse(jsonText);
                if (doc.RootElement.TryGetProperty("blocks", out var blocksElement))
                {
                    var blocks = new List<IResponseBlock>();
                    foreach (var block in blocksElement.EnumerateArray())
                    {
                        var type = block.GetProperty("blockType").GetString();
                        if (type == "text")
                        {
                            blocks.Add(new TextBlock
                            {
                                BlockType = BlockType.Text,
                                Content = block.GetProperty("content").GetString()
                            });
                        }
                        else if (type == "tool")
                        {
                            var toolName = block.GetProperty("toolName").GetString();
                            var props = new Dictionary<string, object>();
                            if (block.TryGetProperty("props", out var propsElement))
                            {
                                foreach (var prop in propsElement.EnumerateObject())
                                    props[prop.Name] = prop.Value.GetString();
                            }
                            blocks.Add(new ToolBlock
                            {
                                BlockType = BlockType.Tool,
                                ToolName = toolName,
                                Props = props
                            });
                        }
                    }
                    args.Blocks = blocks;
                }
            }
            catch
            {
            }
        }
        else
        {
            args.Response = "For real-time prompt processing, connect the AI AssistView component to your preferred AI service.";
        }
    }
}

```

## Best Practices

1. **Register tools before referencing them.** A `ToolBlock` whose `ToolName` has no matching branch in `ToolUIBlock` will not render. Define all tools up front.
2. **Keep tool templates self-contained.** Tools should not rely on component state that is not passed via `Props` — treat each tool rendering as a pure function of its props.
3. **Validate props at the boundary.** Since `Props` is `IDictionary<string, object>`, guard every read with a default value (`TryGetValue` or null-coalescing) so missing keys do not throw.
4. **Use text blocks to set context.** Wrap a `ToolBlock` between a leading `TextBlock` (explanation) and a trailing `TextBlock` (follow-up) so the chat remains readable.
5. **Return structured JSON from the AI.** Tell the model to always return a single `blocks` array and to use only the `toolName`s you have registered. Validate the response in C# before assigning it to `args.Blocks`.
6. **Pre-load via `Prompts`.** Static conversations can be pre-loaded by setting `Blocks` on each `AssistViewPrompt` entry — useful for demos and seed data.
7. **Combine with `EnableStreaming`.** Tools work with streaming; the AI AssistView renders each block as it is appended.

## See Also

* [About response blocks](./chain-of-thoughts.md)
* [AI Service Integration](./ai-service-integration.md)
* [Streaming Responses](./streaming.md)
* [Methods & Programmatic API](./methods-api.md)
````