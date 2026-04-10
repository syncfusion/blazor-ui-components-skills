# Button Component - Advanced Features

## Table of Contents
- [Accessibility Features](#accessibility-features)
- [Disabled State Handling](#disabled-state-handling)
- [Toggle Button Functionality](#toggle-button-functionality)
- [Server vs Web App Differences](#server-vs-web-app-differences)
- [Performance Optimization](#performance-optimization)
- [Best Practices](#best-practices)
- [Testing Strategies](#testing-strategies)
- [Security Considerations](#security-considerations)

---

## Accessibility Features

### WCAG Compliance

The Syncfusion Blazor Button component is designed to be WCAG 2.1 Level AA compliant.

### Keyboard Navigation

Buttons support standard keyboard interactions:

| Key | Action |
|-----|--------|
| **Enter** | Activates the button |
| **Space** | Activates the button |
| **Tab** | Moves focus to next focusable element |
| **Shift+Tab** | Moves focus to previous focusable element |

```razor
<SfButton OnClick="HandleClick" CssClass="e-primary">
    Keyboard Accessible Button
</SfButton>

@code {
    private void HandleClick()
    {
        Console.WriteLine("Button activated via mouse or keyboard");
    }
}
```

### ARIA Attributes

#### aria-label

Provide accessible names for icon-only buttons:

```razor
<SfButton HtmlAttributes="@ariaLabel" 
          IconCss="e-icons e-close-icon" 
          CssClass="e-round">
</SfButton>

@code {
    private Dictionary<string, object> ariaLabel = new()
    {
        { "aria-label", "Close dialog" }
    };
}

<style>
    .e-close-icon::before { content: '\e7fc'; }
</style>
```

#### aria-pressed

For toggle buttons, indicate pressed state:

```razor
<SfButton HtmlAttributes="@toggleAttributes"
          IsToggle="true"
          @onclick="ToggleState"
          CssClass="@(isPressed ? "e-active" : "")">
    @(isPressed ? "Pressed" : "Not Pressed")
</SfButton>

@code {
    private bool isPressed = false;

    private Dictionary<string, object> toggleAttributes => new()
    {
        { "aria-pressed", isPressed.ToString().ToLower() }
    };

    private void ToggleState()
    {
        isPressed = !isPressed;
    }
}
```

#### aria-disabled

For disabled buttons:

```razor
<SfButton Disabled="@isDisabled" 
          HtmlAttributes="@disabledAttributes">
    Disabled Button
</SfButton>

@code {
    private bool isDisabled = true;

    private Dictionary<string, object> disabledAttributes => new()
    {
        { "aria-disabled", "true" }
    };
}
```

#### aria-describedby

Provide additional context:

```razor
<SfButton HtmlAttributes="@describedBy" CssClass="e-danger">
    Delete Account
</SfButton>
<div id="delete-description" style="display: none;">
    This action cannot be undone. All your data will be permanently deleted.
</div>

@code {
    private Dictionary<string, object> describedBy = new()
    {
        { "aria-describedby", "delete-description" }
    };
}
```

### Screen Reader Support

#### Meaningful Button Text

```razor
<!-- Good: Descriptive -->
<SfButton>Submit Application</SfButton>

<!-- Bad: Generic -->
<SfButton>Click Here</SfButton>
```

#### Context for Icon Buttons

```razor
<SfButton IconCss="e-icons e-save-icon" 
          HtmlAttributes="@saveLabel">
    <span class="visually-hidden">Save</span>
</SfButton>

@code {
    private Dictionary<string, object> saveLabel = new()
    {
        { "aria-label", "Save document" }
    };
}

<style>
    .visually-hidden {
        position: absolute;
        width: 1px;
        height: 1px;
        margin: -1px;
        padding: 0;
        overflow: hidden;
        clip: rect(0, 0, 0, 0);
        border: 0;
    }

    .e-save-icon::before { content: '\e74e'; }
</style>
```

### Color Contrast

Ensure WCAG AA compliance (4.5:1 for normal text, 3:1 for large text):

```razor
<SfButton CssClass="high-contrast e-primary">
    High Contrast Button
</SfButton>

<style>
    .high-contrast.e-btn {
        /* Ensure sufficient contrast */
        background-color: #0066cc;  /* Dark enough for white text */
        color: #ffffff;
    }

    .high-contrast.e-btn:focus {
        outline: 3px solid #ffbf47;  /* Visible focus indicator */
        outline-offset: 2px;
    }
</style>
```

### Focus Indicators

#### Visible Focus State

```razor
<SfButton CssClass="custom-focus e-primary">
    Focus Indicator
</SfButton>

<style>
    .custom-focus.e-btn:focus {
        outline: 3px solid #4A90E2;
        outline-offset: 3px;
        box-shadow: 0 0 0 4px rgba(74, 144, 226, 0.2);
    }

    .custom-focus.e-btn:focus:not(:focus-visible) {
        outline: none;
        box-shadow: none;
    }
</style>
```

#### Skip Link Styling

```razor
<SfButton CssClass="skip-link" @onclick="SkipToContent">
    Skip to main content
</SfButton>

@code {
    private void SkipToContent()
    {
        // Navigate to main content
    }
}

<style>
    .skip-link.e-btn {
        position: absolute;
        top: -40px;
        left: 0;
        z-index: 100;
    }

    .skip-link.e-btn:focus {
        top: 0;
    }
</style>
```

### Role and Live Regions

For dynamic content updates:

```razor
<SfButton OnClick="LoadContent">Load Content</SfButton>

<div role="status" aria-live="polite" aria-atomic="true">
    @if (isLoading)
    {
        <span>Loading content...</span>
    }
    else if (content != null)
    {
        <span>Content loaded successfully</span>
    }
</div>

@code {
    private bool isLoading = false;
    private string content = null;

    private async Task LoadContent()
    {
        isLoading = true;
        await Task.Delay(2000);
        content = "New content";
        isLoading = false;
    }
}
```

---

## Disabled State Handling

### Basic Disabled State

```razor
<SfButton Disabled="@isDisabled" CssClass="e-primary">
    @(isDisabled ? "Disabled" : "Enabled")
</SfButton>

<SfButton OnClick="ToggleDisabled">
    Toggle State
</SfButton>

@code {
    private bool isDisabled = true;

    private void ToggleDisabled()
    {
        isDisabled = !isDisabled;
    }
}
```

### Conditional Disabling

```razor
<EditForm Model="@formModel" OnValidSubmit="HandleSubmit">
    <DataAnnotationsValidator />
    
    <InputText @bind-Value="formModel.Email" />
    
    <SfButton Disabled="@(!context.Validate())" 
              CssClass="e-primary">
        Submit
    </SfButton>
</EditForm>

@code {
    private FormModel formModel = new();

    private void HandleSubmit()
    {
        // Process form
    }

    public class FormModel
    {
        [Required, EmailAddress]
        public string Email { get; set; }
    }
}
```

### Async Operation Disabling

```razor
<SfButton Disabled="@isProcessing" 
          OnClick="ProcessData"
          CssClass="e-primary">
    @(isProcessing ? "Processing..." : "Process Data")
</SfButton>

@code {
    private bool isProcessing = false;

    private async Task ProcessData()
    {
        isProcessing = true;
        try
        {
            await Task.Delay(3000); // Simulate async operation
            // Process data
        }
        finally
        {
            isProcessing = false;
        }
    }
}
```

### Custom Disabled Styling

```razor
<SfButton Disabled="@isDisabled" CssClass="custom-disabled e-primary">
    Disabled Button
</SfButton>

<style>
    .custom-disabled.e-btn:disabled {
        opacity: 0.5;
        cursor: not-allowed;
        background-color: #cccccc;
        color: #666666;
    }
</style>

@code {
    private bool isDisabled = true;
}
```

---

## Toggle Button Functionality

### Basic Toggle Button

```razor
<SfButton IsToggle="true" 
          @onclick="OnToggle"
          CssClass="@(isToggled ? "e-active" : "")">
    @(isToggled ? "ON" : "OFF")
</SfButton>

@code {
    private bool isToggled = false;

    private void OnToggle()
    {
        isToggled = !isToggled;
    }
}
```

### Toggle with Icon Change

```razor
<SfButton IsToggle="true"
          IconCss="@iconCss"
          Content="@content"
          @onclick="TogglePlayPause"
          @ref="toggleBtn"
          CssClass="e-primary">
</SfButton>

@code {
    private SfButton toggleBtn;
    private string iconCss = "e-icons e-play";
    private string content = "Play";

    private void TogglePlayPause()
    {
        if (content == "Play")
        {
            content = "Pause";
            iconCss = "e-icons e-pause";
        }
        else
        {
            content = "Play";
            iconCss = "e-icons e-play";
        }
    }
}

<style>
    .e-play::before { content: '\e324'; }
    .e-pause::before { content: '\e326'; }
</style>
```

### Toggle Group

```razor
<div class="toggle-group">
    @foreach (var option in options)
    {
        <SfButton IsToggle="true"
                  @onclick="() => SelectOption(option)"
                  CssClass="@(selectedOption == option ? "e-active" : "")">
            @option
        </SfButton>
    }
</div>

@code {
    private string[] options = { "Option 1", "Option 2", "Option 3" };
    private string selectedOption = "Option 1";

    private void SelectOption(string option)
    {
        selectedOption = option;
    }
}

<style>
    .toggle-group {
        display: flex;
        gap: 5px;
    }
</style>
```

### State Persistence

```razor
@inject ProtectedLocalStorage LocalStorage

<SfButton IsToggle="true"
          @onclick="ToggleWithPersistence"
          CssClass="@(isToggled ? "e-active" : "")">
    @(isToggled ? "Enabled" : "Disabled")
</SfButton>

@code {
    private bool isToggled = false;

    protected override async Task OnInitializedAsync()
    {
        var result = await LocalStorage.GetAsync<bool>("toggleState");
        isToggled = result.Success ? result.Value : false;
    }

    private async Task ToggleWithPersistence()
    {
        isToggled = !isToggled;
        await LocalStorage.SetAsync("toggleState", isToggled);
    }
}
```

---

## Server vs Web App Differences

### WebAssembly Specific

```razor
@* WebAssembly App *@
<SfButton OnClick="ClientSideClick">
    Client-Side Click
</SfButton>

@code {
    private void ClientSideClick()
    {
        // Executes entirely on client
        Console.WriteLine("Client-side execution");
    }
}
```

### Server-Side Rendering

```razor
@* Blazor Server *@
<SfButton OnClick="ServerSideClick">
    Server-Side Click
</SfButton>

@code {
    private async Task ServerSideClick()
    {
        // Executes on server, UI updates via SignalR
        await Task.Delay(100);
        Console.WriteLine("Server-side execution");
    }
}
```

### Prerendering Considerations

```razor
@inject NavigationManager Navigation

<SfButton OnClick="HandleClick" Disabled="@isPrerendering">
    @buttonText
</SfButton>

@code {
    private bool isPrerendering = true;
    private string buttonText = "Loading...";

    protected override void OnAfterRender(bool firstRender)
    {
        if (firstRender)
        {
            isPrerendering = false;
            buttonText = "Click Me";
            StateHasChanged();
        }
    }

    private void HandleClick()
    {
        // Handle click
    }
}
```

---

## Performance Optimization

### Debouncing Clicks

```razor
<SfButton OnClick="DebouncedClick">
    Debounced Button
</SfButton>

@code {
    private System.Threading.Timer debounceTimer;
    private const int DebounceDelay = 300;

    private void DebouncedClick()
    {
        debounceTimer?.Dispose();
        debounceTimer = new System.Threading.Timer(_ =>
        {
            InvokeAsync(() =>
            {
                // Execute action
                Console.WriteLine("Debounced action executed");
                StateHasChanged();
            });
        }, null, DebounceDelay, Timeout.Infinite);
    }

    public void Dispose()
    {
        debounceTimer?.Dispose();
    }
}
```

### Throttling Clicks

```razor
<SfButton OnClick="ThrottledClick">
    Throttled Button
</SfButton>

@code {
    private DateTime lastClickTime = DateTime.MinValue;
    private const int ThrottleMs = 1000;

    private void ThrottledClick()
    {
        var now = DateTime.Now;
        if ((now - lastClickTime).TotalMilliseconds >= ThrottleMs)
        {
            lastClickTime = now;
            // Execute action
            Console.WriteLine("Throttled action executed");
        }
    }
}
```

### Prevent Double-Click

```razor
<SfButton OnClick="PreventDoubleClick" 
          Disabled="@isProcessing">
    @(isProcessing ? "Processing..." : "Submit")
</SfButton>

@code {
    private bool isProcessing = false;

    private async Task PreventDoubleClick()
    {
        if (isProcessing) return;

        isProcessing = true;
        try
        {
            await ProcessAsync();
        }
        finally
        {
            isProcessing = false;
        }
    }

    private async Task ProcessAsync()
    {
        await Task.Delay(2000);
    }
}
```

### Virtualization for Many Buttons

```razor
@using Microsoft.AspNetCore.Components.Web.Virtualization

<Virtualize Items="@buttons" Context="button">
    <SfButton OnClick="() => HandleClick(button.Id)">
        @button.Text
    </SfButton>
</Virtualize>

@code {
    private List<ButtonModel> buttons = Enumerable.Range(1, 1000)
        .Select(i => new ButtonModel { Id = i, Text = $"Button {i}" })
        .ToList();

    private void HandleClick(int id)
    {
        Console.WriteLine($"Button {id} clicked");
    }

    public class ButtonModel
    {
        public int Id { get; set; }
        public string Text { get; set; }
    }
}
```

---

## Best Practices

### 1. Use Semantic Button Types

```razor
<!-- Good: Semantic meaning -->
<SfButton CssClass="e-primary">Primary Action</SfButton>
<SfButton CssClass="e-danger">Delete</SfButton>

<!-- Avoid: Generic styling -->
<SfButton CssClass="blue-button">Action</SfButton>
```

### 2. Provide Clear Labels

```razor
<!-- Good: Clear purpose -->
<SfButton>Save Changes</SfButton>
<SfButton>Download Report</SfButton>

<!-- Avoid: Vague labels -->
<SfButton>OK</SfButton>
<SfButton>Click Here</SfButton>
```

### 3. Handle Loading States

```razor
<SfButton OnClick="LoadData" 
          Disabled="@isLoading"
          IconCss="@(isLoading ? "e-icons e-spinner" : "")">
    @(isLoading ? "Loading..." : "Load Data")
</SfButton>

@code {
    private bool isLoading = false;

    private async Task LoadData()
    {
        isLoading = true;
        try
        {
            await FetchDataAsync();
        }
        finally
        {
            isLoading = false;
        }
    }

    private async Task FetchDataAsync()
    {
        await Task.Delay(2000);
    }
}
```

### 4. Confirm Destructive Actions

```razor
<SfButton OnClick="ConfirmDelete" CssClass="e-danger">
    Delete Account
</SfButton>

@code {
    [Inject] private IDialogService DialogService { get; set; }

    private async Task ConfirmDelete()
    {
        var confirmed = await DialogService.ConfirmAsync(
            "Are you sure you want to delete your account?",
            "Confirm Deletion"
        );

        if (confirmed)
        {
            await DeleteAccount();
        }
    }

    private async Task DeleteAccount()
    {
        // Delete logic
    }
}
```

### 5. Group Related Actions

```razor
<div class="button-group">
    <SfButton CssClass="e-primary">Save</SfButton>
    <SfButton>Cancel</SfButton>
</div>

<style>
    .button-group {
        display: flex;
        gap: 10px;
        justify-content: flex-end;
    }
</style>
```

---

## Testing Strategies

### Unit Testing

```csharp
[Fact]
public void Button_Click_InvokesCallback()
{
    // Arrange
    var clicked = false;
    var component = RenderComponent<SfButton>(parameters => parameters
        .Add(p => p.OnClick, () => clicked = true)
    );

    // Act
    component.Find("button").Click();

    // Assert
    Assert.True(clicked);
}
```

### Integration Testing

```csharp
[Fact]
public async Task Button_DisabledDuringAsyncOperation()
{
    // Arrange
    var component = RenderComponent<MyComponent>();
    var button = component.Find("button");

    // Act
    button.Click();
    
    // Assert - Button should be disabled
    Assert.True(button.HasAttribute("disabled"));
    
    // Wait for operation
    await Task.Delay(100);
    
    // Assert - Button should be enabled again
    Assert.False(button.HasAttribute("disabled"));
}
```

### Accessibility Testing

```csharp
[Fact]
public void Button_HasProperAriaLabel()
{
    // Arrange & Act
    var component = RenderComponent<IconButton>();
    var button = component.Find("button");

    // Assert
    Assert.Equal("Close dialog", button.GetAttribute("aria-label"));
}
```

---

## Security Considerations

### Prevent CSRF in Forms

```razor
<EditForm Model="@model" OnValidSubmit="HandleSubmit">
    <AntiforgeryToken />
    
    <SfButton HtmlAttributes="@submitAttrs" CssClass="e-primary">
        Submit
    </SfButton>
</EditForm>

@code {
    private Model model = new();
    private Dictionary<string, object> submitAttrs = new() { { "type", "submit" } };

    private async Task HandleSubmit()
    {
        // Form submission with CSRF token
    }
}
```

### Validate User Permissions

```razor
@if (hasPermission)
{
    <SfButton OnClick="DeleteItem" CssClass="e-danger">
        Delete
    </SfButton>
}

@code {
    [CascadingParameter] private User CurrentUser { get; set; }
    private bool hasPermission => CurrentUser?.HasRole("Admin") ?? false;

    private async Task DeleteItem()
    {
        // Verify permission on server-side as well
    }
}
```

### Rate Limiting

```razor
<SfButton OnClick="RateLimitedAction" Disabled="@isRateLimited">
    Submit
</SfButton>

@code {
    private bool isRateLimited = false;
    private int clickCount = 0;
    private DateTime windowStart = DateTime.Now;
    private const int MaxClicks = 5;
    private const int WindowSeconds = 60;

    private async Task RateLimitedAction()
    {
        if ((DateTime.Now - windowStart).TotalSeconds > WindowSeconds)
        {
            clickCount = 0;
            windowStart = DateTime.Now;
        }

        clickCount++;

        if (clickCount > MaxClicks)
        {
            isRateLimited = true;
            return;
        }

        // Perform action
    }
}
```

---

## Common Advanced Scenarios

### Button with Confirmation Dialog

```razor
@inject IJSRuntime JSRuntime

<SfButton OnClick="DeleteWithConfirmation" CssClass="e-danger">
    Delete
</SfButton>

@code {
    private async Task DeleteWithConfirmation()
    {
        bool confirmed = await JSRuntime.InvokeAsync<bool>(
            "confirm", 
            "Are you sure you want to delete this item?"
        );

        if (confirmed)
        {
            await DeleteItem();
        }
    }

    private async Task DeleteItem()
    {
        // Delete logic
    }
}
```

### Button with Progress Tracking

```razor
<SfButton OnClick="ProcessWithProgress" Disabled="@isProcessing">
    Process (@progress%)
</SfButton>

@code {
    private bool isProcessing = false;
    private int progress = 0;

    private async Task ProcessWithProgress()
    {
        isProcessing = true;
        progress = 0;

        for (int i = 0; i <= 100; i += 10)
        {
            progress = i;
            StateHasChanged();
            await Task.Delay(200);
        }

        isProcessing = false;
    }
}
```

---

## Next Steps

- Review [button-features.md](button-features.md) for basic functionality
- Explore [button-styling.md](button-styling.md) for customization options
