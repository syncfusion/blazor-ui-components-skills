# Advanced Features and Customization

## Table of Contents
- [Nested Tabs](#nested-tabs)
- [ContentTemplate Property](#contenttemplate-property)
- [HeaderTemplate](#headertemplate)
- [Create Wizard Using Tabs](#create-wizard-using-tabs)
- [Drag and Drop Support](#drag-and-drop-support)
- [Prevent Content Swipe Selection](#prevent-content-swipe-selection)
- [State Persistence](#state-persistence)
- [Close Button Functionality](#close-button-functionality)
- [Touch and Swipe Support](#touch-and-swipe-support)

## Nested Tabs

Render tabs within other tabs using `ContentTemplate` to create hierarchical tab structures.

### Implementation

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabItems>
        <!-- USA Tab with nested tabs -->
        <TabItem>
            <ChildContent>
                <TabHeader Text="USA"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <!-- Nested SfTab component -->
                <SfTab>
                    <TabItems>
                        <TabItem Content="New York City comprises 5 boroughs sitting where the Hudson River meets the Atlantic Ocean...">
                            <ChildContent>
                                <TabHeader Text="New York"></TabHeader>
                            </ChildContent>
                        </TabItem>
                        <TabItem Content="Los Angeles is a sprawling Southern California city and the center of the nation's film industry...">
                            <ChildContent>
                                <TabHeader Text="Los Angeles"></TabHeader>
                            </ChildContent>
                        </TabItem>
                        <TabItem Content="Chicago, on Lake Michigan in Illinois, is among the largest cities in the U.S...">
                            <ChildContent>
                                <TabHeader Text="Chicago"></TabHeader>
                            </ChildContent>
                        </TabItem>
                    </TabItems>
                </SfTab>
            </ContentTemplate>
        </TabItem>
        
        <!-- France Tab with nested tabs -->
        <TabItem>
            <ChildContent>
                <TabHeader Text="France"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <SfTab>
                    <TabItems>
                        <TabItem Content="Paris, France capital, is a major European city...">
                            <ChildContent>
                                <TabHeader Text="Paris"></TabHeader>
                            </ChildContent>
                        </TabItem>
                        <TabItem Content="Marseille, a port city in southern France...">
                            <ChildContent>
                                <TabHeader Text="Marseille"></TabHeader>
                            </ChildContent>
                        </TabItem>
                        <TabItem Content="Lyon, the capital city in France's Auvergne-Rhône-Alpes region...">
                            <ChildContent>
                                <TabHeader Text="Lyon"></TabHeader>
                            </ChildContent>
                        </TabItem>
                    </TabItems>
                </SfTab>
            </ContentTemplate>
        </TabItem>
        
        <!-- Australia Tab with nested tabs -->
        <TabItem>
            <ChildContent>
                <TabHeader Text="Australia"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <SfTab>
                    <TabItems>
                        <TabItem Content="Sydney is best known for its harbourfront Sydney Opera House...">
                            <ChildContent>
                                <TabHeader Text="Sydney"></TabHeader>
                            </ChildContent>
                        </TabItem>
                        <TabItem Content="Melbourne is the coastal capital of Victoria...">
                            <ChildContent>
                                <TabHeader Text="Melbourne"></TabHeader>
                            </ChildContent>
                        </TabItem>
                        <TabItem Content="Brisbane is the capital of Queensland...">
                            <ChildContent>
                                <TabHeader Text="Brisbane"></TabHeader>
                            </ChildContent>
                        </TabItem>
                    </TabItems>
                </SfTab>
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>
```

### Use Cases
- Multi-level navigation
- Hierarchical content organization
- Category and subcategory grouping
- Complex information structures

## ContentTemplate Property

`ContentTemplate` allows rendering complex Blazor components and markup instead of simple text content.

### With Blazor Components

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfTab>
    <TabItems>
        <!-- Tab with Button Component -->
        <TabItem>
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div>
                    <p>Click the button below:</p>
                    <SfButton OnClick="ShowMessage">Click Me</SfButton>
                    <p>@Message</p>
                </div>
            </ContentTemplate>
        </TabItem>
        
        <!-- Tab with Form -->
        <TabItem>
            <ChildContent>
                <TabHeader Text="Tab 2"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div>
                    <h3>User Form</h3>
                    <label>Name:</label>
                    <input type="text" @bind="UserName" />
                    <br />
                    <label>Email:</label>
                    <input type="email" @bind="UserEmail" />
                    <br />
                    <SfButton OnClick="SubmitForm">Submit</SfButton>
                </div>
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private string Message = "";
    private string UserName = "";
    private string UserEmail = "";
    
    private void ShowMessage()
    {
        Message = "Button clicked!";
    }
    
    private void SubmitForm()
    {
        Message = $"Form submitted: {UserName}, {UserEmail}";
    }
}
```

### With Data Binding

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Grids

<SfTab>
    <TabItems>
        <TabItem>
            <ChildContent>
                <TabHeader Text="Employees"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <SfGrid DataSource="@Employees">
                    <GridColumns>
                        <GridColumn Field="@nameof(Employee.ID)" HeaderText="ID" Width="50"></GridColumn>
                        <GridColumn Field="@nameof(Employee.Name)" HeaderText="Name" Width="100"></GridColumn>
                        <GridColumn Field="@nameof(Employee.Email)" HeaderText="Email" Width="150"></GridColumn>
                    </GridColumns>
                </SfGrid>
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    public List<Employee> Employees = new()
    {
        new Employee { ID = 1, Name = "John", Email = "john@example.com" },
        new Employee { ID = 2, Name = "Jane", Email = "jane@example.com" },
        new Employee { ID = 3, Name = "Bob", Email = "bob@example.com" }
    };
    
    public class Employee
    {
        public int ID { get; set; }
        public string Name { get; set; }
        public string Email { get; set; }
    }
}
```

## HeaderTemplate

Customize tab headers with complex markup using `HeaderTemplate`.

```razor
@using Syncfusion.Blazor.Navigations

<SfTab>
    <TabItems>
        <TabItem>
            <HeaderTemplate>
                <div style="display: flex; align-items: center; gap: 10px;">
                    <span style="font-weight: bold;">📧 Mail</span>
                </div>
            </HeaderTemplate>
            <ContentTemplate>
                <div>Email content goes here</div>
            </ContentTemplate>
        </TabItem>
        <TabItem>
            <HeaderTemplate>
                <div style="display: flex; align-items: center; gap: 10px;">
                    <span style="font-weight: bold;">📅 Calendar</span>
                </div>
            </HeaderTemplate>
            <ContentTemplate>
                <div>Calendar content goes here</div>
            </ContentTemplate>
        </TabItem>
        <TabItem>
            <HeaderTemplate>
                <div style="display: flex; align-items: center; gap: 10px;">
                    <span style="font-weight: bold;">⚙️ Settings</span>
                </div>
            </HeaderTemplate>
            <ContentTemplate>
                <div>Settings content goes here</div>
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>
```

## Create Wizard Using Tabs

Use tabs to create a multi-step wizard interface for guided workflows.

### Wizard Implementation

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons
@using Syncfusion.Blazor.Inputs

<SfTab @bind-SelectedItem="CurrentStep">
    <TabItems>
        <!-- Step 1: Personal Information -->
        <TabItem>
            <ChildContent>
                <TabHeader Text="Personal Info"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div style="padding: 20px;">
                    <h3>Step 1: Personal Information</h3>
                    <div style="margin: 10px 0;">
                        <label>First Name:</label>
                        <SfTextBox @bind-Value="FirstName" Placeholder="Enter first name"></SfTextBox>
                    </div>
                    <div style="margin: 10px 0;">
                        <label>Last Name:</label>
                        <SfTextBox @bind-Value="LastName" Placeholder="Enter last name"></SfTextBox>
                    </div>
                    <div style="margin: 10px 0;">
                        <label>Email:</label>
                        <SfTextBox @bind-Value="Email" Placeholder="Enter email"></SfTextBox>
                    </div>
                    <div style="margin-top: 20px;">
                        <SfButton @onclick="GoToStep2">Next</SfButton>
                    </div>
                </div>
            </ContentTemplate>
        </TabItem>
        
        <!-- Step 2: Address -->
        <TabItem>
            <ChildContent>
                <TabHeader Text="Address"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div style="padding: 20px;">
                    <h3>Step 2: Address</h3>
                    <div style="margin: 10px 0;">
                        <label>Street:</label>
                        <SfTextBox @bind-Value="Street" Placeholder="Enter street address"></SfTextBox>
                    </div>
                    <div style="margin: 10px 0;">
                        <label>City:</label>
                        <SfTextBox @bind-Value="City" Placeholder="Enter city"></SfTextBox>
                    </div>
                    <div style="margin: 10px 0;">
                        <label>Country:</label>
                        <SfTextBox @bind-Value="Country" Placeholder="Enter country"></SfTextBox>
                    </div>
                    <div style="margin-top: 20px;">
                        <SfButton @onclick="GoToStep1">Back</SfButton>
                        <SfButton @onclick="GoToStep3" style="margin-left: 10px;">Next</SfButton>
                    </div>
                </div>
            </ContentTemplate>
        </TabItem>
        
        <!-- Step 3: Confirmation -->
        <TabItem>
            <ChildContent>
                <TabHeader Text="Confirmation"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div style="padding: 20px;">
                    <h3>Step 3: Confirmation</h3>
                    <p><strong>Name:</strong> @FirstName @LastName</p>
                    <p><strong>Email:</strong> @Email</p>
                    <p><strong>Address:</strong> @Street, @City, @Country</p>
                    <div style="margin-top: 20px;">
                        <SfButton @onclick="GoToStep2">Back</SfButton>
                        <SfButton IsPrimary="true" @onclick="SubmitWizard" style="margin-left: 10px;">Submit</SfButton>
                    </div>
                </div>
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>

<div style="margin-top: 20px; padding: 10px; border: 1px solid #ccc;">
    <p>@Message</p>
</div>

@code {
    private int CurrentStep = 0;
    private string FirstName = "";
    private string LastName = "";
    private string Email = "";
    private string Street = "";
    private string City = "";
    private string Country = "";
    private string Message = "";
    
    private void GoToStep1() => CurrentStep = 0;
    private void GoToStep2() => CurrentStep = 1;
    private void GoToStep3() => CurrentStep = 2;
    
    private void SubmitWizard()
    {
        Message = $"Wizard completed! User: {FirstName} {LastName}";
    }
}
```

## Drag and Drop Support

Enable drag and drop functionality to reorder tab items.

```razor
@using Syncfusion.Blazor.Navigations

<SfTab AllowDragAndDrop="true">
    <TabItems>
        <TabItem Content="Drag this tab">
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Or this tab">
            <ChildContent>
                <TabHeader Text="Tab 2"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Or this one">
            <ChildContent>
                <TabHeader Text="Tab 3"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>
```

## Prevent Content Swipe Selection

Control swipe gesture behavior on touch devices using `SwipeMode` property.

```razor
@using Syncfusion.Blazor.Navigations

<!-- Disable both touch and mouse swipe to allow content selection -->
<SfTab SwipeMode="~TabSwipeMode.Touch & ~TabSwipeMode.Mouse">
    <TabItems>
        <TabItem>
            <ChildContent>
                <TabHeader Text="Content Tab"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div style="user-select: auto;">
                    <p>Select this text: This content can now be selected on touch devices</p>
                    <p>Swiping will not change tabs anymore</p>
                </div>
            </ContentTemplate>
        </TabItem>
        <TabItem>
            <ChildContent>
                <TabHeader Text="Tab 2"></TabHeader>
            </ChildContent>
            <ContentTemplate>
                <div>Tab 2 content</div>
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>
```

### SwipeMode Options

- `TabSwipeMode.Touch & TabSwipeMode.Mouse` - Default: Both touch and mouse swipe enabled
- `TabSwipeMode.Touch` - Touch swipe only
- `TabSwipeMode.Mouse` - Mouse swipe only  
- `~TabSwipeMode.Touch & ~TabSwipeMode.Mouse` - Disable all swipe actions

## State Persistence

Persist the selected tab using browser localStorage.

### Implementation with Blazor

```razor
@using Syncfusion.Blazor.Navigations
@using System.Text.Json

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
    private int SelectedTabIndex = 0;
    
    protected override async Task OnInitializedAsync()
    {
        // Load saved tab index from localStorage
        var savedIndex = await JS.InvokeAsync<string>("localStorage.getItem", "selectedTabIndex");
        if (!string.IsNullOrEmpty(savedIndex) && int.TryParse(savedIndex, out int index))
        {
            SelectedTabIndex = index;
        }
    }
    
    protected override async Task OnAfterRenderAsync(bool firstRender)
    {
        // Save selected tab when changed
        await JS.InvokeVoidAsync("localStorage.setItem", "selectedTabIndex", SelectedTabIndex.ToString());
    }
    
    @inject IJSRuntime JS;
}
```

## Close Button Functionality

Display close buttons on tabs for user-initiated removal.

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfTab @ref="TabControl" ShowCloseButton="true">
    <TabItems>
        <TabItem Content="Click the X button to close this tab">
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

<p>Closed tabs count: @ClosedCount</p>

@code {
    private SfTab TabControl;
    private int ClosedCount = 0;
}
```

## Touch and Swipe Support

The Tabs component supports touch and swipe gestures on mobile devices using the `SwipeMode` property.

### Swipe to Navigate
- Swipe left: Navigate to next tab
- Swipe right: Navigate to previous tab
- Works in Scrollable overflow mode
- Can be controlled with `SwipeMode` property

### Touch Scroll
- Scrollable mode: Touch and drag to scroll through tabs
- Popup mode: Tap dropdown to open overflow items

### Implementation with Touch Support

```razor
@using Syncfusion.Blazor.Navigations

<!-- Enable swipe gestures (touch and mouse) - default behavior -->
<SfTab SwipeMode="TabSwipeMode.Touch & TabSwipeMode.Mouse" OverflowMode="OverflowMode.Scrollable">
    <TabItems>
        <TabItem Content="Swipe left to navigate">
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Swipe right to go back">
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
```

### Best Practices for Mobile

```razor
<!-- Mobile-optimized tabs with touch swipe only -->
<SfTab HeaderPlacement="HeaderPosition.Top" 
        OverflowMode="OverflowMode.Popup"
        SwipeMode="TabSwipeMode.Touch"
        Width="100%"
        Height="300px">
    <TabItems>
        <TabItem Content="Mobile optimized content">
            <ChildContent>
                <TabHeader Text="Tab 1"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Tab 2">
            <ChildContent>
                <TabHeader Text="Tab 2"></TabHeader>
            </ChildContent>
        </TabItem>
        <TabItem Content="Tab 3">
            <ChildContent>
                <TabHeader Text="Tab 3"></TabHeader>
            </ChildContent>
        </TabItem>
    </TabItems>
</SfTab>
```
