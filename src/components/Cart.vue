<template>
  <div class="cart">
    <h2>Кошик ({{ items.length }} товарів)</h2>
    <p>Загальна сума: {{ totalPrice }} грн</p>

    <div v-if="items.length === 0">
      Кошик порожній
    </div>

    <ul v-else>
      <li v-for="item in items" :key="item.id">
        {{ item.name }} - {{ item.price }} грн
        <button @click="$emit('remove-from-cart', item.id)">Видалити</button>
        <small>Додано: {{ formatDate(item.addedAt) }}</small>
      </li>
    </ul>
  </div>
</template>

<script lang="ts">
import { defineComponent, PropType } from 'vue';

export interface CartItem {
  id: number;
  name: string;
  price: number;
  description: string;
  inStock: boolean;
  addedAt: Date;
}

export default defineComponent({
  name: 'Cart',
  props: {
    items: {
      type: Array as PropType<CartItem[]>,
      required: true
    }
  },
  methods: {
    formatDate(date: Date) {
      return date.toLocaleTimeString();
    }
  },
  computed: {
    totalPrice() {
      return this.items.reduce((sum, item) => sum + item.price, 0);
    }
  }
});
</script>

<style scoped>
.cart {
  margin-top: 24px;
  border-top: 2px solid #333;
  padding-top: 12px;
}
button {
  margin-left: 8px;
}
</style>
