- `PropWithEmit`
   - Child
     ```vue
     <script setup lang="ts">
     const props = defineProps<{ input: string }>();
     const emit = defineEmits<{(e: "update:input", value: string): void}>();
     const inputModel = computed({
       get() {
         return props.input;
       },
       set(newInputValue) {
         emit("update:input", newInputValue);
       },
     });
     </script>
     <template>
       <input v-model="inputModel" />
     </template>
     ```
   - Parent
     ```vue
     <script setup>
        const input = ref('parent-ref')
     </script>
     <template>
         <Child :input="input" @update:input="$event=>(input=event)"/>
     </template>
     ```
- `defineModel`
   - Child
	   ```vue
	    <script setup lang="ts">
		const input = defineModel("input")
		</script>
		<template>
		<input v-model="input" />
		</template>
			```
   - Parent
	  ```vue
	   <script setup>
	     const input = ref('parent-ref')
	   </script>
	   <template>
	    <Child v-model:input="input"/>
	   </template>
	  ```
# Slots
```vue
<script setup lang="ts">
  defineSlots<{
    default(props: { label: string }): void,
    content(props: { contentLabel: string }): void
  }>()
    const label = "default label from setup slot"
</script>
<template>
  <div>
    <slot name="default" :label="label"/>
    <slot name="content" :content-label="'content-label'"/>
  </div>
</template>
```

# Share Properties From Child 
### Child

```vue

<template>
  <input ref="input"/>
</template>
<script setup lang="ts">
  import {defineExpose, useTemplateRef} from "@vue/runtime-core";

  const input = useTemplateRef("input");
  const canFocus = ref(false)

  function focus() {
    input.value.focus()
  }

  defineExpose({
    focus,
    canFocus
  })
</script>
```
### Parent

```vue
<template>
  <Child ref="childInput"/>
</template>
<script setup lang="ts">
  const childInput = ref("childInput");

  function focusFromParent() {
    childInput.value.focus()
  }
</script>
```
