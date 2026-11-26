# Progress Indicators and Skeletons

## Learning goals

After this chapter, you should be able to:

- Choose an indeterminate or determinate progress indicator.
- Use LinearProgress or CircularProgress in a fitting context.
- Label progress indicators and connect them to busy regions.
- Build skeleton placeholders that match the content being loaded.
- Distinguish loading, empty, success, and error states.
- Avoid misleading progress values and unnecessary animation.

## Progress indicator types

Progress indicators communicate an operation that is currently underway.

- An indeterminate indicator shows that work is happening when its duration or completion percentage is unknown.
- A determinate indicator shows a meaningful amount of completed work, using a value and a known range.

Choose LinearProgress for a region or task that naturally moves along a path. Choose CircularProgress for compact feedback near a control or for a clearly bounded task. The shape should fit the operation instead of being chosen only for decoration.

## Indeterminate progress

Use indeterminate progress when the system cannot calculate a reliable percentage:

```jsx
<LinearProgress aria-label="Loading project reports" />
<CircularProgress aria-label="Saving project settings" />
```

Keep an explanatory label near the indicator when the user needs to know what is loading. A spinner without context can leave people unsure whether the whole page or one small region is affected.

## Determinate progress

Use `variant="determinate"` with `value` when the application has a trustworthy measure:

```jsx
<LinearProgress
  variant="determinate"
  value={progress}
  aria-label="Uploading project files"
/>

<CircularProgress
  variant="determinate"
  value={progress}
  aria-label="Uploading project files"
/>
```

The default value range is 0 to 100. Use the `min` and `max` props when the underlying task has a different range. The value should come from the operation, not from a decorative timer. Do not show 100 percent until the operation is actually complete.

For a buffered operation, LinearProgress can show both completed work and available buffered work. The buffer value must be greater than or equal to the current progress value.

If a percentage is meaningful to users, show it as text too. The visual indicator communicates shape and motion, while a number or phrase provides an explicit status.

## Accessible progress

Give every progress bar an accessible name through `aria-label` or `aria-labelledby`. When it describes one loading region, connect the region to the indicator with `aria-describedby` and set `aria-busy="true"` on the region until the work finishes:

```jsx
<Box
  component="section"
  aria-labelledby="reports-heading"
  aria-describedby={loading ? 'reports-progress' : undefined}
  aria-busy={loading}
>
  <Typography id="reports-heading" component="h2" variant="h6">
    Reports
  </Typography>

  {loading && (
    <LinearProgress
      id="reports-progress"
      aria-label="Loading reports"
    />
  )}

  {!loading && <ReportList reports={reports} />}
</Box>
```

When loading completes, set `aria-busy` to false and remove the progress description if the indicator is no longer rendered. Do not use an indeterminate indicator as the only announcement of a state change. A concise status message can help users understand completion or failure.

## Skeleton placeholders

Skeleton approximates the shape of content that is not available yet. It can reduce visual layout changes when the real content arrives. Use the variant that resembles the expected content:

- `text` for lines of text.
- `circular` for an avatar or circular element.
- `rectangular` for an image or block.
- `rounded` for a rounded content surface.

```jsx
<Stack spacing={1}>
  <Skeleton variant="text" width="45%" />
  <Skeleton variant="text" width="80%" />
  <Skeleton variant="rounded" height={120} />
</Stack>
```

Skeleton can infer dimensions from its child when it helps:

```jsx
<Skeleton variant="circular">
  <Avatar />
</Skeleton>
```

Match the placeholder dimensions to the content where possible. Avoid drawing a large decorative skeleton when the actual content is small or when the data arrives too quickly for a loading state to be useful.

Skeleton itself has no progress semantics and is not keyboard focusable. Pair it with a clear loading status for assistive technology. Treat the shimmer as decorative and do not make placeholder shapes behave like real controls.

## Loading versus empty and error states

Loading means the application is waiting for work to finish. Empty means the work completed successfully and there is no content to show. Error means the work failed. These states need different messages and actions.

Do not keep showing a spinner for a completed empty result. Do not replace an error with a permanent skeleton. Show the appropriate state once the operation resolves.

## Timing and perceived responsiveness

Very short waits do not always need a progress indicator. Showing a spinner for a quick action can make the interface feel slower. For longer waits, show the affected region and a clear explanation. For lengthy operations, provide determinate progress when it can be measured and an option to cancel when the operation supports cancellation.

Do not invent percentages or estimate completion with an arbitrary timer. If progress is not measurable, use an indeterminate indicator and truthful status text.

## Animation and performance

Skeleton animates by default. You can select the wave animation or disable animation. Use less motion when the effect is distracting, when the screen already contains multiple animated elements, or when supporting a reduced-motion preference.

Progress components animate their values. Very frequent updates can lag behind the actual operation. Update at a useful rate and keep the underlying data accurate. Heavy CPU work can prevent the browser from painting smooth animation; move suitable work off the main thread rather than relying on a spinner to make a blocked page feel responsive.

## Common mistakes

### Showing an indeterminate spinner for a measurable operation

If the application knows how much work is complete, determinate progress gives users more useful information.

### Showing fake progress

A fabricated percentage can mislead people about how long they need to wait. Use a real measure or an indeterminate state.

### Making a progress indicator inaccessible

Give it an accessible name. If it describes a region, connect the region and indicator and update `aria-busy` for the duration of the work.

### Using a Skeleton as a loading announcement

Skeleton has no progress semantics. Pair it with an accessible status message and mark decorative placeholder shapes as hidden from assistive technology where appropriate.

### Confusing loading and empty content

When loading finishes with no results, show an intentional empty state. Do not leave a skeleton on screen.

### Leaving progress at 100 percent before the operation finishes

Only report completion when the underlying task has completed. If final server confirmation is still pending, keep the state truthful.

## Practical guidance

1. Show progress only when a wait lasts long enough to benefit from feedback.
2. Use indeterminate feedback when duration is unknown.
3. Use determinate values only when measured by the real operation.
4. Label the indicator and mark its content region busy.
5. Match Skeleton shapes to the content and announce the loading state separately.
6. Replace loading with success, empty, or error content when the operation resolves.

## Recap

Progress indicators describe ongoing work. Determinate indicators show measured completion; indeterminate indicators show unknown duration. Skeletons preview content shape but do not announce loading by themselves. Name progress indicators, mark busy regions, and transition cleanly to the final state.

## Practice

Create a report panel that initially renders a loading message and matching skeleton rows. When data becomes available, show the report list or a deliberate empty state. If the request fails, show a useful error message. Verify the busy state and progress label.

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
      <td><a href="027-alerts-snackbars-and-feedback.md">027 - Alerts, snackbars, and feedback</a></td>
      <td><a href="../qnas/028-progress-indicators-and-skeletons.md">Q&amp;A</a></td>
      <td><a href="../examples/028-progress-indicators-and-skeletons.md">Example</a></td>
      <td><a href="029-chips-avatars-and-badges.md">029 - Chips, avatars, and badges</a></td>
    </tr>
  </tbody>
</table>
