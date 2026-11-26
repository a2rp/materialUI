# Server-Side Rendering and Next.js

Server-side rendering sends useful HTML to the browser before client JavaScript becomes interactive. With Material UI, server-rendered markup and generated CSS must arrive together, and the client must hydrate a compatible component tree with the same theme and cache settings.

Next.js supports more than one rendering model. The right setup depends on whether the project uses the App Router or Pages Router. Material UI provides framework integration helpers for both.

## Learning goals

After this chapter, you should be able to:

- Explain how server rendering, streaming, and hydration fit together.
- Integrate Material UI with the Next.js App Router.
- Set up the Pages Router with its document and app integration points.
- Keep server and client component responsibilities clear.
- Avoid common hydration, styling, and cache mistakes.
- Make decisions about CSS layers, fonts, and responsive rendering.

## Rendering, CSS, and hydration

On an initial request, Next.js can render page markup on the server and send it with the response. The browser can display that HTML while JavaScript loads. Hydration attaches event handlers to client component output so it can respond to user input.

A Material UI page also needs its generated CSS. If markup arrives without styles, the page can briefly appear unstyled. If server and client produce incompatible markup or styles, hydration warnings, visual shifts, or duplicated CSS can result.

The framework integration coordinates:

1. Rendering the React tree on the server.
2. Collecting styles produced during rendering.
3. Placing those styles in the document in the right order.
4. Reusing compatible configuration during client hydration.

Use the official Next.js integration package for the router in your application. Avoid adding a second manual Emotion extraction system on top of the framework helper.

## App Router fundamentals

The App Router uses layouts and pages as Server Components by default. A Server Component can fetch server-side data, read secrets that must not reach the browser, and render static structure. Interactive controls, browser APIs, React state, and context belong in Client Components.

Material UI components that require client-side React behavior should be rendered within a client boundary. A provider can expose the theme and context to descendants while the root layout remains a Server Component.

The provider does not turn all server-rendered children into client code simply because it wraps them. Keep the client boundary focused and pass route content through the provider's children slot.

## Install the App Router integration

Install Material UI and the Next.js integration package, plus Emotion's cache package:

~~~bash
npm install @mui/material @emotion/react @emotion/styled
npm install @mui/material-nextjs @emotion/cache
~~~

Use the integration export that matches the Next.js major version in the installed package. The following layout uses the App Router integration for Next.js 15. For another major version, use its matching export.

## Collect styles in the root layout

In **app/layout.tsx**, keep the root layout a Server Component and wrap the body content with the cache provider:

~~~tsx
import { AppRouterCacheProvider } from "@mui/material-nextjs/v15-appRouter";

export default function RootLayout(props: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>
        <AppRouterCacheProvider>
          {props.children}
        </AppRouterCacheProvider>
      </body>
    </html>
  );
}
~~~

The cache provider collects MUI System styles as Next.js streams the page and places them in the document head. This reduces unstyled flashes and keeps styles out of the body.

The cache provider is recommended for App Router integration. It is not a replacement for a theme provider; it handles style collection and cache coordination.

## Add a client-side theme provider

Create **app/theme-provider.tsx**. The directive must appear before imports because this component creates a client boundary:

~~~tsx
"use client";

import * as React from "react";
import CssBaseline from "@mui/material/CssBaseline";
import { createTheme, ThemeProvider } from "@mui/material/styles";

const theme = createTheme({
  palette: {
    primary: { main: "#3b5ccc" },
  },
});

export default function ThemeRegistry(props: {
  children: React.ReactNode;
}) {
  return (
    <ThemeProvider theme={theme}>
      <CssBaseline />
      {props.children}
    </ThemeProvider>
  );
}
~~~

Then render the provider from the Server Component layout:

~~~tsx
import { AppRouterCacheProvider } from "@mui/material-nextjs/v15-appRouter";
import ThemeRegistry from "./theme-provider";

export default function RootLayout(props: {
  children: React.ReactNode;
}) {
  return (
    <html lang="en">
      <body>
        <AppRouterCacheProvider>
          <ThemeRegistry>{props.children}</ThemeRegistry>
        </AppRouterCacheProvider>
      </body>
    </html>
  );
}
~~~

Keep providers as deep in the tree as practical. Wrapping only the page subtree that needs the theme gives Next.js more freedom to optimize static server-rendered regions.

A Client Component provider can accept Server Component output through its children prop. Keep data fetching and secret access in Server Components, then pass serializable values or rendered children into client controls.

## Client component boundaries

Add the client directive to the smallest component that needs state, event handlers, effects, or browser APIs:

~~~tsx
"use client";

import * as React from "react";
import Button from "@mui/material/Button";

export default function FavoriteButton() {
  const [favorite, setFavorite] = React.useState(false);

  return (
    <Button
      aria-pressed={favorite}
      onClick={() => setFavorite((current) => !current)}
    >
      {favorite ? "Saved" : "Save"}
    </Button>
  );
}
~~~

The directive creates a client module boundary. Modules imported by that Client Component become part of its client bundle, so placing it high in the tree can send more JavaScript than needed. A Server Component can still render outside that boundary and pass its output as children.

Do not pass functions, class instances, or other non-serializable values from a Server Component to a Client Component unless the framework explicitly supports that boundary. Use server actions or API boundaries for server operations rather than shipping credentials or database access to the browser.

## Pages Router fundamentals

The Pages Router uses **pages/_app.tsx** for application-wide providers and **pages/_document.tsx** to customize the document and collect styles during server rendering.

Install the integration packages:

~~~bash
npm install @mui/material @emotion/react @emotion/styled
npm install @mui/material-nextjs @emotion/cache @emotion/server
~~~

Use the Next.js version-specific export that matches the installed Next.js major. The following examples show the Pages Router integration for Next.js 15.

## Wrap the Pages Router application

In **pages/_app.tsx**, use the cache provider and shared theme:

~~~tsx
import * as React from "react";
import CssBaseline from "@mui/material/CssBaseline";
import { AppCacheProvider } from "@mui/material-nextjs/v15-pagesRouter";
import { createTheme, ThemeProvider } from "@mui/material/styles";
import type { AppProps } from "next/app";

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

## Collect document styles

In **pages/_document.tsx**, include the integration's document helpers:

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

The helper extracts styles generated during the page render and places the corresponding style tags in the document head. Use its documented cache options when customizing Emotion; server and client cache configuration must agree.

The Pages Router setup differs from App Router setup. Do not combine both router providers in a project that uses only one router.

## Themes, caches, and request isolation

Keep the theme definition deterministic and available to both server and client rendering. The server and browser must agree about theme values that affect markup or generated CSS.

For a custom server-rendering implementation outside the framework integrations, create a fresh Emotion cache for each request, render the tree using that cache, extract the critical CSS, and include it in the response. Reusing one mutable cache across concurrent requests can leak styles between pages or users.

With Next.js, prefer the router integration helpers. If a custom cache is required, use the supported options for that helper and configure the client-side cache consistently.

## Streaming and CSS layers

App Router can stream rendered content. The cache provider coordinates generated Material UI styles with that output. Keep the provider around the route content whose styles must be collected.

When combining Material UI with CSS Modules, utility CSS, or plain stylesheets, cascade layers can make style precedence more predictable. The App Router cache provider accepts an enableCssLayer option:

~~~tsx
<AppRouterCacheProvider options={{ enableCssLayer: true }}>
  {children}
</AppRouterCacheProvider>
~~~

Enable CSS layers only when the application's style strategy is designed around them. Review actual cascade order, because layering changes which declarations override others.

## Fonts and theme typography

Next.js font optimization can load and self-host fonts at build time. Apply the generated font class to the document or an appropriate ancestor so Material UI typography can use it.

When using the default Roboto stack, leave the theme's default typography font family intact if you want its Roboto-specific letter spacing. For a different font, configure theme typography deliberately and verify every variant.

Fonts affect line breaks and layout. Check server-rendered output with production font settings rather than assuming development fallback fonts will match.

## Responsive rendering and media queries

CSS breakpoints are generally preferable to JavaScript media-query decisions for responsive layout. They allow the browser to adapt rendered styles without choosing different markup on the server and client.

Media-query hooks can be difficult to evaluate during SSR because the server does not know the browser viewport. If a hook needs an initial server value, configure its server-side behavior or render a stable fallback, then verify hydration does not produce a visual jump.

Do not read **window**, **document**, viewport size, or local storage during server rendering. Put browser-only work in a Client Component and run it after hydration when needed.

## Hydration consistency

Hydration expects the browser to attach behavior to compatible server-rendered output. Avoid rendering values that differ between server and client:

- Random identifiers generated independently during render.
- Current time or locale-sensitive text computed with different time zones.
- Browser-only state that changes markup before hydration.
- A theme or color mode that differs between server and client.
- A different HTML language or direction after the first paint.
- Data that changes between the server render and client initialization.

Use stable data and IDs, provide the active locale and theme to both sides, and defer browser-specific differences until after hydration where suitable. Investigate the first divergent element in a hydration warning instead of suppressing the warning without understanding it.

## Common mistakes

- Omitting the MUI Next.js cache provider and seeing styles injected in the wrong location.
- Importing an integration entry point for a different Next.js major.
- Using the client directive at the root and sending a large application tree to the browser.
- Expecting a Server Component to read React context directly.
- Accessing browser APIs during server rendering.
- Reusing one mutable Emotion cache for unrelated server requests.
- Using different theme or cache settings on the server and client.
- Rendering dates or random values differently during hydration.
- Combining both router integration setups in a project that uses one router.
- Adding manual style extraction on top of a helper that already collects styles.

## Chapter checklist

Before shipping, confirm that:

- The correct Next.js router integration is installed and configured.
- The integration export matches the project's Next.js major version.
- Material UI styles are collected into the document head.
- Theme and cache configuration are deterministic and compatible on server and client.
- Client boundaries are limited to interactive or browser-dependent components.
- The document language and other server-rendered attributes are correct on first paint.
- Responsive design does not cause server/client markup disagreement.
- Production build output has no hydration warnings or unstyled flash.

## Chapter navigation

<table style="width:100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="037-localization-rtl-and-internationalization.md">037 - Localization, RTL, and internationalization</a></td><td><a href="../qnas/038-server-side-rendering-and-nextjs.md">Q&amp;A</a></td><td><a href="../examples/038-server-side-rendering-and-nextjs.md">Example</a></td><td><a href="039-testing-material-ui-components.md">039 - Testing Material UI components</a></td></tr></tbody>
</table>
