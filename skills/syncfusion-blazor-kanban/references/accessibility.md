# Accessibility in Blazor Kanban Component

The Blazor Kanban component follows the WAI-ARIA specification and WCAG 2.2 standards, providing full keyboard navigation and screen reader support.

## Table of Contents

- [WCAG Compliance](#wcag-compliance)
- [WAI-ARIA Attributes](#wai-aria-attributes)
- [Keyboard Navigation](#keyboard-navigation)
- [Enabling Keyboard Interaction](#enabling-keyboard-interaction)

## WCAG Compliance

The Kanban component conforms to **WCAG 2.2 AA** level accessibility standards.

| Criteria | Description |
|----------|-------------|
| **1.1.1 Non-text content** | All controls have accessible labels |
| **1.3.1 Info and relationships** | Semantic HTML structure and ARIA roles |
| **1.3.2 Meaningful sequence** | Logical reading/focus order |
| **2.1.1 Keyboard** | Full keyboard navigation support |
| **2.1.2 No keyboard trap** | Focus can always move away |
| **3.2.2 On input** | No unexpected context changes |
| **4.1.2 Name, Role, Value** | All UI components have proper ARIA attributes |

## WAI-ARIA Attributes

| Element | Attribute | Description |
|---------|-----------|-------------|
| Kanban element | `role="main"` | Root role for the board |
| Column header | `role="columnheader"` | Identifies column header cells |
| Column header | `aria-label` | Accessible label with column name |
| Column toggle | `role="button"` | Toggle collapse/expand |
| Column toggle | `aria-expanded` | State of column expansion |
| Card | `role="listitem"` | Each card as a list item |
| Card | `aria-label` | Card header value |
| Card | `aria-selected` | Whether card is selected |
| Card | `aria-grabbed` | Whether card is being dragged |
| Card container | `role="list"` | Cards container |
| Swimlane | `role="rowgroup"` | Swimlane row |
| Swimlane header | `role="row"` | Swimlane header row |
| Swimlane toggle | `aria-expanded` | State of swimlane expansion |
| Dialog | `role="dialog"` | Card editing dialog |
| Dialog | `aria-modal` | Marks dialog as modal |
| Dialog | `aria-labelledby` | Dialog title reference |

## Keyboard Navigation

### Card Navigation

| Key | Description |
|-----|-------------|
| `Home` | Focus the first card in the column |
| `End` | Focus the last card in the column |
| `Arrow Up` | Move focus to the card above |
| `Arrow Down` | Move focus to the card below |
| `Arrow Left` | Move focus to the card in the previous column |
| `Arrow Right` | Move focus to the card in the next column |
| `Enter` | Open the card editing dialog |
| `Ctrl + Enter` | Select/deselect the focused card |
| `Escape` | Deselect all cards / close dialog |
| `Delete` | Delete the focused card |
| `Ctrl + X` | Cut the selected card(s) |
| `Ctrl + C` | Copy the selected card(s) |
| `Ctrl + V` | Paste card(s) into the focused column |
| `Space` | Select/deselect the focused card |

### Column Navigation

| Key | Description |
|-----|-------------|
| `Tab` | Move focus to the next column or interactive element |
| `Shift + Tab` | Move focus to the previous column or element |
| `Ctrl + Arrow Left` | Focus previous column header |
| `Ctrl + Arrow Right` | Focus next column header |

### Swimlane Navigation

| Key | Description |
|-----|-------------|
| `Ctrl + Arrow Up` | Move to the swimlane row above |
| `Ctrl + Arrow Down` | Move to the swimlane row below |
| `Enter` on swimlane | Toggle swimlane expand/collapse |

## Enabling Keyboard Interaction

Keyboard interaction is enabled by default. Disable it with `AllowKeyboard="false"`:

```cshtml
<SfKanban TValue="TasksModel" KeyField="Status" DataSource="Tasks" AllowKeyboard="true">
    <KanbanColumns>
        <KanbanColumn HeaderText="Backlog" KeyField="@(new List<string>() {"Open"})"></KanbanColumn>
        <KanbanColumn HeaderText="In Progress" KeyField="@(new List<string>() {"InProgress"})"></KanbanColumn>
        <KanbanColumn HeaderText="Done" KeyField="@(new List<string>() {"Close"})"></KanbanColumn>
    </KanbanColumns>
    <KanbanCardSettings HeaderField="Id" ContentField="Summary"></KanbanCardSettings>
</SfKanban>
```

> The `AllowKeyboard` property defaults to `true`. Set it to `false` only if keyboard accessibility is explicitly not needed.
