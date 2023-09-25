<template>
  <div class="app">
    <label> 
      <input type="checkbox" v-model="showAvailable" /> Show Only Available Products
    </label>
    <div class="brand-filters">
      <label v-for="brand in brands" :key="brand">
        <input type="checkbox" v-model="selectedBrands" :value="brand">
      {{ brand }}
    </label>
    </div>
    <p class="result-count">Remaining Shoes: {{ filteredProducts.length }}</p>
    <div class="product-grid">
      <ProductGridItem
        v-for="product in filteredProducts"
        :key="product.name"
        :product="product"
      />
    </div>
  </div>
</template>


<script>
import ProductGridItem from './components/ProductGridItem.vue';
import products from './data/products.json';

export default {
  name: 'App',
  components: {
    ProductGridItem,
  },
  data() {
    return {
      products,
      showAvailable: true,
      selectedBrands: [],
      brands: [...new Set(products.map(product => product.brand))], 
    };
  },
  computed: {
    filteredProducts() {
      if (this.showAvailable) {
        return this.products.filter(
          (product) => 
            product.isAvailable && (this.selectedBrands.length === 0 || this.selectedBrands.includes(product.brand))
        );
      } else {
        return this.products.filter((product) => this.selectedBrands.length === 0 || this.selectedBrands.includes(product.brand));
      }
    }
  }
};
</script>

<style>
.app {
  max-width: 1024px;
  margin: 0 auto;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  color: #606569;
}

.product-grid {
  display: grid;                             /* Use grid layout */
  grid-template-columns: repeat(auto-fill, minmax(200px, 1fr)); /* Create columns */
  gap: 20px;                                 /* Define gap between items */
}


.product-item {
  /* Add styling for individual product items here */
}

.link {
  color: #3a7f71;
}
</style>

