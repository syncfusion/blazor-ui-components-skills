# Accessibility and Localization

## Table of Contents
- [Tab Key Navigation](#tab-key-navigation)
- [Keyboard Navigation](#keyboard-navigation)
- [Screen Reader Support](#screen-reader-support)
- [ARIA Attributes](#aria-attributes)
- [Focus Management](#focus-management)
- [Localization Support](#localization-support)
- [Right-to-Left (RTL) Support](#right-to-left-rtl-support)
- [Color Contrast](#color-contrast)

## Tab Key Navigation

The `TabIndex` property enables Tab key navigation for tab items. By default, users navigate using arrow keys.

### Implementation

```razor
@using Syncfusion.Blazor.Navigations

<!-- Enable Tab key navigation -->
<SfTab>
    <TabItems>
        <!-- Setting TabIndex=0 makes item keyboard accessible -->
        <TabItem TabIndex="0" Content="First tab accessible via Tab key">
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem TabIndex="0" Content="Second tab accessible via Tab key">
            <ChildContent>
                <TabHeader Text="Tab 2"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem TabIndex="0" Content="Third tab accessible via Tab key">
            <ChildContent>
                <TabHeader Text="Tab 3"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>
```

### TabIndex Values

| Value | Behavior |
|-------|----------|
| **0** | Tab order follows DOM order |
| **Positive (1+)** | Custom tab order (lower values first) |
| **Negative (-1)** | Element is removed from tab order |

### Custom Tab Order

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabItems>
        <!-- Navigate in custom order: 2nd, 1st, 3rd -->
        <TabItem TabIndex="2" Content="Navigated second">
            <ChildContent>
                <TabHeader Text="Navigated 2nd"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem TabIndex="1" Content="Navigated first">
            <ChildContent>
                <TabHeader Text="Navigated 1st"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem TabIndex="3" Content="Navigated third">
            <ChildContent>
                <TabHeader Text="Navigated 3rd"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>
```

## Keyboard Navigation

Syncfusion Tabs supports standard keyboard navigation patterns:

### Standard Keyboard Shortcuts

| Key | Action |
|-----|--------|
| **Tab** | Focus next focusable element (if TabIndex enabled) |
| **Shift + Tab** | Focus previous focusable element |
| **Right Arrow** | Select next tab (when focused on tab header) |
| **Left Arrow** | Select previous tab (when focused on tab header) |
| **Home** | Select first tab (when focused on tab header) |
| **End** | Select last tab (when focused on tab header) |
| **Enter/Space** | Activate focused tab |

### Implementation

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabItems>
        <TabItem TabIndex="0" Content="Use arrow keys to navigate between tabs">
            <ChildContent>
                <TabHeader Text="Navigation Demo"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem TabIndex="0" Content="Press Home key to go to first tab">
            <ChildContent>
                <TabHeader Text="Try Keyboard"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem TabIndex="0" Content="Press End key to go to last tab">
            <ChildContent>
                <TabHeader Text="End Key Demo"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>
```

## Screen Reader Support

Syncfusion Tabs includes built-in screen reader support with appropriate ARIA labels and roles.

### Default Screen Reader Announcements

- Tab component announces as "tablist"
- Each tab header announces role and selected state
- Selected content announces as "tabpanel"
- Status updates announce when tab changes

### Testing with Screen Readers

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabItems>
        <!-- Screen readers announce: "Tab 1, tab, selected" -->
        <TabItem TabIndex="0" Content="Content for first tab">
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
        </TabItem>
        <!-- Screen readers announce: "Tab 2, tab" -->
        <TabItem TabIndex="0" Content="Content for second tab">
            <ChildContent>
                <TabHeader Text="Tab 2"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>
```

### Enable in Testing

1. **NVDA (Windows)**: Download from https://www.nvaccess.org/
2. **JAWS (Windows)**: Commercial screen reader
3. **VoiceOver (macOS/iOS)**: Built-in accessibility
4. **TalkBack (Android)**: Built-in accessibility

## ARIA Attributes

Syncfusion Tabs automatically applies ARIA attributes. Understanding them helps with custom implementations:

### Key ARIA Roles

```html
<!-- Tab list container -->
<div role="tablist">
    <!-- Tab header -->
    <button role="tab" aria-selected="true" aria-controls="tabpanel1">Tab 1</button>
    <button role="tab" aria-selected="false" aria-controls="tabpanel2">Tab 2</button>
</div>

<!-- Tab content -->
<div role="tabpanel" id="tabpanel1" aria-labelledby="tab1-button">Content 1</div>
<div role="tabpanel" id="tabpanel2" aria-labelledby="tab2-button">Content 2</div>
```

### ARIA Attributes Explained

| Attribute | Purpose |
|-----------|---------|
| `role="tablist"` | Container for all tabs |
| `role="tab"` | Individual tab button |
| `role="tabpanel"` | Content area for a tab |
| `aria-selected="true/false"` | Indicates selected state |
| `aria-controls="id"` | Links tab to its content |
| `aria-labelledby="id"` | Links content to its tab |

### Custom ARIA Implementation

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabItems>
        <TabItem TabIndex="0" Content="Accessible content">
            <ChildContent>
                <!-- ARIA attributes are automatically managed -->
                <TabHeader Text="Accessible Tab"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

<!-- Syncfusion automatically adds:
    - role="tablist" to SfTab
    - role="tab" to each TabHeader
    - role="tabpanel" to each content
    - aria-selected on active tab
    - aria-controls linking tab to panel
-->
```

## Focus Management

Proper focus management ensures keyboard navigation is clear and accessible.

### Focus Visible State

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabItems>
        <TabItem TabIndex="0" Content="Press Tab to see focus indicator">
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem TabIndex="0" Content="Focus indicator shows which tab is focused">
            <ChildContent>
                <TabHeader Text="Tab 2"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

<style>
    /* Ensure focus is visible -->
    .e-tab .e-tab-header-item:focus-visible {
        outline: 2px solid #000;
        outline-offset: 2px;
    }
</style>
```

### Focus Trap (Modal Tabs)

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<!-- Trap focus within tab component for modal dialog -->
<div @ref="TabContainer" @onkeydown="OnKeyDown">
    <SfTab @ref="TabControl">
        <TabItems>
            <TabItem TabIndex="0" Content="Content 1">
                <ChildContent>
                    <TabHeader Text="Tab 1"></TabHeader>
                </ChildContent>
            </TabItem>
            <TabItem TabIndex="0" Content="Content 2">
                <ChildContent>
                    <TabHeader Text="Tab 2"></TabHeader>
                </ChildContent>
            </TabItem>
        </TabItems>
    </SfTab>
    
    <SfButton @ref="CloseButton" @onclick="Close">Close</SfButton>
</div>

@code {
    private ElementReference TabContainer;
    private SfTab TabControl;
    private SfButton CloseButton;
    
    private void OnKeyDown(KeyboardEventArgs args)
    {
        // Implement focus trapping if needed
        if (args.Key == "Escape")
        {
            Close();
        }
    }
    
    private void Close()
    {
        // Close modal
    }
}
```

## Localization Support

Syncfusion Tabs supports localization through culture-specific resources.

### Available Localization Strings

Common localization keys:
- `TabHeaderText`: Tab header text
- `AriaLabel`: ARIA labels for accessibility

### Implementation

```razor
@using Syncfusion.Blazor.Navigations
@using System.Globalization

<!-- Blazor automatically uses current culture for localization -->
<SfTab>
    <TabItems>
        <TabItem Content="Content in current culture">
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    protected override void OnInitialized()
    {
        // Set culture for localization
        CultureInfo.CurrentCulture = new CultureInfo("es-ES"); // Spanish
        // or
        CultureInfo.CurrentCulture = new CultureInfo("fr-FR"); // French
    }
}
```

### Custom Localization

```razor
@using Syncfusion.Blazor.Navigations
@using System.Collections.Generic
@using System.Globalization

<SfTab>
    <TabItems>
        <TabItem Content="Localized content">
            <ChildContent>
                <TabHeader Text="@GetLocalizedText("tab1")"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="More localized content">
            <ChildContent>
                <TabHeader Text="@GetLocalizedText("tab2")"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private Dictionary<string, Dictionary<string, string>> LocalizedStrings = new()
    {
        { "en-US", new Dictionary<string, string>
        {
            { "tab1", "Home" },
            { "tab2", "Profile" }
        }},
        { "es-ES", new Dictionary<string, string>
        {
            { "tab1", "Inicio" },
            { "tab2", "Perfil" }
        }},
        { "fr-FR", new Dictionary<string, string>
        {
            { "tab1", "Accueil" },
            { "tab2", "Profil" }
        }}
    };
    
    private string GetLocalizedText(string key)
    {
        string culture = CultureInfo.CurrentCulture.Name;
        if (LocalizedStrings.ContainsKey(culture) && 
            LocalizedStrings[culture].ContainsKey(key))
        {
            return LocalizedStrings[culture][key];
        }
        
        // Fallback to English
        return LocalizedStrings["en-US"].ContainsKey(key) 
            ? LocalizedStrings["en-US"][key] 
            : key;
    }
}
```

## Right-to-Left (RTL) Support

Syncfusion Tabs supports RTL languages through the `EnableRtl` property.

### Enable RTL

```razor
@using Syncfusion.Blazor.Navigations

<!-- Enable RTL for Arabic, Hebrew, etc. -->
<SfTab EnableRtl="true">
    <TabItems>
        <TabItem Content="محتوى التبويب الأول">
            <ChildContent>
                <TabHeader Text="التبويب الأول"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="محتوى التبويب الثاني">
            <ChildContent>
                <TabHeader Text="التبويب الثاني"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>
```

### Dynamic RTL Toggling

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton @onclick="ToggleRtl">Toggle RTL</SfButton>

<SfTab @ref="TabControl" EnableRtl="@IsRtlEnabled">
    <TabItems>
        <TabItem Content="Tab content">
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private SfTab TabControl;
    private bool IsRtlEnabled = false;
    
    private void ToggleRtl()
    {
        IsRtlEnabled = !IsRtlEnabled;
    }
}
```

## Color Contrast

Ensure sufficient color contrast for readability and accessibility compliance.

### WCAG Compliance Levels

| Level | Contrast Ratio | Use Case |
|-------|---|----------|
| **AA (normal text)** | 4.5:1 | Standard web content |
| **AA (large text)** | 3:1 | Text 18pt+ or 14pt+ bold |
| **AAA (normal text)** | 7:1 | High accessibility requirement |
| **AAA (large text)** | 4.5:1 | Text 18pt+ or 14pt+ bold |

### Check Color Contrast

Use tools like:
- WebAIM Contrast Checker: https://webaim.org/resources/contrastchecker/
- Color Contrast Analyzer
- Browser DevTools

### Accessible Styling

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabItems>
        <TabItem Content="Content with good contrast">
            <ChildContent>
                <TabHeader Text="Accessible Tab"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

<style>
    /* High contrast for accessibility (AA compliant) -->
    .e-tab .e-tab-header-item {
        /* Dark text on light background = 4.5:1 ratio */
        color: #1a1a1a;
        background-color: #ffffff;
    }
    
    .e-tab .e-tab-header-item.e-active {
        /* Active state high contrast */
        color: #ffffff;
        background-color: #0052cc; /* Dark blue */
    }
    
    .e-tab .e-content .e-tab-content {
        /* Body text on background */
        color: #333333;
        background-color: #ffffff;
    }
</style>
```

## Accessibility Checklist

Before releasing your implementation:

- [ ] Tab key navigation works (TabIndex configured)
- [ ] Arrow keys navigate between tabs
- [ ] Home/End keys work
- [ ] Focus is clearly visible
- [ ] Screen reader announces tabs correctly
- [ ] Color contrast meets WCAG AA standards
- [ ] No color-only indicators
- [ ] Focus trap implemented if modal
- [ ] Content is semantically correct
- [ ] ARIA labels are descriptive
- [ ] RTL support enabled if needed
- [ ] Tested with screen reader (NVDA, JAWS, VoiceOver)
- [ ] Tested with keyboard only (no mouse)

## Testing Resources

- **WCAG 2.1 Guidelines**: https://www.w3.org/WAI/WCAG21/quickref/
- **WebAIM**: https://webaim.org/
- **Deque axe DevTools**: Browser extension for accessibility testing
- **Lighthouse**: Built into Chrome DevTools
