# Styled Components Best Practices

## References

<https://www.joshwcomeau.com/css/styled-components/>
<https://www.joshwcomeau.com/css/css-variables-for-react-devs/>

## Import styled components as object

- Each component has a folder, including a js file for the component layout and a js file for the styles
- In the component:

```javascript
import * as S from './styles';
<S.Headline>{title}</S.Headline>;
```

## Using CSS variables instead of using props

```JSX
function Backdrop({ opacity, color, children }) {
  return (
    <Wrapper opacity={opacity} color={color}>
      {children}
    </Wrapper>
  )
}
const Wrapper = styled.div`
  opacity: ${p => p.opacity};
  background-color: ${p => p.color};
`;
```

This code above works fine. However, when the props change, styled-components will need to re-generate the class and re-inject it into the document's `head`

Here is another way to solve the problem, using CSS variables

```JSX
function Backdrop({ opacity, color, children }) {
  return (
    <Wrapper
      style={{
        '--color': color,
        '--opacity': opacity,
      }}
    >
      {children}
    </Wrapper>
  )
}
const Wrapper = styled.div`
  opacity: var(--opacity);
  background-color: var(--color);
`;
```

## Modify the CSS of a component when it's inside other components using Single source of styles

Let's say you have a component `TextLink` like this:

```JSX
const TextLink = styled.a`
 color: var(--color-primary);
 font-weight: var(--font-weight-medium);
`;
```

And you want to apply different styles when it's inside other component.

```JSX
// Aside.js
const Aside = ({ children }) => {
  return (
    <Wrapper>
      {children}
    </Wrapper>
  );
}

const Wrapper = styled.aside`
  /* Base styles */
  a {
    color: var(--color-text);
    font-weight: var(--font-weight-bold);
  }
`;
```

The problem with this approach is that we style the `TextLink` in different location, that could make our CSS to be harder to manage.
Here is the better way to do it

```JSX
// Aside.js
const Aside = ({ children }) => {
 /* Omitted for brevity */
};
// Export this wrapper
export const Wrapper = styled.aside`
 /* styles */
`;
export default Aside;
```

```JSX
// TextLink.js
import { Wrapper as AsideWrapper } from '../Aside';
const TextLink = styled.a`
 color: var(--color-primary);
 font-weight: var(--font-weight-medium);
 ${AsideWrapper} & {
  color: var(--color-text);
  font-weight: var(--font-weight-bold);
 }
`;
```

`&` is the placeholder for the generated class, which is like the `&` in Sass.
However this one still have a problem. You can easily bloat your `TextLink` component if it has a lot of version. To avoid that, you need to create another version of `TextLink`. For example, `TextLink` has different font on About page. This is how we do it.

```javascript
// About.js
import TextLink from '../TextLink';
const AboutTextLink = styled(TextLink)`
 font-family: 'Spooky Font', cursive;
`;
```

## `as` Props

This is really handy if the tags depend on circumstance:

```JSX
// `level` is a number from 1 to 6, mapping to h1-h6
function Heading({ level, children }) {
  const tag = `h${level}`;
  return (
    <Wrapper as={tag}>
      {children}
    </Wrapper>
  );
}
// The `h2` down here doesn't really matter,
// since it'll always get overwritten!
const Wrapper = styled.h2`
  /* Stuff */
`;
```

```JSX
function LinkButton({ href, children, ...delegated }) {
 const tag = typeof href === 'string' ? 'a' : 'button';
 return (
  <Wrapper as={tag} href={href} {...delegated}>
   {children}
  </Wrapper>
 );
}
```

## Global styles

```JSX
import { createGlobalStyle } from 'styled-components';
// Create a `GlobalStyles` component.
// I usually already have this, to include a CSS
// reset, set border-box, and other global concerns.
// You can even change the variable value based on media size
const GlobalStyles = createGlobalStyle`
  html {
    --color-text: black;
    --color-background: white;
    --color-primary: rebeccapurple;
    --buttonPadding: 10px;
    @media(min-width: 768px) {
     --buttonPadding: 20px;
    }
  }
`;
const App = ({ children }) => {
 return (
  <>
   <GlobalStyles />
   {children}
  </>
 );
};
```

and you can use this in your component

```JSX
const Button = styled.button`
 background: var(--color-primary);
`;
```

If you'd like, you can store all the value in a file `constants.js`

```javascript
const COLORS = {
 text: 'black',
 background: 'white',
 primary: 'rebeccapurple',
};
```
