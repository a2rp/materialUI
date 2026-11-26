# Chips, Avatars, and Badges: Q&A

This Q&A accompanies [029 - Chips, Avatars, and Badges](../lessons/029-chips-avatars-and-badges.md). It focuses on component choice, keyboard use, accessible labels, and common implementation questions.

## Chips

### 1. What is a Chip for?

A Chip is a compact visual element for a short label, category, status, person, or small action. Common uses include tags, selected filters, and recipients. Use a Button for a prominent action that needs stronger emphasis or a larger target.

### 2. What is the difference between a passive and clickable Chip?

A passive Chip presents information and does not invite interaction. A clickable Chip performs a meaningful action, such as applying a filter or opening details. The `clickable` prop changes interactive styling; supply `onClick` or link semantics to perform an actual action.

### 3. What does `onDelete` do?

It adds a delete affordance and calls your handler when activated. The callback does not remove the Chip automatically. Update the React state or data source that renders the item.

### 4. Can a Chip be both clickable and deletable?

Yes. Keep the actions understandable and independently usable. For example, the body can open filter details and the delete affordance can remove the filter. Do not make the same action appear to have two different outcomes.

### 5. Which keyboard interactions should I support?

Test interactive Chips using the keyboard, including activation and deletion when `onDelete` is present. Keep a visible focus indicator and verify that keyboard users can move away from the Chip. Do not make pointer hover the only indication that it is actionable.

### 6. Should every tag be clickable?

No. A tag that only describes an item should look and behave like a label. Pointer styling without a real action creates a misleading control.

### 7. Should a filter be a Chip, Button, or Checkbox?

Use a Chip for a compact filter token, especially when selected filters can be removed individually. Use a Button for a prominent action. Use a checkbox or another selection pattern when users need to understand and change a persistent on/off state.

### 8. How should a Chip navigate to another page?

Render it using link semantics supported by your router integration. Navigation should work with the keyboard and normal browser behavior. Do not represent navigation as a button action.

### 9. How should I key a list of Chips?

Use a stable key from the represented value, such as a unique ID. Avoid array indexes for lists that can be removed or reordered, because React can then associate component state or focus with the wrong item.

## Avatars

### 10. When should I use an Avatar?

Use Avatar to identify a person, team, or entity next to related content, such as a comment or assignment. It can show an image, initials, or an icon. Show a nearby name when identity matters.

### 11. What happens if an Avatar image fails to load?

Avatar can display provided children, derive a fallback character from `alt`, or show a generic icon. Provide meaningful fallback content and test both valid and unavailable image URLs.

### 12. What should an Avatar image's `alt` text contain?

Use concise identification when the portrait adds information, such as the person's name. If adjacent text already provides that name and the image is decorative, avoid repeating it to assistive technology; an empty alternative may be appropriate. Decide based on the image's purpose in context.

### 13. How do I show initials?

Pass short initials as Avatar children, such as `AR`. Also provide the person's full name in nearby visible text or an accessible name; initials alone may not identify someone reliably.

### 14. What does `AvatarGroup` do?

It visually groups Avatars and can limit how many are visible. The `max` prop sets that limit, with an overflow indicator for additional members. Provide a way to find the full member list when it matters.

### 15. Can an Avatar be interactive?

Yes, when it is placed inside an appropriate link or button, such as a profile link or user menu trigger. Give that control a useful accessible name. Do not nest interactive controls inside one another.

## Badges

### 16. What belongs in a Badge?

A Badge adds a small count, short status, or dot to a related element. Use it for supplementary information such as unread notifications. Make the relationship between Badge and its child clear.

### 17. Why is a zero Badge hidden?

A zero value is hidden by default. Set `showZero` when the zero itself is meaningful, such as a dashboard count. Decide whether showing zero helps users understand the interface.

### 18. What does `max` do?

It caps the displayed number and adds a plus suffix for larger values. The default maximum is 99, so a larger number can appear as `99+`. Make the exact value available when users need it.

### 19. When should I use a dot instead of a number?

A dot communicates presence without a quantity. A number communicates a count. Choose the form that helps users understand the state and make a decision.

### 20. What do `overlap` and `anchorOrigin` control?

`overlap` adjusts how the Badge aligns to the shape of its child, for example a circular Avatar. `anchorOrigin` chooses its vertical and horizontal corner. Check the result at different text sizes and zoom levels so it remains visible.

### 21. How can a Badge be accessible?

Give its interactive parent a useful accessible name that describes the action and, when useful, the count, such as “Notifications, 4 unread.” Do not rely only on color or a dot to communicate an important state.

### 22. Should the Badge itself usually be a button?

No. A Badge is normally an indicator attached to a meaningful control. Put the action on the parent button or link. This avoids a tiny target and confusing nested interactive elements.

## Combining components

### 23. How can one participant row use all three components?

Use Avatar for identity, Chip for a compact role or removable filter, and Badge for supplementary activity. Keep the three purposes distinct and avoid unexplained indicators.

### 24. What happens if the Chip's delete handler does not update state?

The Chip remains rendered because `onDelete` only calls your callback. Remove the represented item from the state or data source, then React will render the updated list.

### 25. What should I check before shipping these components?

- Clickable and deletable Chips have clear actions and work by keyboard.
- Removing a Chip updates the source data and preserves the correct remaining items.
- Avatar identity remains understandable when images fail.
- AvatarGroup overflow does not hide information users need.
- Badge meaning is not conveyed by color alone.
- Parent controls have clear accessible names and no nested controls.
- Focus remains visible and indicators are not clipped on narrow screens.

## Chapter navigation

<table>
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
      <td><a href="028-progress-indicators-and-skeletons.md">028 - Progress indicators and skeletons</a></td>
      <td><a href="../lessons/029-chips-avatars-and-badges.md">Lesson</a></td>
      <td><a href="../examples/029-chips-avatars-and-badges.md">Example</a></td>
      <td><a href="030-tables-and-data-display.md">030 - Tables and data display</a></td>
    </tr>
  </tbody>
</table>
