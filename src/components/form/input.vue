<script setup lang="ts">
import { shallowReactive, watch } from "vue"
const { label } = defineProps<{ label?: string }>()

const value = defineModel<string>()

const state = shallowReactive({
  hasValue: false,
  hasHover: false,
  hasFocus: false,
})

watch(value, (newValue) => state.hasValue = !!newValue)
</script>

<template>
  <div class="providor" :class="state">
    <label class="label" v-if="label" v-html="label"></label>

    <input class="input" type="text"
      v-model="value"
      @mouseenter="() => state.hasHover = true"
      @mouseleave="() => state.hasHover = false"
      @focus="() => state.hasFocus = true"
      @blur="() => state.hasFocus = false"
    >
  </div>
</template>

<style scoped>
.providor {
  position: relative;
  display: inline-block;
  font-size: 1.6rem;
  background-color: #1a1a1a;
  border: 1px solid transparent;
  border-radius: 0.5em;
  transition: border 150ms ease, outline 150ms ease;
}
.providor.hasHover { border-color: #646cff; }
.providor.hasFocus { border-color: #646cff; outline: 3px solid rgba(100, 108, 255, 0.3); }

.input {
  position: relative;
  z-index: 1;
  display: block;
  font-size: inherit;
  padding: 1.1em 1.2em 0.9em;
  background-color: transparent; 
  border: none;
  outline: none;
}

.label {
  position: absolute;
  transform-origin: left top;
  z-index: 0;
  top: 0.8em;
  left: 1em;
  color: inherit;
  transition: top 150ms ease, color 150ms ease, transform 150ms ease;
  color: rgba(248, 248, 248, 0.6);
}

.providor.hasValue .label,
.providor.hasFocus .label {
  top: 0.3em;
  color: #646cff;
  transform: scale(0.8);
}

.label + .input { padding: 1.6em 1em 0.4em; }
</style>
