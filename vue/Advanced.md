# Watchers
- one && multiple
	```ts
	const isAuthenticated = ref<boolean>(false); //single reactive value
	const hasClicked = ref<boolean>();
	const user = reactive({ // multiple reactive value within object
	    name: undefined,
	    additionalInfo: {
	        hasPlaceOrder: false
	    }
	})
	// only specefic data
	watch(isAuthenticated, (isAuthenticatedValue) => {
	})
	// additional you can watch multiple refs
	watch([isAuthenticated, hasClicked], ([isAuthenticatedValue, hasClickedValue]) => {
	})
	/**
	 * @deep      (option you can watch properties inside reactive object)
	 * @immediate (watchCallBack) execute when watch created;
	 */
	watch(user, (userNewValue) => {
	}, {deep: true, immediate: true})
	```
-  `WatchEffect`
	- `Vue` Composition `Api` function automatically tracks reactive data
	- `rerun` in any change
	- `watchOptions:{immediate:true,deep:true}` can be simplified by  `watchEffect`
- watch `toValue(target)` => `target` accept [`MaybeRefOrGetter`](Types#MaybeRefOrGetter) `type`
	```ts
	watch(()=>toValue(target))
	```
# Composable
-  `useFetch`
	```ts
	import { ref, toValue, watchEffect, type ComputedRef, watch } from "vue"
	export const useFetch = (URL: ComputedRef) => {
		const data = ref(null);
		const error = ref(null)
		watchEffect(async () => {
		const reactiveURL = toValue(URL) // ref computed => value
		try {
			fetch(reactiveURL).then(res => res.json()).then((json) => (data.value = json))``
		} catch (err: any) {
			error.value = err;
			}
		})
		return { data, error }
	}
	```
# Prevents unnecessary reactivity overhead

```vue
<script setup lang="ts">
import { ref, markRaw, Component } from "vue";
import MyComponent from "@/components/MyComponent.vue";

const dynamicComponent = ref<Component | null>(null);

const loadComponent = () => {
dynamicComponent.value = markRaw(MyComponent); // Prevent reactivity
};
</script>

<template>
  <button @click="loadComponent">Load Component</button>
  <component v-if="dynamicComponent" :is="dynamicComponent" />
</template>
```


# `EventBinding`
```vue
<template>
  <button v-on="isFetchingBtn ? {click:sendFetchAccess}:{}"/>
  <button v-on:['click']="sendFetchAccess"/>
  <button @[eventHandler]="sendFetchAccess"/>
</template>
```
# `defineComponent`

```ts
const quasarBtn = defineComponent((props) => () => h(QBtn, <QBtnProps>{
flat: true, size: "small"
}, "text"))
const endSessionActionComponent = defineComponent((props) => () => h("a", {
href: `test?closewssession=${props.params.value}`,
target: "_target"
}, "End Session"),
{
props: {
  params: {
	required: true,
	type: Object as PropType<ICellRendererParams>
  }
}
}
)
```


# Export && Import
```ts
export default function useMouse() {
}
export const useCounter = 0;
export { default as HomeView } from "./index.vue";
import useMouse, {useCounter} from "path_example";
```

# Style binding
```vue
<style>
.dynamic {
  background-color: v-bind(color)
}
</style>
```
# Remove un Necessary Attribute
```vue
<script setup lang="ts">
	const title = "title";
	const falsyValues = [null, undefined, false, NaN, 0, -0, 0n, "", document.all]
</script>
<template>
	<button v-bind:[title]="null"/>
	<button v-bind="{hasText:null}"/>
	<button v-bind:[title]="undefined"/>
</template>
```


## Styles
```vue
<style scoped>
/* Targets elements within child components*/
:deep(.button) {
background-color: tomato;
} 
/* Targets elements globally*/
:global(body) 
{ font-family: Inter, sans-serif;
} /* Targets elements within a components slot (normally controlled by the parent)*/ :slotted(div) {
color: red; 
} 
</style>
```