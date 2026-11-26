# 042 - Production Design Systems and Maintenance Q&A

## Foundations and tokens

### 1. What makes a design system different from a theme?

A theme is configuration that supplies shared values and component defaults. A design system also includes reusable components, usage guidance, accessibility expectations, ownership, release practices, and a feedback loop. A theme can be part of a design system, but it does not provide governance or guarantee consistent usage by itself.

### 2. What is a design token?

A design token is a named design decision, such as a semantic color, spacing step, or corner radius. The name gives code and documentation a stable meaning even when the underlying value changes.

### 3. Why use semantic token names instead of names such as `gray100`?

A semantic name such as `textSecondary` describes the role of a value. A palette name such as `gray100` describes its current appearance. The semantic name can remain stable if the brand palette or contrast requirements change.

### 4. Should every CSS value become a token?

No. Begin with values that recur or carry shared meaning. Tokenizing every one-off value adds maintenance and vocabulary without necessarily increasing consistency.

### 5. Which tokens should a team define first?

Start with decisions that are already repeated: brand and status colors, text and surfaces, a spacing scale, typography roles, common radii, and responsive breakpoints. Add motion or elevation tokens when products actually use them consistently.

### 6. Where should custom tokens live?

Use the Material UI theme areas for concepts they already model, including palette, typography, spacing, breakpoints, z-index, transitions, and components. Add a custom theme field when the concept is stable, shared, typed, documented, and has a clear owner.

## Choosing a customization approach

### 7. When should I use `sx`?

Use `sx` for a local adjustment or a unique layout on one component instance. If the same rule appears repeatedly, promote it to a reusable component or an appropriate shared theme rule.

### 8. When should a style become a reusable component?

When several screens share a meaningful pattern, especially if the pattern includes behavior, state handling, layout, or accessibility requirements. A reusable component should add a stable product decision rather than merely rename MUI props.

### 9. When should a component use a theme override?

Use a theme override when a shared style or default should apply consistently to many instances of an existing MUI component. Keep overrides scoped to supported component slots and avoid putting complex behavior into the theme.

### 10. Why shouldn't every customization go into the theme?

Theme configuration is global and can be difficult to reason about when it grows large. It is not tree-shakable as a customization mechanism, and large product-specific behaviors are easier to understand and maintain in reusable components.

### 11. When are global styles appropriate?

Use global styles for genuine document-wide rules, such as baseline element styling. A component-specific exception should normally stay with that component or in a reusable pattern instead of changing every matching element globally.

### 12. Are generated MUI hash class names safe to target?

No. Generated hash names are implementation details. Prefer the documented global slot classes, supported component APIs, theme overrides, or composition. Private DOM structure can change between releases.

## Component API design

### 13. What should a design-system wrapper expose?

Expose the props and slots consumers need, establish intentional product defaults, and define the added product-specific variants. Document behavior for loading, error, disabled, focus, and other relevant states. Avoid exposing every inherited prop without context.

### 14. Should a wrapper pass through all MUI props?

Only when that is useful and understood. A wrapper can preserve a subset of MUI's flexible API, but should not create an unreviewed second API surface that lets consumers bypass important design-system rules.

### 15. How should I add a custom component variant?

Define the variant's visual and behavioral meaning, implement it at the component or theme layer that fits, and document where to use it. In TypeScript, augment the component's variant types so consumers get proper checking and editor support.

### 16. When should I use slots instead of a new prop?

Use slots when consumers need to replace or configure a structural part of a component. Use a new prop when it represents a clear, stable behavior or semantic choice. Avoid adding both a slot and prop for the same choice without a concrete need.

### 17. How much should a component allow consumers to customize?

Enough to support real product composition while retaining the guarantees that make the component useful. Document escape hatches and keep visual or accessibility-critical behavior protected where possible.

## Color schemes and rendering

### 18. Why use CSS theme variables for light and dark schemes?

CSS variables allow styles to refer to values that change with the active scheme without re-computing every style branch in JavaScript. They can also make scheme-aware styling more consistent across components and rendered output.

### 19. Why should server-rendered styles avoid `theme.palette.mode` conditionals?

The server and browser can initially disagree about the user's color scheme. If styles are chosen from a JavaScript mode check, the first render can differ from the hydrated render and produce flicker. Use CSS variables and supported scheme-aware styling such as `theme.applyStyles`.

### 20. What does `InitColorSchemeScript` do?

It runs before the React app in a server-rendered page and sets the color-scheme attribute early. This helps prevent the page from first showing the wrong scheme and then changing during hydration.

### 21. When do I need `InitColorSchemeScript`?

Use it when a server-rendered Material UI application supports multiple schemes and needs the correct scheme selected before hydration. Follow the rendering framework integration and make its attribute and default mode match the theme setup.

### 22. Is it necessary to configure light and dark schemes in every project?

No. Configure only the schemes the product supports. A fixed single-scheme site may not need the extra scheme and toggle logic.

## Accessibility and quality

### 23. Can a global default such as disabling ripple be only a visual decision?

No. A global default can change interaction behavior across many components. Review keyboard focus and other feedback states to ensure that users still receive clear cues.

### 24. What states should be considered in a design-system component?

Consider the states relevant to the component, including default, hover, focus-visible, active, disabled, selected, error, loading, and read-only. Also review responsive layouts, scheme contrast, and content variations such as long labels.

### 25. Are automated accessibility checks enough?

No. Automated checks can catch some issues, but they do not establish that keyboard interaction, accessible names, focus order, or real screen-reader usage is correct. Combine automated checks with manual review.

### 26. What do visual regression checks tell me?

They can highlight unintended changes in layout, color, or component appearance across representative screens. They do not prove semantic correctness, accessibility, or that an interaction works.

### 27. How should I test a shared token change?

Test representative component families and screens that use the token, including relevant color schemes and breakpoints. The broader the token's use, the broader the visual and integration coverage should be.

## Governance and maintenance

### 28. Who should own the design system?

Name maintainers who can triage reports, review changes, and communicate releases. Several people can share ownership, but consumers need to know where to ask and who approves changes.

### 29. How should a team decide whether to add a shared component?

Look for a repeated need, a meaningful common API, and a clear owner. Validate it with representative consumers before making it a shared contract. A one-off page requirement may remain local.

### 30. How should a breaking component change be released?

Identify consumers, state the change clearly, provide a migration path, update examples, and test consuming applications. Release it according to the repository's versioning policy and give consumers time to adopt it.

### 31. Does semantic versioning automatically make upgrades safe?

No. Version numbers communicate intended compatibility, but an upgrade still needs release-note review, dependency checks, application tests, and manual validation of critical screens.

### 32. Can a codemod complete an API migration without review?

No. A codemod automates mechanical transformations. Inspect the diff, resolve cases it cannot handle, and run the checks required by the project.

### 33. Do Material UI Core and MUI X use the same version sequence?

No. MUI X packages are versioned independently. Check each package's supported compatibility information and do not assume matching version numbers imply a compatible pair.

### 34. How should dependency upgrades be handled?

Upgrade in a controlled change, consult migration notes, keep the package manager lockfile, and validate the application. Separate dependency work from unrelated visual redesign so that regressions are easier to trace.

### 35. How often should design tokens and components be reviewed?

Review them continuously through feedback and planned maintenance. A token or component should be revisited when products reveal duplication, accessibility issues, confusing APIs, or changes in the underlying platform. Avoid changing stable APIs only to make them look newer.

### 36. What is a good measure of design-system health?

Useful signals include adoption of shared patterns, fewer repeated local fixes, accessible behavior, clear documentation, manageable upgrade work, and feedback from consuming teams. A high component count by itself is not evidence of success.

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
      <td><a href="041-form-library-integration.md">041 - Form library integration Q&amp;A</a></td>
      <td><a href="../lessons/042-production-design-system-and-maintenance.md">042 - Production design systems and maintenance</a></td>
      <td><a href="../examples/042-production-design-system-and-maintenance.md">042 - Production design systems and maintenance example</a></td>
      <td>End of course</td>
    </tr>
  </tbody>
</table>