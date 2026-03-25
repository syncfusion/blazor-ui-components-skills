# Adaptive Layout in Syncfusion Blazor TreeGrid

## Table of Contents
- [Overview](#overview)
- [Enable Adaptive UI](#enable-adaptive-ui)
- [Recommended CSS Setup](#recommended-css-setup)
- [Key Notes](#key-notes)

---

## Overview

The TreeGrid UI is redesigned for small screens — filter, sort, and edit dialogs render as full-screen overlays, and rows can render vertically for better mobile usability.

---

## Enable Adaptive UI

Set `EnableAdaptiveUI="true"` to activate mobile-optimized dialogs:

```razor
@using Syncfusion.Blazor.TreeGrid
@using Syncfusion.Blazor.Grids

<div class="e-mobile-layout">
    <div class="e-mobile-content">
        <SfTreeGrid DataSource="@TreeData"
                    IdMapping="TaskID"
                    ParentIdMapping="ParentID"
                    TreeColumnIndex="1"
                    EnableAdaptiveUI="true"
                    AllowSorting="true"
                    AllowFiltering="true"
                    AllowPaging="true"
                    Height="100%"
                    Width="100%"
                    Toolbar="@(new List<string> { "Add", "Edit", "Delete", "Cancel", "Update", "Search" })">
            
            <TreeGridFilterSettings Type="FilterType.Excel" />
            <TreeGridPageSettings PageSize="5" PageSizeMode="PageSizeMode.Root" />
            <TreeGridEditSettings AllowAdding="true"
                                  AllowEditing="true"
                                  AllowDeleting="true"
                                  Mode="EditMode.Dialog" />
            
            <TreeGridColumns>
                <TreeGridColumn Field="TaskID"   HeaderText="Task ID"   IsPrimaryKey="true" Width="135" TextAlign="TextAlign.Right"
                                ValidationRules="@(new ValidationRules { Required = true, Number = true })" />
                <TreeGridColumn Field="TaskName" HeaderText="Task Name" Width="280"
                                ValidationRules="@(new ValidationRules { Required = true })" />
                <TreeGridColumn Field="Duration" HeaderText="Duration"  Width="140" TextAlign="TextAlign.Right" />
                <TreeGridColumn Field="Progress" HeaderText="Progress"  Width="145"
                                EditType="EditType.DropDownEdit" />
            </TreeGridColumns>
        </SfTreeGrid>
    </div>
</div>

@code {
    private List<SelfReferenceData> TreeData { get; set; }

    protected override void OnInitialized()
    {
        TreeData = SelfReferenceData.GetTree().Take(30).ToList();
    }
}
```

**What changes with `EnableAdaptiveUI="true"`:**
- Filter dialog opens full-screen instead of inline/popup
- Sort dialog opens full-screen
- Edit dialog opens full-screen (Dialog mode recommended)
- Toolbar collapses to a mobile-friendly layout
- Pager adapts to mobile screen size

---

## Recommended CSS Setup

Wrap the grid in a mobile container for best results:

```css
.e-mobile-layout {
    position: relative;
    width: 360px;
    height: 640px;
    margin: auto;
    border: 2px solid black;
}

.e-mobile-content {
    overflow: auto;
    height: 100%;
    width: 100%;
}
```

> The `e-adaptive-demo` and `e-bigger` CSS classes help trigger the Syncfusion mobile breakpoints.

---

## Key Notes

| Setting | Recommendation |
|---|---|
| Edit mode | Use `EditMode.Dialog` with adaptive UI for best experience |
| Filter type | `FilterType.Excel` works well in full-screen adaptive dialog |
| Height/Width | Set to `100%` to fill the mobile container |
| `PageSizeMode` | Use `PageSizeMode.Root` to page only root-level rows |

---
