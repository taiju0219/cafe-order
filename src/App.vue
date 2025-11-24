<script setup>
import { ref, computed } from "vue";
const menuItems = ref([
  { id: 1, name: 'コーヒー', price: 400, count: 0 },
  { id: 2, name: '紅茶', price: 350, count: 0 },
  { id: 3, name: 'チーズケーキ', price: 500, count: 0 }
])
const itemPlus = (targetId) => {
  const targetItem = menuItems.value.find(m => m.id === targetId)
  targetItem.count += 1
}
const itemMinus = (targetId) => {
  const targetItem = menuItems.value.find(m => m.id === targetId)
  if (targetItem.count > 0) {
   targetItem.count -= 1 
  }
}
const totalPrice = computed(() => {
  return menuItems.value.reduce((sum, item) => {
    return sum + item.price * item.count;
  }, 0);
})
const isFreeShipping = computed(() => {
  return totalPrice.value >= 1500
})
</script>

<template>
  <div class="container">
    <h1>カフェ注文画面</h1>
    <hr>
    <ul>
      <li v-for = "menuItem in menuItems" :key = "menuItem.id" class = "menu-item">
        <div class = "item-info">
          <span>{{ menuItem.name }}({{ menuItem.price }}円)</span>
        </div>
        <div class = "item-controls">
          <button @click = "itemMinus(menuItem.id)">-</button>
          <span>{{ menuItem.count }}</span>
          <button @click = "itemPlus(menuItem.id)">+</button>
        </div>
      </li>
    </ul>
    <hr>
    <p>合計: {{ totalPrice }}円</p>
    <p v-if="isFreeShipping" style = "color: red; font-weight: bold;">
      送料無料です
    </p>
  </div>
</template>

<style scoped>
.container {
  padding: 20px;
  font-family: sans-serif;
  max-width: 500px;
}
ul {
  list-style: none;
  padding: 0;
}
li {
  display: flex;
  justify-content: space-between;
  margin-bottom: 10px;
  border: 1px solid #ddd;
  padding: 10px;
}
button {
  margin: 0 10px;
  cursor: pointer;
}
</style>