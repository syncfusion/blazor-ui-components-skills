# Localization in Blazor Kanban Component

The Kanban component supports localization of text strings and RTL (Right-to-Left) layout for international audiences.

## Table of Contents

- [Setting the Locale](#setting-the-locale)
- [RTL (Right-to-Left) Mode](#rtl-right-to-left-mode)
- [RTL with Swimlanes](#rtl-with-swimlanes)

## Setting the Locale

Use the `Locale` property to load locale-specific text. The locale package must be registered in the app:

```cshtml
@using Syncfusion.Blazor.Kanban

<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks" Locale="ar">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
</SfKanban>
```

### Registering Locale Data

Add the locale data to your `Program.cs` (or startup configuration):

```csharp
using Syncfusion.Blazor;

// Register locale data
var culture = new CultureInfo("ar");
CultureInfo.DefaultThreadCurrentCulture = culture;
CultureInfo.DefaultThreadCurrentUICulture = culture;
```

Or load locale JSON data at runtime:

```javascript
// In wwwroot/js/loadLocale.js
window.loadLocale = async function (locale) {
    const data = await fetch(`/locales/${locale}.json`);
    const json = await data.json();
    ej.base.L10n.load(json);
};
```

## RTL (Right-to-Left) Mode

Enable RTL layout for Arabic, Hebrew, and other RTL languages with `EnableRtl`:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks"
          Locale="ar" EnableRtl="true">
    <KanbanColumns>
        <KanbanColumn HeaderText="في المعالجة" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="قيد التنفيذ" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="مكتمل" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
</SfKanban>
```

When RTL is enabled:
- Columns render from right to left
- Text alignment is right-aligned
- Card drag direction is reversed
- Dialog layout mirrors for RTL reading

## RTL with Swimlanes

RTL also applies to swimlane rows:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks"
          Locale="ar" EnableRtl="true">
    <KanbanColumns>
        <KanbanColumn HeaderText="في المعالجة" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="قيد التنفيذ" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="مكتمل" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
    <KanbanSwimlaneSettings KeyField="Assignee" TextField="Assignee"></KanbanSwimlaneSettings>
</SfKanban>
```

## Localization Properties Reference

| Property | Type | Description |
|----------|------|-------------|
| `Locale` | string | Locale code (e.g., `"ar"`, `"fr"`, `"de"`) for translating UI strings |
| `EnableRtl` | bool | Enable right-to-left layout (default: false) |

## Localizable String Keys

The following Kanban string keys can be overridden in locale files:

| Key | Default Value | Description |
|-----|---------------|-------------|
| `Kanban_Items` | `"items"` | Items label in column header |
| `Kanban_Min` | `"Min"` | Minimum constraint label |
| `Kanban_Max` | `"Max"` | Maximum constraint label |
| `Kanban_Cards` | `"Cards"` | Cards label |
| `Kanban_EmptyContent` | `"No cards to display"` | Empty column message |
| `Kanban_EmptyFilters` | `"No records"` | Empty filter message |
| `Kanban_AddTitle` | `"Add New Card"` | Dialog title when adding |
| `Kanban_EditTitle` | `"Edit Card"` | Dialog title when editing |
| `Kanban_DeleteContent` | `"Are you sure you want to delete this card?"` | Delete confirmation |
| `Kanban_DeleteTitle` | `"Delete Card"` | Delete dialog title |
