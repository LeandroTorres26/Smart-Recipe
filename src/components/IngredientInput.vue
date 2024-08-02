<script setup lang="ts">
import { ref } from 'vue'
import RecipeResponse from './RecipeResponse.vue'
import Button from './Button/Button.vue'

const prompt = ref('')

let ingredientList = ref<string[]>([])

function addIngredient() {
  if (prompt?.value) {
    ingredientList.value = [...ingredientList.value, prompt.value]
    prompt.value = ''
  }
}

function removeIngredient(index) {
  ingredientList.value.splice(index, 1)
}
</script>

<template>
  <div class="container">
    <label class="cta" for="ingredientPromp">Add ingredient</label>
    <div class="promptWrapper">
      <input id="ingredientPrompt" type="text" v-model="prompt" @keydown.enter="addIngredient" />
      <!-- <button @click="addIngredient">Add</button> -->
      <Button @click="addIngredient" text="Add" />
    </div>
    <ul class="ingredientList" v-if="ingredientList.length > 0">
      <li
        v-for="(ingredient, index) in ingredientList"
        :key="index"
        @click="removeIngredient(index)"
      >
        {{ ingredient }}
      </li>
    </ul>
  </div>
  <RecipeResponse :IngredientList="ingredientList" />
</template>

<style scoped>
.cta {
  text-align: center;
  padding-left: var(--m);
  text-transform: lowercase;
  font-size: 2rem;
}

.promptWrapper {
  position: relative;
}

input[type='text'] {
  padding: var(--s);
  width: 100%;
  /* min-height: 3.5rem; */
  border-radius: var(--round);
  /* border: 1px solid var(--black); */
  border: none;
  transition: 0.1s;
  font-size: var(--text-h3);
}
input[type='text']:is(:active, :focus) {
  outline: var(--primary) 2px solid;
}

button {
  position: absolute;
  right: 0;
  top: 0;
  bottom: 0;
}

.recipe {
  padding: var(--m);
}

.ingredientList {
  display: flex;
  flex-wrap: wrap;
  gap: 1rem;
  list-style: none;
  padding: var(--sm);
}

.ingredientList li {
  background-color: var(--primary);
  border-radius: var(--rounded-square);
  padding: var(--xxs) var(--s);
  font-size: var(--text-p);
  font-weight: bold;
  position: relative;
  cursor: pointer;
  transition: 0.7s;
  &:hover {
    /* background-color: red; */
    box-shadow: inset 0 0 0 2em red;
  }
}
</style>
