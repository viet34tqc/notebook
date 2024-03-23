# Proxy

Proxy help us interact with a object via another object, which is the `Proxy` object.

Use cases: 

When working with objects in JavaScript, it's common to set and get properties. However, if you access the property directly using square brackets or dot notation, there's no way to prevent users from passing an invalid value.

To solve this issue, we can implement additional logic or restrictions when setting or getting a property with `Js Proxy`

## Key terms

- Target: the target is what stands behind the proxy. This can be any object
- Handler: An object that contains the logic of all the proxy’s traps
- Trap: a method on the handler object that intercepts the proxy's default behavior for a particular operation. There are default traps, such as `get`, `set`, `apply`, and `construct`

The `get` trap is called when a property of the target object is accessed using dot notation or square brackets. Here is the default:

```ts
const handler = {
    get: function(target, prop) {
        return target[prop];
    },
};
```

Similarly, the `set` trap is called when a property of the target object is set using dot notation or square brackets. Here is the default:

```ts
const handler = {
    set: function(target, prop, value) {
        target[prop] = value;
    },
};
```

## Customize Proxy `traps`

In order to fulfill our requirements, such as validation, we have to customize the default traps (`set` and `get`). For instance, here we are using Proxy to enforce the value of an object must not be exceed 5 characters

```ts
const productDesc = {
  desc: '',
};

const handler = {
  set: (
    target: typeof productDesc,
    key: keyof typeof productDesc,
    value: string
  ) => {
    if (value.length > 5) {
      value = value.substring(0, 4);
      target[key] = value;
      return true;
    }
    return false;
  },
};

const proxy = new Proxy(productDesc, handler);
proxy.desc = '12312321';
console.log(productDesc);
```


