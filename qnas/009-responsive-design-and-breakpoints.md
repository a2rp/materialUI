# Responsive Design and Breakpoints - Q&A

## 1. What is a breakpoint?

A breakpoint is a width threshold where a responsive layout can change. Material UI names common thresholds xs, sm, md, lg, and xl. The theme maps these names to minimum widths.

## 2. What are the default Material UI breakpoints?

The defaults are xs 0 px, sm 600 px, md 900 px, lg 1200 px, and xl 1536 px. A theme can change these values, so check the project's active theme.

## 3. Does md mean tablet?

No. It is a threshold name, not a guaranteed device category. It can be configured, and devices can have different viewport widths or orientations.

## 4. What does a responsive sx object mean?

It assigns values at named breakpoints. A value applies at that threshold and above, until a later breakpoint provides another value.

## 5. Should I use object or array syntax for responsive sx values?

Object syntax uses breakpoint names and is usually easier to understand. Arrays can work for a small, fixed breakpoint set, but values are positional and less clear as the set grows.

## 6. When should I use theme.breakpoints.up or down?

Use the breakpoint helpers when you need a media query in a style callback or in another styling system. For simple values in sx, responsive objects are often shorter.

## 7. What does theme.breakpoints.down('md') target?

It targets widths below the md threshold. The exact generated query depends on the theme breakpoint values and its step configuration.

## 8. Should I use CSS or useMediaQuery?

Use CSS responsive styles when only appearance or layout changes. Use useMediaQuery when JavaScript needs to change behavior or the React rendering tree based on a media query.

## 9. Why can useMediaQuery be tricky with server rendering?

The server does not know the eventual browser viewport. The server and client can initially evaluate a query differently, producing a mismatch or extra render. Material UI supports SSR configuration, but CSS media queries are preferable for visual-only adaptations.

## 10. Can I rename the breakpoint keys?

Yes. Configure the complete breakpoint values map in the theme. In TypeScript, augment the breakpoint types to match the custom names.

## 11. Can I change only md and leave the other defaults implicit?

When replacing the breakpoint values object, provide all values that the theme should support. This makes the scale explicit and prevents accidental missing thresholds.

## 12. What should I do when content breaks between two breakpoints?

Test the widths between the breakpoints and adjust the layout based on content constraints. A design should not be optimized only for the named threshold widths.

## 13. What is a container query, and when might I use it?

A container query adapts a component based on its containing region's size rather than the full viewport. It is useful for reusable components that can appear in differently sized panels. Check browser support and the MUI System syntax for the installed version.

## 14. Does a responsive sx object change the React tree?

No. It changes generated CSS values. The same React elements remain rendered. Use JavaScript media queries only when the component structure or behavior must change.

## 15. Why is the viewport meta tag important?

It tells mobile browsers to use the device width as the page viewport and supports expected responsive scaling and zoom. Check that the app's HTML document already contains it.

## 16. How many breakpoints should an application use?

Use as many as the content needs, not a fixed number for every device. A small set of well-defined thresholds is easier to maintain than many nearly identical breakpoints.

## 17. How should I test responsive behavior?

Resize to widths below, at, and above each relevant breakpoint. Also test intermediate widths, zoom, long content, keyboard access, and horizontal overflow.

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
      <td><a href="008-css-overrides-and-global-styles.md">008 - CSS overrides and global styles</a></td>
      <td><a href="../lessons/009-responsive-design-and-breakpoints.md">Lesson</a></td>
      <td><a href="../examples/009-responsive-design-and-breakpoints.md">Example</a></td>
      <td><a href="010-color-schemes-dark-mode-and-css-variables.md">010 - Color schemes, dark mode, and CSS variables</a></td>
    </tr>
  </tbody>
</table>
