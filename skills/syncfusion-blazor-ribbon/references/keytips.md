# KeyTips in Blazor Ribbon Component

KeyTips enable keyboard-only navigation of the Ribbon interface. Users press Alt (or Cmd on macOS) followed by letter combinations to access ribbon commands without using the mouse. This improves accessibility and productivity for keyboard-centric workflows.

## When to Use KeyTips

- Creating accessible ribbon interfaces
- Supporting keyboard-first workflows
- Improving productivity for power users
- Meeting accessibility standards (WCAG 2.1)
- Enterprise applications where keyboard navigation is critical

## Enabling KeyTips

Enable KeyTips at the ribbon level:

```razor
<SfRibbon EnableKeyTips="true">
    <!-- Ribbon content -->
</SfRibbon>
```

## KeyTips for Ribbon Tabs

```razor
<RibbonTab HeaderText="Home" KeyTip="H">
    <RibbonGroups>
        <!-- Groups -->
    </RibbonGroups>
</RibbonTab>

<RibbonTab HeaderText="Insert" KeyTip="I">
    <RibbonGroups>
        <!-- Groups -->
    </RibbonGroups>
</RibbonTab>

<RibbonTab HeaderText="Design" KeyTip="D">
    <RibbonGroups>
        <!-- Groups -->
    </RibbonGroups>
</RibbonTab>
```

## KeyTips for Groups

```razor
<RibbonGroup HeaderText="Clipboard" KeyTip="CD">
    <RibbonCollections>
        <!-- Items -->
    </RibbonCollections>
</RibbonGroup>

<RibbonGroup HeaderText="Font" KeyTip="FB">
    <RibbonCollections>
        <!-- Items -->
    </RibbonCollections>
</RibbonGroup>
```

## KeyTips for Ribbon Items

```razor
<RibbonItem Type="RibbonItemType.SplitButton" KeyTip="PA">
    <RibbonSplitButtonSettings Content="Paste" IconCss="e-icons e-paste">
    </RibbonSplitButtonSettings>
</RibbonItem>

<RibbonItem Type="RibbonItemType.Button" KeyTip="CU">
    <RibbonButtonSettings Content="Cut" IconCss="e-icons e-cut">
    </RibbonButtonSettings>
</RibbonItem>

<RibbonItem Type="RibbonItemType.Button" KeyTip="CO">
    <RibbonButtonSettings Content="Copy" IconCss="e-icons e-copy">
    </RibbonButtonSettings>
</RibbonItem>
```

## KeyTips for File Menu

```razor
<RibbonFileMenuSettings Visible="true" MenuItems="@fileMenuItems" KeyTip="F">
</RibbonFileMenuSettings>

@code {
    List<MenuItem> fileMenuItems = new List<MenuItem>()
    {
        new MenuItem { Text = "New", IconCss = "e-icons e-file-new", Id = "new" },
        new MenuItem { Text = "Open", IconCss = "e-icons e-folder-open", Id = "open" },
        new MenuItem { Text = "Save", IconCss = "e-icons e-save", Id = "save" }
    };
}
```

## KeyTips for Backstage Menu

```razor
<RibbonBackstageMenuSettings KeyTip="F" Visible="true">
    <BackstageMenuItems>
        <BackstageMenuItem KeyTip="H" ID="home" Text="Home" IconCss="e-icons e-home">
            <!-- Content -->
        </BackstageMenuItem>
        <BackstageMenuItem KeyTip="N" ID="new" Text="New" IconCss="e-icons e-file-new">
            <!-- Content -->
        </BackstageMenuItem>
        <BackstageMenuItem KeyTip="O" ID="open" Text="Open" IconCss="e-icons e-folder-open">
            <!-- Content -->
        </BackstageMenuItem>
    </BackstageMenuItems>
</RibbonBackstageMenuSettings>
```

## KeyTips for Layout Switcher

```razor
<SfRibbon EnableKeyTips="true" LayoutSwitcherKeyTip="LS">
    <!-- Ribbon content -->
</SfRibbon>
```

## KeyTips for Launcher Icons

```razor
<RibbonGroup HeaderText="Clipboard" ShowLauncherIcon="true" LauncherIconKeyTip="L">
    <RibbonCollections>
        <!-- Items -->
    </RibbonCollections>
</RibbonGroup>
```

## KeyTips for Group Button Items

For group buttons, assign KeyTips to individual items:

```razor
<RibbonItem Type="RibbonItemType.GroupButton" ID="formatGroup">
    <RibbonGroupButtonSettings Selection="GroupButtonSelection.Single" Items="@formatItems">
    </RibbonGroupButtonSettings>
</RibbonItem>

@code {
    List<GroupButtonItem> formatItems = new List<GroupButtonItem>
    {
        new GroupButtonItem 
        { 
            IconCss = "e-icons e-bold", 
            Content = "Bold", 
            KeyTip = "1" 
        },
        new GroupButtonItem 
        { 
            IconCss = "e-icons e-italic", 
            Content = "Italic", 
            KeyTip = "2" 
        },
        new GroupButtonItem 
        { 
            IconCss = "e-icons e-underline", 
            Content = "Underline", 
            KeyTip = "3" 
        }
    };
}
```

## Complete Example with KeyTips

```razor
<SfRibbon EnableKeyTips="true">
    <RibbonFileMenuSettings Visible="true" KeyTip="F">
    </RibbonFileMenuSettings>
    
    <RibbonTabs>
        <RibbonTab HeaderText="Home" KeyTip="H">
            <RibbonGroups>
                <RibbonGroup HeaderText="Clipboard" KeyTip="CD" ShowLauncherIcon="true" LauncherIconKeyTip="L">
                    <RibbonCollections>
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.SplitButton" KeyTip="PA">
                                    <RibbonSplitButtonSettings Content="Paste" IconCss="e-icons e-paste">
                                    </RibbonSplitButtonSettings>
                                </RibbonItem>
                            </RibbonItems>
                        </RibbonCollection>
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.Button" KeyTip="CU">
                                    <RibbonButtonSettings Content="Cut" IconCss="e-icons e-cut">
                                    </RibbonButtonSettings>
                                </RibbonItem>
                                <RibbonItem Type="RibbonItemType.Button" KeyTip="CO">
                                    <RibbonButtonSettings Content="Copy" IconCss="e-icons e-copy">
                                    </RibbonButtonSettings>
                                </RibbonItem>
                            </RibbonItems>
                        </RibbonCollection>
                    </RibbonCollections>
                </RibbonGroup>
                
                <RibbonGroup HeaderText="Font" KeyTip="FB">
                    <RibbonCollections>
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.ComboBox" KeyTip="FF">
                                    <RibbonComboBoxSettings DataSource="@fontFamilies">
                                    </RibbonComboBoxSettings>
                                </RibbonItem>
                            </RibbonItems>
                        </RibbonCollection>
                    </RibbonCollections>
                </RibbonGroup>
            </RibbonGroups>
        </RibbonTab>

        <RibbonTab HeaderText="Insert" KeyTip="I">
            <RibbonGroups>
                <RibbonGroup HeaderText="Tables" KeyTip="T">
                    <RibbonCollections>
                        <RibbonCollection>
                            <RibbonItems>
                                <RibbonItem Type="RibbonItemType.Button" KeyTip="TI">
                                    <RibbonButtonSettings Content="Table" IconCss="e-icons e-table">
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

@code {
    List<string> fontFamilies = new List<string> { "Arial", "Calibri", "Georgia" };
}
```

## KeyTip Methods

### ShowKeyTipsAsync
Display KeyTips programmatically:

```csharp
await ribbon.ShowKeyTipsAsync();
```

Activate a specific KeyTip:

```csharp
await ribbon.ShowKeyTipsAsync("H");  // Activate Home tab
```

### HideKeyTipsAsync
Hide all visible KeyTips:

```csharp
await ribbon.HideKeyTipsAsync();
```

## KeyTip Guidelines and Best Practices

### 1. **Uniqueness**
Each KeyTip must be unique. Avoid duplicating KeyTips across different items.

```razor
<!-- ✓ Good - Unique KeyTips -->
<RibbonItem Type="RibbonItemType.Button" KeyTip="C">
    <RibbonButtonSettings Content="Copy"></RibbonButtonSettings>
</RibbonItem>
<RibbonItem Type="RibbonItemType.Button" KeyTip="X">
    <RibbonButtonSettings Content="Cut"></RibbonButtonSettings>
</RibbonItem>

<!-- ✗ Bad - Duplicate KeyTips -->
<RibbonItem Type="RibbonItemType.Button" KeyTip="C">
    <RibbonButtonSettings Content="Copy"></RibbonButtonSettings>
</RibbonItem>
<RibbonItem Type="RibbonItemType.Button" KeyTip="C">
    <RibbonButtonSettings Content="Close"></RibbonButtonSettings>
</RibbonItem>
```

### 2. **Hierarchy Consistency**
Use meaningful, consistent patterns for multi-character KeyTips:

```razor
<!-- ✓ Good - Hierarchical KeyTips -->
<RibbonTab HeaderText="Format" KeyTip="O">
    <RibbonGroup HeaderText="Text" KeyTip="T">
        <RibbonItem KeyTip="B">Bold</RibbonItem>  <!-- Alt+O, T, B -->
        <RibbonItem KeyTip="I">Italic</RibbonItem> <!-- Alt+O, T, I -->
    </RibbonGroup>
</RibbonTab>
```

### 3. **Intuitive Shortcuts**
Use first letters or logical mnemonics:

```razor
<!-- ✓ Good - Intuitive KeyTips -->
<RibbonItem Type="RibbonItemType.Button" KeyTip="C">
    <RibbonButtonSettings Content="Copy"></RibbonButtonSettings>
</RibbonItem>
<RibbonItem Type="RibbonItemType.Button" KeyTip="P">
    <RibbonButtonSettings Content="Paste"></RibbonButtonSettings>
</RibbonItem>
```

### 4. **Avoidable KeyTips**
Don't use single letters that conflict with standard shortcuts:

```razor
<!-- Avoid these single-letter KeyTips -->
<!-- Alt+A (typically: Select All) -->
<!-- Alt+X (typically: Cut in some apps) -->
<!-- Alt+Z (typically: Undo) -->
<!-- Alt+Y (typically: Redo) -->
```

### 5. **Documentation**
Provide a KeyTip reference for users:

```razor
<div class="keytip-reference">
    <h3>Keyboard Shortcuts</h3>
    <p>Alt+H - Home Tab</p>
    <p>Alt+H,C,P - Paste from Clipboard</p>
    <p>Alt+H,C,C - Copy</p>
</div>
```

## Accessibility Considerations

- **Screen Readers**: KeyTips should be announced by screen readers
- **Keyboard Focus**: Ensure visual focus indicators are clear
- **WCAG Compliance**: KeyTips support WCAG 2.1 Level AA standards
- **Alternative Input**: Support keyboard-only navigation
- **Learning Curve**: Provide tooltips showing KeyTip combinations

## Common KeyTip Patterns

```
File Menu:     Alt+F
Home Tab:      Alt+H
Insert Tab:    Alt+I
Design Tab:    Alt+D

Clipboard:     Alt+H,CD
Copy:          Alt+H,CD,C
Paste:         Alt+H,CD,P

Font:          Alt+H,FB
Font Size:     Alt+H,FB,FS
Bold:          Alt+H,FB,1
```

## Performance Tips

- KeyTips don't significantly impact performance
- Lazy load complex ribbon structures if needed
- Avoid excessive DOM manipulation in KeyTip handlers
- Cache ribbon references for repeated access

## Related Components

- **Contextual Tabs** - KeyTips also apply to contextual tabs
- **Backstage View** - Support KeyTips for backstage navigation
- **File Menu** - KeyTips for file operations
- **Accessibility** - Part of overall accessibility strategy
