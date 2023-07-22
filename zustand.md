# Zustand

## References

- <https://tkdodo.eu/blog/working-with-zustand>
- <https://refine.dev/blog/zustand-react-state/#getting-started-with-zustand>

## Why not context

<https://formidable.com/blog/2021/stores-no-context-api/>

## Best practice

- Only export selector instead of exporting entire store
- Atomic selectors: each selector for each state.
- Using `shallow` if your selector returns an object or array because object and array is not the same, even if the content is the same (<https://github.com/pmndrs/zustand/tree/2b29d736841dc7b3fd7dec8cbfea50fee7295974#selecting-multiple-state-slices>)
- Separate Actions from State: create an `actions` object
- Have multiple stores. Each store can be responsible for a single piece of state. You don't need to seperate it into slices like Redux Toolkits. Having multiple store is simpler than using slices.