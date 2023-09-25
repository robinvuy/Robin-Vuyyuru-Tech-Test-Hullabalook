<template>
  <div class="product-grid-item">
    <img class="product-image" :src="product.image" />
    <p class="product-name">Name: {{ product.name }}</p>
    <p class="product-price">Price: £{{ product.price.toFixed(2) }}</p>
  </div>
</template>

<script>
export default {
  name: 'ProductGridItem',
  props: {
    product: { type: Object, required: true, default: () => ({}) },
  },
};
</script>

<style scoped>
.product-grid-item {
  background: #ddebe8;
  max-width: 150px;
}

.product-image {
  width: 100%;
}
</style>
