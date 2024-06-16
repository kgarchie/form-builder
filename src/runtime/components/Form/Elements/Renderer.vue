<script setup lang="ts">
const props = defineProps<{
  data: FormElementData
  edit: boolean | Ref<boolean>
}>()
const emit = defineEmits<{
  delete: [number]
}>()

const _edit = computed(() => {
  if (typeof props.edit === 'boolean') return props.edit
  return props.edit.value
})
</script>
<template>
  <div class="container relative w-full grid place-items-center" :draggable="_edit" data-parent="canvas"
    :class="{ 'hover-active': _edit }">
    <div class="cursor-pointer absolute w-[81%] flex justify-end pointer-events-none" v-if="_edit">
      <svg width="30px" height="30px" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"
        @click="emit('delete', data.index as number)" class="mt-4 pointer-events-auto">
        <path fill-rule="evenodd" clip-rule="evenodd"
          d="M12 22C7.28595 22 4.92893 22 3.46447 20.5355C2 19.0711 2 16.714 2 12C2 7.28595 2 4.92893 3.46447 3.46447C4.92893 2 7.28595 2 12 2C16.714 2 19.0711 2 20.5355 3.46447C22 4.92893 22 7.28595 22 12C22 16.714 22 19.0711 20.5355 20.5355C19.0711 22 16.714 22 12 22ZM8.96965 8.96967C9.26254 8.67678 9.73742 8.67678 10.0303 8.96967L12 10.9394L13.9696 8.96969C14.2625 8.6768 14.7374 8.6768 15.0303 8.96969C15.3232 9.26258 15.3232 9.73746 15.0303 10.0303L13.0606 12L15.0303 13.9697C15.3232 14.2625 15.3232 14.7374 15.0303 15.0303C14.7374 15.3232 14.2625 15.3232 13.9696 15.0303L12 13.0607L10.0303 15.0303C9.73744 15.3232 9.26256 15.3232 8.96967 15.0303C8.67678 14.7374 8.67678 14.2626 8.96967 13.9697L10.9393 12L8.96965 10.0303C8.67676 9.73744 8.67676 9.26256 8.96965 8.96967Z"
          fill="#d22020" />
      </svg>
    </div>

    <FormElementsStatic v-if="isStatic(data.type)" :data="data as StaticElementData" />
    <FormElementsInput v-else-if="isInput(data.type)" :data="data as InputElementData" :edit="_edit" />
    <FormElementsButton v-else-if="isButton(data.type)" :data="data as ButtonElementData" :edit="_edit" />

    <div v-else>
      <p class="text-red">Unknown element type {{ data.type }}</p>
    </div>
  </div>
</template>
<style scoped lang="scss">
.container {
  display: flex;
  flex-direction: column;
  padding: 0.4rem 0;
  padding-bottom: 1.85rem;
}

.hover-active:hover {
  background-color: #f0f0f0;
  cursor: grab;
  box-shadow: 0 0 0 1px #f0f0f0;
}
</style>