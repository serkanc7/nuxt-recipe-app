<template>
  <div>
    <Loading v-if="$fetchState.pending" />

    <div class="recipe container" v-else>
      <NuxtLink class="button recipe__button" :to="{ name: 'index' }"
        >Back</NuxtLink
      >
      <div class="recipe__content">
        <h1>{{ recipe.strMeal }}</h1>
        <img class="recipe__img" :src="recipe.strMealThumb" alt="" />
        <p class="recipe__text">{{ recipe.strInstructions }}</p>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  name: 'recipe',
  data() {
    return {
      recipe: null,
    }
  },

  async fetch() {
    await this.getRecipe()
  },
  fetchDelay: 1000,
  methods: {
    async getRecipe() {
      const data = axios.get(
        `https://www.themealdb.com/api/json/v1/1/lookup.php?i=${this.$route.params.recipeid}`
      )
      const result = await data
      this.recipe = result.data.meals[0]
    },
  },
}
</script>

<style lang="scss">
.recipe {
  &__button {
    background: #911f27;
    color: white;
    margin-top: 50px;
  }
  &__content {
    display: flex;
    flex-direction: column;
    align-items: center;
  }
  &__img {
    width: 100%;
    max-width: 500px;
  }
}
</style>
