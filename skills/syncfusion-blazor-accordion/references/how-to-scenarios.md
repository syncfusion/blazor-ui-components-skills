# How-To Scenarios for Blazor Accordion Component

## Table of Contents
- [Add Icons to Accordion Headers](#add-icons-to-accordion-headers)
- [Create Nested Accordions](#create-nested-accordions)
- [Add and Remove Items Dynamically](#add-and-remove-items-dynamically)
- [Create a Wizard Using Accordion](#create-a-wizard-using-accordion)
- [Enable or Disable Accordion Items](#enable-or-disable-accordion-items)
- [Integrate Components Inside Accordion](#integrate-components-inside-accordion)
- [Prevent Expand or Collapse](#prevent-expand-or-collapse)
- [Show or Hide Accordion Items](#show-or-hide-accordion-items)

## Add Icons to Accordion Headers

Add custom icons to accordion headers using the `IconCss` property to provide visual cues for different content types.

### Basic Icon Implementation

```cshtml
@using Syncfusion.Blazor.Navigations

<SfAccordion>
    <AccordionItems>
        <AccordionItem Header="Athletics" IconCss="e-icons e-people" Expanded="true" Content="Marathon, Javelin Throw, Discus Throw, High Jump, Long Jump"></AccordionItem>
        <AccordionItem Header="Water Games" IconCss="e-icons e-water-drop" Content="Diving, Swimming, Marathon Swimming, Synchronized Swimming, Water Polo"></AccordionItem>
        <AccordionItem Header="Racing" IconCss="e-icons e-flag" Content="Cycling BMX, Cycling Mountain Bike, Cycle Racing, Sailing, Rowing"></AccordionItem>
    </AccordionItems>
</SfAccordion>
```

### Custom Icon Fonts

Use custom icon fonts by defining `@font-face` and applying via `IconCss`:

```cshtml
<SfAccordion>
    <AccordionItems>
        <AccordionItem Header="Documents" IconCss="custom-icon doc-icon" Content="Document content here"></AccordionItem>
        <AccordionItem Header="Images" IconCss="custom-icon img-icon" Content="Image content here"></AccordionItem>
    </AccordionItems>
</SfAccordion>

<style>
    @@font-face {
        font-family: 'custom-icons';
        src: url('path/to/custom-icons.ttf') format('truetype');
    }
    
    .custom-icon {
        font-family: 'custom-icons';
        font-size: 16px;
        padding-right: 10px;
    }
    
    .doc-icon::before {
        content: "\e001";
    }
    
    .img-icon::before {
        content: "\e002";
    }
</style>
```

### SVG Icons

Use SVG icons within HeaderTemplate for more flexibility:

```cshtml
<SfAccordion>
    <AccordionItems>
        <AccordionItem>
            <HeaderTemplate>
                <div style="display: flex; align-items: center;">
                    <svg width="20" height="20" style="margin-right: 10px;">
                        <circle cx="10" cy="10" r="8" fill="#2196F3"/>
                    </svg>
                    <span>Custom SVG Icon</span>
                </div>
            </HeaderTemplate>
            <ContentTemplate>
                <div>Content with SVG icon in header</div>
            </ContentTemplate>
        </AccordionItem>
    </AccordionItems>
</SfAccordion>
```

### Best Practices

- Use consistent icon sizes across all headers
- Choose icons that clearly represent the content
- Ensure icons have sufficient contrast with background
- Provide meaningful header text alongside icons (don't rely on icons alone)
- Test icon visibility in different themes

## Create Nested Accordions

Create hierarchical collapsible sections by placing accordion components within `ContentTemplate`.

### Two-Level Nesting

```cshtml
@using Syncfusion.Blazor.Navigations

<SfAccordion>
    <AccordionItems>
        <AccordionItem Header="Video">
            <ContentTemplate>
                <SfAccordion>
                    <AccordionItems>
                        <AccordionItem Header="Video Track 1" Content="Video content 1"></AccordionItem>
                        <AccordionItem Header="Video Track 2" Content="Video content 2"></AccordionItem>
                    </AccordionItems>
                </SfAccordion>
            </ContentTemplate>
        </AccordionItem>
        <AccordionItem Header="Music">
            <ContentTemplate>
                <SfAccordion>
                    <AccordionItems>
                        <AccordionItem Header="Music Track 1" Content="Music content 1"></AccordionItem>
                        <AccordionItem Header="Music Track 2" Content="Music content 2"></AccordionItem>
                    </AccordionItems>
                </SfAccordion>
            </ContentTemplate>
        </AccordionItem>
    </AccordionItems>
</SfAccordion>
```

### Three-Level Nesting

```cshtml
<SfAccordion>
    <AccordionItems>
        <AccordionItem Header="Media">
            <ContentTemplate>
                <SfAccordion>
                    <AccordionItems>
                        <AccordionItem Header="Music">
                            <ContentTemplate>
                                <SfAccordion>
                                    <AccordionItems>
                                        <AccordionItem Header="Rock" Content="Rock music files"></AccordionItem>
                                        <AccordionItem Header="Jazz" Content="Jazz music files"></AccordionItem>
                                    </AccordionItems>
                                </SfAccordion>
                            </ContentTemplate>
                        </AccordionItem>
                        <AccordionItem Header="Videos" Content="Video files"></AccordionItem>
                    </AccordionItems>
                </SfAccordion>
            </ContentTemplate>
        </AccordionItem>
    </AccordionItems>
</SfAccordion>
```

### Styling Nested Accordions

```css
/* Remove extra padding from nested accordions */
.e-accordion .e-acrdn-panel.e-nested {
    padding: 0;
}

/* Differentiate nested levels with indentation */
.e-accordion .e-accordion .e-acrdn-header {
    padding-left: 30px;
    background-color: #F5F5F5;
}

/* Further indent for third level */
.e-accordion .e-accordion .e-accordion .e-acrdn-header {
    padding-left: 50px;
    background-color: #EEEEEE;
}
```

### Use Cases

- File/folder navigation systems
- Multi-level category menus
- Hierarchical documentation
- Organizational structures
- Nested settings panels

## Add and Remove Items Dynamically

Dynamically manage accordion items by manipulating the underlying data collection.

### Add Items

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton @onclick="AddItem">Add Item</SfButton>

<SfAccordion>
    <AccordionItems>
        @foreach (var item in AccordionData)
        {
            <AccordionItem @bind-Expanded="item.IsExpanded">
                <HeaderTemplate>
                    <div>@item.Header</div>
                </HeaderTemplate>
                <ContentTemplate>
                    <div>@item.Content</div>
                </ContentTemplate>
            </AccordionItem>
        }
    </AccordionItems>
</SfAccordion>

@code {
    List<AccordionItemData> AccordionData = new List<AccordionItemData>() {
        new AccordionItemData { Header = "ASP.NET", Content = "ASP.NET content", IsExpanded = true },
        new AccordionItemData { Header = "ASP.NET MVC", Content = "MVC content", IsExpanded = false }
    };

    void AddItem() {
        AccordionData.Add(new AccordionItemData {
            Header = "JavaScript",
            Content = "JavaScript programming language content",
            IsExpanded = false
        });
    }

    public class AccordionItemData {
        public string Header { get; set; }
        public string Content { get; set; }
        public bool IsExpanded { get; set; }
    }
}
```

### Remove Items

```cshtml
<SfButton @onclick="RemoveFirst">Remove First Item</SfButton>
<SfButton @onclick="RemoveLast">Remove Last Item</SfButton>

<SfAccordion>
    <AccordionItems>
        @foreach (var item in AccordionData)
        {
            <AccordionItem Header="@item.Header" Content="@item.Content"></AccordionItem>
        }
    </AccordionItems>
</SfAccordion>

@code {
    List<AccordionItemData> AccordionData = new List<AccordionItemData>();

    void RemoveFirst() {
        if (AccordionData.Count > 0) {
            AccordionData.RemoveAt(0);
        }
    }

    void RemoveLast() {
        if (AccordionData.Count > 0) {
            AccordionData.RemoveAt(AccordionData.Count - 1);
        }
    }
}
```

### Add/Remove with User Input

```cshtml
@using Syncfusion.Blazor.Inputs

<SfTextBox @bind-Value="NewItemHeader" Placeholder="Header"></SfTextBox>
<SfTextBox @bind-Value="NewItemContent" Placeholder="Content"></SfTextBox>
<SfButton @onclick="AddCustomItem">Add</SfButton>

<SfAccordion>
    <AccordionItems>
        @foreach (var item in Items)
        {
            <AccordionItem>
                <HeaderTemplate>
                    <div style="display: flex; justify-content: space-between; width: 100%;">
                        <span>@item.Header</span>
                        <SfButton CssClass="e-small" @onclick="() => RemoveItem(item)">Remove</SfButton>
                    </div>
                </HeaderTemplate>
                <ContentTemplate>
                    <div>@item.Content</div>
                </ContentTemplate>
            </AccordionItem>
        }
    </AccordionItems>
</SfAccordion>

@code {
    string NewItemHeader = "";
    string NewItemContent = "";
    List<AccordionItemData> Items = new List<AccordionItemData>();

    void AddCustomItem() {
        if (!string.IsNullOrEmpty(NewItemHeader)) {
            Items.Add(new AccordionItemData { Header = NewItemHeader, Content = NewItemContent });
            NewItemHeader = "";
            NewItemContent = "";
        }
    }

    void RemoveItem(AccordionItemData item) {
        Items.Remove(item);
    }
}
```

## Create a Wizard Using Accordion

Build multi-step wizards using accordion with controlled expansion and validation.

### Sequential Wizard

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Inputs
@using Syncfusion.Blazor.Buttons

<SfAccordion ExpandMode="ExpandMode.Single" @bind-ExpandedIndices="CurrentStep">
    <AccordionEvents Expanding="OnExpanding"></AccordionEvents>
    <AccordionItems>
        <AccordionItem Disabled="@(CurrentStep[0] != 0)">
            <HeaderTemplate>
                <div>Step 1: Personal Information</div>
            </HeaderTemplate>
            <ContentTemplate>
                <div style="padding: 15px;">
                    <SfTextBox @bind-Value="PersonalInfo.Name" Placeholder="Name"></SfTextBox>
                    <br /><br />
                    <SfTextBox @bind-Value="PersonalInfo.Email" Placeholder="Email"></SfTextBox>
                    <br /><br />
                    <SfButton @onclick="GoToStep2">Next</SfButton>
                </div>
            </ContentTemplate>
        </AccordionItem>
        
        <AccordionItem Disabled="@Step2Disabled">
            <HeaderTemplate>
                <div>Step 2: Address Details</div>
            </HeaderTemplate>
            <ContentTemplate>
                <div style="padding: 15px;">
                    <SfTextBox @bind-Value="AddressInfo.Street" Placeholder="Street"></SfTextBox>
                    <br /><br />
                    <SfTextBox @bind-Value="AddressInfo.City" Placeholder="City"></SfTextBox>
                    <br /><br />
                    <SfButton @onclick="GoToStep1">Back</SfButton>
                    <SfButton @onclick="GoToStep3">Next</SfButton>
                </div>
            </ContentTemplate>
        </AccordionItem>
        
        <AccordionItem Disabled="@Step3Disabled">
            <HeaderTemplate>
                <div>Step 3: Review and Submit</div>
            </HeaderTemplate>
            <ContentTemplate>
                <div style="padding: 15px;">
                    <p><b>Name:</b> @PersonalInfo.Name</p>
                    <p><b>Email:</b> @PersonalInfo.Email</p>
                    <p><b>Street:</b> @AddressInfo.Street</p>
                    <p><b>City:</b> @AddressInfo.City</p>
                    <SfButton @onclick="GoToStep2">Back</SfButton>
                    <SfButton @onclick="Submit">Submit</SfButton>
                </div>
            </ContentTemplate>
        </AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    int[] CurrentStep = new int[] { 0 };
    bool Step2Disabled = true;
    bool Step3Disabled = true;

    PersonalData PersonalInfo = new PersonalData();
    AddressData AddressInfo = new AddressData();

    void OnExpanding(ExpandEventArgs args) {
        // Prevent skipping steps
        if (args.Index == 1 && Step2Disabled) {
            args.Cancel = true;
        }
        if (args.Index == 2 && Step3Disabled) {
            args.Cancel = true;
        }
    }

    void GoToStep2() {
        if (!string.IsNullOrEmpty(PersonalInfo.Name) && !string.IsNullOrEmpty(PersonalInfo.Email)) {
            Step2Disabled = false;
            CurrentStep = new int[] { 1 };
        }
    }

    void GoToStep3() {
        if (!string.IsNullOrEmpty(AddressInfo.Street) && !string.IsNullOrEmpty(AddressInfo.City)) {
            Step3Disabled = false;
            CurrentStep = new int[] { 2 };
        }
    }

    void GoToStep1() {
        CurrentStep = new int[] { 0 };
    }

    void Submit() {
        // Handle submission
        Console.WriteLine("Form submitted!");
    }

    public class PersonalData {
        public string Name { get; set; }
        public string Email { get; set; }
    }

    public class AddressData {
        public string Street { get; set; }
        public string City { get; set; }
    }
}
```

## Enable or Disable Accordion Items

Control the enabled/disabled state of accordion items dynamically.

### Toggle Disabled State

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton @onclick="ToggleFirstItem">Enable/Disable First Item</SfButton>

<SfAccordion>
    <AccordionItems>
        <AccordionItem Disabled="@IsDisabled" Expanded="true" Header="ASP.NET" Content="Microsoft ASP.NET is a set of technologies for building Web applications."></AccordionItem>
        <AccordionItem Header="ASP.NET MVC" Content="The Model-View-Controller pattern separates an application into three components."></AccordionItem>
        <AccordionItem Header="JavaScript" Content="JavaScript is an interpreted programming language."></AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    bool IsDisabled = false;

    void ToggleFirstItem() {
        IsDisabled = !IsDisabled;
    }
}
```

### Conditional Disabling

```cshtml
<SfAccordion>
    <AccordionItems>
        @foreach (var item in Items)
        {
            <AccordionItem Disabled="@item.IsLocked" Header="@item.Title" Content="@item.Content"></AccordionItem>
        }
    </AccordionItems>
</SfAccordion>

@code {
    List<ItemData> Items = new List<ItemData>() {
        new ItemData { Title = "Free Content", Content = "Available to all", IsLocked = false },
        new ItemData { Title = "Premium Content", Content = "Requires subscription", IsLocked = true }
    };

    public class ItemData {
        public string Title { get; set; }
        public string Content { get; set; }
        public bool IsLocked { get; set; }
    }
}
```

## Integrate Components Inside Accordion

Embed other Syncfusion or custom components within accordion panels.

### TreeView Integration

```cshtml
@using Syncfusion.Blazor.Navigations

<SfAccordion>
    <AccordionItems>
        <AccordionItem>
            <HeaderTemplate>
                <div>Documents</div>
            </HeaderTemplate>
            <ContentTemplate>
                <SfTreeView TValue="TreeItem">
                    <TreeViewFieldsSettings TValue="TreeItem" Id="Id" DataSource="@Documents" Text="Name" ParentID="ParentId" HasChildren="HasChildren" Expanded="Expanded"></TreeViewFieldsSettings>
                </SfTreeView>
            </ContentTemplate>
        </AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    List<TreeItem> Documents = new List<TreeItem>() {
        new TreeItem { Id = "1", Name = "Documents", HasChildren = true, Expanded = true },
        new TreeItem { Id = "2", ParentId = "1", Name = "File1.docx" },
        new TreeItem { Id = "3", ParentId = "1", Name = "File2.pdf" }
    };

    public class TreeItem {
        public string Id { get; set; }
        public string ParentId { get; set; }
        public string Name { get; set; }
        public bool HasChildren { get; set; }
        public bool Expanded { get; set; }
    }
}
```

### Grid Integration

```cshtml
<SfAccordion>
    <AccordionItems>
        <AccordionItem Header="Employee Data">
            <ContentTemplate>
                <SfGrid DataSource="@Employees">
                    <GridColumns>
                        <GridColumn Field="@nameof(Employee.Id)" HeaderText="ID" Width="100"></GridColumn>
                        <GridColumn Field="@nameof(Employee.Name)" HeaderText="Name" Width="150"></GridColumn>
                        <GridColumn Field="@nameof(Employee.Department)" HeaderText="Department" Width="150"></GridColumn>
                    </GridColumns>
                </SfGrid>
            </ContentTemplate>
        </AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    List<Employee> Employees = new List<Employee>() {
        new Employee { Id = 1, Name = "John Doe", Department = "IT" },
        new Employee { Id = 2, Name = "Jane Smith", Department = "HR" }
    };

    public class Employee {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Department { get; set; }
    }
}
```

## Prevent Expand or Collapse

Prevent accordion items from expanding or collapsing under specific conditions.

### Prevent Expansion Based on Conditions

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.DropDowns

<SfAccordion>
    <AccordionEvents Expanding="OnExpanding" Collapsing="OnCollapsing"></AccordionEvents>
    <AccordionItems>
        <AccordionItem Expanded="true">
            <HeaderTemplate>
                <SfDropDownList TValue="string" TItem="Country" Placeholder="Select a country" DataSource="@Countries" Width="200">
                    <DropDownListEvents TValue="string" TItem="Country" OnOpen="OnDropdownOpen" OnClose="OnDropdownClose"></DropDownListEvents>
                    <DropDownListFieldSettings Value="Name"></DropDownListFieldSettings>
                </SfDropDownList>
            </HeaderTemplate>
            <ContentTemplate>
                <div>Dropdown content</div>
            </ContentTemplate>
        </AccordionItem>
        <AccordionItem>
            <HeaderTemplate>
                <SfButton @onclick="ButtonClick">Click Me</SfButton>
            </HeaderTemplate>
            <ContentTemplate>
                <div>Button content</div>
            </ContentTemplate>
        </AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    bool IsDropdownOpen = false;
    bool IsButtonClicked = false;

    List<Country> Countries = new List<Country>() {
        new Country { Name = "Australia" },
        new Country { Name = "United States" }
    };

    void OnDropdownOpen() {
        IsDropdownOpen = true;
    }

    void OnDropdownClose() {
        IsDropdownOpen = false;
    }

    void ButtonClick() {
        IsButtonClicked = true;
    }

    void OnExpanding(ExpandEventArgs args) {
        if (IsButtonClicked || IsDropdownOpen) {
            args.Cancel = true;
            ResetFlags();
        }
    }

    void OnCollapsing(CollapseEventArgs args) {
        if (IsButtonClicked || IsDropdownOpen) {
            args.Cancel = true;
            ResetFlags();
        }
    }

    void ResetFlags() {
        IsButtonClicked = false;
        IsDropdownOpen = false;
    }

    public class Country {
        public string Name { get; set; }
    }
}
```

## Show or Hide Accordion Items

Dynamically show or hide accordion items based on conditions.

### Using Conditional Rendering

```cshtml
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton @onclick="ToggleItem">Show/Hide Middle Item</SfButton>

<SfAccordion>
    <AccordionItems>
        <AccordionItem Expanded="true" Header="ASP.NET" Content="ASP.NET content"></AccordionItem>
        
        @if (ShowItem)
        {
            <AccordionItem Header="ASP.NET MVC" Content="MVC content"></AccordionItem>
        }
        
        <AccordionItem Header="JavaScript" Content="JavaScript content"></AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    bool ShowItem = true;

    void ToggleItem() {
        ShowItem = !ShowItem;
    }
}
```

### Using Visible Property

```cshtml
<SfButton @onclick="ToggleVisibility">Show/Hide Second Item</SfButton>

<SfAccordion>
    <AccordionItems>
        <AccordionItem Expanded="true" Header="ASP.NET" Content="ASP.NET content"></AccordionItem>
        <AccordionItem Visible="@IsVisible" Header="ASP.NET MVC" Content="MVC content"></AccordionItem>
        <AccordionItem Header="JavaScript" Content="JavaScript content"></AccordionItem>
    </AccordionItems>
</SfAccordion>

@code {
    bool IsVisible = true;

    void ToggleVisibility() {
        IsVisible = !IsVisible;
    }
}
```

### Conditional Visibility Based on Data

```cshtml
<SfAccordion>
    <AccordionItems>
        @foreach (var item in Items)
        {
            <AccordionItem Visible="@item.IsVisible" Header="@item.Header" Content="@item.Content"></AccordionItem>
        }
    </AccordionItems>
</SfAccordion>

@code {
    List<ItemData> Items = new List<ItemData>() {
        new ItemData { Header = "Always Visible", Content = "Content 1", IsVisible = true },
        new ItemData { Header = "Conditionally Visible", Content = "Content 2", IsVisible = false },
        new ItemData { Header = "Always Visible", Content = "Content 3", IsVisible = true }
    };

    public class ItemData {
        public string Header { get; set; }
        public string Content { get; set; }
        public bool IsVisible { get; set; }
    }
}
```

## Related Topics

- [Getting Started](getting-started.md) - Basic accordion setup
- [Expand Modes](expand-modes.md) - Controlling expansion behavior
- [Data Binding and Events](data-binding-events.md) - Dynamic content and event handling
- [Customization and Styling](customization-styling.md) - CSS customization for scenarios
- [Accessibility and Animations](accessibility-animations.md) - Ensuring accessible implementations
