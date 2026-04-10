# ToggleSwitch Component - Advanced Features

## Accessibility

### ARIA Support

ToggleSwitch automatically includes:
- `role="switch"`
- `aria-checked` state
- `aria-label` for screen readers

```razor
<SfSwitch @bind-Checked="accessible" CssClass="accessible-switch"></SfSwitch>

<style>
    .accessible-switch:focus-visible {
        outline: 2px solid #0078d4;
        outline-offset: 2px;
        box-shadow: 0 0 0 4px rgba(0, 120, 212, 0.1);
    }
</style>

@code {
    private bool accessible;
}
```

## Keyboard Navigation

- **Space**: Toggle switch
- **Enter**: Toggle switch
- **Tab**: Move focus to/from switch

## State Management

```razor
<SfSwitch @bind-Checked="featureEnabled">
    <SwitchEvents ValueChange="OnToggle"></SwitchEvents>
</SfSwitch>

<p>Feature: @(featureEnabled ? "Enabled" : "Disabled")</p>

<button @onclick="SaveSettings">Save</button>
<button @onclick="LoadSettings">Load</button>

@code {
    private bool featureEnabled = false;
    private bool savedState = false;

    private async Task OnToggle(ChangeEventArgs<bool> args)
    {
        Console.WriteLine($"Toggled to: {args.Checked}");
        
        if (args.Checked)
        {
            await EnableFeatureAsync();
        }
        else
        {
            await DisableFeatureAsync();
        }
    }

    private void SaveSettings()
    {
        savedState = featureEnabled;
        // Save to local storage or database
        Console.WriteLine($"Saved state: {savedState}");
    }

    private void LoadSettings()
    {
        featureEnabled = savedState;
        StateHasChanged();
    }

    private async Task EnableFeatureAsync() => await Task.Delay(100);
    private async Task DisableFeatureAsync() => await Task.Delay(100);
}
```

## Form Integration

```razor
<EditForm Model="@userSettings" OnValidSubmit="SaveUserSettings">
    <DataAnnotationsValidator />
    
    <div class="form-group">
        <label>Email Notifications</label>
        <SfSwitch @bind-Checked="userSettings.EmailNotifications"></SfSwitch>
    </div>
    
    <div class="form-group">
        <label>SMS Alerts</label>
        <SfSwitch @bind-Checked="userSettings.SmsAlerts"></SfSwitch>
    </div>
    
    <div class="form-group">
        <label>Auto Backup</label>
        <SfSwitch @bind-Checked="userSettings.AutoBackup"></SfSwitch>
    </div>
    
    <button type="submit">Save Settings</button>
</EditForm>

@code {
    private UserSettings userSettings = new();

    private async Task SaveUserSettings()
    {
        // Save to database
        await Task.Delay(500);
        Console.WriteLine("Settings saved");
    }

    public class UserSettings
    {
        public bool EmailNotifications { get; set; }
        public bool SmsAlerts { get; set; }
        public bool AutoBackup { get; set; }
    }
}
```

## Event Handling

```razor
<SfSwitch @bind-Checked="toggleValue">
    <SwitchEvents 
        Created="OnCreated"
        ValueChange="OnValueChange">
    </SwitchEvents>
</SfSwitch>

@code {
    private bool toggleValue = false;

    private void OnCreated()
    {
        Console.WriteLine("Switch created");
    }

    private async Task OnValueChange(ChangeEventArgs<bool> args)
    {
        Console.WriteLine($"Value changed to: {args.Checked}");
        
        // Perform async operations
        if (args.Checked)
        {
            await ProcessEnableAsync();
        }
        else
        {
            await ProcessDisableAsync();
        }
    }

    private async Task ProcessEnableAsync()
    {
        // API call or async operation
        await Task.Delay(200);
    }

    private async Task ProcessDisableAsync()
    {
        // API call or async operation
        await Task.Delay(200);
    }
}
```

## Programmatic Control

```razor
<SfSwitch @ref="switchRef" @bind-Checked="switchState"></SfSwitch>

<button @onclick="EnableSwitch">Enable</button>
<button @onclick="DisableSwitch">Disable</button>
<button @onclick="ToggleSwitch">Toggle</button>

@code {
    private SfSwitch switchRef;
    private bool switchState = false;

    private void EnableSwitch()
    {
        switchState = true;
    }

    private void DisableSwitch()
    {
        switchState = false;
    }

    private void ToggleSwitch()
    {
        switchState = !switchState;
    }
}
```

## Conditional Disabled State

```razor
<SfSwitch @bind-Checked="primarySwitch">
    <SwitchEvents ValueChange="OnPrimaryToggle"></SwitchEvents>
</SfSwitch>

<SfSwitch @bind-Checked="dependentSwitch" Disabled="@(!primarySwitch)"></SfSwitch>

<p>Primary: @primarySwitch</p>
<p>Dependent (disabled when primary is off): @dependentSwitch</p>

@code {
    private bool primarySwitch = false;
    private bool dependentSwitch = false;

    private void OnPrimaryToggle(ChangeEventArgs<bool> args)
    {
        if (!args.Checked)
        {
            // Reset dependent switch when primary is disabled
            dependentSwitch = false;
        }
    }
}
```

## Performance Best Practices

1. **Minimize Re-renders**: Use `@bind-Checked` for two-way binding
2. **Avoid Excessive Events**: Handle `ValueChange` efficiently
3. **Debounce Actions**: For expensive operations triggered by toggle
4. **State Persistence**: Save toggle states to local storage for UX

## Testing

```csharp
using Bunit;
using Xunit;
using Syncfusion.Blazor.Buttons;

public class ToggleSwitchTests : TestContext
{
    [Fact]
    public void ToggleSwitch_Changes_State()
    {
        // Arrange
        bool currentValue = false;
        var cut = RenderComponent<SfSwitch>(parameters => parameters
            .Add(p => p.Checked, currentValue)
            .Add(p => p.CheckedChanged, newValue => currentValue = newValue));

        // Act
        var switchElement = cut.Find(".e-switch");
        switchElement.Click();

        // Assert
        Assert.True(currentValue);
    }

    [Fact]
    public void ToggleSwitch_Disabled_Prevents_Change()
    {
        // Arrange
        bool currentValue = false;
        var cut = RenderComponent<SfSwitch>(parameters => parameters
            .Add(p => p.Checked, currentValue)
            .Add(p => p.Disabled, true)
            .Add(p => p.CheckedChanged, newValue => currentValue = newValue));

        // Act
        var switchElement = cut.Find(".e-switch");
        switchElement.Click();

        // Assert
        Assert.False(currentValue); // Should remain false
    }
}
```

## Common Patterns

### Settings Panel with Multiple Toggles

```razor
<div class="settings-panel">
    @foreach (var setting in settings)
    {
        <div class="setting-row">
            <label>@setting.Label</label>
            <SfSwitch @bind-Checked="setting.Enabled">
                <SwitchEvents ValueChange="@(args => OnSettingChange(setting, args))"></SwitchEvents>
            </SfSwitch>
        </div>
    }
</div>

<button @onclick="SaveAllSettings">Save All</button>

@code {
    private List<Setting> settings = new()
    {
        new Setting { Id = 1, Label = "Notifications", Enabled = true },
        new Setting { Id = 2, Label = "Auto-Save", Enabled = false },
        new Setting { Id = 3, Label = "Dark Mode", Enabled = false }
    };

    private void OnSettingChange(Setting setting, ChangeEventArgs<bool> args)
    {
        setting.Enabled = args.Checked;
        Console.WriteLine($"{setting.Label}: {args.Checked}");
    }

    private async Task SaveAllSettings()
    {
        // Save to API or database
        await Task.Delay(500);
        Console.WriteLine("All settings saved");
    }

    public class Setting
    {
        public int Id { get; set; }
        public string Label { get; set; }
        public bool Enabled { get; set; }
    }
}
```

---

See also: [toggle-switch-features.md](toggle-switch-features.md) | [toggle-switch-styling.md](toggle-switch-styling.md)
