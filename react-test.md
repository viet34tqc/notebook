# React Testing

## Reference

<https://www.smashingmagazine.com/2020/06/practical-guide-testing-react-applications-jest/>
<https://jestjs.io/docs/tutorial-react>
<https://www.freecodecamp.org/news/testing-react-hooks/>

## Tools

- Jest: provides API for testing
- Enzyme: provides `mount` and `shallow` to make a copy of React Component
- @testing-library/react: testing DOM manipulation, provides `render` for rendering a component, `fireEvent` for stimulating an event (such as 'click')

## Type of testing

- Unit testing: test individual component, ex: test component with props (**less important**)
- Integration testing: test interaction between components to see if they works properly together (**important**)
- Snapshot testing: create a snapshot of a component and test if that component has been changed (**unimportant**)

## Key things to note

- `it` or `test`: pass a function to this method, and the test runner would execute that function as a block of tests.
- `describe`: This optional method is for grouping any number of it or test statements.
- `expect`: This is the condition that the test needs to pass. It compares the received parameter to the matcher. It also gives you access to a number of matchers that let you validate different things. You can read more about it in the documentation.
- `mount` This method renders the full DOM, including the child components of the parent component, in which we are running the tests.
- `shallow` This renders only the individual components that we are testing. It does not render child components. This enables us to test components in isolation.
