<script setup lang="ts">
const props = defineProps({
  text: String,
  width: {
    type: String,
    default: false
  },
  height: {
    type: String,
    default: 'auto'
  },
  padding: {
    type: String,
    default: false
  },
  margin: {
    type: String,
    default: false
  }
})
</script>

<template>
  <button :style="{ width: width, height: height, padding: padding, margin: margin }">
    {{ text }}
  </button>
</template>

<style scoped>
button {
  display: block;
  border: none;
  border-radius: var(--round);
  /* width: clamp(1rem, 20%, 10rem); */
  background-color: var(--primary);
  color: var(--white);
  font-weight: bold;
  text-transform: uppercase;
  font-size: 1rem;
  cursor: pointer;
}
</style>
