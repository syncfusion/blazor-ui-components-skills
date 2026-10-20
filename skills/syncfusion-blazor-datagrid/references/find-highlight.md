# Find and Highlight — Syncfusion Blazor DataGrid

Find and Highlight locates and highlights matching text inside visible DataGrid cells while keeping all rows on screen. Unlike the toolbar `Search` filter, it does not hide non-matching rows, so context and comparisons are preserved.

The feature is enabled by adding the `Find` item to the built-in toolbar, and its behavior is configured through `GridFindSettings`. It works independently of filtering, sorting, and other data operations.

**Key capabilities:**

| Capability | Details |
|---|---|
| Real-time highlighting | Matches highlight as text is typed in the find input — no Enter required. |
| Match navigation | Navigate via **Previous** / **Next** toolbar icons, or `Shift+Enter` / `Enter` shortcuts. |
| Match counter | Toolbar shows `current of total` while typing and during navigation. |
| Case-sensitive matching | Configure via `GridFindSettings.CaseSensitive`. |
| Column-level opt-out | Set `GridColumn.AllowSearchHighlight="false"` to exclude a column from scanning, highlighting, and the match count. |
| Custom highlight style | Set `GridFindSettings.HighlightCssClass` to change the `<mark>` element appearance. |
| Programmatic control | `FindHighlightAsync`, `ResetFindAsync`, `FindNextHighlightAsync`, `FindPreviousHighlightAsync`, `FindGoToAsync`. |
| Paging support | Highlighting is automatically reapplied on page navigation. |

> - Cells rendered through a `ColumnTemplate`, `EditTemplate`, or command column are excluded from scanning, highlighting, and match counting.
> - Text in an active Add/Edit editor, the toolbar `Search` input, or any in-progress editor is not searched or highlighted. Only committed cell values are matched.

---

## Table of Contents

- [Enable Find and Highlight Using Toolbar](#enable-find-and-highlight-using-toolbar)
- [Configure Find and Highlight with GridFindSettings](#configure-find-and-highlight-with-gridfindsettings)
- [Exclude a Column from Find and Highlight](#exclude-a-column-from-find-and-highlight)
- [Programmatic API Methods](#programmatic-api-methods)
- [Keyboard Shortcuts](#keyboard-shortcuts)

---

## Enable Find and Highlight Using Toolbar

Add the `"Find"` string to the built-in `Toolbar` list. This renders the find input, the **Previous** and **Next** icons, and the match counter. No additional configuration is required for basic usage:

```razor
@using Syncfusion.Blazor.Grids

<SfGrid @ref="Grid" DataSource="@GridData" AllowSorting="true" Height="500"
        AllowTextWrap="true" AllowReordering="true"
        Toolbar="@(new List<string>() { "Find" })">
    <GridColumns>
        <GridColumn Field="@nameof(Ticket.TicketId)" HeaderText="Ticket ID" Width="130" IsPrimaryKey="true"></GridColumn>
        <GridColumn Field="@nameof(Ticket.Subject)" HeaderText="Subject" Width="250"></GridColumn>
        <GridColumn Field="@nameof(Ticket.CustomerName)" HeaderText="Customer Name" Width="180"></GridColumn>
        <GridColumn Field="@nameof(Ticket.Email)" HeaderText="Email" Width="220"></GridColumn>
        <GridColumn Field="@nameof(Ticket.Priority)" HeaderText="Priority" Width="120"></GridColumn>
        <GridColumn Field="@nameof(Ticket.Status)" HeaderText="Status" Width="120"></GridColumn>
        <GridColumn Field="@nameof(Ticket.Category)" HeaderText="Category" Width="130"></GridColumn>
        <GridColumn Field="@nameof(Ticket.AssignedTo)" HeaderText="Assigned To" Width="180"></GridColumn>
    </GridColumns>
</SfGrid>

@code {
    private SfGrid<Ticket> Grid { get; set; }
    public List<Ticket> GridData { get; set; } = Ticket.GetAllRecords().ToList();

    public class Ticket
    {
        public string? TicketId { get; set; }
        public string? CustomerName { get; set; }
        public string? Email { get; set; }
        public string? Subject { get; set; }
        public string? Description { get; set; }
        public string? Priority { get; set; }
        public string? Status { get; set; }
        public string? Category { get; set; }
        public string? AssignedTo { get; set; }

        public static IReadOnlyList<Ticket> GetAllRecords()
        {
            var list = new List<Ticket>();
            for (int i = 1; i <= 50; i++)
            {
                list.Add(new Ticket
                {
                    TicketId = $"TKT-{10000 + i}",
                    CustomerName = $"Customer {i}",
                    Email = $"user{i}@example.com",
                    Subject = i % 2 == 0 ? "Payment not processed" : "Login failed after password reset",
                    Description = "Sample description for find-and-highlight testing.",
                    Priority = i % 3 == 0 ? "High" : i % 3 == 1 ? "Medium" : "Low",
                    Status = i % 4 == 0 ? "Resolved" : "Open",
                    Category = i % 2 == 0 ? "Billing" : "Authentication",
                    AssignedTo = i % 2 == 0 ? "Sarah Johnson" : "David Clark"
                });
            }
            return list;
        }
    }
}
```

---

## Configure Find and Highlight with GridFindSettings

Use the `GridFindSettings` component inside `SfGrid` to customize behavior and appearance:

| Property | Type | Default | Description |
|---|---|---|---|
| `PlaceholderText` | `string` | `"Find"` | Placeholder shown in the find input. |
| `HighlightCssClass` | `string` | `"e-find-highlight"` | CSS class applied to the `<mark>` element that highlights matching text. Set a custom class to change the highlight appearance. |
| `CaseSensitive` | `bool` | `false` | When `true`, only text with matching letter casing is highlighted. When `false`, casing is ignored. |

**Example** — case-sensitive matching, custom placeholder, custom highlight style:

```razor
@using Syncfusion.Blazor.Grids

<SfGrid @ref="Grid" DataSource="@GridData" Height="500" AllowTextWrap="true"
        Toolbar="@(new List<string>() { "Find" })">
    <GridFindSettings PlaceholderText="Search in grid..."
                      CaseSensitive="true"
                      HighlightCssClass="custom-highlight"></GridFindSettings>
    <GridColumns>
        <GridColumn Field="@nameof(Ticket.TicketId)" HeaderText="Ticket ID" Width="130" IsPrimaryKey="true"></GridColumn>
        <GridColumn Field="@nameof(Ticket.Subject)" HeaderText="Subject" Width="250"></GridColumn>
        <GridColumn Field="@nameof(Ticket.CustomerName)" HeaderText="Customer Name" Width="180"></GridColumn>
        <GridColumn Field="@nameof(Ticket.Email)" HeaderText="Email" Width="220"></GridColumn>
        <GridColumn Field="@nameof(Ticket.Priority)" HeaderText="Priority" Width="120"></GridColumn>
        <GridColumn Field="@nameof(Ticket.Status)" HeaderText="Status" Width="120"></GridColumn>
        <GridColumn Field="@nameof(Ticket.Category)" HeaderText="Category" Width="130"></GridColumn>
        <GridColumn Field="@nameof(Ticket.AssignedTo)" HeaderText="Assigned To" Width="180"></GridColumn>
    </GridColumns>
</SfGrid>

<style>
    .custom-highlight {
        background-color: #ff9800;
        color: #fff;
        border-radius: 2px;
        padding: 0 2px;
    }
</style>
```

---

## Exclude a Column from Find and Highlight

By default, all columns are scanned. To exclude a column, set `GridColumn.AllowSearchHighlight="false"`. This is useful for ID columns, internal codes, or sensitive data.

For an excluded column:
- Cell content is not scanned for matches.
- No highlighting is applied.
- Cells are excluded from the total match count.
- **Previous** / **Next** navigation skips the column entirely.

```razor
@using Syncfusion.Blazor.Grids

<SfGrid @ref="Grid" DataSource="@GridData" Height="500" AllowTextWrap="true"
        Toolbar="@(new List<string>() { "Find" })">
    <GridColumns>
        <GridColumn Field="@nameof(Ticket.TicketId)" HeaderText="Ticket ID" Width="130" IsPrimaryKey="true"></GridColumn>
        <GridColumn Field="@nameof(Ticket.Subject)" HeaderText="Subject" Width="250"></GridColumn>
        <GridColumn Field="@nameof(Ticket.CustomerName)" HeaderText="Customer Name" AllowSearchHighlight="false" Width="180"></GridColumn>
        <GridColumn Field="@nameof(Ticket.Priority)" HeaderText="Priority" AllowSearchHighlight="false" Width="120"></GridColumn>
        <GridColumn Field="@nameof(Ticket.Status)" HeaderText="Status" Width="120"></GridColumn>
        <GridColumn Field="@nameof(Ticket.Category)" HeaderText="Category" Width="130"></GridColumn>
        <GridColumn Field="@nameof(Ticket.AssignedTo)" HeaderText="Assigned To" Width="180"></GridColumn>
    </GridColumns>
</SfGrid>
```

> In the example above, **CustomerName** and **Priority** are excluded — text entered in those columns will not produce a match or highlight.

---

## Programmatic API Methods

Drive Find and Highlight entirely from code-behind — build custom find panels, integrate keyboard shortcuts, or trigger highlighting from business logic.

| Method | Description |
|---|---|
| `FindHighlightAsync(string text)` | Highlights all matching text in the DataGrid based on the specified search text. Pass an empty string to remove existing highlights. |
| `ResetFindAsync()` | Removes all highlights and resets the match counter to `0`. |
| `FindNextHighlightAsync()` | Moves the active selection to the next match. After the last match, navigation continues from the first match. |
| `FindPreviousHighlightAsync()` | Moves the active selection to the previous match. Before the first match, navigation continues from the last match. |
| `FindGoToAsync(int index)` | Navigates directly to a 1-based match index. Values below the valid range select the first match; values above the valid range select the last match (for example, `FindGoToAsync(0)` and `FindGoToAsync(-1)` select the first match; `FindGoToAsync(9999)` selects the last match when the count is lower). |

**Example** — drive the feature with external inputs and buttons:

```razor
@using Syncfusion.Blazor.Buttons
@using Syncfusion.Blazor.Grids
@using Syncfusion.Blazor.Inputs

<label>Find:</label>
<SfTextBox FloatLabelType="FloatLabelType.Always" Width="250px" @bind-Value="@searchText"
           CssClass="e-outline e-small e-searchbox" Input="@OnValueChange"></SfTextBox>
<SfButton Content="Previous" OnClick="FindPreviousMatch"></SfButton>
<SfButton Content="Next" OnClick="FindNextMatch"></SfButton>
<br /><br />
<label>Go to match: </label>
<SfNumericTextBox TValue="int" FloatLabelType="FloatLabelType.Always" @bind-Value="@matchNumber"
                  Width="250px" Min="1" Max="100"
                  CssClass="e-outline e-small e-searchbox"></SfNumericTextBox>
<SfButton Content="Go To" OnClick="GoToMatch"></SfButton>
<br />

<SfGrid @ref="Grid" DataSource="@GridData" AllowSorting="true" Height="500"
        AllowTextWrap="true" AllowReordering="true">
    <GridColumns>
        <GridColumn Field="@nameof(Ticket.TicketId)" HeaderText="Ticket ID" Width="130" IsPrimaryKey="true"></GridColumn>
        <GridColumn Field="@nameof(Ticket.Subject)" HeaderText="Subject" Width="250"></GridColumn>
        <GridColumn Field="@nameof(Ticket.CustomerName)" HeaderText="Customer Name" Width="180"></GridColumn>
        <GridColumn Field="@nameof(Ticket.Email)" HeaderText="Email" Width="220"></GridColumn>
        <GridColumn Field="@nameof(Ticket.Priority)" HeaderText="Priority" Width="120"></GridColumn>
        <GridColumn Field="@nameof(Ticket.Status)" HeaderText="Status" Width="120"></GridColumn>
        <GridColumn Field="@nameof(Ticket.Category)" HeaderText="Category" Width="130"></GridColumn>
        <GridColumn Field="@nameof(Ticket.AssignedTo)" HeaderText="Assigned To" Width="180"></GridColumn>
    </GridColumns>
</SfGrid>

@code {
    private SfGrid<Ticket> Grid { get; set; }
    private string searchText { get; set; }
    private int matchNumber { get; set; }
    public List<Ticket> GridData { get; set; } = Ticket.GetAllRecords().ToList();

    private async Task OnValueChange(InputEventArgs args)
    {
        if (Grid != null)
        {
            await Grid.FindHighlightAsync(args.Value);
        }
    }

    private async Task FindPreviousMatch() => await Grid.FindPreviousHighlightAsync();
    private async Task FindNextMatch() => await Grid.FindNextHighlightAsync();
    private async Task GoToMatch() => await Grid.FindGoToAsync(matchNumber);

    public class Ticket
    {
        public string? TicketId { get; set; }
        public string? CustomerName { get; set; }
        public string? Email { get; set; }
        public string? Subject { get; set; }
        public string? Description { get; set; }
        public string? Priority { get; set; }
        public string? Status { get; set; }
        public string? Category { get; set; }
        public string? AssignedTo { get; set; }

        public static IReadOnlyList<Ticket> GetAllRecords()
        {
            var list = new List<Ticket>();
            for (int i = 1; i <= 50; i++)
            {
                list.Add(new Ticket
                {
                    TicketId = $"TKT-{10000 + i}",
                    CustomerName = $"Customer {i}",
                    Email = $"user{i}@example.com",
                    Subject = i % 2 == 0 ? "Payment not processed" : "Login failed after password reset",
                    Description = "Sample description for find-and-highlight testing.",
                    Priority = i % 3 == 0 ? "High" : i % 3 == 1 ? "Medium" : "Low",
                    Status = i % 4 == 0 ? "Resolved" : "Open",
                    Category = i % 2 == 0 ? "Billing" : "Authentication",
                    AssignedTo = i % 2 == 0 ? "Sarah Johnson" : "David Clark"
                });
            }
            return list;
        }
    }
}
```

---

## Keyboard Shortcuts

The following shortcuts work when the find input is focused:

| Key | Action |
|---|---|
| `Enter` | Move to the next match. |
| `Shift+Enter` | Move to the previous match. |
| `Escape` | Close the find toolbar and clear all highlights. |

These match standard find behavior in browsers and desktop applications.

---

## See Also

- [searching.md](searching.md) — toolbar `Search` filter (hides non-matching rows).
- [toolbar.md](toolbar.md) — built-in toolbar items including `"Find"`.
- [columns.md](columns.md) — `GridColumn` properties including `AllowSearchHighlight`.
