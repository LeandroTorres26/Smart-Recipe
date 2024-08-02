<script setup lang="ts">
import { GoogleGenerativeAI } from '@google/generative-ai'
import { ref } from 'vue'

const props = defineProps({
  IngredientList: {
    type: Array,
    default: () => []
  }
})

interface Recipe {
  name: string
  ingredients: string[]
  instructions: string[]
}
const apiKey = import.meta.env.VITE_API_KEY
const genAI = new GoogleGenerativeAI(apiKey)

const recipe = ref<Recipe>({
  name: '',
  ingredients: [],
  instructions: []
})

let loaded = false
async function generatePrompt() {
  const model = genAI.getGenerativeModel({
    model: 'gemini-1.5-flash',
    generationConfig: { responseMimeType: 'application/json' }
  })
  try {
    const result = await model.generateContent(
      `write the recipe on the language of the prompt. I have these ingredients available:` +
        props?.IngredientList?.join(', ') +
        `, suggest a recipe using this JSON schema:
        { "type": "object",
          "properties": {
            "name": { "type": "string" },
            "ingredients": { "type": "array" },
            "instructions": { "type": "array" },
          }
        }`
    )
    const text = result.response.text()
    recipe.value = JSON.parse(text)
    loaded = true
  } catch (error) {
    console.error(error)
  }
}
</script>

<template>
  <div class="container">
    <button @click="generatePrompt" v-if="props.IngredientList?.length > 0">Generate</button>
    <div class="recipe">
      <h2>{{ recipe.name }}</h2>
      <h3 v-if="loaded">Ingredients</h3>
      <ul class="ingredients">
        <li v-for="(ingredient, index) in recipe.ingredients" :key="index">
          {{ ingredient }}
        </li>
      </ul>
      <h3 v-if="loaded">Instructions</h3>
      <ul class="instructions">
        <li v-for="(instruction, index) in recipe.instructions" :key="index">
          {{ instruction }}
        </li>
      </ul>
    </div>
  </div>
</template>

<style scoped>
button {
  display: block;
  padding: var(--m) var(--l);
  margin: 0 var(--m) 0 auto;
  border: none;
  border-radius: var(--round);
  background-color: var(--primary);
  cursor: pointer;
  color: var(--white);
  font-size: var(--text-h3);
  font-weight: bold;
}

.recipe {
  padding: var(--m);
}

ul {
  padding-left: var(--m);
}
li {
  list-style: disc;
}
</style>
