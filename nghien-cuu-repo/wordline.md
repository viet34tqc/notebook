# Wordline

## Sử dụng react-hook-form và Yup

- Sử dụng `watch` để lấy giá trị của field trong event handler. `watch` sẽ thay thế cho việc `setState` cho từng field 1
- Sử dụng `trigger` thay cho submit. Dùng khi tương tác với nhiều button, mỗi button sẽ có 1 event handler gọi API riêng. Lưu ý là `trigger` này sẽ không focus vào error input nên mình sẽ phải viết `trigger(undefined, { shouldFocus: true })`
- Optional field validation in Yup schema: chỉ validate field khi field có giá trị

```js
contactPhone: yup.string().when('contactPhone', (val) => {
    if (val) {
    return yup.string().matches(PHONE_REGEX, PHONE_FORMAT_ERROR).max(MAX_PHONE_CHAR);
    } else {
    return yup.string();
    }
}),
```
- Custom error message sử dụng giá trị động: dùng `creatError` từ `context`

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

## MaxLength cho input number

Mặc định input number sẽ không hiểu được thuộc tính maxLength như input text. Ngoài ra, input number còn cho phép điền kí tự +, -, e, E. Việc cần làm là chặn các kí tự này ngay khi gõ vào và cắt chiều dài của giá trị input


```js
const blockInvalidCharInputNumber = (e) =>
  ['e', 'E', '+', '-'].includes(e.key) && e.preventDefault();
```

```jsx
 <InputField
    type="number"
    min={0}
    label="UIC code"
    registration={register('uicCode')}
    error={errors?.uicCode?.message}
    onKeyDown={blockInvalidCharInputNumber}
    onChange={(e) => (e.target.value = e.target.value.slice(0, MAX_UIC_CHAR))}
/>
```