<template>
  <div :class="['product-card', { 'out-of-stock': !product.inStock }]">
    <h2>{{ product.name }}</h2>
    <p>{{ product.description }}</p>
    <p>Ціна: {{ product.price }} грн</p>

    <button 
      :disabled="!product.inStock"
      :class="{ 'disabled-button': !product.inStock }"
      @click="$emit('add-to-cart', product)"
    >
      {{ product.inStock ? 'Додати до кошика' : 'Немає в наявності' }}
    </button>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';

export interface Product {
  id: number;
  name: string;
  price: number;
  description: string;
  inStock: boolean;
}

export default defineComponent({
  name: 'ProductCard',
  props: {
    product: {
      type: Object as PropType<Product>,
      required: true
    }
  }
});
</script>

<style scoped>
.product-card {
  border: 1px solid #ccc;
  padding: 12px;
  width: 200px;
  margin-bottom: 16px;
}
.out-of-stock {
  opacity: 0.5; /* Товар не в наявності — прозорий блок */
}
button {
  margin-top: 8px;
  padding: 6px 12px;
  cursor: pointer;
}
.disabled-button {
  background-color: #ccc;
  cursor: not-allowed;
}
</style>
