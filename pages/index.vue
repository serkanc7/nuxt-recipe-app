<template>
  <div class="home">
    <Hero />
    <div class="search container">
      <input
        type="text"
        class="search__input"
        :class="{ '-searched': searchInput }"
        placeholder="Search Recipe"
        v-model.lazy="searchInput"
        @keyup.enter="$fetch"
      />
      <button class="search__button" @click="clearSearch" v-if="searchInput">
        Clear Search
      </button>
    </div>

    <Loading v-if="$fetchState.pending" />

    <div class="container" v-else>
      <div class="recipes" v-if="searchInput === ''">
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
      <div class="recipes" v-else>
        <div class="recipes__grid" id="recipes">
          <div
            class="recipes__recipe"
            v-for="recipe in searchedRecipes"
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
  </div>
</template>

<script>
import axios from 'axios'
import Loading from '../components/Loading.vue'
export default {
  components: { Loading },
  data() {
    return {
      recipes: [],
      searchInput: '',
      searchedRecipes: [],
    }
  },
  async fetch() {
    if (this.searchInput === '') {
      await this.getRecipes()
      return
    }
    if (this.searchInput !== '') {
      await this.searchRecipes()
    }
  },
  fetchDelay: 1000,
  methods: {
    async getRecipes() {
      const data = axios.get(
        'https://www.themealdb.com/api/json/v1/1/filter.php?c=Beef'
      )
      const result = await data
      result.data.meals.forEach((recipe) => this.recipes.push(recipe))
    },

    async searchRecipes() {
      const data = axios.get(
        `https://www.themealdb.com/api/json/v1/1/search.php?s=${this.searchInput}`
      )
      const result = await data
      result.data.meals.forEach((recipe) => this.searchedRecipes.push(recipe))
      console.log(this.searchedRecipes)
    },
    clearSearch() {
      this.searchInput = ''
      this.searchedRecipes = []
    },
  },
}
</script>

<style lang="scss">
@import '@/assets/scss/mixin/media-query.scss';
.home {
  width: 100%;
}

.search {
  display: flex;
  flex-direction: row;

  margin-top: 20px;
  &__input {
    padding: 10px;
    border-radius: 5px;
    border: 2px solid #911f27;
    outline: none;

    &.-searched {
      border-right: none;
      border-top-right-radius: 0;
      border-bottom-right-radius: 0;
    }
  }
  &__button {
    cursor: pointer;
    border: 2px solid #911f27;
    border-left: none;
    background: #fcf0c8;
    color: #911f27;
    font-weight: bold;
    border-top-right-radius: 5px;
    border-bottom-right-radius: 5px;
  }
}

.recipes {
  padding-top: 32px;
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
    box-shadow: 0px -1px 17px 0px #fdc05070;
    padding: 20px;
    display: flex;
    flex-direction: column;
    border: 1px solid #fdc150;
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
