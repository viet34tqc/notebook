# Zod

<https://scastiel.dev/zod-typescript>

- Declare: `z.object({name: z.string()})`
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

## Required field

By default, all fields in zod schema are required. However, we can still submit field data with empty string because it's valid. To solve this problem, define schema with `min` like this: `name: z.string().min(1, 'Name field is required')`

## `transform`

Use when you want to transform a piece of data after `parse`

```js
z.object({name: z.string().transform(val => `hello ${val}`)})
```

## `refine`

Validate using value from other field

```js
z.object({}).refine( data => data.password === data.confirmPassword, {
  message: 'Not match',
  path: ['confirmPassword']
})
```

## `infer`

Get type from schema (to infer into `useForm` of React Hook Form)

```js
import type { z } from 'zod';

type TRegisterFormInputs = z.infer<typeof registerSchema>

const methods = useForm<TRegisterFormInputs>({
  resolver: zodResolver(registerSchema),
  defaultValues: {
    name: '',
    email: '',
    password: '',
  },
});
```


## Validate

- `schema.safeParse(data)`
- `schema.parse(data)`

## Get error message

- Without library:

```js
const validation = email.safeParse(test);
if (!validation.success) {
  console.log(validation.error.issues.map(i => i.message));
}
```