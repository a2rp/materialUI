# Dialogs and Modal Workflows - Q&A

## 1. What is the difference between a dialog and a modal?

A dialog is a UI surface for information or a task. Modal describes behavior that blocks interaction with the rest of the application while the surface is open. A Dialog is commonly modal, but the terms describe different things.

## 2. When should I use Material UI Dialog?

Use Dialog for an important decision, short focused task, or detail that requires the user's attention before returning to the page. Avoid it for routine information that can live inline or in a non-blocking surface.

## 3. What is the difference between Dialog and Modal?

Dialog is a higher-level component with dialog structure and related parts such as DialogTitle, DialogContent, and DialogActions. Modal is a lower-level foundation for custom modal surfaces. Prefer Dialog for ordinary modal dialogs.

## 4. Is Dialog controlled or uncontrolled?

It is controlled. Pass a boolean to `open`, and update React state in the event handlers that open or close it.

## 5. Does onClose close the dialog automatically?

No. `onClose` reports a close request. Set the state that controls `open` to false. The callback can include a reason such as `escapeKeyDown` or `backdropClick`.

## 6. How do I prevent a backdrop click from closing a dialog?

Inspect the `reason` argument in `onClose` and ignore `backdropClick` only if the workflow requires it. Preserve another clear cancel or close path, and be careful not to trap users in a dialog unnecessarily.

## 7. Should Escape close a dialog?

Usually yes. Escape is a standard dismissal path for modal dialogs. If the task cannot safely be dismissed, explain why and provide another clear route forward. Do not disable Escape as a shortcut to avoid implementing state.

## 8. How do I name a dialog for assistive technology?

Give Dialog an `aria-labelledby` attribute that references the id of its visible DialogTitle. If a short paragraph explains the task, connect it with `aria-describedby`.

## 9. Is DialogTitle automatically connected to Dialog?

Do not rely on implicit behavior for a custom composition. Set a stable id on DialogTitle and reference it through `aria-labelledby`. This makes the relationship explicit and easier to review.

## 10. What does the modal focus trap do?

It moves focus into the open dialog and keeps keyboard focus inside while the modal is active. When it closes, focus is normally restored to the previously focused element. Keep these behaviors enabled for standard dialog workflows.

## 11. Where should focus go when a dialog opens?

Choose a location that fits the task. A short confirmation may start at the dialog or a safe action. A form can focus its first useful field. A long informational dialog may focus its title or the beginning of its content. Test with keyboard and screen reader use.

## 12. What if the dialog trigger disappears after closing?

Move focus to a logical place in the updated page, such as the next row or a page heading. The modal normally restores focus to its trigger, but an action may remove that trigger from the DOM.

## 13. How should I label destructive dialog actions?

Use an action label that states the result, such as `Delete project`. Explain the consequence near the action, provide a safe cancel option, and do not rely on red color alone to convey risk.

## 14. How do DialogContent and DialogActions work?

DialogContent holds the main text, form, or other task content. DialogActions groups the available buttons. DialogTitle identifies the task, and DialogContentText is a styled wrapper for explanatory text.

## 15. What does fullWidth do?

`fullWidth` lets the dialog expand to its `maxWidth`. It does not mean the dialog takes the full viewport width. The default page margin still constrains it.

## 16. When should I use fullScreen?

Use `fullScreen` when a task needs an immersive, full-viewport layout, commonly on a small device. Do not make every dialog full-screen; a brief confirmation is usually clearer as a compact surface.

## 17. What is the difference between scroll="paper" and scroll="body"?

`scroll="paper"` scrolls the dialog surface while keeping the rest of the page locked. `scroll="body"` allows the content around the dialog Paper to scroll. Choose a mode that keeps the title, actions, and content usable for the workflow.

## 18. Why did my dialog form lose its values after closing?

Modal content is normally unmounted when closed. Store important form state above the dialog or use the supported keep-mounted behavior if retaining the component is necessary. Weigh retained memory and effects against the cost of reinitializing the form.

## 19. Should I use role="alertdialog"?

Only for urgent content that requires immediate attention and a response. Most dialogs should use the default `dialog` role. Using alertdialog for ordinary confirmation can make announcements unnecessarily forceful.

## 20. Can I open a dialog inside another dialog?

Avoid stacked modal surfaces unless the workflow truly needs them. Multiple backdrops and focus traps can be confusing. Prefer a single dialog that changes step or a non-modal choice surface when possible.

## 21. How do I handle an asynchronous confirmation?

Show a pending state, prevent duplicate submission, and keep the user informed. On success, close the dialog and announce or display the result. On failure, keep the dialog open, preserve the input, and explain how to recover.

## 22. What should I test before shipping a dialog?

Test its title and description, initial and restored focus, Escape and backdrop handling, action labels, form validation, async failure, responsive width, long-content scrolling, browser zoom, and interaction with screen readers.

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
      <td><a href="024-tabs-and-tab-panels.md">024 - Tabs and tab panels</a></td>
      <td><a href="../lessons/025-dialogs-and-modal-workflows.md">Lesson</a></td>
      <td><a href="../examples/025-dialogs-and-modal-workflows.md">Example</a></td>
      <td><a href="026-popovers-tooltips-and-click-away.md">026 - Popovers, tooltips, and click-away</a></td>
    </tr>
  </tbody>
</table>
