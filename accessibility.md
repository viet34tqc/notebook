# Accessibility

## Reference

- <https://dev.to/this-is-learning/top-20-must-know-tips-for-web-accessibility-452>
- <https://accessibleweb.dev/>

## Common ARIA attributes

Aria attributes have the `aria-` prefix. Some common ones include:

- `aria-label`: often used in a button, so the screen reader will tell us the value of `aria-label` when we click the button
- `aria-labelledby="some-id"` will tell the user agent that this element's label is #some-id
- `aria-hidden="true"` will tell user agent to hide its contents to accessibility tools (e.g. screen readers).
For example if you had a two identical images but with different size (with alt tag) to help label something, you might want to hide one of them to screen readers as it wouldn't need to read the same thing out twice
- `aria-checked="true"` used for checkboxes to say if its checked or not

## Common ARIA roles

Use the role="something" attribute for HTML elements that are not available for that role to tell screen-reader what kind of element this is meant to be. There are a set of standard values you can use. Here are some examples:

- `<span role="button">`
- `<span role="tab">` for a tab button
- `<div role="tabpanel"> `for a tab panel (which is hidden/visible)