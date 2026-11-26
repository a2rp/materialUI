# Accessibility and Keyboard Interaction: Q&A

This Q&A accompanies [036 - Accessibility and Keyboard Interaction](../lessons/036-accessibility-and-keyboard-interaction.md). It focuses on semantic controls, names and descriptions, focus, keyboard behavior, dialogs, status changes, and visual access.

## Names, labels, and semantics

### 1. What is an accessible name?

It identifies a control to assistive technology. Visible button text usually provides the name. An icon-only button needs a label such as “Delete project” that describes its action in context.

### 2. What is the difference between a name and a description?

A name identifies the control or content. A description adds supporting information, such as explaining what happens after activation. A tooltip can provide a description when the control already has a name.

### 3. Should I use a button or a link?

Use a button for an action and a link for navigation. Choose the root semantics through the appropriate component API and provide the props required by that element.

### 4. Does changing a component's root keep its original semantics?

Not necessarily. The component prop changes the rendered root. Check that the selected element and role still match the interaction, and provide its required attributes.

### 5. How should I label a TextField?

Use the label prop or explicitly associate a label with the input. Placeholder text is not a reliable replacement for a persistent label because it can disappear while users enter a value.

### 6. Should every icon inside a control have its own label?

Usually no. If the button already has an accessible name, a decorative icon should not repeat it. A standalone informative icon may need an accessible title.

### 7. Should I add ARIA roles to every Material UI component?

No. Prefer native elements and documented component props. Add ARIA only when the native semantics do not express the required name, description, state, or relationship. Conflicting ARIA can make behavior less clear.

## State and keyboard behavior

### 8. How do I expose a toggle button's state?

Use aria-pressed for a two-state toggle button and update it with the actual application state. Do not communicate the pressed state only with color or a changed icon.

### 9. When should I use aria-expanded?

Use it on a control that shows or hides related content, such as a disclosure or menu trigger. Keep the value synchronized with whether the controlled content is currently expanded.

### 10. Why should I avoid positive tabindex values?

Positive values create a focus order that can diverge from the DOM and reading order. Use natural document order and correct semantic elements instead.

### 11. Is hover behavior enough to make a control understandable?

No. Keyboard users do not hover. Make interactive controls focusable, give them visible focus, and ensure their action does not require a pointer-only interaction.

### 12. How should I keep focus visible?

Do not remove the focus indicator when changing outlines, shadows, or ripples. Review the indicator on every relevant background and check that overflow containers do not clip it.

### 13. Does Material UI have a global focus-visible theme option?

Current Material UI documentation includes a theme-level focusVisible option starting with v9.4. Check the installed version before using it. If the project version does not support it, provide visible focus styling through its supported customization APIs.

### 14. Should I disable focus management in a dialog?

Usually not. Modal focus movement, focus containment, and restoration help users stay oriented. Disable those behaviors only for a specific integration need, and provide an equivalent accessible interaction.

### 15. How should a dialog be labelled?

Connect the dialog to a visible title with aria-labelledby. Add aria-describedby when explanatory text is useful. Use Dialog for common modal dialog patterns instead of building a modal foundation from scratch.

### 16. Where should focus go after a dialog closes?

It should return to the control that opened the dialog, unless that control no longer exists or another destination is more appropriate. Test the full open, interact, close, and return sequence with a keyboard.

### 17. How do keyboard patterns differ among components?

Tabs, menus, dialogs, date pickers, and data grids each have their own documented keyboard model. Use those patterns rather than making every nested control a separate Tab stop. Custom content must cooperate with the parent component's focus behavior.

## Errors and changing content

### 18. How should I show form errors?

Use text that identifies the problem and tells the user how to correct it. Connect it to the field and retain the value so the user can fix it. Do not rely only on a red outline.

### 19. Should every error use role="alert"?

No. Use an assertive alert for urgent information. Less urgent updates can use a polite status region. Avoid adding a second live role when the component already announces the message.

### 20. How do I announce that an asynchronous action succeeded?

Update a status region, such as an element with role="status", when the action completes. Keep the announcement concise and do not move focus unless the next task requires it.

### 21. Does a spinner announce loading by itself?

Not always. Provide an accessible name and, when needed, a nearby status message that explains what is loading or whether the action completed. Check the semantics of the specific progress component.

## Visual access

### 22. Can I rely on a theme's default colors for contrast?

No. Check text, borders, focus indicators, and disabled states against their backgrounds. Default palette colors may need adjustment for the specific contrast requirements of the interface.

### 23. Why should I not use color as the only status cue?

People may not perceive the same color differences, and colors can change with display settings. Add text, shape, icon, or another cue that communicates the state independently.

### 24. What should I check at high zoom?

Ensure users can still read content, reach controls, and understand relationships without clipped text or hidden actions. Reflow the layout when possible instead of forcing a tiny fixed viewport.

### 25. How should I handle motion?

Avoid unnecessary or rapid motion, and respect reduced-motion preferences for nonessential animation. Loading indicators should appear only while work is actually in progress.

### 26. Is an automated accessibility scan enough?

No. Automated tools can detect some structural problems, but they cannot judge whether labels make sense or whether a workflow is usable. Combine automated scans with keyboard, screen-reader, zoom, contrast, and content review.

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
      <td><a href="035-typescript-with-material-ui.md">035 - TypeScript with Material UI</a></td>
      <td><a href="../lessons/036-accessibility-and-keyboard-interaction.md">Lesson</a></td>
      <td><a href="../examples/036-accessibility-and-keyboard-interaction.md">Example</a></td>
      <td><a href="037-localization-rtl-and-internationalization.md">037 - Localization, RTL, and Internationalization</a></td>
    </tr>
  </tbody>
</table>
