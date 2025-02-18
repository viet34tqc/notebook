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

## `coerce`

coerce to another data type. Often used when the data type passed in is having a different data type from what we want. For example, the formData submitted from a form is always string and we need a a piece of data must be number

```js
z.object({
  amount: z.coerce.number(),
});
```

## `refine`

<https://wasp-lang.dev/blog/2025/01/22/advanced-react-hook-form-zod-shadcn>

Allow us to do custom validation to enhance Zod validation

```js
// In this example, first we check if the value is string, then we check its length.
z.string().refine((val) => val.length <= 255, {
  // You can also pass in a string instead of object: "String can't be more than 255 characters"
  message: "String can't be more than 255 characters", 
});
```

Validate using value from other field

```js
const schema = z.object({
    password: z.string(),
    confirm: z.string(),
  }).refine( data => data.password === data.confirmPassword, {
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

## `omit`

Remove any fields from validation

```js
const CreateInvoice = InvoiceSchema.omit({ id: true, date: true });
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