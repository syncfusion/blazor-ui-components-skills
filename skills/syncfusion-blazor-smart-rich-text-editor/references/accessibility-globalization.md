# Accessibility and Globalization

## Table of Contents
- [Accessibility Compliance Overview](#accessibility-compliance-overview)
- [WAI-ARIA Attributes](#wai-aria-attributes)
- [Keyboard Shortcuts Reference](#keyboard-shortcuts-reference)
- [Custom Key Configuration](#custom-key-configuration)
- [RTL Support](#rtl-support)
- [Localization](#localization)
- [XHTML Validation](#xhtml-validation)

---

## Accessibility Compliance Overview

The Smart Rich Text Editor is built to comply with the following accessibility standards:

| Standard | Level / Status |
|---|---|
| WCAG 2.2 | AA |
| Section 508 | ✅ Full support |
| WAI-ARIA | ✅ Roles, states, properties applied |
| Screen Reader | ✅ Supported |
| Right-to-Left (RTL) | ✅ Supported |
| Keyboard Navigation | ✅ Full support |
| Color Contrast | ⚠️ Partial (some elements) |
| Mobile Device | ✅ Supported |
| Axe-core Validation | ✅ Passes automated checks |

---

## WAI-ARIA Attributes

The toolbar element is assigned `role="toolbar"`:

| Attribute | Value / Description |
|---|---|
| `role="toolbar"` | Identifies the toolbar region |
| `aria-orientation` | `horizontal` (default) |
| `aria-haspopup` | `true` when popup mode is active |
| `aria-disabled` | Reflects toolbar disabled state |
| `aria-owns` | References the editor element ID from the toolbar popup |

The editor content area carries `role="application"`:

| Attribute | Description |
|---|---|
| `role="application"` | Marks the editing surface as an interactive application region |
| `aria-disabled` | Reflects read-only/disabled state |

---

## Keyboard Shortcuts Reference

### Toolbar Navigation

| Action | Windows | Mac |
|---|---|---|
| Focus toolbar | `Alt + F10` | `⌥ + F10` |
| Next tool | `→` | `→` |
| Previous tool | `←` | `←` |
| Execute focused tool | `Enter` / `Space` | `Enter` / `Space` |
| Close dropdown / dialog | `Esc` | `Esc` |

### Content Editing and Formatting

| Action | Windows | Mac |
|---|---|---|
| Select all | `Ctrl + A` | `⌘ + A` |
| Bold | `Ctrl + B` | `⌘ + B` |
| Italic | `Ctrl + I` | `⌘ + I` |
| Strikethrough | `Ctrl + Shift + S` | `⌘ + ⇧ + S` |
| Inline code | `Ctrl + `` ` `` ` | `⌘ + `` ` `` ` |
| Create link | `Ctrl + K` | `⌘ + K` |
| New paragraph (hard break) | `Enter` | `Enter` |
| Soft line break | `Shift + Enter` | `⇧ + Enter` |
| Copy format painter | `Alt + Shift + C` | `⌥ + ⌘ + C` |
| Paste format painter | `Alt + Shift + V` | `⌥ + ⌘ + V` |
| Clear format painter | `Esc` | `Esc` |

### Text Case and Script

| Action | Windows | Mac |
|---|---|---|
| Uppercase | `Ctrl + Shift + U` | `⌘ + ⇧ + U` |
| Lowercase | `Ctrl + Shift + L` | `⌘ + ⇧ + L` |
| Superscript | `Ctrl + Shift + =` | `⌘ + ⇧ + =` |
| Subscript | `Ctrl + =` | `⌘ + =` |

### Alignment and Indentation

| Action | Windows | Mac |
|---|---|---|
| Align left | `Ctrl + L` | `⌘ + L` |
| Align centre | `Ctrl + E` | `⌘ + E` |
| Align right | `Ctrl + R` | `⌘ + R` |
| Justify | `Ctrl + J` | `⌘ + J` |
| Increase indent | `Ctrl + ]` | `⌘ + ]` |
| Decrease indent | `Ctrl + [` | `⌘ + [` |

### Lists

| Action | Windows | Mac |
|---|---|---|
| Ordered list | `Ctrl + Shift + O` | `⌘ + ⇧ + O` |
| Unordered list | `Ctrl + Alt + O` | `⌘ + ⌥ + O` |

### Insert

| Action | Windows | Mac |
|---|---|---|
| Insert table dialog | `Ctrl + Shift + E` | `⌘ + ⇧ + E` |
| Insert image dialog | `Ctrl + Shift + I` | `⌘ + ⇧ + I` |
| Insert audio dialog | `Ctrl + Shift + A` | `⌘ + ⇧ + A` |
| Insert video dialog | `Ctrl + Alt + V` | `⌘ + ⌥ + V` |

### Table Navigation

| Action | Windows | Mac |
|---|---|---|
| Next cell | `Tab` | `Tab` |
| Previous cell | `Shift + Tab` | `⇧ + Tab` |
| Navigate cells | `↑ ↓ ← →` | `↑ ↓ ← →` |
| Insert new row (last cell) | `Tab` | `Tab` |

### Clipboard

| Action | Windows | Mac |
|---|---|---|
| Copy | `Ctrl + C` | `⌘ + C` |
| Cut | `Ctrl + X` | `⌘ + X` |
| Paste | `Ctrl + V` | `⌘ + V` |
| Paste as plain text | `Ctrl + Shift + V` | `⌘ + ⌥ + ⇧ + V` |

### Undo / Redo and Misc

| Action | Windows | Mac |
|---|---|---|
| Undo | `Ctrl + Z` | `⌘ + Z` |
| Redo | `Ctrl + Y` | `⌘ + Y` |
| View HTML source | `Ctrl + Shift + H` | `⌘ + ⇧ + H` |
| Toggle fullscreen | `Ctrl + Shift + F` | `⌘ + ⇧ + F` |
| Exit fullscreen | `Esc` | `Esc` |
| Clear all formatting | `Ctrl + Shift + R` | `⌘ + ⇧ + R` |

---

## Custom Key Configuration

Override default shortcuts with `KeyConfigure`:

```razor
<SfSmartRichTextEditor KeyConfigure="@Keys">
    <p>Bold is now Ctrl+1, Italic is Ctrl+2.</p>
</SfSmartRichTextEditor>

@code {
    private ShortcutKeys Keys = new()
    {
        Bold   = "ctrl+1",
        Italic = "ctrl+2"
    };
}
```

> Use `ShortcutKeys` properties matching the `CommandName` actions you want to override. Unspecified keys keep their defaults.

---

## RTL Support

Enable right-to-left layout for Arabic, Hebrew, and other RTL languages:

```razor
<SfSmartRichTextEditor EnableRtl="true">
    <p dir="rtl">محتوى باللغة العربية</p>
</SfSmartRichTextEditor>
```

> `EnableRtl` does **not** change automatically based on the browser culture — set it explicitly when needed.

---

## Localization

The RTE participates in Syncfusion's standard Blazor localization pipeline. Register resource strings in `Program.cs`:

```csharp
// Program.cs (Blazor Server)
builder.Services.AddLocalization(options => options.ResourcesPath = "Resources");
builder.Services.AddSyncfusionBlazor();

var app = builder.Build();
app.UseRequestLocalization(new RequestLocalizationOptions()
    .SetDefaultCulture("ar")
    .AddSupportedCultures("en-US", "ar", "de")
    .AddSupportedUICultures("en-US", "ar", "de"));
```

Refer to the [Syncfusion Blazor Localization](https://blazor.syncfusion.com/documentation/common/localization) guide for adding locale resource files.

---

## XHTML Validation

Enable `EnableXhtml` to continuously validate the editor's content against XHTML rules. Invalid constructs are automatically removed:

```razor
<SfSmartRichTextEditor EnableXhtml="true">
    <p>Content is continuously validated as you type.</p>
</SfSmartRichTextEditor>
```

**What is validated:**

| Rule | Detail |
|---|---|
| Attribute case | All attributes must be lowercase |
| Quoted values | Attribute values must be in quotation marks |
| Valid attributes | Only spec-valid attributes per element |
| Required attributes | Missing required attributes are flagged |
| Tag case | All HTML tags must be lowercase |
| Proper closing | Every opening tag must have a matching close |
| Valid elements | Unknown elements are removed |
| Nesting | Inline elements cannot wrap block elements |
| Single root | Content must have one root element |

> Combine `EnableXhtml="true"` with `EnableHtmlSanitizer="true"` (the default) for maximum security when displaying user-generated content.
``