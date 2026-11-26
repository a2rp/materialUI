# Typography - Q&A

## 1. What does the Typography component do?

It applies a named typography style from the theme and renders a text element. It helps keep headings, body copy, and supporting text consistent across the application.

## 2. What is the default Typography variant?

The default variant is `body1`. Set `variant` explicitly when the text has a different role:

```jsx
<Typography variant="h2">Notifications</Typography>
<Typography variant="body1">Choose which updates you receive.</Typography>
```

## 3. Which variants are available by default?

The built-in variants are `h1` through `h6`, `subtitle1`, `subtitle2`, `body1`, `body2`, `button`, `caption`, `overline`, and `inherit`. Use the built-in scale when possible so that the design remains coherent.

## 4. Does `variant="h2"` always render an `h2` element?

It follows the default variant mapping, but the visual variant and the rendered element are separate choices. The `component` prop can override the element for one instance:

```jsx
<Typography variant="h2" component="h3">
  Notification preferences
</Typography>
```

This keeps the h2 visual style while rendering an h3 element. Use the element that fits the document outline.

## 5. How should I choose heading variants?

Choose heading levels according to the content outline: one page-level heading, then section headings at the appropriate levels. Do not skip heading levels just to get a particular font size. If the semantic level and visual size differ, combine the intended `component` and `variant` values.

## 6. How do I change typography across the application?

Customize the theme's `typography` object and provide that theme through `ThemeProvider`:

```jsx
const theme = createTheme({
  typography: {
    fontFamily: 'Arial, sans-serif',
    body1: {
      lineHeight: 1.6,
    },
  },
});
```

Theme tokens can define the shared font family and adjust individual variants.

## 7. Does setting `fontFamily` load a font file?

No. The theme selects a CSS font family. The font must also be available in the browser through a system font or the application's font loading setup. Always include fallback families.

## 8. When should I use `sx` instead of changing the theme?

Use a theme variant change when the rule represents a shared text role throughout the application. Use `sx` for a local adjustment that does not justify a new global token:

```jsx
<Typography variant="body2" sx={{ color: 'text.secondary' }}>
  Optional details
</Typography>
```

Avoid using local styling to create many nearly identical typography roles.

## 9. How do I make a heading responsive?

You can define breakpoint styles in the theme variant, or use `responsiveFontSizes` to adjust the theme's typography scale. Choose a strategy based on the design, and inspect the result at several viewport widths. Do not assume a heading that fits one screen will fit every screen.

## 10. Why are relative units commonly used for typography?

Relative units such as `rem` can follow the root font size, including user browser preferences. Avoid changing the root font size solely to make a pixel conversion easier, because this can make text harder for users to resize.

## 11. What does `gutterBottom` do?

It adds bottom margin using the component's standard heading spacing. Use it when that spacing matches the surrounding layout. For more specific or responsive spacing, use the layout system or `sx`.

## 12. What does `noWrap` do?

It prevents wrapping and truncates overflowing text with an ellipsis when the element has a constrained width. Use it only when hiding the remainder is acceptable. Important content should remain available, for example by allowing it to wrap or providing a suitable way to inspect its full value.

## 13. How should I add a custom variant?

Add it to the theme's `typography` object when it represents a repeated design role. Decide what semantic element it should use separately, and configure the default mapping only if the same mapping is right everywhere. In TypeScript, add the required type augmentation for the new variant.

## 14. Can I use a custom variant name without TypeScript changes?

In plain JavaScript, the theme can supply a custom variant style directly. In a TypeScript project, the component prop types need to be augmented so the compiler recognizes the new variant. Keep the runtime theme and the type declarations aligned.

## 15. What makes a typography system accessible?

Use a logical heading outline, readable sizes and line heights, sufficient contrast, and text that remains usable when enlarged. Do not rely on color alone to express status. Make sure text can wrap at narrow widths and that essential information is not hidden by truncation.

## 16. Why should I avoid using `caption` for every small label?

A smaller visual style is not automatically a good label. Use a style that remains legible and make sure the text clearly explains the associated control or information. Important instructions should not be demoted to tiny supporting text.

## 17. Where should a font size be changed if only one component needs it?

Keep the existing variant and apply `sx` to that instance. If the same adjustment appears repeatedly and represents a consistent role, consider updating the theme variant instead.

## 18. What should I inspect after changing the typography theme?

Check the actual semantic elements, heading sequence, line wrapping, contrast, and size at narrow and wide viewports. Verify that long text and user-enlarged text do not clip or overlap neighboring content.

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
      <td><a href="010-color-schemes-dark-mode-and-css-variables.md">010 - Color schemes, dark mode, and CSS variables</a></td>
      <td><a href="../lessons/011-typography.md">Lesson</a></td>
      <td><a href="../examples/011-typography.md">Example</a></td>
      <td><a href="012-box-container-and-stack-layout.md">012 - Box, Container, and Stack layout</a></td>
    </tr>
  </tbody>
</table>
