# Localization, RTL, and Internationalization - Example

This example builds a preferences screen with English, French, and Arabic. One locale selection updates application messages, Material UI labels, date picker labels, date formatting, number formatting, text direction, and the Emotion style cache.

## What this example demonstrates

- A single locale selection drives application messages.
- Material UI and MUI X Date Picker use their own locale bundles.
- Intl.NumberFormat formats a currency value for the active locale.
- Arabic switches the document region and theme to right-to-left.
- Emotion applies the RTL Stylis plugin only in RTL mode.
- A dialog receives its own direction because it renders in a portal.

## Assumptions

The snippets are for a React application using the Emotion style engine. Install packages compatible with the React and Material UI versions in your project:

~~~bash
npm install @mui/material @emotion/react @emotion/styled @emotion/cache
npm install @mui/stylis-plugin-rtl stylis @mui/x-date-pickers dayjs
~~~

The application already has a React entry point that renders App. This example is a client-rendered single page app. In a server-rendered app, render the selected language and direction in the initial HTML too.

## 1. Define locale resources

Create **src/i18n/locales.js**. Keep application-owned strings alongside framework locale bundles so one locale switch updates them together.

~~~jsx
import "dayjs/locale/en";
import "dayjs/locale/fr";
import "dayjs/locale/ar";

import { enUS, frFR, arSD } from "@mui/material/locale";
import {
  enUS as pickerEnUS,
  frFR as pickerFrFR,
  arSD as pickerArSD,
} from "@mui/x-date-pickers/locales";

export const locales = {
  "en-US": {
    label: "English (United States)",
    direction: "ltr",
    adapterLocale: "en",
    materialLocale: enUS,
    pickerLocale: pickerEnUS,
    messages: {
      title: "Project preferences",
      language: "Interface language",
      date: "Review date",
      budget: "Current budget",
      openDialog: "Review settings",
      dialogTitle: "Settings review",
      dialogBody: "Your preferences are saved on this device.",
      close: "Close",
    },
  },
  "fr-FR": {
    label: "Français",
    direction: "ltr",
    adapterLocale: "fr",
    materialLocale: frFR,
    pickerLocale: pickerFrFR,
    messages: {
      title: "Préférences du projet",
      language: "Langue de l’interface",
      date: "Date de révision",
      budget: "Budget actuel",
      openDialog: "Vérifier les paramètres",
      dialogTitle: "Vérification des paramètres",
      dialogBody: "Vos préférences sont enregistrées sur cet appareil.",
      close: "Fermer",
    },
  },
  "ar-SD": {
    label: "العربية",
    direction: "rtl",
    adapterLocale: "ar",
    materialLocale: arSD,
    pickerLocale: pickerArSD,
    messages: {
      title: "تفضيلات المشروع",
      language: "لغة الواجهة",
      date: "تاريخ المراجعة",
      budget: "الميزانية الحالية",
      openDialog: "مراجعة الإعدادات",
      dialogTitle: "مراجعة الإعدادات",
      dialogBody: "تم حفظ تفضيلاتك على هذا الجهاز.",
      close: "إغلاق",
    },
  },
};
~~~

Use translated messages from a real localization workflow in production. This compact resource object keeps the example self-contained, but a translation platform or message library is a better fit for plural rules, interpolation, loading, and a large catalog.

## 2. Create LTR and RTL Emotion caches

Create **src/theme/styleCaches.js**. The RTL cache includes the prefixer before the direction transformation plugin. The LTR cache keeps Emotion's normal defaults.

~~~jsx
import createCache from "@emotion/cache";
import { prefixer } from "stylis";
import rtlPlugin from "@mui/stylis-plugin-rtl";

export const ltrCache = createCache({
  key: "mui",
});

export const rtlCache = createCache({
  key: "muirtl",
  stylisPlugins: [prefixer, rtlPlugin],
});
~~~

If a server extracts or rehydrates Emotion styles, configure its cache and insertion behavior for the framework rather than copying this client-only setup unchanged.

## 3. Provide the active locale

Create **src/i18n/LocaleProvider.jsx**. The selected resource drives both the Material UI theme and the date picker localization provider.

~~~jsx
import * as React from "react";
import { CacheProvider } from "@emotion/react";
import { createTheme, ThemeProvider } from "@mui/material/styles";
import { LocalizationProvider } from "@mui/x-date-pickers/LocalizationProvider";
import { AdapterDayjs } from "@mui/x-date-pickers/AdapterDayjs";

import { locales } from "./locales";
import { ltrCache, rtlCache } from "../theme/styleCaches";

export const LocaleContext = React.createContext(null);

export function LocaleProvider({ locale, children }) {
  const active = locales[locale] ?? locales["en-US"];

  const theme = React.useMemo(
    () => createTheme({ direction: active.direction }, active.materialLocale),
    [active],
  );

  const cache = active.direction === "rtl" ? rtlCache : ltrCache;
  const pickerText =
    active.pickerLocale.components.MuiLocalizationProvider.defaultProps.localeText;

  React.useEffect(() => {
    document.documentElement.lang = locale;
    document.documentElement.dir = active.direction;
  }, [locale, active.direction]);

  const value = { locale, ...active };

  return (
    <CacheProvider value={cache}>
      <ThemeProvider theme={theme}>
        <LocalizationProvider
          dateAdapter={AdapterDayjs}
          adapterLocale={active.adapterLocale}
          localeText={pickerText}
        >
          <LocaleContext.Provider value={value}>
            {children}
          </LocaleContext.Provider>
        </LocalizationProvider>
      </ThemeProvider>
    </CacheProvider>
  );
}

export function useLocale() {
  const value = React.useContext(LocaleContext);
  if (!value) {
    throw new Error("useLocale must be used inside LocaleProvider");
  }
  return value;
}
~~~

The effect synchronizes document language and direction in this client-rendered example. A server-rendered application should put the selected values on the document before the first render to prevent a direction flash or hydration mismatch.

## 4. Build a localized preferences screen

Create **src/SettingsScreen.jsx**. The dialog gets an explicit direction because portal content does not inherit direction from a local wrapper.

~~~jsx
import * as React from "react";
import {
  Button,
  Dialog,
  DialogActions,
  DialogContent,
  DialogTitle,
  MenuItem,
  Stack,
  TextField,
  Typography,
} from "@mui/material";
import { DatePicker } from "@mui/x-date-pickers/DatePicker";

import { useLocale } from "./i18n/LocaleProvider";

export default function SettingsScreen({ onLocaleChange }) {
  const [dialogOpen, setDialogOpen] = React.useState(false);
  const { locale, direction, messages } = useLocale();

  const budget = new Intl.NumberFormat(locale, {
    style: "currency",
    currency: "USD",
    maximumFractionDigits: 0,
  }).format(12500);

  return (
    <Stack
      component="main"
      spacing={3}
      sx={{
        maxWidth: 560,
        mx: "auto",
        my: 5,
        px: 3,
        textAlign: "start",
      }}
    >
      <Typography component="h1" variant="h4">
        {messages.title}
      </Typography>

      <TextField
        select
        id="interface-language"
        label={messages.language}
        value={locale}
        onChange={(event) => onLocaleChange(event.target.value)}
      >
        <MenuItem value="en-US">English (United States)</MenuItem>
        <MenuItem value="fr-FR">Français</MenuItem>
        <MenuItem value="ar-SD">العربية</MenuItem>
      </TextField>

      <DatePicker label={messages.date} />

      <Typography component="p">
        {messages.budget}: {budget}
      </Typography>

      <Button
        variant="contained"
        onClick={() => setDialogOpen(true)}
      >
        {messages.openDialog}
      </Button>

      <Dialog
        dir={direction}
        open={dialogOpen}
        onClose={() => setDialogOpen(false)}
        aria-labelledby="settings-dialog-title"
        aria-describedby="settings-dialog-description"
      >
        <DialogTitle id="settings-dialog-title">
          {messages.dialogTitle}
        </DialogTitle>
        <DialogContent id="settings-dialog-description">
          {messages.dialogBody}
        </DialogContent>
        <DialogActions>
          <Button onClick={() => setDialogOpen(false)}>
            {messages.close}
          </Button>
        </DialogActions>
      </Dialog>
    </Stack>
  );
}
~~~

The selector keeps native language names visible so users can find a language even when the rest of the page is unfamiliar. The date picker receives both the date adapter locale and translated picker labels.

## 5. Connect the locale state

Create **src/App.jsx**. Locale state belongs above the provider because it selects the provider configuration.

~~~jsx
import * as React from "react";
import { LocaleProvider } from "./i18n/LocaleProvider";
import SettingsScreen from "./SettingsScreen";

export default function App() {
  const [locale, setLocale] = React.useState("en-US");

  return (
    <LocaleProvider locale={locale}>
      <SettingsScreen onLocaleChange={setLocale} />
    </LocaleProvider>
  );
}
~~~

For a production app, persist the selected locale if appropriate and decide how it interacts with browser or account preferences. Keep domain values independent from translated display strings.

## 6. Use logical CSS properties

If custom CSS is needed, describe inline edges rather than assuming that leading content is always on the left:

~~~css
.settingsNote {
  margin-inline-start: 1rem;
  padding-inline: 1.25rem;
  border-inline-start: 3px solid currentColor;
  text-align: start;
}
~~~

Material UI layout props can be combined with logical CSS properties. Check custom styles in both directions; an RTL plugin cannot determine whether every physical left or right declaration is intentional.

## Verify the example

Switch between all three locales and confirm that:

- Application messages, Material UI labels, and picker labels use the selected language.
- The date field uses the selected date adapter locale.
- Currency separators and symbols follow the selected locale.
- Arabic sets document direction, theme direction, and Emotion cache to RTL.
- The dialog is RTL even though it renders in a portal.
- Focus remains visible and returns correctly after the dialog closes.
- Text remains readable at a narrow viewport and with longer translated labels.
- Production server-rendered HTML starts with the selected language and direction.

## Chapter navigation

<table style="width:100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="036-accessibility-and-keyboard-interaction.md">036 - Accessibility and keyboard interaction</a></td><td><a href="../lessons/037-localization-rtl-and-internationalization.md">Lesson</a></td><td><a href="../qnas/037-localization-rtl-and-internationalization.md">Q&amp;A</a></td><td><a href="038-server-side-rendering-and-nextjs.md">038 - Server-side rendering and Next.js</a></td></tr></tbody>
</table>
