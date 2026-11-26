# MUI X Data Grid Community: Q&A

This Q&A accompanies [031 - MUI X Data Grid Community](../lessons/031-mui-x-data-grid-community.md). It explains setup, row and column definitions, built-in behavior, plan boundaries, and common troubleshooting.

## Setup and structure

### 1. Which package provides the Community Data Grid?

Install the @mui/x-data-grid package and import DataGrid from it. The Pro and Premium products use separate packages and licenses.

### 2. Does Data Grid work without Material UI?

It has a peer dependency on @mui/material. A project that does not already use Material UI also needs the Material UI styling dependencies required by its selected styling engine.

### 3. What are rows and columns?

Rows are an array of record objects. Columns are definitions that describe which fields to show, their headings, sizing, and other behavior. The field property connects a column to a value in a row.

### 4. Does every row need an id?

Yes, each row needs a unique stable identifier. Data Grid uses it to track rows during sorting, filtering, selection, and updates. The default property name is id.

### 5. What if my identifier property has another name?

Pass a getRowId function that returns that property's value. Keep the function reference stable between renders, and make sure the returned value does not change for an existing record.

### 6. Why should columns be defined outside the React component?

Stable column references help Data Grid avoid repeating work and preserve column state. Define static columns outside the component or memoize them when they depend on component state.

### 7. Do I need stable rows too?

Yes. Keep the rows array reference stable when its contents have not changed. When records change, provide a new array with the updates instead of mutating the existing array in place.

### 8. Why does the grid show no visible rows?

Check that the parent has a height. A percentage height needs an ancestor with a defined height. A fixed height, viewport-based height, or properly constrained flex layout gives the grid room to render.

## Built-in behavior

### 9. Are sorting and filtering implemented automatically?

Sorting and filtering are enabled by default for supported columns. Users can use the column headers and grid controls. Your row data and column types still need to represent values correctly.

### 10. How many columns can Community sort at once?

Community sorts according to one criterion at a time. Multi-column sorting is a Pro feature. If users need a compound ordering, implement a clearly defined server or application sort, or use a plan that supports the required interaction.

### 11. How many filter conditions can Community apply at once?

Community supports a single filter criterion at a time. Multiple simultaneous filters and advanced filtering require a higher plan. Confirm that boundary before building a filter workflow around several conditions.

### 12. Can I turn pagination off in Community?

No. Pagination is always enabled in the Community grid. Plan for its footer and page model as part of the grid layout.

### 13. What is the maximum page size?

Community pages can contain up to 100 rows. The default page size is 100. Configure pageSizeOptions to give users suitable smaller choices, and do not include an option above the Community limit.

### 14. Can the grid sort or filter rows the server has not sent?

No. Client-side operations can only use rows currently supplied to the grid. If the server provides one page at a time, client sorting and filtering will only process that page.

### 15. What is needed for server-side pagination?

Set paginationMode to server, track the page model, request the corresponding page, and provide the total row count. If sorting and filtering must cover the whole dataset, implement those modes on the server too.

## Selection and keyboard use

### 16. How does row selection work in Community?

Single-row selection is available by default. Community also supports checkbox selection, including a header checkbox for select-all workflows. Pro adds modifier-based selection of multiple independent rows and selection ranges.

### 17. Should I add a checkbox column?

Add checkboxSelection when users need an explicit selection affordance or a select-all action. Keep the resulting bulk actions visible and label them clearly. Test selection behavior when filters or pages change.

### 18. How does keyboard navigation work?

The grid uses an interactive grid keyboard model. Users navigate between cells, and focus is managed within the grid rather than requiring every cell to be a separate tab stop. Test custom cell content with keyboard and assistive technology.

### 19. Can a cell contain a button or link?

Yes, but the control needs a meaningful accessible name and a predictable focus order. If clicking a cell control should not select its row, configure row selection behavior accordingly. Avoid adding interactive content that competes with the grid's own keyboard controls.

## Sizing and customization

### 20. What is the difference between width and flex on a column?

width gives a column a fixed width. flex lets it share remaining space proportionally. Use minWidth when a flexible column must remain readable. A fixed-height grid with narrow width may require horizontal scrolling.

### 21. Should I define columns inside the component?

Avoid recreating static column objects on each render. If a column depends on changing state, memoize its definition and include the relevant dependencies. Preserve stable references for callbacks and row ID functions too.

### 22. Can I use Data Grid for a simple report?

You can, but a standard Material UI Table may be more appropriate if you only need semantic rows and columns with little custom interaction. Choose Data Grid when its built-in sorting, filtering, selection, or pagination materially reduces implementation work.

## Troubleshooting and plan boundaries

### 23. Why do filtered results look incomplete with server pagination?

The grid cannot filter rows that were never supplied. Use server-side filtering together with server-side pagination when filtering must cover the complete server dataset. The same principle applies to sorting.

### 24. When should I consider Pro or Premium?

Consider a paid plan when the application needs a feature reserved for it, such as multi-column sorting, multiple filter criteria, column pinning or reordering, or other advanced interactions. Verify the current plan comparison before committing to that capability.

### 25. What should I check when the grid slows down after each render?

Check whether rows, columns, or getRowId are being recreated unnecessarily. Stable references help avoid repeating sorting, filtering, and column work. For very large data and advanced performance needs, evaluate the suitable MUI X plan and server-side data strategy.

## Chapter navigation

<table width="100%">
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
      <td><a href="030-tables-and-data-display.md">030 - Tables and Data Display</a></td>
      <td><a href="../lessons/031-mui-x-data-grid-community.md">Lesson</a></td>
      <td><a href="../examples/031-mui-x-data-grid-community.md">Example</a></td>
      <td><a href="032-icons-and-icon-buttons.md">032 - Icons and Icon Buttons</a></td>
    </tr>
  </tbody>
</table>
