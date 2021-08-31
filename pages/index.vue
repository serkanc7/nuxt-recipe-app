<template>
  <div class="home">
    <Hero />
    <div class="recipes">
      <div class="recipes__grid" id="recipes">
        <div
          class="recipes__recipe"
          v-for="recipe in recipes"
          :key="recipe.idMeal"
        >
          <img
            class="recipes__img"
            :src="recipe.strMealThumb"
            :alt="recipe.strMeal"
          />

          <div class="recipes__name">
            {{ recipe.strMeal.slice(0, 25)
            }}<span v-if="recipe.strMeal.length > 25">...</span>
          </div>
          <NuxtLink
            class="recipes__button button"
            :to="{
              name: 'recipes-recipeid',
              params: { recipeid: recipe.idMeal },
            }"
            >Get Recipe</NuxtLink
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'
export default {
  data() {
    return {
      recipes: [],
    }
  },
  async fetch() {
    await this.getRecipes()
  },
  methods: {
    async getRecipes() {
      const data = axios.get(
        'https://www.themealdb.com/api/json/v1/1/filter.php?c=Beef'
      )
      const result = await data
      result.data.meals.forEach((recipe) => this.recipes.push(recipe))
      console.log(result.data)
    },
  },
}
</script>

<style lang="scss">
@import '@/assets/scss/mixin/media-query.scss';
.home {
  width: 100%;
}

.recipes {
  padding: 32px 16px;
  &__grid {
    display: grid;
    column-gap: 32px;
    row-gap: 32px;
    grid-template-columns: repeat(4, 1fr);

    @include mq(desktop, max) {
      grid-template-columns: repeat(3, 1fr);
      column-gap: 20px;
      row-gap: 20px;
    }

    @include mq(tablet, max) {
      grid-template-columns: repeat(2, 1fr);
    }
    @include mq(mobile, max) {
      grid-template-columns: repeat(1, 1fr);
    }
  }

  &__recipe {
    padding: 20px;
    display: flex;
    flex-direction: column;
    border: 1px solid #911f27;
    border-radius: 10px;
    @include mq(desktop, max) {
      padding: 15px;
    }
  }

  &__img {
    width: 100%;
    height: 100%;
    border-radius: 10px;
  }

  &__name {
    margin-top: 5px;
    margin-bottom: 5px;
    font-size: 18px;
    font-weight: bold;
    @include mq(desktop, max) {
      font-size: 16px;
    }
  }

  &__button {
    border: 2px solid #630a10;
    align-self: flex-start;
    color: #630a10;
    font-weight: 600;
  }
}
</style>
