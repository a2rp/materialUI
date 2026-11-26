# Tables and Data Display: Q&A

This Q&A accompanies [030 - Tables and Data Display](../lessons/030-tables-and-data-display.md). It covers native table semantics, sorting, pagination, responsive layouts, and common implementation decisions.

## Structure and accessibility

### 1. When should I use a Material UI Table?

Use a Table when the information is naturally a set of records with consistent columns and users need to compare values across rows. It suits reports, summaries, and straightforward record lists.

### 2. What does TableContainer do?

TableContainer wraps a Table and can provide horizontal scrolling when the table is wider than its available space. It does not define the table's row and column semantics; the Table and its child components do that.

### 3. Which HTML elements do the table components render?

Table renders a table, TableHead a thead, TableBody a tbody, TableRow a tr, and TableCell a th in a table head or a td in a table body by default. TableFooter can render a tfoot.

### 4. Do I need a caption if the table already has a heading?

A nearby heading gives visual context, while a caption describes the table within its structure. A caption is useful when it improves navigation and understanding for assistive technology. An accessible name can also be supplied with aria-label or aria-labelledby; choose a clear name without repeating unnecessary text.

### 5. What are scope="col" and scope="row" for?

They identify column and row headings. Use scope="col" on a column heading and scope="row" on a cell that names the record in its row. This can make cell relationships easier to understand.

### 6. Should a table row use hover styling?

Hover styling can help sighted users track a row. It does not make the row interactive or provide keyboard behavior. If a row performs an action, provide an actual link or button with a clear accessible name.

### 7. Why should I use stable keys for rows?

React uses keys to match rendered items when data changes. Use a stable record identifier so sorting, filtering, or deletion does not attach component state or focus to the wrong row. Avoid array indexes for changing lists.

## Sorting

### 8. Does TableSortLabel sort the rows automatically?

No. It provides a sort control and visual affordance. Your application must maintain sort state, compare the data, and render the rows in the resulting order.

### 9. How should I show which column is sorted?

Use TableSortLabel with its active and direction props, and set the TableCell sortDirection for the active column. This exposes the current direction in addition to showing the sort indicator.

### 10. What happens when a user clicks the current sort heading?

A common pattern is to reverse the direction when the active column is clicked again. When a different column is selected, choose a default direction and update the sort key. Keep this behavior consistent across columns.

### 11. Should I sort before or after pagination?

Sort the full filtered data set before slicing it into pages. Sorting only the current page can leave values in an unexpected order across page boundaries.

### 12. How should I compare mixed data types?

Use a comparator that matches each column's value type. Numeric values should be compared numerically; dates should be compared as dates or timestamps; strings can use locale-aware comparison. Normalize missing values intentionally rather than relying on incidental coercion.

### 13. Should every column be sortable?

No. Make only meaningful columns sortable. Do not style a heading like a sort control unless activating it changes the order.

## Pagination

### 14. Is TablePagination page numbering zero-based?

Yes. Its page prop is a zero-based index, so the first page is page 0. This differs from the standalone Pagination component, whose visible page numbers start at 1.

### 15. What should TablePagination count represent?

It should represent the total number of rows in the current result set, after applying any filters. For client-side pagination, this is the number of matching records available to slice.

### 16. Why reset the page when rowsPerPage changes?

The previous page may no longer be valid after the page size changes. Resetting to page 0 keeps the displayed slice predictable. Apply the same reset when a search or filter shortens the result list.

### 17. What if I paginate data on a server?

Keep the current page and page size in application state and request the corresponding records. Ensure the total count and loading state reflect the server response. Avoid presenting a page count that contradicts the rows available.

### 18. Where should TablePagination appear?

It can be placed within a TableFooter or rendered as a separate control after the Table. When placed outside the table structure, setting component="div" avoids treating the pagination control as a table cell.

## Layout and states

### 19. How should a wide table work on a phone?

A common option is horizontal scrolling in TableContainer with a sensible minWidth on Table. Other options include hiding low-priority columns or switching to cards. Preserve labels and context if you change the presentation.

### 20. What does stickyHeader need?

It works best when the table scrolls inside a container with a constrained height. Check stacking and overflow so the heading stays visible without covering content or escaping its intended container.

### 21. How should an empty table behave?

Explain whether there are no records, no filter matches, or an error. A clear message is better than an unexplained blank body. Keep column headings when they help users understand the empty result.

### 22. Should loading and error states go in a table cell?

They can be shown in a spanning cell or in a nearby status region, depending on the layout. Make loading and error messages distinguishable from an empty result, and expose important state changes accessibly.

### 23. How should I display a status color?

Pair color with readable text, an icon with an accessible name, or another meaningful cue. Do not make color the only way to tell whether a record is active, blocked, or complete.

## Choosing a component

### 24. When should I choose MUI X Data Grid instead?

Consider Data Grid for richer features such as extensive editing, column controls, complex filtering, or large collections that need virtualization. A standard Table is often simpler when those interactions are unnecessary.

### 25. Can I add arbitrary buttons to cells?

Yes, when the action belongs to that record. Give each control an accessible name that includes its purpose and, if needed, the row identity. Keep actions distinct, and ensure keyboard focus order remains understandable.

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
      <td><a href="029-chips-avatars-and-badges.md">029 - Chips, Avatars, and Badges</a></td>
      <td><a href="../lessons/030-tables-and-data-display.md">Lesson</a></td>
      <td><a href="../examples/030-tables-and-data-display.md">Example</a></td>
      <td><a href="031-mui-x-data-grid-community.md">031 - MUI X Data Grid Community</a></td>
    </tr>
  </tbody>
</table>
