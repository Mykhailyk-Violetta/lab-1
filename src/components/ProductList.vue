<template>
  <div>
    <h1>Міні-каталог товарів</h1>
    <p>Доступно товарів: {{ availableProductsCount }}</p>

    <div class="products">
      <ProductCard
        v-for="product in products"
        :key="product.id"
        :product="product"
        @add-to-cart="addToCart"
      />
    </div>

    <Cart :items="cartItems" @remove-from-cart="removeFromCart" />
  </div>
</template>

<script lang="ts">
import { defineComponent, reactive, computed } from 'vue';
import ProductCard, { Product } from './ProductCard.vue';
import Cart, { CartItem } from './Cart.vue';

export default defineComponent({
  name: 'ProductList',
  components: { ProductCard, Cart },
  setup() {
    // Масив товарів – шкільні приладдя
    const products = reactive<Product[]>([
      { id: 1, name: 'Ручка', price: 15, description: 'Синя кулькова ручка', inStock: true },
      { id: 2, name: 'Зошит', price: 40, description: 'Зошит у клітинку, 48 аркушів', inStock: false },
      { id: 3, name: 'Олівці', price: 120, description: 'Набір кольорових олівців, 12 шт', inStock: true },
    ]);

    // Список товарів у кошику
    const cartItems = reactive<CartItem[]>([]);

    // Додавання товару у кошик
    const addToCart = (product: Product) => {
      cartItems.push({ ...product, addedAt: new Date() });
    };

    // Видалення товару з кошика
    const removeFromCart = (productId: number) => {
      const index = cartItems.findIndex(item => item.id === productId);
      if (index !== -1) cartItems.splice(index, 1);
    };

    // Кількість доступних товарів
    const availableProductsCount = computed(() => products.filter(p => p.inStock).length);

    return { products, cartItems, addToCart, removeFromCart, availableProductsCount };
  }
});
</script>

<style scoped>
.products {
  display: flex;
  flex-wrap: wrap;
  gap: 16px;
}
</style>
