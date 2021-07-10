Why react use immutability?
- Avoiding direct data mutation lets us keep previous versions of the state, and reuse them later
- Create pure componens. Immutable data can determine if changes have been made, which helps to determine when a component require re-rerendering.
If you mutate the data directly, the components might not re-render that could lead to bugs.