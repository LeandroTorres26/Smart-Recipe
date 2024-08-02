<script setup lang="ts">
import IngredientInput from './components/IngredientInput.vue'
</script>

<template>
  <!-- <header>header</header> -->

  <main>
    <IngredientInput />
  </main>
</template>

<style>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}

.container {
  width: clamp(300px, 50vw, 700px);
  margin: var(--m);
}
</style>
