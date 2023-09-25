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
    <p class="result-count">Remaining Shoes: {{ filteredProducts ? filteredProducts.length:0 }}</p>
    <div class="sort-dropdown">
      <label for="sort">Sort By Price:</label>
      <select id="sort" v-model="sortPriceOrder">
        <option value="asc">Low to High</option>
        <option value="desc">High to Low</option>
      </select>
    </div>
    <div class="sort-dropdown">
      <label for="sortRelevance">Sort By Relevance:</label>
      <select id="sortRelevance" v-model="sortRelevanceOrder">
        <option value="asc">Ascending</option>
        <option value="desc">Descending</option>
      </select>
    </div>
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
      sortPriceOrder: 'asc',
      sortRelevanceOrder: 'asc'
    };

  },
  computed: {
    filteredProducts() {
      let filtered = this.products.filter(
          (product) => {
            if (this.showAvailable) { 
              return product.isAvailable && (this.selectedBrands.length === 0) ||
              this.selectedBrands.includes(product.brand)
            } else {
              return this.selectedBrands.length === 0 || this.selectedBrands.includes(product.brand);
            }
          });

        if (this.sortPriceOrder === 'asc') {
          return filtered.sort((a, b) => a.price - b.price);
        } else if (this.sortPriceOrder === 'desc') {
          return filtered.sort((a, b) => b.price - a.price); 
        } else if (this.sortPriceOrder === 'relevance') {
          const availableProducts = filtered.filter((product) => product.isAvailable);
          const unavailableProducts = filtered.filter((product) => !product.isAvailable);

          return availableProducts.concat(unavailableProducts).sort((a, b) => a.rank - b.rank);
        }
      },
    },
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

