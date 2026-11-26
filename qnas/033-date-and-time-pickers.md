# Date and Time Pickers: Q&A

This Q&A accompanies [033 - Date and Time Pickers](../lessons/033-date-and-time-pickers.md). It covers adapters, picker values, validation, localization, responsive behavior, and date serialization.

## Setup and values

### 1. Why do MUI X pickers need a date adapter?

The adapter connects picker components to a date library's value type, parsing, formatting, and date operations. Configure the matching adapter through LocalizationProvider.

### 2. Which date library should I use?

Choose one that fits the application and its existing dependencies. MUI X supports libraries including Day.js, date-fns, Luxon, and Moment. Keep the selected library and adapter versions compatible.

### 3. Why install Day.js separately from the picker package?

The picker package provides the UI and adapter integration, while Day.js is a separate date library. Install both when using AdapterDayjs.

### 4. Is a picker value a JavaScript Date?

Not necessarily. The value type depends on the configured adapter. With AdapterDayjs, use Day.js values. Do not pass a JavaScript Date unless the configured adapter expects that type.

### 5. Should I use value or defaultValue?

Use value with onChange when React or a form library owns the current value. Use defaultValue when the picker can manage its initial value internally. Do not mix both patterns for the same field.

### 6. How do I represent an empty picker?

Use null for an empty value. Avoid undefined and invalid date strings as substitutes for a deliberate empty state.

### 7. Where should LocalizationProvider go?

Place it around the part of the application that uses pickers. A shared provider near the application root is usually enough, and avoids repeating adapter setup for each field.

## Picker behavior

### 8. What is the difference between DatePicker, TimePicker, and DateTimePicker?

DatePicker selects a calendar date, TimePicker selects a time of day, and DateTimePicker selects both. Choose based on the domain value the form needs to collect.

### 9. Does DatePicker always show the same interface?

The responsive DatePicker chooses a desktop or mobile variant based on the device environment. Desktop uses a popover, and mobile uses a modal. Use the explicit desktop or mobile component when the product requires a fixed interaction.

### 10. Why does the picker look different in a test or server-rendered page?

Its responsive variant depends on a media query. A server or test environment may not provide the same pointer information as the browser. Configure or mock the media query behavior when deterministic rendering is required.

### 11. What does minDate do?

It prevents choosing dates earlier than the supplied boundary. maxDate prevents choosing dates later than its boundary. Use both when the allowed range has fixed limits.

### 12. What is the difference between disablePast and minDate?

disablePast is relative to the current date. minDate uses an explicit date boundary. Use the explicit boundary for a business rule such as a contract start; use disablePast when the policy is relative to today.

### 13. How can I disable particular days?

Use shouldDisableDate to return true for unavailable dates. Keep the callback inexpensive because it can run repeatedly while calendar views are rendered.

### 14. Do picker validation props replace form validation?

No. Picker validation improves the editing experience, but submitted values still need application and server validation. The server remains the source of truth for data rules.

### 15. How can I show a useful validation message?

Track the reason from onError or validate the value in the form, then pass explanatory helper text to the field through the picker slot props. Tell users what value is acceptable, not only that the field is invalid.

## Localization and data handling

### 16. Is changing the display format the same as changing the stored value?

No. Display formatting and serialization are separate. A picker can show a localized date while the application serializes a domain-specific date or timestamp.

### 17. What is the difference between adapterLocale and localeText?

adapterLocale configures date-library locale behavior such as date formatting and week conventions. localeText translates picker interface strings. An application may need both.

### 18. How should I store a date-only value?

Preserve it as a calendar date, such as a date-only representation, instead of converting it blindly to a UTC timestamp. A timestamp conversion can shift the day for users in another timezone.

### 19. How should I store a date-time value?

Decide whether the value represents an absolute instant or a local time tied to a timezone or location. Serialize it consistently with that decision and test timezone boundaries.

### 20. What should I test around daylight-saving transitions?

Test dates and times that occur near the transition, including nonexistent or repeated local times where relevant. The correct behavior depends on whether the product models local wall-clock time or a global instant.

## Common issues

### 21. Why does a picker not render in my component tree?

Check that LocalizationProvider is an ancestor, that its adapter matches the chosen date library, and that the required packages are installed. Also verify that the value has the type expected by that adapter.

### 22. Why are old dates still selectable after midnight?

The current time used for disablePast is computed when LocalizationProvider first renders and does not automatically advance during that provider's lifetime. If the application can remain open across midnight, refresh or recompute the relevant boundary in application state.

### 23. When should I use onAccept instead of onChange?

onChange reports edits as the picker value changes. onAccept is useful when the application should act only after the user confirms a value. Choose based on whether the form should react to intermediate edits or only an accepted value.

### 24. Should date range pickers be part of the Community package?

Check the current package and plan for the specific range component. Some range pickers are in commercial MUI X packages. Do not assume that every picker family is included in the Community package.

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
      <td><a href="032-icons-and-icon-buttons.md">032 - Icons and Icon Buttons</a></td>
      <td><a href="../lessons/033-date-and-time-pickers.md">Lesson</a></td>
      <td><a href="../examples/033-date-and-time-pickers.md">Example</a></td>
      <td><a href="034-component-composition-slots-and-customization.md">034 - Component Composition, Slots, and Customization</a></td>
    </tr>
  </tbody>
</table>
