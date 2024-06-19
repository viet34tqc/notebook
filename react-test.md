# React Testing

## Reference

- Mocking: <https://matthewwolfe.github.io/blog/mocking-in-unit-tests>
- <https://dev.to/refine/a-comprehensive-guide-of-react-unit-testing-18bh>
- <https://www.codecademy.com/learn/learn-react-testing/modules/react-testing-library/cheatsheet>
- <https://www.smashingmagazine.com/2020/06/practical-guide-testing-react-applications-jest/>
- <https://jestjs.io/docs/tutorial-react>

## Examples

<https://github.com/react-hookz/web>

## Packages to install:

If we are not using vite (CRA, NextJS), we can use `jest`. Otherwise, vitest is recommended rather than vite (<https://github.com/CodingGarden/react-ts-starter>)
t
Required packages:

- Test runner
  - Jest: provides API for testing
  - Vitest: faster
- @testing-library/jest-dom: <https://github.com/testing-library/jest-dom#usage>
- @testing-library/react: testing DOM manipulation, provides `render` for rendering a component, `fireEvent` for stimulating an event (such as 'click')
- @testing-library/user-event: simulate the real events, `userEvent.click` simulates the event more realistically than `fireEvent`.
- eslint-plugin-testing-library

If you use Jest:

- ts-jest (typescript project)
- @types/jest (typescript project)
- jest-environment-jsdom: By default, Jest uses node for testing. This package creates browser env for UI tests
- eslint-plugin-jest-dom: provides a set of custom jest matchers, like `toBeInTheDocument`, `toHaveClass`...
- jest-scss-transform (if you are using SCSS import)
- jest-svg-transformer (for svg)

Optional:

- identity-obj-proxy (css modules)
- Jest Preview: preview the HTML without imagining the layout in your head (<https://github.com/nvh95/jest-preview>)
- ts-node (optional)

## Configuration files

### Using Jest

If you are using CRA, this configuration file won't work by default and you have to run `react-scripts test -- --config jest.config.js`. However, it's NOT RECOMMEND to have a seperate jest configuration file with CRA. If you need other settings, put it in the `package.json`

If your project use JSX, you will need `babel` setup, otherwise, `ts-jest` will handle `ts, tsx` files

- `jest.config.js`

```js
module.exports = {
	preset: 'ts-jest',
	testEnvironment: 'jsdom',
	globals: {
		'ts-jest': {
			tsconfig: './tsconfig.json',
		},
	},
	transform: {
		'^.+\\.jsx?$': 'babel-jest',
		'^.+\\.scss$': 'jest-scss-transform',
	},
	moduleNameMapper: {
    '^.+\\.svg$': 'jest-svg-transformer',
    '^.+\\.css$': 'identity-obj-proxy',
  },
	setupFilesAfterEnv: ['./jest.setup.ts'],
};
```
- `jest.setup.ts`

`import '@testing-library/jest-dom';`

- `babelrc`
```
{
	"presets": ["@babel/preset-env", "@babel/preset-react"]
}
```

### Using Vitest

```ts
/// <reference types="vitest" />
import react from '@vitejs/plugin-react';
import { defineConfig } from 'vite';

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [react()],
  test: {
    globals: true, // So we can use function from vitest globally without importing it
    environment: 'jsdom',
    setupFiles: './setupTests.ts',
    coverage: {
      all: true,
      provider: 'istanbul',
      reporter: ['text', 'json', 'html'],
      include: ['src/**/*.tsx'],
      exclude: [],
    },
  },
});

// tsconfig.json
"compilerOptions": {
		"types": ["vitest/globals"],
}

// setupTests.ts
import '@testing-library/jest-dom';
import * as matchers from '@testing-library/jest-dom/matchers';
import { cleanup } from '@testing-library/react';
import { afterEach, expect } from 'vitest';

expect.extend(matchers);

afterEach(() => {
  cleanup();
});
```

## Type of testing

- Unit testing: test individual component, ex: test component with props (**less important**)
- Integration testing: test interaction between components to see if they works properly together (**important**)
- Snapshot testing: create a snapshot of a component and test if that component has been changed (**unimportant**)

## Basic test structure

### Single tests

Use `it` or `test`

**NOTE**: after each `it` block, rendered React component is unmounted => you need to re-render it in the next test

```jsx
it('description of the test', () => {
	render(<Counter defaultCount={0} description="My Counter" />); // Render the component with props'value
	expect(screen.getByText(/Current Count: 0/)).toBeInTheDocument(); // Query the DOM element and expect it to be in the document
	expect(screen.getByText(/My Counter/)).toBeInTheDocument();
});

it('defaultCount = 0, click plus, counter = 1', () => {
	render(<Counter defaultCount={0} description="My Counter" />);
	fireEvent.click(screen.getByRole('button', { name: 'Add to Counter' })); // Query the button by aria-label (or by inner text) then trigger click event.
	expect(screen.getByText('Current Count: 1')).toBeInTheDocument();
});
```

### Grouped Test

Use `describe` block

```js
describe('When + button is clicked', () => {
	// Run this before each test
	beforeEach(() => {
		fireEvent.click(screen.getByRole('button', { name: 'Add to Counter' }));
	});

	// You might have more test with + button click here
	it('defaultCount = 0, click plus, counter = 1', () => {
		expect(screen.getByText('Current Count: 1')).toBeInTheDocument();
	});
});
```

You can have `describe` in `describe` block. This will help your test more readable

```js
describe('Test counter', () => {
	describe('When + button is clicked', () => {})
})
```

## DOM interaction

### Query selector and assertion function

Use `screen` to query element

- `getByText`: query element by the inner text: `screen.getByText('/confirm/i)`
- `getAllBy*()[0]`: query the first element
- `getByRole`: query element by HTML tag: `screen.getByRole('button', {name: /confirm/i})`
- `getByLabelText`: query input by label
- `queryByText`: Returns the matching node for a query, and return null if no elements match. This is useful for asserting an element that is not present. Throws an error if more than one match is found. Use with `not.toBeInTheDocument`. If the element is disappears asynchronously, we can combine with `waitFor()`
- `findBy*`: use with asynchronous test: `expect(await screen.findByRole('button', { name: /one up/i })).toBeInTheDocument()`

For assertion

- `DomElem.toHaveTextContent('text')`
- `Input.toHaveValue('value')`
- `toBeEnabled()`
- `toBeInTheDocument()`
- `toBeNull()`
- `Function.toHaveBeenCalled()`
- `toHaveStyle`: test the `style` attribute of element. Use case: when we change the `style` attribute via js. Remember to use findBy* if the test fails

**RECAP**:

- If you want to select an element that is rendered after an asynchronous operation, use the `findBy*` or `findByAll*` variants.
- If you want to assert that an element should not be in the DOM (combine `queryBy*` or `queryByAll*` with `not.toBeInTheDocument` or `toBeNull`) - Otherwise, use `getBy*` and `getByAll*` variants.

### Event

- `fireEvent` from `@testing-library/react`

```js
fireEvent.click(
	screen.getByRole('button', { name: 'Subtract from Counter' })
);
```

- `userEvent` from `@testing-library/user-event`

```js
userEvent.click(
	screen.getByRole('button', { name: 'Subtract from Counter' })
);

userEvent.type(screen.getByRole('input'), 'Oreos are delicious');
```

## Examples

### Get an element from a rendered component

```jsx
it('should show correct action buttons on edit tab', async () => {
	// Get the button inside ReportingActions component
	// We can also use `screen.getByRole`
	const { getByRole } = render(<ReportingActions />)
	const editButton = getByRole('button', { name: 'Edit Tab' })
	expect(editButton).toBeInTheDocument()
})
```


### Click a button then remove an element

```	jsx
render(<App/>);

const button = screen.getAllByText('×')[0]

// TODO: Mimic clicking on the button
userEvent.click( button );

// We grab the thought again. It should be null after we clicked the '×' button using userEvent.
const removedThought = screen.queryByText('This is a place for your passing thoughts.')
expect(removedThought).toBeNull()
```

### Wait for an element to be disappeared

```	jsx
test('should remove header display', async () => {
  // Render Header
  render(<Header/>)
  // Extract button node 
  const button = screen.getByRole('button');
  // click button
  userEvent.click(button);
 
  // Must have `await` before `waitFor`
  await waitFor(() => {
    const header = screen.queryByText('Hey Everybody');
    expect(header).toBeNull()
  })
});
```

## Test for class

```jsx
describe('Test AvatarGroup', () => {
	it('should have class md if size = md', () => {
		const maxLength = 2;
		/*
		Or:
		render(<AvatarGroup data={data} size="md" maxLength={maxLength} />);
		const container = screen.getByTestId('avatar-wrapper');
		*/
		const { getByTestId } = render(
			<AvatarGroup data={data} size="md" maxLength={maxLength} />
		);
		const container = getByTestId('avatar-wrapper');
		expect(container.getAttribute('class')).toContain('md');
	});
});
```

## Mock

<https://pawelgrzybek.com/mocking-functions-and-modules-with-jest/>

`jest.fn(implementationFunction)`: mock a *function*.
<https://www.pluralsight.com/guides/how-does-jest.fn()-work>
`jest.spyOn(object, methodName)`: mock a *method of object*
<https://jestjs.io/docs/jest-object#jestspyonobject-methodname>
`jest.mock()`: mock a *module*

## Debug test

If you are stuck in the middle of the test and want to know what is rendered during this test, you could use `screen.debug()`

## Asynchronous test

Using `msw` packages. The requests are sent to `msw` instead of the real server. We need to mock the data to be return on each requests

**NOTE**

- Test with CRA

When we do asynchonous test using creat-react-app, we need to run `yarn test` which runs `react-scripts test`. The reason is react-scripts will run polyfill for `fetch` (Jest run in NodeJS environment where `fetch` is not available). Otherwise, we might encounter error `ReferenceError: fetch is not defined`

In case we don't use CRA, then we will have to import `fetch` polyfill by ourselves.

- Request matching:

<https://mswjs.io/docs/basics/request-matching>
We have to provide exact request URL string, only those request that match that string are mock

For example, we send a GET request to this URL: 'https://example.com/todo'. Then in the request using msw, we need to provide the same URL
`rest.post('https://example.com/todo', responseResolver)`

## Context test

Before testing, we have to render the context and the component to test that lied inside that context

```jsx
const value = {
	todos: [],
	setTodos: jest.fn(),
};
beforeEach(() => {
	render(
		<TodoContext.Provider value={value}>
			<TodoForm />
		</TodoContext.Provider>
	);
});
```

In case you are testing a component that **requires re-rendering**, you need to use the parent component of context provider
```jsx
<TodoContextProvider>
	<TodoList />
</TodoContextProvider>
```
If you pass only the context provider, the child component might not be re-rendered

### Async rendering component

- <https://github.com/threepointone/react-act-examples/blob/master/sync.md>

First method: Using `act()` to wrapped around the event handler. Explaination is from the above link. So everytime, your test involves a user action (click, fetch...), which alters the state, try wrapping the user action with `act()`

This is the component that uses asynchronous code (fetch) to render. Normally, we call an API and update the state. When the state updates, the component re-renders.

When you test this kind of component, you might get an error like this:

`When testing, code that causes React state updates should be wrapped into act(...):`
`Can't perform a React state update on an unmounted component.`

Therefore, we need to wait for the API to run and the component to re-render before doing other test. The reason is that the test will have finished by the time the component is done with first rendering and it doesn't aware of this re-render

So, how to wait for the component to re-render? First, make a test for the view of the component after re-render. This test should be on top of other test. You will have to use `findBy` and `async/await` or `waitFor` because we are dealing with asynchronous code, 
