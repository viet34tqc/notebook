# React Testing

## Reference

<https://www.codecademy.com/learn/learn-react-testing/modules/react-testing-library/cheatsheet>

<https://www.smashingmagazine.com/2020/06/practical-guide-testing-react-applications-jest/>
<https://jestjs.io/docs/tutorial-react>

## Tools

- Jest: provides API for testing
- Jest Preview: preview the HTML without imagining the layout in your head (<https://github.com/nvh95/jest-preview>)
- @testing-library/react: testing DOM manipulation, provides `render` for rendering a component, `fireEvent` for stimulating an event (such as 'click')

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
- `findByText`: use with asynchronous test.

For assertion

- `DomElem.toHaveTextContent('text')`
- `Input.toHaveValue('value')`
- `toBeEnabled()`
- `toBeInTheDocument()`
- `toBeNull()`
- `Function.toHaveBeenCalled()`

Recap:

- If you want to select an element that is rendered after an asynchronous operation, use the `findBy*` or `findByAll*` variants.
- If you want to assert that an element should not be in the DOM (using `not.toBeInTheDocument` and `toBeNull`, use `queryBy*` or `queryByAll*` variants. Otherwise use `getBy*` and `getByAll*` variants.

### Event

- `fireEvent` from `@testing-library/react`
```js
fireEvent.click(
	screen.getByRole('button', { name: 'Subtract from Counter' })
);
```

- `user` from `@testing-library/user-event`
```js
user.click(
	screen.getByRole('button', { name: 'Subtract from Counter' })
);

userEvent.type(screen.getByRole('input'), 'Oreos are delicious');
```

## Examples

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


## Mock

<https://pawelgrzybek.com/mocking-functions-and-modules-with-jest/>

`jest.fn(implementationFunction)`: mock a *function*.
<https://www.pluralsight.com/guides/how-does-jest.fn()-work>
`jest.spyOn(object, methodName)`: mock a *method of object*
<https://jestjs.io/docs/jest-object#jestspyonobject-methodname>
`jest.mock()`: mock a *module*

## Config with TypeScript

### Packages to installed:

- ts-jest (version 26.4.2: Fix `Jest: a transform must export something`)
- jest-scss-transform (if you are using SCSS import)
If you are not using create-react-app (like: NextJS), you need to install these additional packages
- @testing-library/jest-dom
- @testing-library/react
- @testing-library/user-event
- @types/jest

### Configuration files

If you are using CRA, this configuration file won't work by default and you have to run `react-scripts test -- --config jest.config.js`. However, it's NOT RECOMMEND to have a seperate jest configuration file with CRA. If you need other settings, put it in the `package.json`

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

This is the component that uses asynchronous code (fetch) to render. Normally, we call an API and update the state. When the state updates, the component re-renders.

When you test this kind of component, you might get an error like this:

`When testing, code that causes React state updates should be wrapped into act(...):`
`Can't perform a React state update on an unmounted component.`

Therefore, we need to wait for the API to run and the component to re-render before doing other test. The reason is that the test will have finished by the time the component is done with first rendering and it doesn't aware of this re-render

So, how to wait for the component to re-render? First, make a test for the view of the component after re-render. This test should be on top of other test. You will have to use `findBy` and `async/await` or `waitFor` because we are dealing with asynchronous code, 
