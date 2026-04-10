# ToggleSwitch Component - Styling

## Size Variations

### Small

```razor
<SfSwitch CssClass="e-small" @bind-Checked="isChecked"></SfSwitch>

<style>
    .e-small.e-switch-wrapper {
        width: 32px;
        height: 18px;
    }
    
    .e-small .e-switch-inner {
        width: 14px;
        height: 14px;
    }
</style>

@code {
    private bool isChecked = false;
}
```

### Medium (Default)

```razor
<SfSwitch @bind-Checked="isChecked"></SfSwitch>

@code {
    private bool isChecked = false;
}
```

### Large

```razor
<SfSwitch CssClass="e-large" @bind-Checked="isChecked"></SfSwitch>

<style>
    .e-large.e-switch-wrapper {
        width: 60px;
        height: 30px;
    }
    
    .e-large .e-switch-inner {
        width: 26px;
        height: 26px;
    }
</style>

@code {
    private bool isChecked = false;
}
```

## Color Customization

### Custom Colors

```razor
<SfSwitch @bind-Checked="isChecked" CssClass="custom-switch"></SfSwitch>

<style>
    .custom-switch.e-switch-wrapper .e-switch-on {
        background-color: #4caf50;
        border-color: #4caf50;
    }
    
    .custom-switch.e-switch-wrapper .e-switch-off {
        background-color: #f44336;
        border-color: #f44336;
    }
</style>

@code {
    private bool isChecked = false;
}
```

### Brand Colors

```razor
<!-- Primary -->
<SfSwitch @bind-Checked="switch1" CssClass="primary-switch"></SfSwitch>

<!-- Success -->
<SfSwitch @bind-Checked="switch2" CssClass="success-switch"></SfSwitch>

<!-- Warning -->
<SfSwitch @bind-Checked="switch3" CssClass="warning-switch"></SfSwitch>

<style>
    .primary-switch.e-switch-wrapper .e-switch-on {
        background-color: #0078d4;
    }
    
    .success-switch.e-switch-wrapper .e-switch-on {
        background-color: #28a745;
    }
    
    .warning-switch.e-switch-wrapper .e-switch-on {
        background-color: #ffc107;
    }
</style>

@code {
    private bool switch1, switch2, switch3;
}
```

## Label Customization

### With Custom Labels

```razor
<SfSwitch @bind-Checked="isEnabled" 
          OnLabel="✓" 
          OffLabel="✗" 
          CssClass="custom-labels">
</SfSwitch>

<style>
    .custom-labels .e-switch-on,
    .custom-labels .e-switch-off {
        font-size: 14px;
        font-weight: bold;
    }
</style>

@code {
    private bool isEnabled = false;
}
```

**Note**: Material theme doesn't support On/Off label text.

## RTL Support

```razor
<SfSwitch @bind-Checked="isChecked" EnableRtl="true" OnLabel="تشغيل" OffLabel="إيقاف"></SfSwitch>

@code {
    private bool isChecked = false;
}
```

## Disabled Appearance

```razor
<SfSwitch Disabled="true" Checked="false" CssClass="disabled-switch"></SfSwitch>
<SfSwitch Disabled="true" Checked="true" CssClass="disabled-switch"></SfSwitch>

<style>
    .disabled-switch.e-switch-wrapper {
        opacity: 0.5;
        cursor: not-allowed;
    }
</style>
```

## Custom Styles

### iOS Style

```razor
<SfSwitch @bind-Checked="isChecked" CssClass="ios-switch"></SfSwitch>

<style>
    .ios-switch.e-switch-wrapper {
        width: 51px;
        height: 31px;
        border-radius: 31px;
    }
    
    .ios-switch .e-switch-inner {
        width: 27px;
        height: 27px;
        border-radius: 50%;
        box-shadow: 0 3px 8px rgba(0, 0, 0, 0.15);
    }
    
    .ios-switch.e-switch-wrapper .e-switch-on {
        background-color: #34c759;
    }
</style>

@code {
    private bool isChecked = false;
}
```

### Material Style

```razor
<SfSwitch @bind-Checked="isChecked" CssClass="material-switch"></SfSwitch>

<style>
    .material-switch.e-switch-wrapper .e-switch-on {
        background-color: #6200ea;
    }
    
    .material-switch .e-switch-inner {
        box-shadow: 0 2px 4px rgba(0, 0, 0, 0.3);
    }
</style>

@code {
    private bool isChecked = false;
}
```

---

See also: [toggle-switch-features.md](toggle-switch-features.md) | [toggle-switch-advanced-features.md](toggle-switch-advanced-features.md)
