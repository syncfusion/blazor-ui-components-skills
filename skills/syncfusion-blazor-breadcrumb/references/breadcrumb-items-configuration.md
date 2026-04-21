# Breadcrumb Items Configuration

## Table of Contents
- [BreadcrumbItem Properties](#breadcrumbitem-properties)
- [Declaring Items](#declaring-items)
- [Auto-Generated Items from Current URL](#auto-generated-items-from-current-url)
- [Absolute URL Generation](#absolute-url-generation)
- [Dynamic Item Management](#dynamic-item-management)

---

## BreadcrumbItem Properties

The `BreadcrumbItem` component has three main properties for configuration:

| Property | Type | Description |
|----------|------|-------------|
| `Text` | `string` | Display text for the breadcrumb item |
| `Url` | `string` | Navigation URL (relative or absolute) |
| `IconCss` | `string` | CSS class for icon/image display |

**Example:**

```cshtml
<BreadcrumbItem Text="Home" Url="/" IconCss="e-icons e-home"></BreadcrumbItem>
```

---

## Declaring Items

Use the `BreadcrumbItems` tag directive to explicitly declare breadcrumb items:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb>
    <BreadcrumbItems>
        <BreadcrumbItem Text="Home" Url="https://blazor.syncfusion.com/demos/"></BreadcrumbItem>
        <BreadcrumbItem Text="Components" Url="https://blazor.syncfusion.com/demos/datagrid/overview"></BreadcrumbItem>
        <BreadcrumbItem Text="Navigations" Url="https://blazor.syncfusion.com/demos/menu-bar/default-functionalities"></BreadcrumbItem>
        <BreadcrumbItem Text="Breadcrumb" Url="./breadcrumb/default-functionalities"></BreadcrumbItem>
    </BreadcrumbItems>
</SfBreadcrumb>
```

Each `BreadcrumbItem` renders as a clickable link (if `Url` is set) or static text (if `Url` is omitted). This approach gives you explicit control over breadcrumb structure and content.

---

## Auto-Generated Items from Current URL

When items are not explicitly declared, the breadcrumb automatically generates items from the current page URL:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb></SfBreadcrumb>
```

**Example:** If your current page URL is `/docs/components/navigations/breadcrumb`, the breadcrumb will auto-generate:
- `docs` > `components` > `navigations` > `breadcrumb`

Each segment becomes a clickable item that navigates to its corresponding URL. This is useful when you want breadcrumbs to automatically reflect the user's navigation path without manual configuration.

---

## Absolute URL Generation

You can also generate breadcrumb items from an absolute URL using the `Url` property on the `SfBreadcrumb` component:

```cshtml
@using Syncfusion.Blazor.Navigations

<SfBreadcrumb Url="https://blazor.syncfusion.com/demos/breadcrumb/navigation">
</SfBreadcrumb>
```

This generates breadcrumb items based on the URL path structure: `/demos` > `/breadcrumb` > `/navigation`

---

## Dynamic Item Management

Manage breadcrumb items at runtime using the `Items` property:

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfBreadcrumb Items="@items"></SfBreadcrumb>

<SfButton OnClick="AddBefore">Insert Before</SfButton>
<SfButton OnClick="AddAfter">Insert After</SfButton>
<SfButton OnClick="Remove">Remove Last</SfButton>

@code {
    List<BreadcrumbItem> items = new List<BreadcrumbItem>
    {
        new BreadcrumbItem { IconCss = "e-icons e-home" },
        new BreadcrumbItem { Text = "Open", IconCss = "e-icons e-folder-open", Url = "https://blazor.syncfusion.com/demos/datagrid/overview" },
        new BreadcrumbItem { Text = "New", IconCss = "e-icons e-file-new" }
    };

    private void AddBefore()
    {
        var index = items.IndexOf(items.Where(item => item.Text == "Open").FirstOrDefault());
        items.Insert(index, new BreadcrumbItem { Text = "Save", IconCss = "e-icons e-save" });
    }

    private void AddAfter()
    {
        var index = items.IndexOf(items.Where(item => item.Text == "New").FirstOrDefault());
        items.Insert((index + 1), new BreadcrumbItem { Text = "Delete", IconCss = "e-icons e-delete" });
    }

    private void Remove()
    {
        if (items.Count > 0)
            items.RemoveAt(items.Count() - 1);
    }
}
```

**Operations:**
- **Insert:** Use `items.Insert(index, newItem)` to add an item at a specific position
- **Remove:** Use `items.RemoveAt(index)` to remove an item
- **Clear:** Use `items.Clear()` to remove all items

The breadcrumb UI updates automatically when the `items` collection changes, enabling reactive navigation flows.
