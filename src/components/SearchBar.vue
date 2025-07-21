<template>
  <div class="flex flex-col sm:flex-row gap-4 mb-6">
    <input
      v-model="searchText"
      type="text"
      placeholder="Search by name or email..."
      class="px-4 py-2 rounded-md border border-gray-300 w-full sm:w-1/2"
    />

    <select
      v-model="selectedSource"
      class="px-4 py-2 rounded-md border border-gray-300 w-full sm:w-1/3"
    >
      <option value="">All Sources</option>
      <option v-for="source in uniqueSources" :key="source" :value="source">
        {{ source }}
      </option>
    </select>
  </div>
</template>

<script setup>
import { ref, computed, watch } from 'vue'

const props = defineProps({
  users: Array
})

const emit = defineEmits(['filter'])

const searchText = ref('')
const selectedSource = ref('')

const uniqueSources = computed(() => {
  const sources = props.users.map(u => u.source)
  return [...new Set(sources)]
})

watch([searchText, selectedSource], () => {
  emit('filter', {
    searchText: searchText.value,
    selectedSource: selectedSource.value
  })
})
</script>
