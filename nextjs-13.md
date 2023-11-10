# NextJS 13

## References

- Concept: <https://demystifying-rsc.vercel.app/> and [official documentaion](https://nextjs.org/docs/getting-started/react-essentials)

## Server actions

- define a server action
  - server action accepts a `formData` as param
  - Then you can send POST request to the server and `revalidatePath`
- In client form, add the server action to the `action` attribute of form
