# Vue

## Ecosystem

- Hooks: VueUse

## Basic concepts

- Render template

Using handlebars `{{ variable_name }}`

- In template, if we want to write JS like accessing state or executing function, we use string (`""`) instead of `{}` like in JSX. Remember to use `v-model` if you are using a variable

```html
<button @click="say('hello')">Say hello</button>
<button :class="`text-sm mb-2 ${statusColor}`">Button</button>
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
<script setup>
const input = ref(null);
onMounted() {
  input.value.focus()
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

## `v-model`

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

You might not need to declare a state to work with `v-model`, in case you are working with event handler

```html
<script setup>
import { ref } from 'vue'

function addTodo(e: KeyboardEvent) {
  const value = (e.target as HTMLInputElement).value.trim()
  if (value) {
    todos.value.push({
      id: Math.floor(Math.random() * Date.now()).toString(16),
      name: value,
      completed: false
    })
    e.target.value= ''
  }
}
</script>

<template>
  <input @keyup.enter="addTodo">
  {{newTodo}}
</template>
```

Or if you have a state as an array, your input can react to array item instead of the whole state of array

```html
<li v-for="todo in todos" :key="todo.id">
    <input type="checkbox" v-model="todo.completed" />
</li>
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

### Using `v-model` with custom component

<https://vuejs.org/guide/components/v-model.html>

### Mutate props

Vue doesn't have a `setState` function to modify the state. If we pass a state as props and we want to modify that state in child component, we should create another updater function and pass both the updater function and the state to child component

## Component

### Pass method (event) to child component

In parent component:

```html
<BlogPost
  ...
  @enlarge-text="postFontSize += 0.1"
/>
```

In child component: using `$emit` to fire the event

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

```html
<script setup lang="ts">
  // This means the component has two Props. In React, we also has props object and pass to function component as a param.
  defineProps<{
    title?: string
    likes?: number
  }>()
</script>
```

Like React, we should never mutate the props object directly, props are read-only

**Destructuring props**

<https://dmitripavlutin.com/props-destructure-vue-composition/>

We should not destructure props like React because the destructured property will lost reactivity and won't be updated when the props updates. To destructure props, you should use `toRefs`

## `computed`

- Use when you want to return a value based on the defined state, like when you want a filterd value of an array.
- The result is cached and only need to be re-evaluated once one of its reactive dependencies changes.
- Lazy Evaluation: The callback function of computed will only be run once the computed's value is being read or being used

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

## `watch` and `watchEffect`

<https://www.vuemastery.com/blog/vues-watch-vs-watcheffect-which-should-i-use/>
<https://vuejs.org/api/reactivity-core.html>

The usage of `watch` and `watchEffect` is nearly the same as `computed`, but instead of returning a value, it does something. It's more like `useEffect` in React.

- In `watch`, we need to declare a dependancy or a state or reactive value
- `watchEffect` run the callback immediately.
- `watch` is lazy by default. As opposed to `watchEffect`, `watch` will NOT execute the effect function as soon as the watched source has changed.

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

## Vue with Typescript

First, we need to declare `lang=ts` in `script` tag

```html
<script setup lang=ts></script>
```

When we import types from other modules, we have to use `import type ...` like `import type { Restaurant } from '@/types'`

### Define type for props

```ts
type PropTypes = {
  restaurant: Restaurant
}

defineProps<{
  restaurant: Restaurant
}>()

// Define props with default value
withDefaults(defineProps<{
  restaurant: Restaurant
}>(), {
  restaurant: {...}
})
```

### Typing `ref` and `computed`

Normally, `ref` and `computed` infer type from their value. If you want to specify types, you can do like this

```ts
import type { Ref, computed } from 'vue'
const year: Ref<string | number> = ref('2020')

const double = computed<number>(() => {
  // type error if this doesn't return a number
})
```

### Typing template refs

```html
<script setup lang="ts">
import { ref } from 'vue'

const el = ref<HTMLInputElement | null>(null)

</script>

<template>
  <input ref="el" />
</template>
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

## Compare to React

- Vue doesn't have `setState` function and you can mutate the state directly, while React doesn't allow. For example, you can use `push()` to add item to array state, or you can assign state to the new value (`state = newState`)
- Because of the direct mutation, you can also assign state to new value in Vue. **This could be dangerous** if we mutate one, the other mutate as well
- Vue doesn't have a `setState` function to modify the state. If we pass a state as props and we want to modify that state in child component, we should create another updater function and pass both the updater function and the state to child
- Data binding: Data binding is easier in Vue. For example, for checkbox value, you only need to define a state and assign it to element via `v-model`. Updating value is handled automatically. Wheareas, React handles it manually via a `setState`
- List rendering: I prefer using array method (`map`, `filter`) to render a list in React because it's more like JS
- Computed: React doesn't need API to calculate a state based on other state. Vue requires `computed()`
- Method props: In React, prop as a method can be call directly, in Vue we need to `defineEmit` first and use `$emit()` to emit events from parent
- Working with TS: React makes it easier to work with TS, at least for typing event handlers. Vue doens't have type for each kind of event.
- You cannot destructure props in Vue like React as the destructured properties will lost its reactivity.
- Using `v-model` for Custom component is not straighfoward
- You might not need to declare a state to work with input, in case you are working with event handler [https://vuejs.org/examples/#todomvc](https://vuejs.org/examples/#todomvc)
