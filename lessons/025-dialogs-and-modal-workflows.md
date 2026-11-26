# Dialogs and Modal Workflows

## Learning goals

After this chapter, you should be able to:

- Use Dialog for a focused decision, form, or important detail.
- Control whether a dialog is open and respond to close requests.
- Give the dialog a useful accessible name and description.
- Design actions that make the next step clear.
- Choose an appropriate size, scroll behavior, and mobile layout.
- Preserve expected focus and dismissal behavior.

## Dialog and modal concepts

A dialog is a focused surface that presents information or requests an action. A modal dialog blocks interaction with the rest of the application until it is dismissed or completed. Dialogs are intentionally interruptive, so use them for important decisions or tasks rather than routine content.

Material UI's `Dialog` component provides the modal surface and standard dialog structure. Use its related components:

- `DialogTitle` for a concise title.
- `DialogContent` for explanatory text, forms, or other content.
- `DialogContentText` for dialog body text.
- `DialogActions` for the available actions.

For a modal dialog, use Dialog rather than assembling a raw Modal unless you need a lower-level primitive for a specialized surface.

## Control the open state

Dialog is controlled through its `open` prop. The `onClose` callback is a request to close, so update React state there:

```jsx
const [open, setOpen] = useState(false);

<Button onClick={() => setOpen(true)}>Delete project</Button>

<Dialog open={open} onClose={() => setOpen(false)}>
  {/* Dialog content */}
</Dialog>
```

The callback can receive a reason such as an Escape key press or backdrop click. If the workflow must ignore a particular close request, inspect that reason intentionally and leave another clear way to cancel or continue. Avoid disabling normal dismissal without a strong product reason.

## Give the dialog a name and description

Associate the Dialog with its title and explanatory text. This helps assistive technology announce what opened and what the dialog is asking:

```jsx
<Dialog
  open={open}
  onClose={handleClose}
  aria-labelledby="delete-project-title"
  aria-describedby="delete-project-description"
>
  <DialogTitle id="delete-project-title">Delete this project?</DialogTitle>
  <DialogContent>
    <DialogContentText id="delete-project-description">
      The project and its saved settings will be removed.
    </DialogContentText>
  </DialogContent>
</Dialog>
```

Use a specific title rather than a generic label such as "Notice". Keep the description concise and make the consequence clear.

## Make actions easy to understand

Order actions so the primary next step is easy to find. For a reversible task, offer a clear confirmation and cancellation. For a destructive action, name the consequence in the action itself when space allows, and style it to communicate the risk without relying on color alone.

```jsx
<DialogActions>
  <Button onClick={handleClose}>Cancel</Button>
  <Button color="error" variant="contained" onClick={handleDelete}>
    Delete project
  </Button>
</DialogActions>
```

Avoid ambiguous action labels such as "OK" when the user needs to understand what will happen. Do not make cancellation visually difficult to find.

## Size and responsive behavior

`maxWidth` controls the dialog's maximum width, and `fullWidth` lets it expand to that width. The default maximum width is small. Choose a size that fits the task, not the amount of content you could place inside it.

`fullScreen` can turn a dialog into a full-screen task flow, often on a small viewport. Use a media query when the rendering mode itself should change. If the application is server-rendered, account for the initial media-query result to avoid a server/client mismatch.

## Scrolling long content

Dialogs with long content should keep the title and actions available while the body scrolls. Use `scroll="paper"` or `scroll="body"` based on the desired behavior. `DialogContent dividers` can help distinguish a scrolling region from its surrounding controls.

For a long form, group related fields, explain required information, preserve entered values if validation fails, and put inline errors next to the relevant controls. Do not force users to lose input by closing the dialog after an invalid submission.

## Focus behavior

Material UI's modal foundation moves focus into the open dialog, keeps keyboard focus within it while modal, and restores focus after it closes. Keep this behavior enabled in ordinary workflows.

Choose initial focus based on the task:

- For a short confirmation, focus may begin at a sensible action or the dialog container.
- For a long informational dialog, focus the title or a focusable region near the beginning.
- For a form, choose a clear first field when it will not disorient users.
- For a destructive action, consider whether initial focus should favor the safe cancel action.

Test focus with a keyboard and screen reader. If the dialog trigger disappears after the dialog closes, restore focus to a logical location in the updated page.

## Dialog versus other surfaces

| Need | Suitable pattern |
| --- | --- |
| An interruptive decision that blocks the page | Dialog |
| A short, non-blocking explanation near a control | Tooltip or Popover |
| Navigation that slides in from an edge | Drawer |
| Brief feedback that should not require a decision | Snackbar |
| An urgent alert requiring immediate attention | An alert dialog, used sparingly |

Use `role="alertdialog"` only when the content needs immediate attention and an urgent response. Most dialogs should keep the default `dialog` role.

Avoid opening a dialog from inside another dialog unless the workflow truly needs nested modal surfaces. Stacked backdrops and focus contexts are difficult to understand and maintain.

## Common mistakes

### Using a dialog for routine information

Dialogs interrupt the user's task. Prefer inline content, a menu, tooltip, or non-modal surface when interaction does not need to be blocked.

### Leaving the dialog unnamed

Give the Dialog an `aria-labelledby` reference to a visible title. Add `aria-describedby` when a concise description will help explain the task.

### Removing all dismissal paths

Users need a clear way to cancel or close a dialog unless the task truly requires completion first. If Escape or backdrop dismissal is blocked, offer an explicit and accessible alternative.

### Using vague action labels

Name the action and its result. "Delete project" gives clearer confirmation than "Yes".

### Disabling focus management

The modal's focus behavior is important for keyboard and assistive technology users. Do not disable autofocus, focus enforcement, or focus restoration as a styling shortcut.

### Making a long dialog hard to use

Keep the title and primary actions available, allow the content area to scroll, and preserve form values when validation needs correction.

## Practical guidance

1. Use Dialog only when the task merits interruption.
2. Control it from React state and handle close requests.
3. Provide a descriptive title, optional explanation, and clear actions.
4. Keep focus inside while open and restore it logically after closing.
5. Choose a width and scroll mode that fit the content and device.
6. Prevent duplicate submission while asynchronous work is pending, and announce the result after completion.

## Recap

Dialog is the standard Material UI surface for modal workflows. Its open state is controlled, and its close callback reports a request rather than changing state by itself. Name the dialog, describe important consequences, make actions clear, and preserve modal focus behavior. Keep dialog workflows concise and responsive.

## Practice

Create a confirmation dialog for removing a team member. Include a specific title, a short consequence description, a safe cancel action, and a confirmation action that names what will be removed. After confirmation, return focus to a logical place in the updated member list.

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
      <td><a href="024-tabs-and-tab-panels.md">024 - Tabs and tab panels</a></td>
      <td><a href="../qnas/025-dialogs-and-modal-workflows.md">Q&amp;A</a></td>
      <td><a href="../examples/025-dialogs-and-modal-workflows.md">Example</a></td>
      <td><a href="026-popovers-tooltips-and-click-away.md">026 - Popovers, tooltips, and click-away</a></td>
    </tr>
  </tbody>
</table>
