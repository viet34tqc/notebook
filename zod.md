# Zod

<https://scastiel.dev/zod-typescript>

Declare:

- `z.object({name: z.string()})`
- `z.array(z.string())`
- Validation for select: `z.enum['123', '456']`
- `z.tuple([z.string(), z.number])`
- Extend or merge from a base type
```js
const Base = z.object({id: z.string()})
const User = Base.extend({
  name: z.string()
})
// Or
const Comment = Base.merge(z.object({
  text: z.string();
}))
```
- `transform`: Use when you want to transform a piece of data after `parse`
```js
z.object({name: z.string().transform(val => `hello ${val}`)})
```
- `refine`: Validate using value from other field
```js
z.object({}).refine( data => data.password === data.confirmPassword, {
  message: 'Not match',
  path: ['confirmPassword']
})
```

Validate:

- `schema.safeParse(data)`
- `schema.parse(data)`

Get error message:

- Without library:

```js
const validation = email.safeParse(test);
if (!validation.success) {
  console.log(validation.error.issues.map(i => i.message));
}
```