## `ref()`vs `reactive()` vs `shallowRef`
- `ref()`
	- Wraps primitive (`number,string,boolean,object`)
	- access, mutate inner value via `.value`
	- ideal for single state
- `reactive()`
	- takes an object(`arrays,nested structure`)  and makes all props reactive
	- `deeply reactive`access, mutate props without `.value` needed.
	- unwraps all props and makes all nested fields reactive.
- `shallowRef()`
	- only tracks reactivity at the `top level` — changes to nested properties won't trigger re-renders
	-  not deeply `reactive` only `.value`
	- used for (`avoid deep reactivity cost`)`performance optimizations`of large data structures
- `computed` → returns a `value`
- `watch` → runs a callback on change, for side effect
# `watchEffect` 
- `Vue` Composition `Api` function automatically tracks any reactive data
# `LifeCycle`
- `onUpdated`: This hook is called after any `DOM update of the component`
-  `watchEffect`: called after any reactive data changed

