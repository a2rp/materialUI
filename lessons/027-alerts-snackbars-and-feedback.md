# Alerts, Snackbars, and Feedback

## Learning goals

After this chapter, you should be able to:

- Choose Alert, Snackbar, or Dialog based on the urgency and interaction.
- Present success, information, warning, and error messages clearly.
- Control Snackbar visibility and understand its close reasons.
- Use auto-dismiss timing without hiding essential information.
- Make feedback understandable to screen readers and keyboard users.
- Handle consecutive notifications without confusing message state.

## Choose the feedback pattern

| Component | Use it for | Interaction |
| --- | --- | --- |
| `Alert` | A message that belongs in the page flow and may need to remain visible. | The page remains usable. An optional action or close control can be provided. |
| `Snackbar` | A brief, non-critical update about an action or process. | It floats above the page and does not block normal interaction. |
| `Dialog` | A critical decision or task that needs the user's response before continuing. | It blocks interaction with the page until dismissed or completed. |

Do not choose only by appearance. Ask whether the user needs to act, how urgent the message is, and how long the information must remain available.

## Alert

Alert displays brief information in the page flow. Its `severity` can be `success`, `info`, `warning`, or `error`. Severity changes the visual treatment and default icon; the message still needs to state what happened and what the user can do.

```jsx
<Alert severity="success">
  Settings saved.
</Alert>
```

Use `AlertTitle` for a short heading when a message needs more structure. An Alert can be dismissible with `onClose`, or can contain a clear action. If `onClose` is provided and no custom `action` is set, Material UI displays a close button.

Alerts render with the `alert` role by default. This is an assertive live announcement and should be reserved for messages that need prompt attention. Less urgent updates can use a less assertive role such as `status`. Do not announce a large stream of repeated alerts assertively.

Essential warnings or errors should remain visible until the user can understand and address them. Do not automatically hide information that blocks a task or describes a serious failure.

## Snackbar

Snackbar displays a temporary, non-critical update in a fixed position above the page content. Control it through `open` and close it from `onClose`:

```jsx
const [snackbarOpen, setSnackbarOpen] = useState(false);

<Button onClick={() => setSnackbarOpen(true)}>Save changes</Button>

<Snackbar
  open={snackbarOpen}
  autoHideDuration={6000}
  onClose={() => setSnackbarOpen(false)}
  message="Changes saved"
/>
```

`autoHideDuration` calls `onClose` after the specified number of milliseconds. It does not change the controlled `open` state by itself. The default is `null`, which means no automatic dismissal.

The callback reason can be `timeout`, `clickaway`, or `escapeKeyDown`. You can ignore a reason when the workflow requires the Snackbar to stay visible, but provide a clear and accessible way to dismiss or act on it.

Snackbars should not convey critical information that disappears before the user can respond. Allow enough reading time, consider users who need more time, and pause the timer when the browser window loses focus unless the application has a specific reason not to.

## Actions and undo

A Snackbar can contain an action such as Undo. Keep the action label specific, ensure it is keyboard accessible, and provide enough time for the action to be found and activated. If losing the message would make the action impossible to recover, use a persistent Alert or another page-level mechanism.

For longer or more important actions, prefer a visible status area with a persistent control rather than a brief toast.

## Consecutive feedback

If one Snackbar component displays a sequence of different messages, pass a changing `key` so each notification is treated as a distinct message:

```jsx
<Snackbar
  key={message.id}
  open={Boolean(message)}
  message={message?.text}
  autoHideDuration={6000}
  onClose={handleClose}
/>
```

Keep the notification state coherent. A stale timeout should not dismiss a newer message. Avoid stacking many snackbars; queue or replace messages in a predictable way.

## Position and layout

Use `anchorOrigin` to choose where a Snackbar appears. Keep it clear of persistent controls such as a mobile bottom bar or floating action button. Check narrow screens, browser zoom, and layouts with on-screen keyboards.

Alerts fill their surrounding layout container. Place them close to the relevant form, section, or page heading so users can associate the message with its cause.

## Accessibility

- State the result in text. Do not communicate success or error through color alone.
- Keep alert text concise and specific.
- Use the default assertive Alert role only for information that needs immediate attention.
- Use a less assertive role for routine status updates.
- Make action and close controls keyboard accessible with clear names.
- Do not move keyboard focus to an ordinary Alert or Snackbar.
- Do not automatically dismiss a message that users must read or act on.
- Do not use a Snackbar as the only way to report a critical save failure.
- Check that repeated live announcements do not overwhelm users.

## Common mistakes

### Using Snackbar for a critical error

A short timer can hide a serious failure. Keep critical error details visible and let the user recover.

### Assuming autoHideDuration controls the open prop

It only requests closure through `onClose`. Update the state that controls `open`.

### Making every Alert assertive

The default `alert` role is assertive. Frequent non-urgent alerts can interrupt screen reader users. Use `status` for less urgent updates.

### Hiding an action too quickly

Undo and other actions need enough time for users to read and activate them. If the action is essential, keep it available elsewhere too.

### Stacking every message as a toast

A long stack obscures content and creates announcement noise. Queue or replace messages and summarize repeated updates when possible.

### Using a dialog for simple feedback

A dialog blocks the page and requires dismissal. Use it only when the user needs to make a decision or complete a focused task.

## Practical guidance

1. Use Alert for inline messages that may need to stay visible.
2. Use Snackbar for brief, non-critical feedback.
3. Use Dialog when a response is required before continuing.
4. Choose severity and live role according to urgency.
5. Control Snackbar state through `open` and `onClose`.
6. Give users enough time and an accessible action path.

## Recap

Alert, Snackbar, and Dialog have different urgency and interaction models. Alerts stay in the page flow, Snackbars float temporarily without blocking the page, and Dialogs interrupt for a decision or task. Use text as well as visual severity, and make timed feedback safe to miss.

## Practice

Build a form that shows inline error details in an Alert and a Snackbar after a successful save. Make the error persistent, make the success message dismissible, and confirm that neither message takes keyboard focus unexpectedly.

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
      <td><a href="026-popovers-tooltips-and-click-away.md">026 - Popovers, tooltips, and click-away</a></td>
      <td><a href="../qnas/027-alerts-snackbars-and-feedback.md">Q&amp;A</a></td>
      <td><a href="../examples/027-alerts-snackbars-and-feedback.md">Example</a></td>
      <td><a href="028-progress-indicators-and-skeletons.md">028 - Progress indicators and skeletons</a></td>
    </tr>
  </tbody>
</table>
