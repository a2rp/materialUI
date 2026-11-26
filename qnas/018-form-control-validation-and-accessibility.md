# Form Control, Validation, and Accessibility - Q&A

## 1. What does FormControl provide?

It shares state such as error, disabled, required, focused, and variant with one form control and its label or helper text. Keep one input in a FormControl.

## 2. When should I use TextField instead of FormControl?

Use TextField for the common label, input, and helper-text pattern. Use FormControl with lower-level pieces when the composition needs more control.

## 3. Why should every input have a label?

The label gives the control an accessible name and explains its purpose to every user. A placeholder disappears and should not be the only label.

## 4. How do I connect a label to a custom input?

Give the input an \`id\` and set the label's \`htmlFor\` to that same unique value.

## 5. How do I connect helper text to a custom input?

Give the helper text an ID and reference it from the input with \`aria-describedby\`:

```jsx
<Input id="account-name" aria-describedby="account-name-help" />
<FormHelperText id="account-name-help">
  Shown to your team.
</FormHelperText>
```

TextField connects its own label and helper text when supplied with the appropriate props.

## 6. What does the \`error\` prop do?

It applies the component's visual error state. It does not explain the problem by itself. Pair it with a concise message that tells users what to fix.

## 7. Should I add \`aria-invalid\` to every TextField?

TextField's error state handles its own error presentation. For custom inputs, set \`aria-invalid="true"\` when the current value is invalid and connect the explanatory message.

## 8. Does \`required\` replace application validation?

No. It communicates a requirement and enables browser constraint validation. The application still needs to validate submitted data and handle server responses.

## 9. When should validation errors appear?

Choose a predictable point, such as after blur or form submission. Avoid interrupting users with errors before they have had a reasonable chance to enter a value.

## 10. Should I clear a field after an error?

No. Preserve the user's value so they can correct it instead of starting over.

## 11. What is the difference between helper text and an error message?

Helper text provides normal instructions or context. An error message explains why a value is invalid and what action can correct it.

## 12. How do I give a radio group an accessible heading?

Use a FormLabel or legend and connect it to RadioGroup with \`aria-labelledby\`. Use a semantic fieldset when it fits the form structure.

## 13. How should a group error message be associated with the radio choices?

Give the message an ID and reference it from RadioGroup using \`aria-describedby\`. Set an error state on the group when the selection is invalid.

## 14. What is an error summary?

It is a concise summary near the form that tells users the submission needs attention. For longer forms, link each issue to its field and keep field-level messages as well.

## 15. Should I move focus after invalid submission?

For a form with errors, moving focus to the first invalid field or to an error summary can help users locate the problem. Do it after submission, not on every keystroke.

## 16. When should I use an alert role?

Use an alert for important errors that need immediate announcement. Use a polite status region for successful completion or background updates. Do not announce every changing error as urgent.

## 17. Does FormControl support several inputs?

It is designed to provide shared state for one input and its related label or helper text. Give each field its own FormControl or TextField.

## 18. What should happen when the server rejects a value?

Keep the user's input, show a message associated with the relevant field, and let them correct and resubmit. Client-side validation can improve feedback but cannot replace server validation.

## 19. How can I preserve focus visibility?

Keep Material UI's focus styles when customizing controls. If custom styles remove the default focus appearance, add a clearly visible replacement for keyboard users.

## 20. What accessibility checks should I perform on a form?

Verify that every input has an accessible name, helper and error text are associated, keyboard order is sensible, focus is visible, group labels are announced, and errors explain how to recover.

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
      <td><a href="017-checkboxes-radio-buttons-and-switches.md">017 - Checkboxes, radio buttons, and switches</a></td>
      <td><a href="../lessons/018-form-control-validation-and-accessibility.md">Lesson</a></td>
      <td><a href="../examples/018-form-control-validation-and-accessibility.md">Example</a></td>
      <td><a href="019-autocomplete.md">019 - Autocomplete</a></td>
    </tr>
  </tbody>
</table>
