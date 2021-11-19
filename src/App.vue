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
  font-family: Roboto, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
