<template>
  <div class="p-6 font-sans bg-gray-50 min-h-screen w-full">
    <h1 class="text-4xl font-extrabold mb-8 text-[#006E51]">TenaMart Waiting List</h1>

    <!-- Search & Filter -->
    <SearchBar :users="allUsers" @filter="applyFilter" />

    <!-- CSV Button -->
    <div class="mt-6 mb-8 flex justify-start">
      <button
        @click="downloadCSV"
        class="bg-[#006E51] hover:bg-[#00533e] text-white px-5 py-3 rounded shadow transition"
      >
        Download CSV
      </button>
    </div>

    <!-- Loading -->
    <div v-if="isLoading" class="text-center text-gray-500 italic py-8">
      Loading users...
    </div>

    <!-- User Cards with Transition -->
    <TransitionGroup
      name="fade-scale"
      tag="div"
      class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-6"
    >
      <UserCard
        v-for="user in paginatedUsers"
        :key="user.id"
        :user="user"
        @block="confirmBlock(user)"
        @delete="confirmDelete(user.id)"
      />
    </TransitionGroup>

    <!-- Signup Chart ABOVE Pagination, wider container -->
    <div
      class="mt-12 bg-white shadow-lg rounded-lg p-6 max-w-6xl mx-auto w-full"
      style="min-height: 320px;"
    >
      <SignupChart :users="allUsers" />
    </div>

    <!-- Pagination BELOW Chart -->
    <div class="mt-8 flex items-center justify-center gap-3">
      <button
        @click="goToPage(currentPage - 1)"
        :disabled="currentPage === 1"
        class="px-4 py-2 rounded bg-gray-200 disabled:opacity-50 hover:bg-gray-300 transition"
      >
        Prev
      </button>

      <button
        v-for="page in totalPages"
        :key="page"
        @click="goToPage(page)"
        :class="[
          'px-4 py-2 rounded font-semibold transition',
          page === currentPage ? 'bg-[#006E51] text-white' : 'bg-gray-100 hover:bg-gray-200'
        ]"
      >
        {{ page }}
      </button>

      <button
        @click="goToPage(currentPage + 1)"
        :disabled="currentPage === totalPages"
        class="px-4 py-2 rounded bg-gray-200 disabled:opacity-50 hover:bg-gray-300 transition"
      >
        Next
      </button>
    </div>
  </div>
</template>

<script setup>
import Papa from 'papaparse'
import { ref, computed, onMounted } from 'vue'
import { users as mockUsers } from '../data/users'
import UserCard from '../components/UserCard.vue'
import SearchBar from '../components/SearchBar.vue'
import SignupChart from '../components/SignupChart.vue'

// State
const allUsers = ref([])
const filteredUsers = ref([])
const isLoading = ref(true)
const currentPage = ref(1)
const pageSize = 6

onMounted(() => {
  allUsers.value = mockUsers.map(u => ({
    ...u,
    status: u.status || 'active',
    createdAt: u.createdAt || new Date().toISOString()
  }))
  filteredUsers.value = [...allUsers.value]
  isLoading.value = false
})

// Computed
const totalPages = computed(() =>
  Math.ceil(filteredUsers.value.length / pageSize)
)

const paginatedUsers = computed(() => {
  const start = (currentPage.value - 1) * pageSize
  return filteredUsers.value.slice(start, start + pageSize)
})

// Filter
function applyFilter({ searchText, selectedSource }) {
  const text = searchText.toLowerCase()
  filteredUsers.value = allUsers.value.filter(user => {
    const matchText =
      user.name.toLowerCase().includes(text) ||
      user.email.toLowerCase().includes(text)
    const matchSource = selectedSource ? user.source === selectedSource : true
    return matchText && matchSource
  })
  currentPage.value = 1
}

// Pagination
function goToPage(page) {
  if (page >= 1 && page <= totalPages.value) {
    currentPage.value = page
  }
}

// Confirm before block/unblock
function confirmBlock(user) {
  const action = user.status === 'blocked' ? 'Unblock' : 'Block'
  if (confirm(`${action} ${user.name}?`)) {
    toggleBlock(user)
  }
}

function toggleBlock(user) {
  const index = allUsers.value.findIndex(u => u.id === user.id)
  if (index !== -1) {
    allUsers.value[index].status =
      allUsers.value[index].status === 'blocked' ? 'active' : 'blocked'
    applyFilter({ searchText: '', selectedSource: '' })
  }
}

// Confirm before delete
function confirmDelete(userId) {
  const user = allUsers.value.find(u => u.id === userId)
  if (user && confirm(`Are you sure you want to delete ${user.name}?`)) {
    deleteUser(userId)
  }
}

function deleteUser(userId) {
  allUsers.value = allUsers.value.filter(u => u.id !== userId)
  filteredUsers.value = filteredUsers.value.filter(u => u.id !== userId)
}

// CSV
function downloadCSV() {
  const cleanData = filteredUsers.value
    .filter(u => u.status !== 'blocked')
    .map(({ id, name, email, source, createdAt }) => ({
      ID: id,
      Name: name,
      Email: email,
      Source: source,
      "Signup Date": createdAt
    }))

  const csv = Papa.unparse(cleanData)
  const blob = new Blob([csv], { type: 'text/csv;charset=utf-8;' })
  const link = document.createElement('a')
  link.href = URL.createObjectURL(blob)
  link.download = 'tenamart_waiting_list.csv'
  link.click()
}
</script>

<style>
.fade-scale-enter-active,
.fade-scale-leave-active {
  transition: all 0.3s ease;
}
.fade-scale-enter-from,
.fade-scale-leave-to {
  opacity: 0;
  transform: scale(0.95);
}
</style>
