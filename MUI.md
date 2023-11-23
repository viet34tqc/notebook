# MUI

## Customize a single MUI component

Use `sx` or `styled`. Here is the difference: <https://mui.com/system/styled/#difference-with-the-sx-prop>

### Using `sx`

Use `sx` attribute to override default CSS of the component and the nested component inside a single component

With `sx`, you can access to all theme properties without using `theme` variable

```jsx
<Slider
  defaultValue={30}
  sx={{
    width: 300,
    color: 'success.main', // equaivalent to theme.palette.success.main
    color: (theme) => theme.palette.success.main // or you can use a callback. This is useful when you want to calculate a value
    '& .MuiSlider-thumb': {
      borderRadius: '1px',
    },
  }}
/>
```

Variant styles based on component props

```jsx
<Box
  sx={[
    {
      '&:hover': {
        color: 'red',
        backgroundColor: 'white',
      },
    },
    foo && {
      '&:hover': { backgroundColor: 'grey' },
    },
    bar && {
      '&:hover': { backgroundColor: 'yellow' },
    },
  ]}
/>
```

### `useTheme`

If global theme is used everywhere in your component, you can use `useTheme` to get the global theme configuration

```js
const theme = useTheme()
```

### Using `styled` function

It works like styled component

```jsx
// You can also destructure the `theme` from `props`
// const GridContainer = styled(Grid)(({theme}) => ({
const GridContainer = styled(Grid)((props) => ({
  display: 'grid',
  gap: '16px',
  [props.theme.breakpoints.up('xs')]: {
    gridTemplateColumns: 'repeat(3, 1fr)'
  },
}))
```

## Styled component

```jsx
import Grid from '@mui/material/Grid';

// Theme here is the system theme. You can overide this system theme
const GridContainer = styled(Grid)(({theme}) => ({
  display: 'grid',
  gap: '16px',
  [theme.breakpoints.up('xs')]: {
    gridTemplateColumns: 'repeat(3, 1fr)'
  },
}))

<GridContainer container>
    <Grid item>
      <Item>xs</Item>
    </Grid>
    <Grid item>
      <Item>xs=6</Item>
    </Grid>
    <Grid item>
      <Item>xs</Item>
    </Grid>
</GridContainer>
```

## media query

use `theme.breakpoints.up('md')` or `theme.breakpoints.down('md')`. Need `theme` variable

```jsx
const GridContainer = styled(Grid)(({theme}) => ({
  display: 'grid',
  gap: '16px',
  [theme.breakpoints.up('xs')]: {
    gridTemplateColumns: 'repeat(3, 1fr)'
  },
}))
```

## Customize globally with MUI

### using `createTheme` (NOT RECOMMENDED)

Create a theme using `createTheme`. For example, to customize `maxWidth` of `Container` component

```tsx

// If you are using TS and you want to use your custom palette, you must add it to the interface
declare module '@mui/material/styles' {
  interface Palette {
    accent: string;
  }

  interface PaletteOptions {
    accent?: string;
  }
}

const theme = createTheme({
	palette: {
    success: {
      main: '#ff0000',
    },
    // Custom color
    accent: '#333'
  },
  // Customize breakpoint value
   breakpoints: {
    values: {
      xs: 0,
      sm: 600,
      md: 900,
      lg: 1200,
      xl: 1536,
    },
  },
  components: {
  	MuiCssBaseline: {
  		// We can use callback here to access the theme param
  		// Or we can write it as a string normally
      styleOverrides: (themeParam) => `
        h1 {
          color: ${themeParam.palette.success.main};
        }
        h2 {
        	color: grey
        }
      `,
    },

    // Customize the MUI component globally
    // 'root' is the part of classname. Inspect the component to know
    MuiPaper: {
      styleOverrides: {
        root: {
          textAlign: 'left',
        	color: '#000'
      	}
    	}
    },
    MuiContainer: {
      styleOverrides: {
        maxWidthSm: {
          maxWidth: 200,
        },
        maxWidthMd: {
          maxWidth: 320,
        },
        maxWidthLg: {
          maxWidth: 500,
        },
      },
    },
  },
});

return (
	<ThemeProvider theme={theme}>
		<CssBaseline />
		<App />
	</ThemeProvider>
)
```

### Using `extendTheme`

<https://mui.com/material-ui/experimental-api/css-theme-variables/usage/>

## Customize JOY UI

<https://mui.com/joy-ui/customization/approaches/>

### Customize globally

- Using `extendTheme` instead of `createTheme` to customize default styles of JOY UI's components and also other base CSS

```jsx
const joyTheme = extendTheme({
  fontFamily: {
    body: 'SuisseIntl,"Public Sans", var(--joy-fontFamily-fallback, var(--joy--apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol"))',
    display:
      'SuisseIntl,"Public Sans", var(--joy-fontFamily-fallback, var(--joy--apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol"))',
  },
  components: {
    JoyBadge: {
      styleOverrides: {
        badge: ({ theme }) => ({
          boxShadow: 'none',
          backgroundColor: theme.vars.palette.danger[500],
        }),
      },
    },
    JoyChip: {
      defaultProps: { size: 'sm' },
    },
  }
})
```

- Using `CssVarsProvider` instead of `ThemeProvider`
- Using `theme.vars.pallete.primary` instead of `theme.vars.primary`

`CSSVarsProvider` is more efficient than `ThemeProvider` because we can modify the CSS variable and don't have to change CSS for multiple nested components.

### Customize each library's component

- Using css variable (need to know the name of the variable in advance)

```jsx
<Input
  size="md"
  placeholder="email@mui.com"
  endDecorator={
    <Button variant="soft" size="sm">
      Subscribe
    </Button>
  }
  sx={{
    '--Input-radius': `${radius}px`,
    '--Input-decoratorChildHeight': `${childHeight}px`,
  }}
/>
```