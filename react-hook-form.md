# React Hook Form

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

## `watch`

It replace the use of `useState`. RHF will watch value of all register fields. However, when using watch, all the fields will become uncontrolled input.

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
			field.onChange(selectedOption);
			handleVoiceChange(selectedOption);
		}}
		error={errors?.voice?.message}
		/>
	)}
/>
```