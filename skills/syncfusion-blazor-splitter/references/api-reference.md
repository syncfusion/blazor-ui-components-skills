# Blazor Splitter API Reference

## Overview

The SfSplitter is a layout user interface (UI) component that splits the layout into multiple panes with resizable and collapsible support.

---

## Table of Contents

- [SfSplitter Properties](#sf splitter-properties)
- [SfSplitter Methods](#sf splitter-methods)
- [SplitterPane Properties](#splitterpane-properties)
- [SplitterEvents Properties](#splitterevents-properties)
- [Orientation Enum](#orientation-enum)
- [Event Args Classes](#event-args-classes)
- [Coverage Summary](#coverage-summary)
- [Examples for Not Previously Covered APIs](#examples-for-not-previously-covered-apis)
  - [Content Property Example](#content-property-example)
  - [EnableRtl Example](#enablertl-example)
  - [Enabled Example](#enabled-example)
  - [EnableReversePanes Example](#enablereversepanes-example)
  - [HtmlAttributes Example](#htmlattributes-example)
  - [ToggleAsync Example](#toggleasync-example)
  - [SplitterPane CssClass Example](#splitterpane-cssclass-example)
  - [Destroyed Event Example](#destroyed-event-example)
- [Usage Examples](#usage-examples)

---

## SfSplitter Properties

| Property | Type | Default | Accepted Values | Description | Reference |
|----------|------|---------|-----------------|-------------|-----------|
| `Height` | string | "100%" | px, % | Sets the height of the Splitter component | [view](split-panes.md#horizontal-layout) |
| `Width` | string | "100%" | px, % | Sets the width of the Splitter component | [view](split-panes.md#horizontal-layout) |
| `Orientation` | Orientation | Horizontal | Horizontal, Vertical | Specifies the orientation of the splitter panes | [view](split-panes.md#vertical-layout) |
| `SeparatorSize` | double | 1 | numeric value | Specifies the separator size between panes | [view](split-panes.md#separator-customization) |
| `CssClass` | string | empty | CSS class names | Customizes the appearance of the Splitter | [view](styling-and-theming.md#custom-styling-example) |
| `ID` | string | empty | string | Sets unique identifier for the Splitter | [view](state-management.md#enabling-persistence-in-splitter) |
| `EnablePersistence` | bool | false | true, false | Enables state persistence across page reloads | [view](state-management.md#enabling-persistence-in-splitter) |
| `EnableRtl` | bool | false | true, false | Enables right-to-left (RTL) mode | [view](#enablertl-example) |
| `Enabled` | bool | true | true, false | Enables/disables the Splitter component | [view](#enabled-example) |
| `EnableReversePanes` | bool | false | true, false | Reverses the order of splitter panes | [view](#enablereversepanes-example) |
| `HtmlAttributes` | Dictionary | null | key-value pairs | Adds additional HTML attributes | [view](#htmlattributes-example) |
| `ChildContent` | RenderFragment | null | RenderFragment | Sets the content for panes | [view](pane-configuration.md#content-types) |

---

## SfSplitter Methods

| Method | Parameters | Return Type | Description | Reference |
|--------|------------|-------------|-------------|-----------|
| `AddPaneAsync` | paneProperties: SplitterPane, index: int | Task | Adds a new pane at specified index | [view](split-panes.md#add-pane-dynamically) |
| `RemovePaneAsync` | index: int | Task | Removes pane at specified index | [view](split-panes.md#remove-pane-dynamically) |
| `ExpandAsync` | index: double | Task | Expands pane at specified index | [view](resizing-and-expand-collapse.md#programmatic-control) |
| `CollapseAsync` | index: double | Task | Collapses pane at specified index | [view](resizing-and-expand-collapse.md#programmatic-control) |
| `ToggleAsync` | expandIndices: List<double>, collapseIndices: List<double> | Task | Toggles multiple panes | [view](#toggleasync-example) |

---

## SplitterPane Properties

| Property | Type | Default | Accepted Values | Description | Reference |
|----------|------|---------|-----------------|-------------|-----------|
| `Size` | string | null | px, % | Sets the size of the pane | [view](pane-configuration.md#fixed-size) |
| `Min` | string | null | px, % | Sets minimum size of the pane | [view](resizing-and-expand-collapse.md#min-and-max-validation) |
| `Max` | string | null | px, % | Sets maximum size of the pane | [view](resizing-and-expand-collapse.md#min-and-max-validation) |
| `Collapsible` | bool | false | true, false | Enables collapsible behavior | [view](pane-configuration.md#collapsible-panes) |
| `Collapsed` | bool | false | true, false | Sets initial collapsed state | [view](pane-configuration.md#initial-collapsed-state) |
| `Resizable` | bool | true | true, false | Enables/disables resizing | [view](resizing-and-expand-collapse.md#prevent-resizing) |
| `Visible` | bool | true | true, false | Shows/hides the pane | [view](pane-configuration.md#pane-visibility) |
| `Content` | string | null | HTML string | Sets plain text or HTML content as string | [view](#content-property-example) |
| `ContentTemplate` | RenderFragment | null | RenderFragment | Sets template content | [view](pane-configuration.md#html-markup-content) |
| `CssClass` | string | empty | CSS class names | Customizes pane appearance | [view](#splitterpane-cssclass-example) |

---

## SplitterEvents Properties

| Event | Arguments | Description | Reference |
|-------|-----------|-------------|-----------|
| `Created` | object | Triggered when splitter is created | [view](events-and-interactions.md#created-event) |
| `Destroyed` | object | Triggered when splitter is destroyed | [view](#destroyed-event-example) |
| `OnResizeStart` | ResizeEventArgs | Triggered when resize starts | [view](events-and-interactions.md#onresizestart) |
| `Resizing` | ResizingEventArgs | Triggered during resize | [view](events-and-interactions.md#resizing) |
| `OnResizeStop` | ResizingEventArgs | Triggered when resize stops | [view](events-and-interactions.md#onresizestop) |
| `OnCollapse` | BeforeExpandEventArgs | Triggered before pane collapses | [view](events-and-interactions.md#oncollapse) |
| `OnExpand` | BeforeExpandEventArgs | Triggered before pane expands | [view](events-and-interactions.md#onexpand) |
| `Collapsed` | ExpandedEventArgs | Triggered after pane collapsed | [view](events-and-interactions.md#collapsed) |
| `Expanded` | ExpandedEventArgs | Triggered after pane expanded | [view](events-and-interactions.md#expanded) |

---

## Orientation Enum

| Value | Description | Reference |
|-------|-------------|-----------|
| `Horizontal` | Panes arranged side by side | [view](split-panes.md#horizontal-layout) |
| `Vertical` | Panes arranged one above the other | [view](split-panes.md#vertical-layout) |

---

## Event Args Classes

| Class | Properties | Description | Reference |
|-------|------------|-------------|-----------|
| `ResizeEventArgs` | Cancel: bool | Arguments for OnResizeStart event | [view](events-and-interactions.md#onresizestart) |
| `ResizingEventArgs` | EventArgs properties | Arguments for Resizing and OnResizeStop events | [view](events-and-interactions.md#resizing) |
| `BeforeExpandEventArgs` | Cancel: bool | Arguments for OnCollapse and OnExpand events | [view](events-and-interactions.md#oncollapse) |
| `ExpandedEventArgs` | EventArgs properties | Arguments for Collapsed and Expanded events | [view](events-and-interactions.md#collapsed) |

---

## Coverage Summary

| Category | Total | Covered | Missing |
|----------|-------|---------|---------|
| SfSplitter Properties | 12 | 12 | 0 |
| SfSplitter Methods | 5 | 5 | 0 |
| SplitterPane Properties | 11 | 11 | 0 |
| SplitterEvents | 9 | 9 | 0 |
| **Total** | **37** | **37** | **0** |

---

## Examples for Not Previously Covered APIs

### Content Property Example

Use the `Content` property to set plain HTML string content directly in the pane without using ContentTemplate.

```cshtml
<SfSplitter Height="240px" Width="100%">
    <SplitterPanes>
        <SplitterPane Size="50%" Content="<div><h3>ASP.NET Scheduler</h3><p>The ASP.NET Scheduler, a.k.a. event calendar, facilitates almost all calendar features, thus allowing users to manage their time efficiently.</p></div>">
        </SplitterPane>
        <SplitterPane Size="50%" Content="<div><h3>ASP.NET DataGrid</h3><p>The ASP.NET DataGrid control, or DataTable is a feature-rich control used to display data in a tabular format.</p></div>">
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>
```

**Use Cases**:
- Simple HTML text content
- Static content (no dynamic binding required)
- Lightweight alternative to ContentTemplate
- Basic styling and markup

**Comparison: Content vs ContentTemplate**:

| Feature | Content | ContentTemplate |
|---------|---------|-----------------|
| Complexity | Simple HTML strings | Complex components |
| Binding | Static | Two-way binding supported |
| Components | ❌ Not supported | ✅ Blazor components |
| Performance | Lightweight | Slightly heavier |
| Use Case | Text & simple HTML | Dynamic/interactive content |

---

### EnableRtl Example

Enable right-to-left (RTL) mode for languages like Arabic, Hebrew, or Persian.

```cshtml
<SfSplitter Height="200px" Width="100%" EnableRtl="true">
    <SplitterPanes>
        <SplitterPane Size="50%">
            <ContentTemplate>
                <div>Pane 1</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="50%">
            <ContentTemplate>
                <div>Pane 2</div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>
```

---

### Enabled Example

Disable the Splitter to prevent user interactions like resizing panes.

```cshtml
<SfSplitter Height="200px" Width="100%" Enabled="false">
    <SplitterPanes>
        <SplitterPane Size="50%">
            <ContentTemplate>
                <div>Pane 1</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="50%">
            <ContentTemplate>
                <div>Pane 2</div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>
```

---

### EnableReversePanes Example

Reverse the order of splitter panes - the right-most pane appears first.

```cshtml
<SfSplitter Height="200px" Width="100%" EnableReversePanes="true">
    <SplitterPanes>
        <SplitterPane Size="33%">
            <ContentTemplate>
                <div>Pane 1 (appears last)</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="33%">
            <ContentTemplate>
                <div>Pane 2 (appears middle)</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="33%">
            <ContentTemplate>
                <div>Pane 3 (appears first)</div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>
```

---

### HtmlAttributes Example

Add additional HTML attributes like id, title, or custom data attributes to the Splitter.

```cshtml
<SfSplitter Height="200px" Width="100%" 
    HtmlAttributes="@HtmlAttr">
    <SplitterPanes>
        <SplitterPane Size="50%">
            <ContentTemplate>
                <div>Pane 1</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="50%">
            <ContentTemplate>
                <div>Pane 2</div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    private Dictionary<string, object> HtmlAttr = new Dictionary<string, object>
    {
        { "id", "custom-splitter" },
        { "title", "Custom Splitter" },
        { "data-theme", "dark" }
    };
}
```

---

### ToggleAsync Example

Toggle multiple panes (expand some, collapse others) in a single method call.

```cshtml
<SfSplitter @ref="SplitterObj" Height="200px" Width="100%">
    <SplitterPanes>
        <SplitterPane Size="33%" Collapsible="true">
            <ContentTemplate>
                <div>Pane 1</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="33%" Collapsible="true">
            <ContentTemplate>
                <div>Pane 2</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="33%" Collapsible="true">
            <ContentTemplate>
                <div>Pane 3</div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

<SfButton Content="Toggle Panes" @onclick="@TogglePanes" />

@code {
    private SfSplitter SplitterObj;

    private async Task TogglePanes()
    {
        // Expand pane at index 0, collapse pane at index 2
        await this.SplitterObj.ToggleAsync(
            new List<double> { 0 },  // Indices to expand
            new List<double> { 2 }   // Indices to collapse
        );
    }
}
```

---

### SplitterPane CssClass Example

Apply custom CSS class to individual SplitterPane for specific styling.

```cshtml
<SfSplitter Height="200px" Width="100%">
    <SplitterPanes>
        <SplitterPane Size="50%" CssClass="custom-pane">
            <ContentTemplate>
                <div>Custom Styled Pane</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="50%">
            <ContentTemplate>
                <div>Normal Pane</div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

<style>
    .custom-pane {
        background-color: #f0f0f0;
        border: 2px solid #007bff;
    }
</style>
```

---

### Destroyed Event Example

Handle the event when the Splitter component is destroyed/removed from DOM.

```cshtml
<SfSplitter Height="200px" Width="100%">
    <SplitterEvents Destroyed="@OnDestroyed" />
    <SplitterPanes>
        <SplitterPane Size="50%">
            <ContentTemplate>
                <div>Pane 1</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane Size="50%">
            <ContentTemplate>
                <div>Pane 2</div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    private void OnDestroyed(object args)
    {
        // Cleanup logic when splitter is destroyed
        Console.WriteLine("Splitter component destroyed");
    }
}
```

---

## Usage Examples

### Basic Splitter

```cshtml
<SfSplitter Height="240px" Width="100%">
    <SplitterPanes>
        <SplitterPane>
            <ContentTemplate>
                <div>Left Pane</div>
            </ContentTemplate>
        </SplitterPane>
        <SplitterPane>
            <ContentTemplate>
                <div>Right Pane</div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>
```

### With Events

```cshtml
<SfSplitter>
    <SplitterEvents OnResizeStart="@OnResizeStartHandler" />
    <SplitterPanes>
        <SplitterPane Size="200px">
            <ContentTemplate>
                <div>Pane Content</div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    private void OnResizeStartHandler(ResizeEventArgs args)
    {
        // Handle resize start
    }
}
```

### With State Persistence

```cshtml
<SfSplitter ID="Splitter" Height="300px" EnablePersistence="true">
    <SplitterPanes>
        <SplitterPane @bind-Size="@PaneSize" Min="60px" Collapsible="true">
            <ContentTemplate>
                <div>Pane Content</div>
            </ContentTemplate>
        </SplitterPane>
    </SplitterPanes>
</SfSplitter>

@code {
    private string PaneSize { get; set; } = "25%";
}
```