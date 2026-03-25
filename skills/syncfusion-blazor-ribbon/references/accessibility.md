# Accessibility

The Syncfusion Blazor Ribbon component is built with accessibility in mind, following WCAG 2.1 Level AA standards. All ribbon items support keyboard navigation, ARIA attributes, screen reader compatibility, and focus management.

## WCAG 2.1 Compliance

| Criterion | Level | Compliance | Implementation |
|-----------|-------|-----------|-----------------|
| **1.4.3 Contrast** | AA | ✅ | Color contrast ratios meet standards |
| **2.1.1 Keyboard** | A | ✅ | All functionality available via keyboard |
| **2.4.3 Focus Order** | A | ✅ | Logical focus order through ribbon |
| **2.4.7 Focus Visible** | AA | ✅ | Clear visual focus indicators |
| **3.2.1 On Focus** | A | ✅ | No unexpected context changes on focus |
| **3.2.2 On Input** | A | ✅ | Changes only on user request |
| **4.1.2 Name/Role/Value** | A | ✅ | Proper ARIA attributes for all items |
| **4.1.3 Status Messages** | AAA | ✅ | Live regions for dynamic updates |

## Keyboard Navigation

### Tab Navigation

Navigate between ribbon tabs with keyboard:

```
Tab        → Move to next focusable element
Shift+Tab  → Move to previous focusable element
```

Example with custom Tab handling:

```razor
@using Syncfusion.Blazor.Ribbon

<SfRibbon TabSelecting="@OnTabSelect">
    <RibbonTabs>
        <RibbonTab HeaderText="Home"><!-- Tab 1 --></RibbonTab>
        <RibbonTab HeaderText="Insert"><!-- Tab 2 --></RibbonTab>
        <RibbonTab HeaderText="Design"><!-- Tab 3 --></RibbonTab>
    </RibbonTabs>
</SfRibbon>

@code {
    private void OnTabSelect(TabSelectingEventArgs args)
    {
        Console.WriteLine($"Tab changed: {args.SelectedTab}");
    }
}
```

### Arrow Key Navigation

Within ribbon groups and items:

```
Left Arrow   → Move to previous item in group
Right Arrow  → Move to next item in group
Up Arrow     → Move up in vertical group
Down Arrow   → Move down in vertical group
```

### Enter and Space Keys

Activate ribbon items:

```
Enter → Click button, open dropdown, select item
Space → Click button, toggle checkbox
```

### Keyboard Navigation Example

```razor
<RibbonGroup HeaderText="Clipboard" Orientation="Orientation.Row">
    <RibbonCollections>
        <RibbonCollection>
            <RibbonItems>
                <RibbonItem Type="RibbonItemType.Button" TabIndex="0">
                    <RibbonButtonSettings Content="Cut" IconCss="e-icons e-cut">
                    </RibbonButtonSettings>
                </RibbonItem>
                <RibbonItem Type="RibbonItemType.Button" TabIndex="0">
                    <RibbonButtonSettings Content="Copy" IconCss="e-icons e-copy">
                    </RibbonButtonSettings>
                </RibbonItem>
                <RibbonItem Type="RibbonItemType.Button" TabIndex="0">
                    <RibbonButtonSettings Content="Paste" IconCss="e-icons e-paste">
                    </RibbonButtonSettings>
                </RibbonItem>
            </RibbonItems>
        </RibbonCollection>
    </RibbonCollections>
</RibbonGroup>
```

## ARIA Attributes

### Ribbon Regions

Mark semantic regions with ARIA roles:

```razor
<div role="application" aria-label="Office-style editor with ribbon">
    <SfRibbon role="navigation" aria-label="Ribbon toolbar">
        <!-- Ribbon content -->
    </SfRibbon>
</div>
```

### Tab Labels

Provide accessible labels for tabs:

```razor
<RibbonTab HeaderText="Home" aria-label="Home tab - common formatting options">
    <!-- Tab content -->
</RibbonTab>
```

### Group Labels

Add labels to grouped items:

```razor
<RibbonGroup HeaderText="Formatting" aria-label="Text formatting options">
    <RibbonCollections>
        <!-- Group items -->
    </RibbonCollections>
</RibbonGroup>
```

### Item Labels

Provide descriptive labels for items:

```razor
<RibbonItem Type="RibbonItemType.Button" aria-label="Save document (Ctrl+S)">
    <RibbonButtonSettings Content="Save" IconCss="e-icons e-save">
    </RibbonButtonSettings>
</RibbonItem>
```

## Screen Reader Support

### Button Items

Announce button purpose and state:

```razor
<RibbonItem Type="RibbonItemType.Button">
    <RibbonButtonSettings Content="Bold"
                          IconCss="e-icons e-bold"
                          aria-pressed="false"
                          aria-label="Toggle bold formatting">
    </RibbonButtonSettings>
</RibbonItem>
```

### Checkbox Items

Announce checkbox state:

```razor
<RibbonItem Type="RibbonItemType.CheckBox">
    <RibbonCheckBoxSettings Label="Show Ruler"
                            Checked="true"
                            aria-label="Toggle ruler visibility"
                            aria-checked="true">
    </RibbonCheckBoxSettings>
</RibbonItem>
```

### DropDown Items

Announce available options:

```razor
<RibbonItem Type="RibbonItemType.DropDown">
    <RibbonDropDownSettings Content="Style"
                            Items="@styleOptions"
                            aria-label="Text style options"
                            aria-haspopup="listbox">
    </RibbonDropDownSettings>
</RibbonItem>

@code {
    private List<DropDownMenuItem> styleOptions = new List<DropDownMenuItem>()
    {
        new DropDownMenuItem{ Text = "Normal", Id = "normal" },
        new DropDownMenuItem{ Text = "Heading 1", Id = "h1" },
        new DropDownMenuItem{ Text = "Heading 2", Id = "h2" }
    };
}
```

### ComboBox Items

Announce editable list:

```razor
<RibbonItem Type="RibbonItemType.ComboBox">
    <RibbonComboBoxSettings DataSource="@fonts"
                            FieldSettings="@fieldSettings"
                            aria-label="Font family selection"
                            aria-autocomplete="list">
    </RibbonComboBoxSettings>
</RibbonItem>
```

## Focus Management

### Focus Visible

Visual focus indicator configuration:

```css
.e-ribbon-item:focus {
    outline: 2px solid #0056b3;  /* Sufficient contrast */
    outline-offset: 2px;
}
```

### Tab Index Management

Control keyboard focus order:

```razor
<RibbonGroup HeaderText="Primary">
    <RibbonCollections>
        <RibbonCollection>
            <RibbonItems>
                <!-- Primary action: first in tab order -->
                <RibbonItem Type="RibbonItemType.Button" TabIndex="0">
                    <RibbonButtonSettings Content="Save">
                    </RibbonButtonSettings>
                </RibbonItem>
                <!-- Secondary actions: skip tab initially -->
                <RibbonItem Type="RibbonItemType.Button" TabIndex="-1">
                    <RibbonButtonSettings Content="Save As">
                    </RibbonButtonSettings>
                </RibbonItem>
            </RibbonItems>
        </RibbonCollection>
    </RibbonCollections>
</RibbonGroup>
```

### Focus Restoration

Restore focus after closing popups:

```razor
@using Syncfusion.Blazor.Ribbon

<RibbonItem Type="RibbonItemType.DropDown">
    <RibbonDropDownSettings Content="Format"
                            Items="@formatOptions"
                            PopupClosed="@OnPopupClosed">
    </RibbonDropDownSettings>
</RibbonItem>

@code {

    private void OnPopupClosed(DropDownPopupClosedEventArgs args)
    {
        // Focus returns to button automatically
        Console.WriteLine("Popup closed, focus restored to button");
    }
}
```

## Testing Keyboard Navigation

### Manual Testing Checklist

- [ ] **Tab Key**: Navigate through all focusable elements
- [ ] **Shift+Tab**: Reverse navigation works correctly
- [ ] **Enter/Space**: Buttons and items activate properly
- [ ] **Arrow Keys**: Navigate within groups and dropdowns
- [ ] **Escape**: Close open menus and popups
- [ ] **Focus Visible**: Clear visual focus indicator appears
- [ ] **Focus Order**: Logical, predictable order

### Testing Example

```razor
@page "/accessibility-test"
@using Syncfusion.Blazor.Ribbon

<h2>Keyboard Navigation Test</h2>

<SfRibbon>
    <RibbonTabs>
        <RibbonTab HeaderText="Home">
            <RibbonGroups>
                <RibbonGroup HeaderText="Test Group">
                    <RibbonCollections>
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.Button" TabIndex="0">
                                    <RibbonButtonSettings Content="Item 1" OnClick="@LogClick">
                                    </RibbonButtonSettings>
                                </RibbonItem>
                                <RibbonItem Type="RibbonItemType.Button" TabIndex="0">
                                    <RibbonButtonSettings Content="Item 2" OnClick="@LogClick">
                                    </RibbonButtonSettings>
                                </RibbonItem>
                                <RibbonItem Type="RibbonItemType.Button" TabIndex="0">
                                    <RibbonButtonSettings Content="Item 3" OnClick="@LogClick">
                                    </RibbonButtonSettings>
                                </RibbonItem>
                            </RibbonItems>
                        </RibbonCollection>
                    </RibbonCollections>
                </RibbonGroup>
            </RibbonGroups>
        </RibbonTab>
    </RibbonTabs>
</SfRibbon>

<div style="margin-top: 20px;">
    <h3>Test Results:</h3>
    <ul>
        @foreach (var result in testResults)
        {
            <li>@result</li>
        }
    </ul>
</div>

@code {
    private List<string> testResults = new List<string>();

    private void LogClick(MouseEventArgs args)
    {
        testResults.Insert(0, $"[{DateTime.Now:HH:mm:ss}] Button clicked");
        if (testResults.Count > 10) testResults.RemoveAt(testResults.Count - 1);
        StateHasChanged();
    }
}
```

## Screen Reader Testing

### NVDA (Free, Windows)
- Download from: https://www.nvaccess.org/
- Test navigation, item announcement, status updates

### JAWS (Commercial, Windows)
- Industry standard screen reader
- Comprehensive test scenarios

### VoiceOver (macOS/iOS)
- Built-in screen reader
- Test Touch ID interactions for mobile

## Accessible Design Patterns

### 1. Semantic HTML
```razor
<nav role="navigation" aria-label="Main ribbon toolbar">
    <SfRibbon><!-- ribbon --></SfRibbon>
</nav>
```

### 2. Meaningful Icon Alt Text
```razor
<RibbonItem Type="RibbonItemType.Button" aria-label="Save document">
    <RibbonButtonSettings Content="Save" IconCss="e-icons e-save">
    </RibbonButtonSettings>
</RibbonItem>
```

### 3. Grouping Related Items
```razor
<RibbonGroup HeaderText="Formatting" role="group" aria-label="Text formatting commands">
    <!-- Group items -->
</RibbonGroup>
```

### 4. Clear Error Messages
```csharp
private void OnSave()
{
    if (!ValidateForm())
    {
        ariaLive = "polite";
        errorMessage = "Please fill in all required fields";
    }
}
```

## Color Contrast

### Sufficient Contrast
- **Normal Text**: Minimum 4.5:1 ratio (AA)
- **Large Text**: Minimum 3:1 ratio (AA)
- **UI Components**: Minimum 3:1 ratio

### Testing Tool
- Use WebAIM Contrast Checker: https://webaim.org/resources/contrastchecker/

## Accessible Ribbon Example

Complete accessible ribbon implementation:

```razor
@page "/accessible-ribbon"
@using Syncfusion.Blazor.Ribbon
@using Syncfusion.Blazor.SplitButtons

<nav role="navigation" aria-label="Document editor ribbon">
    <SfRibbon>
        <RibbonTabs>
            <RibbonTab HeaderText="Home" aria-label="Home tab with common commands">
                <RibbonGroups>
                    <RibbonGroup HeaderText="Clipboard" 
                                Orientation="Orientation.Row"
                                role="group"
                                aria-label="Clipboard operations">
                        <RibbonCollections>
                            <RibbonCollection>
                                <RibbonItems>
                                    <RibbonItem Type="RibbonItemType.Button" 
                                               aria-label="Cut selected text (Ctrl+X)">
                                        <RibbonButtonSettings Content="Cut" IconCss="e-icons e-cut">
                                        </RibbonButtonSettings>
                                    </RibbonItem>
                                    <RibbonItem Type="RibbonItemType.Button"
                                               aria-label="Copy selected text (Ctrl+C)">
                                        <RibbonButtonSettings Content="Copy" IconCss="e-icons e-copy">
                                        </RibbonButtonSettings>
                                    </RibbonItem>
                                    <RibbonItem Type="RibbonItemType.Button"
                                               aria-label="Paste from clipboard (Ctrl+V)">
                                        <RibbonButtonSettings Content="Paste" IconCss="e-icons e-paste">
                                        </RibbonButtonSettings>
                                    </RibbonItem>
                                </RibbonItems>
                            </RibbonCollection>
                        </RibbonCollections>
                    </RibbonGroup>
                </RibbonGroups>
            </RibbonTab>
        </RibbonTabs>
    </SfRibbon>
</nav>
```

This implementation ensures full accessibility for users with disabilities, keyboard-only users, and assistive technology users.
