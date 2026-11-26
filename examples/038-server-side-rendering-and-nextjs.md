# Server-Side Rendering and Next.js - Example

This example wires Material UI into a small Next.js App Router page. It keeps the route layout on the server, collects generated styles during streaming, applies a shared theme, and moves a single interactive button into a Client Component.

## What this example demonstrates

- The App Router cache provider collects Material UI styles.
- A Client Component provides the theme and baseline styles.
- Server-rendered page content stays in a Server Component.
- Only the interactive action is included in the client module graph.
- A Pages Router alternative is included for projects that use that router.

## Assumptions

The main implementation below uses Next.js 15 and the App Router. Use the integration package export matching the Next.js major version installed in your project.

Install the dependencies:

~~~bash
npm install next react react-dom
npm install @mui/material @emotion/react @emotion/styled
npm install @mui/material-nextjs @emotion/cache
~~~

## App Router implementation

### 1. Collect styles in the root layout

Create **app/layout.tsx**:

~~~tsx
import type { ReactNode } from "react";
import { AppRouterCacheProvider } from "@mui/material-nextjs/v15-appRouter";
import ThemeRegistry from "./theme-registry";

export default function RootLayout(props: {
  children: ReactNode;
}) {
  return (
    <html lang="en">
      <body>
        <AppRouterCacheProvider options={{ key: "mui" }}>
          <ThemeRegistry>{props.children}</ThemeRegistry>
        </AppRouterCacheProvider>
      </body>
    </html>
  );
}
~~~

The layout remains a Server Component. The MUI cache provider integrates Emotion style collection with Next.js streaming and places generated styles in the document head.

### 2. Add a client-side theme registry

Create **app/theme-registry.tsx**:

~~~tsx
"use client";

import type { ReactNode } from "react";
import CssBaseline from "@mui/material/CssBaseline";
import { createTheme, ThemeProvider } from "@mui/material/styles";

const theme = createTheme({
  palette: {
    primary: { main: "#3b5ccc" },
    background: { default: "#f6f7fb" },
  },
});

export default function ThemeRegistry(props: {
  children: ReactNode;
}) {
  return (
    <ThemeProvider theme={theme}>
      <CssBaseline />
      {props.children}
    </ThemeProvider>
  );
}
~~~

The theme is deterministic and shared by every render using this provider. If the theme depends on a user's preference, make the initial preference available to both the server and client so they render compatible output.

### 3. Keep the route on the server

Create **app/page.tsx**:

~~~tsx
import { Box, Container, Paper, Stack, Typography } from "@mui/material";
import ProjectActions from "./project-actions";

export default function ProjectPage() {
  const project = {
    id: "project-42",
    name: "Design system refresh",
    owner: "Product team",
  };

  return (
    <Box component="main" sx={{ py: 6 }}>
      <Container maxWidth="sm">
        <Paper component="section" elevation={2} sx={{ p: 4 }}>
          <Stack spacing={2}>
            <Typography component="p" color="text.secondary">
              {project.owner}
            </Typography>
            <Typography component="h1" variant="h4">
              {project.name}
            </Typography>
            <Typography>
              This project page is rendered as part of the server route.
            </Typography>
            <ProjectActions projectId={project.id} />
          </Stack>
        </Paper>
      </Container>
    </Box>
  );
}
~~~

In a real route, fetch protected data in the Server Component or a server-only data module. Pass only the values needed by the interactive component.

### 4. Isolate the interactive control

Create **app/project-actions.tsx**:

~~~tsx
"use client";

import * as React from "react";
import Button from "@mui/material/Button";
import Stack from "@mui/material/Stack";
import Typography from "@mui/material/Typography";

export default function ProjectActions(props: {
  projectId: string;
}) {
  const [saved, setSaved] = React.useState(false);

  function handleSave() {
    setSaved((current) => !current);
  }

  return (
    <Stack direction="row" spacing={2} alignItems="center">
      <Button
        variant="contained"
        aria-pressed={saved}
        onClick={handleSave}
      >
        {saved ? "Saved" : "Save project"}
      </Button>
      <Typography variant="body2" color="text.secondary">
        Project ID: {props.projectId}
      </Typography>
    </Stack>
  );
}
~~~

The project title and static description stay in the server-rendered route. The button needs state and an event handler, so it is a Client Component.

In a real application, saving should call a server action or API endpoint with appropriate authorization. Do not put a secret key or database connection in this client file.

### 5. Optional cascade layer configuration

If the application combines Material UI with CSS Modules, utility CSS, or plain CSS and intentionally uses cascade layers, enable the integration option in the root layout:

~~~tsx
<AppRouterCacheProvider options={{ key: "mui", enableCssLayer: true }}>
  <ThemeRegistry>{props.children}</ThemeRegistry>
</AppRouterCacheProvider>
~~~

Review the resulting cascade order. Do not enable layers without deciding how the application's other styles participate.

## Pages Router alternative

For a Pages Router project, use the Pages Router integration entry points instead of the App Router provider.

### 1. Add application providers

Create **pages/_app.tsx**:

~~~tsx
import * as React from "react";
import type { AppProps } from "next/app";
import CssBaseline from "@mui/material/CssBaseline";
import { AppCacheProvider } from "@mui/material-nextjs/v15-pagesRouter";
import { createTheme, ThemeProvider } from "@mui/material/styles";

const theme = createTheme({
  palette: {
    primary: { main: "#3b5ccc" },
  },
});

export default function MyApp(props: AppProps) {
  const { Component, pageProps } = props;

  return (
    <AppCacheProvider {...props}>
      <ThemeProvider theme={theme}>
        <CssBaseline />
        <Component {...pageProps} />
      </ThemeProvider>
    </AppCacheProvider>
  );
}
~~~

### 2. Collect style tags in the document

Create **pages/_document.tsx**:

~~~tsx
import {
  DocumentHeadTags,
  documentGetInitialProps,
} from "@mui/material-nextjs/v15-pagesRouter";
import {
  Html,
  Head,
  Main,
  NextScript,
  type DocumentProps,
} from "next/document";

export default function MyDocument(props: DocumentProps) {
  return (
    <Html lang="en">
      <Head>
        <DocumentHeadTags {...props} />
      </Head>
      <body>
        <Main />
        <NextScript />
      </body>
    </Html>
  );
}

MyDocument.getInitialProps = async (context) => {
  const initialProps = await documentGetInitialProps(context);
  return initialProps;
};
~~~

Keep only the router setup used by the project. Do not install both providers into one application just to follow both snippets.

## Production checks

Before shipping, confirm that:

- The integration import path matches the installed Next.js major.
- The server response contains Material UI style tags in the document head.
- The first paint does not flash unstyled Material UI components.
- Theme settings are identical for the initial server render and client hydration.
- The route contains no browser API access during server rendering.
- Interactive state is isolated in Client Components.
- The project builds with the production configuration.
- No hydration warnings appear in the browser console.
- Any user-specific theme or locale is selected consistently before the first render.

## Chapter navigation

<table style="width:100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="037-localization-rtl-and-internationalization.md">037 - Localization, RTL, and internationalization</a></td><td><a href="../lessons/038-server-side-rendering-and-nextjs.md">Lesson</a></td><td><a href="../qnas/038-server-side-rendering-and-nextjs.md">Q&amp;A</a></td><td><a href="039-testing-material-ui-components.md">039 - Testing Material UI components</a></td></tr></tbody>
</table>
