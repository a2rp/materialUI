# TypeScript with Material UI

Material UI components and themes include TypeScript declarations. Good types make component APIs easier to discover and help catch invalid props, theme keys, and event handlers before runtime.

This chapter covers typed component props, the sx prop, polymorphic roots, styled components, and module augmentation for custom palette colors and variants.

## Learning goals

After this chapter, you should be able to:

- Reuse Material UI prop types when creating application components.
- Type children, callbacks, and forwarded refs.
- Accept and merge a typed sx prop.
- Use the component prop with appropriate element attributes.
- Extend theme and component types through module augmentation.
- Keep custom types in a file TypeScript includes in its program.

## Project setup

A Vite React TypeScript template includes the TypeScript compiler and React type packages. For an existing project, install TypeScript and the React declarations when they are not already present:

```bash
npm install -D typescript @types/react @types/react-dom
```

A .tsx file is required for components that contain JSX. Use .ts for files that only contain TypeScript declarations or non-JSX code. Keep strict type checking enabled where practical.

## Reuse Material UI component prop types

When wrapping a Material UI component, start from its public prop type rather than copying props by hand:

```tsx
import Button from "@mui/material/Button";
import type { ButtonProps } from "@mui/material/Button";

type ActionButtonProps = Omit<ButtonProps, "children"> & {
  label: string;
};

export function ActionButton({ label, ...buttonProps }: ActionButtonProps) {
  return <Button {...buttonProps}>{label}</Button>;
}
```

The wrapper accepts the Button props while requiring a label. Omit a prop when the wrapper owns it or needs to give it a stricter meaning. Avoid widening values to any; that removes useful checking and autocomplete.

For event handlers, allow TypeScript to infer the event type from the component where possible. When a callback is part of your own public API, give it an explicit type:

```tsx
type SearchFieldProps = {
  value: string;
  onValueChange: (nextValue: string) => void;
};

function SearchField({ value, onValueChange }: SearchFieldProps) {
  return (
    <TextField
      label="Search"
      value={value}
      onChange={(event) => onValueChange(event.target.value)}
    />
  );
}
```

The consumer deals with a string value rather than a Material UI event object. This keeps the application component API focused on its purpose.

## Type children and refs

Use ReactNode for renderable children. Use the element type that the wrapped component actually renders when you forward a ref:

```tsx
import { forwardRef, type ReactNode } from "react";
import Button from "@mui/material/Button";
import type { ButtonProps } from "@mui/material/Button";

type LabeledButtonProps = Omit<ButtonProps, "children" | "component"> & {
  children: ReactNode;
};

const LabeledButton = forwardRef<HTMLButtonElement, LabeledButtonProps>(
  function LabeledButton({ children, ...props }, ref) {
    return (
      <Button ref={ref} {...props}>
        {children}
      </Button>
    );
  },
);
```

This wrapper intentionally omits the component prop, so its ref remains an HTMLButtonElement. If a wrapper allows the root component to vary, its ref type may vary too. Use Material UI's polymorphic prop types or keep the wrapper's supported root explicit.

## Type the sx prop

The sx prop accepts a theme-aware object, a callback, or an array of style values. Use SxProps with the same Theme type as the application:

```tsx
import Box from "@mui/material/Box";
import type { ReactNode } from "react";
import type { SxProps, Theme } from "@mui/material/styles";

type PanelProps = {
  children: ReactNode;
  sx?: SxProps<Theme>;
};

export function Panel({ children, sx = [] }: PanelProps) {
  const callerStyles = Array.isArray(sx) ? sx : [sx];

  return (
    <Box sx={[{ p: 2, borderRadius: 1 }, ...callerStyles]}>
      {children}
    </Box>
  );
}
```

Normalize the caller's sx value before placing it in an array. SxProps can itself be an array, so directly nesting it can produce an invalid style structure or a TypeScript error.

## Use the component prop

Material UI components can render a different root element with the component prop. The built-in component types infer many of the attributes for the selected root:

```tsx
<Button component="a" href="/reports">
  Open reports
</Button>
```

Use the corresponding semantics and props for that element. When creating your own generic wrapper around a polymorphic component, test its type behavior against the installed Material UI version; generic component and ref types can require additional typing.

## Style a component with styled

The styled utility uses the theme in its callback. Material UI's TypeScript guide notes that the resulting styled component may need to be cast back to the original component type when its generic component prop surface is important:

```tsx
import Button from "@mui/material/Button";
import { styled } from "@mui/material/styles";

const BrandedButton = styled(Button)(({ theme }) => ({
  borderRadius: theme.shape.borderRadius * 2,
  textTransform: "none",
})) as typeof Button;
```

Use this cast only when the styled wrapper should preserve the original component's public prop API. Do not cast a component to a type that accepts props its implementation does not support.

## Add a custom palette color

TypeScript needs module augmentation before application code can use a new theme color. Place declarations in a .d.ts file included by tsconfig:

```ts
import "@mui/material/styles";

declare module "@mui/material/styles" {
  interface Palette {
    brand: Palette["primary"];
  }

  interface PaletteOptions {
    brand?: PaletteOptions["primary"];
  }
}

declare module "@mui/material/Button" {
  interface ButtonPropsColorOverrides {
    brand: true;
  }
}
```

Then provide the palette tokens when creating the theme:

```ts
import { createTheme } from "@mui/material/styles";

export const theme = createTheme({
  palette: {
    brand: {
      main: "#2457C5",
      light: "#6D91E8",
      dark: "#173B8A",
      contrastText: "#FFFFFF",
    },
  },
});
```

The application can now use the custom color in its theme and supported component props:

```tsx
<Button color="brand" variant="contained">
  Create project
</Button>
```

## Add a custom component variant

When a project adds a new variant, augment the component's variant type and define the matching theme style:

```ts
import "@mui/material/Button";

declare module "@mui/material/Button" {
  interface ButtonPropsVariantOverrides {
    soft: true;
  }
}
```

```ts
import { createTheme } from "@mui/material/styles";

export const theme = createTheme({
  components: {
    MuiButton: {
      styleOverrides: {
        root: {
          variants: [
            {
              props: { variant: "soft", color: "brand" },
              style: {
                backgroundColor: "#E8EEFC",
                color: "#173B8A",
              },
            },
          ],
        },
      },
    },
  },
});
```

Type augmentation changes compile-time knowledge; it does not create runtime styles. The theme rule and the declaration must agree on the same variant name and props.

## Extend theme variables

For an application-specific theme value outside the standard palette, augment Theme and ThemeOptions:

```ts
import "@mui/material/styles";

declare module "@mui/material/styles" {
  interface Theme {
    status: {
      danger: string;
    };
  }

  interface ThemeOptions {
    status?: {
      danger?: string;
    };
  }
}
```

A declaration file only applies if it is included in the TypeScript program. Check tsconfig include patterns when an augmentation appears to have no effect.

## Common mistakes

- Copying Material UI prop definitions instead of reusing the public type.
- Using any to silence a type error instead of correcting the component contract.
- Forgetting to omit props that a wrapper takes ownership of.
- Typing a ref as a button when the component can render as an anchor.
- Nesting SxProps directly inside an sx array without normalizing it.
- Adding a custom color or variant at runtime without module augmentation.
- Adding module augmentation without adding the corresponding runtime theme data.
- Placing a .d.ts file outside the TypeScript include patterns.
- Casting a styled component to a type its implementation does not support.
- Assuming a TypeScript declaration changes runtime behavior.

## Chapter navigation

<table>
  <thead>
    <tr>
      <th>Previous lesson</th>
      <th>Chapter Q&amp;A</th>
      <th>Chapter example</th>
      <th>Next lesson</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td><a href="034-component-composition-slots-and-customization.md">034 - Component Composition, Slots, and Customization</a></td>
      <td><a href="../qnas/035-typescript-with-material-ui.md">Q&amp;A</a></td>
      <td><a href="../examples/035-typescript-with-material-ui.md">Example</a></td>
      <td><a href="036-accessibility-and-keyboard-interaction.md">036 - Accessibility and Keyboard Interaction</a></td>
    </tr>
  </tbody>
</table>
