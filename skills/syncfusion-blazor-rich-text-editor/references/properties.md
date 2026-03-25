````markdown
# SfRichTextEditor – Properties Reference

## Table of Contents
- [Content & Value](#content--value)
- [Editor Behaviour](#editor-behaviour)
- [Appearance & Layout](#appearance--layout)
- [Security & Sanitization](#security--sanitization)
- [Keyboard & Shortcuts](#keyboard--shortcuts)
- [Persistence & Auto-Save](#persistence--auto-save)
- [Undo / Redo](#undo--redo)
- [Full Property Summary Table](#full-property-summary-table)

---

## Content & Value

### `Value` — `string?` — default: `null`
Current HTML (or Markdown) content of the editor. Use `@bind-Value` for two-way binding.

```razor
<SfRichTextEditor @bind-Value="@HtmlContent" />

@code {
    private string HtmlContent { get; set; } = "<p>Start editing…</p>";
}
```

### `Placeholder` — `string?` — default: `""`
Text shown when the editor has no content. Disappears on focus.

```razor
<SfRichTextEditor Placeholder="Start to type…" />
```

### `Readonly` — `bool` — default: `false`
When `true`, the editor content cannot be changed by user interaction. You can still update `Value` programmatically.

```razor
<SfRichTextEditor Value="@HtmlContent" Readonly="true">
    <RichTextEditorToolbarSettings Visible="false" />
</SfRichTextEditor>
```

### `Enabled` — `bool` — default: `true`
When `false`, the entire editor is disabled and non-interactive.

### `MaxLength` — `int` — default: `-1` (no limit)
Maximum characters allowed. Excess content is truncated on paste. Pair with `ShowCharCount` to give visual feedback.

```razor
<SfRichTextEditor MaxLength="2000" ShowCharCount="true" />
```

### `ShowCharCount` — `bool` — default: `false`
Displays a character counter at the bottom of the editor.

### `ChildContent` — `RenderFragment?` — default: `null`
Embed child components (toolbar settings, events, image settings, etc.) as nested Razor tags.

---

## Editor Behaviour

### `EditorMode` — `EditorMode` — default: `EditorMode.HTML`
Switches between HTML WYSIWYG mode and Markdown editing mode.

```razor
<!-- Markdown mode -->
<SfRichTextEditor EditorMode="EditorMode.Markdown" @bind-Value="@MarkdownContent" />
```

| Value | Behaviour |
|---|---|
| `HTML` | WYSIWYG HTML editing (default) |
| `Markdown` | Raw Markdown editing with syntax support |

### `EnterKey` — `EnterKeyTag` — default: `EnterKeyTag.P`
HTML tag inserted when **Enter** is pressed.

| Value | Tag inserted |
|---|---|
| `P` (default) | `<p>` |
| `DIV` | `<div>` |
| `BR` | `<br>` |

### `ShiftEnterKey` — `ShiftEnterKeyTag` — default: `ShiftEnterKeyTag.BR`
HTML tag inserted when **Shift + Enter** is pressed.

| Value | Tag inserted |
|---|---|
| `BR` (default) | `<br>` |
| `P` | `<p>` |
| `DIV` | `<div>` |

### `EnableTabKey` — `bool` — default: `false`
When `true`, pressing **Tab** inserts a tab space inside the content instead of moving focus.

### `EnableAutoUrl` — `bool` — default: `false`
When `false` (default), typed relative URLs are auto-prefixed with `https://`. Set to `true` to accept URLs as-is without validation.

### `EnableMarkdownAutoFormat` — `bool` — default: `true`
Auto-converts Markdown shortcodes (e.g., `**text**` → bold) during typing and paste in Markdown mode.

### `EnableClipboardCleanup` — `bool` — default: `true`
Intercepts copy and cut operations to strip unwanted inline styles from clipboard content.

### `EnableXhtml` — `bool` — default: `false`
Enforces XHTML-valid output. Use `GetXhtmlAsync()` to retrieve the validated content.

### `EnableHtmlEncode` — `bool` — default: `false`
Displays source code in HTML-encoded format (applies to HTML mode only).

### `EnableResize` — `bool` — default: `false`
Enables a resize handle on the editor content area.

---

## Child Component Settings

### `RichTextEditorIFrameSettings` — child component
Renders the editor content area inside a sandboxed `<iframe>`, isolating it from the host page's global CSS and scripts. Nest this component directly inside `<SfRichTextEditor>`.

> ⚠️ **Do NOT use `EnableIFrame="true"` on `SfRichTextEditor`** — that attribute does not exist. IFrame mode is configured exclusively through `<RichTextEditorIFrameSettings>`.

**Basic usage:**
```razor
<SfRichTextEditor>
    <RichTextEditorIFrameSettings Enable="true" />
</SfRichTextEditor>
```

**Properties of `RichTextEditorIFrameSettings`:**

| Property | Type | Default | Description |
|---|---|---|---|
| `Enable` | `bool` | `false` | Activates iframe editing mode |
| `Attributes` | `Dictionary<string, object>?` | `null` | Additional HTML attributes applied to the iframe `<body>` element |
| `Resources` | `ResourcesModel?` | `null` | External CSS/JS files to inject into the iframe document |

**`ResourcesModel` properties:**

| Property | Type | Description |
|---|---|---|
| `Styles` | `string[]` | Paths to external CSS files (resolved from app root) |
| `Scripts` | `string[]` | Paths to external JS files (resolved from app root) |

**With custom body attributes:**
```razor
<SfRichTextEditor>
    <RichTextEditorIFrameSettings Enable="true" Attributes="@IframeAttributes" />
</SfRichTextEditor>

@code {
    private Dictionary<string, object> IframeAttributes = new()
    {
        { "style", "background: lightgray;" }
    };
}
```

**With injected styles and scripts:**
```razor
<SfRichTextEditor>
    <RichTextEditorIFrameSettings Enable="true" Resources="@Resources" />
</SfRichTextEditor>

@code {
    private ResourcesModel Resources { get; set; } = new ResourcesModel()
    {
        Styles = new string[] { "/editor-content.css" },
        Scripts = new string[] { "/editor-extensions.js" }
    };
}
```

---

## Appearance & Layout

### `Height` — `string` — default: `"auto"`
Sets the editor height in pixels or percentage.

```razor
<SfRichTextEditor Height="400px" />
<SfRichTextEditor Height="60%" />
```

### `Width` — `string` — default: `"100%"`
Sets the editor width.

```razor
<SfRichTextEditor Width="800px" />
```

### `CssClass` — `string?` — default: `""`
One or more custom CSS classes (space-separated) for styling the editor container.

### `ShowTooltip` — `bool` — default: `true`
Controls whether tooltips are shown for toolbar and inline toolbar items.

### `FloatingToolbarOffset` — `double` — default: `0`
Top offset (px) to preserve the floating toolbar position during page scroll.

---

## Security & Sanitization

### `EnableHtmlSanitizer` — `bool` — default: `true`
Sanitizes editor content to prevent XSS attacks. Keep `true` in production.

### `AdditionalSanitizeAttributes` — `List<SanitizeAttribute>?`
Specify additional element+attribute pairs to be removed during sanitization.

```razor
<SfRichTextEditor EnableHtmlSanitizer="true"
                  AdditionalSanitizeAttributes="@sanitizeAttrs" />

@code {
    private List<SanitizeAttribute> sanitizeAttrs = new()
    {
        new SanitizeAttribute { Selector = "span", Attribute = "style" }
    };
}
```

**`SanitizeAttribute` properties:**
| Property | Type | Description |
|---|---|---|
| `Selector` | `string?` | CSS selector for the target elements |
| `Attribute` | `string?` | Attribute name to be removed from matched elements |

### `AdditionalSanitizeTags` — `string[]?`
Extra tag names to add to the block list (prevent insertion).

```razor
<SfRichTextEditor AdditionalSanitizeTags="@(new[] { "script", "style" })" />
```

### `DeniedSanitizeSelectors` — `string[]?`
Remove selectors from the **default** sanitization list to allow them in content.

```razor
<!-- Allow iframes from known sources -->
<SfRichTextEditor DeniedSanitizeSelectors="@(new[] { "iframe[src]" })" />
```

Default sanitized selectors include: `script`, `iframe[src]`, `link[href*="javascript:"]`, and several others.

---

## Keyboard & Shortcuts

### `KeyConfigure` — `ShortcutKeys?`
Customize keyboard shortcuts for editor operations.

```razor
<SfRichTextEditor KeyConfigure="@shortcutKeys" />

@code {
    ShortcutKeys shortcutKeys = new() { Bold = "ctrl+d", Italic = "ctrl+m" };
}
```

**`ShortcutKeys` default bindings (key selections):**

| Property | Default |
|---|---|
| `Bold` | `ctrl+b` |
| `Italic` | `ctrl+i` |
| `Underline` | `ctrl+u` |
| `Undo` | `ctrl+z` |
| `Redo` | `ctrl+y` |
| `InsertLink` | `ctrl+k` |
| `InsertImage` | `ctrl+shift+i` |
| `InsertTable` | `ctrl+shift+e` |
| `FullScreen` | `ctrl+shift+f` |
| `HtmlSource` | `ctrl+shift+h` |
| `FormatCopy` | `alt+shift+c` |
| `FormatPaste` | `alt+shift+v` |
| `ToolbarFocus` | `alt+f10` |

> All 28 shortcut keys can be overridden. See property.md for the full list.

### `ID` — `string?`
Unique identifier for the component. Required when using multiple editors on one page.

### `HttpClientInstance` — `HttpClient?` — default: `null`
Custom `HttpClient` for all upload/import/export requests. Configure headers (e.g., `Authorization`) on the injected instance.

```razor
@inject HttpClient httpClient

<SfRichTextEditor HttpClientInstance="@httpClient">
    <RichTextEditorImageSettings SaveUrl="https://api.example.com/upload" />
</SfRichTextEditor>
```

---

## Persistence & Auto-Save

### `EnablePersistence` — `bool` — default: `false`
Saves `Value` to browser `localStorage` and restores it on page reload.

### `SaveInterval` — `double` — default: `10000` (ms)
Interval in milliseconds after which `ValueChange` fires if content was modified. Used with `AutoSaveOnIdle`.

### `AutoSaveOnIdle` — `bool` — default: `false`
When `true`, triggers a save after the editor has been idle for `SaveInterval` ms.

```razor
<!-- Auto-save every 5s of idle time -->
<SfRichTextEditor SaveInterval="5000" AutoSaveOnIdle="true">
    <RichTextEditorEvents ValueChange="@OnSave" />
</SfRichTextEditor>
```

---

## Undo / Redo

### `UndoRedoSteps` — `int` — default: `30`
Maximum number of undo/redo history steps retained.

```razor
<SfRichTextEditor UndoRedoSteps="50" />
```

### `UndoRedoTimer` — `int` — default: `300` (ms)
Idle time in milliseconds after typing stops before a new undo checkpoint is created.

```razor
<!-- Capture undo points faster while typing -->
<SfRichTextEditor UndoRedoTimer="150" />
```

---

## Full Property Summary Table

| # | Property | Type | Default |
|---|---|---|---|
| 1 | `AdditionalSanitizeAttributes` | `List<SanitizeAttribute>?` | empty list |
| 2 | `AdditionalSanitizeTags` | `string[]?` | empty array |
| 3 | `AutoSaveOnIdle` | `bool` | `false` |
| 4 | `ChildContent` | `RenderFragment?` | `null` |
| 5 | `CssClass` | `string?` | `""` |
| 6 | `DeniedSanitizeSelectors` | `string[]?` | empty array |
| 7 | `EditorMode` | `EditorMode` | `HTML` |
| 8 | `EnableAutoUrl` | `bool` | `false` |
| 9 | `EnableClipboardCleanup` | `bool` | `true` |
| 10 | `EnableHtmlEncode` | `bool` | `false` |
| 11 | `EnableHtmlSanitizer` | `bool` | `true` |
| 12 | `EnableMarkdownAutoFormat` | `bool` | `true` |
| 13 | `EnablePersistence` | `bool` | `false` |
| 14 | `EnableResize` | `bool` | `false` |
| 15 | `EnableRtl` | `bool` | `false` |
| 16 | `EnableTabKey` | `bool` | `false` |
| 17 | `EnableXhtml` | `bool` | `false` |
| 18 | `Enabled` | `bool` | `true` |
| 19 | `EnterKey` | `EnterKeyTag` | `P` |
| 20 | `FloatingToolbarOffset` | `double` | `0` |
| 21 | `Height` | `string` | `"auto"` |
| 22 | `HttpClientInstance` | `HttpClient?` | `null` |
| 23 | `ID` | `string?` | `""` |
| 24 | `KeyConfigure` | `ShortcutKeys?` | built-in defaults |
| 25 | `MaxLength` | `int` | `-1` |
| 26 | `Placeholder` | `string?` | `""` |
| 27 | `Readonly` | `bool` | `false` |
| 28 | `RichTextEditorIFrameSettings` | child component | `Enable=false` — see `## Child Component Settings` |
| 29 | `SaveInterval` | `double` | `10000` ms |
| 30 | `ShiftEnterKey` | `ShiftEnterKeyTag` | `BR` |
| 31 | `ShowCharCount` | `bool` | `false` |
| 32 | `ShowTooltip` | `bool` | `true` |
| 33 | `UndoRedoSteps` | `int` | `30` |
| 34 | `UndoRedoTimer` | `int` | `300` ms |
| 35 | `Value` | `string?` | — |
| 36 | `ValueChanged` | `EventCallback<string?>` | — |
| 37 | `ValueExpression` | `Expression<Func<string?>>?` | — |
| 38 | `Width` | `string` | `"100%"` |
````
