# Proxy

Proxy help us interact with a object via another object, which is the Proxy object.

## Key terms

- Target: The real deal that the proxy is substituting, the target is what stands behind the proxy. This can be any object
- Handler: An object that contains the logic of all the proxy’s traps
- Trap: Similar to traps in operating systems, traps in this context are methods that provide access to the object in a certain way

**Example**

Here we are using Proxy to enforce the value of an object must not be exceed 5 characters

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
