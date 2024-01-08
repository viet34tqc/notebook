# Sonner

## Render

Sonner uses observer pattern to manage the toasts

First, when `<Toaster />` component renders, it will run its `useEffect`

`index.tsx`

```tsx
React.useEffect(() => {
    return ToastState.subscribe((toast) => {...
```

`ToastState.subscribe` will subscribe a function and it also returns another function which is the cleanup function for the `useEffect` above. By writting code like this, we won't need another unsubscribe function

```tsx
subscribe = (subscriber: (toast: ToastT | ToastToDismiss) => void) => {
    this.subscribers.push(subscriber);

    return () => {
      const index = this.subscribers.indexOf(subscriber);
      this.subscribers.splice(index, 1);
    };
  };
```

We will only have one subscriber defined in the `useEffect`

## Add toast

Sonner use a normal function to addToast named `toastFunction` and pass it to another function named `toast` along with other method like `success`, `error`... It doesn't use a normal object but a function, so we can call `toast` function and its method like `toast.success`

- For normal toast: call `toast()` => call `addToast()`
- For other types of toast: call `toast.success()` => call `create()` => call `addToast()`

`created` will have a check if the toast is existing, then we can update it with the new data and also update `this.toasts`

```js
if (alreadyExists) {
  this.toasts = this.toasts.map((toast) => {
    if (toast.id === id) {
      this.publish({ ...toast, ...data, id, title: message });
```

If the toast is brand new, just call `addToast` which call `publish`

`publish` will trigger the subscriber to update/create the toast in UI. The logic for updating/creating lie in the `useEffect` above in `index.tsx`

## Remove toaster timer