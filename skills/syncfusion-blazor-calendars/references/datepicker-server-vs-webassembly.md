# Server vs WebAssembly in Blazor DatePicker

This guide covers the differences, considerations, and best practices for using DatePicker in Blazor Server, Blazor WebAssembly, and Blazor Web App projects.

## Architecture Differences

### Blazor Server

- **Execution:** Server-side (C# runs on server)
- **Communication:** Real-time WebSocket connection (SignalR)
- **Rendering:** Server renders, sends HTML to client
- **Use Case:** Business apps, real-time updates, complex logic

**Advantages:**
- Full access to server resources (databases, APIs)
- Better performance on low-end devices
- Code stays on server (security)

**Disadvantages:**
- Requires constant server connection
- Higher latency with remote servers
- Higher bandwidth usage

### Blazor WebAssembly (WASM)

- **Execution:** Client-side (.NET runs in browser as WebAssembly)
- **Communication:** HTTP requests to APIs
- **Rendering:** Client renders everything
- **Use Case:** Standalone apps, offline-capable, rich UIs

**Advantages:**
- Works offline (after initial load)
- No server dependency
- Lower latency locally
- Reduced server load

**Disadvantages:**
- Slower initial load
- Secrets exposed in client code
- Limited device resources
- Can't access server resources directly

### Blazor Web App (.NET 8+)

- **Execution:** Hybrid (Server + WebAssembly)
- **Communication:** SignalR + HTTP
- **Rendering:** Multiple render modes per component
- **Use Case:** Modern apps requiring flexibility

**Advantages:**
- Use best approach for each component
- Offline-capable sections + real-time sync
- Gradual adoption path
- Best of both worlds

**Disadvantages:**
- More complex setup
- Requires .NET 8+

## Project Setup

### Blazor Server Setup

```csharp
// Program.cs
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents();

builder.Services.AddSyncfusionBlazor();

var app = builder.Build();

app.MapRazorComponents<App>()
    .AddInteractiveServerRenderMode();

app.Run();
```

**Component usage:**
```razor
@page "/datepicker"

<SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>

@code {
    DateTime? selectedDate;
}
```

### Blazor WebAssembly Setup

```csharp
// Program.cs (in Client project)
var builder = WebAssemblyHostBuilder.CreateDefault(args);

builder.RootComponents.Add<App>("#app");

builder.Services.AddSyncfusionBlazor();

await builder.Build().RunAsync();
```

**index.html setup:**
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>DatePicker App</title>
    
    <!-- Syncfusion theme -->
    <link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
</head>
<body>
    <div id="app"></div>

    <!-- Syncfusion script -->
    <script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
    <script src="_framework/blazor.web.js"></script>
</body>
</html>
```

### Blazor Web App Setup (.NET 8+)

```csharp
// Program.cs
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents()
    .AddInteractiveWebAssemblyComponents();

builder.Services.AddSyncfusionBlazor();

var app = builder.Build();

app.MapRazorComponents<App>()
    .AddInteractiveServerRenderMode()
    .AddInteractiveWebAssemblyRenderMode();

app.Run();
```

## Render Modes (Blazor Web App)

### InteractiveServer

```razor
@page "/datepicker"
@rendermode InteractiveServer

<SfDatePicker TValue="DateTime?" @bind-Value="@selectedDate"></SfDatePicker>

<p>Selected: @selectedDate</p>

@code {
    DateTime? selectedDate;
}
```

**Use when:** Need real-time updates, server-side validation, database access

### InteractiveWebAssembly

```razor
@page "/datepicker"
@rendermode InteractiveWebAssembly

<SfDatePicker TValue="DateTime?" @bind-Value="@selectedDate"></SfDatePicker>

<p>Selected: @selectedDate</p>

@code {
    DateTime? selectedDate;
}
```

**Use when:** Need offline support, reduce server load, client-side processing

### InteractiveAuto

```razor
@page "/datepicker"
@rendermode InteractiveAuto

<SfDatePicker TValue="DateTime?" @bind-Value="@selectedDate"></SfDatePicker>

@code {
    DateTime? selectedDate;
}
```

Automatically picks Server for first request, then WebAssembly. Requires both components.

### SSR (Static Server-Side Rendering)

```razor
@page "/datepicker"

<!-- No @rendermode means Static SSR -->

<SfDatePicker TValue="DateTime?" @bind-Value="@selectedDate"></SfDatePicker>

@code {
    DateTime? selectedDate; // Not interactive!
}
```

⚠️ **Note:** DatePicker won't work with SSR; requires interactivity.

## Event Handling

### Blazor Server Events

```razor
<SfDatePicker TValue="DateTime?" ValueChange="@OnDateChange"></SfDatePicker>

<p>Selected: @selectedDate</p>

@code {
    DateTime? selectedDate;

    void OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        selectedDate = args.Value;
        // Server-side logic runs immediately
        Console.WriteLine($"Server received: {selectedDate}");
    }
}
```

Events fire immediately with bidirectional communication.

### Blazor WebAssembly Events

```razor
<SfDatePicker TValue="DateTime?" ValueChange="@OnDateChange"></SfDatePicker>

<p>Selected: @selectedDate</p>

@code {
    DateTime? selectedDate;

    void OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        selectedDate = args.Value;
        // Client-side logic only
        Console.WriteLine($"Client-side: {selectedDate}");
    }
}
```

Events fire on client. To trigger server-side logic, call an API or use SignalR.

### WebAssembly to Server Communication

```razor
<!-- WebAssembly Component -->
<SfDatePicker TValue="DateTime?" ValueChange="@OnDateChange"></SfDatePicker>

<p>Selected: @selectedDate</p>

@code {
    DateTime? selectedDate;
    HttpClient http;

    protected override void OnInitialized()
    {
        http = new HttpClient { BaseAddress = new Uri(navigationManager.BaseUri) };
    }

    async Task OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        selectedDate = args.Value;
        
        // Send to server via API
        if (selectedDate.HasValue)
        {
            var response = await http.PostAsJsonAsync("/api/dates", new { date = selectedDate });
        }
    }
}
```

## Data Binding

### Blazor Server - Direct Binding

```razor
<SfDatePicker TValue="DateTime?" @bind-Value="@formData.EventDate"></SfDatePicker>

@code {
    FormModel formData = new();

    class FormModel
    {
        public DateTime? EventDate { get; set; }
    }
}
```

Changes reflect immediately in component state.

### Blazor WebAssembly - Client Storage

```razor
<SfDatePicker TValue="DateTime?" @bind-Value="@selectedDate"></SfDatePicker>

<button @onclick="SaveToLocalStorage">Save</button>

@code {
    DateTime? selectedDate;
    IJSRuntime js;

    async Task SaveToLocalStorage()
    {
        if (selectedDate.HasValue)
        {
            await js.InvokeVoidAsync("localStorage.setItem", 
                "selectedDate", selectedDate.Value.ToString("yyyy-MM-dd"));
        }
    }

    protected override async Task OnInitializedAsync()
    {
        var saved = await js.InvokeAsync<string>("localStorage.getItem", "selectedDate");
        if (!string.IsNullOrEmpty(saved) && DateTime.TryParse(saved, out var date))
        {
            selectedDate = date;
        }
    }
}
```

## Performance Considerations

### Blazor Server Performance

```razor
<!-- Good: One DatePicker per page -->
<SfDatePicker TValue="DateTime?" @bind-Value="@selectedDate"></SfDatePicker>

@code {
    DateTime? selectedDate;
}
```

**Performance Tips:**
- Minimize real-time event updates
- Debounce rapid ValueChange events
- Use `StateHasChanged()` judiciously
- Consider connection latency (200-500ms typical)

### Blazor WebAssembly Performance

```razor
<!-- Good: Lazy-load DatePicker in modal -->
@if (showModal)
{
    <SfDatePicker TValue="DateTime?" @bind-Value="@selectedDate"></SfDatePicker>
}

@code {
    DateTime? selectedDate;
    bool showModal;
}
```

**Performance Tips:**
- First load takes 2-5 seconds (WASM download/JIT)
- Minimize initial bundle size
- Use tree-shaking to remove unused components
- Cache Syncfusion WASM files in service worker

### Measure Performance

```razor
<button @onclick="MeasurePerformance">Test Performance</button>

@code {
    async Task MeasurePerformance()
    {
        var start = DateTime.Now;
        
        // Simulate date selection
        for (int i = 0; i < 1000; i++)
        {
            var temp = DateTime.Now.AddDays(i);
        }
        
        var elapsed = (DateTime.Now - start).TotalMilliseconds;
        Console.WriteLine($"Time: {elapsed}ms");
    }
}
```

## Network Considerations

### Blazor Server - Network Latency

```razor
<SfDatePicker TValue="DateTime?" ValueChange="@OnDateChange"></SfDatePicker>

<p>Server latency: ~@latency ms</p>

@code {
    DateTime? selectedDate;
    int latency = 0;

    async Task OnDateChange(ChangeEventArgs<DateTime?> args)
    {
        var start = DateTime.Now;
        selectedDate = args.Value;
        latency = (int)(DateTime.Now - start).TotalMilliseconds;
    }
}
```

**Impact:** Every interaction roundtrips server, high latency = slow UI

### Blazor WebAssembly - No Latency

```razor
<SfDatePicker TValue="DateTime?" ValueChange="@OnDateChange"></SfDatePicker>

@code {
    DateTime? selectedDate;

    void OnDateChange(ChangeEventArgs<DateTime?> args)
    {
        selectedDate = args.Value;
        // Instant, no network delay
    }
}
```

**Advantage:** UI feels snappy, responsive to user input

## Offline Scenarios

### Blazor WebAssembly - Offline Support

```razor
<SfDatePicker TValue="DateTime?" @bind-Value="@selectedDate"></SfDatePicker>

<button @onclick="SaveOffline">Save (works offline)</button>

@code {
    DateTime? selectedDate;
    IJSRuntime js;

    async Task SaveOffline()
    {
        // Save to IndexedDB for offline access
        await js.InvokeVoidAsync("db.save", "date", selectedDate);
    }
}
```

WASM continues working without server.

### Blazor Server - No Offline

```razor
<!-- Blazor Server requires connection -->
<SfDatePicker TValue="DateTime?" @bind-Value="@selectedDate"></SfDatePicker>

@code {
    DateTime? selectedDate;
    // Stops working if connection lost
}
```

Connection loss = component becomes unresponsive.

## Choosing the Right Model

### Use Blazor Server When:
- Heavy server-side logic needed
- Real-time updates required
- Complex database operations
- Security-sensitive data
- Users on consistent networks

### Use Blazor WebAssembly When:
- Offline support needed
- Low-latency user interaction critical
- Reduce server load
- Standalone application
- Distributed/offline-first

### Use Blazor Web App When:
- Hybrid benefits needed
- Critical features on server, fast UI client-side
- Gradual WASM adoption
- Maximum flexibility

## Example - Same DatePicker, Different Models

**Blazor Server:**
```razor
@rendermode InteractiveServer

<SfDatePicker TValue="DateTime?" ValueChange="@SaveToDatabase"></SfDatePicker>

@code {
    async Task SaveToDatabase(ChangeEventArgs<DateTime?> args)
    {
        // Direct database access
        await dbContext.SaveDateAsync(args.Value);
    }
}
```

**Blazor WebAssembly:**
```razor
@rendermode InteractiveWebAssembly

<SfDatePicker TValue="DateTime?" ValueChange="@SaveViaAPI"></SfDatePicker>

@code {
    async Task SaveViaAPI(ChangeEventArgs<DateTime?> args)
    {
        // API call to server
        await http.PostAsJsonAsync("/api/dates", new { date = args.Value });
    }
}
```

## Troubleshooting

### Issue: Events not firing in WebAssembly
**Solution:** Ensure component has `@rendermode InteractiveWebAssembly`

### Issue: Server slowness with DatePicker
**Solution:** Consider WebAssembly for UI-heavy operations

### Issue: Offline functionality needed
**Solution:** Migrate to WebAssembly or Blazor Web App

### Issue: Can't access server from WebAssembly
**Solution:** Create backend API, call via HttpClient

## Next Steps

- **Getting Started:** Review [installation and setup](getting-started.md)
- **Performance:** Optimize [event handling and data binding](events.md)
- **Advanced:** Explore [multiple render modes in Web App](advanced-features.md)
