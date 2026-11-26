# Localization, RTL, and Internationalization - Q&A

This Q&A reviews the concepts and implementation choices from Chapter 037.

## Concepts

### 1. What is the difference between internationalization and localization?

Internationalization prepares an application to support multiple languages, regions, formats, and writing directions. Localization adapts the interface for a specific locale, including translated messages and regional conventions.

### 2. Is a language code alone enough to identify a locale?

Not always. A locale can include language, region, and script. Region can affect spelling, dates, currency, and number separators, so use an identifier appropriate for the product and user preference.

### 3. What different kinds of localization may a Material UI application need?

Consider application messages, Material UI component labels, date-library locale, text direction, and formatting for dates, numbers, and currency. They are separate configuration layers.

### 4. Does a Material UI locale bundle translate the application's headings and buttons?

No. It translates built-in Material UI component text. Application-owned messages must come from message resources or a translation system.

### 5. Do MUI X components automatically use the Material UI core locale bundle?

No. Separately distributed component packages can have their own localization mechanisms. Configure the package that actually renders the component.

### 6. Why should translations use stable message keys?

Stable keys keep behavior independent from translated wording. Code and tests can refer to a key while translators change the visible message or its word order.

### 7. Why is manual string concatenation risky for translated messages?

Languages can place words and values in different orders, and plural grammar varies. Use a translation system with interpolation and plural support.

### 8. What is a fallback locale?

It is a complete language resource used when a selected translation is unavailable. A complete fallback avoids missing labels or blank sections while a locale is being added or loaded.

## Formatting

### 9. How should a date or amount be stored if it must be shown in several locales?

Store the underlying date or numeric value in a canonical data type, then format it when displaying it. Do not store one locale's formatted string as the data itself.

### 10. What does Intl.DateTimeFormat do?

It formats a date according to a locale and chosen options. Options can include date style and time zone. The time zone matters because the same instant can fall on different local dates.

### 11. What does Intl.NumberFormat do?

It formats numbers with locale-specific separators and conventions. With currency style and a currency code, it also renders a localized currency value.

### 12. Why should a product choose a time zone explicitly for some date displays?

A timestamp represents an instant, but its displayed calendar date depends on a time zone. Choosing the intended time zone avoids results that differ from the product's business meaning or the user's expectation.

## Material UI and RTL

### 13. How do you apply a Material UI locale bundle?

Import the bundle for the selected locale, pass it as a locale argument when creating the theme, and provide the resulting theme through ThemeProvider. Keep application translations configured separately.

### 14. What are the three parts of a typical Material UI RTL setup?

Set DOM direction with dir="rtl", set direction: "rtl" in the theme, and configure the style engine with the RTL transformation plugin. Omitting one part can produce a partially mirrored interface.

### 15. Why does the Emotion RTL cache include the prefixer before the RTL plugin?

The Emotion cache must run the style transformations that produce prefixed CSS and flip directional declarations. The documented setup lists the prefixer before the RTL plugin so generated styles receive the intended processing.

### 16. Is dir="rtl" on a wrapper sufficient for every component inside it?

No. Components rendered in portals can appear elsewhere in the DOM and do not inherit a local wrapper's direction. Set direction on the document when appropriate, or configure the portaled component itself when only a subtree is RTL.

### 17. What should happen when the user changes locale at runtime?

Update message resources, component locale text, direction, document lang and dir, formatting settings, and any date adapter locale as one coherent change. Preserve focus and in-progress form data.

### 18. Should every icon be mirrored in RTL?

No. Mirror icons whose meaning represents direction, such as forward and back navigation, when the design calls for it. An invariant or decorative icon may keep its shape.

### 19. Why prefer CSS logical properties for leading and trailing spacing?

Logical properties such as margin-inline-start describe a writing-direction-aware edge. They work in both LTR and RTL without hard-coding left and right.

### 20. When is a physical left or right property still appropriate?

Use a physical direction when the meaning is physically fixed, such as a real-world coordinate. For general leading and trailing layout, use logical properties.

## Date and time pickers

### 21. What is the difference between a picker adapter locale and picker localeText?

The adapter locale controls date-library conventions such as date format and month names. localeText controls labels and translated component strings. A localized picker may need both.

### 22. Why might a date picker display a localized date but English button labels?

The date adapter locale may be set while picker component text is not. Configure translated component labels through the picker localization mechanism as well.

### 23. Why should date-library and MUI X versions be checked together?

Adapters and localization APIs are package-specific and can change between versions. Use compatible versions and configure the API available in the installed version.

## Accessibility and testing

### 24. What does the HTML lang attribute contribute?

It identifies the language of the page or a text region, helping assistive technology apply appropriate pronunciation and language behavior.

### 25. What should be tested for a right-to-left interface?

Test direction at page and component level, portal-based menus and dialogs, keyboard focus, icon meaning, mixed-direction strings, narrow layouts, and actual translated content.

### 26. Why is a snapshot test not enough to validate localization?

A snapshot can detect changed output, but it does not prove that text is understandable, direction is correct, focus remains usable, or assistive technology receives the right language and accessible names.

### 27. What is a common cause of mixed-language UI during locale switching?

Some resources or component settings update later than others. Load the next locale's resources before applying the change, or keep the previous locale visible until the new configuration is ready.

### 28. What should a language selector display?

Use clear language names users recognize, often including native labels such as “Français” or “العربية.” The selector should remain understandable while the rest of the interface changes language.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="036-accessibility-and-keyboard-interaction.md">036 - Accessibility and keyboard interaction</a></td><td><a href="../lessons/037-localization-rtl-and-internationalization.md">Lesson</a></td><td><a href="../examples/037-localization-rtl-and-internationalization.md">Example</a></td><td><a href="038-server-side-rendering-and-nextjs.md">038 - Server-side rendering and Next.js</a></td></tr></tbody>
</table>
