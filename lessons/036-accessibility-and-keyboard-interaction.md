# Accessibility and Keyboard Interaction

Accessible interfaces work with assistive technology, keyboard input, zoom, and different ways of understanding visual information. Material UI provides semantic components and interaction patterns, but applications still need meaningful labels, correct state, visible focus, and thoughtful content.

This chapter focuses on practical accessibility choices when composing Material UI components.

## Learning goals

After this chapter, you should be able to:

- Choose semantic elements for links, buttons, headings, forms, and tables.
- Provide accessible names, descriptions, and state for controls.
- Keep focus visible and keyboard interaction predictable.
- Use dialogs and overlays without losing focus context.
- Announce important asynchronous status changes.
- Review color, zoom, motion, and layout beyond the default component styling.

## Start with semantic HTML

Use the element that matches the action:

- Use a link when the user navigates to another location.
- Use a button when the user performs an action.
- Use headings in a logical hierarchy.
- Use lists and tables when their native relationships help explain content.
- Use form labels and field descriptions to explain inputs.

Material UI components usually render semantic HTML elements, but props such as component can change the root. Check the resulting role and behavior when changing it. A styled div is not automatically a button, and a button should not be used for navigation.

## Accessible names and descriptions

Every interactive control needs an accessible name. Visible text usually provides one. An icon-only button needs an explicit name:

```jsx
<IconButton aria-label="Delete project" onClick={handleDelete}>
  <DeleteIcon />
</IconButton>
```

Use a name that describes the action in context. “Delete project” is more useful than “trash.” Keep the decorative icon from repeating the name.

A description adds detail without replacing the name. Tooltip can provide a visible description for hover, focus, and touch. When its child already has visible text or aria-label, use describeChild so the tooltip acts as a description rather than becoming the control's only label.

## Label form fields

Use TextField's label prop or explicitly associate a label with the input. Provide helper text for instructions and error details:

```jsx
<TextField
  id="project-name"
  label="Project name"
  value={projectName}
  onChange={handleNameChange}
  error={Boolean(error)}
  helperText={error || "Use a short name that teammates can recognize."}
/>
```

TextField connects its label and helper text to the underlying field. Keep error messages specific and explain how to fix the value. Do not rely only on a red border, placeholder, or icon.

For a multi-field form, group related controls with an appropriate legend or section heading. Preserve clear required-field instructions and identify errors in text.

## Expose state, not just appearance

When a control has a state, expose it through the appropriate native or ARIA attribute:

```jsx
<IconButton
  aria-label="Favorite project"
  aria-pressed={favorite}
  onClick={() => setFavorite((current) => !current)}
>
  {favorite ? <FavoriteIcon /> : <FavoriteBorderIcon />}
</IconButton>
```

Use aria-pressed for a toggle button, aria-expanded for a disclosure or menu trigger, and aria-selected for the relevant selection pattern. Use the component's documented state props where available. Do not add ARIA attributes that conflict with the component's built-in semantics.

Color can reinforce state but must not carry its meaning by itself. Pair status colors with text, shape, or another perceivable cue.

## Keyboard access and focus

Keep interactive controls in a logical DOM order. Use native buttons, links, inputs, and checkboxes so standard keyboard behavior works. Avoid positive tabindex values; they create a focus order that can diverge from the page's reading order.

Keep keyboard focus visible. Do not remove outlines or focus styling when changing ripples, shadows, or component styles. Current Material UI versions provide a theme-level focusVisible option for a consistent indicator; check that the installed version supports it.

```jsx
import { createTheme } from "@mui/material/styles";

const theme = createTheme({
  focusVisible: true,
});
```

After enabling a global indicator, review its contrast against each background and how it behaves in forced-colors mode. A focus ring should not be clipped by an overflow container or hidden behind adjacent content.

## Respect component keyboard patterns

Use the interaction pattern supplied by each component:

- Tabs use arrow-key navigation within the tab list and keyboard activation behavior appropriate to their configuration.
- Menus move focus among menu items and support Escape to close.
- Dialogs manage focus while open and return it to the prior control when closed.
- Date pickers support both field editing and keyboard interaction with their views.
- Data Grid uses a composite grid pattern where arrow keys navigate cells.

Do not add every nested control to the page's Tab sequence when the component is designed to manage internal focus. Custom controls inside a component must cooperate with its keyboard model.

## Dialogs and overlays

Prefer Dialog for modal dialog use cases. MUI's modal foundation manages focus movement, keeps focus within the open modal, restores focus when it closes, and supplies modal semantics.

Connect the dialog to a visible title and, when useful, a description:

```jsx
<Dialog
  open={open}
  onClose={handleClose}
  aria-labelledby="delete-title"
  aria-describedby="delete-description"
>
  <DialogTitle id="delete-title">Delete project?</DialogTitle>
  <DialogContent>
    <DialogContentText id="delete-description">
      This removes the project from the current workspace.
    </DialogContentText>
  </DialogContent>
  <DialogActions>
    <Button onClick={handleClose}>Cancel</Button>
    <Button color="error" onClick={handleConfirm}>
      Delete
    </Button>
  </DialogActions>
</Dialog>
```

Do not disable automatic focus, focus enforcement, or focus restoration casually. Those behaviors protect the modal interaction. If a special integration requires changing them, test the complete keyboard and assistive-technology flow.

## Announce status changes

When an action completes asynchronously, update a status region in addition to changing color or showing a spinner. A polite status message can announce completion without moving focus:

```jsx
<Box role="status" aria-live="polite">
  {message}
</Box>
```

Use an alert announcement for urgent errors, and avoid repeatedly announcing content that has not meaningfully changed. Check whether a component already supplies a live role before adding another one.

## Color, contrast, zoom, and motion

Check text and controls against their backgrounds, including disabled, selected, hover, and focus states. Default theme colors may need adjustment for the contrast requirements of a particular interface.

Do not communicate status through hue alone. Support zoom and increased text size without clipping controls or hiding content. Respect reduced-motion preferences for long or nonessential animation, and avoid rapidly flashing content.

## Manual review checklist

Before releasing a screen, review it using:

1. Keyboard only: move through controls, open and close overlays, and activate actions.
2. Visible focus: confirm the focused control is obvious on every surface.
3. Screen reader: confirm controls have understandable names, descriptions, and states.
4. Zoom and narrow layouts: confirm controls remain available and content remains readable.
5. Color and forced-colors mode: confirm state and focus are still perceivable.
6. Error and loading paths: confirm messages and state changes are announced clearly.

Automated checks can catch some issues, but they do not replace interaction and content review.

## Common mistakes

- Using a clickable div instead of a button or link.
- Leaving icon-only controls unnamed.
- Using a tooltip as the only explanation for a complex or destructive action.
- Showing an error only with color or an icon.
- Removing keyboard focus styling while customizing ripple or outline styles.
- Disabling modal focus management without an accessible replacement.
- Adding ARIA attributes that contradict native behavior.
- Using positive tabindex to patch a confusing DOM order.
- Clipping focus rings with overflow or stacking contexts.
- Forgetting to announce completion after an asynchronous action.

## Recap

- Start with semantic HTML and give each interactive control an accessible name.
- Preserve keyboard operation, visible focus, and the component's expected arrow-key or tab behavior.
- Connect labels, descriptions, errors, and status announcements to the relevant controls.
- Review contrast, zoom, reduced motion, dialogs, and overlays in addition to automated accessibility checks.

## Chapter navigation

<table width="100%">
  <thead>
    <tr>
      <th>Previous lesson</th>
      <th>Chapter Q&amp;A</th>
      <th>Chapter example</th>
      <th>Next lesson</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="035-typescript-with-material-ui.md">035 - TypeScript with Material UI</a></td>
      <td><a href="../qnas/036-accessibility-and-keyboard-interaction.md">Q&amp;A</a></td>
      <td><a href="../examples/036-accessibility-and-keyboard-interaction.md">Example</a></td>
      <td><a href="037-localization-rtl-and-internationalization.md">037 - Localization, RTL, and Internationalization</a></td>
    </tr>
  </tbody>
</table>
