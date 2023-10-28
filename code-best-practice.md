# Coding best practice

## Return early

<https://youtube.com/watch?v=CFRhGnuXG-4>

Let's say we have an `if-else` block. The code inside `else` block is shorter, like we are just throwing an error or returning a value. Then, we convert that `else` block into another `if` statement and return early. By doing this, we can remove `if` statement in the original `if-else` block

## Inversion of control

Make your function (or component) open to new modification instead of changing the base function or component (increasing the number of params or props)

- <https://frontendmastery.com/posts/building-future-facing-frontend-architectures/>
- <https://kentcdodds.com/blog/inversion-of-control>

## Reduce nested level inside your code

<https://www.youtube.com/watch?v=CFRhGnuXG-4>

- Extract nested blocks into its own function
- Return early