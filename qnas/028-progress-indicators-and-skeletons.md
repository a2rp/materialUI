# Progress Indicators and Skeletons - Q&A

## 1. What is the difference between determinate and indeterminate progress?

Determinate progress reports measured completion using a value. Indeterminate progress communicates that work is ongoing when the duration or completion amount is not known.

## 2. When should I use CircularProgress?

Use CircularProgress for compact progress feedback near a control or for a bounded task where a circular indicator fits. Give it an accessible name and add nearby text if users need to know what is happening.

## 3. When should I use LinearProgress?

Use LinearProgress to show progress across a region or longer horizontal task. It can be indeterminate, determinate, or show a buffer alongside completed work.

## 4. How do I show a known percentage?

Set `variant="determinate"` and pass the measured number to `value`. The default range is 0 to 100. Show a visible number too if the percentage is useful to the user.

## 5. Can progress use a range other than zero to one hundred?

Yes. Set `min` and `max` to describe the actual measure. Make sure the application value uses the same range and that the displayed value text explains the scale.

## 6. How do I report progress when the task has no percentage?

Use an indeterminate indicator with a clear accessible name and nearby text such as "Saving project". Do not invent a percentage just to make the bar look more informative.

## 7. What is LinearProgress's buffer variant?

It displays both completed progress and an additional buffered amount. The `valueBuffer` should be greater than or equal to `value`. Use it only when the operation actually has a meaningful buffered stage.

## 8. How should I label a progress indicator?

Use `aria-label` or `aria-labelledby` to provide an accessible name, such as "Uploading project files". The name should identify the operation rather than simply say "Loading".

## 9. How do I associate progress with a busy region?

Give the progress indicator an id, set `aria-describedby` on the affected region to that id, and set `aria-busy="true"` while loading. Clear the relationship and busy state when the operation finishes.

## 10. Does Skeleton announce that content is loading?

No. Skeleton is a visual placeholder with no progress semantics and it is not keyboard focusable. Pair it with an accessible loading status or progress indicator.

## 11. When should I use Skeleton instead of a spinner?

Use Skeleton when the shape of upcoming content helps orient users and prevent layout changes. Use a spinner or progress bar when the operation itself needs a clear status or its progress matters.

## 12. Should Skeleton placeholders look exactly like the finished content?

They should roughly match the size and layout of the expected content. Exact detail is unnecessary, and elaborate skeletons may create more visual noise than help.

## 13. Should Skeleton animation always be enabled?

No. It animates by default, but animation can be disabled or changed. Reduce motion when shimmer is distracting or the page already has substantial movement.

## 14. When should I start showing a progress indicator?

Very short waits often do not need one. For longer waits, show which operation or region is pending. Avoid a brief flash by delaying the indicator slightly when it helps prevent flicker, but do not hide feedback from an operation that will take noticeable time.

## 15. Can I estimate progress using a timer?

Do not show a timer-based percentage as if it were measured. If the operation has no reliable progress data, use an indeterminate indicator and truthful status text.

## 16. Should progress reach 100 percent before the server confirms completion?

No. Keep progress truthful. If work is still awaiting final confirmation, remain in a pending state until the task has actually completed.

## 17. How do I distinguish loading from an empty result?

Loading means the operation has not finished. Empty means it completed and found no content. Replace the loading indicator with an intentional empty state once the result is known.

## 18. How do I distinguish a failed request from ongoing loading?

Stop the busy state and show a clear error with an available recovery action. Do not leave a spinner or skeleton displayed after the request has failed.

## 19. Should a progress indicator take keyboard focus?

Usually no. It should be named and announced without disrupting the user's current focus. Use a separate live status message if a state change needs announcement.

## 20. What is aria-valuetext for?

It provides a human-readable progress description when the underlying scale is not naturally a percentage. For example, it can express a step count or stage name that the raw numeric value cannot explain.

## 21. How often should determinate progress update?

Update it often enough to reflect useful changes without flooding rendering with unnecessary updates. Very frequent values may animate behind the true operation because the visual transition itself takes time.

## 22. What should I do if the work is lengthy?

Show a clear ongoing status, provide determinate progress if it is measured, and offer cancellation when the underlying operation supports it. Do not promise cancellation when the work cannot actually be stopped.

## 23. What should I test before shipping loading feedback?

Check the accessible name, busy-region relationship, loading duration, final empty and error states, value accuracy, reduced motion, narrow layouts, and whether the message remains useful to keyboard and screen reader users.

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
      <td><a href="027-alerts-snackbars-and-feedback.md">027 - Alerts, snackbars, and feedback</a></td>
      <td><a href="../lessons/028-progress-indicators-and-skeletons.md">Lesson</a></td>
      <td><a href="../examples/028-progress-indicators-and-skeletons.md">Example</a></td>
      <td><a href="029-chips-avatars-and-badges.md">029 - Chips, avatars, and badges</a></td>
    </tr>
  </tbody>
</table>
