# Editor Template Customization

## Table of Contents
- [Overview](#overview)
- [Default Editor Window](#default-editor-window)
- [Customizing Editor Labels](#customizing-editor-labels)
- [Field Validation](#field-validation)
- [Editor Time Duration](#editor-time-duration)
- [Preventing Editor/Quick Popups](#preventing-editorquick-popups)
- [Opening Editor Manually](#opening-editor-manually)
- [Custom Editor Template](#custom-editor-template)
- [Custom Editor Header and Footer](#custom-editor-header-and-footer)
- [Resource Options in Editor](#resource-options-in-editor)
- [Recurrence Options in Editor](#recurrence-options-in-editor)
- [Editor Template Validation](#editor-template-validation)
- [Quick Info Popups](#quick-info-popups)
- [Quick Popup on Cell](#quick-popup-on-cell)
- [Quick Popup on Event](#quick-popup-on-event)
- [Quick Popup Customization Combinations](#quick-popup-customization-combinations)
- [Disabling Quick Popups](#disabling-quick-popups)
- [More Events Popup](#more-events-popup)

## Overview

The Scheduler provides comprehensive editor and quick popup customization options. You can create custom event editors using templates, customize labels and validation rules, and control when popups appear. The editor window supports field binding, validation, custom buttons, and resource/recurrence options.

## Default Editor Window

The editor window opens when a cell is double-clicked (Add mode) or an event is double-clicked (Edit mode):

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public string Location { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public string Description { get; set; }
        public bool IsAllDay { get; set; }
        public string RecurrenceRule { get; set; }
        public string RecurrenceException { get; set; }
        public Nullable<int> RecurrenceID { get; set; }
    }
}
```

## Customizing Editor Labels

Change default editor field labels using the `Title` property in `ScheduleField`:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource">
        <ScheduleField>
            <FieldSettings Text="EventName" Title="Event Name"></FieldSettings>
            <FieldSettings Text="EventLocation" Title="Venue"></FieldSettings>
            <FieldSettings Text="EventDescription" Title="Event Details"></FieldSettings>
        </ScheduleField>
    </ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            EventName = "Team Sync", 
            EventLocation = "Conference Room A",
            EventDescription = "Weekly team meeting",
            StartTime = new DateTime(2026, 3, 24, 10, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string EventName { get; set; }
        public string EventLocation { get; set; }
        public string EventDescription { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

## Field Validation

Add validation rules to editor fields using `ValidationRules` properties:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource">
        <ScheduleField>
            <FieldSettings 
                Text="Subject" 
                Title="Subject"
                ValidationRules="@SubjectValidation">
            </FieldSettings>
            <FieldSettings 
                Text="Location" 
                Title="Location"
                ValidationRules="@LocationValidation">
            </FieldSettings>
            <FieldSettings 
                Text="Description" 
                Title="Description"
                ValidationRules="@DescriptionValidation">
            </FieldSettings>
        </ScheduleField>
    </ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    ValidationRules SubjectValidation = new ValidationRules { Required = true };
    
    static Dictionary<string, object> LocationValidationMessages = new Dictionary<string, object>() 
    { 
        { "regex", "Location must contain alphanumeric characters and hyphens only" } 
    };
    ValidationRules LocationValidation = new ValidationRules 
    { 
        Required = true, 
        RegexPattern = "^[a-zA-Z0-9- ]*$", 
        Messages = LocationValidationMessages 
    };
    
    ValidationRules DescriptionValidation = new ValidationRules 
    { 
        Required = true, 
        MinLength = 5, 
        MaxLength = 500 
    };

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            Location = "Room A",
            Description = "Team synchronization meeting",
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public string Location { get; set; }
        public string Description { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**Validation Rule Properties:**
- `Required` - Field must have a value
- `RegexPattern` - Field must match regex pattern
- `MinLength` - Minimum character count
- `MaxLength` - Maximum character count
- `Messages` - Custom validation error messages

## Editor Time Duration

Customize the default time duration in the editor window using the `OnPopupOpen` event:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" OnPopupOpen="@OnPopupOpen"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        }
    };

    public void OnPopupOpen(PopupOpenEventArgs<AppointmentData> args)
    {
        // OnPopupOpen event fires when popup/editor opens
        if (args.Type == PopupType.Editor)
        {
            // Set default event duration to 60 minutes instead of 30
            args.Duration = 60;
        }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**PopupOpenEventArgs Properties:**
- `Type` - Popup type (Editor, QuickInfo, EditEventInfo, etc.)
- `Duration` - Event duration in minutes
- `Data` - Event data being edited
- `Cancel` - Set to true to prevent popup

## Preventing Editor/Quick Popups

Prevent editor and quick popup windows from opening by canceling the `OnPopupOpen` event:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" OnPopupOpen="@OnPopupOpen"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        }
    };

    public void OnPopupOpen(PopupOpenEventArgs<AppointmentData> args)
    {
        // Prevent editor and quick popups
        if (args.Type == PopupType.Editor || args.Type == PopupType.QuickInfo)
        {
            args.Cancel = true;
        }
        
        // Prevent only specific popup types
        if (args.Type == PopupType.Editor)
        {
            args.Cancel = true; // Prevent editor window
        }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**PopupType Options:**
- `Editor` - Detailed editor window
- `QuickInfo` - Quick popup on cell click
- `EditEventInfo` - Quick popup on event click
- `ViewEventInfo` - Quick popup in responsive mode
- `EventContainer` - More events indicator popup
- `RecurrenceAlert` - Edit recurrence event alert
- `DeleteAlert` - Delete confirmation
- `ValidationAlert` - Validation error alert
- `RecurrenceValidationAlert` - Recurrence validation alert

## Opening Editor Manually

Open the editor window programmatically using `OpenEditorAsync`:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Buttons

<SfButton Content="New Event" OnClick="OpenNewEventEditor"></SfButton>
<SfButton Content="Edit First Event" OnClick="EditFirstEvent"></SfButton>

<SfSchedule TValue="AppointmentData" Height="550px" @ref="ScheduleRef" @bind-SelectedDate="@CurrentDate">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    SfSchedule<AppointmentData> ScheduleRef;
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Team Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        }
    };

    public async Task OpenNewEventEditor()
    {
        var newEvent = new AppointmentData 
        { 
            Id = 2,
            Subject = "New Event",
            StartTime = new DateTime(2026, 3, 24, 14, 0, 0),
            EndTime = new DateTime(2026, 3, 24, 15, 0, 0)
        };
        await ScheduleRef.OpenEditorAsync(newEvent, CurrentAction.Add);
    }

    public async Task EditFirstEvent()
    {
        if (DataSource.Count > 0)
        {
            await ScheduleRef.OpenEditorAsync(DataSource[0], CurrentAction.Save);
        }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

## Custom Editor Template

Create a completely custom event editor using `@bind-Value` for two-way binding:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Calendars
@using Syncfusion.Blazor.DropDowns
@using Syncfusion.Blazor.Inputs

<SfSchedule TValue="AppointmentData" Width="100%" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleTemplates>
        <EditorTemplate>
            <table class="custom-event-editor" width="100%" cellpadding="5">
                <tbody>
                    <tr>
                        <td class="e-textlabel">Summary</td>
                        <td colspan="4">
                            <SfTextBox @bind-Value="@((context as AppointmentData).Subject)"></SfTextBox>
                        </td>
                    </tr>
                    <tr>
                        <td class="e-textlabel">Status</td>
                        <td colspan="4">
                            <SfDropDownList TValue="string" TItem="StatusField" 
                                DataSource="@StatusData" 
                                Placeholder="Choose status" 
                                @bind-Value="@((context as AppointmentData).EventType)">
                                <DropDownListFieldSettings Text="Text" Value="Id"></DropDownListFieldSettings>
                            </SfDropDownList>
                        </td>
                    </tr>
                    <tr>
                        <td class="e-textlabel">From</td>
                        <td colspan="4">
                            <SfDateTimePicker @bind-Value="@((context as AppointmentData).StartTime)"></SfDateTimePicker>
                        </td>
                    </tr>
                    <tr>
                        <td class="e-textlabel">To</td>
                        <td colspan="4">
                            <SfDateTimePicker @bind-Value="@((context as AppointmentData).EndTime)"></SfDateTimePicker>
                        </td>
                    </tr>
                    <tr>
                        <td class="e-textlabel">Reason</td>
                        <td colspan="4">
                            <SfTextBox Multiline="true" @bind-Value="@((context as AppointmentData).Description)"></SfTextBox>
                        </td>
                    </tr>
                </tbody>
            </table>
        </EditorTemplate>
    </ScheduleTemplates>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.WorkWeek"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    public class StatusField
    {
        public string Id { get; set; }
        public string Text { get; set; }
    }
    
    List<StatusField> StatusData = new List<StatusField>() 
    {
        new StatusField(){ Id= "New", Text= "New" },
        new StatusField(){ Id= "Requested", Text= "Requested" },
        new StatusField(){ Id= "Confirmed", Text= "Confirmed" },
    };

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0), 
            EventType = "Confirmed" 
        }
    };
    
    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public string Description { get; set; }
        public string EventType { get; set; }
    }
}
```

**EditorTemplate Binding Rules:**
- **Two-way binding is required** - Each field within the template MUST use `@bind-Value` for two-way binding to perform CRUD actions (Add, Edit, Delete)
- **Context object** - The `context` provides the current AppointmentData object being edited
- **Automatic CRUD** - Direct property binding with `@bind-Value` ensures CRUD operations work automatically without manual field updates
- **Component binding** - All Syncfusion components (TextBox, DateTimePicker, DropDownList) support `@bind-Value` directive
- **Dropdown configuration** - DropDownList requires TValue, TItem, and DropDownListFieldSettings for proper binding

## Custom Editor Header and Footer

Customize editor window header and footer:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.Buttons

<SfSchedule TValue="AppointmentData" Height="650px" @ref="ScheduleRef" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" OnPopupClose="OnPopupClose"></ScheduleEvents>
    <ScheduleTemplates>
        <EditorHeaderTemplate>
            <div class="editor-header">
                <h2>@((context as AppointmentData)?.Subject ?? "New Event")</h2>
                <p class="header-info">@((context as AppointmentData)?.StartTime.ToString("dddd, MMMM dd, yyyy"))</p>
            </div>
        </EditorHeaderTemplate>
        
        <EditorFooterTemplate>
            <div class="editor-footer">
                <SfButton Content="Save" CssClass="e-btn-primary" OnClick="@(() => FooterButtonClick(true))"></SfButton>
                <SfButton Content="Cancel" OnClick="@(() => FooterButtonClick(false))"></SfButton>
                <span class="validation-message">@ValidationMessage</span>
            </div>
        </EditorFooterTemplate>
    </ScheduleTemplates>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    SfSchedule<AppointmentData> ScheduleRef;
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    string ValidationMessage = "";
    private bool IsSaveClick = false;

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        }
    };

    public async Task OnPopupClose(PopupCloseEventArgs<AppointmentData> args)
    {
        if (args.Type == PopupType.Editor && args.Data != null && IsSaveClick)
        {
            if (string.IsNullOrEmpty(args.Data.Subject))
            {
                ValidationMessage = "Subject is required";
                return;
            }
            ValidationMessage = "";
        }
    }

    private void FooterButtonClick(bool isSave)
    {
        IsSaveClick = isSave;
        ScheduleRef.CloseEditor();
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

## Resource Options in Editor

Add resource fields to the custom editor template:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.DropDowns

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleGroup Resources="@Resources"></ScheduleGroup>
    <ScheduleTemplates>
        <EditorTemplate>
            <div class="custom-editor">
                <div class="form-group">
                    <label>Subject</label>
                    <input type="text" 
                        value="@((context as AppointmentData)?.Subject ?? "")" 
                        @onchange="@((ChangeEventArgs e) => UpdateField(context as AppointmentData, nameof(AppointmentData.Subject), e.Value))" />
                </div>
                
                <div class="form-group">
                    <label>Resource (Room)</label>
                    <SfDropDownList TValue="int?" TItem="ResourceData" 
                        DataSource="@ResourcesData" 
                        Fields="@(new DropDownListFieldSettings { Text = "Name", Value = "Id" })"
                        Value="@((context as AppointmentData)?.ResourceId)"
                        ValueChange="@((ChangeEventArgs<int?> e) => UpdateField(context as AppointmentData, nameof(AppointmentData.ResourceId), e.Value))">
                    </SfDropDownList>
                </div>
                
                <div class="form-group">
                    <label>Start Time</label>
                    <input type="datetime-local" 
                        value="@((context as AppointmentData)?.StartTime.ToString("yyyy-MM-ddTHH:mm"))" />
                </div>
            </div>
        </EditorTemplate>
    </ScheduleTemplates>
    <ScheduleResources>
        <ScheduleResource TItem="ResourceData" TValue="int?" 
            DataSource="@ResourcesData" 
            Field="ResourceId" 
            Title="Room" 
            Name="Room"
            TextField="Name"
            IdField="Id"
            ColorField="Color">
        </ScheduleResource>
    </ScheduleResources>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    string[] Resources = { "Room" };

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            ResourceId = 1,
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        }
    };

    List<ResourceData> ResourcesData = new List<ResourceData>
    {
        new ResourceData { Id = 1, Name = "Conference Room A", Color = "#1aaa55" },
        new ResourceData { Id = 2, Name = "Conference Room B", Color = "#357cd2" },
        new ResourceData { Id = 3, Name = "Meeting Room", Color = "#e3165b" }
    };

    private void UpdateField(AppointmentData data, string fieldName, object value)
    {
        if (data != null)
        {
            var property = typeof(AppointmentData).GetProperty(fieldName);
            if (property != null)
            {
                property.SetValue(data, value);
            }
        }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public int? ResourceId { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }

    public class ResourceData
    {
        public int Id { get; set; }
        public string Name { get; set; }
        public string Color { get; set; }
    }
}
```

## Recurrence Options in Editor

Add recurrence pattern fields to the custom editor template:

```cshtml
@using Syncfusion.Blazor.Schedule
@using Syncfusion.Blazor.DropDowns
@using Syncfusion.Blazor.Calendars
@using Syncfusion.Blazor.Inputs

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleTemplates>
        <EditorTemplate>
            <div class="custom-editor">
                <div class="form-group">
                    <label>Subject</label>
                    <SfTextBox Value="@((context as AppointmentData)?.Subject)"></SfTextBox>
                </div>
                
                <div class="form-group">
                    <label>Is Recurring</label>
                    <input type="checkbox" 
                        @onchange="@((ChangeEventArgs e) => UpdateRecurrence(context as AppointmentData, (bool)e.Value))" />
                </div>
                
                @if ((context as AppointmentData)?.RecurrenceRule != null)
                {
                    <div class="form-group">
                        <label>Recurrence Pattern</label>
                        <SfDropDownList TValue="string" TItem="RecurrencePattern"
                            DataSource="@RecurrencePatterns"
                            Fields="@(new DropDownListFieldSettings { Text = "Text", Value = "Value" })"
                            Value="@GetRecurrencePattern((context as AppointmentData)?.RecurrenceRule)"
                            ValueChange="@((ChangeEventArgs<string> e) => UpdateField(context as AppointmentData, nameof(AppointmentData.RecurrenceRule), e.Value))">
                        </SfDropDownList>
                    </div>
                    
                    <div class="form-group">
                        <label>Recurrence End Date</label>
                        <SfDatePicker TValue="DateTime?" 
                            Value="@GetRecurrenceEndDate((context as AppointmentData)?.RecurrenceRule)">
                        </SfDatePicker>
                    </div>
                }
                
                <div class="form-group">
                    <label>Start Time</label>
                    <SfDateTimePicker TValue="DateTime" 
                        Value="@((context as AppointmentData)?.StartTime)">
                    </SfDateTimePicker>
                </div>
            </div>
        </EditorTemplate>
    </ScheduleTemplates>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Daily Standup", 
            RecurrenceRule = "FREQ=DAILY;COUNT=10",
            StartTime = new DateTime(2026, 3, 24, 9, 0, 0), 
            EndTime = new DateTime(2026, 3, 24, 9, 30, 0) 
        }
    };

    List<RecurrencePattern> RecurrencePatterns = new List<RecurrencePattern>
    {
        new RecurrencePattern { Text = "Daily", Value = "FREQ=DAILY;COUNT=10" },
        new RecurrencePattern { Text = "Weekly", Value = "FREQ=WEEKLY;COUNT=10" },
        new RecurrencePattern { Text = "Monthly", Value = "FREQ=MONTHLY;COUNT=10" },
        new RecurrencePattern { Text = "Yearly", Value = "FREQ=YEARLY;COUNT=10" }
    };

    private void UpdateRecurrence(AppointmentData data, bool isRecurring)
    {
        if (data != null)
        {
            if (isRecurring)
            {
                data.RecurrenceRule = "FREQ=DAILY;COUNT=10";
            }
            else
            {
                data.RecurrenceRule = null;
            }
        }
    }

    private void UpdateField(AppointmentData data, string fieldName, object value)
    {
        if (data != null)
        {
            var property = typeof(AppointmentData).GetProperty(fieldName);
            if (property != null)
            {
                property.SetValue(data, value);
            }
        }
    }

    private string GetRecurrencePattern(string recurrenceRule)
    {
        if (string.IsNullOrEmpty(recurrenceRule))
            return null;
        
        if (recurrenceRule.Contains("FREQ=DAILY"))
            return "FREQ=DAILY;COUNT=10";
        if (recurrenceRule.Contains("FREQ=WEEKLY"))
            return "FREQ=WEEKLY;COUNT=10";
        if (recurrenceRule.Contains("FREQ=MONTHLY"))
            return "FREQ=MONTHLY;COUNT=10";
        
        return recurrenceRule;
    }

    private DateTime? GetRecurrenceEndDate(string recurrenceRule)
    {
        if (string.IsNullOrEmpty(recurrenceRule) || !recurrenceRule.Contains("UNTIL="))
            return null;
        
        var untilPart = recurrenceRule.Split(';')
            .FirstOrDefault(x => x.StartsWith("UNTIL="));
        
        if (DateTime.TryParse(untilPart?.Replace("UNTIL=", ""), out var date))
            return date;
        
        return null;
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
        public string RecurrenceRule { get; set; }
    }

    public class RecurrencePattern
    {
        public string Text { get; set; }
        public string Value { get; set; }
    }
}
```

## Editor Template Validation

Apply DataAnnotations-based validation to editor template fields:

```cshtml
@using Syncfusion.Blazor.Schedule
@using System.ComponentModel.DataAnnotations

<SfSchedule TValue="AppointmentData" Height="650px" @bind-SelectedDate="@CurrentDate">
    <ScheduleTemplates>
        <EditorTemplate>
            <div class="custom-editor">
                <div class="form-group">
                    <label>Subject (Required)</label>
                    <SfTextBox Value="@((context as AppointmentData)?.Subject)"></SfTextBox>
                    @if (GetValidationErrors("Subject").Any())
                    {
                        <span class="validation-error">@string.Join(", ", GetValidationErrors("Subject"))</span>
                    }
                </div>
                
                <div class="form-group">
                    <label>Email (Valid Email)</label>
                    <SfTextBox Type="InputType.Email" Value="@((context as AppointmentData)?.Email)"></SfTextBox>
                    @if (GetValidationErrors("Email").Any())
                    {
                        <span class="validation-error">@string.Join(", ", GetValidationErrors("Email"))</span>
                    }
                </div>
                
                <div class="form-group">
                    <label>Event Type (Required)</label>
                    <SfDropDownList TValue="string" TItem="EventTypeOption"
                        DataSource="@EventTypeOptions"
                        Fields="@(new DropDownListFieldSettings { Text = "Text", Value = "Value" })"
                        Value="@((context as AppointmentData)?.EventType)">
                    </SfDropDownList>
                    @if (GetValidationErrors("EventType").Any())
                    {
                        <span class="validation-error">@string.Join(", ", GetValidationErrors("EventType"))</span>
                    }
                </div>
            </div>
        </EditorTemplate>
    </ScheduleTemplates>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);

    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            Email = "user@company.com",
            EventType = "Meeting",
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        }
    };

    List<EventTypeOption> EventTypeOptions = new List<EventTypeOption>
    {
        new EventTypeOption { Text = "Meeting", Value = "Meeting" },
        new EventTypeOption { Text = "Conference", Value = "Conference" },
        new EventTypeOption { Text = "Workshop", Value = "Workshop" }
    };

    private List<string> GetValidationErrors(string fieldName)
    {
        // Mock validation - in real scenario, would use Validator class
        var errors = new List<string>();
        
        if (fieldName == "Subject")
        {
            // Check if required
        }
        
        if (fieldName == "Email")
        {
            // Check if valid email format
        }
        
        return errors;
    }

    public class AppointmentData
    {
        [Required(ErrorMessage = "Subject is required")]
        public string Subject { get; set; }

        [EmailAddress(ErrorMessage = "Invalid email address")]
        public string Email { get; set; }

        [Required(ErrorMessage = "Event Type is required")]
        public string EventType { get; set; }

        public int Id { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }

    public class EventTypeOption
    {
        public string Text { get; set; }
        public string Value { get; set; }
    }
}
```

## Quick Info Popups

Quick popups (quick info windows) display when a cell or appointment is single-clicked:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate" ShowQuickInfo="true">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**Quick Popup Behaviors:**
- Single-click cell: Shows quick popup to add quick event
- Single-click appointment: Shows quick popup with event details
- Mobile: Quick popup opens on tap
- Disable with `ShowQuickInfo="false"`

## Quick Popup on Cell

Customize quick popup on cell click:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" @bind-SelectedDate="@CurrentDate" CssClass="quick-popup-custom" Width="100%" Height="650px">
    <ScheduleQuickInfoTemplates TemplateType="TemplateType.Cell">
        <HeaderTemplate>
            <div class="quick-popup-header">
                <h3>Quick Add Event</h3>
            </div>
        </HeaderTemplate>
        
        <ContentTemplate>
            <div class="quick-popup-content">
                <div class="event-field">
                    <label>Event Title:</label>
                    <input type="text" class="event-subject" placeholder="Enter title" />
                </div>
                <div class="event-field">
                    <label>Time: @((context as CellClickEventArgs).StartTime?.ToString("HH:mm")) - @((context as CellClickEventArgs).EndTime?.ToString("HH:mm"))</label>
                </div>
            </div>
        </ContentTemplate>
        
        <FooterTemplate>
            <div class="quick-popup-footer">
                <button class="e-btn e-btn-primary">Add</button>
                <button class="e-btn">Cancel</button>
            </div>
        </FooterTemplate>
    </ScheduleQuickInfoTemplates>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

## Quick Popup on Event

Customize quick popup on event click:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" @bind-SelectedDate="@CurrentDate" CssClass="quick-popup-custom" Width="100%" Height="650px">
    <ScheduleQuickInfoTemplates TemplateType="TemplateType.Event">
        <HeaderTemplate>
            <div class="event-header">
                <h3>@((context as AppointmentData)?.Subject)</h3>
            </div>
        </HeaderTemplate>
        
        <ContentTemplate>
            <div class="event-details">
                <div class="detail-item">
                    <span class="label">Time:</span>
                    <span class="value">@((context as AppointmentData)?.StartTime.ToString("dd MMM yyyy HH:mm")) - @((context as AppointmentData)?.EndTime.ToString("HH:mm"))</span>
                </div>
                <div class="detail-item">
                    <span class="label">Location:</span>
                    <span class="value">@((context as AppointmentData)?.Location ?? "Not specified")</span>
                </div>
                <div class="detail-item">
                    <span class="label">Description:</span>
                    <span class="value">@((context as AppointmentData)?.Description ?? "No description")</span>
                </div>
            </div>
        </ContentTemplate>
        
        <FooterTemplate>
            <div class="event-actions">
                <button class="e-btn e-btn-primary">Edit</button>
                <button class="e-btn e-btn-danger">Delete</button>
            </div>
        </FooterTemplate>
    </ScheduleQuickInfoTemplates>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Team Meeting", 
            Location = "Conference Room A",
            Description = "Weekly team synchronization",
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public string Location { get; set; }
        public string Description { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

## Quick Popup Customization Combinations

Customize both cell and event quick popups with shared template:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" @bind-SelectedDate="@CurrentDate" CssClass="quick-popup-custom" Width="100%" Height="650px">
    <ScheduleQuickInfoTemplates TemplateType="TemplateType.Both">
        <HeaderTemplate>
            @{
                var data = context as AppointmentData;
                var isEvent = data?.Id > 0;
            }
            <div class="quick-header">
                <h3>@(isEvent ? data?.Subject : "New Event")</h3>
            </div>
        </HeaderTemplate>
        
        <ContentTemplate>
            @{
                var cellData = context as CellClickEventArgs;
                var eventData = context as AppointmentData;
            }
            <div class="quick-content">
                @if (eventData != null && eventData.Id > 0)
                {
                    <div class="event-info">
                        <p><strong>@eventData.Subject</strong></p>
                        <p>@eventData.StartTime.ToString("HH:mm") - @eventData.EndTime.ToString("HH:mm")</p>
                        <p>@(eventData.IsAllDay ? "All-day event" : "")</p>
                    </div>
                }
                else
                {
                    <div class="cell-info">
                        <p>@cellData?.StartTime?.ToString("HH:mm")</p>
                    </div>
                }
            </div>
        </ContentTemplate>
        
        <FooterTemplate>
            <div class="quick-footer">
                <button class="e-btn">Done</button>
            </div>
        </FooterTemplate>
    </ScheduleQuickInfoTemplates>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        }
    };

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

## Disabling Quick Popups

Disable quick popups:

```cshtml
@using Syncfusion.Blazor.Schedule

<!-- Method 1: Set ShowQuickInfo to false -->
<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate" ShowQuickInfo="false">
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

<!-- Method 2: Prevent specific popup types -->
<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" OnPopupOpen="@OnPopupOpen"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Day"></ScheduleView>
        <ScheduleView Option="View.Week"></ScheduleView>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData 
        { 
            Id = 1, 
            Subject = "Meeting", 
            StartTime = new DateTime(2026, 3, 24, 9, 30, 0), 
            EndTime = new DateTime(2026, 3, 24, 11, 0, 0) 
        }
    };

    public void OnPopupOpen(PopupOpenEventArgs<AppointmentData> args)
    {
        // Prevent quick popups on cell click
        if (args.Type == PopupType.QuickInfo)
        {
            args.Cancel = true;
        }
        
        // Prevent quick popups on event click
        if (args.Type == PopupType.EditEventInfo)
        {
            args.Cancel = true;
        }
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**Quick Popup Options:**
- `ShowQuickInfo="true"` - Enable quick popups (default)
- `ShowQuickInfo="false"` - Disable all quick popups
- `QuickInfoOnSelectionEnd="false"` - Prevent quick popup at selection end (default)
- `QuickInfoOnSelectionEnd="true"` - Show quick popup after multiple cell selection

## More Events Popup

When multiple appointments fit in a cell, a "more events" indicator appears:

```cshtml
@using Syncfusion.Blazor.Schedule

<SfSchedule TValue="AppointmentData" Height="550px" @bind-SelectedDate="@CurrentDate">
    <ScheduleEvents TValue="AppointmentData" MoreEventsClicked="@OnMoreEventsClicked"></ScheduleEvents>
    <ScheduleEventSettings DataSource="@DataSource"></ScheduleEventSettings>
    <ScheduleViews>
        <ScheduleView Option="View.Month"></ScheduleView>
    </ScheduleViews>
</SfSchedule>

@code {
    DateTime CurrentDate = new DateTime(2026, 3, 24);
    
    List<AppointmentData> DataSource = new List<AppointmentData>
    {
        new AppointmentData { Id = 1, Subject = "Meeting 1", StartTime = new DateTime(2026, 3, 24, 9, 0, 0), EndTime = new DateTime(2026, 3, 24, 10, 0, 0) },
        new AppointmentData { Id = 2, Subject = "Meeting 2", StartTime = new DateTime(2026, 3, 24, 10, 30, 0), EndTime = new DateTime(2026, 3, 24, 11, 30, 0) },
        new AppointmentData { Id = 3, Subject = "Meeting 3", StartTime = new DateTime(2026, 3, 24, 12, 0, 0), EndTime = new DateTime(2026, 3, 24, 13, 0, 0) },
        new AppointmentData { Id = 4, Subject = "Meeting 4", StartTime = new DateTime(2026, 3, 24, 14, 0, 0), EndTime = new DateTime(2026, 3, 24, 15, 0, 0) }
    };

    public void OnMoreEventsClicked(MoreEventsClickedEventArgs args)
    {
        Console.WriteLine($"More events clicked for date: {args.Date}");
        Console.WriteLine($"Events count: {args.Events?.Count}");
    }

    public class AppointmentData
    {
        public int Id { get; set; }
        public string Subject { get; set; }
        public DateTime StartTime { get; set; }
        public DateTime EndTime { get; set; }
        public bool IsAllDay { get; set; }
    }
}
```

**More Events Behavior:**
- Appears when cell content exceeds available height
- Click to view all appointments for that day
- Month view shows "+ more" indicator
- All-day row shows indicator after 3 appointments (day/week views)
- Prevent popup with `MoreEventsClicked` event and `args.Cancel = true`

## Common Patterns

### Quick Popup Watermark Text

By default, quick popup shows "Add Title" watermark. Change by modifying localization resources or use custom templates.

### Multiple Selection Behavior

**AllowMultiRowSelection property:**
- `true` (default) - Allow selecting multiple days by dragging
- `false` - Prevent multi-day selection
- `QuickInfoOnSelectionEnd="true"` - Show quick popup after selection completes

### Editor vs Quick Popup

| Aspect | Editor | Quick Popup |
|--------|--------|------------|
| Trigger | Double-click | Single-click |
| Fields | All event fields | Subject only |
| Mobile | Dialog window | Quick popup |
| Customization | Templates + EditorTemplate | QuickInfoTemplates |
| Close behavior | Manual or auto-save | Auto-close |

### Validation Strategy

1. **Editor validation** - Use ValidationRules in ScheduleField
2. **Template validation** - Use DataAnnotations attributes
3. **Event validation** - Use OnActionBegin event (ActionType checking)
4. **Field validation** - Check args.Data properties
