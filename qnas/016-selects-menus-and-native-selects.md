# Selects, Menus, and Native Selects - Q&A

## 1. What is the difference between Select and Menu?

Select is a form field whose chosen option becomes a value. Menu is a temporary list of commands or destinations opened from another control.

## 2. When should I use Autocomplete instead of Select?

Use Autocomplete when users need to search a long list, load options asynchronously, or create new values. A standard Select is better for a short, fixed list.

## 3. How do I label a custom Select?

Connect an InputLabel ID to Select's `labelId`. For the outlined variant, also pass the visible label through Select's `label` prop:

```jsx
<InputLabel id="region-label">Region</InputLabel>
<Select labelId="region-label" label="Region" value={region}>
  ...
</Select>
```

## 4. Can TextField create a select field?

Yes. Set `select` on TextField and pass MenuItem children. TextField creates the form field structure and label association for the common case.

## 5. How do I keep a Select value in React state?

Pass the state value to Select and update it in `onChange`:

```jsx
<Select
  value={region}
  onChange={(event) => setRegion(event.target.value)}
>
  ...
</Select>
```

## 6. What is the difference between NativeSelect and Select?

NativeSelect renders a platform-native HTML `select` element and `option` children. Select renders a Material UI popup with MenuItem children. Choose based on the experience and customization the interface needs.

## 7. When is a native select useful?

It can provide familiar platform behavior and a convenient picker on mobile devices. The native appearance and interaction may differ between operating systems and browsers.

## 8. How do I label a NativeSelect?

Give the select an ID and set the InputLabel's `htmlFor` to the same value. The accessible name should identify the field.

## 9. Does Select support a `placeholder` prop?

No. Use a visible InputLabel. If an empty visual state is required, use `displayEmpty` and `renderValue` with an empty option, and ensure the empty state is not mistaken for a selected value.

## 10. What is the value type for a multiple Select?

It is an array of selected values. Initialize controlled state with an array and update it from `event.target.value`.

## 11. When should I use a multiple Select?

Use it when users need several choices from a relatively short list. For long lists or search, use Autocomplete or a dedicated selection experience.

## 12. How do I group options in a custom Select?

Use ListSubheader to create meaningful visual groups. For NativeSelect, use HTML `optgroup`.

## 13. Why does the outlined Select need its label in two places?

The InputLabel's ID connects the accessible label, while Select's `label` prop gives the outlined input the text it needs for the floating-label notch. They serve different purposes.

## 14. How do I open a Menu from a button?

Store the clicked button element as the anchor and set Menu's `open` based on whether that element exists. Pass the anchor through `anchorEl` and close the menu by clearing the anchor.

## 15. How should a menu item behave after a choice?

The action should complete and the menu should close. Provide clear labels and preserve the menu's keyboard focus behavior.

## 16. Does Menu handle keyboard focus?

Menu includes focus behavior for menu items. If you disable or replace that behavior, you must provide a correct keyboard focus strategy yourself.

## 17. Can I use a Menu to choose a persistent form value?

A Menu can technically show choices, but use Select or an explicit selection control when the chosen value belongs to a form or needs to remain visible as field state.

## 18. How are Menu and Select different from a simple popover?

A menu presents a command or choice list with menu interaction behavior. A Select is a form control. A generic popover is a positioned surface and does not automatically supply the same menu or field semantics.

## 19. What should I use for an accessible name if there is no visible label?

Provide an appropriate `aria-label` or another programmatic label. A visible label is preferred for most form fields because it helps all users identify the field.

## 20. What should I check when a Select is inside a form?

Check the label association, stable `name`, controlled value, selected option, helper or error text, and keyboard interaction. Also make sure the submitted value is the expected stable value, not just a presentation label.

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
      <td><a href="015-text-fields-and-input-components.md">015 - Text fields and input components</a></td>
      <td><a href="../lessons/016-selects-menus-and-native-selects.md">Lesson</a></td>
      <td><a href="../examples/016-selects-menus-and-native-selects.md">Example</a></td>
      <td><a href="017-checkboxes-radio-buttons-and-switches.md">017 - Checkboxes, radio buttons, and switches</a></td>
    </tr>
  </tbody>
</table>
