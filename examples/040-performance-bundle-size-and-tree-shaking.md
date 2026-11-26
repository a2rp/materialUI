# Performance, Bundle Size, and Tree Shaking - Example

This example shows how to inspect a Next.js route, reduce avoidable development import work, defer a chart until a user requests it, and memoize a measured derived value.

The snippets illustrate patterns. The right optimization depends on the production bundle and the application's actual user journey.

## What this example demonstrates

- Direct component and icon paths for Material UI.
- A route-level bundle analysis workflow.
- A chart loaded only after the user opens its section.
- A focused useMemo example for derived data.
- Verification of user-visible loading and responsive behavior.

## Assumptions

The primary example uses Next.js App Router, Material UI with Emotion, and Recharts for a chart feature.

Install the packages used by the example:

~~~bash
npm install @mui/material @mui/icons-material @emotion/react @emotion/styled
npm install recharts
~~~

Use the bundle analyzer provided by the Next.js version and bundler in the project.

## 1. Prefer direct MUI and icon imports

Use component paths in application code:

~~~jsx
import Button from "@mui/material/Button";
import Card from "@mui/material/Card";
import CardContent from "@mui/material/CardContent";
import Typography from "@mui/material/Typography";
import DeleteIcon from "@mui/icons-material/Delete";
~~~

This avoids broad barrel processing in development and makes dependencies visible. Modern production bundlers can tree-shake unused top-level exports, so verify actual production output instead of assuming a named barrel import always increases the shipped bundle.

## 2. Analyze before changing code

For Next.js versions that include the Turbopack analyzer, run it against the application:

~~~bash
npx next experimental-analyze
~~~

Filter the report to the route of interest. Inspect client and server bundles separately, then trace a large module back through its import chain. The tool is version-dependent; use the analyzer command supported by the installed Next.js version.

For a Webpack project, the official analyzer plugin can create bundle treemaps:

~~~js
// next.config.js
const withBundleAnalyzer = require("@next/bundle-analyzer")({
  enabled: process.env.ANALYZE === "true",
});

module.exports = withBundleAnalyzer({});
~~~

Generate the report with the configured production build command:

~~~bash
ANALYZE=true npm run build
~~~

Save a before report before making a targeted change. Compare the route, client/server view, transfer size, and import chain after the change.

## 3. Keep the route and initial content small

Create **app/reports/page.tsx** as a Server Component. Keep static summary content and server-capable data work outside the interactive chart boundary:

~~~tsx
import Card from "@mui/material/Card";
import CardContent from "@mui/material/CardContent";
import Grid from "@mui/material/Grid";
import Typography from "@mui/material/Typography";
import ReportPanel from "./report-panel";

export default function ReportsPage() {
  const summary = {
    revenue: "$48,200",
    openInvoices: "17",
    title: "Quarterly report",
  };

  return (
    <main>
      <Typography component="h1" variant="h4" sx={{ mb: 3 }}>
        {summary.title}
      </Typography>

      <Grid container spacing={2}>
        <Grid size={{ xs: 12, sm: 6 }}>
          <Card>
            <CardContent>
              <Typography color="text.secondary">Revenue</Typography>
              <Typography variant="h5">{summary.revenue}</Typography>
            </CardContent>
          </Card>
        </Grid>
        <Grid size={{ xs: 12, sm: 6 }}>
          <Card>
            <CardContent>
              <Typography color="text.secondary">Open invoices</Typography>
              <Typography variant="h5">{summary.openInvoices}</Typography>
            </CardContent>
          </Card>
        </Grid>
      </Grid>

      <ReportPanel />
    </main>
  );
}
~~~

For a real dashboard, load protected data in a server module and render the summary on the server. Keep browser state and interaction in client components.

## 4. Defer the chart until it is requested

Create **app/reports/report-panel.tsx**:

~~~tsx
"use client";

import * as React from "react";
import dynamic from "next/dynamic";
import Button from "@mui/material/Button";
import Stack from "@mui/material/Stack";
import Typography from "@mui/material/Typography";

const RevenueChart = dynamic(() => import("./revenue-chart"), {
  loading: () => (
    <Typography role="status">Loading chart...</Typography>
  ),
});

export default function ReportPanel() {
  const [showChart, setShowChart] = React.useState(false);

  return (
    <Stack spacing={2} sx={{ mt: 4 }}>
      <Button
        variant="contained"
        onClick={() => setShowChart(true)}
      >
        Load revenue chart
      </Button>

      {showChart ? (
        <RevenueChart />
      ) : (
        <Typography color="text.secondary">
          The chart loads when you request it.
        </Typography>
      )}
    </Stack>
  );
}
~~~

The dynamic import is declared at module scope, and the component is rendered only after interaction. The default behavior can still prerender a Client Component; this chart does not need server rendering to be disabled. Use a client-only option only when the component truly depends on browser APIs during rendering.

Create **app/reports/revenue-chart.tsx**:

~~~tsx
"use client";

import {
  CartesianGrid,
  Line,
  LineChart,
  ResponsiveContainer,
  Tooltip,
  XAxis,
  YAxis,
} from "recharts";
import Box from "@mui/material/Box";
import Typography from "@mui/material/Typography";

const data = [
  { month: "Jan", revenue: 12000 },
  { month: "Feb", revenue: 14800 },
  { month: "Mar", revenue: 16100 },
  { month: "Apr", revenue: 17300 },
];

export default function RevenueChart() {
  return (
    <Box component="section" aria-label="Revenue chart">
      <Typography component="h2" variant="h6" sx={{ mb: 2 }}>
        Monthly revenue
      </Typography>
      <Box sx={{ width: "100%", height: 280 }}>
        <ResponsiveContainer>
          <LineChart data={data} margin={{ top: 8, right: 16, bottom: 8, left: 0 }}>
            <CartesianGrid strokeDasharray="3 3" />
            <XAxis dataKey="month" />
            <YAxis />
            <Tooltip />
            <Line
              type="monotone"
              dataKey="revenue"
              stroke="#3157b7"
              strokeWidth={2}
            />
          </LineChart>
        </ResponsiveContainer>
      </Box>
    </Box>
  );
}
~~~

This split is useful only if the chart or its dependency is large enough to matter and users do not need it immediately. The loading state should be understandable and should not cause a disruptive layout shift.

## 5. Memoize derived rows only when measurement supports it

A filter over a small list is usually fast enough to compute directly. For a large data set where profiling shows the filter is repeated unnecessarily, memoize the derived result:

~~~tsx
"use client";

import * as React from "react";
import TextField from "@mui/material/TextField";
import Table from "@mui/material/Table";
import TableBody from "@mui/material/TableBody";
import TableCell from "@mui/material/TableCell";
import TableHead from "@mui/material/TableHead";
import TableRow from "@mui/material/TableRow";

type Order = {
  id: string;
  customer: string;
  total: number;
};

function filterOrders(orders: Order[], query: string) {
  const normalizedQuery = query.trim().toLowerCase();

  if (!normalizedQuery) {
    return orders;
  }

  return orders.filter((order) =>
    order.customer.toLowerCase().includes(normalizedQuery),
  );
}

export function OrdersTable({ orders }: { orders: Order[] }) {
  const [query, setQuery] = React.useState("");

  const visibleOrders = React.useMemo(
    () => filterOrders(orders, query),
    [orders, query],
  );

  return (
    <>
      <TextField
        label="Filter by customer"
        value={query}
        onChange={(event) => setQuery(event.target.value)}
      />

      <Table aria-label="Orders">
        <TableHead>
          <TableRow>
            <TableCell>Customer</TableCell>
            <TableCell align="right">Total</TableCell>
          </TableRow>
        </TableHead>
        <TableBody>
          {visibleOrders.map((order) => (
            <TableRow key={order.id}>
              <TableCell>{order.customer}</TableCell>
              <TableCell align="right">{order.total}</TableCell>
            </TableRow>
          ))}
        </TableBody>
      </Table>
    </>
  );
}
~~~

The data and query are the only dependencies used in the calculation. If the list is small or profiling shows no meaningful cost, remove the memoization and keep the simpler calculation.

For very large data sets, consider pagination, virtualization, or server-side filtering. Memoizing a client-side filter does not reduce the number of records downloaded or rendered.

## 6. Compare the result

After each change, compare the same route and interaction:

- Did the initial client bundle shrink?
- Did the chart move to a separate chunk?
- Is the chart absent from the initial route load?
- Does opening the chart add an acceptable wait?
- Did development startup or rebuild time change?
- Did an unrelated route become larger?
- Did the page become harder to use on a small screen?

Keep a change only when it improves the measured target without creating a worse loading or interaction experience.

## Chapter navigation

<table width="100%">
<thead><tr><th>Previous example</th><th>Chapter lesson</th><th>Chapter Q&amp;A</th><th>Next example</th></tr></thead>
<tbody><tr><td><a href="039-testing-material-ui-components.md">039 - Testing Material UI components</a></td><td><a href="../lessons/040-performance-bundle-size-and-tree-shaking.md">Lesson</a></td><td><a href="../qnas/040-performance-bundle-size-and-tree-shaking.md">Q&amp;A</a></td><td><a href="041-form-library-integration.md">041 - Form library integration</a></td></tr></tbody>
</table>
