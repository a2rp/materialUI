# Popovers, Tooltips, and Click-Away - Q&A

## 1. When should I use a Tooltip?

Use Tooltip for a short label or description attached to a control. It should supplement the interface, not carry essential instructions, errors, or interactive content.

## 2. What is the difference between a Tooltip and a Popover?

A Tooltip is a brief, non-interactive label or description triggered by hover, focus, or touch. A Popover is an anchored interactive surface built on modal behavior. It blocks background interaction and scrolling by default.

## 3. What is the difference between Popover and Popper?

Popover uses Modal behavior, including focus management, a backdrop, scroll locking, and click-away dismissal. Popper handles positioning without those modal behaviors. A custom Popper needs its own dismissal and focus decisions.

## 4. How do I position a Popover beside a button?

Pass the button element as `anchorEl` and set `open` and `onClose`. Use `anchorOrigin` to choose a point on the button and `transformOrigin` to choose the corresponding point on the Popover.

## 5. What happens if the Popover anchor is removed?

The positioning reference is no longer reliable. Close the Popover when the trigger is removed, or update `anchorEl` to a new valid reference. Avoid leaving an open surface attached to a stale DOM element.

## 6. Does Popover close when the user clicks outside it?

Yes. Popover uses Modal behavior and dismisses on click-away by default. Its `onClose` callback is still a request; update the controlled open state.

## 7. Does Popper close itself on click-away?

No. Use `ClickAwayListener` or another deliberate outside-interaction handler, and also provide an explicit close path when the content is interactive.

## 8. When should I use ClickAwayListener?

Use it for a custom non-modal popup, such as a Popper panel, that should close after an outside click or touch. It detects outside interaction around one wrapped child element.

## 9. Can ClickAwayListener wrap multiple sibling elements?

It accepts one child. Wrap the trigger and popup in a single element that can receive a ref. Avoid extra non-semantic wrappers when a suitable layout element already exists.

## 10. Does ClickAwayListener work with portals?

By default, it considers the React tree when deciding whether an event is outside. Portaled content in the same React tree is generally treated as inside. `disableReactTree` changes handling to use the DOM tree only.

## 11. What is the default event timing for ClickAwayListener?

It normally listens for trailing click and touch events, such as `onClick` and `onTouchEnd`. Set `mouseEvent` or `touchEvent` to a leading event when earlier dismissal is required. Leading-event mode ignores scrollbar interactions.

## 12. Why does ClickAwayListener make a heading seem clickable?

It attaches event handling to its child. If the wrapped child is not interactive, use a wrapper with `role="presentation"` where appropriate so assistive technology does not announce it as a control.

## 13. How does Tooltip provide an accessible name or description?

By default, the Tooltip title labels its child. If the child already has visible text or an `aria-label` and the Tooltip adds extra information, pass `describeChild` so the title acts as a description.

## 14. Should Tooltip be the only accessible name for an icon button?

The Tooltip can label its child, but give an icon-only action a direct `aria-label` as well. This keeps the action's name clear and independent of hover behavior.

## 15. Why does Tooltip not appear for a disabled button?

Disabled elements generally do not emit the pointer or focus events that Tooltip needs. Wrap the disabled button in a span or other suitable element. Also provide visible text when people need to understand why the action is unavailable.

## 16. Are Tooltips available on touch screens?

Yes. Material UI supports touch activation, typically through a long press and a display delay. Essential instructions should remain available without relying on this gesture.

## 17. Can a Tooltip contain links or buttons?

No. Tooltip is for concise descriptive text, not interactive content. Use Popover, Menu, or Dialog depending on whether the interaction is modal and what keyboard behavior it needs.

## 18. Does Popover block page scrolling?

Yes, by default. It is built on Modal and locks background scrolling while open. Choose Popper if the surrounding page should remain interactive and scrollable.

## 19. Why is a Popover appearing behind another surface?

Popover and Popper may render through a portal, and their stacking context interacts with dialogs, Drawers, and other overlays. Review the theme's layer order, portal container, and ancestor clipping before changing z-index.

## 20. Should I use Tooltip for truncated text?

Do not rely on Tooltip as the only text alternative for truncated content. Keep the full information accessible through the page structure or another deliberate disclosure pattern.

## 21. How do I make Tooltip content accessible by keyboard?

Wrap a focusable trigger, such as a button or link. Tooltip responds to focus as well as hover. Test the description and accessible name using a keyboard and screen reader.

## 22. How do I choose among Popover, Menu, Dialog, and Tooltip?

Use Tooltip for a short description, Menu for a temporary list of choices or commands, Popover for compact anchored interactive content, and Dialog for an interruptive modal task. Choose based on behavior rather than appearance.

## 23. What should I test before shipping a popup?

Test opening and closing by keyboard, click-away behavior, touch, viewport edges, focus, accessible names and descriptions, portal behavior, scrolling, and interactions with other overlay components.

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
      <td><a href="025-dialogs-and-modal-workflows.md">025 - Dialogs and modal workflows</a></td>
      <td><a href="../lessons/026-popovers-tooltips-and-click-away.md">Lesson</a></td>
      <td><a href="../examples/026-popovers-tooltips-and-click-away.md">Example</a></td>
      <td><a href="027-alerts-snackbars-and-feedback.md">027 - Alerts, snackbars, and feedback</a></td>
    </tr>
  </tbody>
</table>
