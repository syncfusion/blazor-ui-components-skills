# Chip Component - Advanced Features

## Table of Contents
- [Accessibility](#accessibility)
- [Keyboard Navigation](#keyboard-navigation)
- [State Management](#state-management)
- [Performance Optimization](#performance-optimization)
- [Testing Strategies](#testing-strategies)

---

## Accessibility

### ARIA Attributes

```razor
<SfChip EnableDelete="true" Selection="SelectionType.Multiple">
    <ChipItems>
        <ChipItem Text="Option 1" CssClass="accessible-chip"></ChipItem>
        <ChipItem Text="Option 2" CssClass="accessible-chip"></ChipItem>
    </ChipItems>
</SfChip>

<style>
    .accessible-chip[aria-selected="true"] {
        outline: 2px solid #0078d4;
        outline-offset: 2px;
    }
</style>
```

### Screen Reader Support

```razor
<SfChip EnableDelete="true">
    <ChipItems>
        <ChipItem Text="Chip 1" 
                  LeadingIconCss="e-icons e-close"
                  CssClass="sr-friendly">
        </ChipItem>
    </ChipItems>
</SfChip>

<style>
    .sr-only {
        position: absolute;
        width: 1px;
        height: 1px;
        padding: 0;
        margin: -1px;
        overflow: hidden;
        clip: rect(0,0,0,0);
        white-space: nowrap;
        border: 0;
    }
</style>
```

### Focus Management

```razor
<SfChip Selection="SelectionType.Single" CssClass="focus-visible">
    <ChipItems>
        <ChipItem Text="Focusable 1"></ChipItem>
        <ChipItem Text="Focusable 2"></ChipItem>
        <ChipItem Text="Focusable 3"></ChipItem>
    </ChipItems>
</SfChip>

<style>
    .focus-visible .e-chip:focus-visible {
        outline: 2px solid #0078d4;
        outline-offset: 2px;
        box-shadow: 0 0 0 4px rgba(0, 120, 212, 0.1);
    }
</style>
```

---

## Keyboard Navigation

### Tab Navigation

Chips support standard keyboard navigation:
- **Tab**: Move focus to chip list
- **Arrow Keys**: Navigate between chips
- **Enter/Space**: Select chip or trigger action
- **Delete**: Remove chip (if EnableDelete="true")
- **Escape**: Cancel chip input

### Custom Keyboard Handling

```razor
<SfChip @ref="chipRef" Selection="SelectionType.Single">
    <ChipItems>
        <ChipItem Text="Chip 1"></ChipItem>
        <ChipItem Text="Chip 2"></ChipItem>
        <ChipItem Text="Chip 3"></ChipItem>
    </ChipItems>
</SfChip>

@code {
    private SfChip<ChipItem> chipRef;

    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        if (firstRender)
        {
            // Set focus to first chip
            await chipRef.FocusAsync();
        }
    }
}
```

---

## State Management

### Selection State

```razor
<SfChip Selection="SelectionType.Multiple">
    <ChipEvents TValue="ChipItem" OnClick="OnChipClick"></ChipEvents>
    <ChipItems>
        @foreach (var item in items)
        {
            <ChipItem Text="@item.Text" 
                      Value="@item.Id"
                      Enabled="@item.IsSelected">
            </ChipItem>
        }
    </ChipItems>
</SfChip>

<button @onclick="SaveSelection">Save Selection</button>
<button @onclick="LoadSelection">Load Selection</button>

@code {
    private List<ChipModel> items = new()
    {
        new ChipModel { Id = 1, Text = "Option 1", IsSelected = false },
        new ChipModel { Id = 2, Text = "Option 2", IsSelected = false },
        new ChipModel { Id = 3, Text = "Option 3", IsSelected = false }
    };

    private List<int> savedSelection = new();

    private void OnChipClick(ClickEventArgs<ChipItem> args)
    {
        var item = items.FirstOrDefault(i => i.Id == (int)args.Value);
        if (item != null)
            item.IsSelected = args.Selected;
    }

    private void SaveSelection()
    {
        savedSelection = items.Where(i => i.IsSelected).Select(i => i.Id).ToList();
        // Save to local storage or database
    }

    private void LoadSelection()
    {
        foreach (var item in items)
        {
            item.IsSelected = savedSelection.Contains(item.Id);
        }
        StateHasChanged();
    }

    public class ChipModel
    {
        public int Id { get; set; }
        public string Text { get; set; }
        public bool IsSelected { get; set; }
    }
}
```

### Dynamic Updates

```razor
<SfChip EnableDelete="true">
    <ChipEvents TValue="ChipItem" Deleted="OnDeleted"></ChipEvents>
    <ChipItems>
        @foreach (var chip in chips)
        {
            <ChipItem Text="@chip.Text" Value="@chip.Id"></ChipItem>
        }
    </ChipItems>
</SfChip>

<button @onclick="AddRandomChip">Add Random Chip</button>

@code {
    private List<ChipData> chips = new()
    {
        new ChipData { Id = 1, Text = "Chip 1" },
        new ChipData { Id = 2, Text = "Chip 2" }
    };

    private int nextId = 3;

    private void AddRandomChip()
    {
        chips.Add(new ChipData 
        { 
            Id = nextId++, 
            Text = $"Chip {nextId}" 
        });
        StateHasChanged();
    }

    private void OnDeleted(DeleteEventArgs<ChipItem> args)
    {
        var chip = chips.FirstOrDefault(c => c.Id == (int)args.Value);
        if (chip != null)
            chips.Remove(chip);
    }

    public class ChipData
    {
        public int Id { get; set; }
        public string Text { get; set; }
    }
}
```

---

## Performance Optimization

### Virtualization for Large Lists

```razor
@if (chips.Count > 50)
{
    <div class="chip-virtualized" style="max-height: 300px; overflow-y: auto;">
        <SfChip EnableDelete="true">
            <ChipItems>
                @foreach (var chip in visibleChips)
                {
                    <ChipItem Text="@chip"></ChipItem>
                }
            </ChipItems>
        </SfChip>
    </div>
}
else
{
    <SfChip EnableDelete="true">
        <ChipItems>
            @foreach (var chip in chips)
            {
                <ChipItem Text="@chip"></ChipItem>
            }
        </ChipItems>
    </SfChip>
}

@code {
    private List<string> chips = Enumerable.Range(1, 100).Select(i => $"Chip {i}").ToList();
    private List<string> visibleChips => chips.Take(20).ToList();
}
```

### Debounced Selection

```razor
<SfChip Selection="SelectionType.Multiple">
    <ChipEvents TValue="ChipItem" OnClick="OnChipClickDebounced"></ChipEvents>
    <ChipItems>
        @foreach (var item in items)
        {
            <ChipItem Text="@item"></ChipItem>
        }
    </ChipItems>
</SfChip>

@code {
    private List<string> items = new() { "Item 1", "Item 2", "Item 3", "Item 4", "Item 5" };
    private System.Threading.Timer debounceTimer;

    private void OnChipClickDebounced(ClickEventArgs<ChipItem> args)
    {
        debounceTimer?.Dispose();
        debounceTimer = new System.Threading.Timer(_ =>
        {
            InvokeAsync(() =>
            {
                ProcessSelection(args);
                StateHasChanged();
            });
        }, null, 300, Timeout.Infinite);
    }

    private void ProcessSelection(ClickEventArgs<ChipItem> args)
    {
        // Heavy processing logic here
        Console.WriteLine($"Processed: {args.Text}");
    }

    public void Dispose()
    {
        debounceTimer?.Dispose();
    }
}
```

---

## Testing Strategies

### Unit Testing

```csharp
using Bunit;
using Xunit;
using Syncfusion.Blazor.Buttons;

public class ChipTests : TestContext
{
    [Fact]
    public void ChipRendersCorrectly()
    {
        // Arrange
        var cut = RenderComponent<SfChip<ChipItem>>(parameters => parameters
            .Add(p => p.EnableDelete, true)
            .AddChildContent<ChipItems>(items => items
                .AddChildContent<ChipItem>(item => item
                    .Add(p => p.Text, "Test Chip"))));

        // Assert
        cut.Find(".e-chip").TextContent.Should().Contain("Test Chip");
    }

    [Fact]
    public void ChipDeleteEventTriggered()
    {
        // Arrange
        bool deleted = false;
        var cut = RenderComponent<SfChip<ChipItem>>(parameters => parameters
            .Add(p => p.EnableDelete, true)
            .Add(p => p.ChipEvents, new ChipEvents<ChipItem>
            {
                Deleted = args => deleted = true
            }));

        // Act
        cut.Find(".e-chip-delete").Click();

        // Assert
        deleted.Should().BeTrue();
    }
}
```

### Integration Testing

```csharp
[Fact]
public async Task ChipSelectionPersists()
{
    // Arrange
    var component = RenderComponent<ChipComponent>();
    var chip = component.Find(".e-chip");

    // Act
    await chip.ClickAsync(new Microsoft.AspNetCore.Components.Web.MouseEventArgs());

    // Assert
    chip.ClassList.Should().Contain("e-active");
}
```

---

## Common Patterns

### Tag Input with Autocomplete

```razor
<div class="tag-autocomplete">
    <SfChip EnableDelete="true">
        <ChipEvents TValue="ChipItem" Deleted="RemoveTag"></ChipEvents>
        <ChipItems>
            @foreach (var tag in selectedTags)
            {
                <ChipItem Text="@tag"></ChipItem>
            }
        </ChipItems>
    </SfChip>
    
    <input @bind="searchText" 
           @oninput="OnSearchInput"
           @onkeydown="OnKeyDown" 
           placeholder="Type to search..." />
    
    @if (suggestions.Any())
    {
        <ul class="suggestions">
            @foreach (var suggestion in suggestions)
            {
                <li @onclick="() => AddTag(suggestion)">@suggestion</li>
            }
        </ul>
    }
</div>

@code {
    private List<string> selectedTags = new();
    private string searchText = "";
    private List<string> suggestions = new();
    private List<string> allTags = new() { "blazor", "csharp", "dotnet", "aspnet", "webassembly" };

    private void OnSearchInput(ChangeEventArgs e)
    {
        searchText = e.Value?.ToString() ?? "";
        suggestions = allTags
            .Where(t => t.Contains(searchText, StringComparison.OrdinalIgnoreCase) 
                     && !selectedTags.Contains(t))
            .ToList();
    }

    private void OnKeyDown(KeyboardEventArgs e)
    {
        if (e.Key == "Enter" && !string.IsNullOrWhiteSpace(searchText))
        {
            AddTag(searchText);
        }
    }

    private void AddTag(string tag)
    {
        if (!selectedTags.Contains(tag))
        {
            selectedTags.Add(tag);
            searchText = "";
            suggestions.Clear();
        }
    }

    private void RemoveTag(DeleteEventArgs<ChipItem> args)
    {
        selectedTags.Remove(args.Text);
    }
}
```

---

## Best Practices

1. **Accessibility First** - Always include proper ARIA attributes
2. **Keyboard Support** - Ensure all interactions work with keyboard
3. **Performance** - Use virtualization for large chip collections
4. **State Management** - Properly handle chip selection and deletion state
5. **Testing** - Write unit tests for chip interactions
6. **Focus Management** - Maintain logical focus order

---

## Next Steps

- Return to [chip-features.md](chip-features.md) for basic usage
- Explore [chip-styling.md](chip-styling.md) for customization
