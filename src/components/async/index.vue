<script setup lang="ts" generic="T extends Promise<Awaited<T>>">
import { defineProps, shallowReactive, shallowRef } from "vue"
const { promise } = defineProps<{ promise: T }>()

const is = shallowReactive({
  pending: true,
  fulfilled: false,
  rejected: false,
})

const awaited = shallowRef<Awaited<typeof promise>>()
const throwed = shallowRef<Error>()

promise
  .then(v => (is.fulfilled = true, awaited.value = v))
  .catch(e => (is.rejected = true, throwed.value = e))
  .finally(() => is.pending = false)
</script>

<template>
  <slot name="await" v-if="is.pending"></slot>
  <slot name="then" v-else-if="is.fulfilled" v-bind="awaited!"></slot>
  <slot name="catch" v-else-if="is.rejected" v-bind="throwed!"></slot>
</template>
