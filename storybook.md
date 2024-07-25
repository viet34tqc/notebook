# Storybook

- <https://formidable.com/blog/2023/unlocking-the-power-of-storybook/>

Examples: 

- <https://github.com/carbon-design-system/ibm-products>
- <https://github.com/tnarla/nextjs-conf>

Storybook is a tool for creating, testing and showcasing the UI of the component in isolation.

Why: Let's say we have two components: parent and child. The parent passes down some data to the child component via props. To test the UI of the child component, we need to do some interaction on the parent first like clicking or fetching data => testing the child component depends on the parent.

## Concepts

- Story: is like the variation of the component. For example: A button component may have a 'Primary' story, 'Large' store... Each story is a combination of the component's props
- `*.stories.tsx`: stories defination of the component
- `args`: stands for arguments that defines how the component should look like when it renders on the storybook. For React component, they are component's props
- `argsTypes`: provides information about `args` (props), like how we change the value of the props on storybook.
- `meta`: just like the metadata of a webpage, defines the information of all stories. `args` and `argsTypes`, if defined in the `meta` object are applied for all the stories. You can manually define them in each component.
  - title: meta title of the component's stories. It's like meta title of webpage. It also defines the name of the section on the sidebar. Ex: 'Example/Button' => 'Example' is the name of the section, 'Button' is the name of the component on the sidebar (<https://i.imgur.com/XVrTg1q.png>)
  - component: the component itself
  - parameters layout: story layout, the position of the stories when display on the Canvas tab 
  - `argsTypes`: defines the type of the `args` in each story (<https://storybook.js.org/docs/api/arg-types>)

## Create a story

```ts
const meta: Meta = {
  component: AlertDialog,
};

const DemoButton = (args: ComponentProps<typeof Button>) => {
  return <div><Button /></div>
}

export const Default: Story = {
  args: {
    children: 'Button',
    variant: 'default',
    size: 'default',
  },
  // By default, `render` will render the component which is passed in as the value of `component` property in `meta` object
  // Here, we can create a custom rendered component
  // `args` is the args define above
  render: args => <Button {...args} />
};
```

By using custom `render` like in the example above, we can create a template of the component that will be reuse in all stories

```ts
const ButtonTemplate: Story = {
  render: args => <Button {...args} />
};

export const RedButton: Story = {
  ...ButtonTemplate,
  args: {
    color: 'red'
  }
}
```

