# Troubleshooting Blazor DatePicker

Common issues, solutions, and debugging strategies for DatePicker implementation.

## Installation and Setup Issues

### Issue: NuGet Package Not Found

**Error:**
```
Package 'Syncfusion.Blazor.Calendars' not found
```

**Solutions:**

1. Check NuGet.org source:
```bash
dotnet nuget add source https://api.nuget.org/v3/index.json --name nuget.org
```

2. Run restore:
```bash
dotnet restore
```

3. Verify package version:
```bash
dotnet package search Syncfusion.Blazor.Calendars
```

### Issue: "AddSyncfusionBlazor Not Found"

**Error:**
```
'IServiceCollection' does not contain a definition for 'AddSyncfusionBlazor'
```

**Solutions:**

1. Add namespace to `Program.cs`:
```csharp
using Syncfusion.Blazor;

builder.Services.AddSyncfusionBlazor();
```

2. Verify package is installed:
```bash
dotnet package list
```

### Issue: Syncfusion Script Not Loading

**Symptom:** Component renders but no styling, calendar popup doesn't work

**Solutions:**

1. Check `App.razor` has theme CSS:
```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

2. For WebAssembly, add to `index.html`:
```html
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />
```

3. Clear browser cache and rebuild:
```bash
dotnet clean
dotnet build
```

## Rendering and Display Issues

### Issue: Component Not Visible

**Symptom:** DatePicker renders but input field appears empty or missing

**Solutions:**

1. Verify container has width:
```razor
<div style="width: 300px;">
    <SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>
</div>
```

2. Check CSS conflicts:
```razor
<style>
    .e-input-group {
        display: block !important;
    }
</style>
```

3. Ensure theme CSS loaded by inspecting DevTools:
```
Press F12 → Network → search "bootstrap5.css"
Should show 200 status code
```

### Issue: Styling Broken or Misaligned

**Symptom:** DatePicker appears unstyled or misaligned

**Solutions:**

1. Verify correct theme file:
```html
<!-- ✅ Correct -->
<link href="_content/Syncfusion.Blazor.Themes/bootstrap5.css" rel="stylesheet" />

<!-- ❌ Incorrect -->
<link href="_content/Syncfusion.Themes/bootstrap5.css" rel="stylesheet" />
```

2. Load Syncfusion script before using DatePicker:
```html
<script src="_content/Syncfusion.Blazor.Core/scripts/syncfusion-blazor.min.js"></script>
```

3. Disable conflicting CSS:
```razor
<style>
    .e-datepicker {
        box-sizing: border-box;
    }
</style>
```

### Issue: Calendar Popup Doesn't Open

**Symptom:** Clicking DatePicker input doesn't open calendar

**Solutions:**

1. Verify Syncfusion script loaded:
```razor
@if (!isScriptLoaded)
{
    <p>Loading components...</p>
}
else
{
    <SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>
}

@code {
    bool isScriptLoaded = true;
}
```

2. Check browser console (F12) for JavaScript errors

3. Verify render mode is interactive:
```razor
@rendermode InteractiveServer
<!-- Not SSR (Static) -->
```

## Data Binding Issues

### Issue: Value Not Updating

**Symptom:** DatePicker shows value but doesn't update when user selects date

**Solutions:**

1. Use `@bind-Value` for two-way binding:
```razor
<!-- ✅ Correct -->
<SfDatePicker TValue="DateTime?" @bind-Value="@selectedDate"></SfDatePicker>

<!-- ❌ Wrong (one-way only) -->
<SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>
```

2. For one-way with updates, use `ValueChange`:
```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              ValueChange="@OnDateChange"></SfDatePicker>

@code {
    void OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        selectedDate = args.Value;
    }
}
```

### Issue: Null Value Causes Error

**Error:**
```
NullReferenceException: Object reference not set to an instance of an object
```

**Solutions:**

1. Use `DateTime?` (nullable):
```razor
<!-- ✅ Correct -->
<SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>

@code {
    DateTime? selectedDate; // null initially, no error
}
```

2. Check for null before using:
```razor
@if (selectedDate.HasValue)
{
    <p>Selected: @selectedDate.Value.ToString("d")</p>
}
else
{
    <p>No date selected</p>
}

@code {
    DateTime? selectedDate;
}
```

### Issue: Value Not Persisting

**Symptom:** Value resets after navigation or page refresh

**Solutions:**

1. Use component state management or local storage:
```razor
<SfDatePicker TValue="DateTime?" @bind-Value="@selectedDate"></SfDatePicker>

@code {
    DateTime? selectedDate;

    protected override async Task OnInitializedAsync()
    {
        var saved = await localStorage.GetItemAsync("date");
        if (!string.IsNullOrEmpty(saved))
        {
            selectedDate = DateTime.Parse(saved);
        }
    }

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (selectedDate.HasValue)
        {
            await localStorage.SetItemAsync("date", selectedDate.ToString());
        }
    }
}
```

2. Persist to database after selection:
```razor
<SfDatePicker TValue="DateTime?" ValueChange="@OnDateChange"></SfDatePicker>

@code {
    async Task OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        await SaveToDatabase(args.Value);
    }
}
```

## Date Format Issues

### Issue: Date Displays Incorrectly

**Symptom:** Selected date shows wrong date or time

**Solutions:**

1. Verify format string:
```razor
<!-- ✅ Correct -->
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              Format="dd/MM/yyyy">
</SfDatePicker>

<!-- Common format strings -->
<!-- dd/MM/yyyy = 18/03/2026 -->
<!-- MM/dd/yyyy = 03/18/2026 -->
<!-- yyyy-MM-dd = 2026-03-18 -->
```

2. Check system locale:
```csharp
var culture = System.Globalization.CultureInfo.CurrentCulture;
Console.WriteLine($"Culture: {culture.Name}");
```

### Issue: User Input Not Recognized

**Symptom:** DatePicker rejects typed dates

**Solutions:**

1. Align placeholder with expected format:
```razor
<SfDatePicker TValue="DateTime?" 
              Format="dd/MM/yyyy"
              Placeholder="DD/MM/YYYY">
</SfDatePicker>
```

2. Allow flexible parsing:
```razor
<!-- DatePicker attempts to parse multiple formats -->
<SfDatePicker TValue="DateTime?" Value="@selectedDate" 
              Format="dd/MM/yyyy">
</SfDatePicker>
<!-- User can type: 18/03/2026 or 18-03-2026 or other variations -->
```

### Issue: Invalid Date Accepted

**Symptom:** DatePicker accepts invalid dates (e.g., Feb 30)

**Solutions:**

1. Add validation with `EditForm`:
```razor
<EditForm Model="@formData">
    <DataAnnotationsValidator />
    
    <SfDatePicker TValue="DateTime?" @bind-Value="@formData.EventDate"></SfDatePicker>
    <ValidationMessage For="@(() => formData.EventDate)" />
    
    <button type="submit">Submit</button>
</EditForm>

@code {
    FormModel formData = new();

    class FormModel
    {
        [Required]
        [Range(typeof(DateTime?), "2026-01-01", "2026-12-31")]
        public DateTime? EventDate { get; set; }
    }
}
```

2. Manual validation:
```razor
<SfDatePicker TValue="DateTime?" ValueChange="@OnDateChange"></SfDatePicker>

@code {
    void OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        if (args.Value.HasValue)
        {
            if (IsValidDate(args.Value.Value))
            {
                // Accept
            }
            else
            {
                // Reject
            }
        }
    }

    bool IsValidDate(DateTime date)
    {
        return date >= DateTime.Now.Date && date <= DateTime.Now.AddYears(1);
    }
}
```

## Event Handling Issues

### Issue: ValueChange Event Not Firing

**Symptom:** `ValueChange` callback never executes

**Solutions:**

1. Verify event handler signature:
```razor
<!-- ✅ Correct -->
<SfDatePicker TValue="DateTime?" ValueChange="@OnDateChange"></SfDatePicker>

@code {
    void OnDateChange(ChangeEventArgs<DateTime?> args) { }
}

<!-- ❌ Wrong signature -->
void OnDateChange(DateTime? date) { } // Missing args wrapper
```

2. Ensure component is interactive:
```razor
<!-- ✅ Has @rendermode -->
@rendermode InteractiveServer
<SfDatePicker TValue="DateTime?" ValueChange="@OnDateChange"></SfDatePicker>

<!-- ❌ Static (no events) -->
<SfDatePicker TValue="DateTime?" ValueChange="@OnDateChange"></SfDatePicker>
```

### Issue: Event Fires Multiple Times

**Symptom:** `ValueChange` executes multiple times for single selection

**Solutions:**

1. Check for duplicate event subscriptions:
```razor
<SfDatePicker TValue="DateTime?" ValueChange="@OnDateChange"></SfDatePicker>

@code {
    private int callCount = 0;

    void OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        callCount++;
        Console.WriteLine($"Called {callCount} times"); // Debug
    }
}
```

2. Avoid re-renders in parent:
```razor
<!-- Don't re-render DatePicker if unnecessary -->
<SfDatePicker TValue="DateTime?" ValueChange="@OnDateChange" key="@DateTime.Now"></SfDatePicker>
```

### Issue: Async Event Causing Issues

**Symptom:** Async `ValueChange` causes UI freeze or errors

**Solutions:**

1. Use proper async signature:
```razor
<SfDatePicker TValue="DateTime?" ValueChange="@OnDateChange"></SfDatePicker>

@code {
    // For async operations, return Task
    async Task OnDateChange(ChangedEventArgs<DateTime?> args)
    {
        if (args.Value.HasValue)
        {
            await FetchDataAsync(args.Value.Value);
        }
    }

    async Task FetchDataAsync(DateTime date)
    {
        await Task.Delay(1000);
        StateHasChanged();
    }
}
```

2. Use `await` properly:
```razor
@code {
    async Task OnDateChange(ChangeEventArgs<DateTime?> args)
    {
        // ✅ Correct - await the async call
        await SaveToDatabase(args.Value);

        // ❌ Wrong - don't await Task
        Task.Run(() => SaveToDatabase(args.Value));
    }
}
```

## Performance Issues

### Issue: Slow Calendar Navigation

**Symptom:** Calendar is slow when switching months/years

**Solutions:**

1. Reduce component count:
```razor
<!-- ✅ Single DatePicker -->
<SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>

<!-- ❌ Many DatePickers (slow) -->
@for (int i = 0; i < 100; i++)
{
    <SfDatePicker TValue="DateTime?" Value="@selectedDate"></SfDatePicker>
}
```

2. Use virtualization for lists:
```razor
<Virtualize Items="@items">
    <ItemContent Context="item">
        <SfDatePicker TValue="DateTime?" Value="@item.Date"></SfDatePicker>
    </ItemContent>
</Virtualize>
```

### Issue: WebAssembly Slow Load Time

**Symptom:** Initial page load takes 5+ seconds

**Solutions:**

1. Bundle WASM files with caching headers
2. Use lazy-loaded modules
3. Tree-shake unused Syncfusion components

### Issue: Server Event Processing Slow

**Symptom:** ValueChange event takes 500ms+ to process

**Solutions:**

1. Offload to background task:
```razor
@code {
    void OnDateChange(ChangeEventArgs<DateTime?> args)
    {
        _ = ProcessDateAsync(args.Value); // Fire and forget
    }

    async Task ProcessDateAsync(DateTime? date)
    {
        await Task.Delay(100);
        // Heavy processing
    }
}
```

2. Use connection state:
```csharp
// In Blazor Server
if (circuitHandler.Circuit.State != CircuitState.Connected)
{
    // Handle disconnected state
}
```

## Accessibility Issues

### Issue: Screen Reader Not Announcing Date

**Solution:** Add ARIA labels:
```razor
<SfDatePicker TValue="DateTime?" 
              Value="@selectedDate"
              aria-label="Event date selection"
              aria-describedby="date-help">
</SfDatePicker>

<small id="date-help">Select a date using the calendar</small>
```

### Issue: Keyboard Navigation Not Working

**Solution:** Verify component is in tab order:
```razor
<SfDatePicker TValue="DateTime?" Value="@selectedDate" tabindex="0"></SfDatePicker>
```

## Mobile Issues

### Issue: Calendar Popup Too Small on Mobile

**Solution:** Set responsive width:
```razor
<SfDatePicker TValue="DateTime?" 
              Value="@selectedDate"
              PopupWidth="90vw">
</SfDatePicker>

<style>
    @media (max-width: 576px) {
        .e-datepicker .e-popup {
            width: 90vw !important;
        }
    }
</style>
```

### Issue: Touch Targets Too Small

**Solution:** Increase target size:
```razor
<style>
    .e-input-group {
        min-height: 48px;
        min-width: 48px;
    }
</style>
```

## Debugging Techniques

### Enable Debugging

```csharp
// Program.cs
builder.WebAssemblyHostBuilder.Logging.SetLevel(LogLevel.Debug);
```

### Console Logging

```razor
@code {
    void OnDateChange(ChangeEventArgs<DateTime?> args)
    {
        Console.WriteLine($"Date changed: {args.Value}");
        Console.WriteLine($"Event: {args}");
    }
}
```

### Browser DevTools

1. Press `F12` to open DevTools
2. Check Console tab for errors
3. Network tab to verify CSS/JS loading
4. Elements tab to inspect component HTML

### Visual Studio Debugger

```csharp
[Debugger.Break()] // Pause execution
void OnDateChange(ChangeEventArgs<DateTime?> args)
{
    selectedDate = args.Value;
}
```

## Getting Help

- Check [Syncfusion Docs](https://www.syncfusion.com/blazor-components/blazor-datepicker)
- Search [Stack Overflow](https://stackoverflow.com/questions/tagged/blazor+datepicker)
- Open [GitHub Issue](https://github.com/syncfusionexamples)
- Contact Syncfusion Support

## Next Steps

- **Advanced:** Explore [multi-calendar and globalization](advanced-features.md)
- **Styling:** Apply [custom themes and appearance](styling-and-appearance.md)
- **Server vs WASM:** Review [architecture differences](server-vs-webassembly.md)
