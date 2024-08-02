<script setup lang="ts">
const props = defineProps({
  text: String
})
</script>

<template>
  <button>{{ text }}</button>
</template>

<style scoped>
button {
  border: none;
  border-radius: 0 var(--round) var(--round) 0;
  width: clamp(1rem, 20%, 10rem);
  background-color: var(--primary);
  color: var(--white);
  font-weight: bold;
  text-transform: uppercase;
  font-size: 1rem;
  cursor: pointer;
}
</style>
