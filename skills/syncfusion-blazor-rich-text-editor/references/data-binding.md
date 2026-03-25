# Data Binding

## Table of Contents
- [Two-way Binding with @bind-Value](#two-way-binding-with-bind-value)
- [One-way Binding with Value](#one-way-binding-with-value)
- [Retrieving Content Programmatically](#retrieving-content-programmatically)
- [Auto-save](#auto-save)
- [Character Count and Limits](#character-count-and-limits)
- [Read-only Mode](#read-only-mode)

---

## Two-way Binding with @bind-Value

`@bind-Value` keeps a C# string variable and the editor's HTML content in sync. Changes in the editor update the variable; changes to the variable update the editor:

```razor
@using Syncfusion.Blazor.RichTextEditor

<SfRichTextEditor @bind-Value="@Content" />

<!-- Live preview of bound value -->
<textarea rows="5" cols="60" @bind="@Content" />

@code {
    private string Content { get; set; } = "<p>Start editing here.</p>";
}
```

> `Value` accepts `string` type and holds valid HTML (or Markdown when `EditorMode="Markdown"`).

---

## One-way Binding with Value

Supply an initial HTML string via `Value` and handle changes with `ValueChange` when you need more control over when the bound state updates:

```razor
<SfRichTextEditor Value="@InitialContent">
    <RichTextEditorEvents ValueChange="@OnChange" />
</SfRichTextEditor>

@code {
    private string InitialContent = "<p>Default content.</p>";

    private void OnChange(Syncfusion.Blazor.RichTextEditor.ChangeEventArgs args)
    {
        // args.Value holds the current editor HTML
        // Save to DB, validate, etc.
        InitialContent = args.Value;
    }
}
```

> `ValueChange` fires when the editor loses focus **or** at the configured `SaveInterval`. It does not fire on every keystroke — use `OnActionComplete` for keystroke-level notifications.

---

## Retrieving Content Programmatically

Use the `@ref` attribute and async API methods to pull content without waiting for a blur:

```razor
<SfRichTextEditor @ref="RteObj" @bind-Value="@Content">
    <p>Type something here.</p>
</SfRichTextEditor>
<button @onclick="GetContent">Get Content</button>

@code {
    private SfRichTextEditor RteObj = default!;
    private string Content { get; set; } = string.Empty;

    private async Task GetContent()
    {
        // Plain-text length (excludes HTML tags)
        int charCount = await RteObj.GetCharCountAsync();

        // Inner text (no tags)
        string text = await RteObj.GetTextAsync();

        // Full HTML string (same as Value)
        string html = Content;
    }
}
```

---

## Auto-save

Configure periodic or idle-triggered saving with `SaveInterval` and `AutoSaveOnIdle`:

| Property | Type | Default | Description |
|---|---|---|---|
| `SaveInterval` | `int` | `10000` | Interval in ms at which `ValueChange` fires (or idle timeout when `AutoSaveOnIdle` is `true`) |
| `AutoSaveOnIdle` | `bool` | `false` | When `true`, saves after the user stops typing for `SaveInterval` ms instead of on a fixed timer |

```razor
<SfRichTextEditor SaveInterval="5000" AutoSaveOnIdle="true" @bind-Value="@Content">
    <RichTextEditorEvents ValueChange="@OnAutoSave" />
    <p>Content is saved automatically after 5 s of idle time.</p>
</SfRichTextEditor>

@code {
    private string Content { get; set; } = string.Empty;

    private async Task OnAutoSave(Syncfusion.Blazor.RichTextEditor.ChangeEventArgs args)
    {
        Content = args.Value;
        await MyService.PersistAsync(Content);   // save to DB / local storage
    }
}
```

---

## Character Count and Limits

Show a live character counter and optionally cap content length:

```razor
<SfRichTextEditor ShowCharCount="true" MaxLength="500" @bind-Value="@Content">
    <p>This editor limits content to 500 characters.</p>
</SfRichTextEditor>

@code {
    private string Content { get; set; } = string.Empty;
}
```

| Property | Type | Default | Description |
|---|---|---|---|
| `ShowCharCount` | `bool` | `false` | Displays a character counter in the bottom-right corner |
| `MaxLength` | `int` | `int.MaxValue` | Hard cap on character count; typing stops at the limit |

---

## Read-only Mode

Set `Readonly="true"` to display content without allowing edits. The toolbar is hidden automatically. Toggle it at runtime via state:

```razor
<SfRichTextEditor Readonly="@IsReadonly" @bind-Value="@Content">
    <p>This content is read-only by default.</p>
</SfRichTextEditor>
<button @onclick="() => IsReadonly = !IsReadonly">Toggle Edit</button>

@code {
    private bool IsReadonly = true;
    private string Content { get; set; } = "<p>Saved content displayed here.</p>";
}
```

> In read-only mode the editor renders as a styled HTML container, preserving all formatting. Combine with `EnableHtmlSanitizer="true"` (the default) to prevent XSS when displaying user-supplied content.
