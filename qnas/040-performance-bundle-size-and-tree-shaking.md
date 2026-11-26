# Performance, Bundle Size, and Tree Shaking - Q&A

This Q&A reviews the performance and bundle-size concepts from Chapter 040.

## Measuring performance

### 1. What different costs contribute to perceived performance?

Download size, JavaScript parsing and execution, React rendering, browser layout and paint, and development startup or rebuild time can all affect performance.

### 2. Why should a performance change start with a measurement?

A measured profile identifies the slow route, interaction, module, or rendering task. Without it, an optimization can target the wrong cost or add complexity without improving the user experience.

### 3. Does a large module in a bundle report always affect the initial page load?

No. It may belong to a server bundle, a secondary route, or a dynamically loaded feature. Inspect the route, client or server environment, and import chain.

### 4. Why compare bundles before and after a change?

A before-and-after comparison shows whether the specific change reduced the relevant output. It also helps catch unexpected dependencies or changes in other routes.

## Imports and tree shaking

### 5. What import style does Material UI recommend for development performance?

Use direct component paths, such as importing Button from its component path. This can reduce development startup and rebuild work compared with processing a large package barrel.

### 6. Does a named import from the Material UI barrel always make the production bundle larger?

No. Modern production bundlers can tree-shake unused exports from top-level imports. The current documented concern with barrels is especially development module processing. Check the emitted production bundle before concluding.

### 7. Why are direct icon imports useful?

The icon package has many exports. Importing an icon by its component path avoids processing an unnecessarily broad barrel during development and makes the required icon explicit.

### 8. What is tree shaking?

It is a production bundler's removal of unused exports and modules when package formats and build configuration allow it.

### 9. Does tree shaking guarantee that every dependency becomes small?

No. Side effects, runtime references, package structure, dynamic imports, and large retained features can prevent unused code from being removed.

### 10. Does a direct path import replace bundle analysis?

No. It is a useful import convention, but the actual output still depends on the build and dependency graph. Analyze the production bundle to verify the result.

### 11. When should Next.js package import optimization be configured?

When a large-export package causes unnecessary module loading and the installed bundler does not already optimize it. First inspect the module graph and the framework's automatic handling.

### 12. Why should package import optimization not be enabled indiscriminately?

Some packages are already optimized automatically, and unnecessary configuration adds maintenance without improving the output.

## Code splitting and server work

### 13. What is code splitting?

It separates application code into chunks so a route or feature can load only the code it needs, possibly delaying a feature until the user requests it.

### 14. Which features might be good candidates for deferred loading?

A rarely opened rich text editor, an advanced visualization on a secondary panel, or a large code viewer may be good candidates if profiling shows that deferring them helps the initial route.

### 15. Why not lazy-load every component?

Splitting creates more loading boundaries and can delay interactions. Small components may cost less to load immediately than the complexity and waiting state required to split them.

### 16. When is disabling server rendering appropriate for a dynamic feature?

Only when the feature genuinely requires browser-only APIs or has another clear client-only constraint. Otherwise, disabling server rendering can delay useful HTML.

### 17. How can Server Components reduce client JavaScript?

Transformations that do not need browser APIs or user interaction can run on the server and return rendered output. This can keep heavy libraries out of the client bundle.

### 18. What must stay out of a client module graph?

Secrets, server-only database dependencies, and code that should never be exposed to the browser. Keep such work in server modules and pass only the required output to client components.

## React rendering performance

### 19. When should useMemo be introduced?

When profiling shows an expensive calculation is repeated with unchanged dependencies, and skipping the recalculation improves a relevant interaction.

### 20. Does useMemo make the initial calculation faster?

No. It can reuse a previous result during later renders when dependencies have not changed. The initial calculation still runs.

### 21. Is memoization a correctness guarantee?

No. It is a performance optimization. Application correctness must not depend on React retaining a memoized value.

### 22. Why can memoized child components still re-render?

Their props may change by reference. A newly created object, array, or function is a different value for shallow comparison even if its contents look the same.

### 23. Should memo, useMemo, and useCallback be added everywhere?

No. They can make code harder to read and maintain without a perceptible gain. Profile first and optimize components whose repeated work matters.

### 24. Can state placement affect rendering work?

Yes. State placed high in the component tree can cause a broad subtree to re-render. Keep state close to the components that use it when that produces clearer and narrower updates.

## Lists, assets, and validation

### 25. What techniques can help a large data view?

Pagination, virtualization, server-side filtering or sorting, and avoiding unneeded data fetches can reduce rendered work. Choose based on actual data volume and user interactions.

### 26. What should be considered when using virtualization?

Only a subset of items may exist in the DOM at one time. Keyboard navigation, accessibility, focus, and automated tests must account for that behavior.

### 27. What non-JavaScript assets affect performance?

Fonts, images, and global styles can affect transfer size, render time, and layout stability. Load only needed font weights and image sizes, and watch for layout shifts.

### 28. Why measure with a production-like build?

Development includes extra tooling and behavior that can distort performance. A production build more closely represents the code and optimizations users receive.

### 29. What is a good stopping point for optimization?

Stop when the measured user-facing result meets its goal and further changes do not provide a meaningful gain. Keep the simplest implementation that satisfies the performance requirement.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous Q&amp;A</th><th>Chapter lesson</th><th>Chapter example</th><th>Next Q&amp;A</th></tr></thead>
<tbody><tr><td><a href="039-testing-material-ui-components.md">039 - Testing Material UI components</a></td><td><a href="../lessons/040-performance-bundle-size-and-tree-shaking.md">Lesson</a></td><td><a href="../examples/040-performance-bundle-size-and-tree-shaking.md">Example</a></td><td><a href="041-form-library-integration.md">041 - Form library integration</a></td></tr></tbody>
</table>
