<template>
  <div class="p-6">
    <h1 class="text-2xl font-bold mb-6">TenaMart Waiting List</h1>

    <!-- Search & Filter -->
    <SearchBar :users="allUsers" @filter="applyFilter" />

    <!-- User Cards -->
    <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
      <UserCard
        v-for="user in paginatedUsers"
        :key="user.id"
        :user="user"
      />
    </div>

    <!-- Pagination Controls -->
    <div class="mt-6 flex items-center justify-center gap-2">
      <button
        @click="goToPage(currentPage - 1)"
        :disabled="currentPage === 1"
        class="px-3 py-1 rounded bg-gray-200 disabled:opacity-50"
      >
        Prev
      </button>

      <button
        v-for="page in totalPages"
        :key="page"
        @click="goToPage(page)"
        :class="[
          'px-3 py-1 rounded',
          page === currentPage ? 'bg-blue-600 text-white' : 'bg-gray-100'
        ]"
      >
        {{ page }}
      </button>

      <button
        @click="goToPage(currentPage + 1)"
        :disabled="currentPage === totalPages"
        class="px-3 py-1 rounded bg-gray-200 disabled:opacity-50"
      >
        Next
      </button>
    </div>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'
import { users as mockUsers } from '../data/users'
import UserCard from '../components/UserCard.vue'
import SearchBar from '../components/SearchBar.vue'

// All user data
const allUsers = ref(mockUsers)
const filteredUsers = ref([...mockUsers])

// Pagination state
const currentPage = ref(1)
const pageSize = 6

const totalPages = computed(() =>
  Math.ceil(filteredUsers.value.length / pageSize)
)

const paginatedUsers = computed(() => {
  const start = (currentPage.value - 1) * pageSize
  return filteredUsers.value.slice(start, start + pageSize)
})

// Handle search/filter input
function applyFilter({ searchText, selectedSource }) {
  const text = searchText.toLowerCase()
  filteredUsers.value = allUsers.value.filter(user => {
    const matchText =
      user.name.toLowerCase().includes(text) ||
      user.email.toLowerCase().includes(text)
    const matchSource = selectedSource
      ? user.source === selectedSource
      : true
    return matchText && matchSource
  })

  currentPage.value = 1 // Reset to first page
}

function goToPage(page) {
  if (page >= 1 && page <= totalPages.value) {
    currentPage.value = page
  }
}
</script>
