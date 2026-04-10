# Button Group Component - Advanced Features

## Table of Contents
- [Accessibility Features](#accessibility-features)
- [Keyboard Navigation](#keyboard-navigation)
- [Selection State Management](#selection-state-management)
- [Event Handling Patterns](#event-handling-patterns)
- [Performance Optimization](#performance-optimization)
- [Best Practices](#best-practices)
- [Testing Strategies](#testing-strategies)
- [Common Patterns](#common-patterns)

---

## Accessibility Features

### ARIA Attributes

```razor
<SfButtonGroup HtmlAttributes="@groupAttributes" Mode="SelectionMode.Single">
    <ButtonGroupButton @onclick="() => Select(1)"
                       Selected="@(selected == 1)"
                       HtmlAttributes="@GetButtonAttributes(1)">
        Option 1
    </ButtonGroupButton>
    <ButtonGroupButton @onclick="() => Select(2)"
                       Selected="@(selected == 2)"
                       HtmlAttributes="@GetButtonAttributes(2)">
        Option 2
    </ButtonGroupButton>
</SfButtonGroup>

@code {
    private int selected = 1;

    private Dictionary<string, object> groupAttributes = new()
    {
        { "role", "group" },
        { "aria-label", "Selection options" }
    };

    private Dictionary<string, object> GetButtonAttributes(int option)
    {
        return new Dictionary<string, object>
        {
            { "aria-pressed", (selected == option).ToString().ToLower() },
            { "aria-label", $"Option {option}" }
        };
    }

    private void Select(int option) => selected = option;
}
```

### Screen Reader Support

```razor
<SfButtonGroup HtmlAttributes="@groupAttrs">
    <ButtonGroupButton IconCss="e-icons e-bold"
                       HtmlAttributes="@boldAttrs">
        <span class="sr-only">Bold</span>
    </ButtonGroupButton>
    <ButtonGroupButton IconCss="e-icons e-italic"
                       HtmlAttributes="@italicAttrs">
        <span class="sr-only">Italic</span>
    </ButtonGroupButton>
</SfButtonGroup>

@code {
    private Dictionary<string, object> groupAttrs = new()
    {
        { "role", "toolbar" },
        { "aria-label", "Text formatting" }
    };

    private Dictionary<string, object> boldAttrs = new()
    {
        { "aria-label", "Bold text" },
        { "title", "Bold (Ctrl+B)" }
    };

    private Dictionary<string, object> italicAttrs = new()
    {
        { "aria-label", "Italic text" },
        { "title", "Italic (Ctrl+I)" }
    };
}

<style>
    .sr-only {
        position: absolute;
        width: 1px;
        height: 1px;
        padding: 0;
        margin: -1px;
        overflow: hidden;
        clip: rect(0,0,0,0);
        border: 0;
    }

    .e-bold::before { content: '\e736'; }
    .e-italic::before { content: '\e737'; }
</style>
```

### Focus Management

```razor
<SfButtonGroup CssClass="focus-styled">
    <ButtonGroupButton @ref="firstButton">First</ButtonGroupButton>
    <ButtonGroupButton>Second</ButtonGroupButton>
    <ButtonGroupButton>Third</ButtonGroupButton>
</SfButtonGroup>

<SfButton OnClick="FocusFirst">Focus First Button</SfButton>

@code {
    private ButtonGroupButton firstButton;

    private async Task FocusFirst()
    {
        // Focus management logic
        await Task.CompletedTask;
    }
}

<style>
    .focus-styled .e-btn:focus {
        outline: 3px solid #4A90E2;
        outline-offset: 2px;
        box-shadow: 0 0 0 4px rgba(74, 144, 226, 0.2);
    }

    .focus-styled .e-btn:focus:not(:focus-visible) {
        outline: none;
        box-shadow: none;
    }
</style>
```

---

## Keyboard Navigation

### Arrow Key Navigation

```razor
<SfButtonGroup @onkeydown="HandleKeyDown">
    <ButtonGroupButton @ref="button1" @onfocus="() => focusedIndex = 0">
        Button 1
    </ButtonGroupButton>
    <ButtonGroupButton @ref="button2" @onfocus="() => focusedIndex = 1">
        Button 2
    </ButtonGroupButton>
    <ButtonGroupButton @ref="button3" @onfocus="() => focusedIndex = 2">
        Button 3
    </ButtonGroupButton>
</SfButtonGroup>

@code {
    private ButtonGroupButton button1, button2, button3;
    private int focusedIndex = 0;
    private ButtonGroupButton[] buttons => new[] { button1, button2, button3 };

    private void HandleKeyDown(KeyboardEventArgs e)
    {
        if (e.Key == "ArrowRight")
        {
            focusedIndex = (focusedIndex + 1) % buttons.Length;
            // Focus next button
        }
        else if (e.Key == "ArrowLeft")
        {
            focusedIndex = (focusedIndex - 1 + buttons.Length) % buttons.Length;
            // Focus previous button
        }
    }
}
```

### Keyboard Shortcuts

```razor
<SfButtonGroup @onkeydown="HandleShortcuts">
    <ButtonGroupButton>Bold (Ctrl+B)</ButtonGroupButton>
    <ButtonGroupButton>Italic (Ctrl+I)</ButtonGroupButton>
    <ButtonGroupButton>Underline (Ctrl+U)</ButtonGroupButton>
</SfButtonGroup>

@code {
    private void HandleShortcuts(KeyboardEventArgs e)
    {
        if (e.CtrlKey)
        {
            switch (e.Key.ToLower())
            {
                case "b":
                    ApplyBold();
                    break;
                case "i":
                    ApplyItalic();
                    break;
                case "u":
                    ApplyUnderline();
                    break;
            }
        }
    }

    private void ApplyBold() => Console.WriteLine("Bold applied");
    private void ApplyItalic() => Console.WriteLine("Italic applied");
    private void ApplyUnderline() => Console.WriteLine("Underline applied");
}
```

---

## Selection State Management

### Single Selection with State Persistence

```razor
@inject ProtectedLocalStorage LocalStorage

<SfButtonGroup Mode="SelectionMode.Single">
    <ButtonGroupButton @onclick="() => SelectAndSave(1)"
                       Selected="@(selected == 1)">
        Option 1
    </ButtonGroupButton>
    <ButtonGroupButton @onclick="() => SelectAndSave(2)"
                       Selected="@(selected == 2)">
        Option 2
    </ButtonGroupButton>
    <ButtonGroupButton @onclick="() => SelectAndSave(3)"
                       Selected="@(selected == 3)">
        Option 3
    </ButtonGroupButton>
</SfButtonGroup>

@code {
    private int selected = 1;

    protected override async Task OnInitializedAsync()
    {
        var result = await LocalStorage.GetAsync<int>("selectedButton");
        selected = result.Success ? result.Value : 1;
    }

    private async Task SelectAndSave(int option)
    {
        selected = option;
        await LocalStorage.SetAsync("selectedButton", selected);
    }
}
```

### Multiple Selection with Complex State

```razor
<SfButtonGroup Mode="SelectionMode.Multiple">
    @foreach (var option in options)
    {
        <ButtonGroupButton @onclick="() => ToggleOption(option.Id)"
                           Selected="@selectedOptions.Contains(option.Id)">
            @option.Name
        </ButtonGroupButton>
    }
</SfButtonGroup>

<p>Selected: @string.Join(", ", GetSelectedNames())</p>

@code {
    private List<Option> options = new()
    {
        new Option { Id = 1, Name = "Bold" },
        new Option { Id = 2, Name = "Italic" },
        new Option { Id = 3, Name = "Underline" },
        new Option { Id = 4, Name = "Strikethrough" }
    };

    private HashSet<int> selectedOptions = new();

    private void ToggleOption(int id)
    {
        if (selectedOptions.Contains(id))
            selectedOptions.Remove(id);
        else
            selectedOptions.Add(id);
    }

    private IEnumerable<string> GetSelectedNames()
    {
        return options
            .Where(o => selectedOptions.Contains(o.Id))
            .Select(o => o.Name);
    }

    private class Option
    {
        public int Id { get; set; }
        public string Name { get; set; }
    }
}
```

### Conditional Selection Rules

```razor
<SfButtonGroup Mode="SelectionMode.Single">
    <ButtonGroupButton @onclick="() => TrySelect(1)"
                       Selected="@(selected == 1)"
                       Disabled="@(IsDisabled(1))">
        Option 1
    </ButtonGroupButton>
    <ButtonGroupButton @onclick="() => TrySelect(2)"
                       Selected="@(selected == 2)"
                       Disabled="@(IsDisabled(2))">
        Option 2
    </ButtonGroupButton>
    <ButtonGroupButton @onclick="() => TrySelect(3)"
                       Selected="@(selected == 3)"
                       Disabled="@(IsDisabled(3))">
        Option 3
    </ButtonGroupButton>
</SfButtonGroup>

@code {
    private int selected = 1;
    private HashSet<int> disabledOptions = new() { 2 };

    private bool IsDisabled(int option) => disabledOptions.Contains(option);

    private void TrySelect(int option)
    {
        if (!IsDisabled(option))
        {
            selected = option;
            // Apply business rules
            UpdateDisabledOptions();
        }
    }

    private void UpdateDisabledOptions()
    {
        // Example: Disable option 3 if option 1 is selected
        if (selected == 1)
            disabledOptions.Add(3);
        else
            disabledOptions.Remove(3);
    }
}
```

---

## Event Handling Patterns

### Centralized Event Handler

```razor
<SfButtonGroup>
    <ButtonGroupButton @onclick="() => HandleClick(\"Action1\")">
        Action 1
    </ButtonGroupButton>
    <ButtonGroupButton @onclick="() => HandleClick(\"Action2\")">
        Action 2
    </ButtonGroupButton>
    <ButtonGroupButton @onclick="() => HandleClick(\"Action3\")">
        Action 3
    </ButtonGroupButton>
</SfButtonGroup>

@code {
    private void HandleClick(string action)
    {
        Console.WriteLine($"Action: {action}");
        
        switch (action)
        {
            case "Action1":
                PerformAction1();
                break;
            case "Action2":
                PerformAction2();
                break;
            case "Action3":
                PerformAction3();
                break;
        }
    }

    private void PerformAction1() { }
    private void PerformAction2() { }
    private void PerformAction3() { }
}
```

### Event Bubbling Pattern

```razor
<div @onclick="HandleGroupClick">
    <SfButtonGroup>
        <ButtonGroupButton data-action="save">Save</ButtonGroupButton>
        <ButtonGroupButton data-action="load">Load</ButtonGroupButton>
        <ButtonGroupButton data-action="delete">Delete</ButtonGroupButton>
    </SfButtonGroup>
</div>

@code {
    private void HandleGroupClick(MouseEventArgs e)
    {
        // Handle click at group level
        Console.WriteLine("Group clicked");
    }
}
```

### Async Event Handling

```razor
<SfButtonGroup>
    <ButtonGroupButton @onclick="async () => await HandleAsyncAction(1)"
                       Disabled="@isProcessing">
        @(isProcessing && currentAction == 1 ? "Processing..." : "Action 1")
    </ButtonGroupButton>
    <ButtonGroupButton @onclick="async () => await HandleAsyncAction(2)"
                       Disabled="@isProcessing">
        @(isProcessing && currentAction == 2 ? "Processing..." : "Action 2")
    </ButtonGroupButton>
</SfButtonGroup>

@code {
    private bool isProcessing = false;
    private int currentAction = 0;

    private async Task HandleAsyncAction(int action)
    {
        isProcessing = true;
        currentAction = action;

        try
        {
            await ProcessActionAsync(action);
        }
        finally
        {
            isProcessing = false;
            currentAction = 0;
        }
    }

    private async Task ProcessActionAsync(int action)
    {
        await Task.Delay(2000);
        Console.WriteLine($"Action {action} completed");
    }
}
```

---

## Performance Optimization

### Debounced Selection

```razor
<SfButtonGroup Mode="SelectionMode.Single">
    @foreach (var option in Enumerable.Range(1, 10))
    {
        <ButtonGroupButton @onclick="() => DebouncedSelect(option)"
                           Selected="@(selected == option)">
            @option
        </ButtonGroupButton>
    }
</SfButtonGroup>

@code {
    private int selected = 1;
    private System.Threading.Timer debounceTimer;

    private void DebouncedSelect(int option)
    {
        debounceTimer?.Dispose();
        debounceTimer = new System.Threading.Timer(_ =>
        {
            InvokeAsync(() =>
            {
                selected = option;
                ProcessSelection(option);
                StateHasChanged();
            });
        }, null, 300, Timeout.Infinite);
    }

    private void ProcessSelection(int option)
    {
        // Expensive operation
        Console.WriteLine($"Processing option {option}");
    }

    public void Dispose()
    {
        debounceTimer?.Dispose();
    }
}
```

### Virtualization for Large Groups

```razor
@using Microsoft.AspNetCore.Components.Web.Virtualization

<div class="button-container">
    <Virtualize Items="@buttons" Context="button">
        <ButtonGroupButton @onclick="() => HandleClick(button.Id)">
            @button.Text
        </ButtonGroupButton>
    </Virtualize>
</div>

@code {
    private List<ButtonModel> buttons = Enumerable.Range(1, 1000)
        .Select(i => new ButtonModel { Id = i, Text = $"Button {i}" })
        .ToList();

    private void HandleClick(int id)
    {
        Console.WriteLine($"Button {id} clicked");
    }

    private class ButtonModel
    {
        public int Id { get; set; }
        public string Text { get; set; }
    }
}
```

---

## Best Practices

### 1. Provide Clear Visual Feedback

```razor
<SfButtonGroup Mode="SelectionMode.Single" CssClass="visual-feedback">
    <ButtonGroupButton @onclick="() => selected = 1"
                       Selected="@(selected == 1)">
        Option 1
    </ButtonGroupButton>
    <ButtonGroupButton @onclick="() => selected = 2"
                       Selected="@(selected == 2)">
        Option 2
    </ButtonGroupButton>
</SfButtonGroup>

@code {
    private int selected = 1;
}

<style>
    .visual-feedback .e-btn.e-active {
        background-color: #4CAF50;
        color: white;
        box-shadow: inset 0 2px 4px rgba(0,0,0,0.2);
    }
</style>
```

### 2. Handle Edge Cases

```razor
<SfButtonGroup Mode="SelectionMode.Single">
    <ButtonGroupButton @onclick="() => SelectWithValidation(1)"
                       Selected="@(selected == 1)">
        Option 1
    </ButtonGroupButton>
    <ButtonGroupButton @onclick="() => SelectWithValidation(2)"
                       Selected="@(selected == 2)">
        Option 2
    </ButtonGroupButton>
</SfButtonGroup>

@code {
    private int selected = 1;

    private void SelectWithValidation(int option)
    {
        if (CanSelect(option))
        {
            selected = option;
            OnSelectionChanged(option);
        }
        else
        {
            ShowValidationMessage();
        }
    }

    private bool CanSelect(int option)
    {
        // Validation logic
        return true;
    }

    private void OnSelectionChanged(int option)
    {
        // Handle selection change
    }

    private void ShowValidationMessage()
    {
        // Show message to user
    }
}
```

### 3. Implement Proper Cleanup

```razor
<SfButtonGroup>
    <ButtonGroupButton @onclick="StartTimer">Start</ButtonGroupButton>
    <ButtonGroupButton @onclick="StopTimer">Stop</ButtonGroupButton>
</SfButtonGroup>

@code {
    private System.Threading.Timer timer;

    private void StartTimer()
    {
        timer = new System.Threading.Timer(_ =>
        {
            InvokeAsync(StateHasChanged);
        }, null, 0, 1000);
    }

    private void StopTimer()
    {
        timer?.Dispose();
        timer = null;
    }

    public void Dispose()
    {
        timer?.Dispose();
    }
}
```

---

## Testing Strategies

### Unit Testing Selection

```csharp
[Fact]
public void ButtonGroup_SingleSelection_UpdatesState()
{
    // Arrange
    var component = RenderComponent<ButtonGroupComponent>();
    var buttons = component.FindAll("button");

    // Act
    buttons[1].Click();

    // Assert
    Assert.True(buttons[1].ClassList.Contains("e-active"));
    Assert.False(buttons[0].ClassList.Contains("e-active"));
}
```

### Integration Testing

```csharp
[Fact]
public async Task ButtonGroup_MultipleSelection_AllowsMultiple()
{
    // Arrange
    var component = RenderComponent<MultiSelectButtonGroup>();
    var buttons = component.FindAll("button");

    // Act
    buttons[0].Click();
    buttons[1].Click();

    // Assert
    Assert.True(buttons[0].ClassList.Contains("e-active"));
    Assert.True(buttons[1].ClassList.Contains("e-active"));
}
```

---

## Common Patterns

### Wizard Navigation

```razor
<SfButtonGroup>
    <ButtonGroupButton Disabled="@(currentStep == 1)"
                       @onclick="() => GoToStep(1)">
        Step 1
    </ButtonGroupButton>
    <ButtonGroupButton Disabled="@(currentStep < 2)"
                       @onclick="() => GoToStep(2)">
        Step 2
    </ButtonGroupButton>
    <ButtonGroupButton Disabled="@(currentStep < 3)"
                       @onclick="() => GoToStep(3)">
        Step 3
    </ButtonGroupButton>
</SfButtonGroup>

@code {
    private int currentStep = 1;

    private void GoToStep(int step)
    {
        if (step <= currentStep)
        {
            currentStep = step;
        }
    }
}
```

### Filter Controls

```razor
<SfButtonGroup Mode="SelectionMode.Multiple">
    <ButtonGroupButton @onclick="() => ToggleFilter(\"active\")"
                       Selected="@filters.Contains(\"active\")">
        Active
    </ButtonGroupButton>
    <ButtonGroupButton @onclick="() => ToggleFilter(\"pending\")"
                       Selected="@filters.Contains(\"pending\")">
        Pending
    </ButtonGroupButton>
    <ButtonGroupButton @onclick="() => ToggleFilter(\"completed\")"
                       Selected="@filters.Contains(\"completed\")">
        Completed
    </ButtonGroupButton>
</SfButtonGroup>

@code {
    private HashSet<string> filters = new();

    private void ToggleFilter(string filter)
    {
        if (filters.Contains(filter))
            filters.Remove(filter);
        else
            filters.Add(filter);

        ApplyFilters();
    }

    private void ApplyFilters()
    {
        // Filter data based on selected filters
    }
}
```

---

## Next Steps

- Review [button-group-features.md](button-group-features.md) for basic functionality
- Explore [button-group-styling.md](button-group-styling.md) for customization
