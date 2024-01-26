# Storybook

- <https://formidable.com/blog/2023/unlocking-the-power-of-storybook/>
- <https://github.com/tnarla/nextjs-conf>

Storybook is a tool for creating, testing and showcasing the UI of the component in isolation.

Why: Let's say we have two components: parent and child. The parent passes down some data to the child component via props. To test the UI of the child component, we need to do some interaction on the parent first by doing some interaction like clicking or fetching data => make it harder for testing

## Concepts

- Story: is like the variation of the component. For example: A button component may have a 'Primary' story, 'Large' store... Each story is a combination of the component's props
- `*.stories.tsx`: stories defination of the component
- `args`: stands for arguments that defines how the component should look like when it renders on the storybook
- `meta`: just like the metadata of a webpage, defines the information of all stories. `args` and `argsTypes`, if defined in the `meta` object are applied for all the stories. You can manually define them in each component.
  - title: meta title of the component's stories. It's like meta title of webpage. It also defines the name of the section on the sidebar. Ex: 'Example/Button' => 'Example' is the name of the section, 'Button' is the name of the component on the sidebar (<https://i.imgur.com/XVrTg1q.png>)
  - component: the component itself
  - parameters layout: story layout, the position of the stories when display on the Canvas tab 
  - `argsTypes`: defines the type of the `args` in each story (<https://storybook.js.org/docs/api/arg-types>)
