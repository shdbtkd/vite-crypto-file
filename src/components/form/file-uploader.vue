<script setup lang="ts">
import { shallowReactive } from "vue"

const { label, multiple } = defineProps<{ label: string, multiple?: boolean }>()

const file = defineModel<FileList | never[]>()
function updateFile(e: Event) {
  file.value = (e.currentTarget as HTMLInputElement).files || []
}

const state = shallowReactive({
  dragged: false
})
</script>

<template>
  <div :class="state">
    <label for="Input">{{ label }}</label>
    <input id="Input" type="file" :multiple="multiple"
      @change="updateFile"
      @dragenter="() => state.dragged = true"
      @dragleave="() => state.dragged = false"
    >
  </div>
</template>
