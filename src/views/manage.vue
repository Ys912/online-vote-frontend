<template>
  <div class="p-4">
    <h1 class="text-2xl font-bold mb-4">新增/刪除投票項目</h1>
    <div class="mb-4">
      <input v-model="newItemName" placeholder="輸入投票名稱" class="border p-2 mr-2" />&nbsp;
      <button @click="addItem" class="bg-blue-500 text-white px-4 py-2">新增</button>
    </div>
    <br />
    <ul>
      <li
        v-for="item in items"
        :key="item.id"
        class="flex items-center justify-between border-b py-2"
      >
        <span>{{ item.name }}</span
        >&nbsp;
        <button @click="deleteItem(item.id)" class="text-red-500">刪除</button>
      </li>
    </ul>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'
import api from '../api/axios'

export default {
  setup() {
    const items = ref([])
    const newItemName = ref('')

    const fetchItems = async () => {
      const res = await api.get('/votes/items')
      items.value = res.data
    }

    const addItem = async () => {
      if (!newItemName.value.trim()) return
      await api.post('/votes/items', { name: newItemName.value })
      newItemName.value = ''
      await fetchItems()
    }

    const deleteItem = async (id) => {
      await api.delete(`/votes/${id}`)
      await fetchItems()
    }

    onMounted(fetchItems)

    return { items, newItemName, addItem, deleteItem }
  },
}
</script>

