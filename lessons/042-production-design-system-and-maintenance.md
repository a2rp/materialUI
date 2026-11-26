# 042 - Production Design Systems and Maintenance

A production design system turns repeated interface decisions into a dependable, documented product. Material UI provides the component and theming foundations; a team still needs to decide which tokens and component behaviors belong to its system, how changes are reviewed, and how the system evolves without surprising application teams.

This chapter brings together the customization choices from earlier lessons and applies them across a larger application or multiple applications. It focuses on ownership, consistency, accessibility, release management, and a maintenance loop that keeps the system useful.

## Learning goals

After this lesson, you should be able to:

- Define and organize design tokens that have clear meaning and owners.
- Choose the narrowest suitable Material UI customization layer.
- Build reusable, accessible components without hiding useful MUI APIs.
- Support light and dark schemes with CSS theme variables.
- Set up practical adoption, review, testing, documentation, and migration practices.
- Plan changes with versioning and deprecation in mind.

## 1. What a production design system contains

A design system is more than a custom theme. It usually combines:

1. **Foundations:** color, typography, spacing, shape, elevation, motion, and responsive rules.
2. **Components:** reusable controls and patterns with clear APIs and accessible behavior.
3. **Guidance:** usage examples, do and do not guidance, and accessibility expectations.
4. **Governance:** owners, review rules, change communication, release process, and migration policy.
5. **Feedback:** a way to find inconsistent or missing patterns through real product use.

Material UI gives you a broad component library, a theme API, CSS variables, and customization options. Your design system defines the product-specific decisions layered on top. It should make common work easier while allowing product teams to handle genuinely unique cases.

A shared theme alone does not guarantee consistency. A team can still use arbitrary one-off values everywhere. Consistency comes from semantic tokens, well-designed component APIs, documented examples, and code review practices that encourage reuse.

## 2. Establish semantic design tokens

A token is a named design decision. Prefer names that explain the role of a value instead of encoding its present appearance.

For example, `surfaceRaised` explains how a color is used. `gray100` only says what it looks like today. Semantic tokens can keep their names when branding or contrast requirements change.

Useful token groups include:

- **Color:** brand actions, text, surfaces, borders, focus, status, and selection.
- **Typography:** font families, type scale, weights, line heights, and letter spacing.
- **Spacing:** a consistent scale for layout and component gaps.
- **Shape:** control, card, dialog, and pill radii.
- **Elevation:** a small set of meaningful surface levels.
- **Responsive behavior:** breakpoints and layout constraints.
- **Motion:** durations and easing for meaningful transitions.
- **Layers:** documented stacking roles for app bars, menus, dialogs, and notifications.

Start with a small set of tokens used by several screens. Tokens that have no clear meaning or use become extra vocabulary without improving consistency. Record a token's meaning, intended usage, contrast requirements where applicable, and owner.

Material UI already provides theme areas such as `palette`, `typography`, `spacing`, `breakpoints`, `zIndex`, `transitions`, and `components`. Use those established locations when the value fits their meaning. Add custom theme fields only when they represent a stable design-system concept and can be typed and documented.

## 3. Choose the narrowest customization layer

Use a customization layer that matches how broadly the decision applies:

1. **One instance:** `sx` for a local exception or a unique layout.
2. **Repeated product pattern:** a reusable component built with MUI components, `styled`, or composition.
3. **Consistent defaults or styles across a family:** theme `components` configuration.
4. **True document-level rules:** `CssBaseline`, `GlobalStyles`, or a dedicated global stylesheet.

A one-off change in `sx` should not become the default for a product-wide pattern. Conversely, a tiny local spacing adjustment does not need a new reusable component.

Theme component configuration can centralize `defaultProps` and `styleOverrides`. Use slots deliberately and keep selectors scoped to stable public slots. Avoid depending on generated hash class names or private implementation details because these can change during upgrades.

A theme is a useful central configuration point, but it is not tree-shakable as a customization mechanism. If a component needs substantial custom behavior, build a reusable wrapper or domain component rather than collecting a large, complex implementation in one theme object.

## 4. Design reusable component APIs

A design-system component should express a product concept and preserve the useful parts of the underlying MUI API.

For each wrapper, decide:

- Which MUI props are intentionally passed through.
- Which defaults are set for the product.
- Which props are added for product-specific variants.
- Whether the component uses slots or accepts custom content.
- What happens for disabled, loading, error, focus, and empty states.
- How labels, descriptions, and errors reach assistive technology.
- Which parts can be overridden without defeating the design system.

Prefer composition when it keeps the API understandable. Do not create a wrapper that merely renames every MUI prop without adding a stable product rule. When a shared variant is needed, define it intentionally and document its allowed values. TypeScript projects should augment the relevant component prop types when adding custom variants or palette values so that the public API remains discoverable and type-safe.

A component API is a long-lived contract. Avoid exposing internal class names or leaking implementation details into consuming applications. Keep escape hatches such as class names, slots, or supported style props deliberate and documented.

## 5. Build color schemes with CSS variables

For applications that need light and dark schemes, CSS theme variables can express scheme-specific values without branching styles on a JavaScript palette value.

~~~tsx
import { createTheme, ThemeProvider } from '@mui/material/styles';

export const theme = createTheme({
  cssVariables: true,
  colorSchemes: {
    light: true,
    dark: true,
  },
});

export function AppTheme({ children }: React.PropsWithChildren) {
  return <ThemeProvider theme={theme}>{children}</ThemeProvider>;
}
~~~

Use the theme's CSS variables for values that should respond to the active color scheme. For a targeted dark-mode style, use `theme.applyStyles('dark', styles)` in a supported styling context. Avoid selecting styles with `theme.palette.mode`. During server rendering, that can cause the server and browser to produce different styles and create a visible flash.

A server-rendered application that allows scheme toggling should render `InitColorSchemeScript` as the first child of the body, with the theme's color scheme selector configured to match. This lets the correct scheme attribute be set before the app hydrates. Make the script's default mode agree with the theme provider. Follow the framework-specific integration steps for the version of MUI and rendering framework in use.

Do not add both schemes simply because they are available. If the product supports one fixed scheme, a simpler theme may be enough. If users can toggle schemes, test preference persistence, initial page rendering, contrast, component states, and browser system preference behavior.

## 6. Preserve accessibility and behavior

The design system must preserve semantic HTML and component interaction behavior. A visual override can accidentally hide focus indicators, reduce contrast, remove error cues, or make a target difficult to use.

Define and review:

- Keyboard focus appearance and predictable tab order.
- Text and non-text contrast for every supported scheme.
- Accessible names and descriptions for icon-only controls.
- Disabled, selected, error, loading, and read-only states.
- Touch target size and responsive layout.
- Reduced motion behavior for nonessential animation.
- Correct HTML element semantics and heading structure.

Global defaults can have broad consequences. For example, disabling ripple at the base-button level affects many controls and can remove a familiar focus-visible signal if no replacement is provided. A global default should be reviewed as an accessibility and behavior change, not only a style preference.

Test both components in isolation and complete flows in context. A button may appear correct alone but fail next to validation text, inside a dialog, or at narrow widths.

## 7. Document usage and ownership

Documentation should answer questions a consumer has while building a real screen:

- When should I use this component or pattern?
- Which variant fits each common situation?
- What is required for accessibility?
- Which props or slots are supported?
- How does it behave in each color scheme and responsive state?
- What should I use when the pattern does not fit?

Show realistic examples, including states and composition. Explain constraints instead of documenting every inherited prop without context. Keep migration notes with the release or component change that introduced them.

Assign maintainers for foundations, components, accessibility review, and release communication. Ownership can be shared, but responsibility for triage and approval must be clear. Provide a simple feedback path for reporting missing patterns and inconsistent usage.

## 8. Test and review changes

Use a layered review approach:

- **Type checking and linting:** catch invalid theme fields, component APIs, and accidental unsafe code.
- **Unit or component tests:** verify behavior such as labels, state transitions, and keyboard interaction.
- **Accessibility checks:** combine automated checks with keyboard and assistive-technology review.
- **Visual review:** compare representative screens for light and dark schemes and relevant breakpoints.
- **Integration review:** verify that a change works inside consuming applications and their rendering setup.

Snapshot tests alone do not prove accessibility or usability. Visual checks can reveal unintended global overrides, but a matching screenshot does not prove correct semantics. Pick representative coverage based on how broadly a token or component is used.

For shared tokens, test a sample of each important component family. For a component API change, test consumers that use its main variants, states, and composition points.

## 9. Version and migrate safely

Treat design-system changes as product changes. Before release:

1. Identify the consumers and affected screens.
2. Classify the change as additive, behavior-changing, or breaking.
3. Update the documentation and examples.
4. Add migration guidance for changed APIs or visual behavior.
5. Run the agreed checks in the design-system package and consuming applications.
6. Publish and communicate a release with a clear summary.

Use the versioning scheme adopted by the repository. For libraries, semantic versioning is useful when followed consistently: patches fix compatible issues, minor versions add compatible functionality, and major versions signal breaking changes. A private application can choose a different release scheme, but should still make changes traceable and communicate risk.

Material UI marks APIs deprecated before removing them according to its deprecation policy and may provide codemods. Review upgrade guides and release notes when updating. Codemods can speed up a migration, but their output still needs review and testing. MUI X packages are versioned independently from Material UI Core, so check the compatibility information for each package rather than assuming their version numbers match.

Pin dependencies with the package manager's lockfile, upgrade in a controlled branch, and separate dependency changes from unrelated design changes. Do not copy an old workaround forward without checking whether the API or recommended pattern has changed.

## 10. Keep the system healthy

A sustainable design system has a maintenance loop:

1. Observe repeated local styling and inconsistent patterns in product code.
2. Decide whether the need is truly shared.
3. Propose a token, component, or documented recipe with an owner.
4. Review usability, accessibility, and API shape with representative consumers.
5. Implement and document the smallest complete change.
6. Test the change in its actual usage contexts.
7. Release it, explain adoption, and gather feedback.
8. Remove obsolete guidance only after consumers have migrated.

The objective is not to centralize every style. Keep the common path consistent, make exceptions visible, and let teams solve unique interface problems without fighting the system.

## Common mistakes

- Creating tokens for every literal value before proving they are shared.
- Putting complex product behavior into one enormous theme object.
- Using global overrides for a single component instance.
- Depending on generated class names or private DOM structure.
- Adding a custom variant without documenting or typing its contract.
- Switching dark-mode styles with `theme.palette.mode` in server-rendered code.
- Removing focus or state cues as part of a visual reset.
- Releasing a breaking component change without consumer migration guidance.
- Treating an upgrade codemod as a substitute for code review and tests.
- Assuming MUI Core and MUI X have synchronized package version numbers.

## Practice task

Choose one repeated interface pattern in an application. Identify the relevant semantic tokens and decide whether its customization belongs in `sx`, a reusable component, the theme, or global styles. Write a small API proposal, note its accessibility states, and list the checks you would run before releasing it. Do not promote it into the shared system until the pattern is genuinely repeated and has an owner.

## Recap

- A production design system combines foundations, components, guidance, governance, and feedback.
- Semantic tokens explain purpose and should stay few enough to understand.
- Match the customization layer to the scope of the decision.
- Keep reusable component APIs accessible, typed, documented, and focused.
- CSS variables and `InitColorSchemeScript` can support stable color-scheme rendering.
- Review global defaults for behavior and accessibility impact.
- Test in representative contexts and ship clear migration guidance.
- Keep Material UI and MUI X dependency upgrades independently informed.

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
      <td><a href="041-form-library-integration.md">041 - Form library integration</a></td>
      <td><a href="../qnas/042-production-design-system-and-maintenance.md">042 - Q&amp;A</a></td>
      <td><a href="../examples/042-production-design-system-and-maintenance.md">042 - Example</a></td>
      <td>End of course</td>
    </tr>
  </tbody>
</table>