<script setup lang="ts">
defineProps<{
  msg: string
}>()

import { GoogleGenerativeAI } from '@google/generative-ai'
import { ref } from 'vue'

const apiKey = 'AIzaSyAFUs9LfPfDNjYcAkLHbvKygBen0NO5Ynk' // colocar como env variable depois
const genAI = new GoogleGenerativeAI(apiKey)
const prompt = ref('')
const response = ref('')

async function generatePrompt() {
  const model = genAI.getGenerativeModel({
    model: 'gemini-1.5-flash',
    generationConfig: { responseMimeType: 'application/json' }
  })
  try {
    console.log(prompt.value)
    const result = await model.generateContent(
      prompt.value +
        ` using this JSON schema:
        { "type": "object",
          "properties": {
            "recipe_name": { "type": "string" },
            "ingredients": { "type": "array" },
            "instructions": { "type": "array" },
          }
        }`
    )
    const text = result.response.text()
    response.value = JSON.parse(text)
    console.log(JSON.parse(text))
  } catch (error) {
    console.error(error)
  }
}
</script>

<template>
  <div class="container">
    <input type="text" v-model="prompt" @keydown.enter="generatePrompt" />
    <!-- <p class="response">{{ response }}</p> -->
    <div class="response">
      <h2>{{ response.recipe_name }}</h2>
      <h3>Ingredients</h3>
      <ul class="ingredients">
        <li v-for="ingredient in response.ingredients">
          {{ ingredient }}
        </li>
      </ul>
      <h3>Instructions</h3>
      <ul class="instructions">
        <li v-for="instructions in response.instructions">
          {{ instructions }}
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
  padding: 1rem;
  width: 100%;
  min-height: 3.5rem;
  border-radius: var(--radius);
  border: 1px solid var(--black);
  transition: 0.3s;
  font-size: var(--text-h3);
}
input[type='text']:is(:active, :focus) {
  outline: var(--primary) 1px solid;
}

.response {
  padding: var(--m);
}

ul {
  padding-left: var(--sm);
}
li {
  list-style: disc;
}
</style>
