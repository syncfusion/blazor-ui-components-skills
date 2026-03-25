---
name: syncfusion-blazor-treegrid
description: Implements the Syncfusion Blazor TreeGrid (SfTreeGrid) for hierarchical or self‑referential data with expandable nodes and nested row structures. Use this skill when building tree‑view workflows in Blazor Server, WebAssembly, Web App, or MAUI applications. Supports GraphQL binding, templates, sorting, filtering, editing, paging, aggregates, search, exporting, virtualization, clipboard actions, and state management for rich hierarchical data experiences.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
---

# Implementing Syncfusion Blazor TreeGrid

The Syncfusion Blazor TreeGrid (`SfTreeGrid<TValue>`) renders hierarchical data in a tabular format with expand/collapse rows. It supports self-referential flat data (IdMapping/ParentIdMapping) and nested child collections, combined with a full suite of grid features including editing, filtering, sorting, virtualization, and export.

## When to Use This Skill

- User needs a **hierarchical/tree-structured data table** in a Blazor app
- User mentions **SfTreeGrid**, **TreeGrid**, **IdMapping**, **ParentIdMapping**
- User needs **parent-child rows** with expand/collapse in a grid
- User needs **CRUD operations** on tree-structured data
- User needs to **export**, **filter**, **sort**, or **page** hierarchical data
- User needs to bind TreeGrid to a **GraphQL API** using `GraphQLAdaptor`
- User is working with **organizational charts**, **task hierarchies**, **bill of materials**, **file explorer-style** data in grid form
- User migrating from flat DataGrid to hierarchical TreeGrid
- User setting up TreeGrid in a **Blazor Web App**, **WASM**, **Server**, or **MAUI** project

**Trigger keywords:** TreeGrid, Tree Grid, SfTreeGrid, hierarchical grid, parent-child grid, IdMapping, ParentIdMapping, tree data table, nested grid rows, Syncfusion Blazor tree, GraphQL TreeGrid, GraphQLAdaptor TreeGrid

---

## 🔒 Mandatory Key Rules

These rules govern how this skill MUST behave. They are mandatory and must be strictly followed.

### 1. Purpose and Responsibility

Your responsibility is to interpret any natural‑language user request and provide **accurate**, **complete**, and **validated** information about all supported aspects of the **Syncfusion Blazor TreeGrid**, including:

- Public APIs
- Properties
- Events
- Features
- Behaviors

### 2. Accuracy and API Compliance

The Skill MUST:

- Use **only officially supported** Syncfusion TreeGrid features.
- **NEVER** invent APIs, methods, properties, events, behaviors, or future features.
- **NEVER** provide unsupported or hypothetical code samples.
- ALWAYS follow official Syncfusion component design patterns.

If a feature is **not supported**: clearly state the limitation and suggest a supported alternative when possible.

### 3. Interpreting Natural-Language Requests

When user requests are incomplete:

- Infer reasonable assumptions using official TreeGrid best practices.
- Fill missing gaps with accurate and relevant information.
- Request clarification **only** when essential.

### 4. Handling Unsupported User Requests

If the user asks for an unsupported capability:

- Explicitly state that it is not supported
- Suggest official alternatives or valid workarounds
- NEVER simulate or fabricate impossible functionality

### 5. Design Pattern Enforcement

The Skill MUST follow Syncfusion official patterns, including:

- Correct component structure (`SfTreeGrid`, `TreeGridColumn`, `TreeGridEditSettings`, `TreeGridEvents`, etc.)
- Proper async event and API usage
- Valid configuration properties and enums
- Supported data‑binding approaches (self-referential, hierarchical, remote, custom adaptor, GraphQL)
- Real event patterns and names

### 6. Quality, Completeness & Reliability

The Skill MUST:

- Use only validated and real TreeGrid capabilities
- Provide actionable and implementation‑ready guidance
- Ensure clarity so users can follow without guesswork
- Maintain clean, readable, and professional formatting

### 7. No-Hallucination Safeguard

The Skill MUST NOT:

- Invent non‑existent APIs or behavior
- Suggest unsupported modes, features, or configuration
- Provide incorrect, misleading, or unverifiable code
- Describe undocumented internal behavior

If unsure: ask for clarification **OR** clearly state the limitation.

### 8. Event Name Verification Requirement

**CRITICAL:** Event names MUST be verified against `references/events.md` BEFORE providing code examples. All tree grid events MUST be defined inside the `<TreeGridEvents>` component

**Common Mistakes to Avoid:**

- ❌ `OnSortChange` — Does NOT exist. Use `Sorting`, `Sorted` instead
- ❌ `OnFilterChange` — Does NOT exist. Use `Filtering`, `Filtered` instead


**Rule:** ALWAYS cross-reference `references/events.md` for:

- Exact event names (case-sensitive)
- Event argument types (see **Rule 11** for namespace resolution)
- Whether events are cancelable (✅ or ❌)
- When they fire (before/after operation)

### 9. Read Reference Files Before Writing Code

Before generating **any** code, identify every feature in the request and **read the reference file for each one**. The snippets in this file are abbreviated — they omit critical rules and known pitfalls. Reference files are the authoritative source.

> ⚠️ **Read ALL reference files that apply — not just the one most obvious from the query.**

### 10. Namespace and Type Resolution Rules

**Rule 1:** Always use full namespace qualification to avoid CS0104 "ambiguous reference" errors. Types exist in both `Syncfusion.Blazor.TreeGrid` and `Syncfusion.Blazor.Grids` namespaces.

**Rule 2:** Always include `@using Syncfusion.Blazor.Grids;` in `_Imports.razor` or component to prevent CS0103 "missing reference" errors for:
- Event argument types (`ActionEventArgs`, `RowDataBoundEventArgs`, `BeforeCopyPasteEventArgs`, etc.)
- The `Action` enum (`Action.Sorting`, `Action.Save`, etc.)
- Component classes (`TreeGridPageSettings`, `TreeGridFilterSettings`, `TreeGridSelectionSettings`, etc.)
- Event handler support classes

**Rule 3:** The following event arg types belong to `Syncfusion.Blazor.TreeGrid` — add `@using Syncfusion.Blazor.TreeGrid;` (NOT `Syncfusion.Blazor.Grids`):
- `RowExpandingEventArgs<TValue>`, `RowExpandedEventArgs<TValue>`
- `RowCollapsingEventArgs<TValue>`, `RowCollapsedEventArgs<TValue>`
- `CheckBoxChangeEventArgs<TValue>`

**Type Reference:**

| Type | Namespace | Example |
|---|---|---|
| **FilterType** | TreeGrid | `Syncfusion.Blazor.TreeGrid.FilterType.Excel` |
| **EditMode** | TreeGrid | `Syncfusion.Blazor.TreeGrid.EditMode.Row` |
| **CopyHierarchyType** | TreeGrid | `Syncfusion.Blazor.TreeGrid.CopyHierarchyType.Both` |
| **Action** | Grids | `Syncfusion.Blazor.Grids.Action.Save` |
| **TextAlign** | Grids | `Syncfusion.Blazor.Grids.TextAlign.Right` |
| **ColumnType** | Grids | `Syncfusion.Blazor.Grids.ColumnType.Date` |
| **EditType** | Grids | `Syncfusion.Blazor.Grids.EditType.DatePickerEdit` |
| **SelectionMode** | Grids | `Syncfusion.Blazor.Grids.SelectionMode.Row` |
| **SelectionType** | Grids | `Syncfusion.Blazor.Grids.SelectionType.Multiple` |
| **ClipMode** | Grids | `Syncfusion.Blazor.Grids.ClipMode.Ellipsis` |
| **GridLine** | Grids | `Syncfusion.Blazor.Grids.GridLine.Horizontal` |
| **AggregateType** | Grids | `Syncfusion.Blazor.Grids.AggregateType.Sum` |
| **CellSelectionMode** | Grids | `Syncfusion.Blazor.Grids.CellSelectionMode.Box` |
---

## Component Overview

```razor
<SfTreeGrid DataSource="@TreeData"
            IdMapping="TaskId"
            ParentIdMapping="ParentId"
            TreeColumnIndex="1"
            AllowSorting="true"
            AllowFiltering="true"
            AllowPaging="true">
    <TreeGridColumns>
        <TreeGridColumn Field="TaskId" HeaderText="ID" Width="80" IsPrimaryKey="true" />
        <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="200" />
        <TreeGridColumn Field="Duration" HeaderText="Duration" Width="100" />
        <TreeGridColumn Field="Progress" HeaderText="Progress" Width="100" />
    </TreeGridColumns>
</SfTreeGrid>
```

**Key concepts:**
- `IdMapping` — property name that uniquely identifies each row
- `ParentIdMapping` — property name pointing to the parent row's ID (null/0 = root)
- `TreeColumnIndex` — column index that shows the expand/collapse icon (0-based)

---

## Navigation Guide

### Setup & Getting Started
- **Read:** [references/getting-started.md](references/getting-started.md)
  - NuGet install, `_Imports.razor`, `Program.cs`, theme/script setup, basic grid, `IdMapping`/`ParentIdMapping`/`TreeColumnIndex`, expand-collapse all, `OnActionFailure`

### Data Binding
- **Read:** [references/data-binding.md](references/data-binding.md)
  - Self-referential flat data, hierarchical nested children, remote URL/OData/Web API, `SfDataManager`, load on demand
- **Read:** [references/custom-binding.md](references/custom-binding.md)
  - `DataAdaptor` override, `Read`/`ReadAsync`, Insert/Update/Delete, paging & sorting support, self-referential only
- **Read:** [references/graphql.md](references/graphql.md)
  - Hot Chocolate server, `GraphQLAdaptor`, `GraphQLAdaptorOptions`, `DataManagerRequestInput`, CRUD mutations, cascading delete, CORS

### Columns & Cells
- **Read:** [references/columns.md](references/columns.md)
  - `ColumnType`, `Format`, `TextAlign`, frozen, reorder, resize, chooser, stacked headers, column menu, foreign key
- **Read:** [references/cell.md](references/cell.md)
  - `QueryCellInfo`, `CustomAttributes`, `ClipMode`, `GridLines`, tooltips

### Sorting, Filtering & Searching
- **Read:** [references/sorting.md](references/sorting.md)
  - `AllowSorting`, multi-sort, `SortComparer`, `SortColumnAsync`, `ClearSortingAsync`, hierarchy preserved
- **Read:** [references/filtering.md](references/filtering.md)
  - `AllowFiltering`, `FilterType` (FilterBar/Menu/Excel/CheckBox), hierarchy modes, operators, `FilterByColumnAsync`. ⚠️ See [filtering.md](references/filtering.md#filter-bar-default) for `FilterTemplate` rules
- **Read:** [references/searching.md](references/searching.md)
  - Toolbar Search, `SearchAsync`, `GridSearchSettings`, hierarchy modes, highlight/clear

### Editing
- **Read:** [references/editing.md](references/editing.md)
  - Edit modes (Cell/Row/Dialog/Batch), edit types, command column, validation, template editing, EF Core. ⚠️ See [editing.md](references/editing.md#column-validation) for `ValidationRules`. Always add `@using Syncfusion.Blazor.Grids` when using `<TreeGridEvents>`

### Rows & Selection
- **Read:** [references/rows.md](references/rows.md)
  - `RowDataBound`, row/detail template, drag-and-drop, row height, row spanning
- **Read:** [references/selection.md](references/selection.md)
  - Row/cell/both modes, checkbox column, toggle, drag select, programmatic selection, touch

### Paging & Virtualization
- **Read:** [references/paging.md](references/paging.md)
  - `AllowPaging`, `TreeGridPageSettings`, `PageSize`, pager position/template, programmatic navigation
- **Read:** [references/virtualization.md](references/virtualization.md)
  - `EnableVirtualization`, `EnableColumnVirtualization`, vs. paging, limitations

### Toolbar, Menus & Templates
- **Read:** [references/toolbar.md](references/toolbar.md)
  - Built-in & custom items, `EnableToolbarItemsAsync`, `OnToolbarClick`
- **Read:** [references/context-menu.md](references/context-menu.md)
  - `ContextMenuItems`, custom items, `ContextMenuItemClicked`, conditional show/hide
- **Read:** [references/templates.md](references/templates.md)
  - Column/header/row/detail/toolbar templates, loading indicator

### Exporting & Scrolling
- **Read:** [references/exporting.md](references/exporting.md)
  - `ExportToExcelAsync`, `ExportToPdfAsync`, `PrintAsync`, custom data/headers, themes
- **Read:** [references/scrolling.md](references/scrolling.md)
  - Frozen rows/columns (`FrozenRows`, `IsFrozen`, `FreezeDirection`), `AllowFreezeLineMoving`, `EnableStickyHeader`, `ScrollIntoViewAsync`

### Advanced Features
- **Read:** [references/aggregates.md](references/aggregates.md)
  - `TreeGridAggregateColumn` (`Field`+`Type` required), `AggregateType`, `FooterTemplate`, `ShowChildSummary`, custom LINQ, page-scope limitation
- **Read:** [references/clipboard.md](references/clipboard.md)
  - Copy (Ctrl+C/Ctrl+Shift+H), `CopyAsync`, paste (Box+Batch), `CopyHierarchyMode`, `BeforeCopyPaste`
- **Read:** [references/autofill.md](references/autofill.md)
  - `EnableAutoFill` + Cell/Box selection + Batch edit, limitations
- **Read:** [references/adaptive-layout.md](references/adaptive-layout.md)
  - `EnableAdaptiveUI`, full-screen dialogs, vertical row mode
- **Read:** [references/state-management.md](references/state-management.md)
  - `EnablePersistence`, persisted properties, manual state methods

### Events
- **Read:** [references/events.md](references/events.md)
  - `TreeGridEvents<TValue>`, `OnActionBegin`/`OnActionComplete`, `RowDataBound`, `QueryCellInfo`, cancel actions

---

## Quick Start Example

**1. Install NuGet:** `Syncfusion.Blazor.TreeGrid`

**2. Register in `_Imports.razor`:**
```razor
@using Syncfusion.Blazor
@using Syncfusion.Blazor.TreeGrid
```

**3. Add CSS in `wwwroot/index.html` or `App.razor`:**
```html
<link href="_content/Syncfusion.Blazor.Themes/material.css" rel="stylesheet" />
```

**4. Add to `Program.cs`:**
```csharp
builder.Services.AddSyncfusionBlazor();
```

**5. Minimal component:**
```razor
@using Syncfusion.Blazor.TreeGrid

<SfTreeGrid DataSource="@TreeData" IdMapping="Id" ParentIdMapping="ParentId" TreeColumnIndex="1">
    <TreeGridColumns>
        <TreeGridColumn Field="Id" HeaderText="ID" Width="80" IsPrimaryKey="true" />
        <TreeGridColumn Field="Name" HeaderText="Name" Width="200" />
        <TreeGridColumn Field="Status" HeaderText="Status" Width="120" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    public class TaskItem
    {
        public int Id { get; set; }
        public int? ParentId { get; set; }
        public string Name { get; set; }
        public string Status { get; set; }
    }

    private List<TaskItem> TreeData = new()
    {
        new TaskItem { Id = 1, ParentId = null, Name = "Project Alpha", Status = "In Progress" },
        new TaskItem { Id = 2, ParentId = 1,    Name = "Design Phase",  Status = "Done" },
        new TaskItem { Id = 3, ParentId = 1,    Name = "Development",   Status = "In Progress" },
        new TaskItem { Id = 4, ParentId = 3,    Name = "Backend API",   Status = "Pending" },
    };
}
```

---

## Common Patterns

### Full-Featured TreeGrid (Sorting + Filtering + Paging + Editing)
```razor
<SfTreeGrid @ref="TreeGridRef"
            DataSource="@TreeData"
            IdMapping="Id"
            ParentIdMapping="ParentId"
            TreeColumnIndex="1"
            AllowSorting="true"
            AllowFiltering="true"
            AllowPaging="true"
            Toolbar="@(new List<string> { "Add", "Edit", "Delete", "Update", "Cancel" })">
    <TreeGridEditSettings AllowAdding="true"
                          AllowEditing="true"
                          AllowDeleting="true"
                          Mode="Syncfusion.Blazor.TreeGrid.EditMode.Row" />
    <TreeGridPageSettings PageSize="10" />
    <TreeGridColumns>
        <TreeGridColumn Field="Id" IsPrimaryKey="true" Visible="false" />
        <TreeGridColumn Field="Name" HeaderText="Task" Width="220" />
        <TreeGridColumn Field="Duration" HeaderText="Days" Width="100" />
        <TreeGridColumn Field="Progress" HeaderText="%" Width="100" />
    </TreeGridColumns>
</SfTreeGrid>

@code {
    private SfTreeGrid<TaskItem> TreeGridRef;
    // ... data and model
}
```

### Excel & PDF Export
```razor
<SfTreeGrid @ref="TreeGridRef" AllowExcelExport="true" AllowPdfExport="true"
            Toolbar="@(new List<string> { "ExcelExport", "PdfExport" })">
    <TreeGridEvents TValue="TaskItem" OnToolbarClick="ToolbarClickHandler" />
    ...
</SfTreeGrid>

@code {
    private SfTreeGrid<TaskItem> TreeGridRef;

    private async Task ToolbarClickHandler(Syncfusion.Blazor.Navigations.ClickEventArgs args)
    {
        if (args.Item.Id.Contains("excelexport"))
            await TreeGridRef.ExportToExcelAsync();
        else if (args.Item.Id.Contains("pdfexport"))
            await TreeGridRef.ExportToPdfAsync();
    }
}
```