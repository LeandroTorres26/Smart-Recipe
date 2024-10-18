<script setup lang="ts">
import { ref } from 'vue'
import RecipeResponse from '@/components/RecipeResponse.vue'
import Button from '@/components/Button/Button.vue'

const prompt = ref('')

let ingredientList = ref<string[]>([])
const RecipeResponseRef = ref()

function addIngredient() {
  if (prompt?.value) {
    ingredientList.value = [...ingredientList.value, prompt.value]
    prompt.value = ''
  }
}

function removeIngredient(index: number) {
  ingredientList.value.splice(index, 1)
}

const callPromptGenerate = () => {
  console.log(RecipeResponse)
  RecipeResponseRef.value.generatePrompt()
}
</script>

<template>
  <div class="container">
    <label class="cta" for="ingredientPromp">Add ingredient</label>
    <div class="promptWrapper">
      <input id="ingredientPrompt" type="text" v-model="prompt" @keydown.enter="addIngredient" />
      <Button @click="addIngredient" text="Add" width="clamp(1rem, 20%, 5rem)" height="auto" />
    </div>
    <Transition>
      <ul class="ingredientList" v-if="ingredientList.length > 0">
        <li
          v-for="(ingredient, index) in ingredientList"
          :key="index"
          @click="removeIngredient(index)"
        >
          {{ ingredient }}
        </li>
      </ul>
    </Transition>
    <Button
      @click="callPromptGenerate"
      text="Generate"
      height="auto"
      padding="var(--l) var(--xl)"
      margin="2rem auto 0"
    />
  </div>
  <RecipeResponse :IngredientList="ingredientList" ref="RecipeResponseRef" />
</template>

<style scoped>
.cta {
  display: inline-block;
  width: 100%;
  padding-block: 2rem;
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
  border-radius: var(--round);
  border: none;
  transition: 0.1s;
  font-size: var(--text-h3);
  @media (min-width: 1024px) {
    min-height: 3.5rem;
  }
}
input[type='text']:is(:active, :focus) {
  outline: var(--primary) 2px solid;
}

.promptWrapper button {
  position: absolute;
  right: 0;
  top: 0;
  bottom: 0;
  border-radius: 0 var(--round) var(--round) 0;
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
  animation: fadeIn 0.5s ease-in-out;
  &:hover {
    /* background-color: red; */
    box-shadow: inset 0 0 0 2em red;
  }
}

@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}
</style>
