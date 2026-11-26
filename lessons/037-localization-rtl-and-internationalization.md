# Localization, RTL, and Internationalization

Internationalized interfaces can support multiple languages and regions. Localization supplies the language, formats, labels, and conventions for one locale. Right-to-left layout is part of localization, but also affects components, icons, portals, and custom styles.

Material UI provides translated component labels and direction-aware styling hooks. Your application still owns product copy, language selection, date and number conventions, and the process for switching locale.

## Learning goals

After this chapter, you should be able to:

- Separate application translation from Material UI component translations.
- Use a locale bundle with a Material UI theme.
- Format dates, numbers, and currency with locale-aware APIs.
- Configure RTL direction in the document, theme, and style engine.
- Handle portals, date pickers, and logical CSS properties.
- Switch locale without leaving parts of the interface inconsistent.

## Locale is more than a language

A language tag can include language, region, and sometimes script. `en-US` and `en-GB` both use English but commonly differ in spelling, date conventions, and number formatting. Arabic locales can also differ.

Keep these concerns distinct:

- **Application messages:** headings, help text, validation messages, and product-specific labels.
- **Material UI locale text:** built-in labels such as pagination actions and dialog controls.
- **Date-library locale:** date order, month names, weekday names, and week-start rules.
- **Direction:** left-to-right or right-to-left text and layout.
- **Formatting:** localized dates, numbers, currency, and percentages.

A single locale change may need to update each layer. Translating headings alone leaves controls, accessible names, date fields, and numeric output inconsistent.

## Translate application messages

Keep user-facing text in message resources rather than scattering conditional strings throughout components. A small application can start with a dictionary; larger applications often use a dedicated internationalization library for message catalogs, plural rules, interpolation, and lazy loading.

```jsx
const messages = {
  "en-US": { save: "Save", greeting: "Welcome back" },
  "fr-FR": { save: "Enregistrer", greeting: "Bon retour" },
};

function SaveButton({ locale }) {
  const copy = messages[locale] ?? messages["en-US"];
  return <Button>{copy.save}</Button>;
}
```

Use a complete fallback locale when a translation is missing. Avoid making component behavior depend on translated words; use stable message keys for logic and tests.

For messages that include counts or user-provided values, use your translation system's plural and interpolation features instead of manually concatenating fragments. Grammar and word order can change between languages.

## Localize Material UI component labels

Material UI ships locale bundles for component text. Pass the appropriate bundle to `createTheme` and provide the resulting theme through `ThemeProvider`:

```jsx
import { createTheme, ThemeProvider } from "@mui/material/styles";
import { frFR } from "@mui/material/locale";

const frenchTheme = createTheme(
  { palette: { primary: { main: "#275dad" } } },
  frFR,
);

function App() {
  return (
    <ThemeProvider theme={frenchTheme}>
      <Application />
    </ThemeProvider>
  );
}
```

The bundle covers Material UI component labels. It does not translate application messages or automatically configure every separately distributed library. MUI X components have their own localization configuration, so use the mechanism for the exact package you render.

Create a theme for the active configuration; do not mutate a shared theme object while rendering.

## Format values with the active locale

Use the browser's `Intl` APIs for common locale-sensitive values. They apply conventions for the requested locale instead of assuming that every user expects the same order or separators.

```js
const locale = "en-IN";

const dateText = new Intl.DateTimeFormat(locale, {
  dateStyle: "medium",
  timeZone: "Asia/Kolkata",
}).format(new Date("2026-11-26T10:30:00Z"));

const priceText = new Intl.NumberFormat(locale, {
  style: "currency",
  currency: "INR",
}).format(12500);
```

Store dates and numbers in canonical data types, then format them for display. Do not store a preformatted string as the underlying amount or date. Date-only values, instants, and time zones have different meanings; choose the correct representation first.

For percentages, compact numbers, units, and plural-sensitive messages, use the corresponding `Intl` formatter or translation library. Check realistic values, including large, negative, and missing values.

## Configure right-to-left layout

A complete Material UI RTL setup has three parts:

1. Set the document or relevant DOM subtree direction with `dir="rtl"`.
2. Set `direction: "rtl"` in the Material UI theme.
3. Configure the style engine with the RTL transformation plugin.

With Emotion, create an RTL cache and include the normal Stylis prefixer before the RTL plugin:

```jsx
import { CacheProvider } from "@emotion/react";
import createCache from "@emotion/cache";
import { prefixer } from "stylis";
import rtlPlugin from "@mui/stylis-plugin-rtl";

const ltrCache = createCache({ key: "mui" });
const rtlCache = createCache({
  key: "muirtl",
  stylisPlugins: [prefixer, rtlPlugin],
});
```

Choose the cache and theme together for the active direction:

```jsx
import { createTheme, ThemeProvider } from "@mui/material/styles";

function AppProviders({ direction, children }) {
  const theme = React.useMemo(
    () => createTheme({ direction }),
    [direction],
  );
  const cache = direction === "rtl" ? rtlCache : ltrCache;

  return (
    <CacheProvider value={cache}>
      <ThemeProvider theme={theme}>
        <div dir={direction}>{children}</div>
      </ThemeProvider>
    </CacheProvider>
  );
}
```

In a server-rendered application, render the correct `lang` and `dir` attributes in the initial HTML. Avoid rendering one direction on the server and flipping it after hydration; that can cause layout shifts and mismatches. Use a cache integration appropriate to the framework and style engine.

When locale changes at runtime, update application messages, theme locale, direction, document `lang` and `dir`, and date-library locale together. Do not leave a section in the previous language.

## Direction and portals

Some components render content in a portal outside the element that contains them. A locally scoped `dir="rtl"` does not automatically reach a portal elsewhere in the document. Dialogs, menus, popovers, and tooltips may need direction on the portal component itself when the whole document is not RTL.

Prefer setting direction on the document root when the entire application uses one direction. If only one region is RTL, check portal-based components and pass direction to the relevant component where supported.

Direction can change icon meaning. A forward arrow, back arrow, breadcrumb, or next-page affordance may need to mirror. Decorative icons do not always need mirroring; decide based on meaning.

## Date pickers have separate localization inputs

Date and time pickers use a date-library adapter. Its locale controls date formatting conventions, while picker text such as button labels and placeholders can come from locale text. Configure both when needed:

```jsx
import "dayjs/locale/de";
import { AdapterDayjs } from "@mui/x-date-pickers/AdapterDayjs";
import { LocalizationProvider } from "@mui/x-date-pickers/LocalizationProvider";
import { DatePicker } from "@mui/x-date-pickers/DatePicker";
import { deDE } from "@mui/x-date-pickers/locales";

function GermanDateField() {
  return (
    <LocalizationProvider
      dateAdapter={AdapterDayjs}
      adapterLocale="de"
      localeText={deDE.components.MuiLocalizationProvider.defaultProps.localeText}
    >
      <DatePicker label="Startdatum" />
    </LocalizationProvider>
  );
}
```

The adapter locale and translated component strings are separate settings. A correctly formatted date can still have English action labels; translated labels can still use an unexpected date order if the adapter locale is missing. Choose compatible versions of MUI X and its adapter library.

## Write direction-resilient styles

Prefer CSS logical properties so the browser maps spacing and alignment to the current writing direction:

```css
.profileCard {
  margin-inline-start: 1rem;
  padding-inline: 1.25rem;
  border-inline-start: 3px solid currentColor;
  text-align: start;
}
```

In component styles, properties such as `marginInlineStart` and `paddingInline` express the same intent. Keep the RTL style transformation for physical CSS properties that need flipping, and use a no-flip annotation only when a physical direction is intentional.

Avoid hard-coding left and right for leading padding or the start edge of a card. Physical directions still make sense for concepts such as positioning a map marker at a geographic coordinate.

## Locale switching and loading

A locale switch should be an application-level state change with a clear source of truth:

1. Load the selected message resources.
2. Select the matching component locale bundle and direction.
3. Create or select the matching theme and style cache.
4. Update document language and direction before presenting the new interface.
5. Configure date, number, and time formatting with the same locale.
6. Preserve the user's current task and form values.

If resources load asynchronously, show a stable loading state or keep the previous locale until the new resources are ready. Avoid a partially translated screen. Persist the user's choice when appropriate, and respect browser or account preferences when the product uses them.

Use language names as people identify them in a language selector. Native labels such as “Français” or “العربية” can help users find their language.

## Accessibility and testing

Language and direction changes affect assistive technology and keyboard use:

- Set the correct `lang` attribute so screen readers can select pronunciation rules.
- Set `dir` on the document or region so text and punctuation are interpreted correctly.
- Keep focus visible and preserve the focused control when switching locales.
- Translate accessible names for icon-only controls.
- Do not use color or icon direction as the only indication of meaning.
- Review mixed-direction content such as email addresses, URLs, identifiers, and numbers.
- Verify dialogs and menus rendered in portals.
- Test long translations, text expansion, and font fallback.
- Check date, currency, decimal, and grouping formats with representative values.

Test at least one LTR locale and one RTL locale. Include keyboard navigation, a screen reader pass, a narrow viewport, and an interaction that opens a portal. Snapshots can catch output changes but do not replace checking rendered direction and actual text.

## Common mistakes

- Assuming a Material UI locale bundle translates application copy.
- Setting theme direction without DOM direction and the RTL style plugin.
- Setting `dir` only on a wrapper while portal content renders elsewhere.
- Treating date formatting and picker label locale as one setting.
- Formatting a stored value once and reusing that string for every locale.
- Hard-coding left and right spacing where logical properties express intent.
- Changing visible text while accessible labels remain in the previous locale.
- Loading translations after render and flashing mixed-language content.
- Mirroring every icon without checking whether it conveys direction.

## Chapter checklist

Before shipping, confirm that:

- Application messages and component locale text are both configured.
- The selected locale is used for date, number, and currency formatting.
- `lang`, DOM direction, theme direction, and style cache agree.
- Portaled components behave correctly in locally scoped RTL regions.
- Logical CSS properties cover leading and trailing layout.
- A language switch preserves focus, form data, and the current task.
- Tests cover translation, RTL layout, keyboard use, and realistic formats.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="036-accessibility-and-keyboard-interaction.md">036 - Accessibility and keyboard interaction</a></td><td><a href="../qnas/037-localization-rtl-and-internationalization.md">Q&amp;A</a></td><td><a href="../examples/037-localization-rtl-and-internationalization.md">Example</a></td><td><a href="038-server-side-rendering-and-nextjs.md">038 - Server-side rendering and Next.js</a></td></tr></tbody>
</table>
