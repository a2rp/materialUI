# Checkboxes, Radio Buttons, and Switches - Q&A

## 1. When should I use a checkbox?

Use a checkbox for one independent choice or for an option in a set where users may select multiple items.

## 2. When should I use radio buttons?

Use radio buttons when users must choose exactly one option from a short visible list. Use a Select when a long choice list would take too much space.

## 3. When should I use a switch?

Use a switch for a single setting that turns on or off, especially when the setting's effect is clear from its label.

## 4. What does FormControlLabel provide?

It connects a visible label with a Checkbox, Radio, or Switch. This makes the label and control work as one understandable target.

## 5. How should I label a switch?

Describe the effect of the on state. For example, "Email me security alerts" is clearer than "Notifications."

## 6. What is FormGroup used for?

FormGroup is a helper for organizing a set of related checkbox or switch controls. Add a descriptive group heading or instructions where needed.

## 7. How do I give a radio group a label?

Use FormLabel with a stable ID and connect it through RadioGroup's \`aria-labelledby\`:

```jsx
<FormLabel id="delivery-label">Delivery speed</FormLabel>
<RadioGroup aria-labelledby="delivery-label" name="delivery">
  ...
</RadioGroup>
```

## 8. Why should a radio group have a name?

The shared name groups the native radio inputs so that only one option in the set can be selected and submitted.

## 9. How do I make a radio group controlled?

Pass the selected string to \`value\` and update it in \`onChange\`:

```jsx
<RadioGroup
  name="delivery"
  value={delivery}
  onChange={(event) => setDelivery(event.target.value)}
>
  ...
</RadioGroup>
```

## 10. How do I control a checkbox or switch?

Use the boolean \`checked\` state and update it from \`event.target.checked\`. Do not use the selected value string for a simple on/off state.

## 11. What is the difference between \`checked\` and \`defaultChecked\`?

\`checked\` makes the control controlled by React. \`defaultChecked\` sets its initial state while leaving it uncontrolled. Do not provide both on the same control.

## 12. What does \`indeterminate\` mean on a checkbox?

It visually represents a partially selected group, such as when some child options are checked. The form value is still checked or unchecked; indeterminate is not a third submitted value.

## 13. How should the parent checkbox state be calculated?

Derive it from the child selections. It is checked when all children are selected, indeterminate when some are selected, and unchecked when none are selected.

## 14. Can I use checkboxes for several related settings?

Yes. A set of independent preferences is a common checkbox use case. If each item is an immediate on/off setting that takes effect on its own, switches may be more appropriate.

## 15. Can a radio group be horizontal?

Yes. Set the \`row\` prop when there is enough space and a horizontal arrangement remains readable. Use a vertical layout when labels are long or the viewport is narrow.

## 16. Should a radio group always select a default option?

If the task expects one choice, a valid default can make the state clear. Do not select a default when doing so could create an unintended or consequential choice.

## 17. How do I label a control without visible label text?

Provide an accessible name directly on the input, for example through \`slotProps.input\`. A visible label is usually preferable because it helps both screen-reader and sighted users.

## 18. Does a disabled checkbox explain why an option is unavailable?

No. Disabled state communicates that the control cannot currently be changed. Add nearby explanatory text when the reason matters.

## 19. Should selected state be indicated only with color?

No. Keep the control's selection state and label available through its semantics and text. Color can reinforce the state but should not be the only signal.

## 20. What should I verify with a keyboard?

Check that each control can receive focus, radio choices can be navigated with the expected group behavior, and the focus indicator is visible. Labels should also be discoverable as the controls are reached.

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
      <td><a href="016-selects-menus-and-native-selects.md">016 - Selects, menus, and native selects</a></td>
      <td><a href="../lessons/017-checkboxes-radio-buttons-and-switches.md">Lesson</a></td>
      <td><a href="../examples/017-checkboxes-radio-buttons-and-switches.md">Example</a></td>
      <td><a href="018-form-control-validation-and-accessibility.md">018 - Form control, validation, and accessibility</a></td>
    </tr>
  </tbody>
</table>
