# Blazor UI Components Skills

Skills for Syncfusion Blazor components, designed for use with AI coding assistants.

This repository contains 50+ AI-ready skill guides for working with Syncfusion Blazor components. Each skill includes a `SKILL.md` file that AI coding assistants can read automatically, plus a `references/` subfolder with detailed documentation covering setup, usage patterns, customization, and troubleshooting.

The skills guide Blazor development across all hosting models (Server, WebAssembly, Hybrid, Auto) including data grid, chart, scheduler, rich text editor, gantt chart, and more.

## Quick Start

### Option 1: Using npx (Recommended)

```bash
npx skills add https://github.com/syncfusion/blazor-ui-components-skills
```

This will automatically add the skills to your workspace.

### Option 2: Manual Installation

**1. Clone this repository**
```bash
git clone https://github.com/syncfusion/blazor-ui-components-skills.git
```

**2. Add it to your VS Code workspace**

Open your `.code-workspace` file (or create one) and add this repo as a second root folder:
```json
{
  "folders": [
    { "path": "/path/to/your-blazor-app" },
    { "path": "/path/to/blazor-ui-components-skills" }
  ]
}
```

**3. Start asking questions**

Your AI assistant will automatically detect and apply the relevant skill based on your prompt:
```text
How do I add grouping to the Syncfusion DataGrid in Blazor?
How do I configure the Blazor Scheduler for week view?
How do I apply a dark theme to Syncfusion Blazor components?
```

No configuration required. Skills are loaded automatically from the workspace.

---

## Prerequisites

- An AI coding assistant that supports skills/context files (e.g., Syncfusion Code Studio, GitHub Copilot, Cursor, or similar tools)
- [.NET 8 SDK or later](https://dotnet.microsoft.com/download/dotnet)
- A Syncfusion license key ([free community license available](https://www.syncfusion.com/products/communitylicense))
- Basic knowledge of Blazor (Server, WASM, or Hybrid hosting models)

## How These Skills Work

Each `SKILL.md` file contains a `description` field in its YAML frontmatter. AI coding assistants read this description to decide when to automatically apply a skill during a conversation. When you ask about a specific Syncfusion Component — for example, "How do I add sorting to my Blazor DataGrid?" — the AI assistant detects the match and loads the corresponding skill to guide its response.

You can also reference a skill explicitly by mentioning the component or control by name in your prompt.

### Example Prompts

```text
How do I bind data to the Syncfusion DataGrid in Blazor?
```
→ The AI assistant loads the DataGrid skill and uses its getting-started and data-binding reference docs.

```text
Show me how to implement master-detail in the Blazor TreeGrid.
```
→ The AI assistant loads the TreeGrid skill.

### Using Reference Files

Each `references/` subfolder contains deeper implementation guides. When the AI assistant loads a skill, it can also pull in these files when you ask follow-up questions:

```text
Show me how to export the DataGrid to Excel.
```
→ The AI assistant uses `references/advanced-features.md` from the DataGrid skill for the detailed answer.

## Skill File Structure

Every skill folder follows this layout:

```text
skills/
└── syncfusion-blazor-<component>/
    ├── SKILL.md                  ← Loaded by AI assistant; contains When to Use, Component Overview, and navigation links
    └── references/
        ├── getting-started.md    ← Installation, setup, NuGet packages, basic configuration
        ├── advanced-features.md  ← In-depth feature guides and code samples
        └── ...                   ← Additional reference files per component
```

`SKILL.md` sections:
- **When to Use This Skill** — trigger phrases and scenarios that activate this skill
- **Component Overview** — NuGet package, namespace, key capabilities at a glance
- **Documentation and Navigation Guide** — links to all reference files in the skill

## Repository Structure

```text
README.md
skills/
    syncfusion-blazor-common/
    syncfusion-blazor-themes/
    syncfusion-blazor-license/
    syncfusion-blazor-charts/
    syncfusion-blazor-datagrid/
    syncfusion-blazor-scheduler/
    ... (45+ total skills)
```

## Skill Index

> **Tip:** Start with [Common](skills/syncfusion-blazor-common/SKILL.md) if you are setting up a new project. For all other tasks, find the skill that matches the specific component below.

### Foundation & Setup

- [Common](skills/syncfusion-blazor-common/SKILL.md) — installation, script references, themes, bunit setup and localization & globalization 
- [Themes](skills/syncfusion-blazor-themes/SKILL.md) — Fluent 2, Material 3, Bootstrap 5, Fabric, Tailwind 3 themes, dark mode and lite themes
- [License](skills/syncfusion-blazor-license/SKILL.md) — license key registration and validation

### Grids & Data Management

- [DataGrid](skills/syncfusion-blazor-datagrid/SKILL.md) — sorting, filtering, paging, grouping, editing, export
- [TreeGrid](skills/syncfusion-blazor-treegrid/SKILL.md) — hierarchical data, master-detail, export
- [Data Manager](skills/syncfusion-blazor-datamanager/SKILL.md) — data binding and remote data operations
- [Data Form](skills/syncfusion-blazor-data-form/SKILL.md) — form generation and validation
- [Query Builder](skills/syncfusion-blazor-query-builder/SKILL.md) — visual query building interface

### Scheduling & Calendars

- [Scheduler](skills/syncfusion-blazor-scheduler/SKILL.md) — calendar events, appointments, recurring events

### Editors & Input components

- [Rich Text Editor](skills/syncfusion-blazor-rich-text-editor/SKILL.md) — WYSIWYG text editing with formatting
- [Image Editor](skills/syncfusion-blazor-image-editor/SKILL.md) — image manipulation and editing
- [Block Editor](skills/syncfusion-blazor-blockeditor/SKILL.md) — block-based document editing
- [Dropdowns](skills/syncfusion-blazor-dropdowns/SKILL.md) — dropdown list, combo box, multi-select
- [Dropdown Tree](skills/syncfusion-blazor-dropdown-tree/SKILL.md) — tree-based dropdown selection
- [Smart Paste](skills/syncfusion-blazor-smart-paste/SKILL.md) — intelligent clipboard paste handling
- [Smart TextArea](skills/syncfusion-blazor-smart-textarea/SKILL.md) — enhanced textarea with autocomplete

### Data Visualization & Charts

- [Charts (Cartesian)](skills/syncfusion-blazor-charts/SKILL.md) — line, area, bar, column, scatter, bubble charts
- [Accumulation Chart](skills/syncfusion-blazor-accumulation-chart/SKILL.md) — pie, doughnut, funnel charts
- [Maps](skills/syncfusion-blazor-maps/SKILL.md) — geographic mapping and location visualization
- [Barcode](skills/syncfusion-blazor-barcode/SKILL.md) — QR codes, barcodes, and data matrix generation


### Navigation & Layout

- [Tabs](skills/syncfusion-blazor-tabs/SKILL.md) — tabbed navigation and content organization
- [Ribbon](skills/syncfusion-blazor-ribbon/SKILL.md) — Office-style ribbon interface
- [File Manager](skills/syncfusion-blazor-file-manager/SKILL.md) — file browser and management
- [Popups](skills/syncfusion-blazor-popups/SKILL.md) — dialogs, tooltips, and popup components

### Gantt & Project Management

- [Gantt Chart](skills/syncfusion-blazor-gantt-chart/SKILL.md) — project timeline, dependencies, and resource management

### Diagram & Visualization

- [Diagram](skills/syncfusion-blazor-diagram/SKILL.md) — flowcharts, organizational charts, and diagramming

### AI Integration

- [AI AssistView](skills/syncfusion-blazor-ai-assistview/SKILL.md) — AI-powered assistant interface
- [Chat UI](skills/syncfusion-blazor-chat-ui/SKILL.md) — conversational UI and chat components

### Security

- [Security](skills/syncfusion-blazor-security/SKILL.md) — CSP configuration and browser-level protections for Blazor applications.

---

## Hosting Models Support

These skills work seamlessly across all Blazor hosting models:

- **Blazor Server** — Server-side rendering with SignalR communication
- **Blazor WebAssembly** — Client-side .NET runtime in the browser
- **Blazor Hybrid** — Desktop applications using MAUI or WPF
- **Blazor Auto (Web)** — Combined Server and WASM with auto-switching

---

## Getting Help

- Check the [Syncfusion Blazor documentation](https://blazor.syncfusion.com/documentation/introduction)
- Review skill reference files in the `references/` folders
- Visit [Syncfusion community forums](https://www.syncfusion.com/forums/blazor-components)

---

## License

These skills are provided as educational resources for working with Syncfusion Blazor components. Syncfusion components require a valid license key. To acquire a license, you can quote a purchase at https://www.syncfusion.com/sales/pricing.