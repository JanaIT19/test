<template>
  <div id="app">
    <ProductCard />
  </div>
</template>

<script>
import ProductCard from './components/product/ProductCard.vue'

export default {
  name: 'App',
  components: {
    ProductCard
  }
}
</script>

<style lang="scss">

#app {
  font-family: Roboto;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

  @font-face {
  font-family: "Roboto";
  font-weight: 400;
  font-style: normal;
  font-display: auto;
  unicode-range: U+000-5FF;
  src: local("Roboto"), url("/fonts/Roboto/Roboto-Regular.ttf") format("ttf");
}
</style>
