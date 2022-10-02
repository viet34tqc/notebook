# Vue

## Basic concepts

- Render template

Using handlebars `{{ variable_name }}`

- Using normal string for function instead of `{}` in JSX

```vue
<button @click="say('hello')">Say hello</button>
```

## Event handling

Syntax: `@event_name="method_name"`, ex: `<button @click="say('hello')">Say hello</button>`

- Pass DOM event into method: use `$event`

```
<button @click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>
```

- Event modifiers like `preventDefault`: `@click.prevent="onSubmit"`
- Key modifiers: 

<https://vuejs.org/guide/essentials/event-handling.html#key-modifiers>
`<input @keyup.enter="submit" />` or `<input @keyup.alt.enter="clear" />`

## Form inputs

Vue is two-ways binding, which means when the UI change, the state update and vice versa.

Input using `v-modal` to synchronize value with the state.

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


