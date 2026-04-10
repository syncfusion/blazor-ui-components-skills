# Events & Interactions

The Syncfusion TextBox component provides comprehensive event handling for capturing user interactions and lifecycle changes.

## Focus Event

The `Focus` event triggers when the TextBox receives focus (user clicks or tabs into it).

```razor
@using Syncfusion.Blazor.Inputs

<SfTextBox Focus="@OnFocus"
           Placeholder="Click to focus">
</SfTextBox>

<p>Focus count: @FocusCount</p>

@code {
    private int FocusCount = 0;

    private void OnFocus(FocusInEventArgs args)
    {
        FocusCount++;
        Console.WriteLine($"TextBox focused. Total: {FocusCount}");
    }
}
```

**Use Cases:**
- Clear placeholder or show hints
- Enable validation display
- Show related suggestions
- Load autocomplete data

---

## Blur Event

The `Blur` event triggers when the TextBox loses focus (user clicks away or tabs out).

```razor
@using Syncfusion.Blazor.Inputs

<SfTextBox @bind-Value="@Email"
           Blur="@OnBlur"
           Placeholder="Enter email">
</SfTextBox>

<p>Status: @ValidationMessage</p>

@code {
    private string Email = "";
    private string ValidationMessage = "";

    private void OnBlur(FocusOutEventArgs args)
    {
        // Validate on blur
        if (Email.Length == 0)
        {
            ValidationMessage = "Email is required";
        }
        else if (!Email.Contains("@"))
        {
            ValidationMessage = "Invalid email format";
        }
        else
        {
            ValidationMessage = "✓ Valid";
        }
    }
}
```

**Use Cases:**
- Validate input on blur
- Trigger data save
- Clear temporary UI (hints, suggestions)
- Submit form automatically

---

## Input Event

The `Input` event triggers on every keystroke or text change in real-time.

```razor
@using Syncfusion.Blazor.Inputs

<SfTextBox @bind-Value="@SearchTerm"
           Input="@OnInput"
           Placeholder="Search...">
</SfTextBox>

<p>Typed: @SearchTerm</p>
<p>Character count: @SearchTerm.Length / 50</p>

@code {
    private string SearchTerm = "";

    private void OnInput(InputEventArgs args)
    {
        SearchTerm = args.Value;
        Console.WriteLine($"Real-time input: {SearchTerm}");
        // Perform real-time search, validation, filtering, etc.
    }
}
```

**Use Cases:**
- Real-time search/filtering
- Live character count display
- Dynamic validation feedback
- Autocomplete suggestions
- Input formatting

**Important:** `Input` fires frequently (every keystroke). For expensive operations, consider debouncing.

---

## ValueChange Event

The `ValueChange` event triggers when the TextBox value is committed and has changed (typically on blur after modification).

```razor
@using Syncfusion.Blazor.Inputs

<SfTextBox Value="@Username"
           ValueChange="@OnValueChange"
           Placeholder="Username">
</SfTextBox>

<p>Username (saved): @SavedUsername</p>

@code {
    private string Username = "";
    private string SavedUsername = "";

    private void OnValueChange(ChangedEventArgs args)
    {
        Username = args.Value; // Manual update when using ValueChange
        SavedUsername = args.Value;
        Console.WriteLine($"Value committed: {SavedUsername}");
        // Save to database, update parent component, etc.
    }
}
```

**Comparison: Input vs ValueChange**

| Event | Frequency | Use Case |
|-------|-----------|----------|
| **Input** | Every keystroke | Real-time UI updates |
| **ValueChange** | On commit (blur/enter) | Data persistence |

---

## ValueChanged Parameter

The `ValueChanged` event is a parameter callback fired when the `Value` property changes. **DO NOT use with `@bind-Value`** - use `Value` property only and manually update in the event handler:

```razor
@using Syncfusion.Blazor.Inputs

<SfTextBox Value="@FirstName"
           ValueChanged="@OnFirstNameChanged"
           Placeholder="First Name">
</SfTextBox>

<p>First Name: @FirstName</p>

@code {
    private string FirstName = "";

    private async Task OnFirstNameChanged(string value)
    {
        FirstName = value;
        // Async operations after value change
        await ValidateFirstName(value);
    }

    private async Task ValidateFirstName(string name)
    {
        // Async validation logic
        Console.WriteLine($"Validating: {name}");
    }
}
```

**Key Differences:**

- **@bind-Value**: Automatic two-way binding (DO NOT mix with ValueChanged)
- **ValueChanged**: Manual value update required + Async callback for custom logic
- **ValueChange**: Synchronous event on commit (use with @bind-Value)

---

## Created Event

The `Created` event triggers after the TextBox component is initialized and rendered in the DOM.

```razor
@using Syncfusion.Blazor.Inputs

<SfTextBox @ref="TextBoxRef"
           Created="@OnCreated"
           Placeholder="TextBox ready">
</SfTextBox>

@code {
    private SfTextBox TextBoxRef;

    private async void OnCreated()
    {
        Console.WriteLine("TextBox created and ready!");
        
        // Now safe to call component methods
        if (TextBoxRef != null)
        {
            await TextBoxRef.AddIconAsync("append", "e-icons e-search");
        }
    }
}
```

**Use Cases:**
- Add icons programmatically
- Set initial focus
- Load autocomplete data
- Initialize third-party libraries

---

## Destroyed Event

The `Destroyed` event triggers when the TextBox component is disposed or removed from the DOM.

```razor
@using Syncfusion.Blazor.Inputs

<SfTextBox Destroyed="@OnDestroyed"
           Placeholder="Monitor destruction">
</SfTextBox>

@code {
    private void OnDestroyed()
    {
        Console.WriteLine("TextBox destroyed and cleaned up");
        // Cleanup resources, timers, event listeners, etc.
    }
}
```

**Use Cases:**
- Clean up resources
- Cancel pending operations
- Remove event listeners
- Log component lifecycle

---

## Common Event Patterns

### Real-time Search with Input Event

```razor
@using Syncfusion.Blazor.Inputs

<SfTextBox @bind-Value="@SearchQuery"
           Input="@OnSearchInput"
           Placeholder="Search users...">
</SfTextBox>

<div>
    @if (SearchResults.Count > 0)
    {
        <ul>
            @foreach (var result in SearchResults)
            {
                <li>@result</li>
            }
        </ul>
    }
    else if (!string.IsNullOrEmpty(SearchQuery))
    {
        <p>No results found</p>
    }
</div>

@code {
    private string SearchQuery = "";
    private List<string> AllUsers = new() { "Alice", "Bob", "Charlie", "Diana" };
    private List<string> SearchResults = new();

    private void OnSearchInput(InputEventArgs args)
    {
        SearchQuery = args.Value;
        
        if (SearchQuery.Length >= 2)
        {
            SearchResults = AllUsers
                .Where(u => u.Contains(SearchQuery, StringComparison.OrdinalIgnoreCase))
                .ToList();
        }
        else
        {
            SearchResults.Clear();
        }
    }
}
```

### Form Validation with Blur Event

```razor
@using Syncfusion.Blazor.Inputs

<style>
    .error { color: red; }
    .success { color: green; }
</style>

<div style="width: 300px; margin: 50px auto;">
    <SfTextBox @bind-Value="@Email"
               Blur="@ValidateEmail"
               Placeholder="Email"
               CssClass="@(EmailValid == null ? "" : EmailValid == true ? "e-success" : "e-error")">
    </SfTextBox>
    @if (EmailValid == false)
    {
        <p class="error">Invalid email format</p>
    }
    else if (EmailValid == true)
    {
        <p class="success">✓ Valid email</p>
    }

    <SfTextBox @bind-Value="@Password"
               Blur="@ValidatePassword"
               Placeholder="Password"
               CssClass="@(PasswordValid == null ? "" : PasswordValid == true ? "e-success" : "e-error")"
               HtmlAttributes="@new Dictionary<string, object> { { \"type\", \"password\" } }">
    </SfTextBox>
    @if (PasswordValid == false)
    {
        <p class="error">Password too short (min 8 chars)</p>
    }
    else if (PasswordValid == true)
    {
        <p class="success">✓ Valid password</p>
    }

    <button @onclick="Submit" 
            disabled="@(EmailValid != true || PasswordValid != true)">
        Submit
    </button>
</div>

@code {
    private string Email = "";
    private string Password = "";
    private bool? EmailValid = null;
    private bool? PasswordValid = null;

    private void ValidateEmail(FocusOutEventArgs args)
    {
        EmailValid = Email.Contains("@") && Email.Contains(".");
    }

    private void ValidatePassword(FocusOutEventArgs args)
    {
        PasswordValid = Password.Length >= 8;
    }

    private void Submit()
    {
        if (EmailValid == true && PasswordValid == true)
        {
            Console.WriteLine($"Submitting: {Email}");
        }
    }
}
```

### Programmatic Value Update with ValueChanged

```razor
@using Syncfusion.Blazor.Inputs

<SfTextBox Value="@UserName"
           ValueChanged="@UpdateUserName"
           Placeholder="User Name">
</SfTextBox>

<button @onclick="@(() => UserName = \"DefaultUser\")">
    Reset to Default
</button>

<p>Current: @UserName</p>

@code {
    private string UserName = "";

    private async Task UpdateUserName(string value)
    {
        UserName = value;
        // Async update (e.g., save to database)
        await SaveUserNameToDatabase(value);
    }

    private async Task SaveUserNameToDatabase(string name)
    {
        Console.WriteLine($"Saving user name: {name}");
        // Simulate API call
        await Task.Delay(500);
    }
}
```

### Icon TextBox with Created Event

```razor
@using Syncfusion.Blazor.Inputs

<SfTextBox @ref="SearchBox"
           @bind-Value="@SearchTerm"
           Input="@OnSearch"
           Created="@AddIcon"
           Placeholder="Search...">
</SfTextBox>

@code {
    private SfTextBox SearchBox;
    private string SearchTerm = "";

    private async void AddIcon()
    {
        if (SearchBox != null)
        {
            await SearchBox.AddIconAsync("append", "e-icons e-search");
        }
    }

    private void OnSearch(InputEventArgs args)
    {
        SearchTerm = args.Value;
        // Perform search
    }
}
```

---

## Event Execution Order

1. **Created** → Component initializes
2. **Focus** → User focuses input
3. **Input** → User types (fires repeatedly)
4. **ValueChange** → Value committed (on blur or programmatic)
5. **Blur** → User leaves input
6. **Destroyed** → Component removed

---

## Best Practices

1. **Use Input for real-time UI** (search, character count, hints)
2. **Use ValueChange/Blur for data persistence** (save, validation)
3. **Use Created to add icons** or initialize behavior
4. **Debounce expensive operations** in Input event
5. **Keep event handlers lightweight** for performance

---

## Next Steps

- ✅ [Data Binding & Validation](../data-binding-validation.md) - Event-driven validation
- ✅ [Advanced Features](../advanced-features.md) - Custom behaviors with events
- ✅ [Accessibility & Troubleshooting](../accessibility-troubleshooting.md) - Performance optimization
