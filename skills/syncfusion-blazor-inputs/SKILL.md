---
name: syncfusion-blazor-inputs
description: Implement Syncfusion Blazor Input components including FileUpload, TextBox, NumericTextBox, TextArea, Signature, RangeSlider, OtpInput, Rating, InputMask, and ColorPicker. Use this when working with file uploads, text entry, numeric values, multi-line text inputs, signatures, ratings, or color selection. This skill covers input validation, events, data binding, and advanced customization options for all input-related components in Blazor applications.
metadata:
  author: "Syncfusion Inc"
  version: "33.1.44"
  category: "Inputs"
---

# Implementing Syncfusion Blazor FileUpload

This skill covers the Syncfusion Blazor FileUpload component, a robust solution for file handling in Blazor applications. Learn to implement single and multiple file uploads, configure validation rules, enable drag-drop interactions, handle large files with chunked uploads, and leverage comprehensive events for complete upload control and user feedback.

---

## FileUpload

Learn to implement Syncfusion Blazor File Upload component with async/sync uploads, validation, events, customization, and always get immediate file handling with drag-drop or form integration for web and server applications.

### Documentation

#### Getting Started
📄 **Read:** [references/file-upload-getting-started.md](references/file-upload-getting-started.md)
- Installation and NuGet package setup
- Visual Studio/Visual Studio Code setup steps
- Blazor WebAssembly vs Server configuration
- Basic SfUploader component rendering
- CSS and script imports
- Minimal working example

#### Core Configuration
📄 **Read:** [references/file-upload-configuration.md](references/file-upload-configuration.md)
- ID property for component identification
- AllowedExtensions for file type restriction
- AllowMultiple vs single file uploads
- AutoUpload behavior configuration
- SequentialUpload for ordered processing
- DirectoryUpload capability
- Enabled state and component control

#### Upload Methods & Behavior
📄 **Read:** [references/file-upload-file-upload-methods.md](references/file-upload-file-upload-methods.md)
- Synchronous vs asynchronous uploads
- Save URL and Remove URL configuration
- Upload button click handlers
- Automatic vs manual upload triggering
- Upload progress tracking mechanisms
- Backend API requirements

#### Events & Handlers
📄 **Read:** [references/file-upload-events-and-handlers.md](references/file-upload-events-and-handlers.md)
- ValueChange event for direct file access (Blazor Server only, without AsyncSettings)
- FileSelected event for pre-validation
- Created event for initialization logic
- OnFileListRender for custom file display
- OnUploadStart, Success, OnFailure events (use with AsyncSettings)
- Event handler patterns and best practices
- **Important:** ValueChange and UploaderAsyncSettings are mutually exclusive

#### File Validation
📄 **Read:** [references/file-upload-validation.md](references/file-upload-validation.md)
- File type validation strategies
- File size constraints (MinFileSize, MaxFileSize)
- Custom validation functions
- Preventing invalid file uploads
- Error messages and user feedback
- Validation within EditForm

#### Advanced Features
📄 **Read:** [references/file-upload-advanced-features.md](references/file-upload-advanced-features.md)
- Chunked upload for large files
- Pause and resume functionality
- Async/await patterns in event handlers
- MemoryStream processing without disk I/O
- Batch upload operations
- Retry and recovery mechanisms

#### Customization & Styling
📄 **Read:** [references/file-upload-customization.md](references/file-upload-customization.md)
- Custom file list templates
- CSS class customization
- Theme Studio integration
- Custom button styling
- Dark mode support
- Responsive design patterns

#### File Source Options
📄 **Read:** [references/file-upload-file-source-options.md](references/file-upload-file-source-options.md)
- Drag-and-drop upload implementation
- Form integration patterns
- Direct file picker interaction
- Browser file dialog usage
- Directory selection and upload
- Multiple input method combinations
- Accessibility best practices

#### Localization & Accessibility
📄 **Read:** [references/file-upload-localization-accessibility.md](references/file-upload-localization-accessibility.md)
- Multi-language UI support
- Locale configuration options
- Custom text labels
- WCAG 2.1 compliance
- Keyboard navigation implementation
- Screen reader support
- ARIA attributes

#### Platform-Specific Setup
📄 **Read:** [references/file-upload-platform-specific-setup.md](references/file-upload-platform-specific-setup.md)
- Blazor WebAssembly app setup
- Blazor Server app setup
- Blazor Web App (.NET 8+) setup
- MAUI integration
- Different render modes
- Platform-specific considerations

### Quick Start Example

```razor
@using Syncfusion.Blazor.Inputs

<SfUploader AutoUpload="true" AllowedExtensions=".jpg,.jpeg,.png,.pdf">
    <UploaderAsyncSettings 
        SaveUrl="api/upload/save" 
        RemoveUrl="api/upload/remove">
    </UploaderAsyncSettings>
</SfUploader>
```

### Common Patterns

#### Pattern 1: Basic File Upload with Validation (Server Upload)
- Use `AutoUpload="true"` for instant uploads
- Configure `UploaderAsyncSettings` with SaveUrl/RemoveUrl
- Set `AllowedExtensions` to restrict file types
- Listen to `FileSelected` event for pre-validation
- Use `Success`/`OnFailure` events for upload feedback

#### Pattern 2: Direct File Access (Blazor Server Only)
- Use `ValueChange` event to access file content directly
- **Do NOT use** `UploaderAsyncSettings` with ValueChange
- Process files in memory or save to directory
- Best for file preview, Base64 conversion, or direct storage

#### Pattern 3: Multiple File Handling
- Set `AllowMultiple="true"` to allow batch uploads
- Use `SequentialUpload` for ordered processing
- Track progress with upload events
- Display file list with individual progress indicators

#### Pattern 4: Large File Upload with Chunking
- Configure `ChunkSize` in `UploaderAsyncSettings`
- Enable pause/resume with chunk upload
- Implement retry logic for failed chunks
- Show chunk-level progress to user

### Key Props

| Property | Default | Use When |
|----------|---------|----------|
| AutoUpload | true | Upload files immediately after selection |
| AllowMultiple | true | User needs to upload multiple files |
| SequentialUpload | false | Files must upload one at a time |
| AllowedExtensions | "" | Only specific file types allowed |
| DirectoryUpload | false | User can select entire folders |
| MaxFileSize | 28.4 MB | Limiting maximum upload file size |
| MinFileSize | 0 | Setting minimum file size requirement |
| ChunkSize | 0 (disabled) | Enable chunked upload for large files |
| ShowFileList | true | Control visibility of uploaded file list |
| ShowProgressBar | true | Display upload progress indicator |
| Enabled | true | Enable or disable the uploader |
| DropArea | null | Specify custom drop zone CSS selector |
| CssClass | "" | Apply custom CSS classes |
| TabIndex | 0 | Set tab navigation order |
| EnablePersistence | false | Maintain state across page reloads |
| EnableRtl | false | Enable right-to-left layout |


### Common Use Cases

1. **Document Upload**: Resume, PDF, certification file uploads
2. **Image Gallery**: User profile pictures, photo collections
3. **Data Import**: CSV/Excel file imports for data processing
4. **Media Library**: Video, audio file uploads and management
5. **Backup Uploads**: Database backups, configuration files
6. **Report Generation**: Monthly reports, analytics data
7. **Invoice Processing**: Financial document uploads
8. **User Attachments**: Email attachments, message files

### Quick Decision Tree

**User needs file upload functionality**
  ├─ Single file only? → Set `AllowMultiple="false"` + use basic setup
  ├─ Multiple files? 
  │  ├─ All at once? → `AllowMultiple="true"` + `SequentialUpload="false"`
  │  └─ One at a time? → `AllowMultiple="true"` + `SequentialUpload="true"`
  └─ Large files (>100MB)?
     ├─ Enable chunking → Set `ChunkSize` property
     └─ Add pause/resume → Listen to `Paused` and `OnResume` events

---

## TextArea

Learn to implement Syncfusion Blazor TextArea component for multi-line text input with configurable resize modes, row/column sizing, character limits, floating labels, and comprehensive validation. Perfect for comments, descriptions, messages, and any scenario requiring extended text entry with real-time feedback and form integration.

### Documentation

#### Getting Started
📄 **Read:** [references/textarea-getting-started.md](references/textarea-getting-started.md)
- Installation and NuGet package setup
- Basic SfTextArea component setup
- Namespace imports and service registration
- CSS theme configuration
- Minimal working example
- Initial component rendering

#### Configuration Options
📄 **Read:** [references/textarea-configuration.md](references/textarea-configuration.md)
- RowCount and ColumnCount for sizing
- ResizeMode (Vertical, Horizontal, Both, None)
- MaxLength property for character limits
- Placeholder text configuration
- FloatLabelType (Auto, Always, Never)
- ReadOnly and Disabled states
- Width property and responsive sizing
- HTML attributes customization

#### Events and Data Binding
📄 **Read:** [references/textarea-events-binding.md](references/textarea-events-binding.md)
- Value property and two-way binding (@bind-Value)
- ValueChange event for real-time updates
- Focus and Blur events (TextAreaFocusInEventArgs, TextAreaFocusOutEventArgs)
- Input event for keystroke tracking
- Created and Destroyed lifecycle events
- Form validation integration with EditForm
- ValueExpression for validation binding

#### Customization and Styling
📄 **Read:** [references/textarea-customization.md](references/textarea-customization.md)
- CssClass for custom styling
- ShowClearButton for quick text removal
- InputAttributes and HtmlAttributes
- Theme customization with Theme Studio
- Responsive design patterns
- Accessibility features (ARIA, keyboard navigation)
- RTL (Right-to-Left) support with EnableRtl

### Quick Start Example

```razor
@using Syncfusion.Blazor.Inputs

<SfTextArea @bind-Value="@description"
            Placeholder="Enter description..."
            RowCount="5"
            ColumnCount="50"
            MaxLength="500"
            FloatLabelType="FloatLabelType.Auto">
</SfTextArea>

@code {
    private string description = "";
}
```

### Common Patterns

#### Pattern 1: Basic Multi-Line Input
- Use `RowCount` to set visible lines (default: 2)
- Set `Placeholder` for user guidance
- Enable `@bind-Value` for two-way binding
- Apply `MaxLength` for character constraints

#### Pattern 2: Resizable TextArea with Limits
- Set `ResizeMode="Resize.Both"` for user resizing
- Configure `RowCount` and `ColumnCount` for initial size
- Use `MaxLength` to prevent excessive input
- Listen to `ValueChange` for live character counting

#### Pattern 3: Form Integration with Validation
- Wrap in `<EditForm>` with model binding
- Use `@bind-Value` with `ValueExpression`
- Apply `[Required]` or `[StringLength]` attributes
- Display validation messages with `<ValidationMessage>`
- Style invalid state with CSS

#### Pattern 4: Auto-Growing TextArea
- Set `ResizeMode="Resize.Vertical"` for vertical expansion
- Start with minimal `RowCount` (e.g., 3)
- Allow user to expand as needed
- Combine with `MaxLength` for upper bounds

### Key Props

| Property | Default | Use When |
|----------|---------|----------|
| Value | "" | Binding textarea content |
| RowCount | 2 | Setting visible number of rows |
| ColumnCount | 20 | Setting visible number of columns |
| MaxLength | null | Limiting maximum characters |
| ResizeMode | Resize.Both | Controlling user resize behavior |
| Placeholder | "" | Showing hint text when empty |
| FloatLabelType | FloatLabelType.Never | Enabling floating label animation |
| ShowClearButton | false | Adding quick clear functionality |
| ReadOnly | false | Preventing user edits while showing content |
| Disabled | false | Disabling the component entirely |
| Width | "100%" | Setting component width |
| CssClass | "" | Applying custom CSS classes |
| EnableRtl | false | Enabling right-to-left text direction |

### Common Use Cases

1. **Comment Sections**: User feedback, review comments, discussion threads
2. **Form Descriptions**: Product descriptions, bio sections, about fields
3. **Message Composition**: Email bodies, chat messages, note-taking
4. **Code/JSON Input**: Configuration files, script input, data entry
5. **Address Fields**: Multi-line address entry with street, city, etc.
6. **Search Queries**: Complex search inputs with multiple criteria
7. **Customer Support**: Ticket descriptions, issue reporting, help requests
8. **Content Management**: Article drafts, blog post editing, documentation

### Quick Decision Tree

**User needs multi-line text input**
  ├─ Fixed size? → Set `ResizeMode="Resize.None"` + specific `RowCount`
  ├─ User-resizable?
  │  ├─ Vertical only? → `ResizeMode="Resize.Vertical"`
  │  ├─ Horizontal only? → `ResizeMode="Resize.Horizontal"`
  │  └─ Both directions? → `ResizeMode="Resize.Both"`
  ├─ Character limit needed? → Set `MaxLength` property
  └─ Form validation?
     ├─ Use within `<EditForm>`
     └─ Add `ValueExpression` for validation binding

---

## Signature

Learn to implement Syncfusion Blazor Signature component for capturing digital signatures with configurable stroke width, colors, background images, save/load functionality in multiple formats (PNG, JPEG, SVG), and comprehensive event handling. Perfect for e-signatures, document approval workflows, digital consent forms, and any scenario requiring handwritten signature capture with touch and mouse support.

### Documentation

#### Getting Started
📄 **Read:** [references/signature-getting-started.md](references/signature-getting-started.md)
- Installation and NuGet package setup
- Basic SfSignature component setup
- Namespace imports and service registration
- CSS theme configuration
- Canvas rendering and initialization
- Touch and mouse input support
- Minimal working example

#### Drawing Configuration
📄 **Read:** [references/signature-drawing-configuration.md](references/signature-drawing-configuration.md)
- MinStrokeWidth and MaxStrokeWidth for pen thickness
- StrokeColor for ink color customization
- BackgroundColor for canvas background
- BackgroundImage for letterhead/watermark
- Velocity property for stroke smoothness
- Drawing behavior and responsiveness
- Pressure sensitivity simulation

#### Save and Load Signatures
📄 **Read:** [references/signature-save-load.md](references/signature-save-load.md)
- Save() method with format options (PNG, JPEG, SVG)
- SaveWithBackground property configuration
- GetSignature() for Base64 string retrieval
- Load() method for existing signatures
- Clear() method for signature removal
- File format selection and quality settings
- Server integration patterns
- Database storage strategies

#### Event Handling
📄 **Read:** [references/signature-events.md](references/signature-events.md)
- Changed event for stroke tracking
- OnSave event for save operations
- Created event for initialization
- Event argument structure
- Real-time signature validation
- Detecting empty vs filled signatures
- Event-driven workflows

#### Customization and Styling
📄 **Read:** [references/signature-customization.md](references/signature-customization.md)
- Disabled and IsReadOnly states
- HtmlAttributes for custom styling
- Canvas size customization
- Theme integration
- Mobile and touch device optimization
- Accessibility considerations
- Responsive design patterns

### Quick Start Example

```razor
@using Syncfusion.Blazor.Inputs

<div class="signature-container">
    <label>Sign below:</label>
    <SfSignature @ref="signatureRef"
                 StrokeColor="#000000"
                 BackgroundColor="#FFFFFF"
                 MaxStrokeWidth="2.0"
                 MinStrokeWidth="0.5">
    </SfSignature>
    
    <div class="signature-actions">
        <button @onclick="SaveSignature">Save</button>
        <button @onclick="ClearSignature">Clear</button>
    </div>
</div>

@code {
    private SfSignature signatureRef;
    
    private async Task SaveSignature()
    {
        await signatureRef.SaveAsync(SignatureFileType.Png, "signature.png");
    }
    
    private async Task ClearSignature()
    {
        await signatureRef.ClearAsync();
    }
}
```

### Common Patterns

#### Pattern 1: Basic Signature Capture
- Use default stroke settings for natural handwriting feel
- Set `BackgroundColor="#FFFFFF"` for clear canvas
- Provide Clear button for user corrections
- Save as PNG for universal compatibility
- Validate signature is not empty before submission

#### Pattern 2: Document Signing with Letterhead
- Use `BackgroundImage` for company letterhead or form template
- Set `SaveWithBackground="true"` to include background in saved file
- Configure `StrokeColor` to contrast with background
- Save as PNG or JPEG with background embedded
- Ideal for contracts, agreements, official documents

#### Pattern 3: Mobile-Optimized Signature
- Increase stroke width for better touch visibility
- Use larger canvas size for thumb-friendly drawing
- Set `IsReadOnly="false"` only when signature mode active
- Auto-save on signature completion
- Provide clear visual feedback for touch interactions

#### Pattern 4: Multi-Signature Forms
- Use multiple SfSignature components for different signatories
- Track completion state per signature field
- Save each signature with unique identifier
- Combine signatures in final document generation
- Validate all required signatures before form submission

### Key Props

| Property | Default | Use When |
|----------|---------|----------|
| MinStrokeWidth | 0.5 | Setting minimum pen thickness |
| MaxStrokeWidth | 2.0 | Setting maximum pen thickness |
| StrokeColor | "#000000" | Changing ink color |
| BackgroundColor | "#FFFFFF" | Setting canvas background color |
| BackgroundImage | null | Adding letterhead or watermark image |
| Velocity | 0.7 | Controlling stroke smoothness (0-1) |
| SaveWithBackground | true | Including background in saved signature |
| Disabled | false | Disabling signature capture entirely |
| IsReadOnly | false | Preventing signature changes while showing existing |
| EnablePersistence | false | Maintaining signature across page reloads |
| HtmlAttributes | null | Adding custom HTML attributes to wrapper |

### Common Use Cases

1. **E-Signature Capture**: Digital document signing, contract approval, consent forms
2. **Financial Services**: Loan applications, account opening, transaction authorization
3. **Healthcare**: Patient consent forms, HIPAA agreements, medical records
4. **Legal Documents**: Contracts, NDAs, legal agreements, court documents
5. **HR Processes**: Employment contracts, onboarding documents, policy acknowledgments
6. **Delivery Confirmation**: Package delivery signatures, service completion
7. **Check-In Systems**: Visitor logs, attendance tracking, registration forms
8. **Educational**: Test proctoring, form submissions, parent consent

### Quick Decision Tree

**User needs signature capture**
  ├─ Basic signature?
  │  └─ Use default settings + Save as PNG
  ├─ Document with letterhead?
  │  ├─ Set `BackgroundImage` property
  │  └─ Enable `SaveWithBackground="true"`
  ├─ Mobile/touch primary?
  │  ├─ Increase `MaxStrokeWidth` to 3.0+
  │  └─ Use larger canvas dimensions
  ├─ Multiple signers?
  │  ├─ Use multiple SfSignature components
  │  ├─ Track each signature state separately
  │  └─ Save with unique identifiers
  └─ Need specific format?
     ├─ PNG → Universal support, transparency
     ├─ JPEG → Smaller file size, no transparency
     └─ SVG → Vector format, scalable

---

## RangeSlider

Learn to implement Syncfusion Blazor Range Slider component with dual handles for range selection, ticks, tooltips, color ranges, movement limits, and always get immediate two-value selection for price filters, date ranges, temperature zones, or any scenario requiring range input with visual feedback and validation in Blazor applications.

### Documentation

#### Getting Started
📄 **Read:** [references/rangeslider-getting-started.md](references/rangeslider-getting-started.md)
- Installation and NuGet package setup
- Basic SfSlider with Type="SliderType.Range"
- Value binding with arrays for dual handles
- CSS imports and theme configuration
- Namespace imports and service registration
- Minimal working example with range selection

#### Range Configuration
📄 **Read:** [references/rangeslider-range-configuration.md](references/rangeslider-range-configuration.md)
- Min, Max, and Step properties for range bounds
- Type property (SliderType.Range vs Default)
- Two-way value binding with arrays (@bind-Value)
- Custom non-numeric values with CustomValues
- IsImmediateValue for real-time updates
- Value array structure and data types

#### Ticks and Tooltip
📄 **Read:** [references/rangeslider-ticks-and-tooltip.md](references/rangeslider-ticks-and-tooltip.md)
- SliderTicks component configuration
- LargeStep and SmallStep for interval markers
- Tick placement options (Before, After, Both)
- ShowSmallTicks property for granular display
- Format property for tick label customization
- SliderTooltip component setup
- Tooltip visibility modes (Focus, Hover, Always, Auto)
- Tooltip placement and format customization
- Custom tooltip templates

#### Color Ranges and Visual Indication
📄 **Read:** [references/rangeslider-color-ranges-visual.md](references/rangeslider-color-ranges-visual.md)
- SliderColorRanges for visual feedback
- ColorRange components with Start, End, Color
- Multiple color segments for different value zones
- Use cases (temperature zones, price tiers, ratings)
- Color customization and styling
- Accessibility considerations for color choices

#### Limits and Constraints
📄 **Read:** [references/rangeslider-limits-and-constraints.md](references/rangeslider-limits-and-constraints.md)
- SliderLimits configuration for movement restrictions
- MinStart, MinEnd, MaxStart, MaxEnd properties
- Enabled property for limit activation
- StartHandleFixed and EndHandleFixed for locked handles
- Restricting handle movement within bounds
- Use cases (booking date ranges, budget constraints)
- Validation patterns with limits

#### Orientation and Customization
📄 **Read:** [references/rangeslider-orientation-and-customization.md](references/rangeslider-orientation-and-customization.md)
- Orientation property (Horizontal vs Vertical)
- ShowButtons for increment/decrement controls
- Width property for responsive sizing
- EnableAnimation for smooth transitions
- CssClass for custom styling
- EnableRtl for right-to-left language support
- ReadOnly and Enabled states
- Theme customization with Theme Studio

#### Events and Data Binding
📄 **Read:** [references/rangeslider-events-and-binding.md](references/rangeslider-events-and-binding.md)
- SliderEvents component configuration
- ValueChange event callback for range updates
- OnChange vs Changed event timing
- Created event for initialization logic
- Rendered event for post-render operations
- OnTooltipChange for dynamic tooltip content
- OnTicksRender for custom tick label rendering
- Form integration with EditForm
- Validation with EditContext and data annotations

### Quick Start Example

```razor
@using Syncfusion.Blazor.Inputs

<div class="range-slider-container">
    <label>Select Price Range: $@priceRange[0] - $@priceRange[1]</label>
    <SfSlider @bind-Value="@priceRange"
              Type="SliderType.Range"
              Min="0"
              Max="1000"
              Step="10">
        <SliderTicks Placement="Placement.After" LargeStep="200" SmallStep="50" ShowSmallTicks="true"></SliderTicks>
        <SliderTooltip IsVisible="true" ShowOn="TooltipShowOn.Always" Format="C0"></SliderTooltip>
    </SfSlider>
</div>

@code {
    private int[] priceRange = new int[] { 200, 800 };
}
```

### Common Patterns

#### Pattern 1: Basic Range Selection
- Set `Type="SliderType.Range"` for dual handles
- Bind value to int[] or double[] array with two elements
- Configure `Min`, `Max`, and `Step` properties
- Enable tooltip with `IsVisible="true"` for user feedback
- Use `ValueChange` event to capture range updates

#### Pattern 2: Range with Visual Color Zones
- Add `SliderColorRanges` component
- Define multiple `ColorRange` segments (e.g., cold/warm/hot)
- Set colors that provide clear visual distinction
- Use for temperature, ratings, or risk indicators
- Combine with ticks for precise value identification

#### Pattern 3: Constrained Range Selection
- Use `SliderLimits` to restrict handle movement
- Set `MinStart`/`MaxStart` for first handle bounds
- Set `MinEnd`/`MaxEnd` for second handle bounds
- Enable `StartHandleFixed` or `EndHandleFixed` if one handle should be locked
- Ideal for booking systems, budget planning, scheduling

#### Pattern 4: Custom Value Range Selection
- Use `CustomValues` array for non-numeric ranges
- Example: string[] { "XS", "S", "M", "L", "XL", "XXL" }
- Value array uses indices, not actual values
- Display custom labels via tick formatting
- Perfect for size selection, priority levels, skill ratings

### Key Props

| Property | Default | Use When |
|----------|---------|----------|
| Type | SliderType.Default | Set to SliderType.Range for dual handles |
| Value | new int[]{} | Binding range values (must be 2-element array) |
| Min | 0 | Setting minimum selectable value |
| Max | 100 | Setting maximum selectable value |
| Step | 1 | Defining increment/decrement value |
| CustomValues | null | Using non-numeric values (sizes, labels) |
| IsImmediateValue | false | Getting real-time updates during drag |
| ShowButtons | false | Adding increment/decrement buttons |
| Orientation | SliderOrientation.Horizontal | Changing to vertical layout |
| Width | null | Setting component width |
| EnableAnimation | true | Controlling handle animation |
| ReadOnly | false | Preventing user interaction while showing value |
| Enabled | true | Enabling/disabling the entire component |

### Common Use Cases

1. **E-Commerce Price Filters**: Min/max price selection, budget range filtering
2. **Date Range Pickers**: Check-in/check-out dates, event duration, scheduling
3. **Temperature Control**: HVAC systems, oven settings, climate zones
4. **Age Range Selection**: Demographics, target audience, age restrictions
5. **Time Range Selection**: Working hours, availability slots, time windows
6. **Score/Rating Ranges**: Grade filtering, performance metrics, review scores
7. **Financial Planning**: Budget allocation, investment ranges, spending limits
8. **Resource Allocation**: CPU/memory limits, bandwidth throttling, capacity planning

### Quick Decision Tree

**User needs range selection (two values)**
  ├─ Numeric range?
  │  ├─ Set `Type="SliderType.Range"`
  │  ├─ Use int[] or double[] for Value
  │  └─ Configure Min, Max, Step
  ├─ Non-numeric values (sizes, labels)?
  │  ├─ Set `CustomValues` array
  │  ├─ Value array contains indices
  │  └─ Use tick formatting for labels
  ├─ Need visual zones?
  │  ├─ Add `SliderColorRanges` component
  │  └─ Define multiple `ColorRange` segments
  ├─ Restrict movement?
  │  ├─ Use `SliderLimits` component
  │  ├─ Set MinStart/MaxStart/MinEnd/MaxEnd
  │  └─ Enable StartHandleFixed or EndHandleFixed if needed
  ├─ Vertical layout needed?
  │  └─ Set `Orientation="SliderOrientation.Vertical"`
  └─ Real-time updates during drag?
     └─ Set `IsImmediateValue="true"`

---

## OtpInput

Learn to implement Syncfusion Blazor OtpInput (One-Time Password) component for secure verification code entry with configurable length, input types (number, text, password), styling modes (outlined, underlined, filled), automatic focus management, and comprehensive event handling. Perfect for 2FA authentication, email verification, SMS codes, PIN entry, and any scenario requiring secure multi-digit code input with keyboard navigation and accessibility support.

### Documentation

#### Getting Started
📄 **Read:** [references/otpinput-getting-started.md](references/otpinput-getting-started.md)
- Installation and NuGet package setup
- Basic SfOtpInput component setup
- Namespace imports and service registration
- CSS theme configuration
- Length property for OTP digit count
- Value binding and retrieval
- Minimal working example

#### Configuration Options
📄 **Read:** [references/otpinput-configuration.md](references/otpinput-configuration.md)
- Length property for digit count (default: 4)
- Type property (Number, Text, Password)
- Placeholder configuration for empty inputs
- Separator for visual grouping
- AutoFocus for immediate input
- Disabled state management
- ID and HtmlAttributes customization

#### Styling Modes
📄 **Read:** [references/otpinput-styling-modes.md](references/otpinput-styling-modes.md)
- StylingMode options (Outlined, Underlined, Filled)
- TextTransform (None, Lowercase, Uppercase)
- CssClass for custom styling
- Theme customization with Theme Studio
- Responsive design patterns
- Visual states and focus indicators

#### Events and Data Binding
📄 **Read:** [references/otpinput-events-binding.md](references/otpinput-events-binding.md)
- Value property and two-way binding (@bind-Value)
- ValueChanged event callback (use Value property only, NOT @bind-Value)
- OnInput event with OtpInputEventArgs
- OnFocus and OnBlur events
- Created lifecycle event
- Form validation integration
- Real-time verification patterns
- Auto-submit on completion

#### Accessibility
📄 **Read:** [references/otpinput-accessibility.md](references/otpinput-accessibility.md)
- AriaLabels array for individual input fields
- Keyboard navigation (arrows, backspace, delete)
- Screen reader support
- WCAG 2.1 compliance
- Focus management best practices
- Password type accessibility considerations
- Mobile device optimization

### Quick Start Example

```razor
@using Syncfusion.Blazor.Inputs

<div class="otp-container">
    <label>Enter verification code:</label>
    <SfOtpInput @bind-Value="@otpValue"
                Length="6"
                Type="OtpInputType.Number"
                StylingMode="OtpInputStyle.Outlined">
    </SfOtpInput>
    
    @if (!string.IsNullOrEmpty(message))
    {
        <div class="message">@message</div>
    }
</div>

@code {
    private string otpValue = "";
    private string message = "";
    
    protected override void OnParametersSet()
    {
        if (otpValue.Length == 6)
        {
            message = "Verifying code...";
            // Call verification API
        }
    }
}
```

### Common Patterns

#### Pattern 1: Basic OTP Verification (6-digit numeric)
- Set `Length="6"` for standard OTP length
- Use `Type="OtpInputType.Number"` for numeric-only input
- Enable `AutoFocus="true"` for immediate input
- Use `@bind-Value` for two-way binding (simplest approach)
- Validate and verify OTP on server

#### Pattern 2: Email/SMS Verification Code
- Configure `Length="4"` or `Length="6"` based on service
- Use `Type="OtpInputType.Number"` for numeric codes
- Set `StylingMode="OtpInputStyle.Underlined"` for clean look
- Auto-focus first input on page load
- Show countdown timer for code expiration
- Provide "Resend code" functionality

#### Pattern 3: Secure PIN Entry
- Use `Type="OtpInputType.Password"` to mask input
- Set `Length="4"` or `Length="6"` for PIN length
- Apply `StylingMode="OtpInputStyle.Filled"` for modern look
- Implement rate limiting for security
- Clear input on failed attempts
- Show visual feedback for validation

#### Pattern 4: Alphanumeric Verification (with separators)
- Set `Type="OtpInputType.Text"` for letters and numbers
- Use `TextTransform="TextTransform.Uppercase"` for readability
- Configure `Separator="-"` to visually group digits
- Example: ABC-123-XYZ pattern
- Set `Length="9"` (including separator positions)
- Useful for activation codes, license keys

### Key Props

| Property | Default | Use When |
|----------|---------|----------|
| Value | "" | Binding OTP value (two-way with @bind-Value) |
| Length | 4 | Setting number of OTP input fields |
| Type | OtpInputType.Number | Defining input type (Number, Text, Password) |
| StylingMode | OtpInputStyle.Outlined | Choosing visual style (Outlined, Underlined, Filled) |
| Placeholder | "" | Showing hint text in empty fields |
| Separator | "" | Adding visual separator between groups |
| TextTransform | TextTransform.None | Transforming text (None, Lowercase, Uppercase) |
| AutoFocus | false | Auto-focusing first input on load |
| Disabled | false | Disabling all input fields |
| CssClass | "" | Applying custom CSS classes |
| AriaLabels | null | Setting custom ARIA labels for each input |
| HtmlAttributes | null | Adding custom HTML attributes |

### Common Use Cases

1. **Two-Factor Authentication (2FA)**: Login security, account verification, multi-factor authentication
2. **Email Verification**: Account activation, email confirmation, newsletter signup
3. **SMS Verification**: Phone number verification, mobile app login, transaction confirmation
4. **Password Reset**: Secure password recovery, account access restoration
5. **Transaction Verification**: Banking transactions, payment confirmation, fund transfers
6. **Access Control**: Building entry codes, secure area access, temporary access codes
7. **Device Pairing**: Bluetooth pairing codes, smart device setup, IoT device linking
8. **Activation Codes**: Software licenses, product activation, subscription validation

### Quick Decision Tree

**User needs OTP/verification code input**
  ├─ Numeric only?
  │  ├─ Set `Type="OtpInputType.Number"`
  │  └─ Use `Length="4"` or `Length="6"`
  ├─ Need to hide input (PIN)?
  │  ├─ Set `Type="OtpInputType.Password"`
  │  └─ Apply security best practices
  ├─ Alphanumeric codes?
  │  ├─ Set `Type="OtpInputType.Text"`
  │  ├─ Use `TextTransform="TextTransform.Uppercase"`
  │  └─ Consider `Separator` for readability
  ├─ Auto-submit when complete?
  │  ├─ Listen to `ValueChanged` event
  │  ├─ Check if `value.Length == Length`
  │  └─ Call verification API automatically
  ├─ Custom styling needed?
  │  ├─ Outlined → `StylingMode="OtpInputStyle.Outlined"` (default)
  │  ├─ Underlined → `StylingMode="OtpInputStyle.Underlined"`
  │  └─ Filled → `StylingMode="OtpInputStyle.Filled"`
  └─ Accessibility important?
     ├─ Set `AriaLabels` array for screen readers
     └─ Enable `AutoFocus` for keyboard users

---

## Rating

Learn to implement Syncfusion Blazor Rating component for intuitive rating and feedback collection with configurable precision modes (full, half, quarter, exact), custom icons and templates, label and tooltip support, comprehensive event handling, and accessibility features. Perfect for product reviews, skill assessments, satisfaction surveys, quality ratings, and any scenario requiring user feedback through star ratings or custom iconography with keyboard navigation and form integration.

### Documentation

#### Getting Started
📄 **Read:** [references/rating-getting-started.md](references/rating-getting-started.md)
- Installation and NuGet package setup
- Basic SfRating component setup
- Namespace imports and service registration
- CSS theme configuration
- ItemsCount property for rating scale
- Value binding and retrieval
- Minimal working example
- Basic 5-star rating implementation

#### Precision and Values
📄 **Read:** [references/rating-precision-and-values.md](references/rating-precision-and-values.md)
- Precision property (Full, Half, Quarter, Exact)
- Full precision for whole numbers only
- Half precision for 0.5 increments
- Quarter precision for 0.25 increments
- Exact precision for decimal values
- Min property for minimum rating value
- AllowReset for clearing ratings
- EnableSingleSelection for single-item mode
- Value calculations and display

#### Labels and Tooltips
📄 **Read:** [references/rating-labels-and-tooltips.md](references/rating-labels-and-tooltips.md)
- ShowLabel property for label display
- LabelPosition (Top, Bottom, Left, Right)
- LabelTemplate for custom label formatting
- ShowTooltip property for hover tooltips
- TooltipTemplate for custom tooltip content
- Dynamic label updates based on value
- Contextual feedback patterns

#### Templates and Customization
📄 **Read:** [references/rating-templates-customization.md](references/rating-templates-customization.md)
- EmptyTemplate for unselected items
- FullTemplate for selected items
- RatingItemContext for template data
- Custom icon implementation (hearts, thumbs, emojis)
- SVG and icon font integration
- CssClass for custom styling
- EnableAnimation property
- Theme customization and responsive design

#### Events and States
📄 **Read:** [references/rating-events-and-states.md](references/rating-events-and-states.md)
- ValueChanged event for rating updates
- OnItemHover event with RatingHoverEventArgs
- Created lifecycle event
- Form validation integration
- ReadOnly state for display-only ratings
- Disabled state management
- Visible property control
- Keyboard navigation support
- Accessibility features and ARIA attributes

### Quick Start Example

```razor
@using Syncfusion.Blazor.Inputs

<div class="rating-container">
    <label>Rate your experience:</label>
    <SfRating @bind-Value="@userRating"
              ItemsCount="5"
              Precision="PrecisionType.Full"
              ShowLabel="true">
    </SfRating>
    
    @if (userRating > 0)
    {
        <p>You rated: @userRating / 5 stars</p>
    }
</div>

@code {
    private double userRating = 0;
}
```

### Common Patterns

#### Pattern 1: Basic 5-Star Product Rating
- Use default `ItemsCount="5"` for standard rating
- Set `Precision="PrecisionType.Full"` for whole stars only
- Enable `@bind-Value` for two-way data binding
- Show `ShowLabel="true"` to display rating value
- Position label with `LabelPosition="LabelPosition.Right"`
- Use `ValueChanged` event for auto-submit

#### Pattern 2: Half-Star Rating with Hover Feedback
- Set `Precision="PrecisionType.Half"` for 0.5 increments
- Enable `ShowTooltip="true"` for hover feedback
- Use `OnItemHover` event for preview
- Display average ratings with `ReadOnly="true"`
- Show rating count in custom label template
- Implement real-time feedback messages

#### Pattern 3: Custom Icon Templates (Hearts, Thumbs, Emojis)
- Define `EmptyTemplate` for unselected state
- Define `FullTemplate` for selected state
- Use `RatingItemContext` for item-specific rendering
- Implement custom icons (♥, 👍, 😊, etc.)
- Apply `CssClass` for custom colors and sizing
- Enable `EnableAnimation="true"` for smooth transitions

#### Pattern 4: Multi-Category Rating Form
- Create multiple `SfRating` components
- Different `ItemsCount` per category if needed
- Combine with `EditForm` for validation
- Calculate overall rating average
- Track completion with event handlers
- Enable submit only when all ratings complete

### Key Props

| Property | Default | Use When |
|----------|---------|----------|
| Value | 0 | Binding rating value (two-way with @bind-Value) |
| ItemsCount | 5 | Setting number of rating items (stars) |
| Precision | PrecisionType.Full | Defining rating granularity (Full, Half, Quarter, Exact) |
| ShowLabel | false | Displaying rating value as text |
| LabelPosition | LabelPosition.Right | Positioning label (Top, Bottom, Left, Right) |
| ShowTooltip | false | Enabling hover tooltips |
| AllowReset | true | Allowing users to clear their rating |
| EnableSingleSelection | false | Single item selection mode (thumbs up/down) |
| Min | null | Setting minimum rating value |
| ReadOnly | false | Display-only mode for showing ratings |
| Disabled | false | Disabling all interactions |
| EnableAnimation | true | Enabling smooth transitions |
| Visible | true | Controlling component visibility |
| EmptyTemplate | null | Custom template for unselected items |
| FullTemplate | null | Custom template for selected items |
| LabelTemplate | null | Custom label content |
| TooltipTemplate | null | Custom tooltip content |
| CssClass | "" | Applying custom CSS classes |

### Common Use Cases

1. **Product Reviews**: E-commerce ratings, marketplace feedback, customer reviews, product quality assessment
2. **Service Quality**: Restaurant ratings, hotel reviews, delivery service feedback, support satisfaction
3. **Skill Assessments**: Employee evaluations, competency ratings, training feedback, proficiency levels
4. **Content Rating**: Article feedback, video ratings, course reviews, documentation usefulness
5. **User Experience**: App store ratings, feature satisfaction, usability scores, NPS surveys
6. **Performance Reviews**: Team member evaluations, project ratings, goal achievements, KPI tracking
7. **Survey Responses**: Satisfaction surveys, opinion polls, feedback forms, questionnaires
8. **Quality Control**: Inspection ratings, compliance scoring, audit results, quality metrics
9. **Entertainment**: Movie ratings, music reviews, book ratings, game scores
10. **Sentiment Analysis**: Customer sentiment, brand perception, mood tracking, emotional feedback

### Quick Decision Tree

**User needs rating/feedback functionality**
  ├─ Display existing rating (read-only)?
  │  ├─ Set `ReadOnly="true"`
  │  ├─ Use `Precision="PrecisionType.Exact"` for average ratings
  │  └─ Show `ShowLabel="true"` with review count
  ├─ Collect new rating from user?
  │  ├─ Whole stars only? → `Precision="PrecisionType.Full"`
  │  ├─ Half stars? → `Precision="PrecisionType.Half"`
  │  ├─ Quarter stars? → `Precision="PrecisionType.Quarter"`
  │  └─ Any decimal? → `Precision="PrecisionType.Exact"`
  ├─ Custom rating scale?
  │  ├─ 3-point scale → `ItemsCount="3"`
  │  ├─ 7-point scale → `ItemsCount="7"`
  │  └─ 10-point scale → `ItemsCount="10"`
  ├─ Need custom icons instead of stars?
  │  ├─ Hearts → Define `EmptyTemplate` and `FullTemplate` with ♥
  │  ├─ Thumbs → Use 👍/👎 icons
  │  ├─ Emojis → Use 😞😐🙂😊🤩 progression
  │  └─ Custom SVG → Render SVG in templates
  ├─ Thumbs up/down only?
  │  ├─ Set `EnableSingleSelection="true"`
  │  ├─ Use `ItemsCount="1"` or `ItemsCount="2"`
  │  └─ Custom templates with thumb icons
  ├─ Form integration needed?
  │  ├─ Use within `<EditForm>`
  │  ├─ Add validation attributes
  │  └─ Listen to `ValueChanged` for validation
  ├─ Show rating feedback?
  │  ├─ Enable `ShowLabel="true"` for numeric display
  │  ├─ Use `LabelTemplate` for custom messages
  │  ├─ Enable `ShowTooltip="true"` for hover feedback
  │  └─ Use `TooltipTemplate` for contextual messages
  └─ Accessibility requirements?
     ├─ Ensure keyboard navigation works (arrow keys)
     ├─ Test screen reader compatibility
     └─ Provide clear ARIA labels

---

## InputMask

Learn to implement Syncfusion Blazor InputMask (MaskedTextBox) component for formatted text input with predefined patterns, custom mask rules, prompt characters, and always get immediate masked input for phone numbers, SSN, credit cards, dates, ZIP codes, license plates, or any scenario requiring structured text entry with format validation, literal preservation, and comprehensive event handling in Blazor applications.

### Documentation

#### Getting Started
📄 **Read:** [references/inputmask-getting-started.md](references/inputmask-getting-started.md)
- Installation and NuGet package setup
- Basic SfMaskedTextBox component setup
- Namespace imports and service registration
- CSS theme configuration
- Simple mask examples (phone, date, SSN)
- Value binding basics
- Minimal working example

#### Mask Patterns and Configuration
📄 **Read:** [references/inputmask-mask-patterns.md](references/inputmask-mask-patterns.md)
- Standard mask characters (0, 9, L, ?, A, &, C, #)
- Predefined patterns (phone, SSN, ZIP, date, time)
- Mask property configuration
- Literal characters in masks
- Optional vs required positions
- Complex pattern examples
- International format patterns
- Pattern validation rules

#### Custom Characters and Formatting
📄 **Read:** [references/inputmask-custom-characters.md](references/inputmask-custom-characters.md)
- CustomCharacters dictionary usage
- Creating custom mask rules
- Regex-based character validation
- Advanced pattern customization
- Use cases (license plates, product codes, etc.)
- Combining custom with standard masks
- Best practices for custom rules

#### Prompt Character Configuration
📄 **Read:** [references/inputmask-prompt-configuration.md](references/inputmask-prompt-configuration.md)
- PromptChar property (default: _)
- PromptPlaceholder for display
- EnableLiterals for value inclusion
- Placeholder vs PromptChar difference
- Visual feedback configuration
- User experience considerations
- Examples with different configurations

#### Events and Data Binding
📄 **Read:** [references/inputmask-events-binding.md](references/inputmask-events-binding.md)
- Value property and @bind-Value
- ValueChange event (MaskChangeEventArgs)
- ValueChanged event callback
- Focus and Blur events (MaskFocusEventArgs, MaskBlurEventArgs)
- OnChange vs OnInput vs ValueChange timing
- EditForm integration with EditContext
- ValueExpression for validation
- Real-time validation patterns
- Event handler best practices

#### Styling and Accessibility
📄 **Read:** [references/inputmask-styling-accessibility.md](references/inputmask-styling-accessibility.md)
- FloatLabelType configuration
- ShowClearButton functionality
- CssClass for custom styling
- Width and responsive sizing
- Enabled vs Readonly states
- Theme customization with Theme Studio
- WCAG 2.1 compliance
- Keyboard navigation
- ARIA attributes
- Screen reader support
- RTL (EnableRtl) support
- HtmlAttributes and InputAttributes
- Accessibility best practices

### Quick Start Example

```razor
@using Syncfusion.Blazor.Inputs

<div class="input-mask-container">
    <label>Phone Number:</label>
    <SfMaskedTextBox @bind-Value="@phoneNumber"
                     Mask="(000) 000-0000"
                     Placeholder="Enter phone number"
                     FloatLabelType="FloatLabelType.Auto">
    </SfMaskedTextBox>
    
    <label>Social Security Number:</label>
    <SfMaskedTextBox @bind-Value="@ssn"
                     Mask="000-00-0000"
                     PromptChar="#"
                     Placeholder="Enter SSN">
    </SfMaskedTextBox>
</div>

@code {
    private string phoneNumber = "";
    private string ssn = "";
}
```

### Common Patterns

#### Pattern 1: Phone Number Input (US Format)
- Use mask `"(000) 000-0000"` for US phone numbers
- Set `PromptChar="_"` for clear visual feedback
- Enable `@bind-Value` for two-way binding
- Use `FloatLabelType.Auto` for modern UX
- Configure `Placeholder` for user guidance
- Set `EnableLiterals="false"` to exclude parentheses/dashes from value

#### Pattern 2: Social Security Number (SSN)
- Use mask `"000-00-0000"` for SSN format
- Set `PromptChar="#"` or `PromptChar="*"` for privacy
- Apply `Readonly="false"` for input, `Readonly="true"` for display
- Use `ShowClearButton="true"` for quick reset
- Implement validation with `ValueChange` event
- Consider masking display value for security

#### Pattern 3: Credit Card Number with Separators
- Use mask `"0000 0000 0000 0000"` for 16-digit cards
- Set `EnableLiterals="false"` to get digits only
- Listen to `ValueChange` to detect card type (Visa, MC, Amex)
- Show card brand icon based on first digits
- Apply real-time Luhn algorithm validation
- Use `PromptChar="X"` for clear empty positions

#### Pattern 4: Date Input with Custom Format
- Use mask `"00/00/0000"` for MM/DD/YYYY format
- Or `"00-00-0000"` for DD-MM-YYYY format
- Set `Placeholder="MM/DD/YYYY"` for format guidance
- Implement custom validation for valid dates
- Use `ValueChange` to validate day/month ranges
- Consider using DatePicker component for complex date selection

#### Pattern 5: Custom Product Code Format
- Define `CustomCharacters` dictionary for special rules
- Example: `@(new Dictionary<string, string> { { "P", "[A-Z]" }, { "N", "[0-9]" } })`
- Use mask like `"PPP-NNN-NNN"` for ABC-123-456 format
- Combine standard and custom characters
- Apply `TextTransform` for uppercase conversion
- Validate against business rules in events

#### Pattern 6: International Phone with Custom Characters
- Use `CustomCharacters` for country-specific patterns
- Example: `"+00 (000) 000-0000"` with country code
- Set `EnableLiterals="true"` to include + and formatting
- Display country flag based on code
- Implement dynamic mask switching per country
- Support multiple international formats

### Key Props

| Property | Default | Use When |
|----------|---------|----------|
| Mask | "" | Defining input format pattern (required) |
| Value | "" | Binding masked input value |
| PromptChar | '_' | Setting placeholder character for empty positions |
| PromptPlaceholder | null | Overriding prompt character in placeholder mode |
| EnableLiterals | true | Include/exclude literal characters in value |
| CustomCharacters | null | Defining custom mask rules with regex |
| Placeholder | "" | Showing hint text when empty |
| FloatLabelType | FloatLabelType.Never | Enabling floating label animation |
| ShowClearButton | false | Adding quick clear functionality |
| Readonly | false | Preventing edits while showing masked value |
| Enabled | true | Enabling/disabling the component |
| Width | "" | Setting component width |
| CssClass | "" | Applying custom CSS classes |
| EnableRtl | false | Enabling right-to-left text direction |
| HtmlAttributes | null | Adding custom HTML attributes to wrapper |
| InputAttributes | null | Adding custom attributes to input element |
| TabIndex | 0 | Setting tab navigation order |

### Common Use Cases

1. **Contact Information**: Phone numbers, fax numbers, mobile numbers with country codes
2. **Personal Identification**: SSN, tax ID, national ID, passport numbers, driver's license
3. **Financial Data**: Credit card numbers, bank account numbers, routing numbers, CVV codes
4. **Dates and Times**: Custom date formats, time entry, date ranges with specific patterns
5. **Postal Codes**: ZIP codes (US), postal codes (international), area codes with formatting
6. **Product Identification**: SKU numbers, serial numbers, product codes, barcode entry
7. **License Numbers**: Vehicle plates, software licenses, registration numbers with patterns
8. **Network Information**: IP addresses, MAC addresses, subnet masks with dot notation
9. **Version Numbers**: Software versions (1.2.3), build numbers with custom formats
10. **Custom Codes**: Voucher codes, promo codes, tracking numbers with specific patterns

### Quick Decision Tree

**User needs formatted text input with pattern**
  ├─ Standard formats?
  │  ├─ US Phone → `Mask="(000) 000-0000"`
  │  ├─ SSN → `Mask="000-00-0000"`
  │  ├─ ZIP Code → `Mask="00000"` or `Mask="00000-0000"`
  │  ├─ Credit Card → `Mask="0000 0000 0000 0000"`
  │  ├─ Date → `Mask="00/00/0000"` (MM/DD/YYYY)
  │  └─ Time → `Mask="00:00"` (HH:MM)
  ├─ Need digits only in value?
  │  ├─ Set `EnableLiterals="false"`
  │  └─ Mask literals will be excluded from Value
  ├─ Include formatting in value?
  │  ├─ Set `EnableLiterals="true"` (default)
  │  └─ Value will include parentheses, dashes, etc.
  ├─ Custom pattern (not standard)?
  │  ├─ Use `CustomCharacters` dictionary
  │  ├─ Define regex for each custom character
  │  ├─ Example: { "P", "[A-Z]" } for uppercase letters
  │  └─ Combine in mask: `"PPP-000-NNN"`
  ├─ Privacy/security concerns?
  │  ├─ Set `PromptChar="*"` or `PromptChar="#"`
  │  ├─ Use `Readonly="true"` for display-only masked data
  │  └─ Implement additional masking on display if needed
  ├─ International format support?
  │  ├─ Use `CustomCharacters` for flexible patterns
  │  ├─ Implement dynamic mask switching based on locale
  │  └─ Consider country/region selection
  ├─ Form validation needed?
  │  ├─ Use within `<EditForm>` with model binding
  │  ├─ Add `ValueExpression` for validation
  │  ├─ Apply data annotations ([Required], [StringLength])
  │  ├─ Listen to `ValueChange` for real-time validation
  │  └─ Implement custom validation logic (Luhn, date ranges)
  ├─ Enhance user experience?
  │  ├─ Set `FloatLabelType="FloatLabelType.Auto"`
  │  ├─ Enable `ShowClearButton="true"` for quick reset
  │  ├─ Use meaningful `Placeholder` text
  │  └─ Provide visual feedback on validation
  └─ Accessibility important?
     ├─ Ensure keyboard navigation works
     ├─ Set proper ARIA labels via `HtmlAttributes`
     ├─ Test screen reader compatibility
     └─ Provide clear format instructions in label/placeholder

---

## ColorPicker

Learn to implement Syncfusion Blazor ColorPicker component for intuitive color selection with picker and palette modes, opacity control, preset color collections, inline or popup display, mode switching, and always get immediate color input for themes, UI customization, design tools, data visualization, branding, or any scenario requiring color selection with hex/rgba values, recent colors tracking, and comprehensive event handling in Blazor applications.

### Documentation

#### Getting Started
📄 **Read:** [references/colorpicker-getting-started.md](references/colorpicker-getting-started.md)
- Installation and NuGet package setup
- Basic SfColorPicker component setup
- Namespace imports and service registration
- CSS theme configuration
- Value binding with color formats
- Mode overview (Picker vs Palette)
- Minimal working example

#### Modes and Configuration
📄 **Read:** [references/colorpicker-modes-configuration.md](references/colorpicker-modes-configuration.md)
- ColorPickerMode (Picker vs Palette)
- Picker mode for gradient color selection
- Palette mode for predefined color grids
- Inline vs Popup display options
- ModeSwitcher for user mode selection
- Columns configuration for palette layout
- ShowButtons for Apply/Cancel actions
- Mode-specific behaviors and use cases

#### Preset Colors and Palettes
📄 **Read:** [references/colorpicker-presets-colors.md](references/colorpicker-presets-colors.md)
- PresetColors dictionary configuration
- Custom color palette groups
- Default preset color collections
- ShowRecentColors functionality
- NoColor option for "no selection"
- Organizing preset categories
- Business and design use cases

#### Opacity and Value Formats
📄 **Read:** [references/colorpicker-opacity-values.md](references/colorpicker-opacity-values.md)
- EnableOpacity for alpha channel control
- Opacity slider configuration
- Value formats (hex, rgba, hsva)
- Reading and setting opacity values
- Transparent color handling
- Use cases (overlays, highlights, transparency)

#### Events and Data Binding
📄 **Read:** [references/colorpicker-events-binding.md](references/colorpicker-events-binding.md)
- Value property and two-way binding (@bind-Value)
- ValueChanged event callback
- Selected event with ColorPickerEventArgs
- OnOpen and OnClose events for popup control
- ModeSwitched event handling
- OnTileRender for custom palette tiles
- Form integration with EditForm
- Real-time color preview patterns

#### Customization and Accessibility
📄 **Read:** [references/colorpicker-customization-accessibility.md](references/colorpicker-customization-accessibility.md)
- ShowButtons configuration
- CssClass for custom styling
- Disabled and EnableRtl states
- HtmlAttributes customization
- Keyboard navigation support
- ARIA attributes for accessibility
- WCAG 2.1 compliance
- Theme customization with Theme Studio
- Responsive design patterns

### Quick Start Example

```razor
@using Syncfusion.Blazor.Inputs

<div class="colorpicker-container">
    <label>Choose a color:</label>
    <SfColorPicker @bind-Value="@selectedColor"
                   Mode="ColorPickerMode.Palette"
                   ShowButtons="true">
    </SfColorPicker>
    
    <div class="color-preview" style="background-color: @selectedColor;">
        Selected: @selectedColor
    </div>
</div>

@code {
    private string selectedColor = "#008000";
}
```

### Common Patterns

#### Pattern 1: Basic Palette Color Picker
- Use `Mode="ColorPickerMode.Palette"` for predefined color grid
- Enable `@bind-Value` for two-way binding
- Set `ShowButtons="true"` for Apply/Cancel actions
- Use default PresetColors or customize
- Bind to string variable for hex values
- Display preview with selected color

#### Pattern 2: Advanced Gradient Picker with Opacity
- Set `Mode="ColorPickerMode.Picker"` for gradient selector
- Enable `EnableOpacity="true"` for alpha channel
- Use `ModeSwitcher="true"` to allow mode switching
- Display rgba value for transparency
- Ideal for design tools, graphics editors
- Show real-time preview with opacity

#### Pattern 3: Inline Color Selector (No Popup)
- Set `Inline="true"` for always-visible picker
- Use for color customization panels
- No popup trigger, immediate display
- Works with both Picker and Palette modes
- Combine with `ShowButtons="false"` for instant selection
- Perfect for settings panels, theme builders

#### Pattern 4: Custom Preset Palette
- Define `PresetColors` dictionary with custom groups
- Organize by categories (brand colors, pastels, etc.)
- Set `Columns` property for grid layout
- Use `ShowRecentColors="true"` for history
- Enable `NoColor="true"` for "no selection" option
- Ideal for brand color systems, design systems

### Key Props

| Property | Default | Use When |
|----------|---------|----------|
| Value | "" | Binding selected color (hex, rgba, hsva) |
| Mode | ColorPickerMode.Picker | Choosing Picker (gradient) or Palette (grid) |
| Inline | false | Display picker inline instead of popup |
| ModeSwitcher | false | Allow user to switch between Picker and Palette |
| EnableOpacity | false | Enable alpha channel/transparency control |
| ShowButtons | false | Show Apply and Cancel buttons (popup mode) |
| ShowRecentColors | true | Display recently selected colors |
| PresetColors | null | Custom preset color palette dictionary |
| Columns | 10 | Number of columns in palette mode |
| NoColor | false | Allow "no color" selection option |
| Disabled | false | Disable color selection entirely |
| EnableRtl | false | Enable right-to-left layout |
| CssClass | "" | Apply custom CSS classes |
| HtmlAttributes | null | Add custom HTML attributes |

### Common Use Cases

1. **UI Theme Customization**: User-selectable themes, color scheme builders, personalization
2. **Design Tools**: Graphic editors, drawing apps, image annotation, markup tools
3. **Data Visualization**: Chart color selection, graph customization, report styling
4. **Branding**: Logo colors, brand identity, style guide implementation, corporate colors
5. **Web Design**: CSS color selection, background colors, text colors, border colors
6. **Product Customization**: Product color variants, custom merchandise, personalization
7. **Highlighting Tools**: Text highlighters, annotation colors, emphasis markers
8. **Dashboard Customization**: Widget colors, status indicators, category colors
9. **Form Builders**: Custom form styling, input colors, validation colors
10. **Calendar/Scheduling**: Event colors, category coding, priority indicators

### Quick Decision Tree

**User needs color selection functionality**
  ├─ Specific predefined colors only?
  │  ├─ Use `Mode="ColorPickerMode.Palette"`
  │  ├─ Set `PresetColors` with custom palette
  │  └─ Configure `Columns` for grid layout
  ├─ Any color from gradient?
  │  ├─ Use `Mode="ColorPickerMode.Picker"` (default)
  │  └─ Users can select from full color spectrum
  ├─ Allow both modes?
  │  ├─ Enable `ModeSwitcher="true"`
  │  └─ Users can toggle between Picker and Palette
  ├─ Need transparency/opacity?
  │  ├─ Set `EnableOpacity="true"`
  │  ├─ Value will include alpha (rgba format)
  │  └─ Show opacity slider for user control
  ├─ Always visible (no popup)?
  │  ├─ Set `Inline="true"`
  │  ├─ Use in settings panels, sidebars
  │  └─ Consider `ShowButtons="false"` for instant selection
  ├─ Popup with confirmation?
  │  ├─ Use `Inline="false"` (default)
  │  ├─ Set `ShowButtons="true"` for Apply/Cancel
  │  └─ Listen to `Selected` event for confirmed selection
  ├─ Track recent colors?
  │  ├─ Enable `ShowRecentColors="true"` (default)
  │  └─ Recent colors displayed automatically
  ├─ Allow "no color" selection?
  │  ├─ Set `NoColor="true"`
  │  └─ User can clear color selection
  ├─ Custom color palette needed?
  │  ├─ Define `PresetColors` dictionary
  │  ├─ Group colors by category
  │  └─ Example: { "Basic": ["#fff", "#000"], "Brand": ["#e74c3c", "#3498db"] }
  ├─ Form integration required?
  │  ├─ Use within `<EditForm>`
  │  ├─ Apply validation attributes
  │  └─ Listen to `ValueChanged` for validation
  └─ Accessibility important?
     ├─ Ensure keyboard navigation works (Tab, Enter, Escape)
     ├─ Test screen reader compatibility
     └─ Provide clear labels via `HtmlAttributes`