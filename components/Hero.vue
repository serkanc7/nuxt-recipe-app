<template>
  <div class="hero">
    <img class="hero__img" src="@/assets/imgs/hero.jpg" alt="" />
    <div class="hero__content container">
      <h1 class="hero__title">Recipe App</h1>
      <a href="#recipes" class="hero__button button">View Recipes</a>
    </div>
  </div>
</template>

<script>
export default {}
</script>

<style lang="scss" scoped>
.hero {
  height: 400px;
  position: relative;

  &::after {
    content: '';
    position: absolute;
    top: 0;
    left: 0;
    height: 100%;
    width: 100%;
    background-color: rgba(0, 0, 0, 0.6);
  }

  &__img {
    width: 100%;
    height: 100%;
    object-fit: cover;
  }

  &__content {
    position: absolute;
    top: 0;
    z-index: 1;
    width: 100%;
    height: 100%;
    display: flex;
    flex-direction: column;
    justify-content: center;
  }

  &__title {
    color: white;
    font-size: 40px;
  }

  &__button {
    align-self: flex-start;
    background: #911f27;
    color: white;
  }
}
</style>
