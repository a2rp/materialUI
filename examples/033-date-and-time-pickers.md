# Date and Time Pickers: Appointment Form Example

This example creates an appointment form with a weekday-only date and a time. It uses Day.js through LocalizationProvider, controlled picker values, validation feedback, and a final check before displaying the scheduled local date and time.

## What you will build

- A controlled DatePicker and TimePicker.
- A weekday-only date rule.
- A minimum date of today.
- Field helper text that explains the validation rule.
- A submit action that combines the chosen date and time.

## Requirements

Install the picker package, Day.js, Material UI, and its default styling engine:

```bash
npm install @mui/x-date-pickers dayjs @mui/material @emotion/react @emotion/styled
```

## `src/App.jsx`

Replace the starter component with this file:

```jsx
import { useState } from "react";
import dayjs from "dayjs";
import {
  Alert,
  Box,
  Button,
  Container,
  Stack,
  Typography,
} from "@mui/material";
import { AdapterDayjs } from "@mui/x-date-pickers/AdapterDayjs";
import { DatePicker } from "@mui/x-date-pickers/DatePicker";
import { LocalizationProvider } from "@mui/x-date-pickers/LocalizationProvider";
import { TimePicker } from "@mui/x-date-pickers/TimePicker";

function isWeekend(date) {
  const dayOfWeek = date.day();
  return dayOfWeek === 0 || dayOfWeek === 6;
}

export default function App() {
  const [date, setDate] = useState(null);
  const [time, setTime] = useState(dayjs().hour(9).minute(0).second(0));
  const [dateError, setDateError] = useState(null);
  const [timeError, setTimeError] = useState(null);
  const [scheduledAt, setScheduledAt] = useState("");

  function handleSubmit(event) {
    event.preventDefault();

    if (!date || !time || dateError || timeError || isWeekend(date)) {
      setScheduledAt("");
      return;
    }

    const localAppointment = date
      .hour(time.hour())
      .minute(time.minute())
      .second(0)
      .millisecond(0);

    setScheduledAt(localAppointment.format("dddd, MMMM D, YYYY [at] h:mm A"));
  }

  return (
    <LocalizationProvider dateAdapter={AdapterDayjs}>
      <Container maxWidth="sm" sx={{ py: 5 }}>
        <Typography component="h1" variant="h4" fontWeight={700} sx={{ mb: 1 }}>
          Schedule an appointment
        </Typography>
        <Typography color="text.secondary" sx={{ mb: 3 }}>
          Choose a weekday and an appointment time.
        </Typography>

        <Box component="form" onSubmit={handleSubmit}>
          <Stack spacing={2}>
            <DatePicker
              label="Appointment date"
              value={date}
              onChange={(nextDate) => {
                setDate(nextDate);
                setScheduledAt("");
              }}
              onError={(reason) => setDateError(reason)}
              minDate={dayjs().startOf("day")}
              shouldDisableDate={isWeekend}
              slotProps={{
                textField: {
                  helperText: dateError
                    ? "Choose a valid weekday today or later."
                    : "Weekends are unavailable.",
                },
              }}
            />

            <TimePicker
              label="Appointment time"
              value={time}
              onChange={(nextTime) => {
                setTime(nextTime);
                setScheduledAt("");
              }}
              onError={(reason) => setTimeError(reason)}
              slotProps={{
                textField: {
                  helperText: timeError
                    ? "Choose a valid time."
                    : "The selected time uses your local clock.",
                },
              }}
            />

            <Button
              type="submit"
              variant="contained"
              disabled={!date || !time || Boolean(dateError) || Boolean(timeError)}
            >
              Schedule appointment
            </Button>

            {scheduledAt && (
              <Alert severity="success">
                Appointment scheduled for {scheduledAt}.
              </Alert>
            )}
          </Stack>
        </Box>
      </Container>
    </LocalizationProvider>
  );
}
```

## `src/main.jsx`

Use the Vite entry point to mount the form:

```jsx
import React from "react";
import ReactDOM from "react-dom/client";
import App from "./App.jsx";

ReactDOM.createRoot(document.getElementById("root")).render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
);
```

## How the example works

### Adapter and provider

LocalizationProvider makes the Day.js adapter available to both pickers. A larger application can move the provider above the route or form so the adapter is configured once.

### Controlled fields

The date and time values are held in React state. Each picker receives value and onChange. The date begins as null so the user must choose a valid day.

### Date validation

minDate disallows dates before today, while shouldDisableDate rejects Saturday and Sunday. The picker shows disabled days in the calendar and reports invalid field values through onError.

The submit handler checks the selected values again. Client-side picker validation improves the form experience but does not replace validation by the application or server.

### Combining date and time

The form uses the selected calendar date and the time-of-day fields from the TimePicker, then formats the result using the configured date library. This example treats the appointment as local time. A real scheduling system should define the applicable timezone and send that context to its server.

## Try these changes

1. Select a weekday and submit the form.
2. Try to select a weekend in the calendar. The picker disables it.
3. Attempt to type a date before today and inspect the helper message.
4. Change the TimePicker value and submit again.
5. Change shouldDisableDate to disable a holiday list as well as weekends.
6. Decide whether the product should use the user's local time or a named business timezone.

## Common mistakes

- Forgetting LocalizationProvider or using an adapter for a different date library.
- Passing a value type that does not match the adapter.
- Relying on the calendar's disabled days without validating submission.
- Leaving the date empty but still allowing submission.
- Performing expensive work inside shouldDisableDate.
- Formatting a local date-time as though it were a UTC instant.
- Forgetting to connect an error state to helpful text.

## Chapter navigation

<table width="100%">
  <thead>
    <tr>
      <th>Previous example</th>
      <th>Chapter lesson</th>
      <th>Chapter Q&amp;A</th>
      <th>Next example</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="032-icons-and-icon-buttons.md">032 - Icons and Icon Buttons</a></td>
      <td><a href="../lessons/033-date-and-time-pickers.md">Lesson</a></td>
      <td><a href="../qnas/033-date-and-time-pickers.md">Q&amp;A</a></td>
      <td><a href="034-component-composition-slots-and-customization.md">034 - Component Composition, Slots, and Customization</a></td>
    </tr>
  </tbody>
</table>
