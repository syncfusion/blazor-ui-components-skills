# AI Assistant Popup Appearance & CSS

Customize the AI Assistant popup using CSS class selectors. All styles target the `.e-rte-aiquery-dialog` container.

## CSS Selectors Reference

| Selector | Targets |
|---|---|
| `.e-rte-aiquery-dialog` | Outermost popup container |
| `.e-rte-aiquery-dialog .e-aiassistview` | Main AI assistant view panel |
| `.e-rte-aiquery-dialog .e-aiassistview .e-view-header .e-toolbar` | Header toolbar |
| `.e-rte-aiquery-dialog .e-aiassistview .e-view-header .e-toolbar-items` | Header toolbar items |
| `.e-rte-aiquery-dialog .e-aiassistview .e-view-content .e-toolbar` | Content area toolbar |
| `.e-rte-aiquery-dialog .e-aiassistview .e-view-content .e-toolbar .e-toolbar-items` | Content toolbar items |
| `.e-rte-aiquery-dialog .e-aiassistview .e-view-content .e-toolbar .e-toolbar-item` | Individual content toolbar item |
| `.e-rte-aiquery-dialog .e-aiassistview .e-view-content .e-footer` | Footer / prompt input area |

---

## Basic Popup Style Override

```css
.e-rte-aiquery-dialog.e-dlg-modal.e-popup {
    color: white;
    background: white;
    z-index: 1;
}
```

---

## Complete Custom Popup Styling Example

Targets individual sub-sections for fine-grained control:

```razor
@using Syncfusion.Blazor.SmartRichTextEditor

<SfSmartRichTextEditor Height="300" />

<style>
    /* Main panel */
    .e-rte-aiquery-dialog .e-aiassistview {
        border-color: #e0e0e0;
        background-color: #f4f4f4;
        box-shadow: 3px 3px 10px 0px rgba(0, 0, 0, 0.2);
    }

    /* Header toolbar background */
    .e-rte-aiquery-dialog .e-aiassistview .e-view-header .e-toolbar,
    .e-rte-aiquery-dialog .e-aiassistview .e-view-header .e-toolbar-items {
        background: #d5d5d5;
    }

    /* Content toolbar background */
    .e-rte-aiquery-dialog .e-aiassistview .e-view-content .e-toolbar,
    .e-rte-aiquery-dialog .e-aiassistview .e-view-content .e-toolbar .e-toolbar-items,
    .e-rte-aiquery-dialog .e-aiassistview .e-view-content .e-toolbar .e-toolbar-item {
        background: #f4f4f4;
    }

    /* Footer / input area border */
    .e-rte-aiquery-dialog .e-aiassistview .e-view-content .e-footer {
        border: 3px solid #e0e0e0;
    }
</style>
```

---

## Tips

- Use `.e-rte-aiquery-dialog` as the outermost scoping selector to avoid affecting other Syncfusion popups on the page.
- For dark-mode themes, pair these overrides with the `[data-theme="dark"]` attribute selector.
- The popup respects `PopupWidth` and `PopupMaxHeight` properties set on `<AssistViewSettings>` — use those for sizing and CSS for visual styling.
