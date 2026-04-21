# Overflow Modes

## Table of Contents
- [Overview](#overview)
- [MaxItems Property](#maxitems-property)
- [Collapsed Mode](#collapsed-mode)
- [Menu Mode](#menu-mode)
- [Wrap Mode](#wrap-mode)
- [Scroll Mode](#scroll-mode)
- [Hidden Mode](#hidden-mode)
- [None Mode](#none-mode)

---

## Overview

When a breadcrumb has many items, the `OverflowMode` property determines how to handle items that exceed the container's available space. Use the `MaxItems` property to specify the maximum number of items to display before overflow handling activates.

Available overflow modes:
- **Collapsed:** Show first/last items, hide middle items in a collapsible section
- **Menu:** Show visible items, place overflow items in a dropdown menu
- **Wrap:** Items wrap to multiple lines
- **Scroll:** Show horizontal scrollbar for overflow items
- **Hidden:** Show maximum items possible, hide overflow items on parent click
- **None:** Display all items on a single line (no overflow handling)

---

## MaxItems Property

Set the `MaxItems` property to limit the number of breadcrumb items displayed:

```cshtml
<SfBreadcrumb MaxItems="3">
    <!-- breadcrumb items -->
</SfBreadcrumb>
```

When the number of breadcrumb items exceeds `MaxItems`, the `OverflowMode` determines how the overflow is handled. If `MaxItems` is not set, no overflow handling is applied.

---

## Collapsed Mode

**Collapsed mode** displays the first and last breadcrumb items and hides the remaining middle items with a collapsed icon. When the collapsed icon is clicked, all items become visible and navigable.

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb MaxItems="3" EnableNavigation="false" OverflowMode="BreadcrumbOverflowMode.Collapsed">
    <BreadcrumbItems>
        <BreadcrumbItem Text="Home" Url="https://blazor.syncfusion.com/documentation/breadcrumb/introduction"></BreadcrumbItem>
        <BreadcrumbItem Text="Getting" Url="https://blazor.syncfusion.com/documentation/breadcrumb/getting-started"></BreadcrumbItem>
        <BreadcrumbItem Text="Icons" Url="https://blazor.syncfusion.com/documentation/breadcrumb/icons"></BreadcrumbItem>
        <BreadcrumbItem Text="Navigation" Url="https://blazor.syncfusion.com/documentation/breadcrumb/navigation"></BreadcrumbItem>
        <BreadcrumbItem Text="Overflow" Url="https://blazor.syncfusion.com/documentation/breadcrumb/overflow"></BreadcrumbItem>
    </BreadcrumbItems>
    <BreadcrumbTemplates>
        <SeparatorTemplate>
            <span class="e-icons e-arrow"></span>
        </SeparatorTemplate>
    </BreadcrumbTemplates>
</SfBreadcrumb>
```

**Use case:** Collapsed mode is ideal for mobile or constrained layouts where you need to show only the start and end of the breadcrumb trail.

---

## Menu Mode

**Menu mode** displays as many breadcrumb items as possible within the container space and places the remaining overflow items in a dropdown menu.

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb MaxItems="3" EnableNavigation="false" OverflowMode="BreadcrumbOverflowMode.Menu">
    <BreadcrumbItems>
        <BreadcrumbItem Text="Home" Url="https://blazor.syncfusion.com/documentation/breadcrumb/introduction"></BreadcrumbItem>
        <BreadcrumbItem Text="Getting" Url="https://blazor.syncfusion.com/documentation/breadcrumb/getting-started"></BreadcrumbItem>
        <BreadcrumbItem Text="Icons" Url="https://blazor.syncfusion.com/documentation/breadcrumb/icons"></BreadcrumbItem>
        <BreadcrumbItem Text="Navigation" Url="https://blazor.syncfusion.com/documentation/breadcrumb/navigation"></BreadcrumbItem>
        <BreadcrumbItem Text="Overflow" Url="https://blazor.syncfusion.com/documentation/breadcrumb/overflow"></BreadcrumbItem>
    </BreadcrumbItems>
    <BreadcrumbTemplates>
        <SeparatorTemplate>
            <span class="e-icons e-arrow"></span>
        </SeparatorTemplate>
    </BreadcrumbTemplates>
</SfBreadcrumb>
```

**Use case:** Menu mode is useful for deep hierarchies where some items are hidden initially but accessible via a dropdown menu.

---

## Wrap Mode

**Wrap mode** wraps breadcrumb items to multiple lines when the breadcrumb's width exceeds the container space.

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb MaxItems="3" EnableNavigation="false" OverflowMode="BreadcrumbOverflowMode.Wrap">
    <BreadcrumbItems>
        <BreadcrumbItem Text="Home" Url="https://blazor.syncfusion.com/documentation/breadcrumb/introduction"></BreadcrumbItem>
        <BreadcrumbItem Text="Getting" Url="https://blazor.syncfusion.com/documentation/breadcrumb/getting-started"></BreadcrumbItem>
        <BreadcrumbItem Text="Icons" Url="https://blazor.syncfusion.com/documentation/breadcrumb/icons"></BreadcrumbItem>
        <BreadcrumbItem Text="Navigation" Url="https://blazor.syncfusion.com/documentation/breadcrumb/navigation"></BreadcrumbItem>
        <BreadcrumbItem Text="Overflow" Url="https://blazor.syncfusion.com/documentation/breadcrumb/overflow"></BreadcrumbItem>
    </BreadcrumbItems>
    <BreadcrumbTemplates>
        <SeparatorTemplate>
            <span class="e-icons e-arrow"></span>
        </SeparatorTemplate>
    </BreadcrumbTemplates>
</SfBreadcrumb>
```

**Use case:** Wrap mode is ideal for responsive designs where you want all items visible, even if they need to wrap to multiple lines.

---

## Scroll Mode

**Scroll mode** displays a horizontal scrollbar when the breadcrumb's width exceeds the container space.

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb MaxItems="3" EnableNavigation="false" OverflowMode="BreadcrumbOverflowMode.Scroll">
    <BreadcrumbItems>
        <BreadcrumbItem Text="Home" Url="https://blazor.syncfusion.com/documentation/breadcrumb/introduction"></BreadcrumbItem>
        <BreadcrumbItem Text="Getting" Url="https://blazor.syncfusion.com/documentation/breadcrumb/getting-started"></BreadcrumbItem>
        <BreadcrumbItem Text="Icons" Url="https://blazor.syncfusion.com/documentation/breadcrumb/icons"></BreadcrumbItem>
        <BreadcrumbItem Text="Navigation" Url="https://blazor.syncfusion.com/documentation/breadcrumb/navigation"></BreadcrumbItem>
        <BreadcrumbItem Text="Overflow" Url="https://blazor.syncfusion.com/documentation/breadcrumb/overflow"></BreadcrumbItem>
        <BreadcrumbItem Text="Overflow1" Url="https://blazor.syncfusion.com/documentation/breadcrumb/overflow"></BreadcrumbItem>
        <BreadcrumbItem Text="Overflow2" Url="https://blazor.syncfusion.com/documentation/breadcrumb/overflow"></BreadcrumbItem>
    </BreadcrumbItems>
    <BreadcrumbTemplates>
        <SeparatorTemplate>
            <span class="e-icons e-arrow"></span>
        </SeparatorTemplate>
    </BreadcrumbTemplates>
</SfBreadcrumb>
```

**Use case:** Scroll mode is useful for fixed-width containers where you want to maintain breadcrumb height while allowing horizontal navigation.

---

## Hidden Mode

**Hidden mode** displays the maximum number of items possible in the container space and hides the remaining items. Clicking on a previous item reveals the hidden items.

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb MaxItems="3" EnableNavigation="false" OverflowMode="BreadcrumbOverflowMode.Hidden">
    <BreadcrumbItems>
        <BreadcrumbItem Text="Home" Url="https://blazor.syncfusion.com/documentation/breadcrumb/introduction"></BreadcrumbItem>
        <BreadcrumbItem Text="Getting" Url="https://blazor.syncfusion.com/documentation/breadcrumb/getting-started"></BreadcrumbItem>
        <BreadcrumbItem Text="Icons" Url="https://blazor.syncfusion.com/documentation/breadcrumb/icons"></BreadcrumbItem>
        <BreadcrumbItem Text="Navigation" Url="https://blazor.syncfusion.com/documentation/breadcrumb/navigation"></BreadcrumbItem>
        <BreadcrumbItem Text="Overflow" Url="https://blazor.syncfusion.com/documentation/breadcrumb/overflow"></BreadcrumbItem>
    </BreadcrumbItems>
    <BreadcrumbTemplates>
        <SeparatorTemplate>
            <span class="e-icons e-arrow"></span>
        </SeparatorTemplate>
    </BreadcrumbTemplates>
</SfBreadcrumb>
```

**Use case:** Hidden mode keeps the breadcrumb compact while allowing users to navigate backwards to reveal hidden items.

---

## None Mode

**None mode** displays all breadcrumb items on a single line without any overflow handling. Items may overflow the container if space is limited.

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb OverflowMode="BreadcrumbOverflowMode.None">
    <BreadcrumbItems>
        <BreadcrumbItem Text="Home" Url="https://blazor.syncfusion.com/documentation/breadcrumb/introduction"></BreadcrumbItem>
        <BreadcrumbItem Text="Getting" Url="https://blazor.syncfusion.com/documentation/breadcrumb/getting-started"></BreadcrumbItem>
        <BreadcrumbItem Text="Icons" Url="https://blazor.syncfusion.com/documentation/breadcrumb/icons"></BreadcrumbItem>
    </BreadcrumbItems>
</SfBreadcrumb>
```

**Use case:** None mode is suitable for applications with predictable, short breadcrumb trails or when space is not a constraint.
