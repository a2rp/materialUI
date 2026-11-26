# Performance, Bundle Size, and Tree Shaking

A responsive Material UI application depends on more than a small JavaScript bundle. The browser must download, parse, and execute code, render components, load fonts and images, and respond to user input.

Start with a measurement, find the work or dependency that matters, and make one targeted change. Import style, code splitting, rendering work, and data volume each affect different parts of the experience.

## Learning goals

After this chapter, you should be able to:

- Distinguish development import cost from production bundle size.
- Choose direct Material UI and icon imports.
- Understand what tree shaking can and cannot remove.
- Inspect route bundles and trace large dependencies.
- Split rarely used or expensive client features.
- Use React memoization only for measured rendering costs.
- Keep large lists and data sets from creating unnecessary work.

## Understand the performance budget

Performance has several related costs:

- **Download:** bytes transferred over the network.
- **Parse and compile:** work needed before JavaScript can run.
- **Execution:** time spent running application and library code.
- **Rendering:** React and browser layout, paint, and compositing work.
- **Development feedback:** startup, module transformation, and rebuild time.

An optimization that improves one cost may not improve another. A direct import can make development rebuilds faster even if the production bundler already removes unused exports. A smaller transfer can still leave an expensive render. Measure the relevant user journey before choosing a fix.

## Prefer direct Material UI imports

Use path-based imports for Material UI components:

~~~jsx
import Button from "@mui/material/Button";
import TextField from "@mui/material/TextField";
import Dialog from "@mui/material/Dialog";
~~~

Avoid importing components from the large top-level barrel in performance-sensitive development workflows:

~~~jsx
import { Button, Dialog, TextField } from "@mui/material";
~~~

Modern production bundlers can tree-shake unused exports from these top-level imports. The main documented concern with barrel imports is slower development startup and rebuilds, because tooling may need to process a broad package entry point. Direct paths avoid loading unrelated package modules during development and require no special configuration.

Use direct component paths consistently so editor auto-imports and team conventions do not reintroduce the slower pattern.

## Import icons by path

Icons are particularly numerous. Import only the icon used, by its component path:

~~~jsx
import DeleteIcon from "@mui/icons-material/Delete";
import SearchIcon from "@mui/icons-material/Search";
~~~

Avoid named imports from the icons barrel in development:

~~~jsx
import { Delete as DeleteIcon, Search as SearchIcon } from "@mui/icons-material";
~~~

Path imports can reduce module work during development. They also make the dependency on each icon explicit. Do not import an entire icon library or load icon names dynamically when only a few fixed icons are used.

## Understand tree shaking

Tree shaking is a production bundler's ability to remove unused exports and modules when the package format and build configuration allow it. It is not a guarantee that every dependency becomes small.

A dependency can remain large when:

- The application imports a broad module with side effects.
- The package's export structure prevents useful dead-code elimination.
- A function or module is referenced indirectly at runtime.
- A large feature is imported into an initial client entry point.
- A package includes data or styles required by the retained modules.

Inspect the production output rather than reasoning only from source import syntax. A named import is not automatically wasteful in every production bundler, and a path import is not a substitute for analyzing the emitted bundle.

## Analyze before optimizing

Use the bundle analyzer supported by the project's Next.js version and bundler. A report should answer:

- Which route or entry point includes the module?
- Is the module in the client or server bundle?
- Which import chain brings it into the bundle?
- Is the code required on the initial screen?
- Does the module have a smaller alternative or a later loading point?

For Next.js with Webpack, the official bundle analyzer plugin can produce treemaps by bundle. Newer Next.js versions also provide a Turbopack analyzer. The command and availability depend on the installed Next.js version, so use the matching CLI guidance.

A report is most useful when compared before and after a specific change. Track transferred and compressed sizes as well as uncompressed JavaScript. A large package in a treemap may not be part of the initial client bundle.

## Use package import optimization selectively

Next.js can optimize some packages with many exports. The Next.js configuration option for package import optimization is useful when:

- A package exposes a large barrel.
- The analyzer shows that importing a small set of names pulls in too many modules.
- The installed bundler does not already optimize those imports automatically.

Do not add every dependency to this configuration. Some packages are handled automatically, and some bundlers already resolve individual modules. Verify the module graph before adding configuration.

## Split code that users do not need immediately

A heavy feature that is used only after an interaction can often be loaded later. Examples include a rich text editor opened on demand, an advanced chart on a secondary tab, or a large code viewer below the fold.

In a client-rendered React feature, lazy loading can defer a component until it is rendered:

~~~jsx
import * as React from "react";

const ReportEditor = React.lazy(() => import("./ReportEditor"));

export function ReportPanel({ editing }) {
  if (!editing) {
    return <p>Choose Edit to open the report editor.</p>;
  }

  return (
    <React.Suspense fallback={<p>Loading editor...</p>}>
      <ReportEditor />
    </React.Suspense>
  );
}
~~~

In Next.js, use its supported dynamic import patterns for the router and component type in the project. A client-only rendering option is appropriate only when a component truly requires browser APIs; disabling server rendering without need can delay useful HTML.

Code splitting has tradeoffs: extra requests, loading states, and possible interaction delay. Split at meaningful feature or route boundaries, not every small component.

## Keep server-only work out of the client bundle

In the App Router, work that does not require browser APIs or user interaction can often stay in a Server Component. This includes protected data access and transformations that produce static markup.

Moving server-capable work off the client can reduce browser JavaScript and avoid exposing server dependencies. Keep secrets and database modules out of client module graphs. Pass only serializable data or rendered server output to client controls.

## Measure rendering before memoizing

React component rendering is often inexpensive. Use browser performance tools and React profiling to identify a slow interaction before adding memoization.

Use a memoized calculation for work that is measurably expensive and whose inputs usually remain unchanged:

~~~jsx
import * as React from "react";

function ResultsList({ records, query }) {
  const visibleRecords = React.useMemo(
    () => filterRecords(records, query),
    [records, query],
  );

  return <ResultsTable rows={visibleRecords} />;
}
~~~

The calculation must be pure, and the dependency list must include every reactive value it uses. Memoization caches work between renders; it does not make the initial calculation faster.

Memoization is a performance optimization, not a correctness guarantee. First fix unnecessary state updates, repeated data fetching, or overly broad component boundaries. Add memo only if profiling shows a useful improvement.

A new object or function is a new reference on each render. Passing such values to a memoized child can defeat the optimization. Keep the child props small and stable when that child is expensive enough to justify memoization.

React Compiler can automatically memoize in configured projects. Follow the project's compiler setup and do not layer manual memoization everywhere without evidence.

## Keep large data views efficient

Material UI tables and data grids can render many DOM nodes when given large data sets. Pagination, virtualization, filtering at the data source, and server-side queries can reduce work.

Choose the approach based on actual row counts and interactions:

- Paginate modest lists when users benefit from clear pages.
- Use virtualization for long scrollable collections.
- Move filtering or sorting to the server when data size or authorization requires it.
- Keep row components focused and avoid recreating expensive derived values unnecessarily.
- Do not fetch or render data that the current route does not need.

Virtualization changes the DOM: only a subset of rows may exist at a time. Design accessibility, keyboard navigation, and tests for the chosen data-view behavior.

## Fonts, images, and styles

Bundle size includes more than JavaScript. Large fonts, duplicate font weights, images, and global CSS can affect loading and rendering.

- Load only the font weights and subsets that the interface needs.
- Use framework font optimization where it fits the application.
- Serve appropriately sized images and use responsive image support.
- Avoid importing large CSS files from rarely used features into the initial route.
- Prefer Material UI's styling and theme tools without repeatedly recreating global styles.
- Check layout shift when fonts and images load.

## Common mistakes

- Assuming named imports always make a production bundle large.
- Using barrel imports despite slower development module processing.
- Importing icons from a full icon barrel or runtime-selected set.
- Adding package optimization configuration without checking bundler behavior.
- Lazy-loading small components whose loading delay is more costly than their code.
- Disabling server rendering for components that do not need browser APIs.
- Adding memoization before measuring a slow interaction.
- Believing a smaller source file guarantees a smaller or faster page.
- Optimizing a shared module without checking which routes actually load it.

## Chapter checklist

Before shipping a performance change, confirm that:

- The target user journey and performance metric are identified.
- A bundle or profiler report points to the suspected work.
- Direct imports are used for Material UI and icons.
- Production output is checked for actual tree-shaking results.
- Heavy features load only when needed when that improves the experience.
- Server-capable work stays off the client when appropriate.
- Memoization is supported by measurements and correct dependencies.
- Large data views, fonts, images, and layout shifts are reviewed.
- The before-and-after result is measured in a production-like build.

## Recap

- Measure the production bundle and rendering behavior before optimizing.
- Prefer direct component and icon imports, and verify the result with the project's bundler analysis.
- Defer large or rarely used routes and dependencies when that improves real user experience.
- Avoid speculative memoization and keep large data views efficient based on measured needs.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous lesson</th><th>Chapter Q&amp;A</th><th>Chapter example</th><th>Next lesson</th></tr></thead>
<tbody><tr><td><a href="039-testing-material-ui-components.md">039 - Testing Material UI components</a></td><td><a href="../qnas/040-performance-bundle-size-and-tree-shaking.md">Q&amp;A</a></td><td><a href="../examples/040-performance-bundle-size-and-tree-shaking.md">Example</a></td><td><a href="041-form-library-integration.md">041 - Form library integration</a></td></tr></tbody>
</table>
