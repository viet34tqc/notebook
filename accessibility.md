# Accessibility

## Reference

- <https://dev.to/this-is-learning/top-20-must-know-tips-for-web-accessibility-452>
- <https://accessibleweb.dev/>

## Common ARIA attributes

Aria attributes have the `aria-` prefix. Some common ones include:

- `aria-label`: Adds an accessible label to elements that do not have visible text. For example, a search button with only an icon should include an `aria-label` clarifying it. It's often used in a button, so the screen reader will tell us the value of `aria-label` when we click the button
- `aria-labelledby="some-id"` will tell the user agent that this element's label is #some-id
- `aria-hidden="true"` Hides elements from screen readers without removing them visually. This is ideal for decorative or redundant elements. For example: `<ReactLogo aria-hidden />`
- `aria-checked="true"` used for checkboxes to say if its checked or not

## Common ARIA roles

Use the role="something" attribute for HTML elements that are not available for that role to tell screen-reader what kind of element this is meant to be. There are a set of standard values you can use. Here are some examples:

- `<span role="button">`
- `<span role="tab">` for a tab button
- `<div role="tabpanel"> `for a tab panel (which is hidden/visible)

## So What we need to do

- Using semantic HTML
- Styling: Enhance accessibility through focus indicators, responsive design, and reduced motion.
  - Highlight the focused element with an outline (as covered earlier).
  - Style links and buttons, ensure links look like links and buttons look like buttons, and they are easy to interact with.
  - Provide clear hover, active and disabled states.
  - Color contrast: Use sufficient contrast to distinguish elements.
  - Responsive design: 
  	- Support custom font sizes and zooming.
  - Font and spacing: Use clear fonts with adequate spacing, particularly for users with dyslexia.
- Using `ARIA Atributes` only when semantic elements can't achieve the desired result. See the section above