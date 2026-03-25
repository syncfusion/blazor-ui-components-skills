# Content Render Modes

## Table of Contents
- [Overview](#overview)
- [Dynamic Rendering (Default)](#dynamic-rendering-default)
- [On Demand Rendering (Lazy Loading)](#on-demand-rendering-lazy-loading)
- [On Initial Rendering](#on-initial-rendering)
- [Comparison and Best Practices](#comparison-and-best-practices)

## Overview

The Blazor Tabs component supports three different content rendering modes via the `LoadOn` property. Each mode optimizes for different scenarios:

- **Dynamic (Default)**: Load only the selected tab content
- **Demand**: Lazy load content when tab is first selected
- **Init**: Load all content upfront on initialization

### LoadOn Property Values
- `ContentLoad.Dynamic` - Content replaced on each tab switch (default)
- `ContentLoad.Demand` - Content loaded on first selection and maintained
- `ContentLoad.Init` - All content loaded initially

---

## Dynamic Rendering (Default)

In this mode, only the selected tab's content is loaded and available in the DOM. When you switch tabs, the old content is replaced with the new content.

### Characteristics
- ✅ Best page loading performance
- ✅ Minimal initial DOM size
- ❌ Tab state not maintained between switches
- ❌ Components reinitialize on each selection

### Use Cases
- Tabs with heavy content (large grids, complex components)
- Performance-critical applications
- When state persistence is handled externally

### Implementation

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons
@using Syncfusion.Blazor.Grids

<SfTab LoadOn="ContentLoad.Dynamic" @bind-SelectedItem="@SelectedTab">
    <TabItems>
        <TabItem Disabled="@IsLoginTabDisabled">
            <HeaderTemplate>Login</HeaderTemplate>
            <ContentTemplate>
                <div style="padding: 20px;">
                    <h3>Sign in to proceed</h3>
                    <div style="margin: 10px 0;">
                        <input type="text" @bind-value="@UserName" placeholder="Username" />
                    </div>
                    <div style="margin: 10px 0;">
                        <input type="password" @bind-value="@Password" placeholder="Password" />
                    </div>
                    <SfButton @onclick="@HandleLogin">Log In</SfButton>
                </div>
            </ContentTemplate>
        </TabItem>
        <TabItem Disabled="@IsGridTabDisabled">
            <HeaderTemplate>Data Grid</HeaderTemplate>
            <ContentTemplate>
                <SfGrid DataSource="@GridData" Height="300px">
                    <GridColumns>
                        <GridColumn Field=@nameof(GridItem.ID) HeaderText="ID" Width="50"></GridColumn>
                        <GridColumn Field=@nameof(GridItem.Name) HeaderText="Name" Width="150"></GridColumn>
                        <GridColumn Field=@nameof(GridItem.Email) HeaderText="Email" Width="200"></GridColumn>
                    </GridColumns>
                </SfGrid>
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private string UserName = "";
    private string Password = "";
    private int SelectedTab = 0;
    private bool IsLoginTabDisabled = false;
    private bool IsGridTabDisabled = true;
    private List<GridItem> GridData = new();

    private void HandleLogin()
    {
        if (!string.IsNullOrEmpty(UserName) && !string.IsNullOrEmpty(Password))
        {
            // Login logic
            IsGridTabDisabled = false;
            SelectedTab = 1;
            // In dynamic mode, grid content is fresh each time
        }
    }

    public class GridItem
    {
        public int ID { get; set; }
        public string Name { get; set; }
        public string Email { get; set; }
    }
}
```

### State Management Note

In dynamic mode, if you need to maintain tab state (form values, scroll position), implement it manually:

```razor
<SfTab LoadOn="ContentLoad.Dynamic">
    <TabItems>
        <TabItem>
            <HeaderTemplate>Form Tab</HeaderTemplate>
            <ContentTemplate>
                <input type="text" @bind-value="@SavedFormData" />
                <!-- Save SavedFormData to local state -->
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private string SavedFormData = "";
    
    protected override async Task OnInitializedAsync()
    {
        // Load saved state from storage if needed
        SavedFormData = await LoadFromStorage();
    }
}
```

---

## On Demand Rendering (Lazy Loading)

In this mode, tab content is loaded only when the tab is first selected. Once loaded, the content remains in the DOM and state is maintained.

### Characteristics
- ✅ State maintained between switches
- ✅ Better than Init for large number of tabs
- ✅ Faster initial page load than Init mode
- ✅ Components initialize only once

### Use Cases
- Tabs with form data that must persist
- Multi-step wizards where previous selections matter
- Tabs with shared state between components
- Calendar, scheduler, or similar stateful components

### Implementation

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Calendars
@using Syncfusion.Blazor.Inputs

<SfTab LoadOn="ContentLoad.Demand">
    <TabItems>
        <TabItem>
            <HeaderTemplate>Calendar</HeaderTemplate>
            <ContentTemplate>
                <SfCalendar TValue="DateTime" @bind-Value="@SelectedDate"></SfCalendar>
            </ContentTemplate>
        </TabItem>
        <TabItem>
            <HeaderTemplate>Date Details</HeaderTemplate>
            <ContentTemplate>
                <div style="padding: 20px;">
                    <p><strong>Selected Date:</strong> @SelectedDate?.ToString("yyyy-MM-dd")</p>
                    <p><strong>Day of Week:</strong> @SelectedDate?.DayOfWeek</p>
                    <SfTextBox @bind-Value="@Notes" Placeholder="Enter notes for this date"></SfTextBox>
                </div>
            </ContentTemplate>
        </TabItem>
        <TabItem>
            <HeaderTemplate>Summary</HeaderTemplate>
            <ContentTemplate>
                <div style="padding: 20px;">
                    <p>Date: @SelectedDate?.ToString("yyyy-MM-dd")</p>
                    <p>Notes: @Notes</p>
                    <p style="color: green;">✓ Both tabs maintain their state</p>
                </div>
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private DateTime? SelectedDate = DateTime.Now;
    private string Notes = "";
}
```

### Multi-Step Wizard with Lazy Loading

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Buttons

<SfTab @ref="TabControl" LoadOn="ContentLoad.Demand" @bind-SelectedItem="@CurrentStep">
    <TabItems>
        <TabItem>
            <HeaderTemplate>Step 1: Personal Info</HeaderTemplate>
            <ContentTemplate>
                <div style="padding: 20px;">
                    <input type="text" @bind-value="@FormData.Name" placeholder="Full Name" />
                    <input type="email" @bind-value="@FormData.Email" placeholder="Email" />
                    <SfButton @onclick="@GoToStep2">Next</SfButton>
                </div>
            </ContentTemplate>
        </TabItem>
        <TabItem>
            <HeaderTemplate>Step 2: Address</HeaderTemplate>
            <ContentTemplate>
                <div style="padding: 20px;">
                    <input type="text" @bind-value="@FormData.Address" placeholder="Address" />
                    <input type="text" @bind-value="@FormData.City" placeholder="City" />
                    <SfButton @onclick="@GoToStep1">Back</SfButton>
                    <SfButton @onclick="@GoToStep3">Next</SfButton>
                </div>
            </ContentTemplate>
        </TabItem>
        <TabItem>
            <HeaderTemplate>Step 3: Confirm</HeaderTemplate>
            <ContentTemplate>
                <div style="padding: 20px;">
                    <p><strong>Name:</strong> @FormData.Name</p>
                    <p><strong>Email:</strong> @FormData.Email</p>
                    <p><strong>Address:</strong> @FormData.Address, @FormData.City</p>
                    <SfButton @onclick="@GoToStep2">Back</SfButton>
                    <SfButton @onclick="@Submit">Submit</SfButton>
                </div>
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private SfTab TabControl;
    private int CurrentStep = 0;
    private FormModel FormData = new();

    private async Task GoToStep2() => CurrentStep = 1;
    private async Task GoToStep3() => CurrentStep = 2;
    private async Task GoToStep1() => CurrentStep = 0;
    
    private async Task Submit()
    {
        // All form data persists from previous steps
        Console.WriteLine($"Submitting: {FormData.Name}, {FormData.Email}");
    }

    public class FormModel
    {
        public string Name { get; set; }
        public string Email { get; set; }
        public string Address { get; set; }
        public string City { get; set; }
    }
}
```

---

## On Initial Rendering

In this mode, all tab content is rendered and loaded into the DOM during component initialization. This is the most memory-intensive mode but provides full access to all components.

### Characteristics
- ✅ All state maintained permanently
- ✅ Can access components from other tabs
- ✅ No re-initialization on tab switch
- ❌ Higher initial load time
- ❌ More memory usage

### Use Cases
- Small number of tabs with lightweight content
- When you need cross-tab communication
- Applications where all content must be immediately available
- Testing and development scenarios

### Implementation

```razor
@using Syncfusion.Blazor.Navigations
@using Syncfusion.Blazor.Inputs

<SfTab LoadOn="ContentLoad.Init" @bind-SelectedItem="@SelectedTab">
    <TabItems>
        <TabItem>
            <HeaderTemplate>User Profile</HeaderTemplate>
            <ContentTemplate>
                <div style="padding: 20px;">
                    <SfTextBox @ref="NameInput" Placeholder="Enter name" @bind-Value="@UserProfile.Name"></SfTextBox>
                    <SfTextBox @ref="EmailInput" Placeholder="Enter email" @bind-Value="@UserProfile.Email"></SfTextBox>
                </div>
            </ContentTemplate>
        </TabItem>
        <TabItem>
            <HeaderTemplate>Preferences</HeaderTemplate>
            <ContentTemplate>
                <div style="padding: 20px;">
                    <label>
                        <input type="checkbox" @bind="@UserProfile.ReceiveNotifications" />
                        Receive Notifications
                    </label>
                    <p>Name entered above: @UserProfile.Name</p>
                </div>
            </ContentTemplate>
        </TabItem>
        <TabItem>
            <HeaderTemplate>Summary</HeaderTemplate>
            <ContentTemplate>
                <div style="padding: 20px;">
                    <p><strong>Name:</strong> @UserProfile.Name</p>
                    <p><strong>Email:</strong> @UserProfile.Email</p>
                    <p><strong>Notifications:</strong> @(UserProfile.ReceiveNotifications ? "Enabled" : "Disabled")</p>
                    <p style="color: green;">✓ All data from other tabs is accessible</p>
                </div>
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private SfTextBox NameInput;
    private SfTextBox EmailInput;
    private int SelectedTab = 0;
    private UserProfile UserProfile = new();

    // Can access components from other tabs in Init mode
    private async Task ClearAllFields()
    {
        await NameInput.ClearAsync();
        await EmailInput.ClearAsync();
    }

    public class UserProfile
    {
        public string Name { get; set; } = "";
        public string Email { get; set; } = "";
        public bool ReceiveNotifications { get; set; } = true;
    }
}
```

### Cross-Tab Communication

```razor
<SfTab LoadOn="ContentLoad.Init" Width="100%">
    <TabItems>
        <TabItem>
            <HeaderTemplate>Tab 1</HeaderTemplate>
            <ContentTemplate>
                <input type="text" @bind-value="@SharedData" placeholder="Shared value" />
            </ContentTemplate>
        </TabItem>
        <TabItem>
            <HeaderTemplate>Tab 2</HeaderTemplate>
            <ContentTemplate>
                <p>Value from Tab 1: @SharedData</p>
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>

@code {
    private string SharedData = "Hello from Tab 1";
    // In Init mode, this variable is accessible to all tabs simultaneously
}
```

---

## Comparison and Best Practices

### Selection Guide

```
Choose DYNAMIC if:
  • You have many tabs (10+)
  • Content is heavy (grids, charts)
  • Performance is critical
  • State is handled externally

Choose DEMAND if:
  • You have medium tabs (3-10)
  • Tabs have form data
  • State matters between switches
  • Building multi-step wizards

Choose INIT if:
  • You have few tabs (< 3)
  • All content is lightweight
  • Cross-tab communication needed
  • All content must be immediately available
```

### Performance Optimization

```razor
<!-- Dynamic Mode: Optimize heavy content loading -->
<SfTab LoadOn="ContentLoad.Dynamic">
    <TabItems>
        <TabItem>
            <ContentTemplate>
                @if (SelectedTab == 0)
                {
                    <!-- Only render heavy grid when selected -->
                    <HeavyGridComponent></HeavyGridComponent>
                }
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>

<!-- Demand Mode: Pre-load with indication -->
<SfTab LoadOn="ContentLoad.Demand">
    <TabItems>
        <TabItem>
            <ContentTemplate>
                @if (IsLoading)
                {
                    <p>Loading...</p>
                }
                else
                {
                    <ComplexComponent></ComplexComponent>
                }
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>
```

### Best Practice: Hybrid Approach

Combine LoadOn modes with conditional rendering:

```razor
<SfTab LoadOn="ContentLoad.Demand">
    <TabItems>
        <TabItem>
            <ContentTemplate>
                <!-- Demand-loaded but conditionally rendered -->
                @if (ShouldRender)
                {
                    <LargeDataComponent></LargeDataComponent>
                }
            </ContentTemplate>
        </TabItem>
    </TabItems>
</SfTab>
```
