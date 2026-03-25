# Tab Selection and Navigation

## Table of Contents
- [SelectedItem Property](#selecteditem-property)
- [Programmatic Selection with Select()](#programmatic-selection-with-select)
- [Detecting User Interaction](#detecting-user-interaction)
- [Selection Events](#selection-events)
- [Finding the Selected Tab](#finding-the-selected-tab)

## SelectedItem Property

The `SelectedItem` property gets or sets the currently selected tab index. It supports two-way binding with `@bind-SelectedItem`.

### Basic Usage

```razor
@using Syncfusion.Blazor.Navigations

<SfTab @bind-SelectedItem="SelectedTabIndex">
    <TabItems>
        <TabItem Content="Tab 1 content">
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Tab 2 content">
            <ChildContent>
                <TabHeader Text="Tab 2"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Tab 3 content">
            <ChildContent>
                <TabHeader Text="Tab 3"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private int SelectedTabIndex = 0; // Default to first tab
}
```

- `SelectedTabIndex` is 0-based (0 = first tab, 1 = second tab, etc.)
- The binding automatically updates when the user clicks a different tab
- Change the property value to switch tabs programmatically

### Example: Sync with Dropdown

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.DropDowns

<SfDropDownList TValue="int" TItem="DropdownData" 
                FloatLabelType="Syncfusion.Blazor.Inputs.FloatLabelType.Always"
                @bind-Index="SelectedTabIndex" 
                Placeholder="Select Tab"
                DataSource="@TabOptions">
    <DropDownListFieldSettings Text="Text" Value="ID"></DropDownListFieldSettings>
</SfDropDownList>

<SfTab @bind-SelectedItem="SelectedTabIndex">
    <TabItems>
        <TabItem Content="HTML content">
            <ChildContent>
                <TabHeader Text="HTML"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="C# content">
            <ChildContent>
                <TabHeader Text="C#"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Java content">
            <ChildContent>
                <TabHeader Text="Java"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private int SelectedTabIndex = 0;
    
    private List<DropdownData> TabOptions = new()
    {
        new DropdownData { ID = 0, Text = "HTML" },
        new DropdownData { ID = 1, Text = "C#" },
        new DropdownData { ID = 2, Text = "Java" }
    };
    
    public class DropdownData
    {
        public int ID { get; set; }
        public string Text { get; set; }
    }
}
```

## Programmatic Selection with Select()

The `Select()` method changes the selected tab programmatically. To use it, get a reference to the `SfTab` component with `@ref`.

### Basic Programmatic Selection

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfButton OnClick="SelectFirstTab">Select First Tab</SfButton>
<SfButton OnClick="SelectSecondTab">Select Second Tab</SfButton>
<SfButton OnClick="SelectThirdTab">Select Third Tab</SfButton>

<SfTab @ref="TabControl">
    <TabItems>
        <TabItem Content="First tab content">
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Second tab content">
            <ChildContent>
                <TabHeader Text="Tab 2"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Third tab content">
            <ChildContent>
                <TabHeader Text="Tab 3"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private SfTab TabControl;
    
    private void SelectFirstTab()
    {
        TabControl.Select(0);
    }
    
    private void SelectSecondTab()
    {
        TabControl.Select(1);
    }
    
    private void SelectThirdTab()
    {
        TabControl.Select(2);
    }
}
```

### Complex Example: Dynamic Tab Selection

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons
@using Syncfusion.Blazor.Inputs

<p>Selected Tab Index: @SelectedTabIndex</p>

<SfNumericTextBox TValue="int" @bind-Value="SelectedTabIndex" Min="0" Max="2"></SfNumericTextBox>

<SfButton OnClick="SelectByNumber">Select</SfButton>

<SfTab @ref="TabControl" @bind-SelectedItem="SelectedTabIndex">
    <TabItems>
        <TabItem Content="First content">
            <ChildContent>
                <TabHeader Text="First"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Second content">
            <ChildContent>
                <TabHeader Text="Second"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Third content">
            <ChildContent>
                <TabHeader Text="Third"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private SfTab TabControl;
    private int SelectedTabIndex = 0;
    
    private void SelectByNumber()
    {
        if (SelectedTabIndex >= 0 && SelectedTabIndex <= 2)
        {
            TabControl.Select(SelectedTabIndex);
        }
    }
}
```

## Detecting User Interaction

The `IsInteracted` property in the `SelectEventArgs` event handler distinguishes between user-initiated tab selection and programmatic selection.

- **`IsInteracted = true`**: User clicked the tab header
- **`IsInteracted = false`**: Tab was selected programmatically via `Select()` or binding

### Example: Log Selection Source

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.DropDowns
@using Syncfusion.Blazor.Buttons

<div class="row">
    <div class="col-xs-6 col-sm-6 col-lg-6 col-md-6">
        <SfDropDownList TValue="int" TItem="DropdownData" 
                        FloatLabelType="Syncfusion.Blazor.Inputs.FloatLabelType.Always"
                        @bind-Index="SelectionIndex"
                        Placeholder="Select Tab Item using dropdown"
                        DataSource="@TabOptions">
            <DropDownListFieldSettings Text="Text" Value="ID"></DropDownListFieldSettings>
            <DropDownListEvents TValue="int" TItem="DropdownData" ValueChange="OnDropdownChange"></DropDownListEvents>
        </SfDropDownList>
    </div>
</div>

<br />
<div class="eventlog">
    <span><b>@InteractionLog</b></span>
</div>

<SfTab @ref="TabControl">
    <TabEvents Selected="OnTabSelected"></TabEvents>
    <TabItems>
        <TabItem Content="Twitter is an online social networking service...">
            <ChildContent>
                <TabHeader Text="Twitter"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Facebook is an online social networking service...">
            <ChildContent>
                <TabHeader Text="Facebook"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="WhatsApp is a cross-platform instant messaging...">
            <ChildContent>
                <TabHeader Text="WhatsApp"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private SfTab TabControl;
    private int SelectionIndex = 0;
    private string InteractionLog = "";
    
    private List<DropdownData> TabOptions = new()
    {
        new DropdownData { ID = 0, Text = "Twitter" },
        new DropdownData { ID = 1, Text = "Facebook" },
        new DropdownData { ID = 2, Text = "WhatsApp" }
    };
    
    private void OnDropdownChange(Syncfusion.Blazor.DropDowns.ChangeEventArgs<int, DropdownData> args)
    {
        TabControl.Select(args.Value);
    }
    
    private void OnTabSelected(Syncfusion.Blazor.Navigations.SelectEventArgs args)
    {
        InteractionLog = args.IsInteracted 
            ? "Tab Item selected by user interaction" 
            : "Tab Item selected programmatically";
    }
    
    public class DropdownData
    {
        public int ID { get; set; }
        public string Text { get; set; }
    }
}
```

## Selection Events

### Selecting Event

The `Selecting` event fires before the tab selection changes. Use it to validate or prevent selection changes.

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabEvents Selecting="OnTabSelecting"></TabEvents>
    <TabItems>
        <TabItem Content="Tab 1 content">
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Tab 2 content">
            <ChildContent>
                <TabHeader Text="Tab 2"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private void OnTabSelecting(SelectingEventArgs args)
    {
        // args.PreviousIndex - Index of previously selected tab
        // args.SelectedIndex - Index of tab being selected
        
        if (args.SelectedIndex == 1)
        {
            // Prevent selecting the second tab
            args.Cancel = true;
        }
    }
}
```

### Selected Event

The `Selected` event fires after the tab selection has changed. Use it to respond to tab changes.

```razor
@using Syncfusion.Blazor.Navigations

<p>Current Tab: @CurrentTabName</p>

<SfTab>
    <TabEvents Selected="OnTabSelected"></TabEvents>
    <TabItems>
        <TabItem Content="Content 1">
            <ChildContent>
                <TabHeader Text="Home"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Content 2">
            <ChildContent>
                <TabHeader Text="Profile"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Content 3">
            <ChildContent>
                <TabHeader Text="Settings"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private string CurrentTabName = "Home";
    private string[] TabNames = { "Home", "Profile", "Settings" };
    
    private void OnTabSelected(Syncfusion.Blazor.Navigations.SelectEventArgs args)
    {
        CurrentTabName = TabNames[args.SelectedIndex];
        
        // Perform actions based on selected tab
        // - Load data
        // - Initialize forms
        // - Update state
    }
}
```

## Finding the Selected Tab

### Using SelectedItem Binding

```razor
@using Syncfusion.Blazor.Navigations

<p>Currently selected tab index: @SelectedTabIndex</p>

<SfTab @bind-SelectedItem="SelectedTabIndex">
    <!-- tab items -->
</SfTab>

@code {
    private int SelectedTabIndex = 0;
    
    // SelectedTabIndex always contains the current selection
    private void DoSomething()
    {
        int current = SelectedTabIndex;
    }
}
```

### Using Component Reference

```razor
@using Syncfusion.Blazor.Navigations

<SfTab @ref="TabControl">
    <!-- tab items -->
</SfTab>

<SfButton OnClick="GetSelected">Get Selected Tab</SfButton>

@code {
    private SfTab TabControl;
    
    private void GetSelected()
    {
        // Access SelectedItem property
        int index = TabControl.SelectedItem;
    }
}
```
