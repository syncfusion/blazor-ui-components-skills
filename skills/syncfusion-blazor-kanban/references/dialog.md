# Dialog in Blazor Kanban Component

The Kanban component provides a built-in dialog for adding, editing, and deleting cards by double-clicking a card or an empty cell.

## Table of Contents

- [Default Dialog Behavior](#dialog)
- [Custom Fields in Dialog](#custom-fields)
- [Dialog Template](#dialog-template)
- [Preventing Dialog from Opening](#prevent-dialog)
- [Server-Side CRUD Operations](#server-side-crud)

## Dialog

The dialog opens automatically on double-click. Use `KanbanDialogSettings` to configure:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
    <KanbanDialogSettings>
        <KanbanDialogSettingsFields>
            <KanbanDialogSettingsField Text="ID" Key="Id" Type="DialogFieldType.TextBox"></KanbanDialogSettingsField>
            <KanbanDialogSettingsField Text="Status" Key="Status" Type="DialogFieldType.DropDown"></KanbanDialogSettingsField>
            <KanbanDialogSettingsField Text="Assignee" Key="Assignee" Type="DialogFieldType.DropDown"></KanbanDialogSettingsField>
            <KanbanDialogSettingsField Text="Priority" Key="Priority" Type="DialogFieldType.DropDown"></KanbanDialogSettingsField>
            <KanbanDialogSettingsField Text="Summary" Key="Summary" Type="DialogFieldType.TextArea"></KanbanDialogSettingsField>
        </KanbanDialogSettingsFields>
    </KanbanDialogSettings>
</SfKanban>
```

## Custom Fields

Control which fields appear in the dialog and their input types:

| `DialogFieldType` | Description |
|-------------------|-------------|
| `TextBox` | Single-line text input field |
| `DropDown` | Dropdown list |
| `Numeric` | Numeric input |
| `TextArea` | Multi-line text area |

```cshtml
<KanbanDialogSettingsFields>
    <KanbanDialogSettingsField Text="Title" Key="Title" Type="DialogFieldType.TextBox"></KanbanDialogSettingsField>
    <KanbanDialogSettingsField Text="Status" Key="Status" Type="DialogFieldType.DropDown"></KanbanDialogSettingsField>
    <KanbanDialogSettingsField Text="Story Points" Key="StoryPoints" Type="DialogFieldType.Numeric"></KanbanDialogSettingsField>
    <KanbanDialogSettingsField Text="Description" Key="Summary" Type="DialogFieldType.TextArea"></KanbanDialogSettingsField>
</KanbanDialogSettingsFields>
```

## Dialog Template

Fully replace the dialog content with a custom template using `Template` inside `KanbanDialogSettings`:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
    <KanbanDialogSettings>
        <Template>
            @{
                TasksModel data = (TasksModel)context;
                <table>
                    <tbody>
                        <tr>
                            <td class="e-label">ID</td>
                            <td><input class="e-field e-input" name="Id" value="@data.Id" /></td>
                        </tr>
                        <tr>
                            <td class="e-label">Status</td>
                            <td>
                                <SfDropDownList TValue="string" TItem="DropDownModel"
                                    DataSource="StatusData" Value="@data.Status">
                                    <DropDownListFieldSettings Text="Value" Value="Value"></DropDownListFieldSettings>
                                </SfDropDownList>
                            </td>
                        </tr>
                        <tr>
                            <td class="e-label">Summary</td>
                            <td><textarea class="e-field e-input" name="Summary">@data.Summary</textarea></td>
                        </tr>
                    </tbody>
                </table>
            }
        </Template>
    </KanbanDialogSettings>
</SfKanban>

@code {
    public class DropDownModel { public string Value { get; set; } }
    public List<DropDownModel> StatusData = new List<DropDownModel>
    {
        new DropDownModel { Value = "Open" },
        new DropDownModel { Value = "InProgress" },
        new DropDownModel { Value = "Close" }
    };
}
```

## Prevent Dialog

Use the `DialogOpen` event and set `args.Cancel = true` to prevent the dialog from opening:

```cshtml
<KanbanEvents TValue="TasksModel" DialogOpen="DialogOpenHandler"></KanbanEvents>

@code {
    private void DialogOpenHandler(DialogOpenEventArgs<TasksModel> args)
    {
        args.Cancel = true; // Prevent dialog from opening
    }
}
```

## Server-Side CRUD

When using a remote data source (UrlAdaptor), CRUD operations are handled by the server. The Kanban sends requests automatically on drag/drop and dialog save/delete:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status">
    <KanbanDataManager Url="/api/Kanban" AdaptorType="Adaptors.UrlAdaptor" CrossDomain="true"></KanbanDataManager>
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
    <KanbanDialogSettings>
        <KanbanDialogSettingsFields>
            <KanbanDialogSettingsField Text="ID" Key="Id" Type="DialogFieldType.TextBox"></KanbanDialogSettingsField>
            <KanbanDialogSettingsField Text="Status" Key="Status" Type="DialogFieldType.DropDown"></KanbanDialogSettingsField>
            <KanbanDialogSettingsField Text="Summary" Key="Summary" Type="DialogFieldType.TextArea"></KanbanDialogSettingsField>
        </KanbanDialogSettingsFields>
    </KanbanDialogSettings>
</SfKanban>
```

Server controller must handle:
- `POST /api/Kanban` — GetCards (initial load)
- `POST /api/Kanban/Insert` — Create card
- `POST /api/Kanban/Update` — Update card
- `POST /api/Kanban/Delete` — Delete card

## Dialog Events

| Event | Description |
|-------|-------------|
| `DialogOpen` | Fires before the dialog opens; set `args.Cancel = true` to prevent |
| `DialogClose` | Fires when the dialog closes |
