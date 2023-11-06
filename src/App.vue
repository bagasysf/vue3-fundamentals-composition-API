<script setup>
import { ref, computed } from 'vue'

const header = ref('Shopping List App')
const characterCount = computed(() => {
  return newItem.value.length
})
const items = ref([
  {id: 1, label: "10 party hats", purchased: true, highPriority: false},
  {id: 2, label: "2 board games", purchased: true, highPriority: false},
  {id: 3, label: "20 cups", purchased: false, highPriority: true}
])
const reversedItems = computed(() => {
  return [...items.value].reverse()
})
const newItem = ref("")
const newItemHighPriority = ref(false)
const editing = ref(false)
const saveItem = () => {
  items.value.push({
    id: items.value.length + 1, 
    label: newItem.value,
    purchased: false,
    highPriority: newItemHighPriority.value
  })
  newItem.value = ""
}
const doEdit = (e) => {
  editing.value = e
  newItem.value = ""
}
const togglePurchased = (item) => {
  item.purchased = !item.purchased
}

</script>

<template>
  <div class="header">
    <h1>{{ header }}</h1>
    <button v-if="editing" class="btn" @click="doEdit(false)">Cancel</button>
    <button v-else class="btn btn-primary" @click="doEdit(true)">Add Item</button>
  </div> 
  <form 
    v-if="editing"
    class="add-item-form"
    @submit.prevent="saveItem">
    <input 
      v-model.trim="newItem"
      type="text" 
      placeholder="Add an item">
    <label>
      <input type="checkbox" v-model="newItemHighPriority">
      High Priority
    </label>
    <button
      v-bind:disabled="newItem.length === 0"
      class="btn btn-primary">
      Save Item
    </button>
  </form>
  <p class="counter" v-if="editing">
    {{characterCount}}/200
  </p>
  <ul>
    <li 
    @click="togglePurchased(item)"
    v-for="(item, index) in reversedItems" 
    :key="item.id"
    class="static-class"
    :class="{strikeout: item.purchased, priority: item.highPriority}"
    >
      {{item.label}}
    </li>
  </ul>
  <p v-if="!items.length">
    Nothing see here
  </p>
</template>