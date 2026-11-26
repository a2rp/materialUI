# Buttons, Icon Buttons, and Button Groups - Q&A

## 1. What is the difference between a Button and an IconButton?

Button usually contains a visible text label and may include start or end icons. IconButton represents an action using an icon alone, so it needs a clear accessible name such as `aria-label="Open navigation"`.

## 2. Which Button variants are available?

The standard variants are `text`, `outlined`, and `contained`. Choose the variant according to the action's priority. A contained button is commonly the primary action, while outlined and text buttons carry less emphasis.

## 3. Should a screen have more than one contained button?

It can, if there are truly multiple equally important actions, but too many high-emphasis buttons make the hierarchy unclear. Make the most important action visually distinct from alternatives.

## 4. How do I make an action run when a button is clicked?

Use the `onClick` handler:

```jsx
<Button onClick={saveChanges}>Save</Button>
```

The handler should perform the action or call the code that does.

## 5. Should I use Button or an anchor for navigation?

Use a link when the user is navigating to a destination. Supplying `href` to Button makes its root render as an anchor. Use a native action button for operations such as saving, deleting, or opening a dialog.

## 6. How do I use a client-side router link?

Pass the router's link component through Button's `component` prop and provide the route destination. This preserves link semantics while allowing the router to handle navigation.

## 7. How do I add an icon to a text button?

Use `startIcon` or `endIcon`:

```jsx
<Button startIcon={<SaveIcon />}>Save changes</Button>
```

Keep the text label so users can understand the action without recognizing the icon.

## 8. Is the Material UI icon set included with the core package?

No. Icons are published in a separate package. Install that package when the application uses its icons.

## 9. Why does an IconButton need an accessible name?

An icon may not communicate its action to a screen reader or to someone unfamiliar with its meaning. Add an `aria-label` that describes what activating the button does.

## 10. Is a tooltip enough to label an IconButton?

No. Give the IconButton an accessible name directly. A tooltip can provide a visual hint, but should not be the only name for an icon-only control.

## 11. What is the difference between disabled and loading?

Disabled means the action is currently unavailable. Loading means an action is in progress and further interaction is prevented while it runs. Explain why an action is disabled if the reason matters to the user.

## 12. How do I show a loading state on a Button?

Use the `loading` prop with a boolean or `null`:

```jsx
<Button loading={isSaving} onClick={saveChanges}>
  Save changes
</Button>
```

The loading state disables the button and displays progress. Keep its label meaningful, and provide additional status text if needed.

## 13. When should I use ButtonGroup?

Use ButtonGroup to visually group related actions that belong together. Label the group with `aria-label` and make the buttons immediate children.

## 14. Can ButtonGroup be vertical?

Yes. Set `orientation="vertical"` to arrange its buttons vertically.

## 15. Is ButtonGroup appropriate for mutually exclusive selection?

Not usually. ButtonGroup groups actions visually. If the interface represents a selected value, use a control designed for selection, such as ToggleButtonGroup, radio buttons, or Select.

## 16. Can ButtonGroup provide the same variant and size to its child buttons?

Yes. Set shared properties such as `variant`, `size`, and `color` on ButtonGroup when the same setting should apply to the whole group.

## 17. What is the difference between `size` and `fullWidth`?

`size` chooses a supported control size. `fullWidth` makes the button fill its parent container. They address different layout needs.

## 18. Why is a visible focus style important?

Keyboard users need to see which control currently has focus. If customization removes the default ripple or focus styling, add a visible focus indicator so keyboard navigation remains clear.

## 19. Should I use color alone for a destructive action?

No. Use a clear text label and an appropriate confirmation or feedback pattern when needed. Color can support the meaning, but should not be the only signal.

## 20. How should a disabled button's reason be communicated?

Place concise explanatory text near the disabled control, such as the missing requirement. Avoid leaving users to guess why they cannot complete the action.

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
      <td><a href="013-grid-layout.md">013 - Grid layout</a></td>
      <td><a href="../lessons/014-buttons-icon-buttons-and-button-groups.md">Lesson</a></td>
      <td><a href="../examples/014-buttons-icon-buttons-and-button-groups.md">Example</a></td>
      <td><a href="015-text-fields-and-input-components.md">015 - Text fields and input components</a></td>
    </tr>
  </tbody>
</table>
