# Alerts, Snackbars, and Feedback - Q&A

## 1. What is the difference between Alert and Snackbar?

Alert is normally placed in the page flow and can remain visible. Snackbar floats above the interface and is intended for brief, non-critical feedback. Choose based on how long the message must remain available and whether it needs an action.

## 2. When should I use a Dialog instead?

Use Dialog when the user must make a decision or complete a task before continuing. Snackbar does not block the page, and Alert does not require the user to stop interacting.

## 3. What values can Alert severity use?

The standard values are `success`, `info`, `warning`, and `error`. Severity affects the default color and icon, but the text should still explain the result or next step.

## 4. Does Alert use role="alert" by default?

Yes. The default `alert` role is assertive and can interrupt screen reader output. Use it for messages that need prompt attention. For routine updates, use a less assertive role such as `status`.

## 5. What is the difference between role="alert" and role="status"?

An alert is intended to interrupt with urgent information. A status message is a less assertive update that should not take over current screen reader output. Choose according to message urgency.

## 6. Should every Alert automatically close?

No. Essential errors and warnings should remain until users can read and address them. A dismiss button can be useful for an informational message, but do not hide important details without a safe recovery path.

## 7. What does Alert's onClose do?

Providing `onClose` displays the default close control when no custom `action` is supplied. The callback is where the application should update state to remove or hide the Alert.

## 8. What does Snackbar autoHideDuration do?

It sets a timeout after which Snackbar calls `onClose`. It does not update the controlled `open` prop. The application must update the state to close it.

## 9. Does Snackbar auto-hide by default?

No. `autoHideDuration` defaults to `null`, so automatic dismissal is off unless a duration is provided.

## 10. What close reasons can Snackbar report?

The current API can report `timeout`, `clickaway`, or `escapeKeyDown`. The event may be null when a timeout expires. Use the reason only when the workflow needs different handling.

## 11. How can I keep a Snackbar open after click-away?

In `onClose`, ignore the `clickaway` reason and update state for other close requests. Provide a visible action or close control so keyboard users can dismiss it too.

## 12. How long should a Snackbar stay visible?

Long enough for users to notice, read, and activate any action. Avoid short timers for messages that need thought or recovery. Pause hiding when the window is blurred unless there is a product-specific reason not to.

## 13. Should a Snackbar contain an Undo action?

It can, when the action is brief and has a clear reversal. Make the action keyboard accessible and keep it available long enough. If undo is essential, offer a persistent recovery path too.

## 14. How do I display different consecutive Snackbar messages?

Store each notification as distinct state and pass a changing `key` when reusing one Snackbar component. This helps the component treat each message as a separate notification with its own timing.

## 15. Should I stack Snackbars?

Usually not. Several floating messages obscure content and create repeated announcements. Queue messages, replace a previous message, or summarize repeated events instead.

## 16. Where should I place a Snackbar?

Use `anchorOrigin` to set its screen location. Keep it clear of important controls such as floating action buttons and bottom navigation. Check how it behaves on narrow screens and with a mobile keyboard open.

## 17. Can Alert be used inside Snackbar?

Yes, a Snackbar can contain an Alert when the visual treatment is useful. It remains a brief floating notification, so do not use it to make essential content disappear automatically.

## 18. Why should I not communicate severity using color alone?

People may not perceive or distinguish the color. Include clear words such as "Saved" or "Could not save", and use an icon or other cue when it helps.

## 19. Should an Alert or Snackbar receive keyboard focus when it appears?

Normally no. Feedback should not unexpectedly move focus away from the user's current task. If the message contains an action, ensure the action is reachable in normal keyboard order.

## 20. How should I report a save failure?

Show the failure details near the relevant form or in a persistent Alert, preserve the user's input, and explain how to retry or recover. A short-lived Snackbar is not enough for a failure that needs attention.

## 21. What is the difference between Alert and an ARIA alert dialog?

Material UI Alert is a non-blocking message component. An alert dialog is an interruptive modal that requests a response. Use Dialog with an alert dialog role only for urgent situations requiring immediate attention.

## 22. How do I prevent a stale timeout from closing a newer notification?

Represent notifications with distinct identity or key state and replace the previous message deliberately. Ensure a timeout for an old message cannot clear the newly active notification.

## 23. What should I test before shipping feedback?

Check the text, urgency, live-region role, dismissal path, timer duration, blur behavior, action reachability, repeated notifications, mobile placement, and recovery from errors.

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
      <td><a href="026-popovers-tooltips-and-click-away.md">026 - Popovers, tooltips, and click-away</a></td>
      <td><a href="../lessons/027-alerts-snackbars-and-feedback.md">Lesson</a></td>
      <td><a href="../examples/027-alerts-snackbars-and-feedback.md">Example</a></td>
      <td><a href="028-progress-indicators-and-skeletons.md">028 - Progress indicators and skeletons</a></td>
    </tr>
  </tbody>
</table>
