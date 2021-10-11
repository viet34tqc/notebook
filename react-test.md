# React Testing

## Reference

<https://www.smashingmagazine.com/2020/06/practical-guide-testing-react-applications-jest/>
<https://jestjs.io/docs/tutorial-react>
<https://www.freecodecamp.org/news/testing-react-hooks/>

## Tools

- Jest: provides API for testing
- @testing-library/react: testing DOM manipulation, provides `render` for rendering a component, `fireEvent` for stimulating an event (such as 'click')

## Type of testing

- Unit testing: test individual component, ex: test component with props (**less important**)
- Integration testing: test interaction between components to see if they works properly together (**important**)
- Snapshot testing: create a snapshot of a component and test if that component has been changed (**unimportant**)

## Basic test structure

### Single tests

Use `it`

**NOTE**: after each `it` block, React component is unmounted
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

### Query selector

Use `screen` to query element

- `getByText`: query element by the inner text
- `getByRole`: query element by HTML tag
- `getByLabelText`: query input by label
- `findByText`: use with `await`

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
```

## Mock

<https://pawelgrzybek.com/mocking-functions-and-modules-with-jest/>

`jest.fn(implementationFunction)`: mock a *function*.
<https://www.pluralsight.com/guides/how-does-jest.fn()-work>
`jest.spyOn(object, methodName)`: mock a *method of object*
<https://jestjs.io/docs/jest-object#jestspyonobject-methodname>
`jest.mock()`: mock a *module*

## Key things to note

- `it` or `test`: pass a function to this method, and the test runner would execute that function as a block of tests.
- `describe`: This optional method is for grouping any number of it or test statements.
- `expect`: This is the condition that the test needs to pass. It compares the received parameter to the matcher. It also gives you access to a number of matchers that let you validate different things. You can read more about it in the documentation.
- If you are using `@testing-library/react`:
  - `render`: render a React component
  - `screen` to query element
    - `getByText`: query element by the inner text
    - `getByRole`: query element by HTML tag
    - `getByLabelText`: query input by label
  - `fireEvent`: trigger Event, for example: click


