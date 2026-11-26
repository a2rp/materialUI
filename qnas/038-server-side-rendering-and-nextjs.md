# Server-Side Rendering and Next.js - Q&A

This Q&A reviews the concepts and implementation choices from Chapter 038.

## Rendering fundamentals

### 1. What does server-side rendering provide on the initial request?

The server can send rendered HTML before the browser has downloaded and run all client JavaScript. The browser can display that markup early, then hydrate interactive client components.

### 2. What is hydration?

Hydration attaches event handlers and client behavior to compatible server-rendered markup. It depends on the client rendering output that agrees with the HTML already in the page.

### 3. Why must Material UI CSS be collected during server rendering?

Without the generated styles, the browser can briefly show unstyled markup. Correct style collection also helps prevent the server and client from producing conflicting or duplicated styles.

### 4. What does the MUI Next.js cache provider do?

It coordinates collection and placement of MUI System styles during Next.js rendering. The App Router helper works with streaming output; the Pages Router helper integrates with its app and document lifecycle.

### 5. Does the cache provider replace ThemeProvider?

No. The cache provider handles style collection and Emotion cache coordination. ThemeProvider makes the application theme available to components.

## App Router

### 6. Are App Router layouts and pages Client Components by default?

No. They are Server Components by default. Use a Client Component for state, event handlers, effects, browser APIs, and context.

### 7. Where should the App Router cache provider be added?

Wrap the body content in the root layout with the App Router cache provider from the Next.js integration package. This lets the helper collect styles while Next.js streams the page.

### 8. Why should a provider boundary be kept focused?

A broad client boundary can include many imported modules in the client bundle and reduce server rendering opportunities. Keep interactive providers and controls as deep as practical.

### 9. Does wrapping server-rendered children with a client provider necessarily make those children client components?

No. A Server Component can render output and pass it through a Client Component's children slot. The server-rendered output remains a server-produced child.

### 10. What should a Server Component handle?

It can fetch server-side data, access secrets that must stay off the browser, and render static structure. Keep browser-only interactions in client components.

### 11. What belongs in a Client Component?

State, event handlers, lifecycle effects, browser APIs, and context usage belong in a Client Component.

### 12. What does the client directive do?

It creates a boundary between server and client module graphs. Modules imported by that Client Component become part of its client bundle, so place the boundary carefully.

## Pages Router

### 13. What are the integration points for the Pages Router?

Use the App component for application-wide providers and the custom Document for document-level style collection and HTML structure.

### 14. What does the Pages Router app cache provider do?

It integrates style collection with Next.js page rendering and helps place MUI styles in the document head.

### 15. What do the DocumentHeadTags and documentGetInitialProps helpers do?

They collect the styles generated during server rendering and add the required style tags to the document head.

### 16. Should an application configure both router integrations?

No. Configure the integration for the router that the app uses. App Router and Pages Router have different lifecycle hooks and setup components.

### 17. Why does the integration import path include a Next.js version?

The integration package has router entry points compatible with different Next.js major versions. Choose the entry point matching the installed Next.js version, following the current package documentation.

## Caches and styles

### 18. Why should a custom Emotion cache be created per server request?

A fresh cache isolates style collection for each request. Sharing one mutable cache across concurrent requests can mix styles between rendered pages or users.

### 19. Should a custom server extraction flow be added when using the Next.js helper?

Usually no. The integration helper already coordinates style collection for that router. Adding a second extraction system can duplicate work or make style ordering incorrect.

### 20. Why must server and client theme settings agree?

Theme values affect generated classes, styles, and sometimes markup. A mismatch can cause hydration warnings, visual changes, or style flicker.

### 21. What is the purpose of enabling CSS layers?

CSS layers provide an explicit style precedence model when combining Material UI with other CSS systems. Enable them only when the project's cascade strategy accounts for the layer ordering.

### 22. Does the Emotion cache provider itself create a theme?

No. It handles cache and style behavior. A separate theme provider supplies a theme created with Material UI's theme API.

## Hydration and responsive behavior

### 23. What can cause a hydration mismatch?

Different server and client output can come from random values, time zone differences, locale-sensitive formatting, browser-only state, mismatched theme configuration, or data that changes between renders.

### 24. Why should browser APIs not be read during server rendering?

The server does not have the browser's window, document, viewport, or local storage. Put browser-only logic in a Client Component and run it at an appropriate time.

### 25. Why can media-query hooks be difficult with server rendering?

The server does not know the client's viewport. A server guess may differ from the browser result and change the rendered output after hydration. CSS breakpoints are often simpler for responsive layout.

### 26. How should hydration warnings be handled?

Find the first differing element and identify why server and client output diverged. Fix the rendering difference rather than suppressing a warning without understanding it.

### 27. How should the initial document language and direction be set?

Render the correct HTML language and direction attributes on the server for the initial response. Changing them only after the client loads can cause a visible shift or inconsistent assistive-technology behavior.

## Production decisions

### 28. What should be considered when using a custom font?

Load the font through the framework's font support or another deliberate strategy, apply its class to the right ancestor, and configure the theme when needed. Verify typography and line wrapping with the actual production font.

### 29. What should be checked for an SSR Material UI page before shipping?

Confirm that styles arrive with the HTML, the integration matches the router and Next.js version, the server and client share compatible theme settings, and the production build has no hydration warnings or unstyled flash.

### 30. Does server-side rendering mean the entire application must run on the client after the first load?

No. Next.js can keep server-rendered regions on the server while hydrating interactive client components. Keeping the client boundary small can reduce browser JavaScript.

## Chapter navigation

<table style="width:100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="037-localization-rtl-and-internationalization.md">037 - Localization, RTL, and internationalization</a></td><td><a href="../lessons/038-server-side-rendering-and-nextjs.md">Lesson</a></td><td><a href="../examples/038-server-side-rendering-and-nextjs.md">Example</a></td><td><a href="039-testing-material-ui-components.md">039 - Testing Material UI components</a></td></tr></tbody>
</table>
