# Form Library Integration - Q&A

This Q&A reviews the form integration concepts from Chapter 041.

## Choosing a field integration

### 1. What does a form library manage?

It can manage field values, registration, validation, errors, touched and dirty state, reset behavior, and form submission.

### 2. What does Material UI provide in a form integration?

Material UI provides the visible controls and presentation states such as label, error styling, helper text, and disabled state. The form library manages the form data and validation workflow.

### 3. When should a field use React Hook Form register?

Use register for a native input or compatible component that exposes the actual input ref and standard change and blur events.

### 4. When should a field use Controller or useController?

Use them for controlled components that require a value prop or report a nonstandard value through a callback. Many component-library selects, autocomplete controls, and pickers fit this pattern.

### 5. Can native and controlled fields be in the same form?

Yes. It is common to register native fields and use Controller for controlled widgets within the same form.

### 6. Can Controller and register both be applied to one field?

No. Controller already registers its field. Registering it again can create duplicate subscriptions and incorrect behavior.

## Values, events, and refs

### 7. What does Controller provide to its render callback?

It provides field values and handlers such as onChange, onBlur, name, and ref, along with field-specific state and form state.

### 8. Why should every controlled field have a default value?

A field that starts as undefined can be uncontrolled, then later become controlled after it receives a value. Complete defaults prevent that transition and provide a baseline for dirty-state tracking.

### 9. Which empty values should be used for controlled fields?

Choose a value that matches the widget: an empty string for text, false for a checkbox, or null for a clearable object value. Avoid undefined as the initial value or an onChange value.

### 10. Why does the input ref matter?

The form library can use it to focus an invalid field after submission. The ref must reach the actual input or a compatible focus interface.

### 11. Why should a TextField form ref usually go to inputRef?

TextField is a wrapper around an input and other form-control markup. Its outer ref can point to the wrapper rather than the inner input expected by the form library.

### 12. What does onBlur tell the form library?

It reports that the user left or interacted with the field. This supports touched state and validation modes that run on blur.

### 13. Does field.onChange always receive a browser event?

No. It can receive a value directly. A controlled component may emit a number, date, boolean, selected option, or other value instead of an input event.

## Material UI field mapping

### 14. How should a Controller field map to TextField?

Connect the field name, value, onChange, and onBlur. Route the ref to TextField's inputRef, and use fieldState.error to set error and helperText.

### 15. How should a checkbox value be sent to the form?

Send the boolean checked value from event.target.checked. A checkbox's value attribute is not the same as its checked state.

### 16. How should a select field be initialized?

Provide a default value that matches one of the select's option values. If an empty or clearable state is allowed, define how it is represented and validated.

### 17. What is special about Autocomplete's selected value?

The selected option and the text displayed in the input are separate concepts. Store the selection that the form needs, and map the Autocomplete onChange callback to that value.

### 18. Why can Autocomplete equality need explicit configuration?

If options are objects recreated during rendering, reference equality may not identify the same logical option. Use a stable identifier for option equality when needed.

### 19. What should be considered when connecting a date picker?

Decide the stored value type, initial empty value, clear behavior, invalid intermediate value handling, and the value to submit. A picker may produce null or an invalid date state.

## Validation and submission

### 20. How should a validation error appear in a Material UI field?

Set the field's error state and show a specific helper message. The message should explain the issue or how to fix it.

### 21. When might a schema resolver be useful?

It helps when rules are shared across layers, composed, or easier to maintain in a schema. Field-level rules are also suitable for local, straightforward validation.

### 22. Which validation mode should a form use?

Choose based on the user experience. Submit-time validation avoids early errors, while blur or change validation can provide earlier feedback but may become noisy.

### 23. How should asynchronous submission affect the form?

Use the form's submitting state to prevent duplicate submissions and communicate progress. Show success or a user-friendly failure message after the operation completes.

### 24. Does client-side validation replace server validation?

No. The server must validate and authorize submitted data. Client validation improves feedback but cannot be trusted as a security boundary.

### 25. How should reset behave after loading a different record?

Call reset with the complete new values, or use a documented reactive values option. Changing the defaultValues object by itself does not replace values already mounted.

### 26. What should be considered before showing errors on every keystroke?

It can make a form noisy, especially for users relying on screen readers. Choose a validation timing that provides useful feedback without announcing incomplete or transient errors too aggressively.

## Architecture and maintenance

### 27. When is FormProvider useful?

It can provide form methods to deeply nested components without passing control props through every layer. Use it when it simplifies the tree; avoid it when direct props make dependencies clearer.

### 28. Should field components maintain duplicate local and form state?

Usually no. Duplicate sources of truth can drift apart. Use local state only when a widget has separate transient state that is intentionally different from the submitted value.

### 29. Do these integration patterns apply only to React Hook Form?

No. Other form libraries have their own controlled-field mechanisms. The same design questions remain: value shape, event mapping, validation state, refs, default values, and reset behavior.

### 30. What should be checked before shipping a form integration?

Confirm defaults, field values, callback mapping, actual input refs, accessible labels, visible error messages, async submission behavior, reset behavior, and server-side validation.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="040-performance-bundle-size-and-tree-shaking.md">040 - Performance, bundle size, and tree shaking</a></td><td><a href="../lessons/041-form-library-integration.md">Lesson</a></td><td><a href="../examples/041-form-library-integration.md">Example</a></td><td><a href="042-production-design-system-and-maintenance.md">042 - Production design systems and maintenance</a></td></tr></tbody>
</table>
