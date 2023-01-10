# React Hook Form

## Example

<https://github.com/react-hook-form/react-hook-form/tree/master/examples>

## Basic usage

We only need to use `handleSubmit` and `register` from `userForm` hook to make your form work with React Hook Form

```ts
export interface FormValues {
	name: string;
	description: string;
}

const {
	register,
	handleSubmit,
} = useForm<FormValues>();

const onSubmit = (data: FormValues) => console.log(data);

<form onSubmit={handleSubmit(onSubmit)}>
	<Input registration={register('name')} />
	<input {...register('name')} />
	<textarea {...register('description')}></textarea>
</form>
```

In case you need to provide **default value** of the inputs:

```ts
const initialValues = {
	name: 'viet',
	description: 'dev'
}
const {
	register,
	handleSubmit,
} = useForm<AddEditFormInput>({
	defaultValues: initialValues
});
```

Now the form works, however, user can submit invalid data, like: wrong email format, name is too short, etc.. So you might need to validate the inputs'value before submiting

React Hook Form has its own basic validation. But for the better maintainance, we will use **Yup** library which is fully supported by Reack Hook Form.

```ts
import * as yup from 'yup';
const schema = yup.object({
	name: yup.string().min(15).required(),
	description: yup.string()
})

const {
	register,
	handleSubmit,
} = useForm<AddEditFormInput>({
	resolver: yupResolver(schema),
	defaultValues: initialValues
});
```

When the user submit wrong data format, it is pretty sure that we will need a place to display the errors to the user. The errors' message are extracted from `useForm` hook.

```ts
const {
	register,
	handleSubmit,
	formState: {errors}
} = useForm<AddEditFormInput>({
	resolver: yupResolver(schema),
	defaultValues: initialValues
});
```

`errors` is an object that includes error's message of each input. Then we can put those messages below the inputs

```jsx
<input id={name} {...register(name)} {...inputProps} />
{errors?.name?.message && (
	<p style={{ color: 'red', fontSize: '13px' }}>
		{errors?.name?.message}
	</p>
)}
```

## `FormError` component

In order to display error message, we can pass `errorMess` prop to form field component. However, if the form has lots of fields, this could be tedious and error-prone. Instead, we can create a `FormError` component that use `FormContext` to retrieve the error for fields

```tsx
interface FieldErrorProps {
	name?: string;
}

export function FieldError({ name }: FieldErrorProps) {
	const {
		formState: { errors },
	} = useFormContext();

	if (!name) return null;

	const error = errors[name];

	if (!error) return null;

	return <div className="text-sm text-red-500 font-bold">{error.message}</div>;
}
```

## `isSubmitting`

`isSubmitting` comes in handy when you are performing asynchronous submit function. Then you can add that value to disabled props of submit button.

## `watch`

It replace the use of `useState`. RHF will watch value of all register fields. However, when using watch, all the fields will become uncontrolled input => the component will be re-rendered when the input changes

## `reset`

- When you want to reset the form to default values after submitting successfully, you need to put `reset` inside `useEffect`

```jsx
useEffect(() => {
	if ( formState.isSubmitSuccessful) {
		reset();
	}
}, [formState, reset]) // formState will be changed
```

- Another case when you want to reset partial default values, you can use `getValues` API: `reset({...getValues(), key: value})`

## `useWatch`

To improve performance, you can use `useWatch` for individual input

```jsx	
function Child({ control }) {
  const firstName = useWatch({
    control,
    name: "firstName",
  });

  return <p>Watch: {firstName}</p>;
}

function App() {
  const { register, control } = useForm({
    firstName: "test"
  });
  
  return (
    <form>
      <input {...register("firstName")} />
      <Child control={control} />
    </form>
  );
}
```


## `setValue`

Set value directly to the field

## `trigger`

`trigger()` will validate all inputs. It returns a promise. We can use trigger then call API to submit form manually.

## custom `onChange`

You have to pass the `onChange` of the field (if you are using `Controller`)

```jsx
<Controller
	name="voice"
	control={control}
	render={({ field }) => (
		<SelectField
		ref={field.ref}
		control={control}
		label="Voice"
		options={voices}
		onChange={(selectedOption) => {
			field.onChange(selectedOption); // You can also customise what value gets sent to hook form by transforming the value during onChange.
			handleVoiceChange(selectedOption);
		}}
		error={errors?.voice?.message}
		/>
	)}
/>
```

## Custom Input component

Custom input componenent needs to expose `ref`, so that form can get the value

```jsx	
const Checkbox = ({ register, ...rest }) => (
  <Form.Check type="checkbox" {...register} {...rest} />
);
<Checkbox register={register("checkbox")} value="A" />
<Checkbox register={register("checkbox")} value="B" />
<Checkbox register={register("checkbox")} value="C" />
```

## Using with `yup`

- Optional field validation in Yup schema: only validate when field has value

```js
contactPhone: yup.string().when('contactPhone', (val) => {
    if (val) {
    return yup.string().matches(PHONE_REGEX, PHONE_FORMAT_ERROR).max(MAX_PHONE_CHAR);
    } else {
    return yup.string();
    }
}),
```
- Custom error message based on other field value: using `creatError` từ `context`

```js
content: yup
  .string()
  .test({
    name: 'len',
    test: (val, context) => {
      const maxLength = getContentMaxCharacter(
        context.parent.includePreamble,
        context.parent.preamble
      );
      if (val.toString().length >= maxLength) {
        const invalidLength = val?.length + context.parent.preamble.length;
        return context.createError({
          message: `Currently Preamble and Content is ${invalidLength} characters. Maximum is 800 characters.`,
          path: context.path,
        });
      }
      return true;
    },
  })
```