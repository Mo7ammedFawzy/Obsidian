## `ref()`vs `reactive()` vs `shallowRef`
- `ref()`
	- Wraps primitive (`number,string,boolean`)
	- access, mutate inner value via `.value`
	- ideal for single state
- `reactive()`
	- takes an object(`arrays,nested structure`)  and makes all props reactive
	- access, mutate props without `.value` needed.
- `shallowRef()`
	-  not deeply `reactive` only `.value` access is reactive
	- used for `performance optimizations`of large data structures
- `computed` → returns a `value`
- `watch` → runs a callback on change, for side effect
# `watchEffect` 
- `Vue` Composition `Api` function automatically tracks any reactive data
# `LifeCycle`
- `onUpdated`: This hook is called after any `DOM update of the component`
-  `watchEffect`: called after any reactive data changed

