# Universal Accessibility Checklist

Technology-agnostic guidelines for building inclusive, accessible applications that meet WCAG 2.1 AA standards and support all users across any platform or technology stack.

## 🎯 Core Principles (POUR)

### Perceivable
Information and user interface components must be presentable to users in ways they can perceive.

### Operable
User interface components and navigation must be operable by all users.

### Understandable
Information and the operation of user interface must be understandable.

### Robust
Content must be robust enough that it can be interpreted reliably by a wide variety of user agents, including assistive technologies.

## ✅ Universal Development Checklist

### 🎨 Visual Design & Content

#### Color & Contrast
- [ ] **Text contrast ratio**: 4.5:1 for normal text, 3:1 for large text (18pt+)
- [ ] **Non-text contrast**: 3:1 for UI components, graphical elements, and focus indicators
- [ ] **Color independence**: Information not conveyed by color alone
- [ ] **Focus indicators**: Visible focus outline with 3:1 contrast ratio against background

```css
/* Example: Accessible focus styles (Web) */
:focus {
  outline: 3px solid #0066cc;
  outline-offset: 2px;
}

/* High contrast text examples */
.high-contrast-text {
  color: #1a1a1a; /* On white: ratio 12.63:1 */
}

.medium-contrast-text {
  color: #595959; /* On white: ratio 7.0:1 */
}
```

```swift
// Example: Focus styling (iOS)
button.layer.borderWidth = 3.0
button.layer.borderColor = UIColor.systemBlue.cgColor
```

```java
// Example: Focus styling (Android)
button.setBackgroundTintList(ColorStateList.valueOf(Color.BLUE));
button.setStrokeWidth(3);
```

#### Typography & Spacing
- [ ] **Font size**: Minimum 14px/12pt for body text across platforms
- [ ] **Line height**: 1.5x font size for body text
- [ ] **Text spacing**: Allow user customization without loss of functionality
- [ ] **Paragraph width**: Maximum 80 characters per line

### ⌨️ Input & Navigation

#### Keyboard Accessibility
- [ ] **Tab order**: Logical and predictable across all interactive elements
- [ ] **Skip navigation**: Provide mechanism to skip repetitive content
- [ ] **Focus management**: Manage focus appropriately in dynamic content
- [ ] **Keyboard shortcuts**: Don't conflict with assistive technology shortcuts

```html
<!-- Example: Skip link (Web) -->
<a href="#main-content" class="skip-link">Skip to main content</a>
<main id="main-content" tabindex="-1">
  <!-- Main content -->
</main>
```

```swift
// Example: VoiceOver navigation (iOS)
view.isAccessibilityElement = true
view.accessibilityLabel = "Main navigation button"
view.accessibilityHint = "Double tap to open menu"
```

```java
// Example: TalkBack navigation (Android)
button.setContentDescription("Submit form");
button.setAccessibilityDelegate(new AccessibilityDelegate() {
    @Override
    public boolean performAccessibilityAction(View host, int action, Bundle args) {
        // Handle accessibility actions
    }
});
```

#### Touch & Pointer Targets
- [ ] **Minimum size**: 44dp (Android) / 44pt (iOS) / 44px (Web) for touch targets
- [ ] **Adequate spacing**: Minimum 8px/pt/dp between interactive elements
- [ ] **Alternative interaction**: Don't rely on complex gestures alone

### 🖼️ Media & Content

#### Alternative Content
- [ ] **Images**: Descriptive alternative text for informative images
- [ ] **Decorative images**: Mark as decorative (empty alt attribute or role="presentation")
- [ ] **Complex images**: Provide detailed descriptions via long description or caption
- [ ] **Icons**: Provide text labels or descriptions for functional icons

```html
<!-- Web examples -->
<img src="chart.png" alt="Sales increased 25% from Q1 to Q2">
<img src="decoration.png" alt="" role="presentation">
<button><span class="icon-save" aria-hidden="true"></span>Save Document</button>
```

```swift
// iOS example
imageView.isAccessibilityElement = true
imageView.accessibilityLabel = "Company logo"
```

```java
// Android example
imageView.setContentDescription("Product image showing blue shirt");
```

#### Audio & Video Content
- [ ] **Captions**: All video content includes synchronized captions
- [ ] **Transcripts**: Audio content has complete transcripts available
- [ ] **Audio descriptions**: Video with visual content has audio descriptions
- [ ] **No autoplay**: Media doesn't autoplay with sound

#### Dynamic Content
- [ ] **Status updates**: Announce important changes to users
- [ ] **Error messages**: Clear, accessible error reporting
- [ ] **Loading states**: Indicate when content is loading or processing
- [ ] **Live regions**: Use appropriate mechanisms for dynamic content updates

```html
<!-- Web: ARIA live regions -->
<div role="status" aria-live="polite">
  Form submitted successfully
</div>
<div role="alert" aria-live="assertive">
  Error: Please fill in required field
</div>
```

```swift
// iOS: VoiceOver announcements
UIAccessibility.post(notification: .announcement, argument: "Form submitted successfully")
```

```java
// Android: TalkBack announcements
AccessibilityManager am = (AccessibilityManager) getSystemService(ACCESSIBILITY_SERVICE);
if (am.isEnabled()) {
    AccessibilityEvent event = AccessibilityEvent.obtain();
    event.setEventType(AccessibilityEvent.TYPE_ANNOUNCEMENT);
    event.getText().add("Form submitted successfully");
    am.sendAccessibilityEvent(event);
}
```

### 📝 Forms & Input

#### Form Structure
- [ ] **Labels**: All form inputs have associated labels
- [ ] **Required fields**: Clearly indicate required vs optional fields
- [ ] **Field groups**: Group related inputs with appropriate labeling
- [ ] **Instructions**: Provide clear instructions before the form

```html
<!-- Web form example -->
<fieldset>
  <legend>Personal Information</legend>
  <div class="form-group">
    <label for="name">Name <span aria-label="required">*</span></label>
    <input type="text" id="name" name="name" required aria-describedby="name-error">
    <span id="name-error" class="error" role="alert"></span>
  </div>
</fieldset>
```

```swift
// iOS form example
nameTextField.accessibilityLabel = "Name"
nameTextField.accessibilityValue = nameTextField.text
nameTextField.accessibilityTraits = .none
```

#### Error Handling & Validation
- [ ] **Error identification**: Clearly identify fields with errors
- [ ] **Descriptive messages**: Provide specific, actionable error messages
- [ ] **Error summary**: Provide a summary of errors for complex forms
- [ ] **Success feedback**: Confirm successful form submission

### 🌐 Platform-Specific Considerations

#### Web Applications
- [ ] **Semantic HTML**: Use proper HTML elements for their intended purpose
- [ ] **ARIA landmarks**: Provide page structure with landmarks
- [ ] **Headings**: Proper heading hierarchy (h1-h6)
- [ ] **Page titles**: Descriptive, unique page titles

```html
<!-- Semantic structure -->
<header role="banner">
  <nav role="navigation" aria-label="Main">
    <!-- Navigation -->
  </nav>
</header>
<main role="main">
  <h1>Page Title</h1>
  <!-- Main content -->
</main>
<aside role="complementary">
  <!-- Sidebar -->
</aside>
<footer role="contentinfo">
  <!-- Footer -->
</footer>
```

#### Mobile Applications (iOS/Android)
- [ ] **Screen reader support**: Full VoiceOver/TalkBack compatibility
- [ ] **Dynamic type**: Support system font size preferences
- [ ] **Reduce motion**: Respect reduced motion preferences
- [ ] **High contrast**: Support high contrast mode

```swift
// iOS: Dynamic Type support
label.font = UIFont.preferredFont(forTextStyle: .body)
label.adjustsFontForContentSizeCategory = true

// iOS: Reduced motion
if UIAccessibility.isReduceMotionEnabled {
    // Reduce or disable animations
}
```

```java
// Android: Large text support
textView.setTextSize(TypedValue.COMPLEX_UNIT_SP, 16);

// Android: High contrast
if ((getResources().getConfiguration().uiMode & Configuration.UI_MODE_NIGHT_MASK) == Configuration.UI_MODE_NIGHT_YES) {
    // Apply high contrast theme
}
```

#### Desktop Applications
- [ ] **Keyboard navigation**: Full keyboard accessibility
- [ ] **Screen reader APIs**: Proper integration with platform accessibility APIs
- [ ] **System themes**: Support high contrast and dark mode themes
- [ ] **Zoom support**: Handle system-level zoom appropriately

```csharp
// C# WPF: Accessibility support
AutomationProperties.SetName(button, "Submit Form");
AutomationProperties.SetHelpText(button, "Click to submit the form data");
```

```python
# Python Tkinter: Accessibility labels
button = tk.Button(root, text="Submit")
button.configure(name="submit_button")  # For screen readers
```

#### Command Line Interfaces
- [ ] **Screen reader compatibility**: Ensure output works with screen readers
- [ ] **Clear output**: Structured, understandable text output
- [ ] **Progress indication**: Show progress for long-running operations
- [ ] **Error handling**: Clear error messages and recovery options

```bash
# Good CLI accessibility practices
echo "Processing files..."
echo "Progress: [####----] 50% (5/10 files)"
echo "✓ File processed successfully"
echo "✗ Error: File not found - /path/to/file"
```

## 🧪 Testing Methods

### Automated Testing

#### Web Testing Tools
```javascript
// jest-axe for automated accessibility testing
import { axe, toHaveNoViolations } from 'jest-axe';

expect.extend(toHaveNoViolations);

test('should be accessible', async () => {
  const { container } = render(<Component />);
  const results = await axe(container);
  expect(results).toHaveNoViolations();
});
```

#### Mobile Testing
```swift
// iOS: XCTest accessibility testing
let app = XCUIApplication()
let button = app.buttons["Submit"]
XCTAssertTrue(button.isAccessibilityElement)
XCTAssertEqual(button.accessibilityLabel, "Submit form")
```

```java
// Android: Espresso accessibility testing
onView(withId(R.id.submit_button))
    .check(matches(hasContentDescription()))
    .check(matches(isDisplayed()));
```

### Manual Testing Checklist
- [ ] **Keyboard only**: Navigate entire application without mouse/touch
- [ ] **Screen reader**: Test with platform screen reader (NVDA, JAWS, VoiceOver, TalkBack)
- [ ] **Zoom testing**: Test at 200% zoom/magnification
- [ ] **Color filters**: Test with color blindness simulators
- [ ] **Reduced motion**: Test with motion reduction enabled
- [ ] **High contrast**: Test with high contrast themes

### User Testing
- [ ] **Diverse users**: Include users with disabilities in testing
- [ ] **Real scenarios**: Test with actual use cases and workflows
- [ ] **Assistive technology**: Test with various assistive technologies
- [ ] **Feedback collection**: Gather and act on accessibility feedback

## 📊 WCAG 2.1 Compliance Levels

### Level A (Minimum)
- Images have alternative text
- Videos have captions
- Content is keyboard accessible
- Page structure uses headings

### Level AA (Standard)
- Color contrast meets 4.5:1 ratio
- Text can resize to 200% without scrolling
- Focus indicators are visible
- Error messages are clear and specific

### Level AAA (Enhanced)
- Color contrast meets 7:1 ratio
- Sign language interpretation for videos
- Context-sensitive help available
- No timing requirements for tasks

## 🚀 Implementation Strategy

### Priority 1: Foundation
1. **Keyboard accessibility**: Ensure all functionality works with keyboard
2. **Screen reader support**: Implement proper labeling and structure
3. **Color contrast**: Meet minimum contrast requirements
4. **Alternative text**: Provide for all informative images

### Priority 2: Enhancement
1. **Focus management**: Implement proper focus handling
2. **Error handling**: Provide clear, actionable error messages
3. **Dynamic content**: Handle live content updates appropriately
4. **Form accessibility**: Ensure all forms are fully accessible

### Priority 3: Optimization
1. **Performance**: Optimize for assistive technology performance
2. **Customization**: Allow user customization of interface elements
3. **Advanced features**: Implement advanced accessibility features
4. **User preferences**: Respect system accessibility preferences

## 📚 Technology-Specific Resources

### Web Development
- [WCAG 2.1 Guidelines](https://www.w3.org/WAI/WCAG21/quickref/)
- [ARIA Authoring Practices](https://www.w3.org/TR/wai-aria-practices-1.1/)
- [MDN Accessibility](https://developer.mozilla.org/en-US/docs/Web/Accessibility)

### Mobile Development
- [iOS Accessibility](https://developer.apple.com/accessibility/)
- [Android Accessibility](https://developer.android.com/guide/topics/ui/accessibility)
- [React Native Accessibility](https://reactnative.dev/docs/accessibility)

### Desktop Development
- [Microsoft Accessibility](https://docs.microsoft.com/en-us/windows/apps/design/accessibility/)
- [macOS Accessibility](https://developer.apple.com/accessibility/mac/)
- [Linux Accessibility](https://wiki.gnome.org/Accessibility)

### Testing Tools
- **Web**: axe DevTools, WAVE, Lighthouse, Pa11y
- **Mobile**: Accessibility Scanner (Android), Accessibility Inspector (iOS)
- **Desktop**: NVDA, JAWS, VoiceOver, Narrator
- **Cross-platform**: Colour Contrast Analyser, Stark

### Screen Readers by Platform
- **Windows**: NVDA (free), JAWS (commercial)
- **macOS/iOS**: VoiceOver (built-in)
- **Android**: TalkBack (built-in)
- **Linux**: Orca (built-in)

## 🎯 Success Criteria

### Technical Metrics
- [ ] Pass automated accessibility testing tools
- [ ] Meet WCAG 2.1 AA standards
- [ ] Support all platform accessibility features
- [ ] Handle edge cases gracefully

### User Experience Metrics
- [ ] Users with disabilities can complete core tasks
- [ ] Navigation is logical and predictable
- [ ] Content is understandable across user groups
- [ ] Performance is acceptable with assistive technology

### Organizational Metrics
- [ ] Accessibility is integrated into development process
- [ ] Team has accessibility knowledge and training
- [ ] Regular accessibility audits are conducted
- [ ] User feedback is collected and acted upon

Remember: Accessibility is not a feature to be added later—it's a fundamental aspect of quality software that ensures everyone can use your application effectively, regardless of their abilities or the technology they use to access it.