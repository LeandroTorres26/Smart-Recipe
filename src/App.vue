<script setup lang="ts">
import IngredientInput from '@/components/IngredientInput.vue'
</script>

<template>
  <main>
    <IngredientInput />
  </main>
</template>

<style>
main {
  display: grid;
  grid-template-rows: 1fr auto;
  place-items: center;
  @media (min-width: 1024px) {
    grid-template-columns: 1fr auto;
  }
}

.container {
  width: clamp(290px, 50vw, 700px);
  margin: var(--m);
}

.v-enter-active,
.v-leave-active {
  transition: opacity 1s ease;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
}
</style>
