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
  sectiontitle1: string
  ingredients: string[]
  sectiontitle2: string
  instructions: string[]
}
const apiKey = import.meta.env.VITE_API_KEY
const genAI = new GoogleGenerativeAI(apiKey)

const recipe = ref<Recipe>({
  name: '',
  sectiontitle1: '',
  ingredients: [],
  sectiontitle2: '',
  instructions: []
})

const loaded = ref(false)
async function generatePrompt() {
  const model = genAI.getGenerativeModel({
    model: 'gemini-1.5-flash',
    generationConfig: { responseMimeType: 'application/json' }
  })
  try {
    const result = await model.generateContent(
      `write the recipe on the language of the prompt (do not mix languages). I have these ingredients available:` +
        props?.IngredientList?.join(', ') +
        `, suggest a recipe using this JSON schema:
        { "type": "object",
          "properties": {
            "name": { "type": "string" },
            "sectiontitle1": { "type": "string" },
            "ingredients": { "type": "array" },
            "sectiontitle2": { "type": "string" },
            "instructions": { "type": "array" },
            }
        }`
    )
    const text = result.response.text()
    recipe.value = JSON.parse(text)
    loaded.value = true
  } catch (error) {
    console.error(error)
  }
}

defineExpose({
  generatePrompt
})
</script>

<template>
  <Transition>
    <div class="container" v-if="loaded" :class="{ loaded: loaded }">
      <div class="recipe">
        <h2>{{ recipe.name }}</h2>
        <h3 v-if="loaded">{{ recipe.sectiontitle1 }}</h3>
        <ul class="ingredients">
          <li v-for="(ingredient, index) in recipe.ingredients" :key="index">
            {{ ingredient }}
          </li>
        </ul>
        <h3 v-if="loaded">{{ recipe.sectiontitle2 }}</h3>
        <ul class="instructions">
          <li v-for="(instruction, index) in recipe.instructions" :key="index">
            {{ instruction }}
          </li>
        </ul>
      </div>
    </div>
  </Transition>
</template>

<style scoped>
.recipe {
  padding: var(--m);
}

ul {
  padding-left: var(--m);
}
li {
  list-style: disc;
}

.v-enter-active,
.v-leave-active {
  transition: 1s ease-in-out;
}

.v-enter-from,
.v-leave-to {
  opacity: 0;
  @media (min-width: 1024px) {
    width: 0;
  }
}
</style>
