<template>
  <div class="p-4">
    <h1 class="text-2xl font-bold mb-4">投票介面</h1>
    <hr class="border-t-2 border-gray-300 mb-4" />
    <form @submit.prevent="submitVote">
      <div v-for="item in items" :key="item.id" class="mb-2">
        <label>
          <input type="checkbox" :value="item.id" v-model="selectedIds" class="mr-2" />
          {{ item.name }}
        </label>
      </div>

      <button class="mt-4 bg-green-500 text-white px-4 py-2">送出</button>
    </form>
  </div>
</template>

<script>
import { ref, onMounted } from 'vue'
import api from '../api/axios'

export default {
  setup() {
    const items = ref([])
    const selectedIds = ref([])

    const fetchVotes = async () => {
      const res = await api.get('/votes/items')
      items.value = res.data
    }

    const submitVote = async () => {
      for (const id of selectedIds.value) {
        await api.post(`/votes/vote?voter=TestUser&itemId=${id}`)
      }
      selectedIds.value = []
      await fetchVotes()
    }

    onMounted(fetchVotes)

    return { items, selectedIds, submitVote }
  },
}
</script>
