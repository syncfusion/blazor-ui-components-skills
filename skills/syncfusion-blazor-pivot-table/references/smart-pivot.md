# Smart Pivot Table (AI Features)

The Pivot Table can be enhanced with AI-driven features using the `Syncfusion.Blazor.AI` NuGet package. Three AI capabilities are available:

- **Smart Data Aggregation** — AI suggests optimal aggregation and field arrangements
- **Predictive Modeling** — AI predicts future data points based on historical trends
- **Adaptive Filtering** — AI applies dynamic filters based on natural language input

---

## Prerequisites — NuGet Packages

```bash
# Core packages (always required)
Install-Package Syncfusion.Blazor.PivotTable
Install-Package Syncfusion.Blazor.Themes
Install-Package Syncfusion.Blazor.AI
Install-Package Microsoft.Extensions.AI
```

Choose your AI provider:

| Provider | Additional Package |
|---|---|
| OpenAI | `Microsoft.Extensions.AI.OpenAI` |
| Azure OpenAI | `Microsoft.Extensions.AI.OpenAI` + `Azure.AI.OpenAI` |
| Ollama (self-hosted) | `OllamaSharp` |

---

## Configure AI Service in `Program.cs`

### OpenAI

```csharp
using Syncfusion.Blazor.AI;
using Microsoft.Extensions.AI;
using OpenAI;

var builder = WebApplication.CreateBuilder(args);

string openAIApiKey = "YOUR_API_KEY";
string openAIModel = "gpt-4"; // e.g., gpt-3.5-turbo, gpt-4

OpenAIClient openAIClient = new OpenAIClient(openAIApiKey);
IChatClient chatClient = openAIClient.GetChatClient(openAIModel).AsIChatClient();

builder.Services.AddChatClient(chatClient);
builder.Services.AddSingleton<IChatInferenceService, SyncfusionAIService>();
builder.Services.AddSyncfusionBlazor();
```

### Azure OpenAI

```csharp
using Syncfusion.Blazor.AI;
using Azure.AI.OpenAI;
using Microsoft.Extensions.AI;
using System.ClientModel;

var builder = WebApplication.CreateBuilder(args);

string azureKey = "AZURE_OPENAI_KEY";
string azureEndpoint = "AZURE_OPENAI_ENDPOINT";
string azureModel = "AZURE_OPENAI_MODEL";

AzureOpenAIClient azureClient = new AzureOpenAIClient(
    new Uri(azureEndpoint),
    new ApiKeyCredential(azureKey)
);
IChatClient chatClient = azureClient.GetChatClient(azureModel).AsIChatClient();

builder.Services.AddChatClient(chatClient);
builder.Services.AddSingleton<IChatInferenceService, SyncfusionAIService>();
builder.Services.AddSyncfusionBlazor();
```

### Ollama (Self-hosted)

```csharp
using Syncfusion.Blazor.AI;
using Microsoft.Extensions.AI;
using OllamaSharp;

var builder = WebApplication.CreateBuilder(args);

// Ensure Ollama is running at http://localhost:11434
string modelName = "llama2:13b"; // or mistral:7b, etc.
IChatClient chatClient = new OllamaApiClient("http://localhost:11434", modelName);

builder.Services.AddChatClient(chatClient);
builder.Services.AddSingleton<IChatInferenceService, SyncfusionAIService>();
builder.Services.AddSyncfusionBlazor();
```

> **For WebAssembly/Auto mode:** Register `AddSyncfusionBlazor()` in both server-side and client-side `Program.cs`. The AI service (ChatClient) only needs to be registered in the server-side `Program.cs`.

---

## Component Setup

The Pivot Table itself requires no special markup for AI — AI features are triggered via a separate AI Assist dialog that interacts with the pivot report programmatically.

```razor
@using Syncfusion.Blazor.PivotView

<SfPivotView ID="PivotView" @ref="pivotRef"
             TValue="PivotProductDetails"
             Width="100%" Height="500"
             ShowFieldList="true"
             ShowToolbar="true"
             AllowConditionalFormatting="true"
             AllowPdfExport="true"
             AllowExcelExport="true"
             Toolbar="@toolbar">
    <PivotViewDataSourceSettings DataSource="@data"
                                  AllowLabelFilter="true"
                                  AllowMemberFilter="true">
        <PivotViewColumns>
            <PivotViewColumn Name="Year"></PivotViewColumn>
        </PivotViewColumns>
        <PivotViewRows>
            <PivotViewRow Name="Country"></PivotViewRow>
            <PivotViewRow Name="Products"></PivotViewRow>
        </PivotViewRows>
        <PivotViewValues>
            <PivotViewValue Name="Sold" Caption="Units Sold"></PivotViewValue>
            <PivotViewValue Name="Amount" Caption="Sold Amount"></PivotViewValue>
        </PivotViewValues>
    </PivotViewDataSourceSettings>
</SfPivotView>

@code {
    private SfPivotView<PivotProductDetails> pivotRef;
    private List<PivotProductDetails> data { get; set; }

    private List<ToolbarItems> toolbar = new List<ToolbarItems>
    {
        ToolbarItems.Grid,
        ToolbarItems.Chart,
        ToolbarItems.Export,
        ToolbarItems.FieldList
    };

    protected override void OnInitialized()
    {
        data = PivotProductDetails.GetData();
    }
}
```

---

## AI Feature Patterns

### Predictive Modeling

The AI analyzes historical data and generates predicted future data points. The response is merged into the pivot data source.

**Typical flow:**
1. User selects a target year in the AI Assist dialog
2. Send current data + prompt to `IChatInferenceService`
3. Parse the AI response (JSON predictions)
4. Add predicted rows to the data source
5. Refresh the pivot with `pivotRef.RefreshAsync()`

### Smart Data Aggregation

The AI suggests which fields to use as rows/columns/values and which aggregation type is most meaningful.

**Typical flow:**
1. User selects fields and aggregation type in dialog
2. Send field metadata + prompt to AI
3. AI returns a suggested report config
4. Update `PivotViewDataSourceSettings` programmatically
5. Re-render pivot

### Adaptive Filtering

The AI translates a natural language filter description into `PivotViewFilterSettings`.

**Typical flow:**
1. User types a filter description (e.g., "products from Asia")
2. Send available members + prompt to AI
3. AI returns matching member names
4. Apply as `FilterType.Include` filter
5. Pivot updates automatically

---

## AI Assist Dialog Pattern

The recommended UX pattern is a dialog with chip selection for the three AI modes:

```razor
@using Syncfusion.Blazor.Popups

<SfDialog @ref="Dialog"
          Height="500px"
          ShowCloseIcon="true"
          @bind-Visible="@dialogVisible">
    <DialogTemplates>
        <Header>
            <span class="e-icons e-ai-chat"></span> AI Assistant
        </Header>
        <Content>
            <!-- Chip selector: Predictive / Aggregation / Filtering -->
            <!-- Dynamic prompt input based on selection -->
        </Content>
    </DialogTemplates>
    <DialogButtons>
        <DialogButton IsPrimary="true" Content="Submit" OnClick="@SubmitAIRequest" />
    </DialogButtons>
</SfDialog>

@code {
    private SfDialog Dialog;
    private bool dialogVisible = false;

    private async Task SubmitAIRequest()
    {
        // 1. Build prompt from user input
        // 2. Call: await chatInferenceService.GetResponseAsync(prompt)
        // 3. Parse response and update pivot data/settings
        // 4. await pivotRef.RefreshAsync();
    }
}
```

---

## Full Sample Reference

The complete working sample is available at:
```
https://github.com/syncfusion/smart-ai-samples/tree/master/blazor/SyncfusionAISamples/Components/Pages/PivotTable
```

---

## Error Handling

| Scenario | Recommended Action |
|---|---|
| API key invalid | Show toast notification, log error |
| AI service timeout | Retry with exponential backoff |
| Unparseable AI response | Fall back to current pivot settings |
| Ollama server unreachable | Verify `http://localhost:11434` is running |
