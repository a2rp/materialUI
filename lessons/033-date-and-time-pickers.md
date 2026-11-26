# Date and Time Pickers

MUI X Date and Time Pickers provide fields and calendar or clock views for choosing dates and times. They use an adapter to connect picker components to a date library such as Day.js, date-fns, Luxon, or Moment.

This chapter covers installation, LocalizationProvider, controlled values, responsive pickers, validation, localization, and the difference between a calendar date and a timestamp.

## Learning goals

After this chapter, you should be able to:

- Install the MUI X picker package and a compatible date library.
- Configure one date adapter for an application.
- Control picker values with React state.
- Add validation and display useful feedback.
- Choose responsive picker behavior and localize its format.
- Store selected values according to whether the domain needs a date or an instant.

## Install the picker package

Install the Community Date and Time Pickers package, Day.js, and the Material UI dependencies if the project does not already have them:

```bash
npm install @mui/x-date-pickers dayjs
npm install @mui/material @emotion/react @emotion/styled
```

The picker package supports multiple date libraries through adapters. Choose one library for the application and use its corresponding adapter. Keep the package versions compatible, especially when using libraries with multiple major versions.

## Configure LocalizationProvider

Wrap the part of the application that uses pickers in LocalizationProvider and pass the adapter for the chosen date library:

```jsx
import { AdapterDayjs } from "@mui/x-date-pickers/AdapterDayjs";
import { LocalizationProvider } from "@mui/x-date-pickers/LocalizationProvider";

function AppProviders({ children }) {
  return (
    <LocalizationProvider dateAdapter={AdapterDayjs}>
      {children}
    </LocalizationProvider>
  );
}
```

Usually one provider near the application root is enough. It supplies the adapter context used by DatePicker, TimePicker, and DateTimePicker descendants.

## Controlled DatePicker

A controlled picker receives its current value through value and reports changes through onChange. Use null for an empty value. With Day.js, the value is a Day.js object rather than a JavaScript Date.

```jsx
import { useState } from "react";
import dayjs from "dayjs";
import { DatePicker } from "@mui/x-date-pickers/DatePicker";

function DueDateField() {
  const [dueDate, setDueDate] = useState(dayjs());

  return (
    <DatePicker
      label="Project due date"
      value={dueDate}
      onChange={(nextValue) => setDueDate(nextValue)}
      minDate={dayjs()}
      slotProps={{
        textField: {
          helperText: "Choose today or a later date.",
        },
      }}
    />
  );
}
```

The picker can also manage its own state when you provide defaultValue. Choose controlled state when the surrounding form or application needs to validate, submit, reset, or coordinate the value. Do not mix value and defaultValue for the same field.

## Date, time, and date-time choices

Choose the component that matches the data:

- DatePicker selects a calendar date.
- TimePicker selects a time of day.
- DateTimePicker selects a combined date and time.

A calendar date such as a birthday is not necessarily a moment on the global timeline. A timestamp such as a meeting start represents an instant and may need timezone handling. Decide the domain meaning before converting the picker value into a string or server payload.

## Responsive behavior

DatePicker is the responsive component. It uses a desktop picker for fine pointer environments and a mobile picker for touch-oriented environments by default. The desktop variant presents views in a popover; the mobile variant uses a modal. You can choose DesktopDatePicker or MobileDatePicker explicitly when the interface requires a fixed interaction.

Responsive behavior relies on media queries. In server rendering and some test environments, media queries may not match the eventual client environment. Test both variants and provide a deliberate configuration when the default query is unsuitable.

## Validation

Use picker validation props to constrain values and give feedback:

```jsx
<DatePicker
  label="Start date"
  value={startDate}
  onChange={setStartDate}
  minDate={dayjs("2026-01-01")}
  maxDate={dayjs("2026-12-31")}
  disablePast
/>
```

minDate and maxDate set inclusive calendar boundaries. disablePast and disableFuture prevent dates outside the current date range. Use shouldDisableDate for domain rules such as unavailable days, and keep that callback efficient because it may run for many rendered days.

Validation affects picker fields and calendar choices, but application code should still validate submitted data. A user can enter values in a field, and server-side rules may differ from what the client currently displays.

## Show validation feedback

The onError callback can track the picker's validation state. Use that state to render field-level guidance:

```jsx
const [pickerError, setPickerError] = useState(null);

<DatePicker
  label="Delivery date"
  value={deliveryDate}
  onChange={setDeliveryDate}
  minDate={dayjs()}
  onError={(reason) => setPickerError(reason)}
  slotProps={{
    textField: {
      helperText: pickerError
        ? "Enter an allowed delivery date."
        : "The date must be today or later.",
    },
  }}
/>
```

Explain how to correct an invalid value instead of relying only on a red border. Keep the error message connected to the field and clear it when the value becomes valid.

## Format and localization

LocalizationProvider accepts locale configuration for the adapter, while picker text can be translated through locale text settings. Date format, week start, labels, and validation messages should match the users' locale and application language.

Do not assume that changing the displayed format also changes the stored value. Display formatting and serialization are separate concerns. Test dates near midnight and daylight-saving transitions when the application handles timestamps.

## Date library and serialization

Use the date library consistently from input through validation and submission. At the API boundary, explicitly serialize the domain value:

- For a date-only field, preserve the calendar date as a date-only value.
- For an instant, convert to a well-defined timestamp and decide which timezone the product uses.
- For a time-only value, preserve the intended local time and any timezone or location context required by the domain.

Avoid converting a local calendar date to a UTC timestamp without considering timezone offsets. That conversion can shift the visible calendar day for users in another timezone.

## Common mistakes

- Forgetting to wrap picker components with LocalizationProvider.
- Installing the picker package without a compatible adapter library.
- Passing a JavaScript Date when the configured adapter expects a different date-library value.
- Mixing controlled value and defaultValue.
- Treating an invalid client field as valid server data.
- Using expensive logic in shouldDisableDate.
- Assuming responsive pickers choose the same variant in every test or server-rendered environment.
- Treating a date-only value as a timestamp.
- Assuming display format determines API serialization.
- Showing a validation border without explaining how to fix the value.

## Recap

- Date and time pickers are a separate MUI X package and require a compatible adapter and date library.
- Keep controlled values, validation, locale, time zone, and serialization rules explicit.
- Choose DatePicker, TimePicker, or DateTimePicker according to the value users must enter.
- Connect validation feedback to the field and test boundary dates, invalid values, and responsive behavior.

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
      <td><a href="032-icons-and-icon-buttons.md">032 - Icons and Icon Buttons</a></td>
      <td><a href="../qnas/033-date-and-time-pickers.md">Q&amp;A</a></td>
      <td><a href="../examples/033-date-and-time-pickers.md">Example</a></td>
      <td><a href="034-component-composition-slots-and-customization.md">034 - Component Composition, Slots, and Customization</a></td>
    </tr>
  </tbody>
</table>
