<script setup lang="ts">
import { GoogleGenerativeAI } from '@google/generative-ai'
import { ref } from 'vue'

interface Recipe {
  name: string
  ingredients: string[]
  instructions: string[]
}
const apiKey = import.meta.env.VITE_API_KEY
const genAI = new GoogleGenerativeAI(apiKey)
const prompt = ref('')
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
      prompt.value +
        ` using this JSON schema:
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
    <input type="text" v-model="prompt" @keydown.enter="generatePrompt" />
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
.container {
  min-width: clamp(300px, 50vw, 1000px);
}
input[type='text'] {
  padding: var(--s);
  width: 100%;
  /* min-height: 3.5rem; */
  border-radius: var(--radius);
  border: 1px solid var(--black);
  transition: 0.3s;
  font-size: var(--text-h3);
}
input[type='text']:is(:active, :focus) {
  outline: var(--primary) 1px solid;
}

.recipe {
  padding: var(--m);
}

ul {
  padding-left: var(--sm);
}
li {
  list-style: disc;
}
</style>
