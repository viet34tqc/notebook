# React Form Module

4 components

- Page/Container: Each page can have one form or multiple forms
- Form Component:
  - Handle form validation and trigger callback on submit
  - Form Component shouldn't handle any app logic on submit. Its only mission is to report to the Container when the user submit. By doing this way, we can reuse this Form Component
  - React Hook Form with Yup, Formmik
- Form Fields:
  - A bridge between Form Component and UI control
  - Bind value from Form Component into UI Control
  - Eg: InputFields, PhotoField, MapPickerField
- UI Control
  - Dump components which receive data from Form Field and render it accordingly.
  - Material-UI, Bootstrap