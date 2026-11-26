# Text Fields and Input Components - Q&A

## 1. What is TextField?

TextField is a convenience wrapper for a common text form control. It combines a label, an input component, and optional helper text.

## 2. Which visual variants does TextField support?

The variants are `outlined`, `filled`, and `standard`. Outlined is the default. Choose a consistent variant that fits the application's design system.

## 3. Is placeholder text a substitute for a label?

No. A placeholder is a short hint and may disappear while users type. Provide a visible label that identifies the field's purpose.

## 4. Why should a TextField have a unique ID?

The ID connects the label and helper text to the input for assistive technology. It also provides a stable association before client-side hydration in applications that render server-side.

## 5. What is the difference between `value` and `defaultValue`?

`value` makes the field controlled by React state. `defaultValue` provides an initial value for an uncontrolled field. Do not set both on one field.

## 6. How do I handle a controlled field change?

Read `event.target.value` in `onChange` and update the state:

```jsx
<TextField
  value={name}
  onChange={(event) => setName(event.target.value)}
/>
```

Initialize a controlled text field with a string, usually an empty string, to keep its value type consistent.

## 7. When should I use an uncontrolled field?

Use `defaultValue` when React does not need to update the value on every keystroke. For more advanced submission and validation flows, use an appropriate form library or controlled state.

## 8. What does the `type` prop control?

It sets the native input type, such as `email`, `password`, `search`, or `date`. Use a type that matches the data and the browser behavior users need.

## 9. Where do I pass native input attributes like `min` and `step`?

Use `slotProps.htmlInput`:

```jsx
<TextField
  type="number"
  slotProps={{ htmlInput: { min: 1, step: 1 } }}
/>
```

This slot targets the native input element.

## 10. What is the difference between `slotProps.input` and `slotProps.htmlInput`?

`slotProps.input` configures the Material UI input component, such as OutlinedInput or FilledInput. `slotProps.htmlInput` configures the native `input` or `textarea` element.

## 11. Why should I avoid using a number input for postal codes or phone numbers?

These values are identifiers, not quantities. They can contain leading zeros, punctuation, or formatting that a numeric field can alter or reject. Use a text input with suitable guidance instead.

## 12. What does `multiline` do?

It renders a textarea-style input. Use `minRows` and `maxRows` to constrain the automatically sized field, or `rows` for a fixed row count.

## 13. How do I display a validation error?

Set `error` and provide a corrective `helperText` message. The color alone does not tell users what to fix.

## 14. Does `required` perform all application validation?

No. It marks the field as required for browser form behavior and display. The application still needs to validate data and handle submission errors correctly.

## 15. What is the difference between disabled and read-only?

Disabled fields cannot be interacted with and are omitted from native form submission. Read-only fields cannot be edited but can generally remain focusable and their values can be copied.

## 16. How do I add text such as a currency sign inside a field?

Use InputAdornment in the Material UI input slot:

```jsx
<TextField
  label="Budget"
  slotProps={{
    input: {
      startAdornment: (
        <InputAdornment position="start">$</InputAdornment>
      ),
    },
  }}
/>
```

The adornment adds context but does not replace the field label.

## 17. When should I use FormControl and OutlinedInput directly?

Use the lower-level components when TextField's convenience API does not expose the composition you need. When composing manually, preserve label associations, helper text relationships, error behavior, and focus handling.

## 18. What does `fullWidth` do?

It makes TextField fill the width of its containing block. The parent still controls the available width.

## 19. Why can helper text change the height of a form row?

Helper text occupies space below the input. If some fields have helper text and others do not, their overall heights can differ. Design the form layout with that content in mind instead of removing useful explanations.

## 20. How should I choose between `inputMode` and `type`?

`type` selects the native input behavior and data type. `inputMode` provides a hint about which virtual keyboard is useful. An input mode does not validate the value.

## Chapter navigation

<table width="100%">
  <thead>
    <tr>
      <th>Previous Q&amp;A</th>
      <th>Chapter lesson</th>
      <th>Chapter example</th>
      <th>Next Q&amp;A</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="014-buttons-icon-buttons-and-button-groups.md">014 - Buttons, icon buttons, and button groups</a></td>
      <td><a href="../lessons/015-text-fields-and-input-components.md">Lesson</a></td>
      <td><a href="../examples/015-text-fields-and-input-components.md">Example</a></td>
      <td><a href="016-selects-menus-and-native-selects.md">016 - Selects, menus, and native selects</a></td>
    </tr>
  </tbody>
</table>
