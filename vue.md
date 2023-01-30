# Vue

## Basic concepts

- Render template

Using handlebars `{{ variable_name }}`

- Using normal string for function instead of `{}` in JSX

```html
<button @click="say('hello')">Say hello</button>
```

- Single-file component

To declare a component in Vue, we using a file with the suffix '.vue'. It includes 3 parts: component's logic, template (HTML) and style (CSS)

```html
<script></script>
<template>
  <button>Save</button>
</template>
<style>
  button {
    font-weight: bold;
  }
</style>
```

There are two types of SFC: the legacy is options API and the newer one is composition API. If you come from React using functional component, composition API is the better choice. For more details, go to <https://vuejs.org/guide/introduction.html#api-styles>

## Event handling

Syntax: `@event_name="method_name"`, ex: `<button @click="say('hello')">Say hello</button>`

- Pass DOM event into method: use `$event`

```html
<button @click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>
```

- Event modifiers like `preventDefault`: `@click.prevent="onSubmit"`
- Key modifiers:

<https://vuejs.org/guide/essentials/event-handling.html#key-modifiers>
`<input @keyup.enter="submit" />` or `<input @keyup.alt.enter="clear" />`

## `Ref`

Like `ref` in react. You can only access ref after the component is mounted

```html
<script>
export default {
  mounted() {
    this.$refs.input.focus()
  }
}
</script>

<template>
  <input ref="input" />
</template>
```

## State

To define the state, we use `ref` like `useState` in React.
The ref's value is accessed by `ref.value` in `script` tag. In `template`, we can use `ref` directly

```html
<script setup>
import { ref } from 'vue'
const newTodo = ref('')
console.log(newTodo.value);
</script>

<template>
  {{newTodo}}
</template>
```

In Vue, state can be mutated. To update the state, we don't need a function like `setState` like React, just replace the old one with the new one

```js
todos.value.push(newTodo)
//or
todos.value = todos.value.filter(/* ... */)
```

## Form inputs

Vue is two-ways binding, which means when the UI change, the state update and vice versa.

Input using `v-model` to synchronize value with the state.

```html
<script setup>
import { ref } from 'vue'


const newTodo = ref('')
</script>

<template>
  <input v-model="newTodo">
  {{newTodo}}
</template>
```

```html
`<input v-model="text">`
<!-- `picked` is a string "a" when checked -->
<input type="radio" v-model="picked" value="a" />

<!-- `toggle` is either true or false -->
<input type="checkbox" v-model="toggle" />

<!-- `selected` is a string "abc" when the first option is selected -->
<select v-model="selected">
  <option value="abc">ABC</option>
</select>
```

### Modifiers

- `.number`:

Here, `age` is typecast to number

```html
<input v-model.number="age" />
```

- `.trim`:

Here, value from input is trim automatically

```html
<input v-model.trim="msg" />
```

## Component

### Pass method (event) to child component

In parent component:

```html
<BlogPost
  ...
  @enlarge-text="postFontSize += 0.1"
/>
```

In child component: using `@emit` to fire the event

```html
<!-- BlogPost.vue, omitting <script> -->
<template>
  <div class="blog-post">
    <h4>{{ title }}</h4>
    <button @click="$emit('enlarge-text')">Enlarge text</button>
  </div>
</template>
```

To `emit` in `<script setup>`

```html
<script setup>
const emit = defineEmits(['customChange'])
const handleChange = (event) => {
  emit('customChange', event.target.value.toUpperCase())
}
</script>
```

### `slot`

This is like `children` prop in React

```jsx
<AlertBox>
  Something bad happened.
</AlertBox>
```

To render the content inside the `AlertBox` component, using slot

```jsx
<template>
  <div class="alert-box">
    <strong>This is an Error for Demo Purposes</strong>
    <slot />
  </div>
</template>
```

### Props

Props need to register before using. We should declare type or validation for the props as well (<https://vuejs.org/guide/components/props.html#prop-validation>)

```js
// This means the component has two Props. In React, we also has props object and pass to function component as a param.
export default {
  props: {
    title: String,
    likes: Number
  }
}
```

Like React, we should never mutate the props object directly, props are read-only

## `computed`

Use when you want to return a value based on the defined state, like when you want a filterd value of an array.

```html
<script setup>
import { ref, computed } from 'vue'

const tasks = ref([
  {name: 'Task 1', completed: true},
  {name: 'Task 2', completed: false}
])

// a computed ref
const completedTasks = computed(() => {
  return tasks.value.filtered( task => task.completed )
})

console.log( completedTasks.value)
</script>

<template>
  {{complatedTasks}}
</template>
```

## `watch`

The usage of `watch` is nearly the same as `computed`, but instead of returning a value, it does something

`watch` and `watchEffect`: <https://www.vuemastery.com/blog/vues-watch-vs-watcheffect-which-should-i-use/>

- In `watch`, we need to declare a dependancy or a state or reactive value
- `watch` is lazy by default. As opposed to `watchEffect`, `watch` will NOT execute the effect function as soon as the `watch` method declaration is reached

To watch an props

```js
const props = defineProps();

watch(() => props.modalActive, (value) => {} )
```

## Fetch data

<https://vuejs.org/examples/#fetching-data>

We put the code for fetching data in `watchEffect` (like `useEffect` in React)
`watchEffect` take no dependancy array like `useEffect`. It re-rerun automatically when the state in the callback change

## Transition component

This is a special component used to give the UI transition. It comes with some some built-in class: [name]-enter-active, [name]-leave-active, [name]-enter-from, [name]-enter-to, [name]-leave-from, [name]-leave-to. `name` here is the `name` attribute of Transition component. These classes are like the state of the component, so you will need to add style to it.

```jsx
<!-- We will have .modal-outer-enter-from, .modal-outer-enter-to and so on...-->
<Transition name="modal-outer">
```

## Vue Router

### `useRoute`

Use this hook when you want to get params and query variables

### `useRouter`

Use this hook when you want to redirect to other page

### Change the title before changing route

```js
router.beforeEach((to, from, next) => {
  document.title = `${
    to.params.state ? `${to.params.city}, ${to.params.state}` : to.meta.title
  } | Weather App`;
  next(); // call next() to render the route's component
});
```

### Compare to react

- Vue can mutate the state, React doesn't
