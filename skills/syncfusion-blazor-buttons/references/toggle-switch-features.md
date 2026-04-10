# ToggleSwitch Component - Features

## Installation

```bash
dotnet add package Syncfusion.Blazor.Buttons
```

## API Reference

### SfSwitch<TChecked> Properties

**Note:** SfSwitch is a generic component. Use `SfSwitch<bool>` or simply `SfSwitch` for boolean values.

**State Properties:**
- `Checked` (TChecked) - Checked state (typically bool)
- `CheckedChanged` (EventCallback<TChecked>) - Two-way binding event
- `CheckedExpression` (Expression<Func<TChecked>>) - Expression for validation
- `Value` (string) - Value attribute for form submission
- `Name` (string) - Name attribute for form submission

**Label Properties:**
- `OnLabel` (string) - Label text for ON state
- `OffLabel` (string) - Label text for OFF state

**Appearance:**
- `CssClass` (string) - Custom CSS classes
- `Disabled` (bool) - Disabled state
- `EnableRtl` (bool) - Right-to-left support
- `EnablePersistence` (bool) - Persist state across sessions
- `HtmlAttributes` (Dictionary<string, object>) - Custom HTML attributes

**Content:**
- `ChildContent` (RenderFragment) - Child content

**Events:**
- `ValueChange` (EventCallback<ChangeEventArgs<TChecked>>) - Value change event
- `Created` (EventCallback<Object>) - Component created event
- `IsOnChangeEvent` (bool) - Trigger event on change vs input

**ChangeEventArgs<TChecked> Properties:**
- `Checked` (TChecked) - New checked state
- `Event` (object) - Native event object
- `IsInteracted` (bool) - True if changed by user interaction

## Basic ToggleSwitch

```razor
<SfSwitch @bind-Checked="isChecked"></SfSwitch>

<p>Status: @(isChecked ? "ON" : "OFF")</p>

@code {
    private bool isChecked = false;
}
```

## With Labels

```razor
<SfSwitch @bind-Checked="isEnabled" OnLabel="ON" OffLabel="OFF"></SfSwitch>

@code {
    private bool isEnabled = false;
}
```

## Change Event

```razor
<SfSwitch @bind-Checked="notificationsEnabled" ValueChange="OnToggleChange"></SfSwitch>

<p>Notifications: @(notificationsEnabled ? "Enabled" : "Disabled")</p>

@code {
    private bool notificationsEnabled = false;

    private void OnToggleChange(ChangeEventArgs<bool> args)
    {
        Console.WriteLine($"Switch toggled: {args.Checked}");
        
        if (args.Checked)
        {
            EnableNotifications();
        }
        else
        {
            DisableNotifications();
        }
    }

    private void EnableNotifications() => Console.WriteLine("Notifications enabled");
    private void DisableNotifications() => Console.WriteLine("Notifications disabled");
}
```

## Checked State

```razor
<!-- Initially Checked -->
<SfSwitch @bind-Checked="@checked" OnLabel="ON" OffLabel="OFF"></SfSwitch>

@code {
    private bool checked = true;
}
```

## Disabled State

```razor
<SfSwitch Disabled="true" Checked="false"></SfSwitch>
<SfSwitch Disabled="true" Checked="true"></SfSwitch>
```

## Name Attribute

```razor
<EditForm Model="@model">
    <SfSwitch @bind-Checked="model.IsActive" Name="isActive"></SfSwitch>
    <button type="submit">Submit</button>
</EditForm>

@code {
    private FormModel model = new();

    public class FormModel
    {
        public bool IsActive { get; set; }
    }
}
```

## Two-Way Binding

```razor
<SfSwitch @bind-Checked="darkMode"></SfSwitch>

<p>Dark Mode: @(darkMode ? "Enabled" : "Disabled")</p>

<button @onclick="ToggleDarkMode">Toggle Programmatically</button>

@code {
    private bool darkMode = false;

    private void ToggleDarkMode()
    {
        darkMode = !darkMode;
    }
}
```

## Value Binding

```razor
<SfSwitch Value="@switchValue" ValueChange="OnValueChange"></SfSwitch>

@code {
    private bool switchValue = false;

    private void OnValueChange(bool newValue)
    {
        switchValue = newValue;
        StateHasChanged();
    }
}
```

## Common Scenarios

### Settings Toggle

```razor
<div class="settings-panel">
    <div class="setting-item">
        <label>Enable Notifications</label>
        <SfSwitch @bind-Checked="settings.Notifications"></SfSwitch>
    </div>
    
    <div class="setting-item">
        <label>Auto-Save</label>
        <SfSwitch @bind-Checked="settings.AutoSave"></SfSwitch>
    </div>
    
    <div class="setting-item">
        <label>Dark Mode</label>
        <SfSwitch @bind-Checked="settings.DarkMode"></SfSwitch>
    </div>
</div>

@code {
    private Settings settings = new();

    public class Settings
    {
        public bool Notifications { get; set; }
        public bool AutoSave { get; set; }
        public bool DarkMode { get; set; }
    }
}
```

### Feature Toggle

```razor
<SfSwitch @bind-Checked="featureEnabled" ValueChange="OnFeatureToggle"></SfSwitch>

<span>@featureLabel</span>

@code {
    private bool featureEnabled = false;
    private string featureLabel = "Feature Disabled";

    private async Task OnFeatureToggle(ChangeEventArgs<bool> args)
    {
        if (args.Checked)
        {
            await EnableFeatureAsync();
            featureLabel = "Feature Enabled";
        }
        else
        {
            await DisableFeatureAsync();
            featureLabel = "Feature Disabled";
        }
    }

    private async Task EnableFeatureAsync()
    {
        // API call to enable feature
        await Task.Delay(500);
    }

    private async Task DisableFeatureAsync()
    {
        // API call to disable feature
        await Task.Delay(500);
    }
}
```

---

See also: [toggle-switch-styling.md](toggle-switch-styling.md) | [toggle-switch-advanced-features.md](toggle-switch-advanced-features.md)
