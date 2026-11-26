# Autocomplete - Q&A

## 1. What is Autocomplete?

It is a text input enhanced with a popup of suggested options. Users can search a known set of choices or, with free-solo mode, enter arbitrary text.

## 2. When should I use Autocomplete instead of Select?

Use Autocomplete for long, searchable, asynchronously loaded, or creatable option sets. Select is usually simpler for a short, fixed list.

## 3. What must \`renderInput\` return?

It should return a TextField and spread the parameters provided by Autocomplete:

```jsx
renderInput={(params) => <TextField {...params} label="Owner" />}
```

Those parameters connect the input with focus, keyboard, popup, and accessibility behavior.

## 4. What is the difference between \`value\` and \`inputValue\`?

\`value\` is the selected option or options. \`inputValue\` is the text currently shown in the textbox. They are separate states and can be controlled independently.

## 5. What does \`getOptionLabel\` do?

It returns the string displayed for an option. It is useful when options are objects rather than strings.

## 6. How does Autocomplete compare object options by default?

It uses strict object equality unless a value mapping or custom comparison is provided. If the options are recreated or reloaded as different object instances, provide stable identity handling.

## 7. When should I use \`isOptionEqualToValue\`?

Use it when option objects may be different instances but represent the same underlying record. Compare a stable key such as the option ID.

## 8. What does \`getOptionKey\` solve?

It supplies a unique key for each option when labels are not unique. This avoids treating duplicate labels as identical list keys.

## 9. What does \`getOptionValue\` do in Material UI v9?

It maps an option object to a unique primitive value such as an ID. Then the component's \`value\`, \`defaultValue\`, and \`onChange\` can use that primitive instead of the full option object.

## 10. What types should \`getOptionValue\` return?

It must return a unique, non-null primitive for each option. When free-solo is enabled, strings are reserved for free-solo values, so the mapped option value must not be a string.

## 11. What value does a multiple Autocomplete use?

It uses an array of selected values. Initialize controlled state with an array and update it from the \`onChange\` callback.

## 12. How are multiple selected values displayed by default?

They are usually displayed as chips. Use \`limitTags\` to limit visible chips when the field is unfocused, or \`renderValue\` for a custom rendering pattern.

## 13. What must custom \`renderValue\` preserve?

Use the props returned by \`getItemProps\` on each custom selected item. They provide the expected interaction and removal behavior.

## 14. What does \`freeSolo\` allow?

It allows arbitrary text that is not one of the options. In this mode, a typed value is a string, even if the options are objects.

## 15. How should \`getOptionLabel\` work in free-solo mode?

It should handle both an option object and a string:

```jsx
getOptionLabel={(option) =>
  typeof option === 'string' ? option : option.name
}
```

## 16. How does \`groupBy\` organize options?

It returns a group label for each option. Sort options by the same property so each group appears as one contiguous section rather than repeating headers.

## 17. What should I do for a remote search field?

Control the input text, debounce requests, ignore stale responses, show a loading state, and distinguish request failure from a valid empty result.

## 18. Why should custom \`renderOption\` spread the received props?

The props contain the option's key, focus, selection, and accessibility wiring. Apply them to the rendered option element to preserve Autocomplete behavior.

## 19. How should disabled suggestions be represented?

Use \`getOptionDisabled\` to identify options that cannot be selected. Do not rely on color alone to explain their state.

## 20. What keyboard behavior should I check?

Check opening the suggestions, moving through them, selecting an option, clearing it, and continuing to type. Preserve the input and option props so built-in keyboard behavior remains intact.

## 21. Should I control both \`value\` and \`inputValue\`?

Only when the application needs to own both states. If control is unnecessary, let Autocomplete manage the state that does not need to be observed externally.

## 22. What should I do if the options array is recreated every render?

Keep controlled values referentially stable. Store selected values in state or memoize derived arrays so they do not change identity when their contents have not changed.

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
      <td><a href="018-form-control-validation-and-accessibility.md">018 - Form control, validation, and accessibility</a></td>
      <td><a href="../lessons/019-autocomplete.md">Lesson</a></td>
      <td><a href="../examples/019-autocomplete.md">Example</a></td>
      <td><a href="020-paper-cards-and-accordions.md">020 - Paper, cards, and accordions</a></td>
    </tr>
  </tbody>
</table>
