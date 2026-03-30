# Editor Modes

## Table of Contents
- [HTML Mode (Default)](#html-mode-default)
- [IFrame Mode](#iframe-mode)
- [Markdown Mode](#markdown-mode)
- [Source Code / HTML View](#source-code--html-view)

---

## HTML Mode (Default)

`EditorMode.HTML` renders a contenteditable DIV. All toolbar formatting produces standard HTML tags. This is the default and requires no explicit configuration:

```razor
@using Syncfusion.Blazor.RichTextEditor

<SfRichTextEditor EditorMode="EditorMode.HTML">
    <p>The Rich Text Editor is a WYSIWYG editor that returns <b>valid HTML</b>.</p>
    <ul>
        <li>Supports IFRAME and DIV modes</li>
        <li>Modular toolbar</li>
        <li>Markdown editing</li>
    </ul>
</SfRichTextEditor>
```

`Value` / `@bind-Value` contains the serialised HTML string.

---

## IFrame Mode

IFrame mode renders the editing surface inside a sandboxed `<iframe>`, isolating its CSS from the host page. Use it when the host application's global styles would interfere with the editor content.

`IFrame mode` is enabled via the `<RichTextEditorIFrameSettings Enable="true" />` **child component** nested inside `<SfRichTextEditor>`

```razor
@using Syncfusion.Blazor.RichTextEditor

<SfRichTextEditor>
    <RichTextEditorIFrameSettings Enable="true" />
    <p>Editing in an isolated iframe — host-page styles do not bleed in.</p>
</SfRichTextEditor>
```

### Customizing IFrame Body Attributes

Pass additional attributes to the iframe body element using `Attributes`:

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

### Injecting External CSS and Scripts

Inject external stylesheets or scripts into the iframe document using `Resources`:

```razor
<SfRichTextEditor>
    <RichTextEditorIFrameSettings Enable="true" Resources="@Resources" />
</SfRichTextEditor>

@code {
    private ResourcesModel Resources { get; set; } = new ResourcesModel()
    {
        Styles = new string[] { "/styles.css" },
        Scripts = new string[] { "/script.js" }
    };
}
```

> The `ResourcesModel` accepts `Styles` (CSS paths) and `Scripts` (JS paths) — both are string arrays resolved relative to the app root. See `properties.md → RichTextEditorIFrameSettings` for the full child-component reference.

---

## Markdown Mode

Set `EditorMode="EditorMode.Markdown"` to switch to Markdown editing. The editor input and `Value` property use Markdown syntax. A third-party library such as **Marked.js** must be added to the page to render a live HTML preview.

**Supported block tags:** `h1`–`h6`, `blockquote`, `pre`, `p`, ordered list (`OL`), unordered list (`UL`)

**Supported inline tags:** Bold, Italic, StrikeThrough, InlineCode, Subscript, Superscript, UpperCase, LowerCase

```razor
<SfRichTextEditor EditorMode="EditorMode.Markdown" @bind-Value="@MarkdownValue">
    ***Overview*** The Rich Text Editor supports Markdown editing mode.
    ***Key features***
    *Mode*: Provides IFRAME and DIV mode.
    *Toolbar*: Provide a fully customizable toolbar.
    *Preview*: Preview the modified content before saving it.
</SfRichTextEditor>

@code {
    private string MarkdownValue { get; set; } = string.Empty;
}
```

### Markdown Toolbar Items

For Markdown mode, use markdown-specific toolbar commands instead of the HTML defaults:

```razor
<RichTextEditorToolbarSettings Items="@MdTools" />

@code {
    private List<ToolbarItemModel> MdTools = new()
    {
        new() { Command = ToolbarCommand.Bold },
        new() { Command = ToolbarCommand.Italic },
        new() { Command = ToolbarCommand.StrikeThrough },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.OrderedList },
        new() { Command = ToolbarCommand.UnorderedList },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.Formats },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.Preview },        // live HTML preview
        new() { Command = ToolbarCommand.Undo },
        new() { Command = ToolbarCommand.Redo }
    };
}
```

### Displaying a Markdown Preview

**Security:** Do not load Marked from external CDNs. Bundle locally instead.

Download `marked.min.js` and place it in `wwwroot/lib/marked/`:

```html
<!-- wwwroot/index.html or Pages/_Host.cshtml -->
<script src="/lib/marked/marked.min.js"></script>
```

```razor
<SfRichTextEditor EditorMode="EditorMode.Markdown" @bind-Value="@MarkdownValue">
    <RichTextEditorEvents ValueChange="@OnValueChange" />
</SfRichTextEditor>

<div @ref="PreviewDiv"></div>

@code {
    private string MarkdownValue { get; set; } = string.Empty;
    private ElementReference PreviewDiv;

    [Inject] private IJSRuntime JS { get; set; } = default!;

    private async Task OnValueChange(Syncfusion.Blazor.RichTextEditor.ChangeEventArgs args)
    {
        await JS.InvokeVoidAsync("renderMarkdown", PreviewDiv, args.Value);
    }
}
```

```js
// wwwroot/app.js
function renderMarkdown(el, markdown) {
    el.innerHTML = marked.parse(markdown ?? "");
}
```

---

## Source Code / HTML View

Add `ToolbarCommand.SourceCode` to the toolbar to give users an HTML source editor. This is available in HTML mode only — Markdown mode has a `Preview` button instead:

```razor
<RichTextEditorToolbarSettings Items="@Tools" />

@code {
    private List<ToolbarItemModel> Tools = new()
    {
        new() { Command = ToolbarCommand.Bold },
        new() { Command = ToolbarCommand.Italic },
        new() { Command = ToolbarCommand.Separator },
        new() { Command = ToolbarCommand.SourceCode }   // toggle HTML source view
    };
}
```

> Toggling to source code view and back does not lose content. The WYSIWYG surface and source view stay in sync.
